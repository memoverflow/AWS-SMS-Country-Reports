# 泰国 (TH, +66) AWS SMS 发送专题研究报告

> 调研日期：2026年4月7日
> 数据来源：AWS 官方文档、NBTC 官网、泰国法律资料

---

## 一、AWS 上的泰国 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +66 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持（需注册，需 LOA + 公司印章）** |
| Toll-free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| MMS | 不支持（转换为 SMS+URL） |
| 国际发送 | 支持 |

**核心特点：** 泰国是少数**支持促销和营销类消息**的国家之一。Sender ID 注册需提供 LOA 并加盖公司印章。不支持短码，高吞吐量场景需通过多个长码或 Sender ID 实现。

---

## 二、费用总览

### 2.1 号码费用

| 费用项目 | 一次性费用 | 月度费用 | 备注 |
|---------|-----------|---------|------|
| Sender ID | $0 | $0 | 需 LOA + 公司印章 |

### 2.2 每条消息费用

| 号码类型 | 基础价格/条 | 备注 |
|---------|-----------|------|
| Sender ID | $0.01272 | 每条消息段（message segment）计费 |

### 2.3 月度消费阈值

新账户默认 $1.00 USD/月，需申请提高。

---

## 三、Sender ID 注册详细流程

### 3.1 注册方式

泰国 Sender ID 需通过 AWS 控制台提交注册，并提供签署且盖章的 LOA（授权书）。

### 3.2 LOA 模板

**下载地址（AWS 提供）：**
```
Thailand_SenderId_LetterOfAuthorization.zip
```

从 AWS End User Messaging SMS 控制台的 Sender ID 注册页面下载此模板。

### 3.3 LOA 特殊要求

| 要求 | 说明 |
|------|------|
| **授权签署人签名** | 必须由公司授权代表亲笔签署 |
| **公司印章** | **必须加盖公司印章（Company Seal / Company Stamp）** |
| 文件格式 | PDF，最大400KB |
| 日期要求 | 签署日期须在最近30天内 |

> **重要：** 泰国 LOA 的公司印章是强制要求，缺少印章将导致注册被拒。如公司无正式印章，需提前制作或咨询 AWS 支持团队是否接受替代方案。

### 3.4 注册所需材料

| 材料 | 说明 |
|------|------|
| **LOA 授权书** | 下载 `Thailand_SenderId_LetterOfAuthorization.zip`，填写、签署并加盖公司印章 |
| 公司法定名称 | 与注册文件一致 |
| 公司注册号 | 泰国公司：税务识别号 (Tax ID)；国际公司：EIN/VAT |
| 公司地址 | 完整注册地址 |
| 联系人信息 | 姓名、邮箱、电话 |
| 公司网站 | 官方网站 URL |
| Sender ID | 3-11个字母数字字符（如 "MyBrand"） |
| 消息样本 | 1-3条泰语或英语消息模板 |
| 使用场景描述 | OTP / 事务性 / 营销等 |
| 预估月发送量 | 月度消息数量 |

### 3.5 控制台注册步骤

1. 登录 AWS End User Messaging SMS 控制台
2. 导航至 **Registrations** → **Create registration**
3. 选择 **Sender ID registration** → 选择 **Thailand**
4. 填写公司信息和 Sender ID
5. 上传签署并盖章的 LOA 文件（PDF格式，最大400KB）
6. 填写消息样本和使用场景
7. 提交注册，等待审核

### 3.6 注册时间线

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 初审 | 1-3个工作日 |
| 运营商审核 | 1-4周 |
| Sender ID 激活 | 1-2个工作日 |
| **总计** | **1-6周** |

---

## 四、长码申请流程

### 4.1 申请方式

泰国长码可通过 AWS 控制台直接购买，也可通过 Support Case 申请。

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
| Region | 你使用的 AWS 区域（如 ap-southeast-1） |
| Resource Type | Dedicated SMS Long Codes |
| Limit Type | 选择消息类型（见下方） |
| Limit Increase Value | **1**（即申请 1 个专用长码） |

**Limit Type 消息类型选项：**
- One Time Password/Two-Factor Authentication
- Promotional/Marketing
- Transactional
- Transactional/Notifications/OTP/2FA

### 4.3 Case Description 模板

