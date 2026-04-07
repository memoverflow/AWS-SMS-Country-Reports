# AWS SMS 欧盟全面支持国家合集报告

> 调研日期：2026年4月7日
> 覆盖国家：9个（德国、意大利、西班牙、奥地利、葡萄牙、罗马尼亚、芬兰、荷兰、瑞典）
> 数据来源：AWS End User Messaging SMS 官方文档、各国电信监管机构资料、GDPR/ePrivacy 法规

---

## 一、通用合规要求（GDPR / ePrivacy）

本合集覆盖的 9 个国家均为欧盟成员国，全部适用 **GDPR**（通用数据保护条例）和 **ePrivacy 指令**（2002/58/EC）。以下为通用合规要求。

### 1.1 GDPR 核心要求

| 要求 | 详情 |
|------|------|
| **合法基础** | 发送营销 SMS 必须基于用户明确同意（Article 6(1)(a)）；事务性 SMS 可基于合同履行（Article 6(1)(b)） |
| **明确同意** | 同意必须自由给出、具体、知情、明确；预勾选复选框无效 |
| **撤回权** | 用户必须能随时撤回同意，且撤回方式应与给予同意一样简便 |
| **数据最小化** | 仅收集发送 SMS 所必需的数据（手机号码、同意记录） |
| **数据访问权** | 用户有权请求获取其个人数据副本（30天内响应） |
| **删除权（被遗忘权）** | 用户有权要求删除其个人数据 |
| **数据可携带权** | 用户有权以结构化格式获取其数据 |
| **数据保护影响评估** | 大规模处理个人数据时可能需要 DPIA |
| **数据泄露通知** | 72小时内向监管机构报告数据泄露 |
| **跨境传输** | 向 EU/EEA 以外传输数据需确保充分保护（如 SCCs） |

### 1.2 ePrivacy 指令要求

| 要求 | 详情 |
|------|------|
| **事先同意** | 直接营销通信（包括 SMS）必须获得事先同意（opt-in） |
| **发送者标识** | 每条消息必须包含发送者身份标识 |
| **退出机制** | 每条营销消息必须提供简便的退出方式 |
| **通信保密** | 电子通信内容受保密保护 |

### 1.3 GDPR 违规处罚

| 违规类型 | 最高罚款 |
|---------|---------|
| 一般违规 | 1,000万欧元或全球年营业额 2%（取较高者） |
| 严重违规（如同意违规） | 2,000万欧元或全球年营业额 4%（取较高者） |

### 1.4 通用 Sender ID 说明

本合集 9 个国家**均支持 Sender ID**，且均**无需强制预注册**。

| 项目 | 说明 |
|------|------|
| 格式 | 3-11个字母数字字符 |
| 注册要求 | 无需强制预注册 |
| 大小写 | 区分大小写，建议全大写或首字母大写 |
| 特殊字符 | 避免使用特殊字符和空格 |
| 运营商行为 | 部分运营商可能覆写未识别的 Sender ID |

**建议：** 虽然不强制注册，但建议使用一致的品牌 Sender ID，并在首次发送前通过小批量测试确认各运营商的实际显示效果。

### 1.5 通用 Opt-in / Opt-out 要求

**Opt-in（用户同意）：**
1. 必须获得明确的、知情的同意
2. 同意记录必须保留（时间、方式、内容）
3. 不同消息类型需分别获得同意
4. 预勾选复选框不构成有效同意
5. 双重确认（Double Opt-in）为最佳实践

**Opt-out（退订）：**
1. 本合集所有国家均支持双向 SMS，可使用 STOP 关键词退订
2. 每条营销消息必须包含退订指引
3. 退订请求应即时处理
4. 退订后不得再发送营销消息（事务性消息需单独判断）
5. AWS 自动处理 STOP/HELP 关键词回复

---

## 二、9国能力对比表

