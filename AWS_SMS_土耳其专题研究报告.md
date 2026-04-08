# 土耳其 (TR, +90) AWS SMS 发送专题研究报告

> 调研日期：2026年4月7日
> 数据来源：AWS 官方文档、BTK/ICTA 官网、土耳其运营商资料

---

## 一、AWS 上的土耳其 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +90 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **不支持** |
| Sender ID | **支持（必须注册）** |
| Toll-free | 不支持 |
| 双向 SMS | **不支持** |
| 消息拼接 | 支持 |
| MMS | 不支持 |
| 国际发送 | 支持 |

**核心限制：** 土耳其仅支持通过已注册的 Sender ID 发送 SMS，不支持短码和长码。注册需要提交两份 LOA（Sender ID LOA + Vendor LOA），且自2026年4月1日起实施严格的 URL 新规：禁止使用短链接和通用 URL，仅允许品牌自有域名。不支持双向 SMS，仅允许事务性消息。

---

## 二、费用总览

### 2.1 号码费用

| 费用项目 | 一次性费用 | 月度费用 | 备注 |
|---------|-----------|---------|------|
| Sender ID | $0 | $0 | 需双 LOA 注册（Sender ID LOA + Vendor LOA） |

### 2.2 每条消息费用

| 号码类型 | 基础价格/条 | 备注 |
|---------|-----------|------|
| Sender ID | $0.03 | 每条消息段（message segment）计费 |

### 2.3 月度消费阈值

新账户默认 $1.00 USD/月，需申请提高。

---

## 三、Sender ID 注册申请详细流程

### 3.1 申请方式

土耳其 Sender ID 注册通过 **AWS End User Messaging SMS 控制台** 提交。土耳其是少数需要提交**两份 LOA**的国家之一。

### 3.2 所需材料（两份 LOA）

| 序号 | 材料 | 说明 |
|------|------|------|
| 1 | **Sender ID 授权书（LOA）** | 下载模板：`Turkey_SenderId_LetterOfAuthorization.zip`，须授权签署人签名 |
| 2 | **供应商授权书（Vendor LOA）** | 下载模板：`Turkey_SenderId_VendorLetterOfAuthorization.zip`，须授权签署人签名 |
| 3 | **公司注册证书** | 公司所在国的营业执照/注册证书（英文或土耳其文） |
| 4 | **Sender ID 关联证明** | 商标注册证书或知识产权证明（证明 Sender ID 与公司的关联） |

### 3.3 两份 LOA 的区别

| LOA 类型 | 用途 | 签署方 |
|---------|------|--------|
| **Sender ID LOA** | 授权 AWS 代表你的公司使用指定的 Sender ID 发送 SMS | 你的公司授权签署人 |
| **Vendor LOA** | 授权 AWS 的上游供应商（carrier aggregator）代表你发送 SMS | 你的公司授权签署人 |

> **重要：** 两份 LOA 缺一不可。缺少任何一份都会导致注册失败。

### 3.4 Support Case 填写指南

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
Subject: Sender ID Registration Request - Turkey (TR, +90)

We are requesting a Sender ID registration for sending transactional
SMS to Turkey (+90).

Company Information:
- Company Name: [公司法定名称]
- Company Registration Number: [公司注册号]
- Country of Incorporation: [注册国家]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站 - 必须为品牌自有域名]

Requested Sender ID: [品牌名称，3-11个字母数字字符]

Message Type: Transactional ONLY (OTP / Notifications)

Target Country: Turkey (+90)

Expected Monthly Volume: [预估月发送量，如 50,000 messages]

Attached Documents:
1. Sender ID LOA (Turkey_SenderId_LetterOfAuthorization - signed)
2. Vendor LOA (Turkey_SenderId_VendorLetterOfAuthorization - signed)
3. Certificate of Incorporation
4. Sender ID Association Proof (Trademark Certificate)

Sample Messages:

Template 1 - OTP (Turkish):
[Brand] Dogrulama kodunuz: 385291. 5 dakika gecerlidir.
Bu kodu kimseyle paylasmayin.

Template 2 - OTP (English):
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.

