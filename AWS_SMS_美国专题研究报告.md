# 美国 (US, +1) AWS SMS 发送专题研究报告

> 调研日期：2026年4月7日
> 数据来源：AWS 官方文档、FCC/FTC 官网、TCPA 法律资料、Campaign Registry 文档

---

## 一、AWS 上的美国 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +1 |
| 短码 (Short Code) | **支持**（$995/月） |
| 长码 / 10DLC | **支持**（必须完成 10DLC 注册） |
| Toll-Free（免费号码） | **支持**（$2/月） |
| Sender ID | **不支持** |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| MMS | 支持 |
| 国际发送 | 支持 |

**核心要点：** 美国是 AWS SMS 功能最全面的市场之一，支持三种号码类型（10DLC、Short Code、Toll-Free）。但所有号码类型均需注册才能用于 A2P 发送。10DLC 是最常用的发送方式，必须完成 Brand + Campaign 注册流程。TCPA 法规极其严格，违规罚款高达每条 $500-$1,500。

---

## 二、三种号码类型对比

| 特性 | 10DLC | Short Code | Toll-Free |
|------|-------|------------|-----------|
| 号码格式 | 10位本地号码 | 5-6位短号码 | 800/888/877等 |
| 月费 | ~$1/号码 + $10/Campaign | $995/月 | $2/月 |
| 一次性费用 | Brand $4 + Vetting $40 | 一次性设置费（需确认） | 无 |
| 吞吐量（默认） | 1 MPS/号码 | 100+ MPS | 3 MPS |
| 注册周期 | 4-8周 | 8-12周 | ~15个工作日 |
| 双向 SMS | 支持 | 支持 | 支持 |
| MMS | 支持 | 支持 | 支持 |
| 适用场景 | 中小规模 A2P | 高吞吐量/品牌认知 | 中等规模/快速上线 |
| 运营商过滤风险 | 低（已注册） | 最低 | 中等 |

**选型建议：**
- **初创/中小企业：** 10DLC（成本低，注册流程标准化）
- **大型企业/高流量：** Short Code（最高吞吐量，最佳送达率）
- **快速上线：** Toll-Free（注册周期最短）

---

## 三、10DLC 注册详细流程

### 3.1 流程概览

10DLC（10-Digit Long Code）是美国及其领地（波多黎各、美属维尔京群岛、关岛、美属萨摩亚）的 A2P SMS 主要发送方式。每个 10DLC 号码必须注册到特定的发送者和使用场景。

**三步注册流程：**
```
Brand Registration → Campaign Registration → Number Association
    （品牌注册）        （Campaign 注册）       （号码关联）
```

### 3.2 第一步：Brand Registration（品牌注册）

**在 AWS 控制台操作：**
1. 前往 AWS End User Messaging SMS 控制台
2. 选择 "Registrations" → "Create registration"
3. 选择 "10DLC brand registration"

**需要提供的信息：**

| 字段 | 说明 |
|------|------|
| Legal Company Name | 公司法定全称 |
| DBA (Doing Business As) | 商号名称（如有） |
| EIN / Tax ID | 美国公司填 EIN；国际公司填当地税号 |
| Company Address | 公司注册地址 |
| Company Website | 公司网站 URL |
| Vertical | 行业类别 |
| Stock Symbol | 上市公司填写股票代码（可选） |
| Contact Name / Email / Phone | 联系人信息 |

**审核时间：**
- 美国公司：1-2 个工作日
- 国际公司：最多 3 周

### 3.3 第二步：Brand Vetting（品牌审核，强烈推荐）

Brand Vetting 是可选步骤，但**强烈建议完成**，原因：
- 未审核的品牌受到严格的运营商吞吐量限制
- 审核后可大幅提升发送速率

**费用：** $40 一次性费用

**审核内容：** 第三方验证机构（Campaign Registry）验证公司信息的真实性，生成 Vetting Score。

**审核时间：**
- 美国公司：1-2 个工作日
- 国际公司：最多 3 周

> **重要：** Brand Vetting 通过后，吞吐量**不会自动提升**。你必须另外提交一个 SMS sending rate increase 请求。