| 国家 | 代码 | 国际代码 | 短码 | 长码 | Sender ID | 双向 SMS | 注册要求 | 监管机构 | 单价 (USD/条) |
|------|------|---------|------|------|-----------|---------|---------|---------|--------------|
| 德国 | DE | +49 | Yes | Yes | Yes | Yes | 无 | BNetzA | 参见定价表 |
| 意大利 | IT | +39 | Yes | Yes | Yes | Yes | 无 | AGCOM | 参见定价表 |
| 西班牙 | ES | +34 | Yes | Yes | Yes | Yes | 无 | CNMC | 参见定价表 |
| 奥地利 | AT | +43 | Yes | Yes | Yes | Yes | 无 | RTR | $0.175 |
| 葡萄牙 | PT | +351 | Yes | Yes | Yes | Yes | 无 | ANACOM | $0.018 |
| 罗马尼亚 | RO | +40 | Yes | Yes | Yes | Yes | 无 | ANCOM | $0.037 |
| 芬兰 | FI | +358 | Yes | Yes | Yes | Yes | 无 | Traficom | $0.139 |
| 荷兰 | NL | +31 | Yes | Yes | Yes | Yes | 无 | ACM/AT | $0.681 |
| 瑞典 | SE | +46 | Yes | Yes | Yes | Yes | 无 | PTS | $0.01 |

**共同特征：**
- 所有 9 国均支持短码、长码、Sender ID 和双向 SMS
- 所有 9 国均适用 GDPR + ePrivacy
- 所有 9 国均无需 Sender ID 强制预注册
- 所有 9 国均支持国际发送

---

## 三、各国详细说明

---

### 3.1 德国 (DE, +49)

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +49 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |

#### 监管机构

**Bundesnetzagentur**（联邦网络局）— 德国电信和网络监管机构，负责电话号码分配、垃圾信息投诉处理和市场监管。

#### 补充法规：BDSG

德国联邦数据保护法（**Bundesdatenschutzgesetz, BDSG**）补充 GDPR，在以下方面有更具体规定：
- 员工数据保护（§26 BDSG）
- 数据保护官任命门槛（20人以上常规处理个人数据）
- 视频监控限制
- 信用评分和商业数据处理

#### 公司识别号

**USt-IdNr**（Umsatzsteuer-Identifikationsnummer，增值税识别号）— 在提交 AWS Support Case 或运营商注册时可能需要提供。

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +49 1XX XXXXXXX(X) | +49 151 12345678 |
| 固话（柏林） | +49 30 XXXXXXXX | +49 30 12345678 |
| 固话（慕尼黑） | +49 89 XXXXXXXX | +49 89 12345678 |

- 手机号码以 15X、16X、17X 开头
- 总长度：11-12位（不含国家代码）
- AWS 格式：`+49XXXXXXXXXXX`（E.164 格式，去掉前导 0）

#### 主要运营商

| 运营商 | 网络类型 | 备注 |
|--------|---------|------|
| Telekom (T-Mobile) | 4G/5G | 德国最大运营商 |
| Vodafone | 4G/5G | 第二大运营商 |
| O2 (Telefonica) | 4G/5G | 合并 E-Plus 后第三大 |
| 1&1 Drillisch | 4G/5G（MVNO） | 使用 O2/Vodafone 网络 |

#### 发送时间建议

- **推荐发送时段：** 09:00-20:00 CET（UTC+1）/ CEST（UTC+2 夏令时）
- 周日和法定假日避免发送营销消息
- 德国消费者对隐私特别敏感，建议严格控制发送频率

---

### 3.2 意大利 (IT, +39)

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +39 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |

#### 监管机构

**AGCOM**（Autorita per le Garanzie nelle Comunicazioni，通信保障管理局）— 意大利电信、媒体和邮政监管机构。

**Garante per la Protezione dei Dati Personali** — 意大利数据保护局，负责 GDPR 执法。

#### 特有要求

