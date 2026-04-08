# AWS SMS 欧洲部分支持国家合集报告

> 调研日期：2026年4月7日
> 覆盖国家：10个（法国、比利时、挪威、丹麦、波兰、捷克、匈牙利、希腊、保加利亚、瑞士）
> 数据来源：AWS End User Messaging SMS 官方文档、各国电信监管机构资料、GDPR/ePrivacy 法规

---

## 一、通用合规要求（GDPR / ePrivacy / FADP）

本合集覆盖的 10 个国家中，9 个为欧盟/EEA 成员国（法国、比利时、丹麦、波兰、捷克、匈牙利、希腊、保加利亚、挪威），适用 **GDPR** 和 **ePrivacy 指令**。瑞士虽非 EU 成员，但其 **FADP/nDSG**（新联邦数据保护法）与 GDPR 高度一致。

### 1.1 GDPR 核心要求

| 要求 | 详情 |
|------|------|
| **合法基础** | 营销 SMS 必须基于用户明确同意（Article 6(1)(a)）；事务性 SMS 可基于合同履行（Article 6(1)(b)） |
| **明确同意** | 同意必须自由给出、具体、知情、明确；预勾选复选框无效 |
| **撤回权** | 用户必须能随时撤回同意，且撤回方式应与给予同意一样简便 |
| **数据最小化** | 仅收集发送 SMS 所必需的数据 |
| **数据访问权** | 用户有权请求获取其个人数据副本（30天内响应） |
| **删除权（被遗忘权）** | 用户有权要求删除其个人数据 |
| **数据泄露通知** | 72小时内向监管机构报告数据泄露 |
| **跨境传输** | 向 EU/EEA 以外传输数据需确保充分保护 |

### 1.2 瑞士 FADP/nDSG 特殊说明

| 项目 | GDPR | 瑞士 FADP/nDSG |
|------|------|---------------|
| 适用范围 | EU/EEA 居民数据 | 瑞士居民数据 |
| 监管机构 | 各国 DPA | FDPIC（联邦数据保护和信息专员） |
| 同意要求 | 明确同意 | 明确同意（与 GDPR 基本一致） |
| 违规处罚 | 最高 2,000万欧元或年营业额4% | 最高 25万瑞士法郎（针对个人） |
| 数据泄露通知 | 72小时 | "尽快"通知 |

### 1.3 GDPR 违规处罚

| 违规类型 | 最高罚款 |
|---------|---------|
| 一般违规 | 1,000万欧元或全球年营业额 2%（取较高者） |
| 严重违规 | 2,000万欧元或全球年营业额 4%（取较高者） |

### 1.4 通用 Opt-in / Opt-out 要求

**Opt-in（用户同意）：**
1. 必须获得明确的、知情的同意
2. 同意记录必须保留（时间、方式、内容）
3. 预勾选复选框不构成有效同意
4. 双重确认（Double Opt-in）为最佳实践

**Opt-out（退订）：**
1. 支持双向 SMS 的国家（法国、比利时、挪威、丹麦、波兰、捷克、匈牙利、希腊、保加利亚）可使用 STOP 关键词
2. **瑞士不支持双向 SMS**，必须提供替代退出方式（网页链接、邮箱等）
3. 每条营销消息必须包含退订指引

---

## 二、10国能力对比表

| 国家 | 代码 | 国际代码 | 短码 | 长码 | Sender ID | 双向 SMS | 国际发送 | 核心限制 | 单价 (USD/条) |
|------|------|---------|------|------|-----------|---------|---------|---------|--------------|
| 法国 | FR | +33 | Yes | **No** | Yes（有限制） | Yes | Yes | Sender ID 不可含破折号，不支持长码 | 需确认 |
| 比利时 | BE | +32 | Yes | Yes | **No** | Yes | Yes | **不支持 Sender ID** | $0.098 |
| 挪威 | NO | +47 | **No** | Yes | Yes | Yes | Yes | **不支持短码**，EEA 成员 | $0.024 |
| 丹麦 | DK | +45 | **No** | Yes ($10/月) | Yes | Yes | Yes | **不支持短码**，Robinson 列表 | $0.039 |
| 波兰 | PL | +48 | **No** | Yes | Yes | Yes | Yes | **不支持短码** | $0.016 |
| 捷克 | CZ | +420 | **No** | Yes | Yes | Yes | Yes | **不支持短码** | $0.05 |
| 匈牙利 | HU | +36 | **No** | Yes | Yes | Yes | Yes | **不支持短码** | $0.014 |
| 希腊 | GR | +30 | **No** | Yes | Yes | Yes | **No** | **不支持短码，不支持国际发送** | $0.088 |
| 保加利亚 | BG | +359 | Yes | **No** | Yes | Yes | Yes | **不支持长码** | $0.212 |
| 瑞士 | CH | +41 | **No** | **No** | Yes | **No** | Yes | **仅 Sender ID，不支持双向** | $0.028 |

