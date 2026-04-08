## 一、AWS 上的英国 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +44 |
| 短码 (Short Code) | **支持（$1,500/月）** |
| 长码 (Long Code) | 支持 |
| Sender ID | **支持（必须在 MEF Registry 注册）** |
| Toll-free | 不支持 |
| 双向 SMS | **不支持** |
| 消息拼接 | 支持 |
| MMS | 不支持 |
| 国际发送 | 支持 |

**核心限制：** 英国要求所有 Sender ID 必须在 MEF SMS Sender ID Protection Registry 注册。未注册的 Sender ID 发送的消息可能被运营商直接拦截。英国是 AWS 控制台直接支持短码申请的国家之一。不支持双向 SMS，意味着用户无法通过回复 STOP 退订，需提供替代退出机制。

---

## 二、费用总览

### 2.1 号码费用

| 费用项目 | 一次性费用 | 月度费用 | 备注 |
|---------|-----------|---------|------|
| 专用短码（Short Code） | $0 | $1,500 | 预置约 16 周 |
| 长码（Long Code） | $0 | $2 | 预置约 1 周 |
| Sender ID | 免费 | 免费 | 需完成 MEF Registry 注册 |

### 2.2 每条消息费用

| 号码类型 | 每条消息费用 | 备注 |
|---------|------------|------|
| 短码 | $0.05 | — |
| 长码 | $0.05 | — |
| Sender ID | $0.05 | — |

> 所有号码类型的每条消息费用相同。

### 2.3 月度消费阈值

新账户默认 **$1.00 USD/月**，需通过 AWS Support Case 申请提高。

## 三、Sender ID 注册详细流程

### 3.1 MEF SMS Sender ID Protection Registry 概述

MEF（Mobile Ecosystem Forum）SMS Sender ID Protection Registry 是英国用于保护消费者免受欺诈短信侵害的行业注册机制。所有通过 AWS 向英国发送 SMS 的企业必须注册 Sender ID。

注册分为两种情况：

### 3.2 情况A：MEF 受保护的 Sender ID（Protected Sender ID）

当你的 Sender ID 已在 MEF Registry 中被某个组织注册保护时，你需要提供授权书（LOA）来证明你有权使用该 Sender ID。

**步骤一：下载 LOA 模板**

从 AWS 文档下载：`AWS_Protected_Sender_ID_Letter-of-Authorisation_UK.zip`

**步骤二：填写 LOA**

| 要求 | 详情 |
|------|------|
| 签署方 | LOA 必须由 Sender ID 的最终拥有公司签署（不是经销商/中间方） |
| 日期要求 | LOA 日期必须在**最近30天内** |
| 格式要求 | 转换为 **PDF 格式** |
| 文件大小 | 最大 **400 KB** |
| 大小写匹配 | Sender ID 大小写必须与 MEF 注册记录**完全一致** |
| 填写完整性 | 所有高亮字段必须完整填写，不允许遗漏 |

**步骤三：在 AWS 控制台提交注册**

1. 登录 AWS 控制台，前往 **End User Messaging SMS**
2. 导航至 **Registrations** → **Create registration**
3. 选择 **UK sender ID registration**
4. 在 **Sender ID info** 部分上传 LOA PDF
5. 填写所有必填字段（见 2.4 节）
6. 提交注册

### 3.3 情况B：非 MEF 受保护的 Sender ID（Non-Protected Sender ID）

当你的 Sender ID 未在 MEF Registry 中被保护时，流程更简单：

1. 登录 AWS 控制台，前往 **End User Messaging SMS**
2. 导航至 **Registrations** → **Create registration**
3. 选择 **UK sender ID registration**
4. **跳过** "Letter of authorization image" 字段（标记为 Optional）
5. 填写所有其他必填字段
6. 提交注册

### 3.4 注册表单字段详情

#### Sender ID Info