```
Subject: Request for Dedicated Long Code and Sender ID - Thailand (TH)

We are requesting a dedicated SMS long code and Sender ID registration
for Thailand (+66).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- Tax ID / EIN / VAT: [公司识别号]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Desired Sender ID: [品牌名，如 MyBrand]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)
- Tertiary: Promotional offers and marketing campaigns (with user consent)

Target Carriers: AIS, DTAC/True (True Corp), TrueMove H

Expected Monthly Volume: [预估月发送量，如 50,000 messages]

Sample Messages (Thai):
1. "[Brand] รหัสยืนยันของคุณคือ: 385291 ใช้ได้ภายใน 5 นาที
   อย่าแชร์รหัสนี้กับใคร"
2. "[Brand] คำสั่งซื้อ #TH12345 ได้รับการยืนยันแล้ว ยอดรวม: 4,500 บาท
   ดูรายละเอียด: https://brand.com/orders/TH12345"
3. "[Brand] โปรโมชั่นพิเศษ! ลด 20% สำหรับการสั่งซื้อครั้งถัดไป
   ใช้โค้ด SAVE20 ดูเพิ่มเติม: https://brand.com/sale
   ยกเลิก: ตอบ STOP"

Sample Messages (English):
1. "[Brand] Your verification code is: 385291. Valid for 5 minutes.
   Do not share this code."
2. "[Brand] Your order #TH12345 confirmed. Total: THB 4,500.
   Details: https://brand.com/orders/TH12345"

Opt-in Process:
Users register on our website/app and provide their Thai mobile number.
During registration, they check a consent box stating (in Thai):
"ข้าพเจ้ายินยอมรับข้อความ SMS จาก [Brand] สำหรับการยืนยันบัญชี
แจ้งเตือนธุรกรรม และข้อเสนอส่งเสริมการขาย"

Opt-out Mechanism:
Users can reply STOP to unsubscribe from SMS messages. They can also
opt out via their account settings or by emailing optout@brand.com.

We have prepared the signed and sealed LOA document and will upload it
during the Sender ID registration process in the console.

We also request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.
```

---

## 五、泰国电信监管 (NBTC)

### 5.1 监管机构

**NBTC**（National Broadcasting and Telecommunications Commission，国家广播和电信委员会）是泰国通信和广播监管机构，成立于2010年12月20日，负责监管无线电频率分配、广播和电信服务。

### 5.2 A2P SMS 监管现状

泰国的 A2P SMS 监管主要通过以下框架：
- NBTC 电信服务法规
- 个人数据保护法（PDPA, B.E. 2562 / 2019年）
- 计算机犯罪法（Computer Crime Act, B.E. 2550 / 2007年）
- 运营商自律机制

### 5.3 禁止/限制内容

| 类别 | 限制程度 |
|------|---------|
| 未经同意的商业消息 | 禁止 |
| 欺诈/误导性内容 | 禁止 |
| 冒犯王室内容 | **严格禁止（刑事犯罪）** |
| 成人内容 | 严格限制 |
| 赌博推广 | **禁止（赌博在泰国违法）** |
| 政治敏感内容 | 严格限制 |
| 金融诈骗/钓鱼 | 禁止 |
| 涉及毒品内容 | 严格禁止 |

> **特别警告：** 泰国对冒犯王室（lèse-majesté）的处罚极其严厉，可判处 3-15 年监禁。任何消息内容必须避免涉及泰国王室。

### 5.4 发送时间建议

虽无明确法定限制，行业最佳实践：
- **推荐发送时段：** 08:00-21:00 ICT（UTC+7）
- 紧急/交易类消息除外
- 避免泰国国定假日发送营销消息（如宋干节、水灯节等）

---

## 六、泰国数据保护法 (PDPA)

### 6.1 法律概述

**PDPA**（Personal Data Protection Act, B.E. 2562）是泰国首部综合性个人数据保护法律，于2019年颁布，2022年6月1日全面生效。该法以欧盟 GDPR 为蓝本制定。

### 6.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **同意** | 营销 SMS 必须获得明确的、知情的同意（Consent） |
| **合法基础** | 数据处理须有合法基础：同意、合同履行、合法利益等 |
| **知情权** | 在收集数据前告知数据主体处理目的和方式 |
| **访问权** | 数据主体有权请求访问其个人数据 |
| **更正权** | 数据主体有权要求更正不准确的数据 |
| **删除权** | 数据主体有权要求删除其数据 |
| **撤回同意权** | 数据主体可随时撤回同意 |
| **数据可携性** | 数据主体有权以常用格式获取其数据 |
| **反对权** | 数据主体有权反对特定的数据处理 |
| **数据保护官 (DPO)** | 特定组织需指定数据保护官 |

### 6.3 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| 行政罚款 | 最高 **5,000,000 泰铢**（约 $140,000 USD） |
| 刑事处罚 | 最高 **1年监禁** 和/或 **1,000,000 泰铢** 罚款 |
| 民事赔偿 | 实际损失 + 惩罚性赔偿（最高实际损失的2倍） |

### 6.4 跨境数据传输