### 限制程度分级

| 级别 | 国家 | 说明 |
|------|------|------|
| **严重受限** | 瑞士 CH | 仅 Sender ID，无短码/长码/双向 |
| **严重受限** | 希腊 GR | 不支持国际发送（唯一），无短码 |
| **中度受限** | 法国 FR | 不支持长码，Sender ID 格式限制 |
| **中度受限** | 保加利亚 BG | 不支持长码 |
| **中度受限** | 比利时 BE | 不支持 Sender ID |
| **轻度受限** | 挪威 NO / 丹麦 DK / 波兰 PL / 捷克 CZ / 匈牙利 HU | 不支持短码 |

---

## 三、费用总览

### 3.1 号码费用对比

| 国家 | 号码类型 | 月度费用 | 预置时间 |
|------|---------|---------|---------|
| DK | 长码 | $10 | 1周 |
| NO | 长码 | $30 | 2周 |
| PL | 长码 | $15 | 1周 |
| HU | 长码 | $60 | 1周 |
| FR | Sender ID | 免费 | — |
| BE | Sender ID | 免费 | — |
| CZ | Sender ID | 免费 | — |
| GR | Sender ID | 免费 | — |
| BG | Sender ID | 免费 | — |
| CH | Sender ID | 免费 | — |

### 3.2 每条消息费用对比

| 国家 | 每条消息价格 (USD) |
|------|-------------------|
| FR | $0.073 |
| BE | $0.09582 |
| NO | $0.08675 |
| DK | $0.06206 |
| PL | $0.03222 |
| CZ | $0.06609 |
| HU | $0.10824 |
| GR | $0.08621 |
| BG | $0.136 |
| CH | $0.05124 |

### 3.3 月度消费阈值

所有国家新账户默认 $1.00 USD/月，需申请提高。

---

## 四、各国详细说明

---

### 4.1 法国 (FR, +33) — Sender ID 不可含破折号，不支持长码

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +33 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **不支持** |
| Sender ID | **支持（有限制）** |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |

#### ⚠ 关键限制

1. **Sender ID 不可含破折号**（2026年3月1日已生效）
   - 仅允许字母数字字符：a-z、A-Z、0-9
   - 不允许破折号（-）、空格、特殊字符
   - 已有包含破折号的 Sender ID 须立即更新
   - 示例：`My-Brand` → 改为 `MyBrand` 或 `MYBRAND`

2. **不支持长码**
   - 只能通过短码或 Sender ID 发送
   - 如需双向 SMS，必须使用短码

#### 监管机构

**ARCEP**（Autorite de Regulation des Communications Electroniques, des Postes et de la Distribution de la Presse）— 法国电子通信、邮政和出版物分发监管局。

**CNIL**（Commission Nationale de l'Informatique et des Libertes）— 法国数据保护局，GDPR 执法机构。以严格执法著称。

#### 特有要求

- 法国《邮政和电子通信法典》补充 ePrivacy
- CNIL 对电子直接营销执法非常活跃，多次开出高额罚单
- 建议仅在工作时间发送营销消息
- Bloctel — 法国反电话营销列表，建议核查

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +33 6 XX XX XX XX | +33 6 12 34 56 78 |
| 手机号 | +33 7 XX XX XX XX | +33 7 12 34 56 78 |

- 手机号以 6 或 7 开头
- 总长度：9位（不含国家代码，去掉前导 0）
- AWS 格式：`+336XXXXXXXX` 或 `+337XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Orange | 最大运营商 |
| SFR | 第二大 |
| Bouygues Telecom | 第三大 |
| Free Mobile | 低价竞争者 |

---

### 4.2 比利时 (BE, +32) — 不支持 Sender ID

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +32 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **不支持** |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.098/条** |

#### ⚠ 关键限制

- **不支持 Sender ID** — 消息将以号码形式显示，无法显示品牌名称
- 必须使用短码或长码发送
- 品牌识别只能通过消息内容体现

#### 监管机构

**BIPT**（Belgisch Instituut voor Postdiensten en Telecommunicatie / Institut Belge des Services Postaux et des Telecommunications）— 比利时邮政电信监管机构。

**APD/GBA**（Autorite de Protection des Donnees / Gegevensbeschermingsautoriteit）— 比利时数据保护局。

#### 特有要求

- 比利时是多语言国家（荷兰语、法语、德语），消息语言需根据目标受众选择
- 比利时电子通信法补充 ePrivacy
- Do Not Call 列表需核查

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +32 4XX XX XX XX | +32 470 12 34 56 |

- 手机号以 4 开头
- 总长度：9位（不含国家代码，去掉前导 0）
- AWS 格式：`+324XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Proximus | 最大运营商 |
| Orange Belgium | 第二大 |
| Base (Telenet) | 第三大 |