### 3.4 第三步：Campaign Registration（Campaign 注册）

**需要提供的信息：**

| 字段 | 说明 |
|------|------|
| Campaign Description | 详细描述消息用途 |
| Use Case Type | 选择用例类型（2FA、Account Notifications、Marketing 等） |
| Sample Messages | 消息模板样本（至少 1-2 条） |
| Opt-in Flow | 用户如何同意接收消息的详细说明 |
| Opt-in Keywords | 如 YES、SUBSCRIBE |
| Opt-out Keywords | 如 STOP、CANCEL |
| Help Keywords | 如 HELP、INFO |
| Message Frequency | 消息发送频率说明 |

**审核时间：** 最多 4 周

**关键规则：**
- 每个 Campaign 至少需要 1 个独立号码
- 1 个号码只能关联 1 个 Campaign
- 1 个 Campaign 可以关联多个号码

### 3.5 第四步：Number Association（号码关联）

1. 在 AWS 控制台购买 10DLC 号码
2. 将号码关联到已批准的 Campaign
3. 关联完成约需 **14 天**

**如已有长码号码：** 可尝试将现有长码转换为 10DLC，需完成注册后创建 AWS Support Case 请求转换。如转换失败，需购买新号码。

### 3.6 完整时间线

| 阶段 | 美国公司 | 国际公司 |
|------|---------|---------|
| Brand Registration | 1-2 个工作日 | 最多 3 周 |
| Brand Vetting（可选） | 1-2 个工作日 | 最多 3 周 |
| Campaign Registration | 最多 4 周 | 最多 4 周 |
| Number Association | ~14 天 | ~14 天 |
| **总计** | **约 4-6 周** | **约 8 周以上** |

### 3.7 10DLC 吞吐量限制（关键）

#### 未审核品牌（Unvetted）

| 运营商 | 限制 | 说明 |
|--------|------|------|
| AT&T | 75 条/分钟/Campaign | 按 Campaign 级别限制 |
| T-Mobile | 2,000 条/天 | **同一公司所有 Campaign 共享**，跨 AWS 账户也共享 |
| Verizon | 无公开限制 | 使用垃圾信息检测系统自动过滤 |

#### 已审核品牌（Vetted）

吞吐量根据 Vetting Score 提升，具体速率因运营商和 Campaign 类型而异。审核后需**单独提交**速率提升请求。

#### 各号码类型默认吞吐量

| 号码类型 | 默认 MPS |
|----------|---------|
| 10DLC | 1 MPS/号码 |
| Short Code | 100+ MPS |
| Toll-Free | 3 MPS |

> **重要：** T-Mobile 的每日 2,000 条限制是**公司级别**的，而非 Campaign 级别。如果你在多个 AWS 账户中使用同一公司注册，该限制在所有账户间共享。

---

## 四、Short Code 申请流程

### 4.1 申请方式

美国短码可通过 AWS 控制台直接申请（美国是 AWS 控制台直接支持短码申请的国家之一）。

### 4.2 Support Case 填写指南

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

### 4.3 Case Description 模板

```
Subject: Request for Dedicated Short Code - United States (US)

We are requesting a dedicated SMS short code for the United States (+1).

Company Information:
- Company Name: [公司法定名称]
- EIN / Tax ID: [税号]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)

Expected Monthly Volume: [预估月发送量，如 500,000 messages]

Sample Messages:
1. "[Brand] Your verification code is: 385291. Valid for 5 minutes.
   Do not share this code."
2. "[Brand] Your order #US12345 has been confirmed. Amount: $150.00.
   Track: https://brand.com/orders/US12345"
3. "[Brand] Alert: A new sign-in was detected on your account.
   If this wasn't you: https://brand.com/security"

Opt-in Process:
Users sign up on our website/app and provide their US mobile number.
During registration, they check a consent box stating: "I agree to
receive SMS messages from [Brand] for account verification and
transaction notifications. Msg & data rates may apply. Reply STOP
to opt out."

Opt-out Mechanism:
Users can reply STOP at any time to opt out. They can also text HELP
for assistance or manage preferences at https://brand.com/sms-prefs.

We also request an increase to our monthly SMS spend limit from $1.00
to $[desired amount] USD.

Please provide the registration form and estimated timeline.
```

