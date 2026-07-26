# China Mobile (china-mobile)

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

**None found.** Every OpenAPI, Swagger, AsyncAPI, GraphQL and Postman candidate probed on China Mobile hosts returned an HTML single-page-app shell, 403, 404 or 500. This repository therefore has no `openapi/` directory. See `review.yml` for the full probe log with HTTP statuses.

## Links

- [Website](https://www.chinamobileltd.com/)
- [Developer Portal (OneNET)](https://open.iot.10086.cn/)
- [Documentation](https://open.iot.10086.cn/doc/)
- [Internet Capability Open Platform](https://dev.10086.cn/)
- [Communication Capability Open Platform](https://ct.open.10086.cn/portal/index.action)
- [IoT Card Capability Open Platform](https://api.iot.10086.cn/)
- [China Mobile Research Institute on GitHub](https://github.com/cmri)
