## 一、AWS 上的新加坡 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +65 |
| 短码 (Short Code) | 支持 |
| 长码 (Long Code) | 支持 |
| Sender ID | **支持（必须在 SSIR 注册）** |
| Toll-free | 不支持 |
| 双向 SMS | 支持 |
| 消息拼接 | 支持 |
| MMS | 不支持 |
| 国际发送 | 支持 |

**核心要求：** 新加坡要求所有使用 Sender ID 发送 A2P SMS 的企业必须在 SSIR（Singapore SMS Sender ID Registry）注册。未注册的 Sender ID 会被自动替换为 "LIKELY-SCAM"。如使用短码或长码发送，则无需 SSIR 注册。

---

## 二、费用总览

### 2.1 号码费用

| 费用项目 | 一次性费用 | 月度费用 | 备注 |
|---------|-----------|---------|------|
| Sender ID | 免费 | 免费 | 需完成 SSIR 注册（三步流程） |

> 新加坡也支持短码和长码，但费用需联系 AWS 确认。Sender ID 本身免费。

### 2.2 每条消息费用

| 发送方式 | 每条消息费用 | 备注 |
|---------|------------|------|
| 所有号码类型 | $0.03918 | — |

### 2.3 月度消费阈值

新账户默认 **$1.00 USD/月**，需通过 AWS Support Case 申请提高。

## 三、SSIR 注册详细流程

### 3.1 SSIR 概述

SSIR（Singapore SMS Sender ID Registry）于2022年3月启用，由 SGNIC（Singapore Network Information Centre）管理，SGNIC 隶属于 IMDA（Info-communications Media Development Authority，资讯通信媒体发展管理局）。SSIR 旨在打击短信诈骗，保护消费者。

注册分为**三步**，必须按顺序完成。

### 3.2 第一步：获取 UEN（Unique Entity Number）

**什么是 UEN：**
UEN 是新加坡企业的唯一实体编号，由 ACRA（Accounting and Corporate Regulatory Authority，会计与企业监管局）颁发。

**获取方式：**
- 如果你的公司已在新加坡注册（如新加坡本地公司、分公司、代表处），则已有 UEN
- 如果是外国公司，需通过以下方式之一获取：
  - 在新加坡注册分公司或代表处
  - 通过新加坡本地合作伙伴代为注册
  - 联系 ACRA 了解外国实体注册流程

**UEN 格式示例：**
```
202312345A      ← 本地公司
T08FC1234A      ← 外国公司分支
```

> **注意：** UEN 是整个 SSIR 注册流程的基础，没有 UEN 无法继续后续步骤。

### 3.3 第二步：在 AWS 控制台提交注册

**访问地址：** https://console.aws.amazon.com/sms-voice/

**步骤：**

1. **创建注册**
   - 导航至 **Registrations** → **Create registration**
   - 选择 **Singapore sender ID registration**

2. **填写 Sender ID Info**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | Sender ID | 是 | 请求的 Sender ID |
   | Registering on behalf of another brand/entity? | 是 | 是否代另一品牌/实体注册 |
   | Letter of Authorization Image | 代注册时必填 | 如代注册，需上传 LOA（PNG 格式，最大 400KB） |
   | Sender ID Connection | 否 | 描述 Sender ID 与公司之间的关联 |

3. **填写 Company Info**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | Company Name | 是 | 公司法定名称 |
   | Tax ID | 是 | **新加坡 UEN（Unique Entity Number）** |
   | Company Website | 是 | 公司网站 URL |
   | Address 1 | 是 | 公司总部街道地址 |
   | Address 2 | 否 | 楼层/套房号 |
   | City | 是 | 城市 |
   | State/Province | 是 | 州/省 |
   | Zip Code/Postal Code | 是 | 邮编 |
   | Country | 是 | 两位 ISO 国家代码（如 SG） |

4. **填写 Contact Info**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | First Name | 是 | 联系人名 |
   | Last Name | 是 | 联系人姓 |
   | Support Email | 是 | 联系人邮箱 |
   | Support Phone Number | 是 | 联系人电话 |

5. **填写 Messaging Use Case**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | Monthly SMS Volume | 是 | 预计月发送量（下拉选择） |
   | Use Case Category | 是 | 选项：Two-factor authentication / One-time passwords / Notifications / Polling and surveys / Info on demand / Promotions and Marketing / Other |
   | Use Case Details | 选 Other 时必填 | 补充说明 |