- 数据传输至境外需确保接收国具有充分的数据保护水平
- 或取得数据主体的明确同意
- 或符合其他法定例外情形

---

## 七、运营商与号码格式

### 7.1 主要运营商

| 运营商 | 母公司 | 市场份额（约） | 备注 |
|--------|--------|-------------|------|
| **AIS** (Advanced Info Service) | CP Group (Charoen Pokphand) | ~45% | 最大运营商 |
| **True Corp** (含 DTAC) | CP Group + Telenor | ~35% | 2023年 True 与 DTAC 合并 |
| **NT** (National Telecom) | 泰国政府 | ~5% | 国有运营商 |
| 其他 MVNO | 各异 | ~15% | Penguin、FINN 等 |

> **注意：** 2023年 True Corporation 与 DTAC 正式合并，泰国移动市场从三大运营商变为两大主导运营商（AIS 和 True Corp），竞争格局发生重大变化。

### 7.2 号码格式

#### 基本结构
- 国家代码：+66
- 手机号码：以 **06/08/09** 开头（国内格式）
- 总长度：9位数字（不含国家代码）

#### 格式示例

| 格式 | 示例 | 说明 |
|------|------|------|
| 国际格式（AWS 使用） | `+66812345678` | 去掉前导0 |
| 国内格式 | `081-234-5678` | 以0开头 |

#### 常见号码前缀

| 前缀 | 运营商 |
|------|--------|
| 08-1, 08-5, 08-6, 08-9 | AIS |
| 06-1, 06-2, 08-0, 08-8, 09-5, 09-6 | True Corp (含原 DTAC) |
| 09-1, 09-2, 09-3, 09-4 | True Corp |
| 08-2, 08-3 | NT |

#### 正确的 AWS SMS 号码格式
```
+66812345678      ← 手机号码（去掉前导0）
+66912345678      ← 手机号码
```

> **SMS 号码必须去掉前导 0！** `+660812345678` 是错误格式，正确格式是 `+66812345678`。

### 7.3 运营商技术细节

- Sender ID 需注册后才能使用
- 运营商有垃圾信息过滤机制
- 支持号码携带（携号转网）
- 固定电话不可达（固话以 02 开头）
- 支持消息拼接
- 支持促销和营销类消息（泰国特有优势）

---

## 八、消息模板范例（泰语/英语）

### 8.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本拉丁字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | **含泰语字符时自动触发** |

**泰语编码重要说明：**

| 场景 | 编码 | 单条上限 |
|------|------|---------|
| 纯英语消息 | GSM-7 | 160字符 |
| 含泰语字符 | UCS-2 | **70字符** |
| 泰英混合 | UCS-2 | **70字符** |

> **重要：** 泰语字符（ก-ฮ、元音、声调符号等）不在 GSM-7 字符集中，任何包含泰语的消息将自动使用 UCS-2 编码，每条上限仅70字符。建议精简泰语消息内容，控制在70字符以内以避免拼接。

### 8.2 OTP 验证码模板

**泰语版（UCS-2，70字符/条）：**
```
[Brand] รหัสยืนยัน: 385291 ใช้ได้ 5 นาที อย่าแชร์รหัสนี้
```
(约50字符，1条消息)

**英语版（GSM-7，160字符/条）：**
```
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.
```
(84字符，1条消息)

### 8.3 交易通知模板

**订单确认（泰语）：**
```
[Brand] คำสั่งซื้อ #TH12345 ยืนยันแล้ว ยอด: 4,500 บาท
ดู: https://brand.com/o/TH12345
```
(约65字符，1条)

**订单确认（英语）：**
```
[Brand] Order #TH12345 confirmed. Total: THB 4,500.
Details: https://brand.com/o/TH12345
```
(87字符，1条)

**配送通知 - 已发货（泰语）：**
```
[Brand] พัสดุ #TH12345 จัดส่งแล้ว
ติดตาม: https://brand.com/track/TH12345
```
(约55字符，1条)

**配送通知 - 已签收（泰语）：**
```
[Brand] พัสดุ #TH12345 ส่งถึงแล้ว
ให้คะแนน: https://brand.com/review/TH12345
```
(约58字符，1条)

**付款确认（泰语）：**
```
[Brand] ชำระเงินแล้ว: 4,500 บาท (#TH12345)
ใบเสร็จ: https://brand.com/receipt/TH12345
```
(约60字符，1条)

**账户安全警报（泰语）：**
```
[Brand] แจ้งเตือน: พบการเข้าสู่ระบบจากอุปกรณ์ใหม่
หากไม่ใช่คุณ: https://brand.com/security
```
(约62字符，1条)

### 8.4 营销消息模板（需 opt-in 同意）

