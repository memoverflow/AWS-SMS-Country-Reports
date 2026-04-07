# 澳大利亚 (AU, +61) AWS SMS 发送专题研究报告

> 调研日期：2026年4月7日
> 数据来源：AWS 官方文档、ACMA 官网、澳大利亚法律资料

---

## 一、AWS 上的澳大利亚 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +61 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **支持**（$22/月，需注册） |
| Sender ID | **支持（需注册，需 LOA）** |
| Toll-free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| MMS | 不支持（转换为 SMS+URL） |
| 国际发送 | 支持 |

**核心特点：** 澳大利亚在 AWS 上支持长码和 Sender ID 两种发送方式，但 Sender ID 必须通过 LOA 注册。不支持短码，因此高吞吐量场景需通过多个长码实现。支持双向 SMS，可使用标准 STOP/HELP 关键字退订。

---

## 二、Sender ID 注册详细流程

### 2.1 注册方式

澳大利亚 Sender ID 需通过 AWS 控制台提交注册，并提供签署的 LOA（授权书）。

### 2.2 LOA 模板

**下载地址（AWS 提供）：**
```
Australia_SenderId_LetterOfAuthorization.zip
```

从 AWS End User Messaging SMS 控制台的 Sender ID 注册页面下载此模板。

### 2.3 注册所需材料

| 材料 | 说明 |
|------|------|
| **LOA 授权书** | 下载 `Australia_SenderId_LetterOfAuthorization.zip`，填写并签署 |
| **公司识别号** | 澳大利亚公司：ABN / ACN / ARBN / ICN；国际公司：EIN / VAT |
| 公司法定名称 | 与注册文件一致 |
| 公司地址 | 完整注册地址 |
| 联系人信息 | 姓名、邮箱、电话 |
| 公司网站 | 官方网站 URL |
| Sender ID | 3-11个字母数字字符（如 "MyBrand"） |
| 消息样本 | 1-3条英语消息模板 |
| 使用场景描述 | OTP / 事务性 / 营销等 |
| 预估月发送量 | 月度消息数量 |

#### 公司识别号说明

| 识别号类型 | 适用范围 | 说明 |
|-----------|---------|------|
| **ABN** (Australian Business Number) | 澳大利亚企业 | 11位数字，由 ATO 颁发 |
| **ACN** (Australian Company Number) | 澳大利亚公司 | 9位数字，由 ASIC 颁发 |
| **ARBN** (Australian Registered Body Number) | 在澳注册的外国公司 | 由 ASIC 颁发 |
| **ICN** (Incorporation Number) | 澳大利亚公司 | 公司注册号 |
| **EIN** (Employer Identification Number) | 美国公司 | 用于国际公司在澳注册 |
| **VAT** (Value Added Tax Number) | 其他国际公司 | 增值税登记号 |

### 2.4 控制台注册步骤

1. 登录 AWS End User Messaging SMS 控制台
2. 导航至 **Registrations** → **Create registration**
3. 选择 **Sender ID registration** → 选择 **Australia**
4. 填写公司信息和 Sender ID
5. 上传签署的 LOA 文件（PDF格式，最大400KB）
6. 填写消息样本和使用场景
7. 提交注册，等待审核

### 2.5 注册时间线

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 初审 | 1-3个工作日 |
| 运营商审核 | 1-4周 |
| Sender ID 激活 | 1-2个工作日 |
| **总计** | **1-6周** |

---

## 三、长码申请流程

### 3.1 申请方式

澳大利亚长码可通过 AWS 控制台直接购买，也可通过 Support Case 申请。

### 3.2 控制台购买步骤

1. 登录 AWS End User Messaging SMS 控制台
2. 导航至 **Phone numbers** → **Request phone number**
3. 选择国家 **Australia (+61)**
4. 选择 **Long code**
5. 选择消息类型（Transactional / Promotional）
6. 确认购买

### 3.3 Support Case 填写指南（如需额外配置）

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
| Region | 你使用的 AWS 区域（如 ap-southeast-2） |
| Resource Type | Dedicated SMS Long Codes |
| Limit Type | 选择消息类型（见下方） |
| Limit Increase Value | **1**（即申请 1 个专用长码） |

**Limit Type 消息类型选项：**
- One Time Password/Two-Factor Authentication
- Promotional/Marketing
- Transactional
- Transactional/Notifications/OTP/2FA

### 3.4 Case Description 模板

