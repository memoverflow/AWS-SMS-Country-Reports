# 阿联酋 (AE, +971) AWS SMS 发送专题研究报告

> 调研日期：2026年4月7日
> 数据来源：AWS 官方文档、TRA/TDRA 官网、Etisalat/du 运营商资料

---

## 一、AWS 上的阿联酋 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +971 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | 支持（**仅入站/接收**，不可用于发送） |
| Sender ID | **支持（必须注册）** |
| Toll-free | 不支持 |
| 双向 SMS | **不支持** |
| 消息拼接 | 支持 |
| MMS | 不支持 |
| 国际发送 | **不支持（仅本地 +971 号码）** |

**核心限制：** 阿联酋是 AWS SMS 中注册材料要求最繁琐的国家之一。需分别获取 Etisalat 和 du 两家运营商的授权书（LOA），国际公司和本地公司的要求不同。仅支持事务性消息，严禁促销内容。不支持国际发送，只能向阿联酋本地 +971 号码发送。

---

## 二、费用总览

### 2.1 号码费用

| 费用项目 | 一次性费用 | 月度费用 | 备注 |
|---------|-----------|---------|------|
| Sender ID | $0 | $0 | 需双运营商（Etisalat + du）LOA 注册 |

### 2.2 每条消息费用

| 号码类型 | 基础价格/条 | 备注 |
|---------|-----------|------|
| Sender ID | $0.10809 | 每条消息段（message segment）计费 |

### 2.3 月度消费阈值

新账户默认 $1.00 USD/月，需申请提高。

---

## 三、Sender ID 注册申请详细流程

### 3.1 申请方式

阿联酋 Sender ID 注册需通过 **AWS End User Messaging SMS 控制台** 提交，但需要预先准备大量材料。注册材料因公司类型（国际公司 vs 阿联酋本地公司）而异。

### 3.2 国际公司所需材料

| 序号 | 材料 | 说明 |
|------|------|------|
| 1 | **Etisalat 国际授权书（LOA/NOC）** | 使用 AWS 提供的 Etisalat LOA 模板，须授权签署人签名 |
| 2 | **du 国际授权书（LOA/NOC）** | 使用 AWS 提供的 du LOA 模板，须授权签署人签名 |
| 3 | **公司注册证书** | 公司所在国的营业执照/注册证书（英文或阿拉伯文） |
| 4 | **Sender ID 关联证明** | 商标注册证书或知识产权证明（证明 Sender ID 与公司的关联） |

### 3.3 阿联酋本地公司所需材料（额外要求）

除上述国际公司材料外，本地公司还需：

| 序号 | 材料 | 说明 |
|------|------|------|
| 5 | **du Sender ID 注册表** | du 运营商专用的 Sender ID 注册表格 |
| 6 | **机构卡（Establishment Card）** | 阿联酋商业机构登记证 |
| 7 | **授权签署人身份证** | 阿联酋身份证（Emirates ID）正反面复印件 |
| 8 | **授权签署人护照** | 护照信息页复印件 |
| 9 | **授权委托书（POA）** | 如签署人非公司法定代表人，需提供授权委托书 |
| 10 | **卫生部授权** | 仅医疗机构需要，需获得卫生部（MOH）特别授权 |

### 3.4 Support Case 填写指南（配合 Sender ID 注册使用）

如需同时申请提高消费限额或其他配置，通过 **AWS Support Case** 提交。

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
| Region | 你使用的 AWS 区域（如 eu-west-1） |
| Resource Type | Sender ID Registration |
| Limit Type | AWS End User Messaging SMS (Pinpoint) |
| Limit Increase Value | **1** |

### 3.5 Case Description 模板