| 字段 | 必填 | 说明 |
|------|------|------|
| Sender ID | 是 | 请求的 Sender ID，必须与 MEF 注册完全匹配（如适用） |
| Letter of authorization image | 仅 Protected | LOA 文件，PDF 格式，最大 400KB，日期在30天内 |
| Sender ID connection | 否 | 描述 Sender ID 与公司之间的关联 |

#### Company Info

| 字段 | 必填 | 说明 |
|------|------|------|
| Company Name | 是 | 公司法定名称 |
| Tax ID or Business Registration Number | 是 | 税号或公司注册号（如 UK Company Number 或 VAT Number） |
| Company website | 是 | 公司网站 URL |
| Address 1 | 是 | 公司总部街道地址 |
| Address 2 | 否 | 楼层/套房号 |
| City | 是 | 城市 |
| State/Province | 是 | 州/省/郡 |
| Zip Code/Postal code | 是 | 邮编 |
| Country | 是 | 两位 ISO 国家代码（如 GB） |

#### Contact Info

| 字段 | 必填 | 说明 |
|------|------|------|
| First Name | 是 | 联系人名 |
| Last Name | 是 | 联系人姓 |
| Contact Email | 是 | 联系人邮箱 |
| Contact Phone Number | 是 | 联系人电话 |

#### Messaging Use Case

| 字段 | 必填 | 说明 |
|------|------|------|
| Monthly SMS Volume | 是 | 预计月发送量（下拉选择） |
| Use case category | 是 | 选项：Two-factor authentication / One-time passwords / Notifications / Polling and surveys / Info on demand / Promotions and Marketing / Other |
| Use case details | 选 Other 时必填 | 补充说明 |

#### Message Samples

| 字段 | 必填 | 说明 |
|------|------|------|
| Message Sample 1 | 是 | 消息样本（英语） |
| Message Sample 2 | 否 | 额外样本 |
| Message Sample 3 | 否 | 额外样本 |

### 3.5 不注册的后果

| 后果 | 详情 |
|------|------|
| 消息被拦截 | 未注册的 Sender ID 发出的消息可能被运营商直接拦截，无法送达 |
| 品牌信任受损 | 即使部分消息送达，也可能被标记为可疑来源 |
| 运营商自主过滤 | 监管机构和运营商可自行决定过滤或屏蔽未注册流量 |

---

## 四、短码申请流程

### 4.1 申请方式

英国**在** AWS 控制台直接支持短码申请的国家列表中，可通过控制台或 Support Case 申请。

### 4.2 Support Case 填写指南（适用于需手动申请的情况）

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
| Region | 你使用的 AWS 区域（如 eu-west-2） |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型（见下方） |
| Limit Increase Value | **1**（即申请 1 个专用短码） |

**Limit Type 消息类型选项：**
- One Time Password/Two-Factor Authentication
- Promotional/Marketing
- Transactional
- Transactional/Notifications/OTP/2FA

### 4.3 Case Description 模板

```
Subject: Request for Dedicated Short Code - United Kingdom (GB)

We are requesting a dedicated SMS short code for the United Kingdom (+44).

Company Information:
- Company Name: [公司法定名称]
- Company Registration Number: [UK Company Number 或 VAT Number]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)

Target Carriers: EE, O2, Three, Vodafone

Expected Monthly Volume: [预估月发送量，如 100,000 messages]

Sample Messages (English):
1. "[Brand] Your verification code is: 385291. Valid for 5 minutes.
   Do not share this code with anyone."
2. "[Brand] Your order #UK12345 has been confirmed. Amount: GBP 89.99.
   Track: https://brand.com/orders/UK12345"
3. "[Brand] Alert: A sign-in was detected from a new device.
   If this wasn't you: https://brand.com/security"

Opt-in Process:
Users register on our website/app and provide their UK mobile number.
During registration, they tick a consent box stating: "I agree to receive
SMS messages from [Brand] for account verification and transaction
notifications."

Opt-out Mechanism:
Since two-way SMS is not supported in the UK on AWS, we provide an
unsubscribe link in each message (https://brand.com/unsub/{token}).
Users can also opt out via email at optout@brand.com or through their
account settings at https://brand.com/settings.

We understand that fees begin upon carrier initiation and are non-refundable.
We also request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.

Please provide the registration form and estimated timeline.
```