```
Subject: Request for Dedicated Long Code - Australia (AU)

We are requesting a dedicated SMS long code for Australia (+61).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- ABN/ACN/EIN/VAT: [公司识别号]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)

Target Carriers: Telstra, Optus, Vodafone (TPG Telecom)

Expected Monthly Volume: [预估月发送量，如 50,000 messages]

Sample Messages (English):
1. "[Brand] Your verification code is: 385291. Valid for 5 minutes.
   Do not share this code with anyone."
2. "[Brand] Your order #AU12345 has been confirmed. Amount: AUD 150.00.
   Details: https://brand.com/orders/AU12345"
3. "[Brand] Alert: A new device login was detected on your account.
   If this wasn't you: https://brand.com/security"

Opt-in Process:
Users register on our website/app and provide their Australian mobile number.
During registration, they check a consent box stating: "I agree to receive
SMS messages from [Brand] for account verification and transaction
notifications."

Opt-out Mechanism:
Users can reply STOP to unsubscribe from SMS messages at any time.
They can also opt out via their account settings or by emailing
optout@brand.com.

We also request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.
```

### 3.5 费用说明

| 费用项 | 说明 |
|--------|------|
| 长码月租 | $22 USD/月 |
| 每条消息费 | 需下载 AWS 定价 CSV 或联系销售 |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

---

## 四、澳大利亚电信监管 (ACMA)

### 4.1 监管机构

**ACMA**（Australian Communications and Media Authority，澳大利亚通信和媒体管理局）是澳大利亚通信和媒体监管机构，负责监管电信服务、广播和互联网内容。

### 4.2 核心法规：Spam Act 2003

**Spam Act 2003** 是澳大利亚规范商业电子消息（包括 SMS）的核心法律。

#### 三大核心原则

| 原则 | 要求 |
|------|------|
| **Consent（同意）** | 发送商业电子消息前必须获得接收者同意（明示或推定） |
| **Identify（标识）** | 消息必须包含发送者的准确身份信息和联系方式 |
| **Unsubscribe（退订）** | 必须提供功能正常的退订机制 |

#### 同意类型

| 类型 | 说明 | 示例 |
|------|------|------|
| **Express consent（明示同意）** | 接收者明确同意接收消息 | 注册时勾选同意框 |
| **Inferred consent（推定同意）** | 基于已有业务关系推定 | 现有客户、近期购买者 |

#### 推定同意的有效期

- 发布的联系方式：推定同意持续有效（除非取消）
- 业务关系：购买后合理时间内（通常2年）
- 推定同意不适用于非直接相关的营销

### 4.3 禁止/限制内容

| 类别 | 限制程度 |
|------|---------|
| 未经同意的商业消息 | **严格禁止** |
| 欺诈/误导性内容 | 禁止 |
| 成人内容 | 严格限制 |
| 赌博推广 | 需持牌运营商，各州规定不同 |
| 健康产品虚假宣传 | 禁止 |
| 金融诈骗/钓鱼 | 禁止 |
| 隐藏发送者身份 | 禁止 |

### 4.4 发送时间建议

虽无明确法定限制，行业最佳实践：
- **推荐发送时段：** 09:00-20:00 AEST/AEDT
- 注意澳大利亚横跨多个时区（UTC+8 至 UTC+11）
- 紧急/交易类消息除外
- 避免周日和国定假日发送营销消息

### 4.5 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| 民事处罚（个人） | 每次违规最高 **$444,000 AUD**（约 $290,000 USD） |
| 民事处罚（企业） | 每次违规最高 **$2,220,000 AUD**（约 $1,450,000 USD） |
| 每日持续违规 | 可累计罚款 |
| 执行令 | ACMA 可发布强制合规令 |
| 诉讼 | ACMA 可向联邦法院提起诉讼 |

> **注意：** 澳大利亚对垃圾短信的处罚极为严厉，ACMA 积极执法。2024-2025年间已对多家公司开出百万级罚单。

---

## 五、澳大利亚数据保护法 (Privacy Act 1988)

### 5.1 法律概述

**Privacy Act 1988** 是澳大利亚核心数据保护法律，包含 **13 项澳大利亚隐私原则（APPs）**，规范年营业额超过 300 万澳元的组织对个人信息的处理。