6. **填写 Message Samples**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | Message Sample 1 | 是 | 消息样本 |
   | Message Sample 2 | 否 | 额外样本 |
   | Message Sample 3 | 否 | 额外样本 |

7. **Review and Submit** — 核对所有信息后提交

> **重要：** 必须先完成第二步（AWS 控制台注册），再进行第三步（SGNIC 注册）。

### 3.4 第三步：在 SGNIC 注册

**访问地址：** https://smsregistry.sg/web/login

**步骤：**

1. **登录 SGNIC SMS Registry 门户**
   - 使用 Singpass（新加坡个人身份认证）或 Corppass（企业身份认证）登录
   - 外国公司可能需要通过本地代理登录

2. **注册 Sender ID**
   - 填写公司信息和 Sender ID

3. **添加参与聚合商（关键步骤）**
   - 在注册过程中，**必须将 AMCS SG Private Limited 列为参与聚合商（Participating Aggregator）**
   - AMCS SG Private Limited 是 Amazon 在新加坡的 SMS 聚合实体
   - > **警告：如果未将 AMCS SG Private Limited 列为聚合商，通过 AWS 发送的 SMS 将无法使用已注册的 Sender ID！**

4. **等待审核**
   - SGNIC 审核完成后，注册信号会自动发送至 AWS
   - AWS 控制台中的注册状态会更新

### 3.5 不注册的后果

| 后果 | 详情 |
|------|------|
| Sender ID 被替换为 **"LIKELY-SCAM"** | 未注册的 Sender ID 会被自动替换为全大写的 "LIKELY-SCAM" 显示在收件人手机上 |
| 流量被过滤或屏蔽 | 监管机构和运营商可自行决定过滤或屏蔽未注册流量 |
| 品牌严重受损 | 消息显示为 "LIKELY-SCAM" 将严重损害品牌信誉和用户信任 |
| 替代方案 | 可使用短码或长码发送（不需要 SSIR 注册），但不显示品牌名称 |

### 3.6 需要准备的材料清单

1. **UEN（Unique Entity Number）** — 新加坡企业唯一编号
2. 公司基本信息（名称、地址、网站）
3. 联系人信息
4. Sender ID
5. 预计月发送量
6. 消息样本（1-3条）
7. **LOA（如代另一实体注册）** — PNG 格式，最大 400KB
8. **Singpass/Corppass 账号** — 用于 SGNIC 门户登录
9. 将 **AMCS SG Private Limited** 列为聚合商的确认

### 3.7 申请时间线

| 阶段 | 预计时间 |
|------|---------|
| 获取 UEN（如已有则跳过） | 1-4周（外国公司注册新加坡实体） |
| AWS 控制台提交注册 | 1天 |
| SGNIC 注册 | 1-3个工作日 |
| SGNIC 审核 | 1-2周 |
| AWS 注册状态更新 | 审核完成后自动 |
| **总计（已有 UEN）** | **2-3周** |
| **总计（需获取 UEN）** | **3-7周** |

---

## 四、短码与长码申请

### 4.1 短码申请

新加坡短码需通过 AWS Support Case 申请。

**Support Case 填写指南：**

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
| Region | 你使用的 AWS 区域（如 ap-southeast-1） |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

**Limit Type 消息类型选项：**
- One Time Password/Two-Factor Authentication
- Promotional/Marketing
- Transactional
- Transactional/Notifications/OTP/2FA

### 4.2 Case Description 模板

```
Subject: Request for Dedicated Short Code - Singapore (SG)

We are requesting a dedicated SMS short code for Singapore (+65).

Company Information:
- Company Name: [公司法定名称]
- UEN: [新加坡 UEN]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)

Target Carriers: Singtel, StarHub, M1

Expected Monthly Volume: [预估月发送量，如 100,000 messages]

Sample Messages (English):
1. "[Brand] Your verification code is: 385291. Valid for 5 minutes.
   Do not share this code with anyone."
2. "[Brand] Your order #SG12345 has been confirmed. Amount: SGD 129.90.
   Track: https://brand.com/orders/SG12345"
3. "[Brand] Alert: A sign-in was detected from a new device.
   If this wasn't you: https://brand.com/security"

Sample Messages (Chinese):
1. "[Brand] 您的验证码是：385291，5分钟内有效。请勿将此验证码分享给任何人。"
2. "[Brand] 您的订单 #SG12345 已确认。金额：SGD 129.90。
   追踪：https://brand.com/orders/SG12345"

Opt-in Process:
Users register on our website/app and provide their Singapore mobile number.
During registration, they tick a consent box stating: "I agree to receive
SMS messages from [Brand] for account verification and transaction
notifications."

Opt-out Mechanism:
Users can reply STOP to any message to unsubscribe. They can also opt out
via their account settings or by emailing optout@brand.com.

We understand that fees begin upon carrier initiation and are non-refundable.
We also request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.

Please provide the registration form and estimated timeline.
```