- 意大利对 A2P SMS 的内容过滤相对宽松
- 营销 SMS 需严格遵循 GDPR 同意要求
- 意大利数据保护局对电子直接营销执法活跃
- Registro delle Opposizioni（反对登记簿）— 2022年起扩展至手机号码，营销消息发送前建议核查

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +39 3XX XXX XXXX | +39 320 1234567 |

- 意大利手机号以 3 开头
- 注意：意大利号码**包含前导 0 作为区号的一部分**（固话），但手机号不含前导 0
- 总长度：9-10位（不含国家代码）
- AWS 格式：`+393XXXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| TIM (Telecom Italia) | 最大运营商 |
| Vodafone Italia | 第二大 |
| WindTre | 合并后第三大 |
| Iliad | 低价竞争者 |

#### 发送时间建议

- **推荐发送时段：** 09:00-21:00 CET/CEST
- 避免 8 月中旬（Ferragosto 假期前后）发送营销消息

---

### 3.3 西班牙 (ES, +34)

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +34 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持**（$20/月） |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |

#### 监管机构

**CNMC**（Comision Nacional de los Mercados y la Competencia，国家市场和竞争委员会）— 西班牙电信和竞争监管机构。

**AEPD**（Agencia Espanola de Proteccion de Datos，西班牙数据保护局）— GDPR 执法机构。

#### 特有要求

- 西班牙 LSSI（信息社会服务法）补充 ePrivacy 指令
- 商业电子通信必须清楚标识为"publicidad"（广告）
- Lista Robinson — 西班牙反对列表，消费者可注册拒绝商业通信，建议发送前核查

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +34 6XX XXX XXX | +34 612 345 678 |
| 手机号 | +34 7XX XXX XXX | +34 712 345 678 |

- 手机号以 6 或 7 开头
- 总长度：9位（不含国家代码）
- AWS 格式：`+346XXXXXXXX` 或 `+347XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Movistar (Telefonica) | 最大运营商 |
| Vodafone Espana | 第二大 |
| Orange Espana | 第三大 |
| MasMovil/Yoigo | 第四大 |

#### 长码费用

- 月租：**$20 USD/月**
- 适合中低量发送场景
- 支持双向 SMS

---

### 3.4 奥地利 (AT, +43)

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +43 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持**（$27/月） |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.175/条** |

#### 监管机构

**RTR**（Rundfunk und Telekom Regulierungs-GmbH，广播电信监管有限公司）— 奥地利电信监管机构。

**DSB**（Datenschutzbehoerde，数据保护局）— GDPR 执法机构。

#### 特有要求

- 奥地利电信法（TKG 2021）补充 GDPR 和 ePrivacy
- 对未经同意的商业通信罚款严格
- 单价 $0.175/条在本合集中属于较高水平

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +43 6XX XXXXXXX | +43 664 1234567 |

- 手机号以 650、660、664、676、680、681、688、699 等开头
- AWS 格式：`+436XXXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| A1 Telekom Austria | 最大运营商 |
| T-Mobile Austria (Magenta) | 第二大 |
| Drei (Hutchison) | 第三大 |

#### 长码费用

- 月租：**$27 USD/月**（本合集中最高）
- 适合需要双向通信的场景

---

### 3.5 葡萄牙 (PT, +351)

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +351 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.018/条** |

#### 监管机构

**ANACOM**（Autoridade Nacional de Comunicacoes，国家通信管理局）— 葡萄牙电信监管机构。

**CNPD**（Comissao Nacional de Proteccao de Dados，国家数据保护委员会）— GDPR 执法机构。

#### 特有要求

- 葡萄牙电子通信法补充 ePrivacy
- 单价 $0.018/条，性价比较高
- Sender ID 无需注册，但建议使用可识别的品牌名称

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +351 9XX XXX XXX | +351 912 345 678 |

- 手机号以 91、92、93、96 开头
- 总长度：9位（不含国家代码）
- AWS 格式：`+3519XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| MEO (Altice Portugal) | 最大运营商 |
| NOS | 第二大 |
| Vodafone Portugal | 第三大 |

