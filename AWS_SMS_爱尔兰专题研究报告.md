## 一、AWS 上的爱尔兰 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +353 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | 支持 |
| Sender ID | **支持（必须在 ComReg Registry 注册）** |
| Toll-free | 不支持 |
| 双向 SMS | **不支持** |
| 消息拼接 | 支持 |
| MMS | 不支持 |
| 国际发送 | 不支持 |

**核心限制：** 爱尔兰必须在 ComReg SMS Sender ID Registry 注册 Sender ID 才能正常发送 SMS。不支持双向 SMS，意味着用户无法通过回复 STOP 退订，需提供替代退出机制。不支持短码和国际发送。

---

## 二、费用总览

### 2.1 号码费用

| 费用项目 | 一次性费用 | 月度费用 | 备注 |
|---------|-----------|---------|------|
| Sender ID | 免费 | 免费 | 需完成 ComReg Registry 注册 |

> 爱尔兰不支持短码和 Toll-Free。Sender ID 是主要发送方式，注册本身免费。

### 2.2 每条消息费用

| 发送方式 | 每条消息费用 | 备注 |
|---------|------------|------|
| Sender ID / 长码 | $0.09556 | — |

### 2.3 月度消费阈值

新账户默认 **$1.00 USD/月**，需通过 AWS Support Case 申请提高。

## 三、ComReg SMS Sender ID Registry 注册详细流程

### 3.1 概述

ComReg（Commission for Communications Regulation，通信监管委员会）是爱尔兰的电信监管机构。自 ComReg SMS Sender ID Registry 启用以来，所有向爱尔兰发送 A2P SMS 的企业必须完成注册。

注册分为**两个阶段**：先在 ComReg 门户注册，再在 AWS 控制台注册。

### 3.2 第一阶段：ComReg 门户注册

**访问地址：** https://senderid.comreg.ie/sender-id-sign-up

**步骤：**

1. **访问 ComReg Sender ID 注册门户**
2. **Section 1：填写公司基本信息**
   - 公司名称
   - 联系人信息
   - 业务类型
3. **Section 2：选择第三方分配（关键步骤）**
   - 选择 **"Assign a 3rd party"**
   - 在第三方列表中选择 **Amazon Web Services**
   - > **警告：如果未选择 Amazon Web Services，将直接影响 SMS 投递！**
4. **Section 3：选择 OPA（关键步骤）**
   - 必须选择**全部四个 OPA（Originating Point of Access）**：
     - **Sinch Sweden AB**
     - **Telesign Corporation**
     - **Twilio Inc**
     - **Vonage**
   - > **警告：必须选择全部四个 OPA！如遗漏任何一个，将影响通过对应聚合商路由的短信投递。**
5. **完成注册，获取 SIDO Number**
   - 注册完成后，在 ComReg 门户账户图标下查找 **SIDO Number**（Sender ID Owner Number ID）
   - **记录此号码，第二阶段需要使用**

### 3.3 第二阶段：AWS 控制台注册

**访问地址：** https://console.aws.amazon.com/sms-voice/

**步骤：**

1. **创建注册**
   - 导航至 **Registrations** → **Create registration**
   - 选择 **Ireland sender ID registration**

2. **填写 Sender ID Info**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | Sender ID | 是 | 3-11个字母数字字符 |
   | Description | 否 | 描述（可选） |
   | Proof of connection document | 否 | 证明文件（PDF/PNG/JPEG，最大500KB） |

3. **填写 Ireland-Specific Info（爱尔兰专属字段）**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | **SIDO Number ID** | **是** | 从 ComReg 门户获取的 Sender ID Owner Number ID |
   | **Company Registration Documentation** | **是** | 公司注册证书（Certificate of Incorporation）、商业执照或等效文件 |

4. **填写 Company Info**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | Company Name | 是 | 公司法定名称 |
   | Company Identification Number | 是 | 税号/EIN/VAT Number |
   | Doing Business As (DBA) | 是 | 商业名称（如与公司名不同） |
   | Website URL | 是 | 公司网站 |
   | Area of Business | 是 | 业务领域 |
   | Customer Care Email | 是 | 客服邮箱 |
   | Customer Care Phone | 是 | 客服电话 |

5. **填写 Company Address**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | Street Address | 是 | 街道地址 |
   | City | 是 | 城市 |
   | State/Province | 是 | 州/省/郡 |
   | Postal Code | 是 | 邮编（爱尔兰 Eircode） |
   | Country | 是 | 两位 ISO 国家代码（如 IE） |

6. **填写 Contact Info**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | First Name | 是 | 联系人名 |
   | Last Name | 是 | 联系人姓 |
   | Email | 是 | 联系人邮箱 |
   | Phone Number | 是 | 联系人电话 |

