## 一、AWS 上的加拿大 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +1（与美国共享） |
| 短码 (Short Code) | **支持**（$995/月 + $3,000 设置费） |
| 长码 (Long Code) | **支持** |
| Toll-Free（免费号码） | **支持**（$2/月） |
| Sender ID | **不支持** |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| MMS | 支持 |
| 国际发送 | 支持 |

**核心要点：** 加拿大与美国共享 +1 区号，AWS 功能支持全面（短码、长码、Toll-Free）。但加拿大受 **CASL（加拿大反垃圾邮件法）** 约束，这是全球最严格的反垃圾通信法之一，违规处罚极为严重（组织最高 $1,000 万加元）。此外，加拿大是双语国家（英语/法语），在魁北克省发送消息时须考虑法语要求。

---

## 二、费用总览

### 2.1 号码费用

| 费用项目 | 一次性费用 | 月度费用 | 备注 |
|---------|-----------|---------|------|
| 专用短码（Short Code） | $3,000 | $995 | 预置约 16 周 |
| Toll-Free 号码 | $0 | $2 | 可跨美加使用，注册周期约 15 个工作日 |
| 长码（Long Code） | $0 | 因地区而异 | 即时可用 |

### 2.2 每条消息费用

| 号码类型 | 基础价格（AWS） | 运营商费用 | 合计 |
|---------|---------------|-----------|------|
| 长码（Long Code） | $0.00700 | $0.00767 | $0.01467 |
| 短码（Short Code） | $0.02183 | $0.00500 | $0.02683 |
| Toll-Free | $0.00581 | $0.00705 | $0.01286 |

### 2.3 月度消费阈值

新账户默认 **$1.00 USD/月**，需通过 AWS Support Case 申请提高。

---

## 三、与美国共享 +1 区号的注意事项

### 3.1 关键区别

加拿大和美国共享 +1 国家代码，但在 AWS SMS 使用中有以下重要区别：

| 项目 | 美国 | 加拿大 |
|------|------|--------|
| 国家代码 | +1 | +1 |
| 10DLC 适用 | **是** | **否**（10DLC 仅限美国） |
| Short Code | 各自独立 | 各自独立（美国短码不能发加拿大） |
| Toll-Free | 共享（同一 TFN 可发两国） | 共享 |
| 长码 | 支持 | 支持 |
| 核心法规 | TCPA | CASL |

### 3.2 号码区分

- 加拿大号码格式：`+1` + 10位数字（与美国格式完全相同）
- **无法从号码格式本身区分美国和加拿大号码**
- 需通过号码查询服务（HLR Lookup）或用户注册时收集国家信息来区分
- 加拿大区号示例：416（多伦多）、514（蒙特利尔）、604（温哥华）、403（卡尔加里）

### 3.3 Toll-Free 号码跨国使用

- 美国 Toll-Free 号码（800/888/877 等）**可以同时向美国和加拿大发送**
- 这是跨北美发送最简便的方式
- 但需分别遵守两国法规（TCPA + CASL）

### 3.4 Short Code 不互通

- 美国短码**不能**用于向加拿大号码发送
- 加拿大短码**不能**用于向美国号码发送
- 如需在两国都使用短码，必须**分别申请**

---

## 四、号码类型与申请流程

### 4.1 号码类型对比

| 特性 | Long Code | Short Code | Toll-Free |
|------|-----------|------------|-----------|
| 号码格式 | 10位本地号码 | 5-6位短号码 | 800/888/877等 |
| 月费 | 因地区而异 | $995/月 | $2/月 |
| 一次性费用 | 无 | $3,000 设置费 | 无 |
| 吞吐量 | 低（~1 MPS） | 高（100+ MPS） | 中（~3 MPS） |
| 注册周期 | 即时 | 8-12周 | ~15个工作日 |
| 双向 SMS | 支持 | 支持 | 支持 |
| 适用场景 | 小规模/测试 | 高吞吐量/品牌 | 中等规模 |

### 4.2 Short Code 申请流程

加拿大短码需通过 **AWS Support Case** 申请。

#### Support Case 填写指南

**访问地址：**
```
https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase
```

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Severity | General Limits |
| Region | 你使用的 AWS 区域（如 us-east-1） |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型（见下方） |
| Limit Increase Value | **1**（即申请 1 个专用短码） |