### 4.4 费用

| 费用项 | 金额 |
|--------|------|
| 短码月租 | $995 USD/月 |
| 一次性设置费 | 需向 AWS 确认 |
| 每条消息费 | 参考定价 CSV |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

### 4.5 申请时间线

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| 运营商审核 | 8-12周 |
| 短码配置完成 | 1-2周 |
| **总计** | **8-12周** |

> **重要：** 费用从向运营商提交申请那一刻开始计费，即使短码尚未配置完成。

---

## 五、Toll-Free 号码注册流程

### 5.1 概述

Toll-Free Number（TFN）是以 800、888、877、866、855、844、833 开头的免费号码，适用于美国及其领地。

### 5.2 注册流程

1. 在 AWS End User Messaging SMS 控制台购买 Toll-Free 号码
2. 系统提示 "Registration Required"，输入注册友好名称
3. 选择 "Begin registration" 开始注册
4. 填写使用场景、公司信息、消息模板等
5. 提交审核

### 5.3 注册要求

| 要求 | 说明 |
|------|------|
| 使用场景 | 每个 TFN 必须声明特定使用场景 |
| 用途限制 | TFN **只能用于注册时声明的用途** |
| 审核时间 | 最多 15 个工作日 |
| 违规后果 | 用于未注册用途可能导致号码被撤销 |

### 5.4 禁止的使用场景

- 受控物质相关
- 网络钓鱼
- AWS 禁止的消息内容

### 5.5 费用

| 费用项 | 金额 |
|--------|------|
| TFN 月租 | $2 USD/月 |
| 每条消息费 | ~$0.00581/条（基础费）+ $0.00705/条（运营商费） |

---

## 六、TCPA 法规详解

### 6.1 法律概述

**TCPA**（Telephone Consumer Protection Act，电话消费者保护法）于 1991 年颁布，是美国 SMS 营销领域最重要的法律。由 **FCC**（联邦通信委员会）执行。

### 6.2 核心要求

| 要求 | 详情 |
|------|------|
| **明确同意** | 发送任何 A2P 短信前必须获得接收者的明确书面或口头同意 |
| **同意记录** | 必须保留完整的同意记录（日期、时间、来源），建议保留至少 90 天 |
| **发送者识别** | 每条消息必须明确标识发送者身份 |
| **退订机制** | 必须支持 STOP 关键词退订，收到后立即停止发送 |
| **帮助机制** | 必须支持 HELP 关键词，提供联系方式和说明 |
| **发送时间** | 建议仅在收件人当地时间 9:00-20:00 发送 |
| **号码去活** | 定期下载去活报告（Deactivation Reports），移除已停用号码 |

### 6.3 罚款金额

| 违规类型 | 罚款金额 |
|---------|---------|
| 一般违规 | **$500/条**（每条未经同意的消息） |
| 故意违规 | **$1,500/条**（最高三倍赔偿） |
| 集体诉讼 | 无上限（按消息总量计算） |

> **案例参考：** TCPA 集体诉讼频繁发生，单个案件赔偿金额可达数百万甚至数千万美元。

### 6.4 号码去活报告（Deactivation Reports）

FCC 要求发送者维护最新的号码列表。AWS 提供去活报告下载：

```bash
# 下载最新去活报告
aws s3api get-object \
  --bucket us-east-1-pinpoint-sms-voice \
  --key sms-deact-reports/us/latest-deact-report.csv \
  OUTFILE.csv \
  --request-payer requester

# 下载特定日期的去活报告
aws s3api get-object \
  --bucket us-east-1-pinpoint-sms-voice \
  --key sms-deact-reports/us/2026-04-07-deact-report.csv \
  OUTFILE.csv \
  --request-payer requester
```

> **重要：** FCC 将向已停用号码发送消息视为垃圾短信，可能导致审计、投诉和发送权限被封禁。

---

## 七、S.H.A.F.T. 禁止内容