### 5.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **收集限制 (APP 3)** | 仅收集合理必要的个人信息（如手机号码） |
| **通知义务 (APP 5)** | 收集时告知收集目的、使用方式、披露对象 |
| **使用限制 (APP 6)** | 仅将信息用于收集时告知的目的 |
| **直接营销 (APP 7)** | 个人可随时要求退出直接营销；每条消息须包含退出方式 |
| **跨境披露 (APP 8)** | 向海外披露个人信息前需确保接收方遵守同等保护标准 |
| **数据质量 (APP 10)** | 确保个人信息准确、最新 |
| **数据安全 (APP 11)** | 采取合理措施保护个人信息安全 |
| **访问权 (APP 12)** | 个人有权访问其个人信息 |
| **更正权 (APP 13)** | 个人有权要求更正不准确的信息 |

### 5.3 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| 严重或反复违规（个人） | 最高 **$2,500,000 AUD** |
| 严重或反复违规（企业） | 最高 **$50,000,000 AUD**、违规相关收益的3倍、或调整后年营业额的30%（取最高值） |
| 民事处罚 | 每次违规最高 $2,500,000 AUD |

### 5.4 Do Not Call Register（谢绝来电登记）

- 澳大利亚运营 **Do Not Call Register**（DNCR）
- 公民可免费注册号码拒绝商业营销电话和短信
- **发送营销 SMS 前必须检查号码是否在 DNCR 中**
- 违规发送最高罚款 **$2,220,000 AUD**

---

## 六、运营商与号码格式

### 6.1 主要运营商

| 运营商 | 母公司 | 市场份额（约） | 备注 |
|--------|--------|-------------|------|
| **Telstra** | Telstra Corporation | ~40% | 最大运营商，覆盖最广 |
| **Optus** | Singapore Telecommunications (Singtel) | ~25% | 第二大运营商 |
| **TPG Telecom** (Vodafone AU) | TPG Telecom | ~20% | 2020年 Vodafone AU 与 TPG 合并 |
| 其他 MVNO | 各异 | ~15% | Boost、Amaysim、Woolworths 等 |

### 6.2 号码格式

#### 基本结构
- 国家代码：+61
- 手机号码：以 **04** 开头（国内格式）或 **4** 开头（国际格式）
- 总长度：9位数字（不含国家代码）

#### 格式示例

| 格式 | 示例 | 说明 |
|------|------|------|
| 国际格式（AWS 使用） | `+61412345678` | 去掉前导0 |
| 国内格式 | `0412 345 678` | 以04开头 |

#### 正确的 AWS SMS 号码格式
```
+61412345678      ← 手机号码（去掉前导0）
+61498765432      ← 手机号码
```

> **SMS 号码必须去掉前导 0！** `+610412345678` 是错误格式，正确格式是 `+61412345678`。

### 6.3 运营商技术细节

- Sender ID 需注册后才能使用
- 运营商有垃圾信息自动过滤
- 支持号码携带（携号转网）
- 固定电话不可达（固话以 02/03/07/08 开头）
- 支持消息拼接
- 双向 SMS 支持 STOP/HELP 关键字

---

## 七、消息模板范例（英语）

### 7.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本拉丁字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含特殊字符/emoji |

**英语消息编码说明：**

英语文本通常完全兼容 GSM-7 编码，除非包含以下字符：
- 商标符号 ®、™
- Emoji 表情
- 特殊货币符号（`$` 在 GSM-7 中兼容）

> **建议：** 英语消息天然适合 GSM-7 编码。避免使用商标符号和 emoji，可将每条消息从70字符提升到160字符，显著降低成本。

### 7.2 OTP 验证码模板

**GSM-7 兼容版（推荐，160字符/条）：**
```
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.
```
(84字符，1条消息)

### 7.3 交易通知模板

**订单确认：**
```
[Brand] Your order #AU12345 has been confirmed. Amount: AUD 150.00.
Estimated delivery: 10 Apr. Details: https://brand.com/o/AU12345
```
(120字符，1条)

**配送通知 - 已发货：**
```
[Brand] Your order #AU12345 has been shipped. Track:
https://brand.com/track/AU12345 Estimated delivery: 10 Apr.
```
(105字符，1条)

**配送通知 - 派送中：**
```
[Brand] Your package is out for delivery. Expected arrival: today 2-5pm.
Track: https://brand.com/track/AU12345
```
(106字符，1条)

**配送通知 - 已签收：**
```
[Brand] Your order #AU12345 has been delivered.
Rate your experience: https://brand.com/review/AU12345
```
(95字符，1条)

**付款确认：**
```
[Brand] Payment received: AUD 150.00 (order #AU12345).
Receipt: https://brand.com/receipt/AU12345
```
(89字符，1条)