**Limit Type 消息类型选项：**
- One Time Password/Two-Factor Authentication
- Promotional/Marketing
- Transactional
- Transactional/Notifications/OTP/2FA

#### Case Description 模板

```
Subject: Request for Dedicated Short Code - Canada (CA)

We are requesting a dedicated SMS short code for Canada (+1).

Company Information:
- Company Name: [公司法定名称]
- Business Number (BN): [加拿大商业号码，如适用]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)

Target Provinces: All Canadian provinces and territories

Expected Monthly Volume: [预估月发送量，如 100,000 messages]

Sample Messages (English):
1. "[Brand] Your verification code is: 385291. Valid for 5 minutes.
   Do not share this code."
2. "[Brand] Your order #CA12345 has been confirmed. Amount: $150.00 CAD.
   Track: https://brand.com/orders/CA12345"
3. "[Brand] Alert: A new sign-in was detected on your account.
   If this wasn't you: https://brand.com/security"

Sample Messages (French - for Quebec):
1. "[Brand] Votre code de verification est: 385291. Valide pendant
   5 minutes. Ne partagez pas ce code."
2. "[Brand] Votre commande #CA12345 a ete confirmee. Montant: 150,00 $ CAD.
   Suivi: https://brand.com/orders/CA12345"

Opt-in Process:
Users register on our website/app and provide their Canadian mobile number.
During registration, they check a consent box stating:
English: "I agree to receive SMS messages from [Brand] for account
verification and transaction notifications. Msg & data rates may apply.
Reply STOP to opt out."
French: "J'accepte de recevoir des messages SMS de [Brand] pour la
verification de compte et les notifications de transactions. Des frais
de messagerie peuvent s'appliquer. Repondez ARRET pour vous desabonner."

Opt-out Mechanism:
Users can reply STOP (English) or ARRET (French) at any time to opt out.
They can also manage preferences at https://brand.com/sms-prefs.

CASL Compliance:
We maintain full CASL compliance including:
- Express consent records with timestamps
- Sender identification in every message
- Functional unsubscribe mechanism
- Contact information for questions

We understand that the setup fee is $3,000 and monthly fee is $995 USD.
We also request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.

Please provide the registration form and estimated timeline.
```

#### 费用

| 费用项 | 金额 |
|--------|------|
| 一次性设置费 | **$3,000 USD** |
| 短码月租 | **$995 USD/月** |
| 每条消息费 | 参考定价 CSV |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

#### 申请时间线

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 提供注册表格 | 1-3个工作日 |
| 填写并提交注册表格 | 取决于你 |
| 运营商审核 | 8-12周 |
| 短码配置完成 | 1-2周 |
| **总计** | **8-12周** |

> **重要：** 费用从向运营商提交申请那一刻开始计费，即使短码尚未配置完成。$3,000 设置费不可退还。

### 4.3 Toll-Free 号码注册

Toll-Free 号码是加拿大市场最具性价比的选择，注册流程与美国一致：

1. 在 AWS 控制台购买 Toll-Free 号码
2. 完成注册表单（公司信息、使用场景、消息模板）
3. 审核周期约 15 个工作日
4. 同一 TFN 可同时向美国和加拿大发送

---

## 五、CASL 法规详解

### 5.1 法律概述

**CASL**（Canada's Anti-Spam Legislation，加拿大反垃圾邮件法）于 2014 年 7 月 1 日生效，是全球最严格的反垃圾通信法之一。由 **CRTC**（加拿大广播电视和电信委员会）、**竞争局**和**隐私专员办公室**三个机构联合执行。

CASL 的全称也称为 "Fighting Internet and Wireless Spam Act"。

### 5.2 核心要求

| 要求 | 详情 |
|------|------|
| **明确同意（Express Consent）** | 发送商业电子消息（CEM）前必须获得接收者的**明确同意** |
| **默示同意（Implied Consent）** | 仅在特定条件下有效（如现有客户关系），且有时效限制 |
| **发送者识别** | 每条消息必须明确标识发送者身份 |
| **联系信息** | 必须提供发送者的联系方式 |
| **退订机制** | 必须提供功能性的退订方式 |
| **退订处理时间** | 收到退订请求后 **10 个工作日内**处理 |
| **同意记录** | 必须保留完整的同意记录（举证责任在发送者） |