### 4.3 长码

新加坡支持长码，可通过 AWS 控制台直接购买，无需 Support Case。

---

## 五、新加坡电信监管

### 5.1 监管机构

**IMDA**（Info-communications Media Development Authority，资讯通信媒体发展管理局）是新加坡的电信和媒体监管机构。

**PDPC**（Personal Data Protection Commission，个人数据保护委员会）是新加坡的数据保护监管机构。

**SGNIC**（Singapore Network Information Centre）管理 SSIR 注册。

### 5.2 A2P SMS 监管法规

| 法规 | 说明 |
|------|------|
| Telecommunications Act | 电信法 |
| SSIR（Singapore SMS Sender ID Registry） | 2022年3月启用的 Sender ID 注册制度 |
| Personal Data Protection Act 2012 (PDPA) | 个人数据保护法 |
| Spam Control Act 2007 | 垃圾信息控制法 |
| Do Not Call (DNC) Registry | 谢绝来电登记 |

### 5.3 Spam Control Act 对 SMS 的要求

| 要求 | 详情 |
|------|------|
| **发送者身份** | 必须在消息中清楚标识发送者 |
| **退出机制** | 必须提供免费的退出方式 |
| **退出处理** | 收到退出请求后10个工作日内执行 |
| **营销消息限制** | 禁止向 DNC 名单中的号码发送营销消息 |

### 5.4 禁止/限制内容

| 类别 | 限制程度 |
|------|---------|
| 未经同意的营销 | 禁止（Spam Control Act） |
| 欺诈/误导性内容 | 禁止 |
| 未注册 Sender ID | 显示 "LIKELY-SCAM" |
| 赌博推广 | 仅合法运营商 |
| 烟草/酒精 | 严格限制 |
| 成人内容 | 严格限制 |

### 5.5 发送时间建议

虽无明确法定限制，行业最佳实践：
- **推荐发送时段：** 08:00-21:00 SGT（UTC+8）
- 紧急/交易类消息除外
- 避免公众假期发送营销消息

---

## 六、新加坡数据保护法 (PDPA)

### 6.1 法律概述

**PDPA**（Personal Data Protection Act 2012）是新加坡核心数据保护法律，自2014年7月2日全面生效。

### 6.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **同意义务** | 收集、使用和披露个人数据（包括手机号码）须获得同意 |
| **目的限制** | 仅可为收集时说明的目的使用个人数据 |
| **通知义务** | 必须告知用户数据收集的目的 |
| **访问和更正** | 用户有权访问和更正其个人数据 |
| **数据保护** | 须采取合理安全措施保护个人数据 |
| **数据保留** | 不应超过商业或法律目的所需期限 |
| **数据传输** | 向新加坡境外传输数据需确保对方有同等保护水平 |
| **数据泄露通知** | 2021年2月起，须在3个日历日内向 PDPC 通报重大数据泄露 |
| **DNC（Do Not Call）** | 发送营销 SMS 前必须检查 DNC 名单 |

### 6.3 DNC Registry（谢绝来电登记）

新加坡 DNC Registry 包含三个名单：
- **No Voice Call** — 拒绝语音营销电话
- **No SMS** — 拒绝营销短信
- **No Fax** — 拒绝营销传真

**对 SMS 业务的影响：**
- 发送营销 SMS 前**必须**检查号码是否在 No SMS 名单中
- 可通过 DNC API 批量查询
- 违规将面临罚款

### 6.4 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| PDPA 违规 | 最高 **100万新元（约75万美元）** 或组织年营业额 **10%**（取高者） |
| Spam Control Act | 最高 **25美元/条**（可累计） |
| DNC 违规 | 最高 **100万新元** |
| 执法行动 | PDPC 可发出执行通知、警告 |

---

## 七、运营商与号码格式

### 7.1 三大运营商

| 运营商 | 备注 |
|--------|------|
| Singtel | 新加坡最大运营商 |
| StarHub | 第二大运营商 |
| M1 | 第三大运营商（Keppel & SPH 旗下） |
| TPG Telecom | 较新进入市场的第四运营商 |

### 7.2 号码格式

#### 基本结构
- 国家代码：+65
- 国内号码长度：8位（无区号）