Template 3 - Transaction Notification (Turkish):
[Brand] Siparisiz #TR12345 onaylandi. Tutar: 500 TL.
Takip: https://brand.com/track/TR12345

URL Compliance Note:
All URLs in our messages use our brand-owned domain (brand.com).
We do not use URL shorteners (bit.ly, tinyurl, etc.) or generic URLs.

We confirm that:
1. All messages are transactional only (NO promotional content)
2. We have obtained user consent
3. All URLs are brand-owned domains (no short links or generic URLs)
4. We will comply with Turkish BTK/ICTA regulations

Please advise on the registration timeline and any additional
requirements.
```

### 3.6 申请时间线

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| 材料初审 | 1-3个工作日 |
| 材料补充（如有） | 取决于你 |
| 运营商审核 | 4-8周 |
| Sender ID 配置完成 | 1-2周 |
| **总计** | **6-12周** |

### 3.7 费用说明

| 费用项 | 说明 |
|--------|------|
| Sender ID 注册费 | 通常免费（AWS 不收取注册费） |
| 每条消息费 | 需下载 AWS 定价 CSV 或联系销售 |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

---

## 四、URL 新规（2026年4月1日起已生效）

### 4.1 新规要点

自2026年4月1日起，土耳其对 SMS 中的 URL 实施严格新规。这是土耳其 BTK/ICTA 为打击 SMS 欺诈和钓鱼攻击而推出的重要监管措施。

### 4.2 URL 规则详解

| 规则 | 详情 |
|------|------|
| **短链接服务** | **禁止** — bit.ly、tinyurl、goo.gl、ow.ly 等一律禁止 |
| **通用 URL** | **禁止** — 不允许使用通用域名或共享域名 |
| **品牌自有域名** | **仅允许** — 必须使用与发送品牌关联的自有域名 |
| **Sender ID 与域名关联** | 要求 URL 域名与已注册的 Sender ID 品牌一致 |

### 4.3 合规 URL 示例

| 类型 | 示例 | 是否合规 |
|------|------|---------|
| 品牌自有域名 | `https://brand.com/track/TR12345` | **合规** |
| 品牌短域名 | `https://brnd.co/TR12345` | **合规**（需证明域名归属） |
| bit.ly 短链接 | `https://bit.ly/abc123` | **违规** |
| tinyurl | `https://tinyurl.com/abc` | **违规** |
| 通用域名 | `https://example.com/page` | **违规** |
| IP 地址 URL | `http://192.168.1.1/page` | **违规** |

### 4.4 违规后果

- 包含违规 URL 的消息将被运营商**直接拦截**，不会送达
- 多次违规可能导致 Sender ID 被**暂停或撤销**
- 可能面临 BTK 的行政处罚

### 4.5 应对建议

1. **立即审查所有消息模板**，确保使用品牌自有域名
2. **注册品牌短域名**（如 `brnd.co`），用于 SMS 中的链接
3. **建立 URL 白名单机制**，确保开发团队不误用短链接服务
4. **更新 API 集成逻辑**，在发送前验证 URL 合规性

---

## 五、土耳其电信监管

### 5.1 监管机构

**BTK**（Bilgi Teknolojileri ve İletişim Kurumu，信息和通信技术管理局），也称 **ICTA**（Information and Communication Technologies Authority），是土耳其电信行业的监管机构。

### 5.2 核心法规

| 法规 | 主要内容 |
|------|---------|
| 电子通信法（第 5809 号，2008年） | 电信行业基本监管框架 |
| 《关于商业电子通信和商业电子通信的条例》 | 规范商业短信发送 |
| BTK URL 合规指令（2026年） | 禁止 SMS 中的短链接和通用 URL |
| 消费者保护法 | 用户权益保护 |

### 5.3 A2P SMS 核心监管要求

- **Sender ID 强制注册**：所有 A2P SMS 必须使用已注册的 Sender ID
- **仅事务性消息**：禁止通过 A2P SMS 发送促销/营销内容
- **URL 合规**：2026年4月1日起禁止短链接和通用 URL
- **用户同意**：必须获得接收者的事先同意
- **内容审核**：运营商对消息内容有过滤机制