### 5.3 明确同意 vs 默示同意

#### 明确同意（Express Consent）

- **无有效期限制** — 持续有效直到用户撤销
- 必须包含：
  - 请求同意的组织名称
  - 请求同意者的联系信息
  - 将发送消息的用途说明
  - 声明可随时撤销同意
- **举证责任在发送者** — 你必须能证明获得了同意

#### 默示同意（Implied Consent）

仅在以下情况适用，且有**时间限制**：

| 情形 | 有效期 |
|------|--------|
| 最近 2 年内有商业交易 | 交易后 2 年 |
| 最近 6 个月内有商业咨询 | 咨询后 6 个月 |
| 已公开的商业联系信息 | 需与消息内容相关 |

> **建议：** 始终争取获得明确同意，不要依赖默示同意。

### 5.4 罚款金额

| 违规主体 | 最高罚款 |
|---------|---------|
| **组织** | **$10,000,000 CAD（约 $7,500,000 USD）** |
| **个人** | **$1,000,000 CAD（约 $750,000 USD）** |
| 集体诉讼 | 无上限 |

### 5.5 CASL 与 TCPA 对比

| 项目 | CASL（加拿大） | TCPA（美国） |
|------|---------------|-------------|
| 同意要求 | 明确同意（更严格） | 明确书面或口头同意 |
| 默示同意 | 有条件允许，有时效 | 无明确规定 |
| 退订处理时间 | 10 个工作日 | 合理时间（通常即时） |
| 发送者识别 | 强制 | 强制 |
| 举证责任 | **在发送者** | 在发送者 |
| 组织罚款 | $10M CAD | 按消息数 $500-$1,500/条 |

---

## 六、监管机构

### 6.1 三大执法机构

| 机构 | 英文名 | 职责 |
|------|--------|------|
| **CRTC** | Canadian Radio-television and Telecommunications Commission | 电信和广播监管，CASL 主要执法机构 |
| **竞争局** | Competition Bureau | 虚假/误导性商业行为 |
| **OPC** | Office of the Privacy Commissioner | 个人信息保护 |

### 6.2 CWTA（加拿大无线电信协会）

CWTA（Canadian Wireless Telecommunications Association）是加拿大无线行业自律组织，管理：
- 短码注册和审批
- 通用短码（如 STOP/HELP 关键词）
- 行业最佳实践指南

---

## 七、加拿大数据保护法 (PIPEDA)

### 7.1 法律概述

**PIPEDA**（Personal Information Protection and Electronic Documents Act，个人信息保护和电子文件法）是加拿大联邦层面的数据保护法律。部分省份有自己的补充法律。

### 7.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **用户同意** | 收集、使用和披露个人信息须获得有意义的同意 |
| **目的限制** | 仅为声明的目的收集和使用个人信息 |
| **访问权** | 用户有权访问其个人信息 |
| **更正权** | 用户可请求更正不准确的信息 |
| **数据保留** | 不应超过必要期限 |
| **安全保障** | 必须采取适当措施保护个人信息 |
| **透明性** | 必须公开隐私政策和实践 |

### 7.3 省级补充法律

| 省份 | 法律 | 说明 |
|------|------|------|
| 魁北克 | **Law 25** (Loi 25) | 2023年起分阶段生效，类似 GDPR |
| 阿尔伯塔 | PIPA | 私营部门隐私法 |
| 不列颠哥伦比亚 | PIPA | 私营部门隐私法 |

> **魁北克 Law 25 特别注意：** 魁北克省自 2023 年起实施了更严格的隐私法（Law 25），要求明确同意、数据保护影响评估、数据泄露通知等。在魁北克省开展 SMS 业务需额外关注。

### 7.4 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| PIPEDA 行政罚款 | 最高 $100,000 CAD |
| 魁北克 Law 25 | 最高 $25,000,000 CAD 或全球营业额 4% |
| 联邦层面 | 可能涉及刑事处罚 |

---

## 八、运营商与号码格式

### 8.1 主要运营商