#### 号码类型

| 类型 | 格式 | 示例 |
|------|------|------|
| 移动号码 | +65 8XXX XXXX 或 +65 9XXX XXXX | +6581234567 |
| 固定电话 | +65 6XXX XXXX | +6561234567 |

#### 移动号码前缀

| 前缀 | 运营商 |
|------|--------|
| 8xxx | 多家运营商 |
| 9xxx | 多家运营商 |

> **注意：** 新加坡号码格式简单，无区号，无前导0，直接使用 +65 加8位号码。

#### 正确的 AWS SMS 号码格式
```
+6581234567      ← 移动号码（8开头）
+6591234567      ← 移动号码（9开头）
```

> **新加坡号码无前导0，直接 +65 加8位数字即可。**

### 7.3 运营商技术细节

- Sender ID 通过 SSIR 管理
- 未注册 Sender ID 显示为 "LIKELY-SCAM"
- 运营商有垃圾信息过滤和欺诈检测
- 支持号码携带
- 固定电话不可达（SMS 仅限移动号码）

---

## 八、消息模板范例（英语/中文）

### 8.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本拉丁字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含中文、emoji 或特殊字符 |

**新加坡双语考量：**

新加坡是多语言国家，官方语言包括英语、中文、马来语和淡米尔语。SMS 通常使用英语或中文。

| 语言 | 编码 | 单条上限 | 建议 |
|------|------|---------|------|
| 英语 | GSM-7 | 160字符 | 首选，成本最低 |
| 中文 | UCS-2 | 70字符 | 字符限制更严，注意消息长度 |
| 中英混合 | UCS-2 | 70字符 | 含任何中文即触发 UCS-2 |

> **重要：** 如果消息中包含**任何**中文字符，整条消息都将使用 UCS-2 编码（70字符/条）。建议英语和中文消息分别发送，避免混合导致不必要的成本增加。

### 8.2 OTP 验证码模板

**英语版（GSM-7，160字符/条）：**
```
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.
```
(85字符，1条消息)

**中文版（UCS-2，70字符/条）：**
```
[Brand] 您的验证码是：385291，5分钟内有效。请勿将此验证码分享给任何人。
```
(40字符，1条消息)

### 8.3 交易通知模板

**订单确认 — 英语版（GSM-7）：**
```
[Brand] Your order #SG12345 has been confirmed. Amount: SGD 129.90.
Estimated delivery: 10 Apr. Track: https://brand.com/o/SG12345
```
(120字符，1条)

**订单确认 — 中文版（UCS-2）：**
```
[Brand] 您的订单 #SG12345 已确认。金额：SGD 129.90。预计送达：4月10日。详情：https://brand.com/o/SG12345
```
(65字符，1条)

**配送通知 - 已发货 — 英语版（GSM-7）：**
```
[Brand] Your order #SG12345 has been dispatched. Track:
https://brand.com/track/SG12345 Estimated delivery: 10 Apr.
```
(110字符，1条)

**配送通知 - 已发货 — 中文版（UCS-2）：**
```
[Brand] 您的订单 #SG12345 已发货。追踪：https://brand.com/track/SG12345
```
(44字符，1条)

**配送通知 - 已签收 — 英语版（GSM-7）：**
```
[Brand] Your order #SG12345 has been delivered.
Rate your experience: https://brand.com/review/SG12345
```
(97字符，1条)

**付款确认 — 英语版（GSM-7）：**
```
[Brand] Payment received: SGD 129.90 (order #SG12345).
Receipt: https://brand.com/receipt/SG12345
```
(90字符，1条)

**账户安全警报 — 英语版（GSM-7）：**
```
[Brand] Alert: A sign-in was detected from a new device.
If this wasn't you: https://brand.com/security
```
(96字符，1条)

**账户安全警报 — 中文版（UCS-2）：**
```
[Brand] 安全提醒：检测到新设备登录。如非本人操作：https://brand.com/security
```
(46字符，1条)

### 8.4 营销消息模板（需 opt-in 同意 + DNC 检查）

**促销活动 — 英语版（GSM-7）：**
```
[Brand] Get 20% off your next purchase! Use code SAVE20.
Valid until 15/04. Shop: https://brand.com/sale
Reply STOP to opt out.
```
(126字符，1条)

**促销活动 — 中文版（UCS-2）：**
```
[Brand] 下单立享8折优惠！使用优惠码 SAVE20，有效期至4月15日。购物：https://brand.com/sale 回复STOP退订。
```
(66字符，1条)