---

### 3.6 罗马尼亚 (RO, +40)

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +40 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.037/条** |

#### 监管机构

**ANCOM**（Autoritatea Nationala pentru Administrare si Reglementare in Comunicatii，国家通信管理和监管局）— 罗马尼亚电信监管机构。

**ANSPDCP**（国家个人数据处理监管局）— GDPR 执法机构。

#### 特有要求

- 罗马尼亚数据保护法 Law 190/2018 补充 GDPR
- A2P SMS 市场相对开放
- 单价 $0.037/条，中等水平

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +40 7XX XXX XXX | +40 721 123 456 |

- 手机号以 7 开头
- 总长度：9位（不含国家代码，去掉前导 0）
- AWS 格式：`+407XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Orange Romania | 最大运营商 |
| Vodafone Romania | 第二大 |
| Digi (RCS & RDS) | 第三大 |
| Telekom Romania | 第四大 |

---

### 3.7 芬兰 (FI, +358)

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +358 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.139/条** |

#### 监管机构

**Traficom**（Finnish Transport and Communications Agency，芬兰交通通信局）— 芬兰电信监管机构。

**Tietosuojavaltuutetun toimisto**（数据保护监察员办公室）— GDPR 执法机构。

#### 特有要求

- 芬兰信息社会法典（Information Society Code）补充 ePrivacy
- 芬兰是 AWS 控制台直接支持短码申请的国家之一
- 单价 $0.139/条，在本合集中属于较高水平
- 芬兰消费者对垃圾短信容忍度低

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +358 4X XXXXXXX | +358 40 1234567 |
| 手机号 | +358 50 XXXXXXX | +358 50 1234567 |

- 手机号以 40、41、42、43、44、45、46、50 开头
- 总长度：7-8位（不含国家代码，去掉前导 0）
- AWS 格式：`+3584XXXXXXXXX` 或 `+35850XXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Elisa | 最大运营商 |
| Telia Finland | 第二大 |
| DNA | 第三大 |

---

### 3.8 荷兰 (NL, +31)

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +31 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.681/条**（本合集最高） |

#### 监管机构

**ACM**（Autoriteit Consument & Markt，消费者和市场管理局）— 荷兰电信和竞争监管机构。

**Autoriteit Persoonsgegevens (AP)**（个人数据管理局）— GDPR 执法机构，以严格执法著称。

#### 特有要求

- **单价最高**：$0.681/条，大批量发送需严格控制成本
- **违规罚款极高**：荷兰数据保护局可处以最高 **90万欧元** 罚款（针对垃圾短信违规），或按 GDPR 上限处罚
- 荷兰电信法（Telecommunicatiewet）第 11.7 条严格规范未经同意的电子通信
- 荷兰是 AWS 控制台直接支持短码申请的国家之一
- 建议使用 GSM-7 编码以减少消息段数，降低高昂的单价影响

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +31 6 XXXX XXXX | +31 6 1234 5678 |

- 所有手机号以 6 开头
- 总长度：9位（不含国家代码，去掉前导 0）
- AWS 格式：`+316XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| KPN | 最大运营商 |
| Vodafone Ziggo | 第二大 |
| T-Mobile Netherlands | 第三大 |

#### 成本优化建议（荷兰特别重要）

由于单价高达 $0.681/条：
1. **严格使用 GSM-7 编码** — 避免 Unicode 导致消息拼接，每多一段就多 $0.681
2. **精简消息长度** — 力求单条 160 字符以内
3. **精确维护号码列表** — 清理无效号码，避免无效发送
4. **评估替代渠道** — 对非紧急通信考虑 WhatsApp Business API 或邮件

---

### 3.9 瑞典 (SE, +46)

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +46 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持**（$10/月） |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.01/条**（本合集最低） |

#### 监管机构

**PTS**（Post- och telestyrelsen，邮政和电信管理局）— 瑞典电信监管机构。

**IMY**（Integritetsskyddsmyndigheten，隐私保护局）— GDPR 执法机构。

#### 特有要求

- **单价最低**：$0.01/条，性价比在 9 国中最高
- 瑞典 LEK（电子通信法）补充 ePrivacy
- NIX-Telefon — 瑞典谢绝来电/短信登记系统，营销消息发送前建议核查
- 长码月租仅 $10/月，适合中小量发送

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +46 7X XXX XX XX | +46 70 123 45 67 |

- 手机号以 70、72、73、76 开头
- 总长度：9位（不含国家代码，去掉前导 0）
- AWS 格式：`+467XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Telia Sweden | 最大运营商 |
| Tele2 | 第二大 |
| Telenor Sweden | 第三大 |
| Tre (3) | 第四大 |