### 5.4 禁止/限制内容

| 类别 | 限制程度 |
|------|---------|
| 促销/营销消息 | **禁止** |
| 未经同意的消息 | **禁止** |
| 赌博/博彩 | **严禁** |
| 色情内容 | **严禁** |
| 政治宣传 | 有限制 |
| 金融诈骗/钓鱼 | **严禁** |
| 短链接/通用 URL | **禁止**（2026年4月1日起） |
| 未授权品牌使用 | 禁止 |

### 5.5 发送时间建议

- **事务性消息（OTP、通知）**：24小时均可发送
- **建议避免深夜发送非紧急通知**：22:00-08:00 TRT（UTC+3）
- 宗教节日期间需注意发送时间

---

## 六、土耳其数据保护法 (KVKK)

### 6.1 法律概述

**KVKK**（Kişisel Verilerin Korunması Kanunu，个人数据保护法，第 6698 号）于2016年生效，是土耳其核心数据保护法律，模仿欧盟数据保护框架但有本地化特点。

### 6.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **用户同意** | 处理个人数据须获得数据主体的明确同意 |
| **VERBIS 注册** | 数据控制者必须在 VERBIS（数据控制者登记系统）注册 |
| **数据主体权利** | 用户有权知情、访问、更正、删除、反对处理其个人数据 |
| **跨境传输** | 个人数据出境须满足充足性认定或数据主体同意+书面承诺 |
| **数据保留** | 不应超过实现处理目的所必需的期限 |
| **数据安全** | 采取技术和组织措施确保数据安全 |
| **数据泄露通知** | 发生数据泄露时须在72小时内通知 KVKK 委员会 |

### 6.3 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| 未履行告知义务 | 5,000-100,000 土耳其里拉 |
| 未履行数据安全义务 | 15,000-1,000,000 土耳其里拉 |
| 未遵守 KVKK 委员会决定 | 25,000-1,000,000 土耳其里拉 |
| 未在 VERBIS 注册 | 20,000-1,000,000 土耳其里拉 |
| 非法获取/传播个人数据 | **1-3年监禁**（刑事） |
| 非法保留应删除的数据 | **最高2年监禁**（刑事） |

### 6.4 KVKK 委员会

个人数据保护委员会（KVKK Board）是土耳其数据保护的执法机构，负责接收投诉、进行调查和做出处罚决定。

---

## 七、运营商与号码格式

### 7.1 主要运营商

| 运营商 | 英文名 | 市场份额（约） | 备注 |
|--------|--------|-------------|------|
| **Turkcell** | Turkcell | ~40% | 土耳其最大运营商 |
| **Vodafone** | Vodafone Turkey | ~30% | 第二大运营商 |
| **Turk Telekom** | Turk Telekom Mobile (TT Mobil) | ~25% | 第三大运营商 |

### 7.2 号码格式

#### 基本结构
- 国家代码：+90
- 手机号码长度：10位（含区号）
- 格式：+90 5XX XXX XX XX

#### 运营商号段

| 运营商 | 号段 | 示例 |
|--------|------|------|
| Turkcell | 530-539, 550-559 | +90 532 XXX XX XX |
| Vodafone | 540-549, 501-509 | +90 542 XXX XX XX |
| Turk Telekom | 510-519, 560-569 | +90 512 XXX XX XX |

#### AWS SMS 正确格式
```
+905321234567      ← Turkcell 示例
+905421234567      ← Vodafone 示例
+905121234567      ← Turk Telekom 示例
```

> **重要：** 使用 E.164 格式，`+90` 后跟 10 位号码（以 `5` 开头）。国内拨打时加前导 `0`（如 0532-XXX-XX-XX），但发送 SMS 时不使用前导 `0`。固定电话号码不可接收短信。

### 7.3 运营商技术细节

- 所有三大运营商均支持 A2P SMS
- 运营商有内容过滤系统，特别是 URL 审核
- 2026年4月起运营商严格执行 URL 新规
- 促销内容会被自动拦截
- 不支持双向 SMS
- 支持号码携带（携号转网）

---

