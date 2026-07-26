---
name: Place a click-to-dial or voice-notification call and track its status
description: Use China Mobile's OneNET Voice Call Service — the first-party product behind
  the CAMARA Click to Dial API it sponsors — to connect two parties or deliver a TTS voice
  notification, and consume the call-status callbacks.
api: openapi/china-mobile-vcs-openapi.yml
operations:
- vcsAction
- clickToDialStatusCallback
- voiceNotifyStatusCallback
generated: '2026-07-25'
method: generated
source: https://open.iot.10086.cn/doc/iot_platform/book/vcs/vcs_api/
---

# Place a click-to-dial or voice-notification call

OneNET 语音通话 (Voice Call Service) is the only China Mobile network-capability product
whose full request and response contract is published anonymously. It is also the domestic
first-party product behind the **CAMARA Click to Dial** API that China Mobile sponsors
upstream — but it does not implement CAMARA's schema, paths or OIDC/CIBA security model.

Namespace `vcs`, and note: **`version=2`**, not 1 like the rest of the platform.

## 0. Before you can call anything

This surface is gated in three stacked ways, and all three produce distinct errors:

| gate | error when unmet |
| --- | --- |
| business enabled on the account | `notOpenTheBusiness` / `theBusinessIsClosed` |
| test account applied for, requests from an approved IP | `notApplyTestAccount` / `testAccountIsInvalid` / `ipWhiteListNotMatch` |
| trial quota not exhausted | `overTheExperienceNumber` |

The service number (`display` / `sponsor`) and every TTS template must clear 资质申请
(qualification review) before use. There are no published test numbers — see
`sandbox/china-mobile-sandbox.yml`.

## 1. Click to dial

Connects a caller to a callee, both legs originated by the network.

```
POST https://openapi.heclouds.com/vcs?action=dialNotify&version=2
Authorization: version=2020-05-29&res=…&et=…&method=sha1&sign=…
Content-Type: application/json;charset=utf-8

{
  "sponsor": "02066240222",
  "caller":  "8618223159111",
  "callee":  "8613696486500",
  "display": "02066240222",
  "notify_url": "https://your-host/callback/statusback"
}
```

- `sponsor`, `caller`, `callee` are required. Numbers are **MSISDN** (`8613…`) or area code
  plus fixed line (`075528000003`) — no `+`, no spaces.
- `sponsor` is the service number the platform allocated to you, not an arbitrary number.
- `display` defaults to `sponsor`. It may only be set to `sponsor` or to `caller`, and
  using `caller` requires that number to be allowlisted. Any other value is rejected.
- `notify_url` is optional; **omit it and you get no callbacks at all**.

## 2. Voice notification

Plays a TTS template to one party, optionally collecting DTMF digits.

```
POST https://openapi.heclouds.com/vcs?action=voiceNotify&version=2

{
  "participant_address": "8618102383000",
  "display": "02066240200",
  "actions": [ { "operation": "PlayAndCollect", "tts_template": "89",
                 "param_value": "{\"param1\":\"张三\"}",
                 "collect_length": "1", "replay_after_collection": "true",
                 "collect_content_trigger_replaying": "1" } ],
  "notify_url": "https://your-host/callback/statusback"
}
```

- `operation` is `Play` or `PlayAndCollect` — nothing else is supported.
- `tts_template` must be an approved template number; `param_value` is a JSON object of its
  variables, UTF-8.
- `collect_length` (1–32) applies only to `PlayAndCollect`. If
  `replay_after_collection=true` the collected digits are **not** reported to you — set it
  false when you actually want the digits.
- `relay_time` repeats the announcement, and applies only to `Play`.

## 3. Read the response

```json
{ "requestId": "a25087f46df04b69b29e90ef0acfd115", "success": true,
  "data": { "call_id": "150104227912386807" } }
```

Persist `call_id` — it is the join key for every subsequent status event. Failures come
back HTTP 200 with `success:false`, a `code` such as `iot.vcs.notApplyTestAccount`, and a
`msg`.

## 4. Consume the status callbacks

OneNET POSTs to your `notify_url`, carrying its own signed `Authorization` header. Payload:
`user_id`, `call_id`, `caller`, `callee`, `status`, plus `reason` and `call_duration` when
the call ends.

- Click to dial statuses: `CallingCaller` → `CallingCallee` → `Connected` → `Disconnected`
  (plus `report_date`, and `caller_call_duration` when the caller is not a China Mobile
  number).
- Voice notification statuses: `CallingCallee` → `Connected` → `Disconnected`, plus
  `CollectResult` carrying `operation_result` when you collected digits.
- On `Disconnected`, `reason` tells you *why*: `HangUp`, `CalleeBusy`, `CalleeNoAnswer`,
  `CalleeEmpty` (unallocated number), `CalleeUnregistere` (handset off), `CalleeReject`,
  `CalleenotReach`, `CallerAbandon`, `Other`. Bill and retry on this field, not on duration.

Acknowledge with the OneNET callback envelope:

```json
{ "request_id": "85fcaba7045247c88b87380149ea0941", "code_no": "000000",
  "code": "onenet_common_success", "message": "调用成功" }
```

The full acknowledgement code table (`000000` success through `001010` business closed) is
in `errors/china-mobile-error-codes.yml`.

## Gotchas

- **These operations place real phone calls and cost real money.** There is no idempotency
  key on this platform — a retried `dialNotify` after a timeout dials a second time. Treat
  a timeout as *unknown*, not *failed*: reconcile via the status callback on `call_id`
  before retrying.
- No rate limit is published, but `talkTimeIsLimit` exists — call duration can be capped by
  entitlement.
- Callbacks are the only completion signal. Without `notify_url` you cannot tell a
  connected call from an unanswered one.