7. **填写 Messaging Use Case**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | Use Case Category | 是 | 选项：One-time passwords / Alerts / Notifications / Polling and surveys / Info on demand / Promotions and Marketing / Other |
   | Monthly SMS Volume | 是 | 预计月发送量 |
   | Opt-in Workflow Description | 是 | 40-500字符，必须包含：程序描述、组织标识、opt-in 详情 |

8. **填写 Message Samples**

   | 字段 | 必填 | 说明 |
   |------|------|------|
   | Message Sample 1 | 是 | 消息样本（英语） |
   | Message Sample 2 | 否 | 额外样本 |
   | Message Sample 3 | 否 | 额外样本 |

9. **Review and Submit** — 核对所有信息后提交

### 3.4 不注册的后果

| 后果 | 详情 |
|------|------|
| Sender ID 被替换为 **"Likely Scam"** | 未注册的 Sender ID 会被自动替换为 "Likely Scam" 显示在收件人手机上 |
| 流量被过滤或屏蔽 | 监管机构和运营商可自行决定过滤或屏蔽未注册流量 |
| 品牌严重受损 | 消息显示为 "Likely Scam" 将严重损害品牌信誉 |

### 3.5 需要准备的材料清单

1. **ComReg 门户账号** — 注册并登录
2. **SIDO Number** — 从 ComReg 门户获取
3. **公司注册证书（Certificate of Incorporation）** — 或商业执照/等效文件
4. 公司基本信息（名称、税号、地址、网站、业务领域）
5. 客服联系方式（邮箱和电话）
6. 联系人信息
7. Sender ID（3-11个字母数字字符）
8. 预计月发送量
9. Opt-in 流程描述（40-500字符）
10. 英语消息样本（至少1条，最多3条）

---

## 四、不支持双向 SMS 的替代退出方案

### 4.1 问题

AWS 上爱尔兰不支持双向 SMS，用户无法回复 STOP 退订。必须提供替代 opt-out 机制。

### 4.2 四种替代方案

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