**促销活动（泰语）：**
```
[Brand] ลด 20%! ใช้โค้ด SAVE20 ถึง 15 เม.ย.
ช้อป: https://brand.com/sale
ยกเลิก: ตอบ STOP
```
(约62字符，1条)

**新品通知（泰语）：**
```
[Brand] คอลเลกชั่นใหม่มาแล้ว!
ดู: https://brand.com/new
ยกเลิก: ตอบ STOP
```
(约52字符，1条)

**促销活动（英语）：**
```
[Brand] 20% off your next order! Use code SAVE20. Valid until 15 Apr.
Shop: https://brand.com/sale Reply STOP to opt out.
```
(119字符，1条)

### 8.5 Opt-in 确认模板

**泰语版：**
```
[Brand] ยินดีต้อนรับ! คุณจะได้รับแจ้งเตือนบัญชีและโปรโมชั่น
ยกเลิก: ตอบ STOP
```
(约55字符，1条)

**英语版：**
```
[Brand] Welcome! You'll receive account alerts, notifications, and
promotional offers. Reply STOP to unsubscribe.
```
(106字符，1条)

### 8.6 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 泰语或英语均可（视目标受众） |
| 泰语编码 | **必定触发 UCS-2**，单条仅70字符 |
| 英语编码 | GSM-7，单条160字符 |
| 泰语消息精简 | 控制在70字符内避免拼接，降低成本 |
| 退订指引 | 每条营销消息包含 "ตอบ STOP" 或 "Reply STOP" |
| 促销消息 | **支持**（泰国特有优势） |
| 货币符号 | 使用 `บาท` 或 `THB`（`$` 在 GSM-7 中兼容） |
| 避免王室相关内容 | **严格禁止** |

---

## 九、定价信息

### 9.1 AWS 定价

| 费用项 | 金额 |
|--------|------|
| 长码月租 | 需确认（通常 $2-$27 USD/月） |
| Sender ID 月费 | 通常免费（需确认） |
| 每条消息费 | 需下载 AWS 定价 CSV |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

**获取完整定价：**
1. 下载 AWS 定价 CSV：`https://aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

### 9.2 成本优化建议

1. **英语优先策略** — 如目标用户能接受英语，优先使用英语发送（GSM-7，160字符）
2. **精简泰语消息** — 泰语消息控制在70字符内，避免拼接
3. **缩短 URL** — 使用品牌自有短域名（如 brand.co/xxx）减少字符数
4. **使用 Sender ID** — 比长码更灵活，品牌辨识度更高
5. **维护号码列表** — 定期清理无效号码
6. **分离消息类型** — OTP 用英语（更便宜），营销用泰语（更有效）

---

## 十、合规清单

在泰国发送 SMS 前，确保完成：

- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 完成 Sender ID 注册（提交签署并盖章的 LOA）
- [ ] 遵守 NBTC 电信法规
- [ ] 遵守 PDPA 个人数据保护法
- [ ] 消息内容使用泰语或英语
- [ ] **绝对不含任何冒犯王室的内容**
- [ ] 每条营销消息包含退订方式（Reply STOP）
- [ ] 退订机制功能正常
- [ ] 发送时间限制在 08:00-21:00 ICT（UTC+7）
- [ ] 正确格式化号码（+66 + 号码，**去掉前导 0**）
- [ ] 泰语消息控制在70字符内（UCS-2 编码限制）
- [ ] 保留同意和退出记录
- [ ] 不在事务性消息中混入营销内容
- [ ] 申请提高默认 $1.00 USD/月消费阈值
- [ ] 指定数据保护官（如适用 PDPA 要求）

---

## 十一、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| LOA 缺少公司印章被拒 | 高 | 确保 LOA 包含授权签署人签名和公司印章 |
| 泰语消息超70字符导致拼接 | 中 | 精简泰语内容，控制在70字符内 |
| PDPA 违规罚款 | 高 | 严格遵守 PDPA，获取明确同意 |
| 冒犯王室内容 | **极高** | 严格审核所有消息内容，绝对避免涉及王室 |
| 运营商内容过滤 | 中 | 避免可疑内容，使用标准模板，使用品牌域名 |
| 号码格式错误（前导0） | 中 | 实施严格的号码格式验证（去掉前导0） |
| True-DTAC 合并后市场变化 | 低 | 关注运营商政策变化，确保覆盖 AIS 和 True Corp |
| 赌博内容被严厉处罚 | 高 | 绝对不发送赌博相关内容 |
| 不支持短码导致吞吐量受限 | 低 | 使用多个长码或 Sender ID 分散发送 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。泰国法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询泰国当地法律顾问以确保合规。