---

### 4.3 挪威 (NO, +47) — 不支持短码，EEA 成员

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +47 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.024/条** |

#### ⚠ 关键限制

- **不支持短码** — 只能通过长码或 Sender ID 发送
- 如需高吞吐量发送，需依赖长码或多个 Sender ID

#### 监管机构

**Nkom**（Nasjonal kommunikasjonsmyndighet，国家通信管理局）— 挪威电信监管机构。

**Datatilsynet**（数据保护局）— GDPR 执法机构。

#### 特有要求

- 挪威为 EEA 成员国，**完全适用 GDPR**（通过 EEA 协议）
- 挪威电子通信法补充 ePrivacy
- Reservasjonsregisteret — 挪威反直接营销登记系统，营销消息发送前必须核查
- 单价 $0.024/条，性价比较高

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +47 4XX XX XXX | +47 412 34 567 |
| 手机号 | +47 9XX XX XXX | +47 912 34 567 |

- 手机号以 4 或 9 开头
- 总长度：8位（不含国家代码）
- 挪威号码**无前导 0**
- AWS 格式：`+47XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Telenor | 最大运营商 |
| Telia Norway | 第二大 |
| Ice | 第三大 |

---

### 4.4 丹麦 (DK, +45) — 不支持短码，Robinson 列表

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +45 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **支持**（$10/月） |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.039/条** |

#### ⚠ 关键限制

- **不支持短码** — 只能通过长码或 Sender ID 发送
- **Robinson 列表** — 丹麦全国性退出机制（Robinsonlisten），消费者可注册拒绝商业通信。**营销消息发送前必须核查此列表。**

#### 监管机构

**DBA**（Danish Business Authority，丹麦商业管理局）— 负责电信监管。

**Datatilsynet**（数据保护局）— GDPR 执法机构。

#### 特有要求

- 丹麦营销法（Markedsfoeringsloven）补充 ePrivacy
- Robinson 列表由丹麦消费者监察员管理
- 违反 Robinson 列表要求可能导致罚款和法律后果

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +45 XX XX XX XX | +45 20 12 34 56 |

- 手机号以 2、3、4、5、6、7、8、9 开头（取决于运营商）
- 总长度：8位（不含国家代码）
- 丹麦号码**无前导 0、无区号**
- AWS 格式：`+45XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| TDC (Nuuday) | 最大运营商 |
| Telenor Denmark | 第二大 |
| Telia Denmark | 第三大 |
| 3 (Hi3G) | 第四大 |

#### 长码费用

- 月租：**$10 USD/月**

---

### 4.5 波兰 (PL, +48) — 不支持短码

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +48 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.016/条** |

#### ⚠ 关键限制

- **不支持短码** — 只能通过长码或 Sender ID 发送

#### 监管机构

**UKE**（Urzad Komunikacji Elektronicznej，电子通信办公室）— 波兰电信监管机构。

**UODO**（Urzad Ochrony Danych Osobowych，个人数据保护办公室）— GDPR 执法机构。

#### 特有要求

- 波兰电信法（Prawo telekomunikacyjne）补充 ePrivacy
- 波兰消费者权益保护积极
- 单价 $0.016/条，性价比极高

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +48 XX XXX XX XX | +48 50 123 45 67 |

- 手机号以 50、51、53、57、60、66、69、72、73、78、79、88 开头
- 总长度：9位（不含国家代码）
- AWS 格式：`+48XXXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Orange Polska | 最大运营商 |
| Play (P4) | 第二大 |
| T-Mobile Polska | 第三大 |
| Plus (Polkomtel) | 第四大 |

---

### 4.6 捷克 (CZ, +420) — 不支持短码

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +420 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.05/条** |

#### ⚠ 关键限制

- **不支持短码** — 只能通过长码或 Sender ID 发送

#### 监管机构

**CTU**（Cesky telekomunikacni urad，捷克电信办公室）— 捷克电信监管机构。

**UOOU**（Urad pro ochranu osobnich udaju，个人数据保护办公室）— GDPR 执法机构。

#### 特有要求

- 捷克电子通信法补充 ePrivacy
- 捷克消费者保护法对商业通信有额外要求

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +420 XXX XXX XXX | +420 601 123 456 |

- 手机号以 601-608、702-705、720-739、770-779 开头
- 总长度：9位（不含国家代码）
- 捷克号码**无前导 0**
- AWS 格式：`+420XXXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| O2 Czech Republic | 最大运营商 |
| T-Mobile Czech Republic | 第二大 |
| Vodafone Czech Republic | 第三大 |