### 4.4 申请时间线

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 提供注册表格 | 1-3个工作日 |
| 填写并提交注册表格 | 取决于你 |
| 运营商审核 | 8-12周 |
| 短码配置完成 | 1-2周 |
| **总计** | **10-15周** |

### 4.5 费用说明

| 费用项 | 说明 |
|--------|------|
| 短码月租 | **$1,500 USD/月** |
| 每条消息费 | 需下载 AWS 定价 CSV 或联系销售 |
| 一次性设置费 | 可能有（需确认） |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

> **重要：** 费用从向运营商提交申请那一刻开始计费，即使短码尚未配置完成。申请被拒绝已产生的费用也不退还。

---

## 五、英国电信监管

### 5.1 监管机构

**Ofcom**（Office of Communications，通信管理局）是英国通信行业的独立监管机构，监管范围包括电信、广播和邮政服务。

**ICO**（Information Commissioner's Office，信息专员办公室）负责数据保护和隐私监管。

### 5.2 A2P SMS 监管法规

| 法规 | 说明 |
|------|------|
| Privacy and Electronic Communications Regulations 2003 (PECR) | 英国电子通信隐私法规，是 ePrivacy 指令在英国的实施 |
| UK GDPR | 英国脱欧后的本地化 GDPR 版本 |
| Data Protection Act 2018 (DPA 2018) | 英国数据保护法 |
| Communications Act 2003 | 通信法 |
| Ofcom General Conditions | Ofcom 通用条件 |

### 5.3 PECR 对 SMS 的关键要求

| 要求 | 详情 |
|------|------|
| **事先同意** | 向个人发送营销 SMS 必须获得事先同意（opt-in） |
| **发送者身份** | 每条消息必须清楚标识发送者 |
| **退出机制** | 每条营销消息必须提供简单的退出方式 |
| **软性退出例外** | 如果号码来自既有客户关系，且消息与类似产品/服务相关，可使用"软性 opt-in" |
| **不适用于服务消息** | 纯事务性消息（如 OTP）不受营销同意要求约束 |

### 5.4 禁止/限制内容

| 类别 | 限制程度 |
|------|---------|
| 未经同意的营销 | 禁止 |
| 欺诈/误导性内容 | 禁止 |
| 成人内容 | 严格限制，需年龄验证 |
| 赌博推广 | 仅持牌运营商（UK Gambling Commission） |
| 金融推广 | 需获得 FCA 授权或批准 |
| 烟草/电子烟 | 禁止广告 |

### 5.5 发送时间建议

虽无明确法定限制，行业最佳实践：
- **推荐发送时段：** 08:00-20:00 GMT/BST
- 紧急/交易类消息除外
- 避免周日和银行假日发送营销消息

---

## 六、英国数据保护法

### 6.1 法律框架

英国数据保护由以下法律共同构成：
- **UK GDPR**（脱欧后保留的 GDPR 版本）
- **Data Protection Act 2018 (DPA 2018)**
- **PECR 2003**

### 6.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **合法基础** | 处理手机号码需有合法基础（同意、合同履行、合法利益等） |
| **透明度** | 隐私政策必须说明如何使用手机号码发送 SMS |
| **数据最小化** | 仅收集发送 SMS 所必需的数据 |
| **数据主体权利** | 用户享有访问权、更正权、删除权、限制处理权、数据可携带权、反对权 |
| **数据保护影响评估 (DPIA)** | 大规模 SMS 营销活动可能需要进行 DPIA |
| **数据泄露通知** | 72小时内向 ICO 通报个人数据泄露事件 |
| **国际数据传输** | 向英国境外传输数据需有适当保障措施 |