### 7.1 S.H.A.F.T. 类别

美国运营商严格限制以下类别的 A2P 短信内容：

| 字母 | 类别 | 英文 | 说明 |
|------|------|------|------|
| **S** | 色情 | Sex | 成人内容、色情服务 |
| **H** | 仇恨 | Hate | 仇恨言论、歧视性内容 |
| **A** | 酒精 | Alcohol | 酒类产品推广 |
| **F** | 枪支 | Firearms | 武器相关内容 |
| **T** | 烟草 | Tobacco/Vape | 烟草和电子烟产品 |

### 7.2 其他禁止/高风险内容

| 类别 | 风险级别 |
|------|---------|
| 赌博 | 禁止（除合法运营商） |
| 高利贷/发薪日贷款 | 禁止 |
| 大麻（即使当地合法） | 高风险 |
| 政治/宗教 | 高过滤风险 |
| 催债 | 需遵守 FDCPA 法规 |
| 房地产/保险 | 需额外合规措施 |

---

## 八、URL 使用规则

### 8.1 禁止使用免费短链接

**以下短链接服务被严格禁止：**
- tinyurl.com
- bitly.com / bit.ly
- goo.gl
- ow.ly
- 其他任何第三方免费短链接服务

**原因：** 运营商将包含免费短链接的消息视为高风险垃圾短信，会自动过滤。

### 8.2 正确做法

- 使用**品牌自有域名**（如 `https://brand.com/verify/abc123`）
- 如需短链接，使用**付费短链接服务**并配置**公司专属域名**
- 10DLC 和 Short Code 注册时可在模板中包含缩短的 URL

**错误示例：**
```
Don't: "Go to https://tinyurl.com/4585y8mr for your special offer!"
```

**正确示例：**
```
Do: "ExampleCorp: Your order is ready. Track at https://examplecorp.com/track/US12345. Reply STOP to opt out."
```

---

## 九、Opt-in / Opt-out 要求详解

### 9.1 Opt-in 要求

**必须在用户同意时披露以下信息：**
1. 品牌/产品描述
2. "Message and data rates may apply"（可能产生短信和数据费用）
3. 消息频率说明（如 "2 msgs/month" 或 "message frequency varies"）
4. 可公开访问的 Terms and Conditions 链接
5. 可公开访问的 Privacy Policy 链接

**Opt-in 确认消息模板：**
```
[Brand] alerts: You're now subscribed. 2 msgs/month. Msg & data rates
may apply. Reply HELP for help, STOP to cancel.
```

**必须记录的 Opt-in 信息：**
- 同意日期和时间
- 同意来源（网页、App、短信等）
- 确认细节

**关键规则：**
- 不同组织之间**不得共享** opt-in 列表
- 同一公司内不同品牌/产品也不应共享
- Opt-in 仅适用于特定消息类型

### 9.2 Opt-out 要求

**支持的关键词：**
- **STOP** — 退订（Toll-Free 号码由运营商级别管理，仅支持 STOP 和 UNSTOP）
- **HELP** — 获取帮助
- **CANCEL** / **UNSUBSCRIBE** / **QUIT** — 也应支持

**STOP 回复模板：**
```
You're unsubscribed from [Brand] alerts. No more messages will be sent.
Reply HELP, email help@brand.com, or call 1-800-XXX-XXXX for more info.
```

**HELP 回复模板：**
```
HELP: [Brand] alerts: email help@brand.com or call 1-800-XXX-XXXX.
2 msgs/month. Msg & data rates may apply. Reply STOP to cancel.
```

---

## 十、运营商与号码格式

### 10.1 主要运营商

| 运营商 | 市场份额（约） | 备注 |
|--------|-------------|------|
| AT&T | ~30% | 10DLC 限制 75条/分钟/Campaign |
| T-Mobile (含 Sprint) | ~30% | 10DLC 限制 2,000条/天/公司 |
| Verizon | ~30% | 使用自动过滤系统 |
| US Cellular | ~5% | 区域运营商 |
| 其他 MVNO | ~5% | 依赖三大运营商网络 |

### 10.2 号码格式