---

### 4.7 匈牙利 (HU, +36) — 不支持短码

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +36 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（需确认） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.014/条**（本合集最低之一） |

#### ⚠ 关键限制

- **不支持短码** — 只能通过长码或 Sender ID 发送
- Sender ID 支持状态需确认（AWS 文档信息略有差异，部分来源显示不支持）

#### 监管机构

**NMHH**（Nemzeti Media- es Hirkozlesi Hatosag，国家媒体和通信管理局）— 匈牙利电信监管机构。

**NAIH**（Nemzeti Adatvedelmi es Informacioszabadsag Hatosag，国家数据保护和信息自由局）— GDPR 执法机构。

#### 特有要求

- 匈牙利电子通信法补充 ePrivacy
- 单价 $0.014/条，是本合集中最便宜的国家之一
- 匈牙利消费者保护法对商业通信有限制

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +36 XX XXX XXXX | +36 20 123 4567 |

- 手机号以 20、30、31、50、70 开头
- 总长度：9位（不含国家代码，去掉前导 06）
- AWS 格式：`+36XXXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Magyar Telekom (T-Mobile) | 最大运营商 |
| Telenor Hungary (Yettel) | 第二大 |
| Vodafone Hungary (One) | 第三大 |

---

### 4.8 希腊 (GR, +30) — 不支持短码，不支持国际发送

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +30 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | **不支持** |
| 单价 | **$0.088/条** |

#### ⚠ 关键限制（本合集中限制最多的 EU 国家之一）

1. **不支持短码** — 只能通过长码或 Sender ID 发送
2. **不支持国际发送** — **希腊是所有 AWS 支持国家中唯一不支持国际发送的国家**。这意味着：
   - 无法从海外直接向希腊号码发送 SMS
   - 可能需要通过本地路由或替代方案
   - 实际投递能力需通过测试验证

#### 监管机构

**EETT**（Ethniki Epitropi Tilepikoinonion kai Tachydromeion，国家电信和邮政委员会）— 希腊电信监管机构。

**HDPA**（Hellenic Data Protection Authority）— GDPR 执法机构。

#### 特有要求

- 希腊电子通信法 4070/2012 补充 ePrivacy
- 希腊反垃圾通信法较严格
- 由于不支持国际发送，建议提前充分测试投递能力

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +30 6XX XXX XXXX | +30 694 123 4567 |

- 手机号以 69 开头
- 总长度：10位（不含国家代码）
- AWS 格式：`+306XXXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Cosmote (OTE) | 最大运营商 |
| Vodafone Greece | 第二大 |
| Wind Hellas (Nova) | 第三大 |

---

### 4.9 保加利亚 (BG, +359) — 不支持长码

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +359 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **不支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **支持** |
| 国际发送 | 支持 |
| 单价 | **$0.212/条**（本合集最高） |

#### ⚠ 关键限制

- **不支持长码** — 只能通过短码或 Sender ID 发送
- **单价最高**：$0.212/条，在本合集中为最高

#### 监管机构

**CRC**（Communications Regulation Commission，通信监管委员会）— 保加利亚电信监管机构。

**KZLD**（Commission for Personal Data Protection）— GDPR 执法机构。

#### 特有要求

- 保加利亚电子通信法补充 ePrivacy
- 保加利亚个人数据保护法补充 GDPR
- 单价 $0.212/条较高，需注意成本控制

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +359 XX XXX XXXX | +359 87 123 4567 |
| 手机号 | +359 XX XXX XXXX | +359 88 123 4567 |

