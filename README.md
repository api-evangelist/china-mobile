# China Mobile (china-mobile)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

China Mobile Limited is the world's largest mobile network operator by subscribers, serving approximately 1,005 million mobile customers and 110 million gigabit broadband customers across all 31 provinces of mainland China plus Hong Kong, with roaming into more than 200 countries. Listed on the Hong Kong Stock Exchange and the Shanghai Stock Exchange and majority-owned by state-held China Mobile Communications Group, it runs mobile, broadband, cellular IoT, satellite internet, data centre, cloud and AI businesses on annual revenue of about RMB 1,050.2 billion.

In the network-API value chain China Mobile sits squarely on the operator side, not the aggregator side. It joined the GSMA Open Gateway initiative in June 2023, sponsors and maintains several CAMARA APIs upstream, and in October 2024 secured GSMA Open Gateway certification for its Network-as-a-Service platform after its Quality on Demand API passed 63 conformance tests. None of that is callable from the open internet. Its API posture is partner-gated and domestic — the capability platforms publish real product and interface documentation but issue credentials only to registered mainland enterprise customers under contract. China Mobile is not a shareholder in Aduna, so it reaches developers through its own Chinese-language capability marketplaces rather than through the global CPaaS or aggregator channel.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/china-mobile/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/china-mobile/refs/heads/main/apis.yml)

## Tags

- Telecommunications
- China
- Mobile Network Operator
- Network APIs
- CAMARA
- GSMA Open Gateway
- IoT
- 5G
- Broadband
- Quality on Demand
- Number Authentication
- Satellite

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## APIs

### OneNET Studio Application API

The one China Mobile surface with a complete, anonymously readable request-and-response contract. An action-dispatched gateway — every call is `https://openapi.heclouds.com/{namespace}?action={Action}&version=1`, with namespaces `application`, `common`, `lwm2m-online` and `lwm2m-offline`. Forty-four operations are documented across devices, products, projects, groups, thing models, properties and desired properties, service invocation, device files and scene-linkage rules. Authentication is a signed, time-boxed `authorization` header (HMAC over `et`, `method`, `res`, `version`; signature algorithm version `2020-05-29`). Every response is a uniform envelope carrying a platform-generated `requestId`; business failures arrive as HTTP 200 with `success: false`.