## 八、不支持双向 SMS 的替代退出方案

### 8.1 问题

AWS 上土耳其不支持双向 SMS，用户无法回复 STOP 退订。必须提供替代 opt-out 机制。

### 8.2 替代方案

#### 方案一：品牌域名退出链接（推荐）

**土耳其语版：**
```
[Brand] Abonelikten cikmak icin: https://brand.com/unsub/{TOKEN}
```

**英语版：**
```
[Brand] Unsubscribe: https://brand.com/unsub/{TOKEN}
```

> **重要：** 退出链接必须使用品牌自有域名，不得使用短链接服务。

#### 方案二：App 内设置

在 App 用户设置中提供通知偏好管理。

#### 方案三：客服渠道

```
Abonelikten cikmak icin: optout@brand.com
```

### 8.3 最佳实践

1. 退出链接必须使用品牌自有域名（URL 新规要求）
2. 退出页面提供土耳其语界面
3. 退出后24小时内停止发送
4. 保留所有退出记录

---

## 九、消息模板范例（土耳其语/英语）

### 9.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含特殊字符 |

**土耳其语特殊字符与 GSM-7 兼容性：**

| 字符 | GSM-7 兼容 | 说明 |
|------|-----------|------|
| `ç` / `Ç` | **是** | GSM-7 扩展表中 |
| `ğ` / `Ğ` | **否** | 触发 Unicode |
| `ı` (无点 i) | **否** | 触发 Unicode |
| `İ` (大写有点 I) | **否** | 触发 Unicode |
| `ö` / `Ö` | **是** | GSM-7 基本表中 |
| `ş` / `Ş` | **否** | 触发 Unicode |
| `ü` / `Ü` | **是** | GSM-7 基本表中 |

> **建议：** 土耳其语中的 `ğ`、`ı`、`İ`、`ş`/`Ş` 不在 GSM-7 字符集中，包含这些字符会触发 Unicode 编码（70字符/条）。可考虑以下策略：
> - **方案A**：使用替代写法（`g` 替代 `ğ`，`i` 替代 `ı`，`s` 替代 `ş`），保持 GSM-7（160字符/条）
> - **方案B**：保留原始土耳其语，接受 Unicode 编码（70字符/条）
> - **方案C**：对 OTP 等短消息使用原始土耳其语，对长消息使用替代写法

### 9.2 OTP 验证码模板

**GSM-7 兼容版（替代写法，160字符/条，推荐）：**
```
[Brand] Dogrulama kodunuz: 385291. 5 dakika gecerlidir.
Bu kodu kimseyle paylasmayiniz.
```
（87字符，1条消息）

**原始土耳其语版（Unicode，70字符/条）：**
```
[Brand] Doğrulama kodunuz: 385291. 5 dakika geçerlidir.
Bu kodu kimseyle paylaşmayınız.
```
（87字符，2条消息 = 双倍费用）

**英语版（GSM-7，160字符/条）：**
```
[Brand] Your verification code is: 385291. Valid for 5 minutes.
Do not share this code with anyone.
```
（88字符，1条消息）

### 9.3 交易通知模板

**订单确认 - GSM-7 兼容版：**
```
[Brand] Siparisiniz #TR12345 onaylandi. Tutar: 500 TL.
Teslim: 10 Nis. Takip: https://brand.com/track/TR12345
```
（107字符，1条消息）

**发货通知 - GSM-7 兼容版：**
```
[Brand] Siparisiniz #TR12345 kargoya verildi.
Takip: https://brand.com/track/TR12345
```
（84字符，1条消息）

**签收确认 - GSM-7 兼容版：**
```
[Brand] Siparisiniz #TR12345 teslim edildi.
Degerlendirin: https://brand.com/review/TR12345
```
（93字符，1条消息）

**付款确认 - GSM-7 兼容版：**
```
[Brand] Odeme alindi: 500 TL (siparis #TR12345).
Makbuz: https://brand.com/receipt/TR12345
```
（91字符，1条消息）

**安全告警 - GSM-7 兼容版：**
```
[Brand] Uyari: Hesabiniza yeni bir cihazdan giris yapildi.
Siz degilseniz: https://brand.com/security
```
（99字符，1条消息）

