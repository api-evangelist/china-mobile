---
name: Onboard and authenticate against the OneNET open API gateway
description: Get from zero to a signed, working call against China Mobile's OneNET Studio
  application API — account, access key, resource scope, and the authorization header.
api: openapi/china-mobile-onenet-studio-openapi.yml
operations:
- QueryProductList
- QueryDeviceList
generated: '2026-07-25'
method: generated
source: https://open.iot.10086.cn/doc/iot_platform/book/api/auth.html
---

# Onboard and authenticate against OneNET

OneNET is China Mobile's IoT platform, operated by CMIOT. It is the only China Mobile API
estate a developer can reach without a signed enterprise contract, but it is still
mainland-oriented: the console and the documentation are Chinese-language, and full
entitlement requires 实名认证 (real-name verification).

## 1. Get credentials

1. Register at <https://open.iot.10086.cn/> and complete 实名认证.
2. Create a project under 应用开发 → 项目管理. Note the `project_id`.
3. Read the **accessKey** from the account access page. There are two scopes:
   - **master user** — `res=userid/{userid}`, uses the master accessKey.
   - **project group** — `res=projectid/{projectid}/groupid/{groupid}`, uses that group's
     accessKey, and may only operate on devices inside the group. Prefer this: it is the
     only privilege reduction the platform offers.

Never ship the master accessKey to a device or a browser. It is a bearer secret with no
scopes, no rotation endpoint and no revocation API.

## 2. Build the authorization header

Every request carries one header, `authorization`, whose value is a `&`-joined set of
`key=value` pairs:

```
authorization: version=2020-05-29&res=userid%2F38055&et=1623982416&method=sha1&sign=<urlencoded>
```

| field | meaning |
| --- | --- |
| `version` | signature algorithm version. `2020-05-29` is the only supported value. |
| `res` | resource scope — `userid/{userid}` or `projectid/{projectid}/groupid/{groupid}`. |
| `et` | expiry as a 10-digit epoch-seconds timestamp. You choose it. |
| `method` | hash method: `md5`, `sha1` or `sha256`. |
| `sign` | the signature. |

Compute the signature as:

```
StringForSignature = et + "\n" + method + "\n" + res + "\n" + version
sign = base64( hmac_<method>( base64decode(accessKey), StringForSignature ) )
```

Two things trip people up:

- The accessKey must be **base64-decoded before** it is used as the HMAC key.
- Values in the header are **URL-encoded** — in particular `/` becomes `%2F`, `+` becomes
  `%2B`, `=` becomes `%3D`. The `sign` value almost always contains `/` and `=`.

Pick `et` deliberately. A long-lived token is a long-lived bearer secret; the docs' own
examples run from one hour to 365 days.

## 3. Make the call

The gateway is action-dispatched. The namespace is the path, the operation is the `action`
query parameter, and the contract is pinned by `version`:

```
GET https://openapi.heclouds.com/application?action=QueryProductList&version=1&project_id=KLza8R
authorization: version=2020-05-29&res=...&et=...&method=sha1&sign=...
```

Namespaces are `application` (application development), `common` (device management),
`lwm2m-online` and `lwm2m-offline`. All platform interfaces are `version=1`; Voice Call
Service is `version=2`.

Start with `QueryProductList` then `QueryDeviceList` for a project — both are read-only and
prove the signature end to end.

## 4. Read the response

Success and failure both arrive as **HTTP 200**. Branch on `success`, never on the status
code:

```json
{ "requestId": "8906582E6722409AA6C40E7863B733A5", "success": true, "data": { } }
{ "requestId": "8906582E6722409AA6C40E7863B733A5", "success": false,
  "code": "iot.application.deviceNotFound", "msg": "device does not exist" }
```

Log `requestId` on every call — it is the only correlation identifier the platform gives
you, and support will ask for it.

Authentication failures surface as `authPermissionDeny` (鉴权失败) and permission failures as
`resourePermissionDeny` (note the platform's own spelling). If you see either while you are
certain the key is right, check the `et` expiry and the URL-encoding of `sign` first — those
are the two common causes. The full registry is in
`errors/china-mobile-error-codes.yml`.

## Gotchas

- No idempotency key exists anywhere on this platform. A retried POST re-executes.
- No rate limits are published. Back off on your own schedule.
- `authorization` is the header name on the platform API; the Voice Call Service reference
  writes it as `Authorization`. Treat the name case-insensitively.