| 运营商 | 市场份额（约） | 备注 |
|--------|-------------|------|
| Rogers | ~33% | 含 Fido、Chatr 子品牌 |
| Bell | ~30% | 含 Virgin Plus、Lucky Mobile |
| Telus | ~28% | 含 Koodo、Public Mobile |
| SaskTel | ~3% | 萨斯喀彻温省区域运营商 |
| 其他 MVNO | ~6% | Freedom Mobile、Videotron 等 |

### 8.2 号码格式

#### 标准格式
```
+1XXXXXXXXXX      ← 国家代码 +1 + 10位号码（含3位区号）
```

#### 主要区号示例

| 城市/地区 | 区号 | 示例号码 |
|----------|------|---------|
| 多伦多 | 416, 647 | +1416XXXXXXX |
| 蒙特利尔 | 514, 438 | +1514XXXXXXX |
| 温哥华 | 604, 778 | +1604XXXXXXX |
| 卡尔加里 | 403, 587 | +1403XXXXXXX |
| 渥太华 | 613, 343 | +1613XXXXXXX |

#### AWS SMS 号码格式要求
- 必须使用 **E.164 格式**：`+1` 后跟 10 位数字
- 不含空格、破折号或括号
- 总长度 12 位（含 `+1`）
- 与美国号码格式完全相同

### 8.3 运营商技术细节

- 不支持 Sender ID（消息显示为号码）
- 运营商有垃圾信息自动过滤
- 支持号码携带（携号转网）
- 固定电话不可达
- STOP / HELP 关键词由运营商级别处理
- 法语区建议同时支持 ARRET（法语 STOP）

---

## 九、双语要求（英语/法语）

### 9.1 法律背景

加拿大是双语国家。**魁北克省《法语宪章》(Charter of the French Language / Charte de la langue francaise)** 要求在魁北克省的商业通信中使用法语，法语内容须至少与英语同等显著。

### 9.2 实践建议

| 场景 | 建议 |
|------|------|
| 全国发送 | 根据用户语言偏好选择英语或法语 |
| 魁北克省 | **必须提供法语版本** |
| 其他省份 | 英语为主，提供法语选项更佳 |
| 用户偏好未知 | 建议在注册时收集语言偏好 |

### 9.3 法语 Opt-in/Opt-out 关键词

| 功能 | 英语 | 法语 |
|------|------|------|
| 退订 | STOP | ARRET |
| 帮助 | HELP | AIDE |
| 确认订阅 | YES | OUI |

> **建议：** 在系统中同时支持英语和法语关键词。

---

## 十、消息模板范例

### 10.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本 ASCII 字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含法语重音字符等 |

**法语重音字符与 GSM-7 兼容性：**

| 字符 | GSM-7 兼容 | 建议 |
|------|-----------|------|
| e grave (`e`) | **是** | 可保留 |
| e acute (`e`) | **是** | 可保留 |
| u grave (`u`) | **是** | 可保留 |
| a grave (`a`) | **是** | 可保留（GSM-7 位置 0x7F） |
| c cedilla (`c`) | **是** | 可保留（GSM-7 位置 0x09） |
| i circumflex (`i`) | **否** | 去掉重音写 `i` |
| o circumflex (`o`) | **否** | 去掉重音写 `o` |

> **建议：** 法语消息中可去掉非 GSM-7 兼容的重音符号（加拿大法语用户在短信中普遍接受无重音写法），以保持 GSM-7 编码（160字符/条），降低成本。

### 10.2 OTP 验证码模板

**英语版：**
```
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.
```
(85字符，1条消息)

**法语版（GSM-7 兼容，去除部分重音）：**
```
[Brand] Votre code de verification est: 385291. Valide pendant
5 minutes. Ne partagez pas ce code.
```
(92字符，1条消息)

### 10.3 交易通知模板

**订单确认 — 英语：**
```
[Brand] Your order #CA12345 has been confirmed. Amount: $150.00 CAD.
Track: https://brand.com/orders/CA12345
```
(100字符，1条)

**订单确认 — 法语（GSM-7）：**
```
[Brand] Votre commande #CA12345 a ete confirmee. Montant: 150,00 $ CAD.
Suivi: https://brand.com/orders/CA12345
```
(104字符，1条)