---

## 四、短码申请流程（通用）

### 4.1 控制台直接支持短码申请的国家

以下国家可在 AWS 控制台直接申请短码：**芬兰、德国、荷兰、西班牙**。

其他国家（意大利、奥地利、葡萄牙、罗马尼亚、瑞典）需通过 **AWS Support Case** 手动申请。

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
| Region | 你使用的 AWS 区域（如 eu-west-1） |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型（One Time Password / Promotional / Transactional） |
| Limit Increase Value | **1**（即申请 1 个专用短码） |

### 4.3 Case Description 模板（适用于所有 9 国）

```
Subject: Request for Dedicated Short Code - [Country Name] ([Country Code])

We are requesting a dedicated SMS short code for [Country Name] (+XX).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]
- VAT / Tax ID: [增值税号或公司识别号]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)

Expected Monthly Volume: [预估月发送量]

Sample Messages:
1. "[Brand] Your verification code is: 385291. Valid for 5 minutes.
   Do not share this code."
2. "[Brand] Your order #EU12345 has been confirmed. Amount: EUR XX.
   Info: https://brand.com/orders/EU12345"

Opt-in Process:
Users register on our website/app and provide their mobile number.
During registration, they check a consent box stating: "I agree to receive
SMS messages from [Brand] for account verification and transaction
notifications." (GDPR-compliant consent form with separate checkbox.)

Opt-out Mechanism:
Users can reply STOP to any message to unsubscribe immediately. They can
also opt out via their account settings or by contacting support@brand.com.

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
| 运营商审核 | 4-12周 |
| 短码配置完成 | 1-2周 |
| **总计** | **6-15周** |

---

## 五、各语言消息模板

### 5.1 字符编码通用说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本拉丁字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含特殊字符/非 GSM-7 字符 |

**重要：** 以下语言的特殊字符会触发 Unicode 编码，导致单条容量从 160 降至 70 字符。建议在非必要时去掉重音符号。

### 5.2 德语 (Deutsch) 模板

**GSM-7 兼容字符说明：** 德语 `a`、`o`、`u`（变音符号）和 `B`（eszett）**在 GSM-7 中支持**，无需替换。

**OTP 验证码：**
```
[Brand] Ihr Verifizierungscode lautet: 385291. Gueltig fuer 5 Minuten.
Teilen Sie diesen Code nicht.
```
(95字符，1条)

**订单确认：**
```
[Brand] Ihre Bestellung #DE12345 wurde bestaetigt. Betrag: EUR 49,99.
Info: https://brand.com/orders/DE12345
```
(102字符，1条)

**营销消息：**
```
[Brand] 20% Rabatt auf Ihren naechsten Einkauf! Code: PROMO20.
Gueltig bis 15.04. Abmelden: Antworten Sie STOP
```
(110字符，1条)

**Opt-in 确认：**
```
[Brand] Willkommen! Sie erhalten Benachrichtigungen per SMS.
Abmelden jederzeit: Antworten Sie STOP
```
(95字符，1条)

### 5.3 意大利语 (Italiano) 模板

**GSM-7 兼容说明：** 意大利语 `e`（在 GSM-7 中）但 `a`、`i`、`o`、`u` **不在 GSM-7 中**。建议去掉重音或接受 Unicode。

**OTP 验证码（GSM-7 兼容版）：**
```
[Brand] Il tuo codice di verifica e: 385291. Valido per 5 minuti.
Non condividere questo codice.
```
(93字符，1条)

**订单确认（GSM-7）：**
```
[Brand] Il tuo ordine #IT12345 e stato confermato. Importo: EUR 49,99.
Info: https://brand.com/orders/IT12345
```
(104字符，1条)

**营销消息（GSM-7）：**
```
[Brand] Sconto del 20% sul prossimo acquisto! Codice: PROMO20.
Valido fino al 15/04. Cancellati: Rispondi STOP
```
(112字符，1条)

### 5.4 西班牙语 (Espanol) 模板

**GSM-7 兼容说明：** `e` 和 `n` 在 GSM-7 中，但 `a`、`i`、`o`、`u` 不在。建议去掉重音。

**OTP 验证码（GSM-7）：**
```
[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos.
No comparta este codigo con nadie.
```
(97字符，1条)

**订单确认（GSM-7）：**
```
[Brand] Su pedido #ES12345 ha sido confirmado. Importe: EUR 49,99.
Info: https://brand.com/orders/ES12345
```
(101字符，1条)

**营销消息（GSM-7）：**
```
[Brand] 20% de descuento en su proxima compra. Use codigo PROMO20.
Valido hasta 15/04. Cancelar: Responda STOP
```
(111字符，1条)

### 5.5 葡萄牙语 (Portugues) 模板

**GSM-7 兼容说明：** 与西班牙语类似，`e` 在 GSM-7 中。`a`、`i`、`o`、`u`、`c`（c cedilha）、`a`/`o`（tilde）不在 GSM-7 中。建议去掉变音符号。

**OTP 验证码（GSM-7）：**
```
[Brand] O seu codigo de verificacao e: 385291. Valido por 5 minutos.
Nao partilhe este codigo.
```
(90字符，1条)

**订单确认（GSM-7）：**
```
[Brand] A sua encomenda #PT12345 foi confirmada. Valor: EUR 49,99.
Info: https://brand.com/orders/PT12345
```
(101字符，1条)

**营销消息（GSM-7）：**
```
[Brand] 20% de desconto na sua proxima compra. Use codigo PROMO20.
Valido ate 15/04. Cancelar: Responda STOP
```
(110字符，1条)

### 5.6 荷兰语 (Nederlands) 模板

**GSM-7 兼容说明：** 荷兰语通常不使用重音符号（偶尔用 trema 如 `e`），大部分文本 GSM-7 兼容。

**OTP 验证码：**
```
[Brand] Uw verificatiecode is: 385291. Geldig voor 5 minuten.
Deel deze code niet.
```
(79字符，1条)

**订单确认：**
```
[Brand] Uw bestelling #NL12345 is bevestigd. Bedrag: EUR 49,99.
Info: https://brand.com/orders/NL12345
```
(99字符，1条)

**营销消息：**
```
[Brand] 20% korting op uw volgende aankoop! Code: PROMO20.
Geldig t/m 15/04. Afmelden: Stuur STOP
```
(99字符，1条)

> **荷兰特别注意：** 由于单价 $0.681/条，每多一个消息段就多 $0.681。务必控制在 160 字符以内。

### 5.7 瑞典语 (Svenska) 模板

**GSM-7 兼容说明：** 瑞典语 `a`、`a`、`o` **在 GSM-7 扩展字符集中**，每个占 2 字节（即有效容量略减）。

**OTP 验证码：**
```
[Brand] Din verifieringskod ar: 385291. Giltig i 5 minuter.
Dela inte denna kod.
```
(78字符，1条)

**订单确认：**
```
[Brand] Din bestallning #SE12345 har bekraftats. Belopp: EUR 49,99.
Info: https://brand.com/orders/SE12345
```
(102字符，1条)

**营销消息：**
```
[Brand] 20% rabatt pa ditt nasta kop! Kod: PROMO20.
Giltigt t.o.m. 15/04. Avanmal: Svara STOP
```
(95字符，1条)

### 5.8 芬兰语 (Suomi) 模板

**GSM-7 兼容说明：** 芬兰语 `a` 和 `o` 同瑞典语，在 GSM-7 扩展字符集中。

**OTP 验证码：**
```
[Brand] Vahvistuskoodisi on: 385291. Voimassa 5 minuuttia.
Ala jaa tata koodia.
```
(78字符，1条)

**订单确认：**
```
[Brand] Tilauksesi #FI12345 on vahvistettu. Summa: EUR 49,99.
Info: https://brand.com/orders/FI12345
```
(96字符，1条)

**营销消息：**
```
[Brand] 20% alennus seuraavasta ostoksestasi! Koodi: PROMO20.
Voimassa 15.04. asti. Peruuta: Vastaa STOP
```
(105字符，1条)

### 5.9 罗马尼亚语 (Romana) 模板

**GSM-7 兼容说明：** 罗马尼亚语特殊字符 `a`（a breve）、`a`/`i`（带 cedilla）、`s`/`t`（带 cedilla）不在 GSM-7 中。建议替换为基本拉丁字符。

**OTP 验证码（GSM-7）：**
```
[Brand] Codul dvs. de verificare este: 385291. Valabil 5 minute.
Nu distribuiti acest cod.
```
(87字符，1条)

**订单确认（GSM-7）：**
```
[Brand] Comanda dvs. #RO12345 a fost confirmata. Suma: EUR 49,99.
Info: https://brand.com/orders/RO12345
```
(100字符，1条)

**营销消息（GSM-7）：**
```
[Brand] 20% reducere la urmatoarea achizitie! Cod: PROMO20.
Valabil pana la 15/04. Dezabonare: Raspundeti STOP
```
(113字符，1条)

---

## 六、定价对比

### 6.1 每条消息单价（从高到低）

| 排名 | 国家 | 单价 (USD/条) | 发送1万条成本 | 发送10万条成本 |
|------|------|-------------|-------------|--------------|
| 1 | 荷兰 NL | $0.681 | $6,810 | $68,100 |
| 2 | 奥地利 AT | $0.175 | $1,750 | $17,500 |
| 3 | 芬兰 FI | $0.139 | $1,390 | $13,900 |
| 4 | 罗马尼亚 RO | $0.037 | $370 | $3,700 |
| 5 | 葡萄牙 PT | $0.018 | $180 | $1,800 |
| 6 | 瑞典 SE | $0.01 | $100 | $1,000 |
| - | 德国 DE | 需确认 | - | - |
| - | 意大利 IT | 需确认 | - | - |
| - | 西班牙 ES | 需确认 | - | - |

> 德国、意大利、西班牙的具体单价请在 AWS 控制台或下载定价 CSV 确认。

### 6.2 长码月租费

| 国家 | 长码月租 (USD) |
|------|--------------|
| 奥地利 AT | $27/月 |
| 西班牙 ES | $20/月 |
| 瑞典 SE | $10/月 |
| 德国 DE | 需确认 |
| 意大利 IT | 需确认 |
| 葡萄牙 PT | 需确认 |
| 罗马尼亚 RO | 需确认 |
| 芬兰 FI | 需确认 |
| 荷兰 NL | 需确认 |

### 6.3 短码月租费（参考）

短码月租通常较高，各国不同。以下为参考范围：

| 项目 | 参考值 |
|------|-------|
| 短码月租范围 | $800-$1,500 USD/月（因国家而异） |
| 一次性设置费 | 可能有（需确认） |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

### 6.4 成本优化建议

1. **使用 GSM-7 编码** — 每条 160 字符 vs Unicode 的 70 字符，可减少约 50% 消息段数
2. **荷兰发送特别注意** — 单价 $0.681/条，Unicode 拼接将导致成本爆炸
3. **精简消息内容** — 保持单条消息在 160 字符内避免拼接
4. **维护号码列表** — 定期清理无效号码
5. **选择瑞典作为测试市场** — 单价仅 $0.01/条，适合初期测试
6. **批量发送** — 利用 AWS 批量 API 提高效率

---

## 七、合规清单

在本合集任何国家发送 SMS 前，确保完成以下所有项目：

### 7.1 GDPR / ePrivacy 合规

- [ ] 获取所有接收者的明确 opt-in 同意（预勾选复选框无效）
- [ ] 同意记录包含时间、方式、内容，并安全保存
- [ ] 营销消息和事务性消息分别获得同意
- [ ] 实施有效的 opt-out 机制（STOP 关键词 + 账户设置）
- [ ] 每条营销消息包含退订指引
- [ ] 退订请求即时处理
- [ ] 隐私政策说明 SMS 数据处理方式
- [ ] 数据保护影响评估（如大规模处理）
- [ ] 数据泄露响应计划（72小时通知义务）
- [ ] 跨境数据传输合规（SCCs 或充分性决定）

### 7.2 内容与发送

- [ ] 消息使用目标国家当地语言
- [ ] 消息内容合法、不含欺诈/误导内容
- [ ] 营销消息清楚标识为广告
- [ ] 发送时间限制在当地工作时间（09:00-20:00）
- [ ] 避免周日和法定假日发送营销消息
- [ ] 使用品牌自有域名短链接（避免 bit.ly 等）
- [ ] 使用 GSM-7 编码以优化成本

### 7.3 号码与技术

- [ ] 正确格式化号码（E.164 格式：+国家代码+号码）
- [ ] 使用一致的品牌 Sender ID
- [ ] 测试各运营商的 Sender ID 显示效果
- [ ] 申请提高默认消费阈值（$1.00 USD → 实际需要值）
- [ ] 如需短码，提前 6-15 周申请

### 7.4 各国特殊清单

- [ ] **德国**：遵守 BDSG 补充要求，准备 USt-IdNr
- [ ] **西班牙**：核查 Lista Robinson（反对列表）
- [ ] **荷兰**：注意 $0.681/条的高单价，控制消息段数
- [ ] **瑞典**：核查 NIX-Telefon（谢绝来电/短信登记）
- [ ] **意大利**：核查 Registro delle Opposizioni（反对登记簿）

---

## 八、风险评估

| 风险 | 涉及国家 | 严重程度 | 缓解措施 |
|------|---------|---------|---------|
| GDPR 违规处罚 | 全部 9 国 | **极高** | 严格遵循同意流程，保留完整记录，咨询当地法律顾问 |
| 荷兰高单价导致成本超支 | NL | **高** | 使用 GSM-7 编码，精简消息长度，考虑替代渠道 |
| 荷兰垃圾短信罚款（90万欧元） | NL | **高** | 严格 opt-in/opt-out，定期审计号码列表 |
| 短码申请被拒 | 全部 9 国 | **中** | 准备详尽使用说明，确保内容合规 |
| 短码审批等待期（6-15周） | 全部 9 国 | **中** | 提前规划，尽早提交申请 |
| 运营商内容过滤 | 全部 9 国 | **中** | 使用标准模板，品牌域名，避免可疑内容 |
| Sender ID 被运营商覆写 | 全部 9 国 | **低** | 小批量测试确认显示效果 |
| Unicode 编码导致成本翻倍 | 全部 9 国 | **中** | 统一使用 GSM-7 兼容写法 |
| 各国反对列表（Robinson/NIX） | ES, SE, IT | **中** | 发送前核查相关列表，维护排除名单 |
| 奥地利高长码月租 | AT | **低** | 评估是否需要长码，或使用 Sender ID 替代 |
| 芬兰消费者低容忍度 | FI | **中** | 严格控制发送频率，仅发送高价值消息 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。各国法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询当地法律顾问以确保合规。