- 手机号以 87、88、89、98、99 开头
- 总长度：8-9位（不含国家代码，去掉前导 0）
- AWS 格式：`+359XXXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| A1 Bulgaria | 最大运营商 |
| Yettel Bulgaria | 第二大 |
| Vivacom | 第三大 |

---

### 4.10 瑞士 (CH, +41) — 仅支持 Sender ID，不支持双向

#### 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +41 |
| 短码 (Short Code) | **不支持** |
| 长码 (Long Code) | **不支持** |
| Sender ID | **支持**（无需注册） |
| 双向 SMS | **不支持** |
| 国际发送 | 支持 |
| 单价 | **$0.028/条** |

#### ⚠ 关键限制（本合集中限制最严重的国家）

1. **仅支持 Sender ID** — 不支持短码和长码
2. **不支持双向 SMS** — 用户无法回复 STOP 退订
3. **非 EU 成员** — 不适用 GDPR，但 FADP/nDSG 与 GDPR 高度一致

#### 因不支持双向 SMS 的替代退出方案

由于用户无法回复 STOP 退订，必须提供替代 opt-out 机制：

**方案一：短链接退出（推荐）**
```
[Brand] Ihr Code: 385291. Gueltig 5 Min. SMS abmelden: https://brand.com/unsub/abc123
```

**方案二：邮件退出**
```
SMS abmelden: E-Mail an optout@brand.com mit Ihrer Telefonnummer.
```

**方案三：账户设置退出**

在 App/网站设置中提供 SMS 偏好管理页面。

#### 监管机构

**OFCOM/BAKOM**（Federal Office of Communications / Bundesamt fuer Kommunikation）— 瑞士联邦通信办公室。

**FDPIC**（Federal Data Protection and Information Commissioner）— 联邦数据保护和信息专员。

#### 特有要求

- 瑞士 FADP/nDSG（2023年9月1日生效）与 GDPR 高度一致
- 瑞士电信法（FMG/LTC）规范电子通信
- 反不正当竞争法（UWG/LCD）禁止未经同意的商业通信
- 瑞士被 EU 认定为具有"充分"数据保护水平
- 单价 $0.028/条，性价比尚可

#### 号码格式

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +41 7X XXX XX XX | +41 76 123 45 67 |
| 手机号 | +41 7X XXX XX XX | +41 78 123 45 67 |

- 手机号以 76、77、78、79 开头
- 总长度：9位（不含国家代码，去掉前导 0）
- AWS 格式：`+417XXXXXXXX`

#### 主要运营商

| 运营商 | 备注 |
|--------|------|
| Swisscom | 最大运营商 |
| Sunrise | 第二大 |
| Salt Mobile | 第三大 |

---

## 五、短码/长码申请流程

### 5.1 需要短码的国家

本合集中支持短码的国家：**法国、比利时、保加利亚**。

均需通过 **AWS Support Case** 申请。

### 5.2 Support Case 填写指南（短码）

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

### 5.3 需要长码的国家

本合集中支持长码的国家：**比利时、挪威、丹麦、波兰、捷克、匈牙利、希腊**。

长码通常可在 AWS 控制台直接购买，无需 Support Case。

### 5.4 Support Case 填写指南（长码，如需手动申请）

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Severity | General Limits |
| Region | 你使用的 AWS 区域 |
| Resource Type | Dedicated SMS Long Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

### 5.5 Case Description 模板

```
Subject: Request for Dedicated [Short Code/Long Code] - [Country Name] ([Country Code])

We are requesting a dedicated SMS [short code/long code] for [Country Name] (+XX).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]
- VAT / Tax ID: [增值税号或公司识别号]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications

Expected Monthly Volume: [预估月发送量]

Sample Messages:
1. "[Brand] Your verification code is: 385291. Valid for 5 minutes."
2. "[Brand] Your order #EU12345 has been confirmed."

Opt-in Process:
GDPR-compliant consent form with separate checkbox for SMS communications.

Opt-out Mechanism:
[支持双向的国家] Users can reply STOP to unsubscribe.
[瑞士] Users can opt out via https://brand.com/unsub or support@brand.com.

