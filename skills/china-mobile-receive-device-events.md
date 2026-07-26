---
name: Receive OneNET device events over HTTP data push
description: Stand up a OneNET HTTP push receiver — pass the address-validation handshake,
  verify the message signature, decrypt secure-mode payloads, and survive the platform's
  16-attempt retry schedule.
api: asyncapi/china-mobile-onenet-asyncapi.yml
operations:
- receivePushInstanceValidation
- receiveHttpDataPush
generated: '2026-07-25'
method: generated
source: https://open.iot.10086.cn/doc/iot_platform/book/application-develop/push/http_push.html
---

# Receive OneNET device events over HTTP data push

OneNET's 数据推送 (HTTP data push) is one-way: the platform is the client, your application
server is the server. There is no subscription API — instances are created in the console
(maximum **10 per user**) and bound to a project's rule engine.

## 1. Register the push instance

In the console: 应用开发 → 消息推送 → 添加实例. You supply:

- **推送地址 (push address)** — must answer **both GET and POST**. GET is the validation
  handshake, POST is delivery.
- **Token** — the shared secret used for the signature. You choose it.
- **消息加密方式** — 明文 (plaintext) or 安全模式 (secure). Secure mode makes the platform
  generate a 16-byte AES key.

## 2. Pass the validation handshake (GET)

Until this passes, the instance sits in 待验证 and nothing is delivered.

OneNET calls `GET https://your-host/path?msg=…&nonce=…&signature=…`. You must:

1. Concatenate `A = token + nonce + msg`.
2. `B = md5(A)`.
3. Base64-encode `B`, then URL-decode the result to get `C`.
4. Compare `C` to `signature`. Equal means the request came from OneNET.
5. **Echo `msg` back verbatim in the response body.**

Skipping verification is allowed — you may just echo `msg` — but do not. It is the only
thing standing between your ingest endpoint and anyone who can guess the URL.

Instance states after this are 验证成功 or 验证失败. Failure is either a network error
reaching your address or a malformed/rejected echo.

## 3. Bind a message source

Push delivers whatever the **rule engine** selects. Per project: 规则引擎 → choose the data
source and filter, then set the destination to HTTP 推送 and pick a validated instance. The
documented sources are 设备生命周期 (device lifecycle), 设备物模型 (thing-model data) and
场景联动触发日志 (scene-linkage trigger log).

The alternative destination is the managed 消息队列 MQ, if you would rather pull than be
pushed to.

## 4. Handle delivery (POST)

Body:

```json
{ "msg": "…", "nonce": "abcdefgh", "signature": "…", "time": 1591340648197, "id": "3799902" }
```

- `msg` — the rule-engine-filtered content as a JSON **string**. Parse it as a second step.
- `signature` — verify exactly as in the handshake: `base64(md5(token + nonce + msg))`.
- `time` — push timestamp in **milliseconds**.
- `id` — message id. Store it; it is your deduplication key (see below).

If the instance is in **安全模式**, `msg` is AES-encrypted, **CBC mode, PKCS7 padding**, with
the 16-byte key the console generated. Decrypt before parsing.

## 5. Answer within 5 seconds

The delivery contract is strict and published:

- Respond **HTTP 200 within 5 seconds**. Anything else counts as a failed delivery.
- OneNET then retries up to **16 times** with exponential backoff: 5s, 10s, 30s, 1m, 2m,
  3m, 4m, 5m, 6m, 7m, 8m, 9m, 10m, 20m, 30m, 1h — a total window of **2h 45m 04s**, after
  which the message is dropped.

Two consequences worth designing for:

1. **Acknowledge fast, process later.** Verify the signature, enqueue, return 200. Doing
   real work inline is how you end up with 16 copies of every message.
2. **Be idempotent yourself.** OneNET offers no idempotency key and no exactly-once
   guarantee; the message `id` is what you deduplicate on. The platform's own API has no
   idempotency contract either (`conventions/china-mobile-conventions.yml`), so this is the
   one place the burden is explicitly yours.

## Related

- Voice Call Service call-status callbacks are a different surface with a different
  contract — see `skills/china-mobile-place-a-click-to-dial-call.md`.
- A first-party reference receiver exists in C#: <https://github.com/cm-heclouds/data-push>.
- Full event schemas: `asyncapi/china-mobile-onenet-asyncapi.yml`.