```
Subject: Sender ID Registration Request - United Arab Emirates (AE, +971)

We are requesting a Sender ID registration for sending transactional
SMS to the United Arab Emirates (+971).

Company Information:
- Company Name: [公司法定名称]
- Company Registration Number: [公司注册号]
- Company Address: [公司地址]
- Country of Incorporation: [注册国家]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Requested Sender ID: [品牌名称，3-11个字母数字字符]

Company Type: International Company / UAE Local Company (选一)

Message Type: Transactional ONLY (OTP / Notifications)

Target Country: United Arab Emirates (+971)
Target Carriers: Etisalat (e&), du

Expected Monthly Volume: [预估月发送量，如 50,000 messages]

Attached Documents:
1. Etisalat International LOA (signed)
2. du International LOA (signed)
3. Certificate of Incorporation
4. Sender ID Association Proof (Trademark Certificate)
[本地公司补充：]
5. du Sender ID Registration Form
6. Establishment Card
7. Authorized Signatory Emirates ID (front & back)
8. Authorized Signatory Passport Copy

Sample Messages:

Template 1 - OTP (English):
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.

Template 2 - OTP (Arabic):
[Brand] رمز التحقق الخاص بك هو: 385291. صالح لمدة 5 دقائق.
لا تشارك هذا الرمز مع أي شخص.

Template 3 - Transaction Notification (English):
[Brand] Your order #AE12345 has been confirmed. Amount: AED 500.
Track: https://brand.com/track/AE12345

We confirm that:
1. All messages are transactional only (NO promotional content)
2. We have obtained user consent
3. We will comply with UAE TRA/TDRA regulations

Please advise on the registration timeline and any additional
requirements.
```

### 3.6 申请时间线

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| 材料初审 | 1-3个工作日 |
| 材料补充（如有） | 取决于你 |
| 运营商审核（Etisalat + du） | 4-8周 |
| Sender ID 配置完成 | 1-2周 |
| **总计** | **6-12周** |

### 3.7 费用说明

| 费用项 | 说明 |
|--------|------|
| Sender ID 注册费 | 通常免费（AWS 不收取注册费） |
| 每条消息费 | 需下载 AWS 定价 CSV 或联系销售 |
| 长码月租（入站用） | 需确认，用于接收退订回复 |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

---

## 四、阿联酋电信监管

### 4.1 监管机构

**TDRA**（Telecommunications and Digital Government Regulatory Authority，电信和数字政府监管局），前身为 TRA，是阿联酋电信行业的监管机构。

### 4.2 核心法规

| 法规 | 主要内容 |
|------|---------|
| 联邦法令第 3/2003 号（电信法） | 电信行业基本监管框架 |
| TDRA 消费者保护条例 | 用户权益保护，包括反垃圾短信 |
| 联邦法令第 34/2021 号（打击谣言和网络犯罪法） | 规范电子通信内容 |
| TDRA 关于 Sender ID 的指令 | 要求所有 A2P 短信使用已注册的 Sender ID |

### 4.3 A2P SMS 核心监管要求

- **Sender ID 强制注册**：所有 A2P SMS 必须使用在运营商注册的 Sender ID
- **仅事务性消息**：禁止通过 A2P SMS 发送促销/营销内容
- **双运营商覆盖**：必须同时在 Etisalat 和 du 注册
- **内容审核**：运营商对消息内容有过滤机制
- **本地发送限制**：不支持从国际号码发送至阿联酋

### 4.4 禁止/限制内容

| 类别 | 限制程度 |
|------|---------|
| 促销/营销消息 | **禁止** |
| 未经同意的消息 | **禁止** |
| 赌博/博彩 | **严禁**（阿联酋法律全面禁止赌博） |
| 色情内容 | **严禁** |
| 酒类广告 | **严禁** |
| 政治敏感内容 | **严禁** |
| 宗教不敬内容 | **严禁** |
| 金融诈骗/钓鱼 | **严禁** |
| 未授权的品牌使用 | 禁止 |

### 4.5 发送时间建议

- **事务性消息（OTP、通知）**：24小时均可发送
- **建议避免深夜发送非紧急通知**：22:00-07:00 GST（UTC+4）
- 斋月期间需特别注意发送时间和内容敏感性

---

## 五、阿联酋数据保护法

### 5.1 法律概述

**联邦法令第 45/2021 号**（个人数据保护法，PDPL）是阿联酋首部综合性数据保护法律，于2022年生效，与欧盟 GDPR 有较多相似之处。

### 5.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **用户同意** | 必须获得明确同意才能处理个人数据（含手机号码） |
| **目的限制** | 个人数据仅可用于收集时声明的目的 |
| **数据最小化** | 仅收集必要的个人数据 |
| **数据主体权利** | 用户有权访问、更正、删除其个人数据 |
| **数据泄露通知** | 发生数据泄露时必须通知监管机构和受影响个人 |
| **跨境传输** | 向境外传输个人数据需满足特定条件（充足性认定或合同保障） |
| **数据保护官** | 特定情况下需指定数据保护官（DPO） |