**新品通知 — 英语版（GSM-7）：**
```
[Brand] New collection now available! Be the first to see it.
Shop now: https://brand.com/new
Reply STOP to opt out.
```
(113字符，1条)

### 8.5 Opt-in 确认模板

**英语版（GSM-7）：**
```
[Brand] Welcome! You'll receive account alerts and notifications.
Msg frequency varies. Reply STOP to cancel.
```
(102字符，1条)

**中文版（UCS-2）：**
```
[Brand] 欢迎！您将收到账户提醒和通知。消息频率不定。回复STOP取消订阅。
```
(42字符，1条)

### 8.6 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 英语为主，可使用中文（注意编码差异） |
| 英语编码 | GSM-7，160字符/条 |
| 中文编码 | UCS-2，70字符/条 |
| 中英混合 | 触发 UCS-2，建议分别发送 |
| 退出指引 | 营销消息必须包含 |
| 退出方式 | 支持双向 SMS（Reply STOP） |
| 货币 | 使用 SGD 或 S$ |
| DNC 检查 | 营销消息发送前必须检查 DNC 名单 |

---

## 九、定价信息

### 9.1 AWS 定价

AWS 未在公开文档直接列出新加坡每条费率，需通过：
1. 下载 AWS 定价 CSV：`aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

**已知费用结构：**

| 费用项 | 说明 |
|--------|------|
| 短码月租 | 需联系 AWS 确认 |
| 长码月租 | 需联系 AWS 确认 |
| Sender ID | 通常免费（仅需 SSIR 注册） |
| 每条消息费 | 需参考 CSV |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

### 9.2 成本优化建议

1. **使用 Sender ID（免费）** — 完成 SSIR 注册后，Sender ID 本身无月租费用
2. **优先使用英语 GSM-7** — 每条160字符，成本最低
3. **中文消息精简** — UCS-2 仅70字符/条，务必精简内容
4. **避免中英混合** — 分别发送英语和中文消息，避免不必要的 UCS-2 触发
5. **维护号码列表** — 定期清理无效号码
6. **检查 DNC 名单** — 避免向 DNC 号码发送营销消息（浪费费用且违规）

---

## 十、合规清单

在新加坡发送 SMS 前，确保完成：

- [ ] 获取 UEN（Unique Entity Number）
- [ ] 在 AWS 控制台完成 Singapore sender ID registration（第二步）
- [ ] 在 SGNIC 门户完成 SSIR 注册（第三步）
- [ ] 在 SGNIC 注册中将 **AMCS SG Private Limited** 列为参与聚合商
- [ ] 等待 SGNIC 审核完成，AWS 注册状态更新
- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 营销消息发送前检查 **DNC Registry**（No SMS 名单）
- [ ] 实施有效的 opt-out 机制（支持 STOP 关键词回复）
- [ ] 遵守 PDPA 数据保护要求
- [ ] 遵守 Spam Control Act 2007
- [ ] 消息内容使用英语或中文
- [ ] 发送时间限制在 08:00-21:00 SGT（营销消息）
- [ ] 每条营销消息包含发送者标识和退出方式
- [ ] 注意编码选择（英语用 GSM-7，中文用 UCS-2，避免混合）
- [ ] 正确格式化号码（+65 加8位数字，无前导0）
- [ ] 保留同意和退出记录
- [ ] 如有数据泄露，3个日历日内通报 PDPC
- [ ] 申请提高 AWS 消费阈值（默认 $1.00/月）

---

## 十一、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| 未注册 Sender ID 显示 "LIKELY-SCAM" | **极高** | 务必完成 AWS + SGNIC 三步注册 |
| SGNIC 注册未将 AMCS SG Private Limited 列为聚合商 | **极高** | 注册时仔细确认聚合商选择 |
| 无 UEN 导致无法注册 | 高 | 外国公司提前规划新加坡实体注册 |
| PDPA 违规（最高100万新元或年营业额10%） | 高 | 严格遵守 PDPA，咨询新加坡法律顾问 |
| DNC 违规 | 高 | 每次营销发送前检查 DNC 名单 |
| 中文消息 UCS-2 导致成本增加 | 中 | 精简中文内容，英语消息用 GSM-7 |
| 运营商内容过滤 | 中 | 使用标准模板，品牌域名 |
| 短码申请等待期 | 中 | 初期使用 Sender ID，后续申请短码 |
| 号码格式错误 | 低 | 新加坡格式简单，+65 加8位数字 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。新加坡法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询新加坡当地法律顾问以确保合规。