**账户安全警报：**
```
[Brand] Alert: New device login detected on your account.
If this wasn't you: https://brand.com/security
```
(93字符，1条)

### 7.4 营销消息模板（需 opt-in 同意）

**促销活动：**
```
[Brand] 20% off your next purchase! Use code SAVE20. Valid until 15 Apr.
Shop: https://brand.com/sale Reply STOP to opt out.
```
(119字符，1条)

**新品通知：**
```
[Brand] New collection just dropped! Be the first to shop.
Browse: https://brand.com/new Reply STOP to opt out.
```
(107字符，1条)

**会员积分提醒：**
```
[Brand] You have 500 points expiring on 30 Apr.
Redeem now: https://brand.com/rewards Reply STOP to opt out.
```
(106字符，1条)

### 7.5 Opt-in 确认模板

**首次订阅确认：**
```
[Brand] Welcome! You'll receive account alerts and notifications.
Frequency varies. Reply STOP to unsubscribe. HELP for help.
```
(117字符，1条)

### 7.6 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 使用英语 |
| 编码 | GSM-7 即可（英语天然兼容） |
| 避免商标符号 | 不要使用 ® ™，会触发 UCS-2 |
| 避免 emoji | 会触发 UCS-2，每条仅70字符 |
| 退订指引 | 每条营销消息必须包含 Reply STOP |
| 发送者标识 | 使用注册的 Sender ID 或长码 |
| 货币符号 | 使用 `AUD` 或 `$`（`$` 在 GSM-7 中兼容） |

---

## 八、定价信息

### 8.1 AWS 定价

| 费用项 | 金额 |
|--------|------|
| 长码月租 | $22 USD/月 |
| Sender ID 月费 | 通常免费（需确认） |
| 每条消息费 | 需下载 AWS 定价 CSV |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

**获取完整定价：**
1. 下载 AWS 定价 CSV：`https://aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

### 8.2 成本优化建议

1. **使用 GSM-7 编码** — 英语消息通常天然兼容，每条160字符
2. **避免商标符号和 emoji** — 防止触发 UCS-2 导致每条仅70字符
3. **精简消息内容** — 保持单条消息在160字符内避免拼接
4. **维护号码列表** — 定期清理无效号码，检查 DNCR
5. **使用 Sender ID** — 比长码更灵活，且通常无月费
6. **低峰时段批量发送** — 可能获得更好的投递率

---

## 九、合规清单

在澳大利亚发送 SMS 前，确保完成：

- [ ] 获取所有接收者的明确 opt-in 同意（Express 或 Inferred consent）
- [ ] 完成 Sender ID 注册（提交 LOA）或购买长码
- [ ] 遵守 Spam Act 2003 三大原则（Consent、Identify、Unsubscribe）
- [ ] 遵守 Privacy Act 1988 澳大利亚隐私原则（APPs）
- [ ] 检查 Do Not Call Register（DNCR）
- [ ] 消息内容使用英语
- [ ] 每条消息包含发送者身份标识
- [ ] 每条营销消息包含退订方式（Reply STOP 或退订链接）
- [ ] 退订机制功能正常，5个工作日内生效
- [ ] 发送时间限制在 09:00-20:00 AEST/AEDT
- [ ] 正确格式化号码（+61 + 号码，**去掉前导 0**）
- [ ] 使用 GSM-7 编码（避免商标符号和 emoji）
- [ ] 保留同意和退出记录
- [ ] 响应数据访问/更正请求（30天内）
- [ ] 申请提高默认 $1.00 USD/月消费阈值
- [ ] 不在事务性消息中混入营销内容

---

## 十、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| Sender ID LOA 被拒 | 中 | 确保公司信息准确，LOA 签署规范 |
| Spam Act 违规罚款 | **极高** | 严格遵守三大原则，每条消息含退订方式 |
| Privacy Act 违规 | **极高** | 遵守 APPs，保护个人信息安全 |
| DNCR 违规 | 高 | 发送前必须检查 DNCR |
| 运营商内容过滤 | 中 | 避免可疑内容，使用标准模板，使用品牌域名 |
| 号码格式错误（前导0） | 中 | 实施严格的号码格式验证（去掉前导0） |
| 不支持短码导致吞吐量受限 | 低 | 使用多个长码或 Sender ID 分散发送 |
| Unicode 导致成本翻倍 | 低 | 避免商标符号和 emoji |
| 跨时区发送不当 | 低 | 根据接收者所在时区调整发送时间 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。澳大利亚法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询澳大利亚当地法律顾问以确保合规。