### 6.3 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| UK GDPR 高级别 | 最高 **1750万英镑** 或全球年营业额 **4%**（取高者） |
| UK GDPR 标准级别 | 最高 **870万英镑** 或全球年营业额 **2%**（取高者） |
| PECR 违规 | ICO 可发出最高 **50万英镑** 的罚款通知 |
| 执法行动 | ICO 可发出执行通知、信息通知、评估通知 |

### 6.4 Telephone Preference Service (TPS)

- 英国的全国性电话营销退出名册
- 虽主要针对语音电话，但 SMS 营销也应参考
- 企业发送营销 SMS 前应检查号码是否在 TPS 名册中
- 注册地址：https://www.tpsonline.org.uk

---

## 七、运营商与号码格式

### 7.1 四大运营商

| 运营商 | 母公司/备注 |
|--------|------------|
| EE | BT Group 旗下，英国最大 4G/5G 网络 |
| O2 | Virgin Media O2（Liberty Global 合资） |
| Three | CK Hutchison Holdings |
| Vodafone | 全球性运营商 |

**MVNO（虚拟运营商）：** 此外有大量 MVNO 运营商，如 giffgaff、Tesco Mobile、Sky Mobile、ASDA Mobile 等，均基于四大网络。

### 7.2 号码格式

#### 基本结构
- 国家代码：+44
- 国内号码长度：10位（去掉前导0后）

#### 号码类型

| 类型 | 格式 | 示例 |
|------|------|------|
| 移动号码 | +44 7XXX XXXXXX | +447911123456 |
| 伦敦固话 | +44 20 XXXX XXXX | +442012345678 |
| 其他城市 | +44 1XXX XXXXXX | +441234567890 |

#### 移动号码前缀

| 前缀 | 运营商 |
|------|--------|
| 074XX-075XX | 多家运营商 |
| 076XX-079XX | 多家运营商 |

> **注意：** 由于号码可携带（携号转网），前缀不能可靠地判断当前运营商。

#### 正确的 AWS SMS 号码格式
```
+447911123456      ← 移动号码（去掉国内前导0）
+447700900123      ← 移动号码
```

> **SMS 发送时去掉国内拨号前导 `0`！** 例如国内号码 07911 123456 应格式化为 +447911123456。

### 7.3 运营商技术细节

- Sender ID 通过 MEF Registry 保护
- 运营商有垃圾信息自动过滤和欺诈检测
- 高频发送可能触发限流
- 重复内容可能被标记为垃圾信息
- 支持号码携带（携号转网）
- 固定电话不可达（SMS 仅限移动号码）

---

## 八、不支持双向 SMS 的替代退出方案

### 8.1 问题

AWS 上英国不支持双向 SMS，用户无法回复 STOP 退订。必须提供替代 opt-out 机制。

### 8.2 四种替代方案

#### 方案一：短链接退出（推荐）

每条 SMS 中包含唯一退出链接：

```
[Brand] Your code: 385291. Valid 5 min. Unsubscribe: https://brand.com/unsub/abc123
```

**实现要点：**
- 为每个用户生成唯一 token
- 退出页面使用英语
- 一键完成退出，无需登录
- 退出后24小时内停止发送
- 保留退出记录作为合规证据

**退出页面文案范例：**
```
Unsubscribe from SMS

Confirm you wish to stop receiving SMS messages from [Brand].

[Button: Confirm Unsubscribe]

Your number: +44 7XXX XXXXXX
Upon confirmation, you will stop receiving messages within 24 hours.
```

#### 方案二：邮件退出

```
Unsubscribe: email optout@brand.com with your phone number.
```

#### 方案三：客服电话退出

```
Unsubscribe: call +44-XXX-XXX-XXXX
```

#### 方案四：App 内设置

在 App 设置中提供 SMS 偏好管理页面。

### 8.3 最佳实践

