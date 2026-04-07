# 沙特阿拉伯 (SA, +966) AWS SMS 发送专题研究报告

> 调研日期：2026年4月7日
> 数据来源：AWS 官方文档、CITC 官网、沙特运营商资料

---

## 一、AWS 上的沙特阿拉伯 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +966 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | 支持（**仅入站/接收**，不可用于发送） |
| Sender ID | **支持（必须注册）** |
| Toll-free | 不支持 |
| 双向 SMS | **不支持** |
| 消息拼接 | 支持 |
| MMS | 不支持 |
| 国际发送 | **不支持（仅本地 +966 号码）** |

**核心限制：** 沙特阿拉伯在 AWS 上仅支持国际公司注册 Sender ID，本地沙特公司目前不支持。仅允许事务性消息，禁止促销内容。不支持双向 SMS，不支持国际发送，且需要 Vodafone LOA。

---

## 二、Sender ID 注册申请详细流程

### 2.1 申请方式

沙特 Sender ID 注册通过 **AWS End User Messaging SMS 控制台** 提交。

### 2.2 重要前提条件

**仅支持国际公司注册，沙特本地公司目前不支持。** 如果公司注册地在沙特阿拉伯境内，目前无法通过 AWS 注册 Sender ID 向沙特号码发送短信。

### 2.3 所需材料

| 序号 | 材料 | 说明 |
|------|------|------|
| 1 | **Vodafone 授权书（LOA/NOC）** | 使用 AWS 提供的 Vodafone LOA 模板，须授权签署人签名 |
| 2 | **公司注册证书** | 公司所在国的营业执照/注册证书（英文） |
| 3 | **Sender ID 关联证明** | 商标注册证书或知识产权证明（证明 Sender ID 与公司的关联） |

### 2.4 Support Case 填写指南

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

### 2.5 Case Description 模板

```
Subject: Sender ID Registration Request - Saudi Arabia (SA, +966)

We are requesting a Sender ID registration for sending transactional
SMS to Saudi Arabia (+966).

Company Information:
- Company Name: [公司法定名称]
- Company Registration Number: [公司注册号]
- Country of Incorporation: [注册国家 - 必须为沙特以外的国家]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Requested Sender ID: [品牌名称，3-11个字母数字字符]

Company Type: International Company (NON-Saudi registered)

Message Type: Transactional ONLY (OTP / Notifications)

Target Country: Saudi Arabia (+966)

Expected Monthly Volume: [预估月发送量，如 30,000 messages]

Attached Documents:
1. Vodafone LOA/NOC (signed)
2. Certificate of Incorporation
3. Sender ID Association Proof (Trademark Certificate)

Sample Messages:

Template 1 - OTP (English):
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.

Template 2 - OTP (Arabic):
[Brand] رمز التحقق الخاص بك هو: 385291. صالح لمدة 5 دقائق.
لا تشارك هذا الرمز مع أي شخص.

Template 3 - Transaction Notification (English):
[Brand] Your order #SA12345 has been confirmed. Amount: SAR 200.
Track: https://brand.com/track/SA12345

We confirm that:
1. Our company is registered outside of Saudi Arabia
2. All messages are transactional only (NO promotional content)
3. We have obtained user consent
4. We will comply with Saudi CITC regulations

Please advise on the registration timeline and any additional
requirements.
```

### 2.6 申请时间线

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| 材料初审 | 1-3个工作日 |
| 材料补充（如有） | 取决于你 |
| 运营商审核 | 4-8周 |
| Sender ID 配置完成 | 1-2周 |
| **总计** | **6-12周** |

### 2.7 费用说明

| 费用项 | 说明 |
|--------|------|
| Sender ID 注册费 | 通常免费（AWS 不收取注册费） |
| 每条消息费 | 需下载 AWS 定价 CSV 或联系销售 |
| 长码月租（入站用） | 需确认，用于接收用户回复 |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

---

## 三、沙特阿拉伯电信监管

### 3.1 监管机构

**CITC**（Communications, Space, and Technology Commission，通信、空间与技术委员会），前身为 CITC（Communications and Information Technology Commission），是沙特阿拉伯电信行业的监管机构。

### 3.2 核心法规

| 法规 | 主要内容 |
|------|---------|
| 电信法（2001年） | 电信行业基本监管框架 |
| CITC 反垃圾短信条例 | 禁止未经同意的商业短信 |
| 沙特愿景2030 数字化政策 | 推动电子政务和数字服务 |
| 电子交易法 | 规范电子通信和交易 |

### 3.3 A2P SMS 核心监管要求

- **Sender ID 强制注册**：所有 A2P SMS 必须使用已注册的 Sender ID
- **仅事务性消息**：禁止通过 A2P SMS 发送促销/营销内容
- **用户同意**：必须获得接收者的事先同意
- **内容审核**：运营商对消息内容有严格的过滤机制
- **本地发送限制**：不支持从国际号码发送至沙特