- **Human URL:** [API 调用说明](https://open.iot.10086.cn/doc/iot_platform/book/api/introduce.html)
- **Base URL:** `https://openapi.heclouds.com`
- **OpenAPI:** `openapi/china-mobile-onenet-studio-openapi.yml`

### OneNET Voice Call Service (VCS) API

语音通话 — click to dial (点击拨号) and TTS voice notification (语音通知) at `https://openapi.heclouds.com/vcs?action={voiceNotify|dialNotify}&version=2`, each returning a `call_id` and delivering call-status callbacks to a caller-supplied `notify_url`. **Click to Dial is the domestic first-party product behind the CAMARA Click to Dial API that China Mobile sponsors upstream** — but it does not implement the CAMARA schema or its OIDC/CIBA security model. Access is gated three ways: business entitlement, a test account pinned to an approved source IP, and a trial-usage quota.

- **Human URL:** [API 使用说明](https://open.iot.10086.cn/doc/iot_platform/book/vcs/vcs_api/request.html)
- **Base URL:** `https://openapi.heclouds.com`
- **OpenAPI:** `openapi/china-mobile-vcs-openapi.yml`

### OneNET IoT Open Platform API

OneNET is China Mobile's IoT PaaS, operated by its CMIOT subsidiary, for device connection, device management, data storage and data visualisation. It is the company's most genuinely developer-facing surface, with open Chinese-language documentation covering MQTT and multi-protocol device access, OneNET Studio, edge computing, device management (DMP), OTA, message queue, SMS, LBS and video capabilities. The legacy REST base URL is live and responds with a JSON token authentication error to unauthenticated calls.

- **Human URL:** [https://open.iot.10086.cn/doc/](https://open.iot.10086.cn/doc/)
- **Base URL:** `https://api.heclouds.com`

#### Properties

- [Documentation](https://open.iot.10086.cn/doc/)
- [API Reference](https://open.iot.10086.cn/doc/iot_platform/)
- [Documentation](https://open.iot.10086.cn/doc/mqtt)
- [Portal](https://open.iot.10086.cn/)

### China Mobile IoT Card Capability Open Platform API

The 物联卡能力开放平台 (IoT Card Capability Open Platform) exposes cellular M2M SIM lifecycle and subscriber-information operations to enterprise IoT customers. Interface documentation is published openly — the confirmed CMIOT_API2003 码号信息查询 page documents `GET/POST https://api.iot.10086.cn/v2/cardinfo`, resolving any one of ICCID, IMSI or MSISDN into the other two. Authentication is a proprietary signed-request scheme whose credentials China Mobile issues by email to approved enterprise customers only.

- **Human URL:** [https://api.iot.10086.cn/apiDocuments/cusInfo/codeInfoQuery.html](https://api.iot.10086.cn/apiDocuments/cusInfo/codeInfoQuery.html)
- **Base URL:** `https://api.iot.10086.cn/v2`

#### Properties

- [Documentation](https://api.iot.10086.cn/)
- [API Reference](https://api.iot.10086.cn/apiDocuments/cusInfo/codeInfoQuery.html)

### China Mobile Communication Capability Open Platform

The 通信能力开放平台 is China Mobile's commercial network-capability channel and the surface where its Open Gateway work actually meets buyers. Its published developer guide lists 语音通知 (voice notification), 语音验证码 (voice OTP), 点击拨号 (click to dial), 中间号 (number masking) and QoS保障 (QoS guarantee). Click to Dial and QoS map directly to the CAMARA Click to Dial and Quality on Demand APIs that China Mobile sponsors upstream. No base URL, endpoint reference or machine-readable definition is published anonymously.

- **Human URL:** [https://ct.open.10086.cn/portal/developer/docAndHelp.action](https://ct.open.10086.cn/portal/developer/docAndHelp.action)

#### Properties

- [Documentation](https://ct.open.10086.cn/portal/developer/docAndHelp.action)
- [Portal](https://ct.open.10086.cn/portal/index.action)

### China Mobile Internet Capability Open Platform

中国移动互联网能力开放平台 at dev.10086.cn describes itself as offering 移动认证/号码认证 (mobile and number authentication), 大数据服务 (big data services), 通信能力, 支付能力 (carrier billing) and promotion capabilities. In practice it is an enterprise procurement console rather than a developer portal — its routes are login, ability ordering, application management, billing, invoices and work orders, and every documentation route resolves to the same single-page shell for anonymous visitors.

- **Human URL:** [https://dev.10086.cn/](https://dev.10086.cn/)

#### Properties

- [Documentation](https://dev.10086.cn/)
- [Portal](https://dev.10086.cn/apmc/abilityList)

## CAMARA and GSMA Open Gateway posture

China Mobile joined **GSMA Open Gateway in June 2023** and received **GSMA Open Gateway certification on 29 October 2024** for its Network-as-a-Service platform, after its **Quality on Demand** API passed 63 technical tests run by the Open Gateway working group over ZTE NEF/SCEF exposure functions.

It is an active upstream **CAMARA** sponsor. The CAMARA API backlog records China Mobile as sponsor or participant for **Click to Dial**, **Model as a Service Family**, **Knowledge Base - Manage**, **Q&A Assistant - Manage**, **Q&A Assistant - Service**, **High-throughput Elastic Network**, **Facial Recognition**, **Network Slice Booking** and **Site-to-cloud VPN**. GSMA material also credits a CAMARA **Number Verification** deployment.

None of it is publicly callable. There is no CAMARA endpoint, no OpenAPI, no OIDC discovery document and **no CIBA reference** on any China Mobile host. China Mobile is **not** an Aduna shareholder, so it does not reach global developers through the Ericsson-led aggregator channel — it sells through its own domestic capability marketplaces under enterprise contract.

## Machine-readable definitions

**China Mobile publishes none.** Every OpenAPI, Swagger, AsyncAPI, GraphQL, gRPC and Postman candidate probed on China Mobile hosts returned an HTML single-page-app shell, 403, 404, 406 or 500 — re-probed on 2026-07-25 against `openapi.heclouds.com`, `api.heclouds.com`, `iot-api.heclouds.com`, `api.iot.10086.cn`, `open.iot.10086.cn` and `dev.10086.cn`. See `review.yml` and `well-known/` for the probe logs with HTTP statuses.

What the 2026-07-25 enrichment round *did* find is that OneNET publishes a complete, anonymously readable reference for two of its API products. Those references have been transcribed — operation by operation, with each operation carrying its source page in `x-evidence` — into machine-readable definitions in this repository:

- `openapi/china-mobile-onenet-studio-openapi.yml` — 44 documented operations on the OneNET Studio application API (`https://openapi.heclouds.com/{namespace}?action={Action}&version=1`)
- `openapi/china-mobile-vcs-openapi.yml` — the Voice Call Service actions `voiceNotify` and `dialNotify`, plus their two call-status webhook callbacks
- `asyncapi/china-mobile-onenet-asyncapi.yml` — the OneNET event surface (HTTP data push, push-instance validation handshake, VCS call-status callbacks)

These are API Evangelist derivations of published first-party documentation, marked as such in each `info.x-provenance` block. They are not China Mobile products and China Mobile does not endorse them.

## Artifacts

| Artifact | File |
| --- | --- |
| Authentication | `authentication/china-mobile-authentication.yml` |
| Conventions | `conventions/china-mobile-conventions.yml` |
| Error codes | `errors/china-mobile-error-codes.yml` |
| Webhooks | `asyncapi/china-mobile-onenet-webhooks.yml` |
| Data model | `data-model/china-mobile-data-model.yml` |
| Packages / SDKs | `packages/china-mobile-packages.yml` |
| Lifecycle | `lifecycle/china-mobile-lifecycle.yml` |
| Changelog | `changelog/china-mobile-changelog.yml` |
| Sandbox | `sandbox/china-mobile-sandbox.yml` |
| Conformance | `conformance/china-mobile-conformance.yml` |
| Domain security | `security/china-mobile-domain-security.yml` |
| Well-known probe log | `well-known/china-mobile-well-known.yml` |
| Agentic access | `agentic-access/china-mobile-agentic-access.yml` |
| MCP (candidate) | `mcp/china-mobile-mcp.yml` |
| Agent skills | `skills/` |
| llms.txt | `llms/china-mobile-llms.txt` |
| Overlays | `overlays/` |

## SDKs

China Mobile's first-party client-library channel is **[github.com/cm-heclouds](https://github.com/cm-heclouds)** — the CMIOT (中移物联网) OneNET account, 38 public repositories covering Java, Node, C, C#, PHP, Go, Android, iOS, Arduino, JavaScript and NB-IoT. The OneNET "API SDK下载" page names the Java and Node SDKs there as the platform's official SDKs, and the `JAVA-HTTP-SDK` README states verbatim that it was built by 中移物联网公司. Only the Java side reaches a package registry, on **Maven Central** under `com.github.cm-heclouds`. Nothing first-party was found on PyPI, NuGet, RubyGems, Packagist, crates.io or pkg.go.dev.

## Links

- [Website](https://www.chinamobileltd.com/)
- [Developer Portal (OneNET)](https://open.iot.10086.cn/)
- [Documentation](https://open.iot.10086.cn/doc/)
- [Internet Capability Open Platform](https://dev.10086.cn/)
- [Communication Capability Open Platform](https://ct.open.10086.cn/portal/index.action)
- [IoT Card Capability Open Platform](https://api.iot.10086.cn/)
- [China Mobile Research Institute on GitHub](https://github.com/cmri)