### 5.3 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| 行政罚款 | 最高 **2,000 万迪拉姆**（约 545 万美元） |
| 刑事处罚 | 可能面临监禁（严重违规） |
| 业务限制 | 可能被要求停止数据处理活动 |

### 5.4 自由区特殊规定

阿联酋各自由区（如 DIFC、ADGM）有各自独立的数据保护法规：
- **DIFC**：适用 DIFC 数据保护法（2020年）
- **ADGM**：适用 ADGM 数据保护条例（2021年）
- 在自由区运营的公司需同时遵守联邦法和自由区法规

---

## 六、运营商与号码格式

### 6.1 两大运营商

| 运营商 | 母公司 | 市场份额（约） | 备注 |
|--------|--------|-------------|------|
| **Etisalat (e&)** | Emirates Telecommunications Group | ~55% | 阿联酋最大运营商 |
| **du** | Emirates Integrated Telecommunications (EITC) | ~45% | 第二大运营商 |

> **重要：** 必须同时获得 Etisalat 和 du 两家运营商的 LOA，缺少任一家将导致部分用户无法接收消息。

### 6.2 号码格式

#### 基本结构
- 国家代码：+971
- 手机号码长度：9位（含区号）
- 格式：+971 5X XXX XXXX

#### 运营商号段

| 运营商 | 号段 | 示例 |
|--------|------|------|
| Etisalat | 050, 052, 055, 056 | +971 50 XXX XXXX |
| du | 054, 055, 058 | +971 58 XXX XXXX |

> **注意：** 部分号段（如 055）在两家运营商间共享，因号码携带制度而存在。

#### AWS SMS 正确格式
```
+97150XXXXXXX      ← Etisalat 示例
+97158XXXXXXX      ← du 示例
+97152XXXXXXX      ← Etisalat 示例
+97154XXXXXXX      ← du 示例
```

> **重要：** 使用 E.164 格式，`+971` 后跟 9 位号码。国内拨打时去掉 `+971` 加 `0`（如 050-XXX-XXXX），但发送 SMS 时不使用前导 `0`。

### 6.3 运营商技术细节

- 两家运营商均需独立注册 Sender ID
- 运营商有内容过滤系统，促销内容会被拦截
- 长码仅支持入站（接收），可用于接收用户退订回复
- Sender ID 不支持 Unicode 字符，仅限字母数字
- 运营商可能要求定期更新 LOA

---

## 七、不支持双向 SMS 的替代退出方案

### 7.1 问题

AWS 上阿联酋不支持双向 SMS（虽然长码支持入站，但无法用于标准 STOP 机制）。必须提供替代 opt-out 机制。

### 7.2 替代方案

#### 方案一：网页链接退出（推荐）

每条 SMS 中包含唯一退出链接（注意字符限制）：

**英语版：**
```
[Brand] Unsubscribe: https://brand.com/unsub/{TOKEN}
```

**阿拉伯语版：**
```
[Brand] لإلغاء الاشتراك: https://brand.com/unsub/{TOKEN}
```

#### 方案二：App 内设置

在 App 用户设置中提供通知偏好管理。

#### 方案三：客服渠道

```
To unsubscribe, email: optout@brand.com
```

---

## 八、消息模板范例（英语/阿拉伯语）

### 8.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 纯英文/基本字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含阿拉伯语字符 |

> **建议：** 阿联酋为多语言环境（阿拉伯语和英语并用）。英语消息使用 GSM-7（160字符），阿拉伯语消息使用 Unicode（70字符）。根据用户语言偏好选择，优先英语可降低成本。

### 8.2 OTP 验证码模板

**英语版（GSM-7，160字符/条）：**
```
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.
```
（88字符，1条消息）

**阿拉伯语版（Unicode，70字符/条）：**
```
[Brand] رمز التحقق الخاص بك هو: 385291. صالح لمدة 5 دقائق.
```
（约55字符，1条消息）

### 8.3 交易通知模板

**订单确认 - 英语（GSM-7）：**
```
[Brand] Your order #AE12345 has been confirmed. Amount: AED 500.
Delivery: Apr 10. Track: https://brand.com/track/AE12345
```
（114字符，1条消息）