### 9.4 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 土耳其语或英语 |
| 编码 | 建议使用 GSM-7 兼容写法（`g` 替代 `ğ`，`i` 替代 `ı`，`s` 替代 `ş`） |
| 消息类型 | **仅限事务性消息，严禁促销内容** |
| URL | **必须使用品牌自有域名，禁止短链接和通用 URL** |
| 退出指引 | 建议每条通知消息包含退出方式（品牌域名链接） |
| 货币符号 | 使用 `TL` 或 `TRY`（`₺` 为 Unicode 字符，会触发 Unicode 编码） |
| Sender ID | 3-11个字母数字字符，仅限拉丁字符 |

---

## 十、定价信息

### 10.1 AWS 定价

AWS 未在公开文档直接列出土耳其每条费率，需通过：
1. 下载 AWS 定价 CSV：`aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

**已知费用结构：**
- 每条消息费：需参考 AWS 定价 CSV
- 默认消费阈值：$1.00 USD/月（需申请提高）
- Sender ID 注册通常免费

### 10.2 成本优化建议

1. **使用 GSM-7 兼容写法** — 每条 160 字符 vs Unicode 的 70 字符，成本差异巨大
2. **精简消息内容** — 保持单条消息在编码上限内避免拼接
3. **使用品牌短域名** — 减少 URL 字符占用（但必须是自有域名）
4. **维护号码列表** — 定期清理无效号码
5. **避免使用 `₺` 符号** — 使用 `TL` 替代，保持 GSM-7 编码
6. **土耳其语替代字符** — `g`→`ğ`、`i`→`ı`、`s`→`ş` 的替代可节省约 50% 消息段数

---

## 十一、合规清单

在土耳其发送 SMS 前，确保完成：

- [ ] Sender ID 已在 AWS 控制台注册并获批
- [ ] Sender ID LOA 已签署并提交
- [ ] Vendor LOA 已签署并提交
- [ ] 公司注册证书已提交
- [ ] Sender ID 关联证明已提交
- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 实施有效的 opt-out 机制（品牌域名链接，因不支持双向 SMS）
- [ ] 所有消息均为事务性内容（无促销/营销）
- [ ] **所有 URL 均为品牌自有域名**（无短链接、无通用 URL）
- [ ] URL 域名与品牌/Sender ID 关联
- [ ] 遵守 KVKK 数据保护要求
- [ ] 数据控制者已在 VERBIS 注册（如适用）
- [ ] 正确格式化号码（+90 + 10位号码，以 5 开头）
- [ ] 选择合适的编码策略（GSM-7 替代写法或 Unicode）
- [ ] 消息中不使用 `₺` 符号（改用 `TL`）
- [ ] 消息内容不涉及禁止内容
- [ ] 保留同意和退出记录
- [ ] 跨境数据传输已满足 KVKK 要求
- [ ] 准备备用通信渠道（WhatsApp/邮件）

---

## 十二、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| URL 新规违规被拦截 | **极高** | 审查所有模板，确保使用品牌自有域名，禁用短链接 |
| 两份 LOA 缺失导致注册失败 | **高** | 同时准备 Sender ID LOA 和 Vendor LOA |
| 促销内容被拦截 | 高 | 严格区分事务性和营销内容，杜绝混用 |
| Sender ID 被暂停/撤销 | 高 | 严格遵守所有合规要求，特别是 URL 新规 |
| Unicode 编码导致成本翻倍 | 中 | 使用 GSM-7 兼容写法替代土耳其语特殊字符 |
| 不支持双向 SMS | 中 | 提供网页退出机制（品牌域名链接） |
| 运营商内容过滤 | 中 | 使用标准模板，避免可疑内容 |
| 数据保护违规（KVKK） | 中 | 严格遵守 KVKK，在 VERBIS 注册 |
| 注册审核时间长 | 中 | 提前提交，预留 6-12 周审核时间 |
| 号码格式错误 | 低 | 实施严格的号码格式验证（+90 + 10位） |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。土耳其法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询土耳其当地法律顾问以确保合规。
