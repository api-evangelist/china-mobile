---
name: Register a device and read or control it through the thing model
description: Create a device under a OneNET product, read its status and latest property
  values, write a property, set a desired value for an offline device, and invoke a
  thing-model service.
api: openapi/china-mobile-onenet-studio-openapi.yml
operations:
- CreateDevice
- QueryDeviceDetail
- QueryDeviceStatus
- QueryDeviceProperty
- SetDeviceProperty
- SetDeviceDesiredProperty
- CallService
- QueryServiceRecord
generated: '2026-07-25'
method: generated
source: https://open.iot.10086.cn/doc/iot_platform/book/api/
---

# Register a device and control it through the OneNET thing model

Prerequisite: a signed `authorization` header — see
`skills/china-mobile-onboard-and-authenticate.md`.

OneNET identifies a device **compositely**: `product_id` + `device_name`. There is no single
opaque device id you can pass everywhere. Get used to carrying the pair.

## 1. Create the device

`CreateDevice` lives in the device-management namespace:

```
POST https://openapi.heclouds.com/common?action=CreateDevice&version=1
Content-Type: application/json;charset=utf-8

{ "product_id": "9MaNe52pNO", "device_name": "no003", "desc": "iot application" }
```

Required: `product_id`, `device_name`. Optional: `desc`, `imsi`, `psk`, `auth_code` — and
`imei`, which becomes **required when the product's protocol is LwM2M**. If you leave `psk`
empty the platform generates one.

For fleets use `BatchCreateDevices` — same namespace, a `devices` array, **maximum 500 per
call**.

Edit with `UpdateDevice`, remove with `DeleteDevice`, move between products with
`MoveProductDevice`. `RegistDeviceIdentity` registers an industrial-internet identity for a
device if you need one.

## 2. Confirm it exists

```
GET https://openapi.heclouds.com/common?action=QueryDeviceDetail&version=1&product_id=…&device_name=…
```

Then, from the application namespace, `QueryDeviceStatus` for the live online/offline state
and `QueryDeviceStatusHistory` for the transition record. `QueryDeviceLog` returns the
operation log.

## 3. Read data

The thing model (物模型) is the contract between the device and the platform. Fetch it with
`QueryThingModel` (per product) or `QuerySystemThingModel` (platform-standard function
points) so you know which `identifier`s exist before you address them.

- `QueryDeviceProperty` — latest values.
- `QueryDevicePropertyDetail` — a specific property.
- `QueryDevicePropertyHistory` / `QueryDeviceEventHistory` — time series.

## 4. Write data

Two different verbs, and choosing wrong is the most common OneNET mistake:

- **`SetDeviceProperty`** writes to a device that is **online now**. If the device is
  offline the write fails (`setPropertyFailed`).
- **`SetDeviceDesiredProperty`** stores a *desired* value the device picks up when it next
  connects. Use this for anything that must survive a sleep cycle — NB-IoT devices spend
  most of their life unreachable. Read back with `QueryDeviceDesiredProperty`, clear with
  `DeleteDeviceDesiredProperty`.

## 5. Invoke a service

```
POST https://openapi.heclouds.com/application?action=CallService&version=1

{ "project_id": "…", "product_id": "…", "device_name": "…",
  "identifier": "<thing-model service identifier>", "params": { } }
```

All five fields are required. `CallService` is asynchronous from the caller's point of
view — poll `QueryServiceRecord` for the execution record rather than assuming the call
result is the device result.

## 6. Handle failure

Every response is HTTP 200. Branch on `success`, then on `code`:

| code | meaning | what to do |
| --- | --- | --- |
| `deviceNotFound` | device does not exist | check the `product_id` + `device_name` pair |
| `productNotFound` | product does not exist | check `product_id` |
| `setPropertyFailed` | property write failed | device is probably offline — use `SetDeviceDesiredProperty` |
| `callTmService` | service invocation failed | check the `identifier` against the thing model |
| `invalidParameter` / `parameterRequired` | bad or missing parameter | compare against the operation's parameter table |
| `authPermissionDeny` | signature rejected | check `et` expiry and URL-encoding of `sign` |

Full registry: `errors/china-mobile-error-codes.yml`.

## Gotchas

- **No idempotency.** `CreateDevice` retried after a timeout will either create a second
  device or fail on a duplicate name — decide which you prefer and write for it.
- A **group-scoped** access key can only touch devices inside its group; a device you can
  see in the console may still be invisible to the key you are signing with.
- Devices must be added to a project (`AddDevice`) before the application-namespace
  operations can address them.