### 3.4 禁止/限制内容

| 类别 | 限制程度 |
|------|---------|
| 促销/营销消息 | **禁止** |
| 未经同意的消息 | **禁止** |
| 赌博/博彩 | **严禁**（沙特法律全面禁止赌博） |
| 色情内容 | **严禁** |
| 酒类广告 | **严禁** |
| 政治敏感内容 | **严禁** |
| 宗教不敬内容 | **严禁** |
| 金融诈骗/钓鱼 | **严禁** |
| 未授权品牌使用 | 禁止 |

### 3.5 发送时间建议

- **事务性消息（OTP、通知）**：24小时均可发送
- **建议避免深夜发送非紧急通知**：22:00-07:00 AST（UTC+3）
- 斋月和朝觐期间需特别注意发送时间和内容敏感性
- 周五（伊斯兰主麻日）下午避免非紧急消息

---

## 四、沙特阿拉伯数据保护法

### 4.1 法律概述

**个人数据保护法（PDPL）** 于2021年颁布，2023年修订并正式生效，是沙特阿拉伯首部综合性数据保护法律。由沙特数据与人工智能管理局（SDAIA）负责执行。

### 4.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **用户同意** | 处理个人数据须获得数据主体的明确同意 |
| **目的限制** | 个人数据仅可用于收集时声明的合法目的 |
| **数据最小化** | 仅收集实现目的所必需的个人数据 |
| **数据准确性** | 确保个人数据准确、完整、及时更新 |
| **数据主体权利** | 用户有权知情、访问、更正、删除其个人数据 |
| **数据泄露通知** | 发生数据泄露时须在72小时内通知 SDAIA |
| **跨境传输** | 个人数据出境须满足特定条件（充足性认定或适当保障） |
| **数据保护影响评估** | 高风险处理活动需进行 DPIA |

### 4.3 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| 行政罚款 | 最高 **500 万沙特里亚尔**（约 133 万美元） |
| 数据泄露 | 最高 **300 万沙特里亚尔** |
| 严重违规 | 可能面临监禁 |
| 重复违规 | 罚款加倍 |

---

## 五、运营商与号码格式

### 5.1 主要运营商

| 运营商 | 英文名 | 市场份额（约） | 备注 |
|--------|--------|-------------|------|
| **STC** | Saudi Telecom Company | ~45% | 沙特最大运营商 |
| **Mobily** | Etihad Etisalat | ~25% | 第二大运营商 |
| **Zain** | Zain KSA | ~20% | 第三大运营商 |
| **Virgin Mobile** | - | ~10% | MVNO，使用 STC 网络 |

> **注意：** AWS 通过 Vodafone 作为中间聚合商进行发送，因此 LOA 是 Vodafone 的授权书，而非上述运营商的直接 LOA。

### 5.2 号码格式

#### 基本结构
- 国家代码：+966
- 手机号码长度：9位（含区号）
- 格式：+966 5X XXX XXXX

#### 运营商号段

| 运营商 | 号段 | 示例 |
|--------|------|------|
| STC | 050, 053, 055 | +966 50 XXX XXXX |
| Mobily | 054, 056 | +966 54 XXX XXXX |
| Zain | 058, 059 | +966 58 XXX XXXX |

#### AWS SMS 正确格式
```
+96650XXXXXXX      ← STC 示例
+96654XXXXXXX      ← Mobily 示例
+96658XXXXXXX      ← Zain 示例
+96655XXXXXXX      ← STC 示例
```

> **重要：** 使用 E.164 格式，`+966` 后跟 9 位号码。国内拨打时去掉 `+966` 加 `0`（如 050-XXX-XXXX），但发送 SMS 时不使用前导 `0`。固定电话号码（01/02/03/04/06/07 开头）不可接收短信。

### 5.3 运营商技术细节

- AWS 通过 Vodafone 聚合商覆盖所有本地运营商
- 运营商有严格的内容过滤系统
- 促销内容会被自动拦截
- 高频发送同一内容会触发限流
- 不支持双向 SMS
- 支持号码携带（携号转网）

---

## 六、不支持双向 SMS 的替代退出方案

### 6.1 问题

AWS 上沙特不支持双向 SMS，用户无法回复 STOP 退订。必须提供替代 opt-out 机制。

### 6.2 替代方案

#### 方案一：网页链接退出（推荐）

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

### 6.3 最佳实践

1. 每条通知消息建议包含退出方式
2. 使用品牌自有域名短链接
3. 退出页面提供阿拉伯语和英语双语支持
4. 退出后24小时内停止发送
5. 保留所有退出记录作为合规证据

---

## 七、消息模板范例（英语/阿拉伯语）

### 7.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 纯英文/基本字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含阿拉伯语字符 |