We request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.
```

---

## 六、各语言消息模板

### 6.1 字符编码通用说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本拉丁字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含特殊字符 |

### 6.2 法语 (Francais) 模板

**GSM-7 兼容说明：** 法语 `e`（在 GSM-7 中），但 `a`、`e`（grave）、`e`（circumflex）、`i`、`o`、`u`、`u`（grave）、`c` 不在 GSM-7 中。建议去掉重音。

**OTP 验证码（GSM-7）：**
```
[Brand] Votre code de verification est: 385291. Valide 5 minutes.
Ne partagez pas ce code.
```
(87字符，1条)

**订单确认（GSM-7）：**
```
[Brand] Votre commande #FR12345 est confirmee. Montant: EUR 49,99.
Info: https://brand.com/orders/FR12345
```
(103字符，1条)

**营销消息（GSM-7）：**
```
[Brand] 20% de reduction sur votre prochain achat! Code: PROMO20.
Valable jusqu'au 15/04. Desabonnement: Repondez STOP
```
(117字符，1条)

### 6.3 比利时消息模板（荷兰语/法语）

比利时为多语言国家，需根据目标受众选择语言。

**荷兰语 OTP：**
```
[Brand] Uw verificatiecode is: 385291. Geldig voor 5 minuten.
Deel deze code niet.
```
(79字符，1条)

**法语 OTP：**
```
[Brand] Votre code de verification est: 385291. Valide 5 minutes.
Ne partagez pas ce code.
```
(87字符，1条)

### 6.4 挪威语 (Norsk) 模板

**GSM-7 兼容说明：** 挪威语 `ae`、`o`、`a` 在 GSM-7 扩展字符集中（每个占 2 字节）。

**OTP 验证码：**
```
[Brand] Din bekreftelseskode er: 385291. Gyldig i 5 minutter.
Ikke del denne koden.
```
(80字符，1条)

**订单确认：**
```
[Brand] Din bestilling #NO12345 er bekreftet. Belop: EUR 49,99.
Info: https://brand.com/orders/NO12345
```
(99字符，1条)

**营销消息：**
```
[Brand] 20% rabatt pa ditt neste kjop! Kode: PROMO20.
Gyldig t.o.m. 15/04. Avmeld: Svar STOP
```
(94字符，1条)

### 6.5 丹麦语 (Dansk) 模板

**GSM-7 兼容说明：** 丹麦语 `ae`、`o`、`a` 在 GSM-7 扩展字符集中。

**OTP 验证码：**
```
[Brand] Din bekraeftelseskode er: 385291. Gyldig i 5 minutter.
Del ikke denne kode.
```
(80字符，1条)

**订单确认：**
```
[Brand] Din ordre #DK12345 er bekraeftet. Belob: EUR 49,99.
Info: https://brand.com/orders/DK12345
```
(96字符，1条)

**营销消息：**
```
[Brand] 20% rabat pa dit naeste kob! Kode: PROMO20.
Gyldig t.o.m. 15/04. Afmeld: Svar STOP
```
(93字符，1条)

### 6.6 波兰语 (Polski) 模板

**GSM-7 兼容说明：** 波兰语特殊字符 `a`、`c`、`e`、`l`、`n`、`o`、`s`、`z`、`z` 不在 GSM-7 中。建议替换为基本拉丁字符。

**OTP 验证码（GSM-7）：**
```
[Brand] Twoj kod weryfikacyjny to: 385291. Wazny przez 5 minut.
Nie udostepniaj tego kodu.
```
(86字符，1条)

**订单确认（GSM-7）：**
```
[Brand] Twoje zamowienie #PL12345 zostalo potwierdzone. Kwota: EUR 49,99.
Info: https://brand.com/orders/PL12345
```
(108字符，1条)

**营销消息（GSM-7）：**
```
[Brand] 20% znizki na nastepny zakup! Kod: PROMO20.
Wazne do 15/04. Rezygnacja: Odpowiedz STOP
```
(98字符，1条)

### 6.7 捷克语 (Cestina) 模板

**GSM-7 兼容说明：** 捷克语带钩号/长音字符（`c`、`r`、`s`、`z`、`a`、`e`、`i`、`o`、`u`、`y`、`d`、`t`、`n`、`u`）不在 GSM-7 中。建议去掉变音符号。

**OTP 验证码（GSM-7）：**
```
[Brand] Vas overovaci kod je: 385291. Platny 5 minut.
Nesdilejte tento kod.
```
(72字符，1条)

**订单确认（GSM-7）：**
```
[Brand] Vase objednavka #CZ12345 byla potvrzena. Castka: EUR 49,99.
Info: https://brand.com/orders/CZ12345
```
(103字符，1条)

**营销消息（GSM-7）：**
```
[Brand] 20% sleva na dalsi nakup! Kod: PROMO20.
Platne do 15/04. Odhlasit: Odpovezte STOP
```
(94字符，1条)

### 6.8 匈牙利语 (Magyar) 模板

**GSM-7 兼容说明：** 匈牙利语长元音（`a`、`e`、`i`、`o`、`o`、`u`、`u`）不在 GSM-7 中。建议替换为短元音。

**OTP 验证码（GSM-7）：**
```
[Brand] Az On ellenorzo kodja: 385291. Ervenyes 5 percig.
Ne ossza meg ezt a kodot.
```
(82字符，1条)

**订单确认（GSM-7）：**
```
[Brand] Az On rendelese #HU12345 visszaigazolva. Osszeg: EUR 49,99.
Info: https://brand.com/orders/HU12345
```
(103字符，1条)

**营销消息（GSM-7）：**
```
[Brand] 20% kedvezmeny a kovetkezo vasarlasra! Kod: PROMO20.
Ervenyes 04.15-ig. Leiratkozas: Valaszoljon STOP
```
(112字符，1条)

### 6.9 希腊语 (Ellinika) 模板

**GSM-7 兼容说明：** 希腊字母在 GSM-7 中有部分支持（大写希腊字母的子集），但完整的现代希腊语（含小写和重音）会触发 Unicode。**建议使用拉丁字母转写（Greeklish）来保持 GSM-7 兼容，或接受 Unicode 70 字符限制。**

**拉丁转写版 OTP（GSM-7，推荐）：**
```
[Brand] O kodikos epivevaiwsis sas einai: 385291. Isxyei gia 5 lepta.
Min moirastite auton ton kodiko.
```
(98字符，1条)

**原生希腊语 OTP（Unicode，70字符/条）：**
```
[Brand] O kodikos epivevaiwsis sas einai: 385291. Isxyei 5 lepta.
```
(注意：使用希腊字母版本将按 Unicode 计费)

### 6.10 保加利亚语 (Balgarski) 模板

**GSM-7 兼容说明：** 保加利亚语使用西里尔字母，**不在 GSM-7 中**，将触发 Unicode（70字符/条）。建议使用拉丁转写以节省成本。

**拉丁转写版 OTP（GSM-7，推荐）：**
```
[Brand] Vashiyat kod za verifikatsiya e: 385291. Validen 5 minuti.
Ne spodelyayte tozi kod.
```
(89字符，1条)

**原生保加利亚语 OTP（Unicode，70字符/条）：**
```
[Brand] Vashiyat kod e: 385291. Validen 5 min.
```
(注意：使用西里尔字母版本将按 Unicode 计费，成本约为拉丁版的 2 倍)

### 6.11 瑞士消息模板（德语/法语/意大利语）

瑞士为多语言国家，需根据目标受众语言区域选择。

**德语 OTP（含退出链接，因不支持 STOP）：**
```
[Brand] Ihr Code: 385291. Gueltig 5 Min.
Abmelden: https://brand.com/unsub/abc123
```
(79字符，1条)

**法语 OTP（含退出链接）：**
```
[Brand] Votre code: 385291. Valide 5 min.
Desabonnement: https://brand.com/unsub/abc123
```
(86字符，1条)

**意大利语 OTP（含退出链接）：**
```
[Brand] Il tuo codice: 385291. Valido 5 min.
Cancellati: https://brand.com/unsub/abc123
```
(84字符，1条)

> **瑞士特别注意：** 由于不支持双向 SMS，营销消息必须包含网页退出链接。

---

## 七、定价对比

### 7.1 每条消息单价（从高到低）

| 排名 | 国家 | 单价 (USD/条) | 发送1万条成本 | 发送10万条成本 | 核心限制 |
|------|------|-------------|-------------|--------------|---------|
| 1 | 保加利亚 BG | $0.212 | $2,120 | $21,200 | 不支持长码 |
| 2 | 比利时 BE | $0.098 | $980 | $9,800 | 不支持 Sender ID |
| 3 | 希腊 GR | $0.088 | $880 | $8,800 | 不支持国际发送 |
| 4 | 捷克 CZ | $0.05 | $500 | $5,000 | 不支持短码 |
| 5 | 丹麦 DK | $0.039 | $390 | $3,900 | 不支持短码 |
| 6 | 瑞士 CH | $0.028 | $280 | $2,800 | 仅 Sender ID |
| 7 | 挪威 NO | $0.024 | $240 | $2,400 | 不支持短码 |
| 8 | 波兰 PL | $0.016 | $160 | $1,600 | 不支持短码 |
| 9 | 匈牙利 HU | $0.014 | $140 | $1,400 | 不支持短码 |
| - | 法国 FR | 需确认 | - | - | Sender ID 限制 |

> 法国的具体单价请在 AWS 控制台或下载定价 CSV 确认。

### 7.2 长码月租费

| 国家 | 长码月租 (USD) | 备注 |
|------|--------------|------|
| 丹麦 DK | $10/月 | 性价比较高 |
| 比利时 BE | 需确认 | - |
| 挪威 NO | 需确认 | - |
| 波兰 PL | 需确认 | - |
| 捷克 CZ | 需确认 | - |
| 匈牙利 HU | 需确认 | - |
| 希腊 GR | 需确认 | - |

### 7.3 成本优化建议

1. **使用 GSM-7 编码** — 特别注意保加利亚语（西里尔字母）和希腊语会触发 Unicode
2. **保加利亚和希腊** — 建议使用拉丁转写以保持 GSM-7 兼容，降低成本
3. **波兰和匈牙利** — 单价极低（$0.016 和 $0.014），适合大批量发送
4. **瑞士** — 不支持双向，每条消息需包含退出链接，占用字符
5. **法国** — 检查现有 Sender ID 是否包含破折号，如有须立即更新

---

## 八、合规清单

### 8.1 通用合规（适用于全部 10 国）

- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 同意记录包含时间、方式、内容，安全保存
- [ ] 实施有效的 opt-out 机制
- [ ] 每条营销消息包含退订指引
- [ ] 隐私政策说明 SMS 数据处理方式
- [ ] 消息使用目标国家当地语言
- [ ] 发送时间限制在当地工作时间（09:00-20:00）
- [ ] 使用品牌自有域名短链接
- [ ] 正确格式化号码（E.164 格式）
- [ ] 申请提高默认消费阈值（$1.00 USD → 实际需要值）

### 8.2 各国特殊清单

- [ ] **法国 FR**：确认 Sender ID 不含破折号（-），仅使用字母数字字符
- [ ] **法国 FR**：核查 Bloctel（反电话营销列表）
- [ ] **比利时 BE**：由于不支持 Sender ID，在消息内容中包含品牌标识
- [ ] **比利时 BE**：根据目标受众选择荷兰语/法语/德语
- [ ] **挪威 NO**：核查 Reservasjonsregisteret（反直接营销登记）
- [ ] **丹麦 DK**：核查 Robinson 列表（Robinsonlisten）
- [ ] **希腊 GR**：确认国际发送限制，提前测试投递能力
- [ ] **希腊 GR**：考虑使用 Greeklish（拉丁转写）降低 Unicode 成本
- [ ] **保加利亚 BG**：考虑使用拉丁转写降低西里尔字母的 Unicode 成本
- [ ] **保加利亚 BG**：注意 $0.212/条的高单价
- [ ] **瑞士 CH**：每条营销消息包含网页退出链接（不支持 STOP 回复）
- [ ] **瑞士 CH**：遵守 FADP/nDSG（非 GDPR 但要求类似）
- [ ] **瑞士 CH**：根据目标受众语言区域选择德语/法语/意大利语
- [ ] **匈牙利 HU**：确认 Sender ID 实际支持状态（需测试）

---

## 九、风险评估

| 风险 | 涉及国家 | 严重程度 | 缓解措施 |
|------|---------|---------|---------|
| GDPR 违规处罚 | FR, BE, NO, DK, PL, CZ, HU, GR, BG | **极高** | 严格遵循同意流程，保留完整记录 |
| 希腊国际发送不可用 | GR | **极高** | 提前充分测试，准备替代方案（本地路由） |
| 瑞士无双向 SMS 影响退出机制 | CH | **高** | 每条消息包含网页退出链接 |
| 法国 Sender ID 格式变更 | FR | **高** | 立即检查并更新含破折号的 Sender ID |
| 比利时无 Sender ID 影响品牌识别 | BE | **中** | 在消息内容中突出品牌名称 |
| 保加利亚高单价 | BG | **中** | 使用拉丁转写降低 Unicode 成本 |
| 匈牙利 Sender ID 不确定性 | HU | **中** | 小批量测试确认实际支持 |
| 丹麦 Robinson 列表违规 | DK | **中** | 发送前核查列表，维护排除名单 |
| 挪威反营销登记违规 | NO | **中** | 核查 Reservasjonsregisteret |
| 6国无短码限制高吞吐量 | NO, DK, PL, CZ, HU, GR | **中** | 使用多个长码或 Sender ID 分散流量 |
| 保加利亚/希腊 Unicode 编码成本 | BG, GR | **中** | 使用拉丁转写保持 GSM-7 |
| 短码审批等待期 | FR, BE, BG | **中** | 提前 6-15 周申请 |
| CNIL（法国）严格执法 | FR | **中** | 咨询法国当地法律顾问 |
| 瑞士 FADP 与 GDPR 的细微差异 | CH | **低** | 参照 GDPR 标准执行，咨询瑞士法律顾问 |

---

## 十、各国限制速查表

快速参考各国不支持的功能及替代方案：

| 国家 | 不支持项 | 替代方案 |
|------|---------|---------|
| **法国 FR** | 长码；Sender ID 不可含破折号 | 使用短码或合规 Sender ID |
| **比利时 BE** | Sender ID | 使用短码或长码，在消息中标识品牌 |
| **挪威 NO** | 短码 | 使用长码或 Sender ID |
| **丹麦 DK** | 短码 | 使用长码（$10/月）或 Sender ID |
| **波兰 PL** | 短码 | 使用长码或 Sender ID |
| **捷克 CZ** | 短码 | 使用长码或 Sender ID |
| **匈牙利 HU** | 短码 | 使用长码或 Sender ID（需确认） |
| **希腊 GR** | 短码；国际发送 | 使用长码或 Sender ID；本地路由 |
| **保加利亚 BG** | 长码 | 使用短码或 Sender ID |
| **瑞士 CH** | 短码；长码；双向 SMS | 仅 Sender ID；网页链接退出 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。各国法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询当地法律顾问以确保合规。
