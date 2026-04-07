# AWS SMS 注册表单填写指南与消息模板

> 调研日期：2026年4月7日
> 本文档为《AWS SMS 各国发送要求研究报告》的配套文档
> 包含：各国注册表单逐字段填写示例 + 多语言消息模板范例

---

## 目录

1. [需注册国家的表单填写指南](#一需注册国家的表单填写指南)
   - [美国 10DLC](#1-美国-us--10dlc-注册)
   - [新加坡 SSIR](#3-新加坡-sg--ssir-sender-id-注册)
   - [英国 MEF](#4-英国-gb--mef-sender-id-注册)
   - [爱尔兰 ComReg](#5-爱尔兰-ie--comreg-sender-id-注册)
   - [澳大利亚](#6-澳大利亚-au--sender-id-注册)
   - [泰国](#7-泰国-th--sender-id-注册)
   - [土耳其](#8-土耳其-tr--sender-id-注册)
   - [沙特阿拉伯](#9-沙特阿拉伯-sa--sender-id-注册)
   - [阿联酋](#10-阿联酋-ae--sender-id-注册)
2. [通用消息模板范例](#二通用消息模板范例)
   - [编码基础](#21-编码基础)
   - [OTP/验证码模板](#22-otp验证码模板多语言)
   - [交易通知模板](#23-交易通知模板)
   - [营销消息模板](#24-营销消息模板)
   - [Opt-in 确认模板](#25-opt-in-确认模板)
   - [Opt-out/HELP 响应模板](#26-opt-outhelp-响应模板)
3. [附录](#三附录)

---

## 一、需注册国家的表单填写指南

---

### 1. 美国 (US) — 10DLC 注册

美国使用 10DLC 体系，需依次完成：**品牌注册 → Campaign 注册 → 号码关联**

#### 1.1 品牌（Brand）注册

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| Legal company name | 是 | `Acme Technology Inc.`（须与 IRS 记录一致） |
| Country of tax registration | 是 | `US` |
| Tax ID (EIN) | 是 | `12-3456789`（9位，须与 IRS 一致） |
| Legal form of organization | 是 | `PRIVATE_FOR_PROFIT` |
| Stock symbol | 上市公司 | `ACME` |
| Stock exchange | 上市公司 | `NASDAQ` |
| Address/Street | 是 | `1234 Innovation Drive, Suite 500` |
| City | 是 | `San Francisco` |
| State | 是 | `CA` |
| Zip Code | 是 | `94105` |
| Country | 是 | `US` |
| Brand verification email | 是 | `john.smith@acmetech.com`（域名须与公司匹配） |
| DBA | 否 | `AcmeTech` |
| Vertical（行业） | 是 | `Technology` |
| Company website | 是 | `https://www.acmetech.com` |
| Support Email | 是 | `support@acmetech.com` |
| Support Phone Number | 是 | `+14155550100` |

#### 1.2 Campaign 注册

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| Campaign description | 是 | `AcmeTech account security alerts and OTP verification codes` |
| Vertical | 是 | `Technology` |
| Terms & Conditions | 是 | `https://www.acmetech.com/terms`（或上传 PDF） |
| Privacy Policy | 是 | `https://www.acmetech.com/privacy`（或上传 PDF） |
| Use case | 是 | `Two factor authentication` |
| Sub use case | 否 | `Account Notifications` |
| Number capabilities | 是 | `SMS` |
| Message type | 否 | `Transactional` |
| Subscriber opt-in | 是 | `Yes` |
| Subscriber opt-out | 是 | `Yes` |
| Subscriber help | 是 | `Yes` |
| Direct lending | 否 | `No` |
| Embedded link | 是 | `No` |
| Embedded phone number | 是 | `No` |

**Use Case 可选值：**
Account Notifications / Customer care / Delivery notifications / Fraud alert messaging / Two factor authentication / Marketing / Mixed / Low Volume / Public service announcement / Polling and voting / Security alert / Higher education / Charity

**Opt-in Workflow Description 范例（40-2048字符）：**

```
Customers opt-in to receive AcmeTech account security alerts by checking
the "I agree to receive SMS notifications" checkbox during account registration
on https://www.acmetech.com/signup. The opt-in page clearly states: "By checking
this box, you consent to receive account security alerts and one-time passwords
from AcmeTech. Message frequency varies. Message and data rates may apply. Reply
STOP to unsubscribe. Reply HELP for help. See our Terms of Service at
https://www.acmetech.com/terms and Privacy Policy at https://www.acmetech.com/privacy."
```

**Opt-in Confirmation Message 范例：**
```
You are now subscribed to AcmeTech Account Alerts. Msg frequency varies.
Msg&data rates may apply. Reply HELP for help, STOP to cancel.
```

**Help Message 范例（20-160字符）：**
```
AcmeTech Account Alerts: For help call 1-888-555-0100 or go to
acmetech.com/help. Msg&data rates may apply. Text STOP to cancel.
```

**Stop Message 范例（20-160字符）：**
```
You are unsubscribed from AcmeTech Account Alerts. No more messages
will be sent. Reply HELP for help or call 1-888-555-0100.
```

**Message Sample 范例（至少1条，每条最少20字符，不可用占位符）：**

```
Sample 1 (OTP):
Your AcmeTech verification code is 836291. This code expires in 10 minutes.
If you did not request this code, please ignore this message.

Sample 2 (Security Alert):
AcmeTech Security Alert: A new login was detected on your account from
San Francisco, CA. If this wasn't you, secure your account at
https://acmetech.com/security

Sample 3 (Order):
AcmeTech: Your order #A12345 has shipped and will arrive by April 10.
Track your package at https://acmetech.com/track/A12345. Reply STOP to opt out.
```

---

### 2. 新加坡 (SG) — SSIR Sender ID 注册

三步流程：**获取 UEN → AWS 表单 → SGNIC 注册**

#### 3.1 AWS 注册表单字段

**公司信息：**

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| Company Name | 是 | `Acme Technology Pte. Ltd.` |
| Tax ID (UEN) | 是 | `202312345A` |
| Company Website | 是 | `https://www.acmetech.sg` |
| Address 1 | 是 | `1 Raffles Place, #20-01 One Raffles Place` |
| City | 是 | `Singapore` |
| State/Province | 是 | `Singapore` |
| Postal Code | 是 | `048616` |
| Country | 是 | `SG` |

**联系人信息：**

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| First Name | 是 | `Wei Ming` |
| Last Name | 是 | `Tan` |
| Support Email | 是 | `weiming.tan@acmetech.sg` |
| Support Phone Number | 是 | `+6591234567` |

**Sender ID 信息：**

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| Sender ID | 是 | `AcmeTech`（3-11个字母数字） |
| Registering on behalf of another brand? | 是 | `False` |
| LOA Image | 代注册时 | PNG，最大400KB |
| Sender ID Connection | 否 | `AcmeTech is our registered trade name used in all customer-facing communications in Singapore.` |

**消息用途：**

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| Monthly SMS Volume | 是 | `10,001 - 50,000` |
| Use Case Category | 是 | `One-time passwords` |

**Use Case Description 范例：**
```
AcmeTech uses SMS to send one-time passwords (OTP) for user account
verification during login and password reset on our e-commerce platform
(https://www.acmetech.sg). Each user receives an OTP upon login attempt.
We estimate approximately 30,000 OTP messages per month. Messages are
transactional and time-sensitive, sent only when triggered by user action.
```

**Message Sample 范例：**
```
Sample 1: [AcmeTech] Your one-time verification code is 482915.
It expires in 5 minutes. Do not share this code with anyone.

Sample 2: [AcmeTech] Your account password was changed on 07 Apr 2026.
If you did not make this change, please contact support@acmetech.sg immediately.

Sample 3: [AcmeTech] Your order #SG20260407 is out for delivery.
Expected arrival: today by 6pm. Track at https://acmetech.sg/track/SG20260407
```

> **SGNIC 注册提醒：** 完成 AWS 表单后，须在 https://smsregistry.sg 注册，并将 **AMCS SG Private Limited** 列为参与聚合商。

---

### 4. 英国 (GB) — MEF Sender ID 注册

#### 4.1 表单字段

**公司信息：**

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| Company Name | 是 | `Acme Technology Ltd` |
| Tax ID | 是 | `GB123456789`（VAT）或 Companies House `12345678` |
| Company Website | 是 | `https://www.acmetech.co.uk` |
| Address 1 | 是 | `10 Downing Street Innovation Centre` |
| City | 是 | `London` |
| State/Province | 是 | `England` |
| Postal Code | 是 | `SW1A 2AA` |
| Country | 是 | `GB` |

**Sender ID 信息：**

| 字段 | 必填 | 说明 |
|------|------|------|
| Sender ID | 是 | `AcmeTech`（MEF 保护的须大小写完全一致） |
| LOA | MEF 保护的需要 | PDF，最大400KB，30天内签署 |
| Sender ID Connection | 否 | `AcmeTech is our registered trademark (UK IPO #UK00003456789)` |

**Use Case Description 范例：**
```
AcmeTech sends transactional SMS notifications to customers in the UK,
including order confirmations, delivery updates, and appointment reminders.
All messages are triggered by customer actions on our platform
(https://www.acmetech.co.uk). Estimated volume is 50,000 messages per month.
```

**Message Sample 范例：**
```
Sample 1: AcmeTech: Your order #UK78901 has been dispatched and will
arrive by 10 Apr. Track at https://acmetech.co.uk/track/UK78901

Sample 2: AcmeTech Reminder: Your appointment is scheduled for tomorrow
at 2:00 PM. Reply YES to confirm or call 020 7123 4567 to reschedule.

Sample 3: Your AcmeTech security code is 649301. This code is valid
for 10 minutes. Never share this code with anyone.
```

#### 4.2 LOA 授权书模板

```
LETTER OF AUTHORIZATION

Date: [日期，须在30天内]

To: AWS End User Messaging SMS Team

I, [姓名], authorized representative of [公司法定名称]
(Company Registration Number: [注册号]), hereby authorize the use
of the SMS Sender ID "[Sender ID]" for sending SMS messages through
AWS End User Messaging SMS services to recipients in the United Kingdom.

Company Details:
- Legal Company Name: [公司法定名称]
- Company Registration Number: [注册号]
- Website: [网站]
- Address: [完整地址]

Sender ID: [Sender ID，大小写须与申请一致]

Purpose: [发送目的描述]

Sample Message: "[示例消息]"

I confirm that [公司名] owns the rights to the "[Sender ID]"
brand and sender ID, and that all messages sent will comply with UK
telecommunications regulations and Ofcom guidelines.

Signature: ___________________________
Name: [签字人姓名]
Title: [职位]
Company: [公司名]
Date: [日期]
```

---

### 5. 爱尔兰 (IE) — ComReg Sender ID 注册

两阶段：**ComReg 门户注册 → AWS 表单**

#### 5.1 ComReg 门户注册要点

1. 访问 https://senderid.comreg.ie/sender-id-sign-up
2. Section 2 选择 **"Assign a 3rd party"** → 选择 **Amazon Web Services**
3. Section 3 必须选择**全部四个 OPA**：
   - Sinch Sweden AB
   - Telesign Corporation
   - Twilio Inc
   - Vonage
4. 完成后获取 **SIDO Number ID**

#### 5.2 AWS 表单字段

**爱尔兰特有字段：**

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| SIDO Number ID | 是 | `SIDO-IE-2026-00123`（从 ComReg 获取） |
| Company Registration Documentation | 是 | 上传 Certificate of Incorporation |
| Area of Business | 是 | `E-commerce and Technology Services` |
| Company Customer Care Email | 是 | `support@acmetech.ie` |
| Company Customer Care Phone | 是 | `+35312345678` |

**其他字段与英国类似。**

**Use Case Description 范例：**
```
AcmeTech Ireland uses SMS to send account security alerts and one-time
passwords to registered users of our e-commerce platform. Messages are
triggered by user actions such as login attempts, password resets, and
suspicious activity detection. All recipients are registered customers
who have explicitly opted in during account creation on https://www.acmetech.ie.
```

**Opt-in Workflow Description 范例（40-500字符）：**
```
Users opt-in during account registration on https://www.acmetech.ie/register
by checking the checkbox labeled "I consent to receive SMS security alerts
and verification codes from AcmeTech." The registration page clearly describes
the program, message frequency, and provides links to our Terms of Service
and Privacy Policy.
```

**Message Sample 范例：**
```
Sample 1: AcmeTech Alert: A login attempt was detected from a new device
in Dublin. If this was you, no action needed. Otherwise visit
https://acmetech.ie/security

Sample 2: Your AcmeTech verification code is 571843. Valid for 5 minutes.
Do not share this code. If you didn't request this, contact support@acmetech.ie

Sample 3: AcmeTech: Your payment of EUR 49.99 for order #IE20260407
has been confirmed. View details at https://acmetech.ie/orders/IE20260407
```

---

### 6. 澳大利亚 (AU) — Sender ID 注册

#### 6.1 表单字段

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| Sender ID | 是 | `MyBrand`（3-11位字母数字） |
| LOA | **是** | 下载模板填写，PDF/PNG/JPEG，最大500KB |
| Company registration doc | 非澳公司必须 | Certificate of Incorporation |
| Proof of sender ID connection | 若关联不明显 | 商标注册证书 |
| Company Name | 是 | `Acme Technology Pty Ltd` |
| Company ID | 是 | `ABN 12 345 678 901`（澳公司用 ABN/ACN） |
| Company website | 是 | `https://www.acmetech.com.au` |

**Use Case Category 选项（含营销）：**
One-time passwords / Purchase or delivery notifications / Public service announcements / Polling and surveys / Info on demand / **Promotions and Marketing** / Other

**Use Case Description 范例：**
```
Our company Acme Technology provides an e-commerce platform serving
Australian customers. We send one-time passwords (OTP) via SMS when
users log in or perform sensitive operations such as payment confirmation.
Users opt in by registering an account on our website and providing
their mobile number. No fees are charged for receiving SMS.
```

**Opt-in Workflow 范例：**
```
Customers opt in to receive SMS by providing their mobile phone number
during account registration on our website (https://www.acmetech.com.au/register).
A checkbox clearly states: "I agree to receive SMS notifications from
Acme Technology." No additional fees are charged.
```

**Message Sample 范例：**
```
Sample 1 (OTP): Your Acme Technology verification code is 385291.
This code expires in 5 minutes. Do not share this code with anyone.

Sample 2 (Delivery): Hi John, your order #AC20240315 has been shipped
via Australia Post. Track your delivery at
https://www.acmetech.com.au/track/AC20240315

Sample 3 (Marketing): Acme Tech: Enjoy 20% off all items this weekend!
Use code SAVE20 at checkout. Shop now: https://www.acmetech.com.au/sale
Reply STOP to opt out.
```

**上传文件清单：**

| 文件 | 必须 | 格式 | 限制 |
|------|------|------|------|
| LOA | 是 | PDF/PNG/JPEG | 500KB |
| Company registration doc | 非澳公司 | PDF/PNG/JPEG | 500KB |
| Proof of sender ID connection | 条件 | PDF/PNG/JPEG | 500KB |

---

### 7. 泰国 (TH) — Sender ID 注册

#### 7.1 关键字段

| 字段 | 必填 | 填写示例 |
|------|------|---------|
| Sender ID | 是 | `AcmeTH` |
| LOA | **是** | 模板：`Thailand_SenderId_LetterOfAuthorization.zip`，**必须签名+盖公章** |
| Company Name | 是 | `Acme Technology Co., Ltd.` |
| Company ID | 是 | `0105563012345`（泰国13位注册号） |

**Use Case Category（含营销和安全提醒）：**
OTP / **Account or security alert** / Purchase/delivery notifications / Public service / Polling / Info on demand / **Promotions and marketing** / Other

**Message Sample 范例：**
```
Sample 1 (OTP): [AcmeTH] Your login verification code is 482917.
Valid for 3 minutes. Never share this code.

Sample 2 (Security): AcmeTH Alert: A login attempt was detected from
a new device on 2026-04-07. If this was not you, please contact support
immediately at https://www.acmetech.co.th/help

Sample 3 (Delivery): Your order #TH20260407 from Acme has been dispatched.
Expected delivery: Apr 9. Track: https://www.acmetech.co.th/track/TH20260407
```

**上传文件：** LOA（必须，签名+盖章）+ Proof of sender ID connection（条件）

---

### 8. 土耳其 (TR) — Sender ID 注册

#### 8.1 特殊限制

- **仅允许事务性消息，禁止营销**
- **2026年4月1日起**：含 URL 的短信必须使用已注册 Sender ID，**禁止短链接**（bit.ly 等），仅允许品牌域名

#### 8.2 关键字段

| 字段 | 必填 | 说明 |
|------|------|------|
| Sender ID | 是 | 3-11字母数字 |
| LOA (NOC) | **是** | `Turkey_SenderId_LetterOfAuthorization.zip` |
| Vendor LOA | **是** | `Turkey_SenderId_VendorLetterOfAuthorization.zip` |
| Company Registration Doc | **是（所有公司）** | 不论国内外 |
| Acknowledgement of Transactional Content | **是** | 勾选确认仅事务性 |

**Use Case Category（无营销选项）：**
OTP / Purchase/delivery notifications / Public service / Polling / Info on demand / Other

**Message Sample 范例：**
```
Sample 1 (OTP): Your Acme verification code is 739215. Valid for 5 minutes.
Do not share this code with anyone.

Sample 2 (Delivery): Acme: Your order #TR2026040701 has shipped.
Estimated delivery: Apr 10. Track at https://www.acmetech.com.tr/track/TR2026040701

Sample 3 (Account): Acme Security: Your password was successfully changed
on 2026-04-07. If you did not make this change, contact us at
https://www.acmetech.com.tr/support
```

**上传文件：** LOA + Vendor LOA + Company Registration Doc（全部必须）+ Proof of sender ID（条件）

---

### 9. 沙特阿拉伯 (SA) — Sender ID 注册

#### 9.1 特殊限制

- **仅支持国际公司**（沙特本地公司不支持）
- **仅事务性消息，禁止营销**

#### 9.2 关键字段

| 字段 | 必填 | 说明 |
|------|------|------|
| Sender ID | 是 | 3-11字母数字 |
| Vodafone LOA/NOC | **是** | `SaudiArabia_SenderId_LetterOfAuthorization.zip` |
| Acknowledgement of Transactional Content | **是** | 确认仅事务性 |
| Country (公司地址) | 是 | **不能填 SA**（不支持本地公司） |

**Message Sample 范例：**
```
Sample 1 (OTP): Your Acme verification code is 294817.
This code is valid for 5 minutes. Do not share this code.

Sample 2 (Payment): Acme: Your payment of SAR 150.00 for order #SA20260407
has been processed. View details at https://www.acmetech.com/orders/SA20260407

Sample 3 (Delivery): Acme: Your order #SA20260407 is out for delivery.
Expected today between 2-5 PM. Track: https://www.acmetech.com/track/SA20260407
```

---

### 10. 阿联酋 (AE) — Sender ID 注册

#### 10.1 特殊限制

- **仅事务性消息，禁止营销**
- **文件要求最多**，本地公司需10+份文件
- 文件语言必须为**英语或阿拉伯语**，否则需认证翻译

#### 10.2 上传文件清单

**国际公司（4份）：**

| 文件 | 必须 | 格式 |
|------|------|------|
| Company Registration Doc (COI) | 是 | PDF/PNG/JPEG, 500KB |
| Etisalat LOA (International) | 是 | PDF/PNG/JPEG, 500KB |
| du LOA (International) | 是 | PDF/PNG/JPEG, 500KB |
| Proof of sender ID connection | 条件 | PDF/PNG/JPEG, 500KB |

**阿联酋本地公司（最多10份）：**

| 文件 | 必须 |
|------|------|
| Company Registration Doc (Trade License) | 是 |
| Etisalat LOA (Local) | 是 |
| du LOA (Local) | 是 |
| du Sender ID Registration Form | 是 |
| UAE Establishment Card | 是 |
| Authorized Signatory UAE ID | 是 |
| Authorized Signatory UAE Passport | 是 |
| Power of Attorney | 条件（签字人未列名时） |
| Ministry of Health Authorization | 条件（医院） |
| Proof of sender ID connection | 条件 |

**Message Sample 范例：**
```
Sample 1 (OTP): Your Acme verification code is 581943. Valid for 3 minutes.
Do not share this code with anyone.

Sample 2 (Order): Acme: Your order #AE20260407 has been confirmed.
Total: AED 225.00. Estimated delivery: Apr 9.
View details: https://www.acmetech.com/orders/AE20260407

Sample 3 (Delivery): Acme: Your package is out for delivery and will
arrive today between 10 AM - 1 PM.
Track: https://www.acmetech.com/track/AE20260407
```

---

## 二、通用消息模板范例

---

### 2.1 编码基础

| 编码类型 | 单条上限 | 拼接每段 | 最大总长 | 适用场景 |
|---------|---------|---------|---------|---------|
| GSM 7-bit | 160 字符 | 153 字符 | 1530 字符 | 英文、数字、基本标点 |
| Unicode/UCS-2 | 70 字符 | 67 字符 | 630 字符 | 中文、日文、韩文、emoji |

**常见陷阱：**
- `€` 符号在 GSM 扩展字符中占**2个字符位**，建议用 `EUR` 替代
- 弯引号 `"" ''` 不在 GSM 字符集，会触发 Unicode，建议用直引号 `" '`
- 商标符号 `(R)` `(TM)` 会触发 Unicode
- 西班牙语/葡萄牙语/波兰语/捷克语/罗马尼亚语的重音字符不在 GSM 字符集，会触发 Unicode
- 德语变音符号（u/o/a）和法语常用重音（e/a）在 GSM 字符集中，可保持160字符

---

### 2.2 OTP/验证码模板（多语言）

#### 英文（GSM，80字符）
```
Your [Brand] verification code is 385291. Valid for 5 minutes.
Do not share this code.
```

#### 中文（Unicode，45字符 = 1条）
```
【品牌名】您的验证码为385291，有效期5分钟。如非本人操作请忽略。
```

#### 法语（GSM 兼容，含常用重音）
```
[Brand] Votre code de verification est 385291. Valide 5 minutes.
Ne partagez pas ce code.
```

#### 德语（GSM 兼容，含变音符）
```
[Brand] Ihr Bestaetigungscode lautet 385291. Gueltig fuer 5 Minuten.
Teilen Sie diesen Code nicht.
```

#### 西班牙语（注意：含重音 = Unicode）
```
[Brand] Su codigo de verificacion es 385291. Valido por 5 minutos.
No comparta este codigo.
```
> 注：使用 `codigo`（无重音）可保持 GSM 编码

#### 葡萄牙语
```
[Brand] Seu codigo de verificacao e 385291. Valido por 5 minutos.
Nao compartilhe este codigo.
```

#### 日语（Unicode，35字符 = 1条）
```
【Brand】認証コード: 385291（5分間有効）。このコードを共有しないでください。
```

#### 韩语（Unicode）
```
[Brand] 인증번호 385291 (5분간 유효). 타인에게 공유하지 마세요.
```

---

### 2.3 交易通知模板

#### 订单确认（英文，GSM）
```
[Brand]: Your order #A12345 is confirmed. Total: $49.99.
We'll notify you when it ships. View: https://brand.com/orders/A12345
```

#### 订单确认（中文，Unicode）
```
【品牌名】您的订单#A12345已确认，金额¥349.00。发货后将通知您。
```

#### 配送通知 - 已发货（英文，GSM）
```
[Brand]: Your order #A12345 has shipped! Tracking:
https://brand.com/track/A12345 Estimated delivery: Apr 10.
```

#### 配送通知 - 派送中（英文，GSM）
```
[Brand]: Your package is out for delivery today.
Expected between 2-5 PM. Track: https://brand.com/track/A12345
```

#### 配送通知 - 已签收（英文，GSM）
```
[Brand]: Your order #A12345 has been delivered.
Rate your experience: https://brand.com/review/A12345
```

#### 付款确认（英文，GSM）
```
[Brand]: Payment of $49.99 received for order #A12345.
Receipt: https://brand.com/receipt/A12345
```

#### 付款确认（中文，Unicode）
```
【品牌名】收到您的付款¥349.00（订单#A12345）。查看收据：https://brand.cn/r/A12345
```

#### 预约提醒（英文，GSM）
```
[Brand] Reminder: Your appointment is on Apr 10 at 2:00 PM.
Reply YES to confirm or call 1-800-555-0100 to reschedule.
```

#### 预约提醒（中文，Unicode）
```
【品牌名】提醒：您的预约在4月10日14:00。回复Y确认，或拨打400-555-0100改期。
```

---

### 2.4 营销消息模板

> **注意：** 以下模板仅适用于**允许营销消息**的国家。土耳其、沙特、阿联酋等国**禁止营销短信**。

#### 促销活动（英文，GSM）
```
[Brand]: Spring Sale! 20% off all items this week.
Use code SPRING20. Shop: https://brand.com/sale
Reply STOP to opt out. Msg&data rates may apply.
```

#### 促销活动（中文，Unicode）
```
【品牌名】春季大促！全场8折，使用优惠码SPRING20。立即抢购：https://brand.cn/sale 回复TD退订
```

#### 新品发布（英文，GSM）
```
[Brand]: Introducing our new Pro Series! Be the first to experience it.
Shop now: https://brand.com/new Reply STOP to unsubscribe.
```

#### 会员通知（英文，GSM）
```
[Brand]: You have 500 reward points expiring on Apr 30!
Redeem now: https://brand.com/rewards Reply STOP to opt out.
```

---

### 2.5 Opt-in 确认模板

#### 单次确认（英文，GSM）
```
Welcome to [Brand] alerts! You'll receive order updates and offers.
Msg frequency varies. Msg&data rates may apply.
Reply HELP for help, STOP to cancel.
```

#### 双重确认 - Double Opt-in（英文，GSM）
```
[Brand]: Reply YES to confirm your subscription to [Brand] alerts.
Msg frequency varies. Msg&data rates may apply.
Reply STOP to cancel. Terms: https://brand.com/terms
```

#### 确认后响应（英文，GSM）
```
You're subscribed to [Brand] alerts! You'll receive up to 4 msgs/month.
Reply HELP for help, STOP to cancel.
```

#### Opt-in 确认（中文，Unicode）
```
【品牌名】您已成功订阅通知服务，每月约2-4条。回复TD退订，回复BZ获取帮助。
```

---

### 2.6 Opt-out/HELP 响应模板

#### STOP 退订响应（英文，GSM）
```
You've been unsubscribed from [Brand] messages.
No more messages will be sent. Reply HELP for help
or contact support@brand.com.
```

#### STOP 退订响应（中文，Unicode）
```
【品牌名】您已成功退订，将不再收到我们的短信。如需帮助请联系 support@brand.cn
```

#### HELP 帮助响应（英文，GSM）
```
[Brand] SMS Help: For support visit https://brand.com/help
or call 1-800-555-0100. Msg frequency varies.
Msg&data rates may apply. Reply STOP to cancel.
```

#### HELP 帮助响应（中文，Unicode）
```
【品牌名】短信帮助：访问 https://brand.cn/help 或拨打400-555-0100。回复TD退订。
```

---

## 三、附录

### 附录A：10国注册复杂度对比

| 国家 | 注册体系 | LOA数量 | 外部注册 | 文件复杂度 | 允许营销 |
|------|---------|---------|---------|-----------|---------|
| 美国 US | 10DLC Brand+Campaign | 0 | Campaign Registry | 中 | 是 |
| 新加坡 SG | SSIR + SGNIC | 0-1 | ACRA + SGNIC | 中 | 是 |
| 英国 GB | MEF Registry | 0-1 | MEF（如适用） | 低-中 | 是 |
| 爱尔兰 IE | ComReg | 0 | ComReg 门户 | 中 | 是 |
| 澳大利亚 AU | LOA | 1 | 无 | 低 | 是 |
| 泰国 TH | LOA（签名+盖章） | 1 | 无 | 低 | 是 |
| 土耳其 TR | LOA + Vendor LOA | 2 | 无 | 中 | **否** |
| 沙特 SA | Vodafone LOA | 1 | 无 | 低 | **否** |
| 阿联酋 AE | 双运营商 LOA | 2-3 | 无 | **高** | **否** |

### 附录B：消息编码快速检查清单

发送前检查以下项目，避免意外触发 Unicode 导致字符数减半：

- [ ] 是否包含弯引号 `""` `''`？→ 替换为直引号 `"` `'`
- [ ] 是否包含 `(R)` `(TM)` `(C)` 等特殊符号？→ 删除或写成 (R)
- [ ] 是否包含 emoji？→ 会触发 Unicode（70字符/条）
- [ ] 是否包含 `€` 符号？→ 在 GSM 中占2位，建议用 EUR
- [ ] 西班牙语/葡萄牙语是否使用了重音字符？→ 会触发 Unicode
- [ ] 中文/日文/韩文内容？→ 必定 Unicode（70字符/条），注意控制长度

### 附录C：各国 Opt-in/Opt-out 关键词

| 语言/国家 | 退订关键词 | 帮助关键词 | 备注 |
|----------|----------|----------|------|
| 英语（通用） | STOP | HELP | 大多数国家 |
| 西班牙语 | PARAR / STOP | AYUDA / HELP | 拉美国家 |
| 法语 | ARRET / STOP | AIDE / HELP | 法国 |
| 德语 | STOPP / STOP | HILFE / HELP | 德国/奥地利/瑞士 |
| 葡萄牙语 | PARAR / STOP | AJUDA / HELP | 巴西/葡萄牙 |
| 日语 | STOP / 停止 | HELP / ヘルプ | 日本 |
| 阿拉伯语 | STOP / ايقاف | HELP / مساعدة | 沙特/阿联酋 |

> **免责声明：** 本指南基于 2026年4月 AWS 官方文档编写。各国要求可能随时变更，实际使用前请在 AWS 控制台确认最新信息。
