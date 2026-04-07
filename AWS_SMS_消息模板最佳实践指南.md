# AWS SMS 消息模板最佳实践指南

> 适用于欧洲及其他不需要 Sender ID 注册的国家

## 目录

1. [编码基础与字符限制](#1-编码基础与字符限制)
2. [通用模板编写原则](#2-通用模板编写原则)
3. [OTP/验证码消息模板](#3-otp验证码消息模板)
4. [交易通知消息模板](#4-交易通知消息模板)
5. [营销消息模板](#5-营销消息模板)
6. [Opt-in 确认消息模板](#6-opt-in-确认消息模板)
7. [Opt-out/HELP 响应模板](#7-opt-outhelp-响应模板)
8. [各国特殊内容限制](#8-各国特殊内容限制)
9. [AWS 模板变量语法](#9-aws-模板变量语法)

---

## 1. 编码基础与字符限制

### 1.1 GSM 7-bit vs Unicode 编码

| 编码类型 | 单条上限 | 多条拼接时每段上限 | 最大拼接长度 |
|---------|---------|------------------|------------|
| GSM 7-bit | 160 字符 | 153 字符 | 1530 字符（10段） |
| Unicode (UTF-16) | 70 字符 | 67 字符 | 630 字符（约9段） |

**关键规则：**

- 一条 SMS 最大承载 **140 字节**
- GSM 7-bit 每字符占 7 位，故 140 x 8 / 7 = 160 字符
- Unicode 每字符占 16 位，故 140 x 8 / 16 = 70 字符
- **只要消息中包含任何一个非 GSM 字符，整条消息都会切换为 Unicode 编码**
- 多段拼接时，每段需要额外 7 字节的 UDH（User Data Header）用于排序

### 1.2 GSM 7-bit 支持的字符

**标准字符（每个占 1 字符位）：**
- 拉丁字母：A-Z, a-z
- 数字：0-9
- 重音字符：a A a A a C E e e i N n o O o O o u U u AE ae ss
- 标点符号：& * @ : , $ = ! > # - ( < % . + ? " ) ; ' / _
- 货币符号：£ ¥ ¤
- 希腊字母：Delta Phi Gamma Lambda Omega Pi Psi Sigma Theta Xi
- 特殊：空格、换行符

**扩展字符（每个占 2 字符位）：**
`^` `{` `}` `\` `[` `]` `~` `|` `€`

### 1.3 常见陷阱

| 陷阱 | 问题 | 解决方案 |
|------|------|---------|
| `®` 商标符号 | 触发 Unicode，70 字符限制 | 替换为 `(R)` |
| `™` 商标符号 | 触发 Unicode | 替换为 `(TM)` |
| `'` 弯引号 | Word/邮件复制来的弯引号是非 GSM 字符 | 使用直引号 `'` |
| `"` `"` 弯双引号 | 同上 | 使用直双引号 `"` |
| 中文/日文/韩文 | 必定 Unicode | 控制在 70 字符内或接受多段计费 |
| emoji | 触发 Unicode，且多数 emoji 占 2 个 Unicode 字符位 | 避免使用 |

---

## 2. 通用模板编写原则

### 2.1 AWS 官方最佳实践

1. **开头标明品牌名**：每条消息的开头必须包含公司/品牌名称
2. **避免伪装个人消息**：不要写 "Hi, this is Jane..."
3. **避免货币符号堆叠**：不要写 "$$$"、"€€€"
4. **使用自有域名短链接**：不要用 tinyurl.com、bit.ly（运营商会过滤）
5. **避免缩写**：保持专业，不要写 "gr8"、"2day"、"u"
6. **包含退出指引**：营销消息必须包含 STOP 退出说明
7. **在收件人时区的工作时间发送**：9:00-20:00

### 2.2 模板结构公式

```
[品牌名] [消息类型标识]: [核心内容]. [行动指引]. [退出说明(营销类必须)]
```

---

## 3. OTP/验证码消息模板

### 3.1 英文版 — GSM 7-bit

```
[BrandName] code: 123456. Valid for 5 min. Do not share this code with anyone.
```
- **字符数：** 74 字符（含占位符变量实际值）
- **编码：** GSM 7-bit（1 段）
- **适用：** 所有国家

```
[BrandName]: Your verification code is 123456. It expires in 10 minutes.
```
- **字符数：** 71 字符
- **编码：** GSM 7-bit（1 段）

**极简版（适合全球）：**
```
123456 is your [BrandName] code. Expires in 5 min.
```
- **字符数：** 51 字符
- **编码：** GSM 7-bit（1 段）

### 3.2 中文版 — Unicode

```
【品牌名】您的验证码是123456，5分钟内有效，请勿泄露。
```
- **字符数：** 26 字符
- **编码：** Unicode（1 段，70 字符上限内）

```
【品牌名】验证码：123456，有效期10分钟。如非本人操作请忽略。
```
- **字符数：** 30 字符
- **编码：** Unicode（1 段）

### 3.3 多语言版本 — Unicode

**法语（Francais）：**
```
[BrandName]: Votre code est 123456. Valide 5 min. Ne partagez pas ce code.
```
- **字符数：** 73 字符
- **编码：** GSM 7-bit（1 段）— 法语重音字符 e 在 GSM 字符集中

**德语（Deutsch）：**
```
[BrandName]: Ihr Code lautet 123456. Gueltig fuer 5 Min. Code nicht teilen.
```
- **字符数：** 74 字符
- **编码：** GSM 7-bit（1 段）
- **注意：** 若使用 u/o 等变音符号（Umlauts），这些在 GSM 7-bit 中受支持

**使用 Umlauts 的德语版：**
```
[BrandName]: Ihr Code lautet 123456. Gültig für 5 Min. Code nicht teilen.
```
- **字符数：** 72 字符
- **编码：** GSM 7-bit（1 段）— `ü` 在 GSM 字符集中

**西班牙语（Espanol）：**
```
[BrandName]: Su codigo es 123456. Valido por 5 min. No comparta este codigo.
```
- **字符数：** 76 字符
- **编码：** GSM 7-bit（1 段）
- **注意：** `o` 和 `i` 不在 GSM 字符集中，会触发 Unicode。可省略或替换

**安全的西班牙语 GSM 版：**
```
[BrandName]: Su codigo es 123456. Valido 5 min. No comparta este codigo.
```
- **字符数：** 71 字符
- **编码：** GSM 7-bit（1 段）— 省略重音符

**葡萄牙语（Portugues）：**
```
[BrandName]: Seu codigo e 123456. Valido por 5 min. Nao compartilhe.
```
- **字符数：** 67 字符
- **编码：** GSM 7-bit（1 段）— 省略重音符

**日语（日本語）— Unicode：**
```
【BrandName】認証コード：123456（5分間有効）他人に教えないでください。
```
- **字符数：** 36 字符
- **编码：** Unicode（1 段）

**韩语（한국어）— Unicode：**
```
[BrandName] 인증코드: 123456 (5분간 유효). 타인에게 공유하지 마세요.
```
- **字符数：** 42 字符
- **编码：** Unicode（1 段）

### 3.4 OTP 模板变量版（AWS SNS/Pinpoint）

```
{{brand}} code: {{otp_code}}. Valid for {{expiry_min}} min. Do not share this code.
```

---

## 4. 交易通知消息模板

### 4.1 订单确认

**英文 GSM 7-bit 版：**
```
[BrandName]: Order #12345 confirmed. Total: $99.00. Est. delivery: Apr 12. Track at example.com/track
```
- **字符数：** 97 字符
- **编码：** GSM 7-bit（1 段）

**中文 Unicode 版：**
```
【品牌名】订单#12345已确认，金额$99.00，预计4月12日送达。查看详情：example.com/t
```
- **字符数：** 48 字符
- **编码：** Unicode（1 段）

**精简中文版（确保 70 字符内）：**
```
【品牌名】订单12345已确认，合计99元，预计4/12送达。
```
- **字符数：** 27 字符
- **编码：** Unicode（1 段）

**法语版：**
```
[BrandName]: Commande #12345 confirmee. Total: 99.00 EUR. Livraison estimee: 12 avr. Suivi: example.com/t
```
- **字符数：** 103 字符
- **编码：** GSM 7-bit（1 段）

**德语版：**
```
[BrandName]: Bestellung #12345 bestaetigt. Summe: 99,00 EUR. Lieferung ca. 12. Apr. Infos: example.com/t
```
- **字符数：** 102 字符
- **编码：** GSM 7-bit（1 段）

### 4.2 配送通知

**英文 GSM 7-bit 版：**
```
[BrandName]: Your order #12345 has shipped! Track: example.com/track/12345
```
- **字符数：** 72 字符
- **编码：** GSM 7-bit（1 段）

```
[BrandName]: Your package is out for delivery today. Track: example.com/t/12345
```
- **字符数：** 77 字符
- **编码：** GSM 7-bit（1 段）

```
[BrandName]: Your order #12345 was delivered. Questions? Visit example.com/help
```
- **字符数：** 78 字符
- **编码：** GSM 7-bit（1 段）

**中文 Unicode 版：**
```
【品牌名】订单12345已发货，物流单号SF1234567890。查看物流：example.com/t
```
- **字符数：** 42 字符
- **编码：** Unicode（1 段）

```
【品牌名】您的包裹正在派送中，预计今日送达。
```
- **字符数：** 21 字符
- **编码：** Unicode（1 段）

```
【品牌名】订单12345已签收。如有问题请联系：example.com/help
```
- **字符数：** 34 字符
- **编码：** Unicode（1 段）

### 4.3 付款确认

**英文 GSM 7-bit 版：**
```
[BrandName]: Payment of $150.00 received for order #12345. Thank you!
```
- **字符数：** 66 字符
- **编码：** GSM 7-bit（1 段）

```
[BrandName]: Payment of 150.00 EUR confirmed. Ref: PAY-12345. Receipt: example.com/r/12345
```
- **字符数：** 89 字符
- **编码：** GSM 7-bit（1 段）
- **注意：** 使用 `EUR` 而非 `€`，因为 `€` 在 GSM 扩展字符中占 2 位

**中文 Unicode 版：**
```
【品牌名】已收到您的付款150.00元，订单号12345。感谢购买！
```
- **字符数：** 29 字符
- **编码：** Unicode（1 段）

**法语版：**
```
[BrandName]: Paiement de 150.00 EUR recu pour commande #12345. Merci!
```
- **字符数：** 66 字符
- **编码：** GSM 7-bit（1 段）

### 4.4 预约提醒

**英文 GSM 7-bit 版：**
```
[BrandName] Reminder: Your appointment is on Apr 10 at 2:30 PM. Reply YES to confirm or NO to reschedule.
```
- **字符数：** 103 字符
- **编码：** GSM 7-bit（1 段）

```
[BrandName]: Reminder - appointment tomorrow at 3:00 PM with Dr. Smith. Reply C to confirm.
```
- **字符数：** 89 字符
- **编码：** GSM 7-bit（1 段）

**中文 Unicode 版：**
```
【品牌名】提醒：您4月10日14:30有预约。回复Y确认，N改期。
```
- **字符数：** 29 字符
- **编码：** Unicode（1 段）

**德语版：**
```
[BrandName]: Erinnerung - Ihr Termin am 10. Apr. um 14:30 Uhr. Antworten Sie JA zur Bestaetigung.
```
- **字符数：** 95 字符
- **编码：** GSM 7-bit（1 段）

**西班牙语版：**
```
[BrandName]: Recordatorio - Su cita es el 10 abr a las 14:30. Responda SI para confirmar.
```
- **字符数：** 89 字符
- **编码：** GSM 7-bit（1 段）

---

## 5. 营销消息模板

> **重要：** 欧洲所有营销 SMS 必须获得用户的事先明确同意（GDPR/ePrivacy Directive），且必须包含退出机制。

### 5.1 促销活动

**英文 GSM 7-bit 版：**
```
[BrandName] Sale: 25% off all items this weekend! Use code SAVE25 at checkout. Shop: example.com/sale Reply STOP to opt out.
```
- **字符数：** 122 字符
- **编码：** GSM 7-bit（1 段）

```
[BrandName] Offers: Flash sale today only - up to 50% off selected items. Visit example.com/flash. Txt STOP to unsubscribe.
```
- **字符数：** 121 字符
- **编码：** GSM 7-bit（1 段）

**中文 Unicode 版：**
```
【品牌名】全场75折！本周末限定，结账使用码SAVE25。详情：example.com/s 回复TD退订
```
- **字符数：** 46 字符
- **编码：** Unicode（1 段）

**精简中文版：**
```
【品牌名】限时特惠5折起！详情example.com/s 回TD退订
```
- **字符数：** 28 字符
- **编码：** Unicode（1 段）

**法语版：**
```
[BrandName]: Soldes - 25% sur tout ce weekend! Code: SAVE25. Achetez: example.com/s. STOP pour se desabonner.
```
- **字符数：** 106 字符
- **编码：** GSM 7-bit（1 段）

**德语版：**
```
[BrandName]: 25% Rabatt auf alles dieses Wochenende! Code: SAVE25. Shop: example.com/s. STOP zum Abbestellen.
```
- **字符数：** 108 字符
- **编码：** GSM 7-bit（1 段）

### 5.2 新品发布

**英文 GSM 7-bit 版：**
```
[BrandName]: New arrival! The ExampleWidget Pro is here. Order now: example.com/new. Reply STOP to opt out.
```
- **字符数：** 101 字符
- **编码：** GSM 7-bit（1 段）

**中文 Unicode 版：**
```
【品牌名】新品上市！ExampleWidget Pro现已开售。立即选购：example.com/new 回TD退订
```
- **字符数：** 47 字符
- **编码：** Unicode（1 段）

### 5.3 会员通知

**英文 GSM 7-bit 版：**
```
[BrandName] VIP: You have 500 points expiring on Apr 30. Redeem now: example.com/rewards. Reply STOP to opt out.
```
- **字符数：** 109 字符
- **编码：** GSM 7-bit（1 段）

```
[BrandName]: As a valued member, enjoy early access to our spring collection. Shop: example.com/vip. STOP to opt out.
```
- **字符数：** 115 字符
- **编码：** GSM 7-bit（1 段）

**中文 Unicode 版：**
```
【品牌名】尊敬的会员，您有500积分将于4月30日过期，请尽快使用：example.com/r 回TD退订
```
- **字符数：** 47 字符
- **编码：** Unicode（1 段）

---

## 6. Opt-in 确认消息模板

> **欧洲要求：** ePrivacy Directive 和 GDPR 要求在发送营销 SMS 前获得明确同意。建议使用双重确认（Double Opt-in）。

### 6.1 首次订阅确认（Single Opt-in）

**英文 GSM 7-bit 版：**
```
[BrandName]: You are now subscribed to alerts. 2 msgs/month. Msg & data rates may apply. Reply HELP for help, STOP to cancel.
```
- **字符数：** 124 字符
- **编码：** GSM 7-bit（1 段）

**AWS 推荐格式：**
```
Text YES to join [BrandName] alerts. 2 msgs/month. Msg & data rates may apply. Reply HELP for help, STOP to cancel.
```
- **字符数：** 115 字符
- **编码：** GSM 7-bit（1 段）

**中文 Unicode 版：**
```
【品牌名】您已成功订阅消息通知，约2条/月。回复HELP获取帮助，回复TD退订。
```
- **字符数：** 36 字符
- **编码：** Unicode（1 段）

### 6.2 双重确认（Double Opt-in）

**英文 GSM 7-bit 版 — 步骤一（发送确认请求）：**
```
[BrandName]: Reply YES to confirm your subscription. 2 msgs/month. Msg & data rates may apply. Reply STOP to cancel.
```
- **字符数：** 115 字符
- **编码：** GSM 7-bit（1 段）

**英文 GSM 7-bit 版 — 步骤二（确认成功）：**
```
[BrandName]: Subscription confirmed! You will receive up to 2 msgs/month. Reply HELP for help, STOP to unsubscribe.
```
- **字符数：** 114 字符
- **编码：** GSM 7-bit（1 段）

**中文 Unicode 版 — 步骤一：**
```
【品牌名】请回复Y确认订阅短信通知（约2条/月）。回复TD取消。
```
- **字符数：** 28 字符
- **编码：** Unicode（1 段）

**中文 Unicode 版 — 步骤二：**
```
【品牌名】订阅成功！每月最多2条通知。回复HELP帮助，TD退订。
```
- **字符数：** 28 字符
- **编码：** Unicode（1 段）

**法语版 — 步骤一：**
```
[BrandName]: Repondez OUI pour confirmer votre abonnement. 2 msgs/mois. Repondez STOP pour annuler.
```
- **字符数：** 96 字符
- **编码：** GSM 7-bit（1 段）

**德语版 — 步骤一：**
```
[BrandName]: Antworten Sie JA um Ihr Abo zu bestaetigen. 2 Nachr./Monat. STOP zum Abbestellen.
```
- **字符数：** 93 字符
- **编码：** GSM 7-bit（1 段）

---

## 7. Opt-out/HELP 响应模板

### 7.1 STOP 响应（退订确认）

**英文 GSM 7-bit 版：**
```
[BrandName]: You have been unsubscribed. No more messages will be sent. Reply HELP or email help@example.com for info.
```
- **字符数：** 115 字符
- **编码：** GSM 7-bit（1 段）

**AWS 推荐格式：**
```
You're unsubscribed from [BrandName] alerts. No more messages will be sent. Reply HELP, email help@example.com, or call 425-555-0199 for more info.
```
- **字符数：** 150 字符
- **编码：** GSM 7-bit（1 段）— 注意接近 160 上限

**中文 Unicode 版：**
```
【品牌名】您已成功退订，不再接收短信通知。如需帮助请发HELP或联系help@example.com
```
- **字符数：** 44 字符
- **编码：** Unicode（1 段）

**法语版：**
```
[BrandName]: Vous etes desabonne. Plus de messages. Repondez HELP ou email help@example.com.
```
- **字符数：** 90 字符
- **编码：** GSM 7-bit（1 段）

**德语版：**
```
[BrandName]: Abgemeldet. Keine weiteren Nachrichten. Antworten Sie HELP oder email help@example.com.
```
- **字符数：** 98 字符
- **编码：** GSM 7-bit（1 段）

### 7.2 HELP 响应（帮助信息）

**英文 GSM 7-bit 版：**
```
[BrandName] Help: Email help@example.com or call +1-425-555-0199. 2 msgs/month. Msg & data rates may apply. Reply STOP to cancel.
```
- **字符数：** 128 字符
- **编码：** GSM 7-bit（1 段）

**AWS 推荐格式：**
```
HELP: [BrandName] alerts: email help@example.com or call 425-555-0199. 2 msgs/month. Msg & data rates may apply. Reply STOP to cancel.
```
- **字符数：** 136 字符
- **编码：** GSM 7-bit（1 段）

**中文 Unicode 版：**
```
【品牌名】帮助：邮件help@example.com或致电400-XXX-XXXX。约2条/月。回复TD退订。
```
- **字符数：** 44 字符
- **编码：** Unicode（1 段）

> **免费电话号码（Toll-Free）注意：** 如果使用 AWS 的 Toll-Free 号码发送，只有 STOP 和 UNSTOP 关键字有效，运营商会自动管理响应，无需自定义 STOP 回复内容。

---

## 8. 各国特殊内容限制

### 8.1 不需要 Sender ID 注册的欧洲国家

| 国家 | 代码 | 特殊内容限制 |
|------|------|------------|
| 奥地利 | AT | 无特殊限制 |
| 比利时 | BE | 无特殊限制 |
| 保加利亚 | BG | 无特殊限制 |
| 克罗地亚 | HR | 无特殊限制 |
| 捷克 | CZ | 无特殊限制 |
| 丹麦 | DK | 无特殊限制 |
| 爱沙尼亚 | EE | 无特殊限制 |
| 芬兰 | FI | 无特殊限制 |
| 德国 | DE | 无特殊限制 |
| 意大利 | IT | 无特殊限制 |
| 立陶宛 | LT | 无特殊限制 |
| 卢森堡 | LU | 无特殊限制 |
| 马耳他 | MT | 无特殊限制 |
| 荷兰 | NL | 无特殊限制 |
| 挪威 | NO | 无特殊限制 |
| 波兰 | PL | 无特殊限制 |
| 葡萄牙 | PT | 无特殊限制 |
| 罗马尼亚 | RO | 无特殊限制 |
| 塞尔维亚 | RS | 无特殊限制 |
| 斯洛伐克 | SK | 无特殊限制 |
| 西班牙 | ES | 无特殊限制 |
| 瑞典 | SE | 无特殊限制 |
| 乌克兰 | UA | 无特殊限制 |

### 8.2 需要注册的欧洲国家

| 国家 | 代码 | 特殊要求 |
|------|------|---------|
| 法国 | FR | 2026年3月1日起，Sender ID **不允许包含破折号（-）**，只能使用字母数字字符 |
| 英国 | GB | 必须注册 Sender ID |
| 爱尔兰 | IE | 必须在 ComReg SMS Sender ID Registry 注册，未注册的 Sender ID 会显示为 "Likely Scam" |

### 8.3 不需要注册的非欧洲国家

| 国家 | 代码 | 特殊要求 |
|------|------|---------|
| 美国 | US | 需要 10DLC/Toll-Free/Short Code 注册（非 Sender ID 注册） |
| 加拿大 | CA | 遵循 CASL 法规 |
| 新西兰 | NZ | 无特殊限制 |
| 巴西 | BR | 无特殊限制 |
| 墨西哥 | MX | 无特殊限制 |
| 日本 | JP | 无特殊限制 |
| 香港 | HK | 无特殊限制 |
| 以色列 | IL | 无特殊限制 |
| 南非 | ZA | 无特殊限制 |

### 8.4 所有国家通用的禁止内容

以下内容在所有国家/地区都被 AWS 及运营商严格禁止：

- **赌博类：** 赌场、彩票、投注
- **高风险金融：** 发薪日贷款、加密货币、催收
- **债务减免：** 债务合并、信用修复
- **快速致富：** 居家赚钱、传销
- **违禁物质：** 大麻、电子烟产品
- **处方药：** 任何处方药物
- **钓鱼/诈骗：** 个人信息收集、安全培训模拟
- **SHAFT 内容：** 色情、仇恨言论、酒精、枪械、烟草
- **第三方引流：** 数据经纪人、联盟营销

### 8.5 欧洲 GDPR/ePrivacy 特别要求

| 要求 | 详情 |
|------|------|
| 事先明确同意 | 营销消息必须获得用户事先明确的、可证明的同意 |
| 发送者身份标识 | 每条消息必须清楚标明发送者身份 |
| 免费退出 | 退出机制必须简单且免费 |
| 同意记录 | 必须保存每个用户的同意日期、时间、来源 |
| 用途限制 | 用户同意一种消息类型，不能用于发送其他类型 |
| 审计准备 | 随时准备接受运营商或监管机构的审计 |

---

## 9. AWS 模板变量语法

### 9.1 AWS SNS 消息属性

在 AWS SNS 中发送 SMS 时，可以通过 MessageAttributes 设置：

```json
{
  "AWS.SNS.SMS.SenderID": { "DataType": "String", "StringValue": "BrandName" },
  "AWS.SNS.SMS.SMSType": { "DataType": "String", "StringValue": "Transactional" },
  "AWS.SNS.SMS.MaxPrice": { "DataType": "Number", "StringValue": "0.50" }
}
```

**SMSType 选项：**
- `Transactional`：OTP、订单确认、配送通知等（优先送达，成本较高）
- `Promotional`：营销消息（成本较低，可能延迟送达）

### 9.2 模板变量占位符建议

在代码中构建消息时，建议使用命名变量：

```
OTP:    "{brand} code: {otp_code}. Valid for {expiry} min. Do not share this code."
ORDER:  "{brand}: Order #{order_id} confirmed. Total: {amount}. Delivery: {date}."
SHIP:   "{brand}: Order #{order_id} shipped! Track: {tracking_url}"
PAY:    "{brand}: Payment of {amount} received for order #{order_id}. Thank you!"
REMIND: "{brand} Reminder: Appointment on {date} at {time}. Reply YES to confirm."
PROMO:  "{brand}: {offer_text}. Shop: {url}. Reply STOP to opt out."
OPTIN:  "Reply YES to join {brand} alerts. {freq} msgs/month. Msg & data rates may apply. STOP to cancel."
STOP:   "You're unsubscribed from {brand} alerts. No more messages. HELP or {email} for info."
HELP:   "HELP: {brand} alerts: {email} or call {phone}. {freq} msgs/month. STOP to cancel."
```

### 9.3 AWS Pinpoint 模板语法

如果使用 AWS Pinpoint（End User Messaging），模板变量使用双花括号：

```
{{BrandName}}: Your verification code is {{OTPCode}}. Valid for {{ExpiryMinutes}} minutes.
```

---

## 附录 A：字符数速查表

### GSM 7-bit 模板（160 字符以内 = 1 段）

| 模板类型 | 模板内容 | 字符数 |
|---------|---------|-------|
| OTP（极简） | `123456 is your [Brand] code.` | 29 |
| OTP（标准） | `[Brand] code: 123456. Valid for 5 min. Do not share this code with anyone.` | 74 |
| 订单确认 | `[Brand]: Order #12345 confirmed. Total: $99.00. Track: example.com/t` | 69 |
| 配送通知 | `[Brand]: Your order #12345 has shipped! Track: example.com/track/12345` | 72 |
| 付款确认 | `[Brand]: Payment of $150.00 received for order #12345. Thank you!` | 66 |
| 预约提醒 | `[Brand] Reminder: Appointment on Apr 10 at 2:30 PM. Reply YES to confirm.` | 75 |
| 促销 | `[Brand] Sale: 25% off all items! Code SAVE25. Shop: example.com/s Reply STOP to opt out.` | 90 |
| 订阅确认 | `Reply YES to join [Brand] alerts. 2 msgs/month. Msg & data rates may apply. STOP to cancel.` | 93 |
| STOP 回复 | `[Brand]: Unsubscribed. No more messages. HELP or help@example.com for info.` | 77 |
| HELP 回复 | `HELP: [Brand]: help@example.com or +1-XXX-XXX-XXXX. 2 msgs/month. STOP to cancel.` | 84 |

### Unicode 模板（70 字符以内 = 1 段）

| 模板类型 | 模板内容 | 字符数 |
|---------|---------|-------|
| OTP | 【品牌】验证码：123456，5分钟有效，请勿泄露。 | 22 |
| 订单确认 | 【品牌】订单12345已确认，合计99元，预计4/12送达。 | 27 |
| 配送通知 | 【品牌】订单12345已发货，查看物流：example.com/t | 27 |
| 付款确认 | 【品牌】已收到付款150元，订单12345。感谢！ | 22 |
| 预约提醒 | 【品牌】提醒：4月10日14:30有预约。回复Y确认。 | 24 |
| 促销 | 【品牌】限时5折起！详情example.com/s 回TD退订 | 26 |
| 订阅确认 | 【品牌】回复Y确认订阅通知（约2条/月）。回TD退订。 | 26 |
| STOP 回复 | 【品牌】已退订，不再发送。帮助请发HELP。 | 19 |
| HELP 回复 | 【品牌】帮助：help@example.com 约2条/月 回TD退订 | 28 |

---

## 附录 B：编码检测清单

发送前请确认：

- [ ] 消息中是否包含非 GSM 字符？（中文/日文/韩文/emoji/特殊符号）
- [ ] 如果是 GSM 7-bit，是否包含扩展字符（`€` `^` `{` `}` `[` `]` `\` `~` `|`）？每个算 2 字符
- [ ] `®` 是否已替换为 `(R)`？`™` 是否已替换为 `(TM)`？
- [ ] 引号是否为直引号（`'` `"`）而非弯引号（`'` `'` `"` `"`）？
- [ ] 消息总字符数是否在单段限制内？GSM: 160 / Unicode: 70
- [ ] 如果超出单段，是否可接受多段计费？
- [ ] 营销消息是否包含退出指引（STOP/TD）？
- [ ] 消息开头是否标明了品牌名？
- [ ] 是否避免了使用公共短链接服务（bit.ly/tinyurl.com）？

---

## 附录 C：欧洲语言 GSM 兼容性速查

| 语言 | GSM 7-bit 兼容性 | 注意事项 |
|------|-----------------|---------|
| 英语 | 完全兼容 | 无特殊字符问题 |
| 法语 | 大部分兼容 | `e` `a` `E` 在 GSM 中，但 `oe`（如 coeur）不在 GSM 中 |
| 德语 | 大部分兼容 | `a` `o` `u` `A` `O` `U` `ss` 全部在 GSM 字符集中 |
| 西班牙语 | 部分兼容 | `n` 在 GSM 中，但 `a` `e` `i` `o` `u` **不在 GSM 中**，会触发 Unicode |
| 葡萄牙语 | 部分兼容 | `a` `o` 等重音字符不在 GSM 中 |
| 意大利语 | 大部分兼容 | `a` `e` `e` `i` `o` `u` 大多在 GSM 中 |
| 荷兰语 | 完全兼容 | 标准拉丁字母即可 |
| 瑞典语/挪威语/丹麦语 | 大部分兼容 | `A` `a` `O` `o` `AE` `ae` 在 GSM 中；`A` `a`（带圆圈）在 GSM 中 |
| 波兰语 | 部分兼容 | 特殊字符如 `ą` `ę` `ł` `ś` `ź` `ż` **不在 GSM 中** |
| 捷克语/斯洛伐克语 | 部分兼容 | 特殊变音字符多数不在 GSM 中 |
| 罗马尼亚语 | 部分兼容 | `ț` `ș` `ă` 不在 GSM 中 |
| 中文/日文/韩文 | 不兼容 | 必须使用 Unicode，70 字符限制 |

**建议：** 对于西班牙语、葡萄牙语、波兰语、捷克语、罗马尼亚语等，如果希望保持 GSM 7-bit 编码（160 字符限制），可以省略重音符号。大多数收件人能够理解无重音的文本。如果需要正确的重音符号，则必须接受 Unicode 编码和 70 字符的限制。