**订单确认 - 阿拉伯语（Unicode）：**
```
[Brand] تم تأكيد طلبك #AE12345. المبلغ: 500 د.إ. التتبع: brand.com/t/AE12345
```
（约65字符，1条消息）

**发货通知 - 英语（GSM-7）：**
```
[Brand] Your order #AE12345 has been shipped.
Track: https://brand.com/track/AE12345
```
（82字符，1条消息）

**付款确认 - 英语（GSM-7）：**
```
[Brand] Payment received: AED 500 (order #AE12345).
Receipt: https://brand.com/receipt/AE12345
```
（92字符，1条消息）

**安全告警 - 英语（GSM-7）：**
```
[Brand] Alert: New device login detected on your account.
If this wasn't you: https://brand.com/security
```
（100字符，1条消息）

### 8.4 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 英语和阿拉伯语并用 |
| 编码 | 英语用 GSM-7（160字符），阿拉伯语用 Unicode（70字符） |
| 消息类型 | **仅限事务性消息，严禁促销内容** |
| 退出指引 | 建议每条通知消息包含退出方式 |
| 货币符号 | 使用 `AED` 或 `د.إ.` |
| URL | 使用品牌自有域名 |
| Sender ID | 3-11个字母数字字符，仅限拉丁字符 |

---

## 九、定价信息

### 9.1 AWS 定价

AWS 未在公开文档直接列出阿联酋每条费率，需通过：
1. 下载 AWS 定价 CSV：`aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

**已知费用结构：**
- 每条消息费：需参考 AWS 定价 CSV
- 默认消费阈值：$1.00 USD/月（需申请提高）
- Sender ID 注册通常免费

### 9.2 成本优化建议

1. **优先使用英语消息** — GSM-7 编码每条 160 字符，阿拉伯语 Unicode 仅 70 字符
2. **精简消息内容** — 保持单条消息在编码上限内避免拼接
3. **维护号码列表** — 定期清理无效号码
4. **按用户语言偏好发送** — 避免不必要的双语发送
5. **使用短 URL** — 品牌短域名减少字符占用

---

## 十、合规清单

在阿联酋发送 SMS 前，确保完成：

- [ ] Sender ID 已在 AWS 控制台注册并获批
- [ ] Etisalat LOA 已签署并提交
- [ ] du LOA 已签署并提交
- [ ] 公司注册证书已提交
- [ ] Sender ID 关联证明已提交
- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 实施有效的 opt-out 机制
- [ ] 所有消息均为事务性内容（无促销/营销）
- [ ] 遵守 PDPL 数据保护要求
- [ ] 正确格式化号码（+971 + 9位号码）
- [ ] 英语消息使用 GSM-7 编码
- [ ] 阿拉伯语消息控制在 70 字符以内
- [ ] 不向国际号码发送（仅支持 +971 本地号码）
- [ ] 消息内容不涉及禁止内容（赌博、酒类、色情等）
- [ ] 斋月期间注意发送时间和内容敏感性
- [ ] 保留同意和退出记录
- [ ] 自由区公司同时遵守联邦法和自由区法规
- [ ] 医疗机构已获得卫生部授权（如适用）

---

## 十一、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| 注册材料繁琐导致延期 | **高** | 提前准备所有材料，尤其是双运营商 LOA |
| 材料审核被拒 | 高 | 确保 LOA 签名有效，Sender ID 关联证明充分 |
| 本地公司额外材料门槛 | 高 | 预留更多时间准备机构卡、身份证等 |
| 促销内容被拦截 | **高** | 严格区分事务性和营销内容，杜绝混用 |
| 不支持国际发送 | 中 | 确认目标号码均为 +971，考虑本地部署方案 |
| 运营商覆盖不全 | 中 | 确保同时获得 Etisalat 和 du 两家批准 |
| 阿拉伯语 Unicode 成本 | 中 | 优先英语，仅在必要时使用阿拉伯语 |
| 数据保护违规（PDPL） | 中 | 严格遵守 PDPL，注意跨境数据传输要求 |
| LOA 过期需更新 | 低 | 建立 LOA 到期提醒机制，及时续签 |
| 斋月期间投递率波动 | 低 | 提前测试，调整发送策略 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。阿联酋法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询阿联酋当地法律顾问以确保合规。