1. 每条营销消息**必须**包含退出指引
2. 使用品牌自有域名短链接（避免 bit.ly 等被运营商过滤）
3. 退出应即时生效，24小时内停止发送
4. 保留所有退出记录
5. 在首次 opt-in 时明确告知退出方式
6. 定期发送提醒告知如何退出

---

## 九、消息模板范例（英语）

### 9.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本拉丁字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含 emoji 或特殊字符 |

**英语字符与 GSM-7 兼容性：**

英语基本字母、数字和常用标点符号（.、,、!、?、-、:、;、/、@、#、$、%、&、*、+、=等）均在 GSM-7 字符集内。

| 字符类型 | GSM-7 兼容 | 建议 |
|---------|-----------|------|
| 基本拉丁字母 a-z A-Z | **是** | 正常使用 |
| 数字 0-9 | **是** | 正常使用 |
| 英镑符号 £ | **是** | 可正常使用 |
| 欧元符号 € | **是**（扩展字符，占2字符位） | 谨慎使用 |
| Emoji | **否** | 避免使用，会触发 UCS-2 |
| 中文/日文 | **否** | 会触发 UCS-2 |

> **建议：** 英语消息天然适合 GSM-7 编码，只要避免使用 emoji 和非拉丁字符，即可充分利用 160 字符的单条容量。

### 9.2 OTP 验证码模板

**GSM-7 兼容（推荐，160字符/条）：**
```
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.
```
(85字符，1条消息)

### 9.3 交易通知模板

**订单确认（GSM-7）：**
```
[Brand] Your order #UK12345 has been confirmed. Amount: GBP 89.99.
Estimated delivery: 10 Apr. Track: https://brand.com/o/UK12345
```
(120字符，1条)

**配送通知 - 已发货（GSM-7）：**
```
[Brand] Your order #UK12345 has been dispatched. Track:
https://brand.com/track/UK12345 Estimated delivery: 10 Apr.
```
(110字符，1条)

**配送通知 - 派送中（GSM-7）：**
```
[Brand] Your parcel is out for delivery. Estimated arrival: today
2-5pm. Track: https://brand.com/track/UK12345
```
(105字符，1条)

**配送通知 - 已签收（GSM-7）：**
```
[Brand] Your order #UK12345 has been delivered.
Rate your experience: https://brand.com/review/UK12345
```
(97字符，1条)

**付款确认（GSM-7）：**
```
[Brand] Payment received: GBP 89.99 (order #UK12345).
Receipt: https://brand.com/receipt/UK12345
```
(89字符，1条)

**账户安全警报（GSM-7）：**
```
[Brand] Alert: A sign-in was detected from a new device.
If this wasn't you: https://brand.com/security
```
(96字符，1条)

### 9.4 营销消息模板（需 opt-in 同意）

**促销活动（GSM-7）：**
```
[Brand] Get 20% off your next purchase! Use code SAVE20.
Valid until 15/04. Shop: https://brand.com/sale
Opt out: https://brand.com/unsub/{TOKEN}
```
(140字符，1条)

**新品通知（GSM-7）：**
```
[Brand] New collection now available! Be the first to see it.
Shop now: https://brand.com/new
Opt out: https://brand.com/unsub/{TOKEN}
```
(127字符，1条)

**会员积分提醒（GSM-7）：**
```
[Brand] You have 500 points expiring on 30/04.
Redeem now: https://brand.com/rewards
Opt out: https://brand.com/unsub/{TOKEN}
```
(123字符，1条)

> **注意：** 英国在 AWS 上不支持双向 SMS，营销消息的退出方式必须使用链接而非 STOP 关键词。

### 9.5 Opt-in 确认模板

**首次订阅确认（GSM-7）：**
```
[Brand] Welcome! You'll receive account alerts and notifications.
Msg frequency varies. Opt out: https://brand.com/unsub/{TOKEN}
```
(118字符，1条)