**配送通知 — 英语：**
```
[Brand] Your order #CA12345 has shipped. Tracking:
https://brand.com/track/CA12345 Est. delivery: Apr 10.
```
(99字符，1条)

**配送通知 — 法语（GSM-7）：**
```
[Brand] Votre commande #CA12345 a ete expediee. Suivi:
https://brand.com/track/CA12345 Livraison estimee: 10 avr.
```
(110字符，1条)

**账户安全警报 — 英语：**
```
[Brand] Alert: New sign-in detected on your account from a new device.
If this wasn't you: https://brand.com/security
```
(107字符，1条)

**账户安全警报 — 法语（GSM-7）：**
```
[Brand] Alerte: Nouvelle connexion detectee sur votre compte.
Si ce n'etait pas vous: https://brand.com/security
```
(103字符，1条)

### 10.4 营销消息模板（需明确同意）

**促销活动 — 英语：**
```
[Brand] Sale: 20% off your next purchase! Use code SAVE20.
Valid through 04/15. Shop: https://brand.com/sale
Reply STOP to opt out.
```
(126字符，1条)

**促销活动 — 法语（GSM-7）：**
```
[Brand] Solde: 20% de rabais sur votre prochain achat!
Code PROMO20. Valide jusqu'au 15/04.
Magasiner: https://brand.com/sale
Repondez ARRET pour vous desabonner.
```
(157字符，2条，因换行和内容长度)

> **注意：** 法语消息通常比英语长 10-20%，需更加注意字符数控制。

### 10.5 Opt-in 确认模板

**英语：**
```
[Brand] You're subscribed to [Brand] alerts. Msg frequency varies.
Msg & data rates may apply. Reply HELP for help, STOP to cancel.
```
(128字符，1条)

**法语（GSM-7）：**
```
[Brand] Vous etes abonne aux alertes [Brand]. Frequence variable.
Des frais peuvent s'appliquer. AIDE pour aide, ARRET pour annuler.
```
(129字符，1条)

### 10.6 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 英语和法语（魁北克省必须提供法语） |
| 编码 | 尽量使用 GSM-7，去掉非兼容重音 |
| STOP 提示 | 英语用 STOP，法语用 ARRET |
| 发送者识别 | CASL 要求每条消息标识发送者 |
| 联系信息 | CASL 要求提供联系方式 |
| 退订机制 | 必须功能性可用，10 个工作日内处理 |
| 货币 | 使用 CAD 或 `$`（`$` 在 GSM-7 中） |
| 法语消息长度 | 比英语长 10-20%，注意字符数 |

---

## 十一、定价信息

### 11.1 号码费用

| 号码类型 | 月费 | 一次性费用 |
|----------|------|-----------|
| Long Code | 因地区而异 | 无 |
| Short Code | $995/月 | $3,000 设置费 |
| Toll-Free | $2/月 | 无 |

### 11.2 消息费用（参考值）

| 费用类型 | 金额 |
|----------|------|
| AWS 基础消息费 | ~$0.00581/条 |
| 运营商费（Carrier Fee） | ~$0.00705/条 |
| **合计（约）** | **~$0.01286/条** |

> 实际费率请在 AWS 控制台确认或下载定价 CSV：`https://aws.amazon.com/end-user-messaging/pricing/`

### 11.3 默认消费阈值

- 新账户默认 **$1.00 USD/月**
- 需通过 Support Case 申请提高

### 11.4 成本优化建议

1. **使用 Toll-Free 号码** — 最具性价比（$2/月），可同时覆盖美国和加拿大
2. **使用 GSM-7 编码** — 法语消息去掉非兼容重音，每条 160 字符
3. **精简消息内容** — 法语比英语长，需特别注意字符数
4. **维护号码列表** — 定期清理无效号码
5. **收集语言偏好** — 避免向英语用户发送法语（浪费更长的法语消息）

---

## 十二、合规清单

在加拿大发送 SMS 前，确保完成：