#### 标准格式
```
+1XXXXXXXXXX      ← 国家代码 +1 + 10位号码（含3位区号）
```

#### 示例
```
+12125551234      ← 纽约 (212)
+14155551234      ← 旧金山 (415)
+13105551234      ← 洛杉矶 (310)
```

#### AWS SMS 号码格式要求
- 必须使用 **E.164 格式**：`+1` 后跟 10 位数字
- 不含空格、破折号或括号
- 总长度 12 位（含 `+1`）

### 10.3 号码类型识别

| 号码类型 | 特征 |
|----------|------|
| 10DLC | 标准 10 位本地号码 |
| Short Code | 5-6 位数字（如 12345, 567890） |
| Toll-Free | 以 800/888/877/866/855/844/833 开头 |

---

## 十一、消息模板范例（英语）

### 11.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本 ASCII 字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含 emoji、商标符号 (R)/(TM) 等 |

> **注意：** 英语消息通常使用 GSM-7 编码。但包含 (R)、(TM)、emoji 等特殊字符会触发 Unicode 编码，将单条容量从 160 降至 70 字符，成本翻倍。

### 11.2 OTP 验证码模板

```
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.
```
(85字符，1条消息)

### 11.3 交易通知模板

**订单确认：**
```
[Brand] Your order #US12345 has been confirmed. Amount: $150.00.
Track: https://brand.com/orders/US12345
```
(96字符，1条)

**配送通知 - 已发货：**
```
[Brand] Your order #US12345 has shipped. Tracking:
https://brand.com/track/US12345 Est. delivery: Apr 10.
```
(99字符，1条)

**配送通知 - 派送中：**
```
[Brand] Your package is out for delivery. Est. arrival: today 2-5PM.
Track: https://brand.com/track/US12345
```
(101字符，1条)

**付款确认：**
```
[Brand] Payment received: $150.00 (order #US12345).
Receipt: https://brand.com/receipt/US12345
```
(89字符，1条)

**账户安全警报：**
```
[Brand] Alert: New sign-in detected on your account from a new device.
If this wasn't you: https://brand.com/security
```
(107字符，1条)

### 11.4 营销消息模板（需 opt-in 同意）

**促销活动：**
```
[Brand] Sale: 20% off your next purchase! Use code SAVE20.
Valid through 04/15. Shop: https://brand.com/sale
Reply STOP to opt out.
```
(126字符，1条)

**新品通知：**
```
[Brand] New arrivals are here! Be the first to shop.
Browse now: https://brand.com/new
Reply STOP to opt out.
```
(104字符，1条)

**会员积分提醒：**
```
[Brand] You have 500 points expiring on 04/30.
Redeem now: https://brand.com/rewards
Reply STOP to opt out.
```
(104字符，1条)

### 11.5 Opt-in 确认模板

```
[Brand] You're subscribed to [Brand] alerts. Msg frequency varies.
Msg & data rates may apply. Reply HELP for help, STOP to cancel.
```
(128字符，1条)

### 11.6 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 使用英语 |
| 编码 | 避免 emoji 和商标符号，保持 GSM-7 |
| STOP 提示 | 每条营销消息**必须**包含 "Reply STOP to opt out" |
| HELP 提示 | 建议在首条消息包含 "Reply HELP for help" |
| 费用提示 | 首条消息须包含 "Msg & data rates may apply" |
| URL | 使用品牌自有域名，**禁止免费短链接** |
| S.H.A.F.T. | 禁止色情、仇恨、酒精、枪支、烟草内容 |

---

## 十二、定价信息

### 12.1 号码费用

| 号码类型 | 月费 | 一次性费用 |
|----------|------|-----------|
| 10DLC | ~$1/号码 + $10/Campaign | Brand $4 + Vetting $40（可选） |
| Short Code | $995/月 | 需向 AWS 确认 |
| Toll-Free | $2/月 | 无 |

### 12.2 消息费用（参考值）

| 费用类型 | 金额 |
|----------|------|
| AWS 基础消息费 | ~$0.00467-$0.00581/条（因号码类型而异） |
| 运营商费（Carrier Fee） | ~$0.00705/条 |
| **合计（约）** | **~$0.01172-$0.01286/条** |