### 9.6 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 使用英语 |
| 编码 | 使用 GSM-7（避免 emoji） |
| £ 符号 | 在 GSM-7 中兼容，可直接使用 |
| € 符号 | 扩展字符占2位，建议用 EUR 替代 |
| 退出指引 | 营销消息必须包含 |
| 退出方式 | 使用网页链接（不支持双向 SMS，不能用 STOP） |
| 货币 | 使用 GBP 或 £ |

---

## 十、定价信息

### 10.1 AWS 定价

| 费用项 | 参考价格 |
|--------|---------|
| 短码月租 | **$1,500 USD/月** |
| 长码月租 | 需联系 AWS 确认 |
| Sender ID | 通常免费（仅需注册） |
| 每条消息费 | 需下载 AWS 定价 CSV |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

**获取最新定价：**
1. 下载 AWS 定价 CSV：`aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

### 10.2 成本优化建议

1. **使用 Sender ID（免费）** — 对于不需要双向通信的场景，Sender ID 比短码便宜得多
2. **使用 GSM-7 编码** — 英语天然兼容，每条160字符
3. **精简消息内容** — 保持单条消息在160字符内避免拼接
4. **维护号码列表** — 定期清理无效号码，减少无效发送
5. **使用品牌域名短链接** — 避免第三方短链接服务被运营商过滤

---

## 十一、合规清单

在英国发送 SMS 前，确保完成：

- [ ] 在 MEF Registry 注册 Sender ID（通过 AWS 控制台）
- [ ] 如为 Protected Sender ID，准备并上传 LOA（30天内签署，PDF，400KB以内）
- [ ] LOA 中 Sender ID 大小写与 MEF 注册完全一致
- [ ] 获取所有接收者的明确 opt-in 同意（营销消息）
- [ ] 实施有效的 opt-out 机制（网页链接，因 AWS 不支持双向 SMS）
- [ ] 遵守 UK GDPR / DPA 2018 数据保护要求
- [ ] 遵守 PECR 2003 电子通信隐私要求
- [ ] 参考 TPS 名册检查营销号码
- [ ] 消息内容使用英语
- [ ] 发送时间限制在 08:00-20:00 GMT/BST（营销消息）
- [ ] 每条营销消息包含发送者身份标识和退出方式
- [ ] 使用 GSM-7 编码（避免 emoji）
- [ ] 正确格式化号码（+44 + 号码，**去掉国内前导0**）
- [ ] 隐私政策中声明 SMS 发送用途
- [ ] 保留同意和退出记录
- [ ] 如处理大量个人数据，考虑进行 DPIA
- [ ] 申请提高 AWS 消费阈值（默认 $1.00/月）

---

## 十二、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| 未注册 Sender ID 导致消息被拦截 | **极高** | 务必完成 MEF Registry 注册，确认注册状态后再发送 |
| Protected Sender ID 的 LOA 过期/不合格 | 高 | 确保 LOA 在30天内签署，大小写完全匹配，填写完整 |
| UK GDPR/PECR 违规 | **极高** | 严格遵守数据保护法规，咨询英国当地法律顾问 |
| ICO 执法处罚（最高1750万英镑） | 高 | 建立合规团队，定期审计 SMS 发送实践 |
| 运营商内容过滤 | 中 | 避免可疑内容，使用标准模板，使用品牌域名 |
| 短码申请等待期（10-15周） | 中 | 提前规划，尽早提交申请；初期可先用 Sender ID |
| 不支持双向 SMS 导致退出机制不完善 | 高 | 实施网页链接退出方案，保留退出记录 |
| 短码月租成本高（$1,500/月） | 中 | 评估是否真正需要短码，小规模可用 Sender ID + 长码 |
| 号码格式错误 | 低 | 实施严格的号码格式验证（去掉前导0，加+44） |
| Unicode 导致成本翻倍 | 低 | 英语内容天然兼容 GSM-7，避免 emoji |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。英国法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询英国当地法律顾问以确保合规。