Your number: +353 XX XXX XXXX
Upon confirmation, you will stop receiving messages within 24 hours.
```

#### 方案二：邮件退出

```
Unsubscribe: email optout@brand.com with your phone number.
```

#### 方案三：客服电话退出

```
Unsubscribe: call +353-X-XXX-XXXX
```

#### 方案四：App 内设置

在 App 设置中提供 SMS 偏好管理页面。

### 4.3 最佳实践

1. 每条营销消息**必须**包含退出指引
2. 使用品牌自有域名短链接（避免 bit.ly 等被运营商过滤）
3. 退出应即时生效，24小时内停止发送
4. 保留所有退出记录
5. 在首次 opt-in 时明确告知退出方式
6. 定期发送提醒告知如何退出

---

## 五、爱尔兰电信监管

### 5.1 监管机构

**ComReg**（Commission for Communications Regulation，通信监管委员会）是爱尔兰的独立电信监管机构，负责监管电子通信网络和服务。

**DPC**（Data Protection Commission，数据保护委员会）是爱尔兰的数据保护监管机构，也是许多大型科技公司在欧盟的主要监管机构（因其欧洲总部设在爱尔兰）。

### 5.2 A2P SMS 监管法规

| 法规 | 说明 |
|------|------|
| European Communities (Electronic Communications Networks and Services) (Privacy and Electronic Communications) Regulations 2011 (S.I. 336 of 2011) | ePrivacy 指令在爱尔兰的实施 |
| EU GDPR | 爱尔兰作为欧盟成员国直接适用 |
| Data Protection Act 2018 | 爱尔兰数据保护法 |
| Communications Regulation Act 2002 | 通信监管法 |
| ComReg SMS Sender ID Registry | Sender ID 注册制度 |

### 5.3 禁止/限制内容

| 类别 | 限制程度 |
|------|---------|
| 未经同意的营销 | 禁止 |
| 欺诈/误导性内容 | 禁止 |
| 成人内容 | 严格限制 |
| 赌博推广 | 需持牌 |
| 金融推广 | 需 Central Bank of Ireland 授权 |
| 政治宣传 | 选举期间有特殊规定 |

### 5.4 发送时间建议

虽无明确法定限制，行业最佳实践：
- **推荐发送时段：** 08:00-21:00 IST（爱尔兰标准时间，UTC+0/UTC+1 夏令时）
- 紧急/交易类消息除外
- 避免周日和公众假期发送营销消息

---

## 六、爱尔兰数据保护法

### 6.1 法律框架

爱尔兰数据保护由以下法律共同构成：
- **EU GDPR**（直接适用）
- **Data Protection Act 2018**
- **ePrivacy Regulations (S.I. 336 of 2011)**

### 6.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **合法基础** | 处理手机号码需有合法基础（同意、合同履行、合法利益等） |
| **营销同意** | 向个人发送营销 SMS 必须获得事先明确同意 |
| **软性 opt-in** | 既有客户关系 + 类似产品/服务 = 可使用软性 opt-in |
| **透明度** | 隐私政策必须说明 SMS 用途 |
| **数据主体权利** | 访问权、更正权、删除权、限制处理权、数据可携带权、反对权 |
| **数据泄露通知** | 72小时内向 DPC 通报 |
| **国际数据传输** | 向 EEA 以外传输数据需有适当保障措施 |
| **DPO 指定** | 大规模处理个人数据的组织可能需要指定数据保护官 |

### 6.3 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| GDPR 高级别 | 最高 **2000万欧元** 或全球年营业额 **4%**（取高者） |
| GDPR 标准级别 | 最高 **1000万欧元** 或全球年营业额 **2%**（取高者） |
| ePrivacy 违规 | DPC 可发出罚款通知 |
| 执法行动 | DPC 可发出执行通知、信息通知、审计 |

> **特别注意：** 爱尔兰 DPC 是许多大型科技公司（如 Meta、Google、Apple）在欧盟的主要数据保护监管机构，执法记录活跃且处罚力度大。

---

## 七、运营商与号码格式

### 7.1 主要运营商

| 运营商 | 备注 |
|--------|------|
| Vodafone Ireland | 最大运营商之一 |
| Three Ireland | CK Hutchison Holdings |
| eir (eircom) | 原国有电信公司 |
| Tesco Mobile Ireland | MVNO（基于 Three 网络） |
| 48 (GoMo) | eir 旗下低成本品牌 |

### 7.2 号码格式

#### 基本结构
- 国家代码：+353
- 国内号码长度：移动号码通常为9位（去掉前导0后）

#### 号码类型

| 类型 | 格式 | 示例 |
|------|------|------|
| 移动号码 | +353 8X XXX XXXX | +353851234567 |
| 都柏林固话 | +353 1 XXX XXXX | +35311234567 |
| 其他城市 | +353 XX XXX XXXX | +353211234567 |

#### 移动号码前缀

| 前缀 | 运营商 |
|------|--------|
| 083 | Three |
| 085 | eir / Three |
| 086 | Vodafone |
| 087 | Vodafone |
| 089 | Three |

> **注意：** 由于号码可携带，前缀不能可靠地判断当前运营商。

#### 正确的 AWS SMS 号码格式
```
+353851234567      ← 移动号码（去掉国内前导0）
+353871234567      ← 移动号码
```

> **SMS 发送时去掉国内拨号前导 `0`！** 例如国内号码 085 123 4567 应格式化为 +353851234567。

### 7.3 运营商技术细节

- Sender ID 通过 ComReg Registry 管理
- 未注册 Sender ID 显示为 "Likely Scam"
- 运营商有垃圾信息过滤机制
- 支持号码携带
- 固定电话不可达

---

## 八、消息模板范例（英语）

### 8.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本拉丁字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含 emoji 或特殊字符 |

> **建议：** 英语消息天然适合 GSM-7 编码，只要避免使用 emoji 和非拉丁字符，即可充分利用160字符的单条容量。爱尔兰消息使用英语（不使用爱尔兰语/盖尔语），避免 emoji。

### 8.2 OTP 验证码模板

**GSM-7 兼容（推荐，160字符/条）：**
```
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.
```
(85字符，1条消息)

### 8.3 交易通知模板

**订单确认（GSM-7）：**
```
[Brand] Your order #IE12345 has been confirmed. Amount: EUR 79.99.
Estimated delivery: 10 Apr. Info: https://brand.com/o/IE12345
```
(117字符，1条)

**配送通知 - 已发货（GSM-7）：**
```
[Brand] Your order #IE12345 has been dispatched. Track:
https://brand.com/track/IE12345 Estimated delivery: 10 Apr.
```
(110字符，1条)

**配送通知 - 已签收（GSM-7）：**
```
[Brand] Your order #IE12345 has been delivered.
Rate your experience: https://brand.com/review/IE12345
```
(97字符，1条)

**付款确认（GSM-7）：**
```
[Brand] Payment received: EUR 79.99 (order #IE12345).
Receipt: https://brand.com/receipt/IE12345
```
(89字符，1条)

**账户安全警报（GSM-7）：**
```
[Brand] Alert: A sign-in was detected from a new device.
If this wasn't you: https://brand.com/security
```
(96字符，1条)

### 8.4 营销消息模板（需 opt-in 同意）

**促销活动（GSM-7）：**
```
[Brand] Get 20% off your next purchase! Use code SAVE20.
Valid until 15/04. Shop: https://brand.com/sale
Opt out: https://brand.com/unsub/{TOKEN}
```
(140字符，1条)

> **注意：** 爱尔兰不支持双向 SMS，营销消息的退出方式必须使用链接而非 STOP 关键词。

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

### 8.5 Opt-in 确认模板

**首次订阅确认（GSM-7）：**
```
[Brand] Welcome! You'll receive account alerts and notifications.
Msg frequency varies. Opt out: https://brand.com/unsub/{TOKEN}
```
(118字符，1条)

### 8.6 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 使用英语 |
| 编码 | 使用 GSM-7（避免 emoji） |
| € 符号 | 扩展字符占2位，建议用 EUR 替代 |
| 退出指引 | 营销消息必须包含 |
| 退出方式 | 使用网页链接（无双向 SMS，不能用 STOP） |
| 货币 | 使用 EUR 或 € |

---

## 九、定价信息

### 9.1 AWS 定价

AWS 未在公开文档直接列出爱尔兰每条费率，需通过：
1. 下载 AWS 定价 CSV：`aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

**已知费用结构：**

| 费用项 | 说明 |
|--------|------|
| 短码 | 不支持 |
| 长码月租 | 需联系 AWS 确认 |
| Sender ID | 通常免费（仅需注册） |
| 每条消息费 | 需参考 CSV |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

### 9.2 成本优化建议

1. **使用 Sender ID（免费）** — 爱尔兰不支持短码，Sender ID 是首选发送方式
2. **使用 GSM-7 编码** — 英语天然兼容，每条160字符
3. **精简消息内容** — 保持单条消息在160字符内避免拼接
4. **维护号码列表** — 定期清理无效号码
5. **使用品牌域名短链接** — 避免第三方短链接被过滤

---

## 十、合规清单

在爱尔兰发送 SMS 前，确保完成：

- [ ] 在 ComReg 门户完成 Sender ID 注册（第一阶段）
- [ ] Section 2 中选择 **Amazon Web Services** 作为第三方
- [ ] Section 3 中选择**全部四个 OPA**（Sinch、Telesign、Twilio、Vonage）
- [ ] 获取 **SIDO Number**
- [ ] 在 AWS 控制台完成 Sender ID 注册（第二阶段）
- [ ] 上传公司注册证书（Certificate of Incorporation）
- [ ] 获取所有接收者的明确 opt-in 同意（营销消息）
- [ ] 实施有效的 opt-out 机制（网页链接，因不支持双向 SMS）
- [ ] 遵守 EU GDPR / Data Protection Act 2018
- [ ] 遵守 ePrivacy Regulations (S.I. 336 of 2011)
- [ ] 消息内容使用英语
- [ ] 发送时间限制在 08:00-21:00 IST（营销消息）
- [ ] 每条营销消息包含发送者标识和退出链接
- [ ] 使用 GSM-7 编码（避免 emoji）
- [ ] 正确格式化号码（+353 + 号码，**去掉国内前导0**）
- [ ] 隐私政策中声明 SMS 发送用途
- [ ] 保留同意和退出记录
- [ ] 申请提高 AWS 消费阈值（默认 $1.00/月）

---

## 十一、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| 未注册 Sender ID 显示 "Likely Scam" | **极高** | 务必完成 ComReg + AWS 双阶段注册 |
| ComReg 注册遗漏 OPA | **高** | 确认 Section 3 中选择了全部四个 OPA |
| ComReg 注册未选 AWS | **高** | 确认 Section 2 中选择了 Amazon Web Services |
| GDPR 违规（DPC 执法） | **极高** | 爱尔兰 DPC 执法活跃，严格遵守 GDPR，建议咨询当地法律顾问 |
| 不支持双向 SMS 导致退出机制不完善 | 高 | 实施网页链接退出方案，保留退出记录 |
| 不支持短码 | 中 | 使用 Sender ID + 长码组合 |
| 不支持国际发送 | 中 | 使用支持爱尔兰的 AWS Region |
| 运营商内容过滤 | 中 | 使用标准模板，品牌域名 |
| 号码格式错误 | 低 | 实施号码格式验证（去掉前导0，加+353） |
| Unicode 导致成本翻倍 | 低 | 英语内容天然兼容 GSM-7，避免 emoji |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。爱尔兰法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询爱尔兰当地法律顾问以确保合规。