> 实际费率请在 AWS 控制台确认或下载定价 CSV：`https://aws.amazon.com/end-user-messaging/pricing/`

### 12.3 默认消费阈值

- 新账户默认 **$1.00 USD/月**
- 需通过 Support Case 申请提高

### 12.4 成本优化建议

1. **使用 GSM-7 编码** — 避免 emoji/特殊符号，保持 160 字符/条
2. **精简消息内容** — 保持单条消息在 160 字符内避免拼接
3. **维护号码列表** — 定期下载去活报告，清理无效号码
4. **分离消息类型** — 事务性和营销性消息使用不同号码
5. **选择合适号码类型** — 中小流量用 10DLC/TFN，高流量用 Short Code

---

## 十三、合规清单

在美国发送 SMS 前，确保完成：

- [ ] 获取所有接收者的**明确书面 opt-in 同意**
- [ ] Opt-in 流程披露：品牌名、消息频率、费用说明、T&C 链接、隐私政策链接
- [ ] 完成 10DLC 注册（Brand + Campaign + Number Association）
- [ ] 或完成 Short Code / Toll-Free 注册
- [ ] 实现 STOP / HELP 关键词自动处理
- [ ] 每条营销消息包含 "Reply STOP to opt out"
- [ ] 首条消息包含 "Msg & data rates may apply"
- [ ] 消息内容不含 S.H.A.F.T. 禁止内容
- [ ] 消息中不使用免费短链接（tinyurl、bitly 等）
- [ ] 使用品牌自有域名作为 URL
- [ ] 发送时间限制在收件人当地 9:00-20:00
- [ ] 避免周日和国定假日发送营销消息
- [ ] 定期下载并处理号码去活报告
- [ ] 保留 opt-in 记录至少 90 天
- [ ] 事务性消息不混入营销内容
- [ ] 正确格式化号码（E.164：`+1XXXXXXXXXX`）
- [ ] 申请提高默认 $1.00 消费阈值
- [ ] 不同组织间不共享 opt-in 列表

---

## 十四、Support Case 模板汇总

### 14.1 生产环境升级 + 消费限额提升

```
Subject: Production Access & Spend Limit Increase - US SMS

We are requesting production access for SMS messaging to the
United States (+1).

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
Target Number Type: 10DLC / Short Code / Toll-Free

Opt-in Process:
[描述用户如何同意接收消息]

Sample Messages:
1. "[Brand] Your verification code is: 385291. Valid for 5 min."
2. "[Brand] Your order #12345 has been confirmed. Amount: $150.00."
```

### 14.2 10DLC 吞吐量提升

```
Subject: 10DLC Throughput Rate Increase Request

We have completed Brand Vetting for our 10DLC registration and
request an increase in our SMS sending rate.

Brand Registration ID: [Brand ID]
Campaign Registration ID: [Campaign ID]
Current Vetting Score: [分数]
Requested MPS: [期望的每秒消息数]

Current monthly volume: [当前月发送量]
Expected monthly volume: [期望月发送量]
```

---

## 十五、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| TCPA 违规集体诉讼 | **极高** | 严格执行 opt-in/opt-out，保留完整同意记录 |
| 10DLC 注册被拒 | 高 | 提供详尽的使用说明和合规的消息模板 |
| 运营商内容过滤 | 中 | 避免 S.H.A.F.T. 内容，使用品牌域名，不用免费短链接 |
| T-Mobile 每日限额耗尽 | 中 | 完成 Brand Vetting 提升吞吐量，或使用 Short Code |
| 号码去活后继续发送 | 中 | 每日下载去活报告，自动清理列表 |
| 10DLC 注册周期长（国际公司） | 中 | 提前规划，尽早提交申请 |
| Toll-Free 号码被撤销 | 中 | 仅用于注册时声明的用途 |
| 费用超预算 | 低 | 使用 GSM-7 编码，精简消息，监控 AWS 账单 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。美国 TCPA 法规、FCC 规则、运营商政策和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询美国当地法律顾问以确保合规。