- [ ] 获取所有接收者的**明确同意（Express Consent）**
- [ ] 同意请求中包含：组织名称、联系方式、消息用途、撤销同意的权利
- [ ] 保留完整的同意记录（日期、时间、方式、内容）
- [ ] 每条消息明确标识发送者身份（CASL 要求）
- [ ] 每条消息提供联系信息
- [ ] 实现功能性退订机制（STOP / ARRET）
- [ ] 退订请求在 **10 个工作日内**处理
- [ ] 魁北克省用户提供法语消息版本
- [ ] 遵守 PIPEDA 数据保护要求
- [ ] 如向魁北克省发送，遵守 Law 25 要求
- [ ] 不依赖默示同意发送营销消息（建议始终使用明确同意）
- [ ] 发送时间限制在收件人当地 9:00-20:00
- [ ] 事务性消息不混入营销内容
- [ ] 正确格式化号码（E.164：`+1XXXXXXXXXX`）
- [ ] 区分美国和加拿大号码（如需不同处理）
- [ ] 申请提高默认 $1.00 消费阈值
- [ ] 使用品牌自有域名 URL
- [ ] 系统同时支持英语和法语关键词（STOP/ARRET、HELP/AIDE）

---

## 十三、发送时间建议

| 消息类型 | 建议发送时段 | 说明 |
|---------|------------|------|
| OTP / 安全警报 | 任何时间 | 用户触发，不受限制 |
| 交易通知 | 08:00-22:00 本地时间 | 订单确认等可适当延长 |
| 营销消息 | 09:00-20:00 本地时间 | 避免周日和国定假日 |

**时区注意：** 加拿大横跨 6 个时区：
- 纽芬兰时间 (NST/NDT): UTC-3:30
- 大西洋时间 (AST/ADT): UTC-4
- 东部时间 (EST/EDT): UTC-5
- 中部时间 (CST/CDT): UTC-6
- 山地时间 (MST/MDT): UTC-7
- 太平洋时间 (PST/PDT): UTC-8

---

## 十四、Support Case 模板汇总

### 14.1 生产环境升级 + 消费限额提升

```
Subject: Production Access & Spend Limit Increase - Canada SMS

We are requesting production access for SMS messaging to Canada (+1).

Region: us-east-1
Resource Type: General Limits
Limit Type: SMS Production Access
Limit Increase Value: 1

Company Information:
- Company Name: [公司名称]
- Website: [网站]
- Use Case: OTP verification and transactional notifications

We also request an increase to our monthly SMS spend limit from
$1.00 to $[desired amount] USD.

Expected Monthly Volume: [预估月发送量]
Target Number Type: Toll-Free / Short Code / Long Code

CASL Compliance:
We maintain full compliance with Canada's Anti-Spam Legislation (CASL):
- Express consent obtained from all recipients
- Sender identification in every message
- Functional unsubscribe mechanism (STOP/ARRET)
- Contact information provided
- Consent records maintained with timestamps

Opt-in Process:
[描述用户如何同意接收消息]

Sample Messages:
English:
1. "[Brand] Your verification code is: 385291. Valid for 5 min."
2. "[Brand] Your order #12345 has been confirmed. Amount: $150.00 CAD."

French (Quebec):
1. "[Brand] Votre code de verification est: 385291. Valide 5 min."
2. "[Brand] Votre commande #12345 a ete confirmee. Montant: 150,00 $ CAD."
```

---

## 十五、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| CASL 违规处罚（$10M CAD） | **极高** | 始终获取明确同意，保留完整记录，举证责任在发送者 |
| 魁北克语言法合规 | 高 | 为魁北克用户提供法语版消息 |
| 魁北克 Law 25 数据保护 | 高 | 进行数据保护影响评估，遵守增强的隐私要求 |
| 美加号码混淆 | 中 | 实施号码查询服务区分两国号码 |
| 短码申请被拒 | 中 | 提供详尽的 CASL 合规证明和使用说明 |
| 短码费用高（$995/月 + $3,000） | 中 | 评估是否可用 Toll-Free 替代（$2/月） |
| 法语消息超长导致拼接 | 低 | 控制法语消息在 160 字符内，使用 GSM-7 编码 |
| 默示同意过期 | 中 | 定期审查同意状态，在到期前请求明确同意 |
| 运营商内容过滤 | 低 | 使用标准模板，品牌域名，避免可疑内容 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。加拿大 CASL 法规、CRTC 规则、PIPEDA/Law 25 隐私法和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询加拿大当地法律顾问以确保合规。