> **建议：** 与阿联酋类似，沙特为双语环境。英语消息使用 GSM-7（160字符），阿拉伯语消息使用 Unicode（70字符）。优先英语可降低成本。

### 7.2 OTP 验证码模板

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

### 7.3 交易通知模板

**订单确认 - 英语（GSM-7）：**
```
[Brand] Your order #SA12345 has been confirmed. Amount: SAR 200.
Delivery: Apr 10. Track: https://brand.com/track/SA12345
```
（114字符，1条消息）

**订单确认 - 阿拉伯语（Unicode）：**
```
[Brand] تم تأكيد طلبك #SA12345. المبلغ: 200 ر.س. التتبع: brand.com/t/SA12345
```
（约65字符，1条消息）

**发货通知 - 英语（GSM-7）：**
```
[Brand] Your order #SA12345 has been shipped.
Track: https://brand.com/track/SA12345
```
（82字符，1条消息）

**付款确认 - 英语（GSM-7）：**
```
[Brand] Payment received: SAR 200 (order #SA12345).
Receipt: https://brand.com/receipt/SA12345
```
（92字符，1条消息）

**安全告警 - 英语（GSM-7）：**
```
[Brand] Alert: New device login detected on your account.
If this wasn't you: https://brand.com/security
```
（100字符，1条消息）

### 7.4 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 英语和阿拉伯语并用 |
| 编码 | 英语用 GSM-7（160字符），阿拉伯语用 Unicode（70字符） |
| 消息类型 | **仅限事务性消息，严禁促销内容** |
| 退出指引 | 建议每条通知消息包含退出方式 |
| 货币符号 | 使用 `SAR` 或 `ر.س.` |
| URL | 使用品牌自有域名 |
| Sender ID | 3-11个字母数字字符，仅限拉丁字符 |

---

## 八、定价信息

### 8.1 AWS 定价

AWS 未在公开文档直接列出沙特每条费率，需通过：
1. 下载 AWS 定价 CSV：`aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

**已知费用结构：**
- 每条消息费：需参考 AWS 定价 CSV
- 默认消费阈值：$1.00 USD/月（需申请提高）
- Sender ID 注册通常免费

### 8.2 成本优化建议

1. **优先使用英语消息** — GSM-7 编码每条 160 字符，阿拉伯语 Unicode 仅 70 字符
2. **精简消息内容** — 保持单条消息在编码上限内避免拼接
3. **维护号码列表** — 定期清理无效号码
4. **按用户语言偏好发送** — 避免不必要的双语发送
5. **使用短 URL** — 品牌短域名减少字符占用

---

## 九、合规清单

在沙特阿拉伯发送 SMS 前，确保完成：

- [ ] 确认公司注册地在沙特以外（本地公司不支持）
- [ ] Sender ID 已在 AWS 控制台注册并获批
- [ ] Vodafone LOA 已签署并提交
- [ ] 公司注册证书已提交
- [ ] Sender ID 关联证明已提交
- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 实施有效的 opt-out 机制（网页链接/App 内设置，因不支持双向 SMS）
- [ ] 所有消息均为事务性内容（无促销/营销）
- [ ] 遵守沙特 PDPL 数据保护要求
- [ ] 正确格式化号码（+966 + 9位号码，以 5 开头）
- [ ] 英语消息使用 GSM-7 编码
- [ ] 阿拉伯语消息控制在 70 字符以内
- [ ] 不向国际号码发送（仅支持 +966 本地号码）
- [ ] 消息内容不涉及禁止内容（赌博、酒类、色情等）
- [ ] 斋月和朝觐期间注意发送时间和内容敏感性
- [ ] 保留同意和退出记录
- [ ] 跨境数据传输已满足 PDPL 要求
- [ ] 数据泄露应急预案已就位

---

## 十、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| 本地公司无法注册 | **极高** | 仅国际公司可注册，本地公司需寻找替代方案或通过境外实体注册 |
| 注册审核被拒 | 高 | 确保 Vodafone LOA 签名有效，Sender ID 关联证明充分 |
| 促销内容被拦截 | **高** | 严格区分事务性和营销内容，杜绝混用 |
| 不支持国际发送 | 中 | 确认目标号码均为 +966，考虑本地合作方案 |
| 不支持双向 SMS | 中 | 提供网页/App 内退出机制，长码可用于入站 |
| 运营商内容过滤 | 中 | 使用标准模板，避免可疑内容 |
| 阿拉伯语 Unicode 成本 | 中 | 优先英语，仅在必要时使用阿拉伯语 |
| 数据保护违规（PDPL） | 中 | 严格遵守 PDPL，注意跨境数据传输要求 |
| Vodafone 聚合商故障 | 低 | 准备备用通信渠道（WhatsApp/邮件） |
| 宗教节日投递率波动 | 低 | 提前测试，调整发送策略 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。沙特阿拉伯法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询沙特当地法律顾问以确保合规。
