# AWS SMS 拉美及大洋洲国家合集报告

> 调研日期：2026年4月7日
> 覆盖国家：巴西 (BR)、智利 (CL)、哥伦比亚 (CO)、哥斯达黎加 (CR)、多米尼加 (DO)、秘鲁 (PE)、新西兰 (NZ)
> 数据来源：AWS End User Messaging SMS 官方文档、各国电信监管机构资料
> 备注：阿根廷 (AR) 已有单独专题报告，不包含在本合集中

---

## 一、各国能力对比表

| 项目 | 巴西 BR +55 | 智利 CL +56 | 哥伦比亚 CO +57 | 哥斯达黎加 CR +506 | 多米尼加 DO +1-809/829/849 | 秘鲁 PE +51 | 新西兰 NZ +64 |
|------|------------|------------|----------------|-------------------|--------------------------|------------|--------------|
| 短码 | **支持（唯一方式）** | 支持 | **支持（唯一方式）** | 支持 | **支持（唯一方式）** | **支持（唯一方式）** | 支持（强烈推荐专用） |
| 长码 | 不支持 | 支持（$21/月） | 不支持 | 支持 | 不支持 | 不支持 | 不支持 |
| Sender ID | 不支持 | 不支持 | 不支持 | 不支持 | 不支持（被覆写为随机短码） | 不支持（被运营商覆写） | 不支持 |
| Toll-Free | 不支持 | 不支持 | 不支持 | 不支持 | 不支持 | 不支持 | 不支持 |
| 双向 SMS | 支持 | 支持 | **不支持** | 支持 | 支持 | 支持 | 支持 |
| 消息拼接 | 支持 | 短码支持 / **长码不支持** | 支持 | **运营商不支持** | 支持 | 支持 | 支持 |
| 国际发送 | 支持 | 支持 | 支持 | 支持 | 支持 | 支持 | 支持 |
| 注册要求 | 短码需 Support Case | 短码需 Support Case | 短码需 Support Case | 无需注册 | 短码需 Support Case | 短码需 Support Case | 专用短码需 Support Case |
| 监管机构 | ANATEL | SUBTEL | CRC / MinTIC | SUTEL | INDOTEL | OSIPTEL / MTC | Commerce Commission |
| 数据保护法 | LGPD | Ley 19.628 | Ley 1581/2012 | Ley 8968 | Ley 172-13 | Ley 29733 | Privacy Act 2020 |
| 主要语言编码 | GSM-7* | GSM-7* | GSM-7* | GSM-7* | GSM-7* | GSM-7* | GSM-7 |
| AWS 推荐 Region | **sa-east-1** | 任意 | 任意 | 任意 | 任意 | 任意 | 任意 |

> *西班牙语/葡萄牙语建议去掉部分重音符号（á í ó ú）以保持 GSM-7 编码（160字符/条），保留 é 和 ñ（GSM-7 兼容）。

---

## 二、各国详细信息

---

### 2.1 巴西 (BR, +55)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +55 |
| 短码 (Short Code) | **支持（唯一发送方式）** |
| 长码 (Long Code) | 不支持 |
| Sender ID | **不支持** |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |
| 推荐 AWS Region | **sa-east-1（圣保罗）** |

**核心特点：** 巴西仅支持通过短码发送。AWS 在南美设有 sa-east-1（圣保罗）区域，建议使用该区域以获得最低延迟。不支持 Sender ID 和长码。

#### 号码申请流程

巴西不在 AWS 控制台直接支持短码申请的国家列表中，需通过 **AWS Support Case** 手动申请。

**Support Case 填写指南：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | **sa-east-1**（推荐） |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型（OTP / Transactional / Promotional） |
| Limit Increase Value | **1** |

**Case Description 模板：**

```
Subject: Request for Dedicated Short Code - Brazil (BR)

We are requesting a dedicated SMS short code for Brazil (+55).

Company Information:
- Company Name: [公司法定名称]
- CNPJ (if applicable): [巴西公司注册号]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)

Target Carriers: Vivo (Telefonica), Claro (America Movil), TIM, Oi

Expected Monthly Volume: [预估月发送量]

Sample Messages (Brazilian Portuguese):
1. "[Brand] Seu codigo de verificacao: 385291. Valido por 5 minutos.
   Nao compartilhe este codigo."
2. "[Brand] Pedido #BR12345 confirmado. Valor: R$ 150,00.
   Entrega prevista: 10/04. Info: https://brand.com/o/BR12345"
3. "[Brand] Alerta: Login detectado de novo dispositivo.
   Se nao foi voce: https://brand.com/security"

Opt-in Process:
Users register on our website/app and provide their Brazilian mobile number.
During registration, they check a consent box (in Portuguese).

Opt-out Mechanism:
Reply STOP to unsubscribe, or manage preferences at
https://brand.com/settings or email optout@brand.com.

We request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.

AWS Region: sa-east-1
```

**申请时间线：**

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 提供注册表格 | 1-3个工作日 |
| 填写并提交注册表格 | 取决于你 |
| 运营商审核 | 4-12周 |
| 短码配置完成 | 1-2周 |
| **总计** | **6-15周** |

#### 监管机构与法规

**ANATEL（Agencia Nacional de Telecomunicacoes，国家电信局）** 是巴西电信监管机构。

**主要法规：**
- Lei Geral de Telecomunicacoes (LGT) — 基础电信法
- Resolucao 477/2007 — 移动通信服务条例
- ANATEL A2P 短信管理规则

**对 SMS 的关键规定：**
- 商业 SMS 须获得用户明确同意
- 禁止发送垃圾短信
- 运营商实施 A2P 过滤
- ANATEL 对 A2P SMS 有注册要求

**发送时间建议：**
- 推荐发送时段：08:00-21:00 BRT（UTC-3）
- 巴西跨 4 个时区，注意目标用户所在时区
- 避免周日和国定假日发送营销消息

#### 数据保护法

**LGPD（Lei Geral de Protecao de Dados，通用数据保护法）** — 2020年生效，与欧盟 GDPR 高度相似，是拉美最严格的数据保护法。

| 要求 | 详情 |
|------|------|
| 用户同意 | 处理个人数据需合法基础（同意为其一） |
| 数据访问权 | 数据主体有权确认数据处理、访问数据 |
| 更正/删除权 | 可请求更正、匿名化或删除数据 |
| 数据携带权 | 有权将数据转移至其他服务提供商 |
| 跨境传输 | 仅限提供充分保护的国家，或获得同意 |
| DPO | 须指定数据保护官（Encarregado） |
| 数据泄露通知 | 须通知 ANPD 和受影响的数据主体 |
| 违规处罚 | 最高 **5000万雷亚尔** 或年营业额 **2%**（以较高者为准） |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 母公司 | 备注 |
|--------|--------|------|
| Vivo | Telefonica（西班牙） | 最大运营商 |
| Claro | America Movil（墨西哥） | 第二大 |
| TIM | TIM S.p.A.（意大利） | 第三大 |
| Oi | 重组中 | 第四大，正在出售移动资产 |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号（圣保罗/里约） | +55 + DDD + 9 + 8位号码 | `+5511912345678` |
| 手机号（其他城市） | +55 + DDD + 9 + 8位号码 | `+5521912345678` |

- 巴西手机号码已全面使用 9 位（含前缀 9），共 11 位国内号码
- DDD（区号）为 2 位数字
- 正确格式：`+55DDXXXXXXXXX`（共 13 位数字）
- 固定电话不可达

**主要 DDD 区号：**

| 城市 | DDD |
|------|-----|
| 圣保罗 | 11 |
| 里约热内卢 | 21 |
| 贝洛奥里藏特 | 31 |
| 巴西利亚 | 61 |
| 萨尔瓦多 | 71 |

#### 特殊限制

- **仅支持短码**，无替代发送方式
- **推荐使用 sa-east-1 区域**以降低延迟
- 公司识别号：CNPJ（14位数字）
- 巴西葡萄牙语与欧洲葡萄牙语有显著差异，须使用巴西本地化内容
- 运营商有较强的 A2P 过滤，避免可疑内容

---

### 2.2 智利 (CL, +56)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +56 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持（$21/月）** |
| Sender ID | 不支持 |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 短码支持 / **长码不支持** |
| 国际发送 | 支持 |

**核心特点：** 智利是拉美少数同时支持短码和长码的国家。但长码有重要限制：禁止营销消息、不支持拼接。

#### 号码申请流程

**方式一：长码（较快，但限制多）**

可在 AWS 控制台直接购买，月费约 $21 USD。

**长码限制：**
- **禁止发送营销/促销消息**，仅限事务性
- **不支持消息拼接**，超过 160 字符（GSM-7）或 70 字符（Unicode）的消息可能投递失败
- 60秒内超 10 条相同消息被限制

**方式二：短码（推荐，功能完整）**

智利是 AWS 控制台直接支持短码申请的国家之一。

**控制台申请路径：**
AWS End User Messaging SMS > Short codes > Request short code

也可通过 Support Case 申请：

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域 |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

**Case Description 模板：**

```
Subject: Request for Dedicated Short Code - Chile (CL)

We are requesting a dedicated SMS short code for Chile (+56).

Company Information:
- Company Name: [公司法定名称]
- RUT: [智利税号]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes
- Secondary: Transactional notifications and promotional messages

Target Carriers: Entel, Movistar (Telefonica), Claro, WOM

Expected Monthly Volume: [预估月发送量]

Sample Messages (Spanish):
1. "[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos.
   No comparta este codigo."
2. "[Brand] Pedido #CL12345 confirmado. Monto: CLP 50,000.
   Entrega estimada: 10 Abr. Info: https://brand.com/o/CL12345"

Opt-in/Opt-out:
Users opt in during registration. Reply STOP to unsubscribe.
```

#### 监管机构与法规

**SUBTEL（Subsecretaria de Telecomunicaciones，电信副部长办公室）** 是智利电信监管机构。

**主要法规：**
- Ley General de Telecomunicaciones (Ley 18.168)
- 消费者保护法相关条款

**对 SMS 的关键规定：**
- **政治消息禁止**
- 商业 SMS 须获得用户同意
- 60秒内超 10 条相同消息被运营商限制
- 长码禁止营销内容

**发送时间建议：**
- 推荐发送时段：08:00-21:00 CLT（UTC-3 冬季 / UTC-4 夏季）

#### 数据保护法

**Ley 19.628（Sobre Proteccion de la Vida Privada，隐私保护法）** 是智利数据保护法。智利正在推进新的数据保护法改革以与 GDPR 对齐。

| 要求 | 详情 |
|------|------|
| 用户同意 | 处理个人数据须获得同意或有法律依据 |
| 数据访问权 | 个人有权查阅、更正、删除其数据 |
| 目的限制 | 仅限收集时声明的用途 |
| 违规处罚 | 现行法律处罚相对较轻，改革后将大幅加强 |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 母公司 | 备注 |
|--------|--------|------|
| Entel | Entel Chile | 最大运营商 |
| Movistar | Telefonica（西班牙） | |
| Claro | America Movil（墨西哥） | |
| WOM | Novator Partners | 低价竞争者 |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +56 + 9 + 8位号码 | `+56912345678` |

- 手机号码以 9 开头，共 9 位国内号码
- 正确格式：`+56XXXXXXXXX`（共 11 位数字）
- 固定电话不可达

#### 特殊限制

- **长码不支持拼接消息**：超单条限制的消息可能投递失败，务必控制在 160 字符（GSM-7）以内
- **长码禁止营销消息**：营销/促销须使用短码
- 60秒内超 10 条相同消息被限制
- 政治消息被禁止
- 使用短码时拼接正常支持

---

### 2.3 哥伦比亚 (CO, +57)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +57 |
| 短码 (Short Code) | **支持（唯一发送方式）** |
| 长码 (Long Code) | 不支持 |
| Sender ID | **不支持** |
| Toll-Free | 不支持 |
| 双向 SMS | **不支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |

**核心特点：** 哥伦比亚仅支持短码，且不支持双向 SMS。这意味着用户无法通过 STOP/HELP 关键字退订，必须提供替代退出机制。Virgin Mobile 会将专用短码替换为共享短码。

#### 号码申请流程

需通过 **AWS Support Case** 手动申请，预计 4-10 周。

**Support Case 填写指南：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域 |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

**Case Description 模板：**

```
Subject: Request for Dedicated Short Code - Colombia (CO)

We are requesting a dedicated SMS short code for Colombia (+57).

Company Information:
- Company Name: [公司法定名称]
- NIT: [哥伦比亚税号]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes
- Secondary: Transactional notifications

Target Carriers: Claro, Movistar, Tigo, Virgin Mobile

Expected Monthly Volume: [预估月发送量]

Sample Messages (Spanish):
1. "[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos.
   No comparta este codigo."
2. "[Brand] Pedido #CO12345 confirmado. Monto: COP 200,000.
   Info: https://brand.com/o/CO12345"

Opt-in Process:
Users opt in during registration by checking a consent box.

Opt-out Mechanism:
Since two-way SMS is not supported in Colombia, we provide an unsubscribe
link in each message (https://brand.com/unsub/{token}). Users can also
opt out via email at optout@brand.com or through their account settings.

We request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.
```

**申请时间线：**

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 提供注册表格 | 1-3个工作日 |
| 运营商审核 | 4-10周 |
| 短码配置完成 | 1-2周 |
| **总计** | **5-13周** |

#### 监管机构与法规

**CRC（Comision de Regulacion de Comunicaciones）** 和 **MinTIC（Ministerio de Tecnologias de la Informacion y las Comunicaciones）** 是哥伦比亚电信监管机构。

**主要法规：**
- Ley 1341 de 2009（信息和通信技术法）
- CRC 相关决议

**对 SMS 的关键规定：**
- 商业 SMS 须获得用户同意
- 禁止垃圾短信
- 运营商有内容过滤机制

**发送时间建议：**
- 推荐发送时段：08:00-21:00 COT（UTC-5）

#### 数据保护法

**Ley 1581 de 2012（Ley de Proteccion de Datos Personales）** 和 **Ley 1266 de 2008（Habeas Data）** 是哥伦比亚数据保护法。

| 要求 | 详情 |
|------|------|
| 用户同意 | 须获得明确、事前的同意 |
| 数据访问权 | 数据主体有权查阅、更正、删除 |
| 数据库注册 | 数据库须在 SIC 国家登记处注册 |
| DPO | 须指定负责数据保护的人员 |
| 违规处罚 | 最高 **2,000 倍当年最低月工资**（约 20 亿哥伦比亚比索） |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 母公司 | 备注 |
|--------|--------|------|
| Claro | America Movil（墨西哥） | 最大运营商 |
| Movistar | Telefonica（西班牙） | |
| Tigo | Millicom | |
| Virgin Mobile | 哥伦比亚本地运营 | **会替换短码**（见下方） |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +57 + 10位号码 | `+573001234567` |

- 手机号码以 3 开头，共 10 位国内号码
- 正确格式：`+57XXXXXXXXXX`（共 12 位数字）
- 固定电话不可达

#### 特殊限制

- **不支持双向 SMS**：必须提供替代退出机制（网页链接/邮箱）
- **Virgin Mobile 替换短码**：Virgin Mobile 会将你的专用短码替换为共享短码，导致用户看到的发送号码不一致
- 每条营销消息必须包含退出指引（使用链接）
- 建议在消息开头加 [Brand] 标识

---

### 2.4 哥斯达黎加 (CR, +506)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +506 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | **支持** |
| Sender ID | 不支持 |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | **运营商不支持** |
| 国际发送 | 支持 |

**核心特点：** 哥斯达黎加同时支持短码和长码，但运营商不支持消息拼接。号码可能被运营商覆写为短码或本地长码。

#### 号码申请流程

**方式一：长码**

可在 AWS 控制台直接购买或通过 Support Case 申请。

**方式二：短码**

需通过 **AWS Support Case** 手动申请。

**Support Case 填写指南：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域 |
| Resource Type | Dedicated SMS Short Codes（或 Long Codes） |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

#### 监管机构与法规

**SUTEL（Superintendencia de Telecomunicaciones，电信监管局）** 是哥斯达黎加电信监管机构。

**主要法规：**
- Ley General de Telecomunicaciones (Ley 8642)
- 消费者保护相关法规

**对 SMS 的关键规定：**
- 商业 SMS 须获得用户同意
- 禁止垃圾短信

**发送时间建议：**
- 推荐发送时段：08:00-21:00 CST（UTC-6）

#### 数据保护法

**Ley 8968（Proteccion de la Persona frente al Tratamiento de sus Datos Personales，个人数据保护法）** — 2011年生效。

| 要求 | 详情 |
|------|------|
| 用户同意 | 须获得知情同意 |
| 数据访问权 | 个人有权查阅、更正、删除 |
| 数据库注册 | 须在 PRODHAB 注册 |
| 违规处罚 | 行政罚款最高 **基本工资的30倍** |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 备注 |
|--------|------|
| Kolbi (ICE) | 国有运营商，最大 |
| Movistar | Telefonica |
| Claro | America Movil |
| Liberty | |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +506 + 8位号码 | `+50681234567` |

- 哥斯达黎加无区号，号码统一 8 位
- 手机号码通常以 5/6/7/8 开头
- 正确格式：`+506XXXXXXXX`（共 11 位数字）
- 固定电话不可达

#### 特殊限制

- **运营商不支持拼接消息**：超过单条限制（160 字符 GSM-7 / 70 字符 Unicode）的消息可能投递失败
- **号码可能被覆写**：你的发送号码可能被运营商替换为短码或本地长码
- 固定电话号码不可达
- **务必将消息控制在单条以内**

---

### 2.5 多米尼加共和国 (DO, +1-809/829/849)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +1-809 / +1-829 / +1-849 |
| 短码 (Short Code) | **支持（唯一发送方式）** |
| 长码 (Long Code) | 不支持 |
| Sender ID | **不支持（被覆写为随机短码）** |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |

**核心特点：** 多米尼加仅支持短码。即使设置了 Sender ID 或使用国际长码，号码也会被运营商覆写为随机短码。申请周期约 4 周，是拉美国家中最快的。

#### 号码申请流程

需通过 **AWS Support Case** 手动申请，预计约 4 周。

**Support Case 填写指南：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域 |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

**Case Description 模板：**

```
Subject: Request for Dedicated Short Code - Dominican Republic (DO)

We are requesting a dedicated SMS short code for Dominican Republic.

Company Information:
- Company Name: [公司法定名称]
- RNC: [多米尼加税号]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes
- Secondary: Transactional notifications

Target Carriers: Claro, Altice (Orange), Viva (Trilogy International)

Expected Monthly Volume: [预估月发送量]

Sample Messages (Spanish):
1. "[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos.
   No comparta este codigo."
2. "[Brand] Pedido #DO12345 confirmado. Monto: RD$ 5,000.
   Info: https://brand.com/o/DO12345"

Opt-in/Opt-out:
Users opt in during registration. Reply STOP to unsubscribe.
```

**申请时间线：**

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 提供注册表格 | 1-3个工作日 |
| 运营商审核 | 约4周 |
| 短码配置完成 | 约1周 |
| **总计** | **约5-6周** |

#### 监管机构与法规

**INDOTEL（Instituto Dominicano de las Telecomunicaciones，多米尼加电信研究所）** 是电信监管机构。

**主要法规：**
- Ley General de Telecomunicaciones (Ley 153-98)

**对 SMS 的关键规定：**
- 商业 SMS 须获得用户同意
- INDOTEL 对电信服务有一般性监管

**发送时间建议：**
- 推荐发送时段：08:00-21:00 AST（UTC-4）

#### 数据保护法

**Ley 172-13（Ley de Proteccion Integral de Datos Personales）** — 2013年生效。

| 要求 | 详情 |
|------|------|
| 用户同意 | 须获得数据主体同意 |
| 数据访问权 | 个人有权查阅、更正 |
| 数据安全 | 须采取适当措施保障安全 |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 母公司 | 备注 |
|--------|--------|------|
| Claro | America Movil（墨西哥） | 最大运营商 |
| Altice | Altice（法国） | 前 Orange |
| Viva | Trilogy International Partners | |

**号码格式：**

多米尼加使用北美编号计划（NANP），与美国共享 +1 前缀。

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +1 + 区号 + 7位号码 | `+18091234567` |

- 区号：809、829、849
- 正确格式：`+1809XXXXXXX` / `+1829XXXXXXX` / `+1849XXXXXXX`（共 11 位数字）
- 固定电话不可达

#### 特殊限制

- **Sender ID 被覆写**：即使设置了 Sender ID，运营商也会将其替换为随机短码
- **国际长码号码被覆写**：同样会被替换为随机短码
- 固定电话号码不可达
- 由于号码被覆写，用户看到的发送号码可能每次不同，建议在消息正文中明确标识品牌

---

### 2.6 秘鲁 (PE, +51)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +51 |
| 短码 (Short Code) | **支持（唯一发送方式）** |
| 长码 (Long Code) | 不支持 |
| Sender ID | **不支持（被运营商覆写）** |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |

**核心特点：** 秘鲁仅支持短码。即使设置了 Sender ID，运营商也会将其覆写为其他号码。

#### 号码申请流程

需通过 **AWS Support Case** 手动申请。

**Support Case 填写指南：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域 |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

**Case Description 模板：**

```
Subject: Request for Dedicated Short Code - Peru (PE)

We are requesting a dedicated SMS short code for Peru (+51).

Company Information:
- Company Name: [公司法定名称]
- RUC: [秘鲁税号]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes
- Secondary: Transactional notifications

Target Carriers: Movistar (Telefonica), Claro, Entel, Bitel

Expected Monthly Volume: [预估月发送量]

Sample Messages (Spanish):
1. "[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos.
   No comparta este codigo."
2. "[Brand] Pedido #PE12345 confirmado. Monto: S/ 500.
   Info: https://brand.com/o/PE12345"

Opt-in/Opt-out:
Users opt in during registration. Reply STOP to unsubscribe.
```

#### 监管机构与法规

**OSIPTEL（Organismo Supervisor de Inversion Privada en Telecomunicaciones）** 和 **MTC（Ministerio de Transportes y Comunicaciones）** 是秘鲁电信监管机构。

**主要法规：**
- Ley de Telecomunicaciones (DL 26096)
- OSIPTEL 相关决议

**对 SMS 的关键规定：**
- 商业 SMS 须获得用户同意
- 禁止垃圾短信
- 运营商有内容过滤机制

**发送时间建议：**
- 推荐发送时段：08:00-21:00 PET（UTC-5）

#### 数据保护法

**Ley 29733（Ley de Proteccion de Datos Personales，个人数据保护法）** — 2011年生效。

| 要求 | 详情 |
|------|------|
| 用户同意 | 须获得数据主体明确同意 |
| 数据访问权 | 个人有权查阅、更正、取消、反对 |
| 数据库注册 | 须在 APDP 国家登记处注册 |
| 跨境传输 | 仅限提供充分保护的国家 |
| 违规处罚 | 最高 **100个税务单位**（UIT），约 50万索尔 |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 母公司 | 备注 |
|--------|--------|------|
| Movistar | Telefonica（西班牙） | 最大运营商 |
| Claro | America Movil（墨西哥） | |
| Entel | Entel Chile | |
| Bitel | Viettel（越南） | |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +51 + 9位号码 | `+51912345678` |

- 手机号码以 9 开头，共 9 位国内号码
- 正确格式：`+51XXXXXXXXX`（共 11 位数字）
- 固定电话不可达

#### 特殊限制

- **Sender ID 被运营商覆写**：即使设置了 Sender ID，也会显示为其他号码
- **仅支持短码**，无替代发送方式
- 建议在消息正文中明确标识品牌

---

### 2.7 新西兰 (NZ, +64)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +64 |
| 短码 (Short Code) | **支持（强烈推荐专用短码）** |
| 长码 (Long Code) | 不支持 |
| Sender ID | 不支持 |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |

**核心特点：** 新西兰默认通过共享短码池发送，投递率为 best-effort，**不可靠**。AWS **强烈推荐** 申请专用短码。含 URL 的消息必须走白名单审批流程。

#### 号码申请流程

**专用短码（强烈推荐）：**

需通过 **AWS Support Case** 手动申请，预计 **5-6 周**。

**Support Case 填写指南：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域（如 ap-southeast-2） |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

**Case Description 模板：**

```
Subject: Request for Dedicated Short Code - New Zealand (NZ)

We are requesting a dedicated SMS short code for New Zealand (+64).

Company Information:
- Company Name: [公司法定名称]
- NZBN: [新西兰商业号码]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes
- Secondary: Transactional notifications

Target Carriers: Spark, Vodafone NZ, 2degrees

Expected Monthly Volume: [预估月发送量]

Sample Messages (English):
1. "[Brand] Your verification code is: 385291. Valid for 5 minutes.
   Do not share this code."
2. "[Brand] Order #NZ12345 confirmed. Amount: NZD 150.
   Delivery expected: 10 Apr. Info: https://brand.com/o/NZ12345"

URL Whitelist Request:
We request that the following domains be whitelisted for SMS content:
- brand.com
- [additional domains]

Opt-in/Opt-out:
Users opt in during registration. Reply STOP to unsubscribe.
Replying to this message may incur charges.

We understand that a dedicated short code is strongly recommended
for reliable delivery in New Zealand.
```

**申请时间线：**

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 提供注册表格 | 1-3个工作日 |
| URL 白名单审批 | 包含在流程中 |
| 运营商审核 + 短码配置 | 5-6周 |
| **总计** | **约5-6周** |

#### 监管机构与法规

**Commerce Commission（商业委员会）** 和 **MBIE（Ministry of Business, Innovation and Employment）** 是新西兰电信监管机构。

**主要法规：**
- Telecommunications Act 2001
- Unsolicited Electronic Messages Act 2007（反垃圾电子消息法）

**Unsolicited Electronic Messages Act 2007 关键规定：**
- 禁止发送未经同意的商业电子消息（含 SMS）
- 必须提供退订机制
- 必须包含发送者身份信息
- **每条消息须包含提示：回复本短信可能被收费**
- 违规可处最高 **$500,000 NZD** 罚款（个人）或 **$2,000,000 NZD**（公司）

**禁止内容：**
- 枪支相关内容
- 赌博推广
- 成人内容
- 高利贷/贷款推广
- 政治宣传
- 宗教推广

**发送时间建议：**
- 推荐发送时段：08:00-21:00 NZST（UTC+12 / UTC+13 夏令时）
- 新西兰跨 2 个时区（含查塔姆群岛 UTC+12:45）

#### 数据保护法

**Privacy Act 2020** 是新西兰核心隐私保护法律，2020年12月生效。

| 要求 | 详情 |
|------|------|
| 信息隐私原则 (IPPs) | 13项原则覆盖收集、存储、使用、披露 |
| 数据收集 | 仅收集合法目的所需数据，须告知当事人 |
| 数据安全 | 须采取合理措施防止丢失、未授权访问 |
| 数据访问权 | 个人有权请求访问和更正 |
| 跨境传输 | 须确保接收国提供可比较的保护水平 |
| 数据泄露通知 | **强制通知** — 须通知隐私专员和受影响个人 |
| 违规处罚 | 最高 **$10,000 NZD** 刑事罚款；民事投诉可获赔偿 |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 备注 |
|--------|------|
| Spark | 最大运营商（前 Telecom NZ） |
| Vodafone NZ | 第二大 |
| 2degrees | 第三大 |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +64 + 去掉首0的号码 | `+64211234567` |

- 手机号码以 02 开头（通常 021/022/027/028/029），发送时去掉首位 0
- 号码长度不固定：021 号码为 7-8 位，022 号码为 7-8 位等
- 正确格式：`+64XXXXXXXXX`（10-11 位数字）
- 固定电话不可达

#### 特殊限制

- **共享短码池不可靠**：默认发送使用共享池，投递率为 best-effort，生产环境**必须**使用专用短码
- **含 URL 的消息必须白名单审批**：未经白名单的 URL 会被运营商过滤
- URL 白名单在专用短码申请流程中一并处理
- **禁止短链接**（bit.ly、tinyurl 等）
- **每条消息须包含收费提示**：告知用户回复短信可能被收费
- 禁止内容范围较广（枪支、赌博、成人、贷款、政治、宗教）

---

## 三、各语言消息模板

### 3.1 字符编码总览

| 语言 | 编码 | 单条上限 | 拼接每段 | 备注 |
|------|------|---------|---------|------|
| 巴西葡萄牙语 | GSM-7* | **160字符** | 153字符 | 去掉 á í ó ú 重音，保留 é ñ |
| 西班牙语 | GSM-7* | **160字符** | 153字符 | 去掉 á í ó ú 重音，保留 é ñ |
| 英语 | GSM-7 | **160字符** | 153字符 | 完全兼容 |

> *GSM-7 兼容性说明：西班牙语/葡萄牙语中 `é` 和 `ñ` 在 GSM-7 字符集内，可保留。`á í ó ú` 不在 GSM-7 字符集内，使用会触发 Unicode 编码（70字符/条）。建议去掉这些重音以保持 GSM-7 编码，拉美用户普遍习惯在短信中不使用重音符号。

| 字符 | GSM-7 兼容 | 建议 |
|------|-----------|------|
| é | **是** | 可保留 |
| ñ | **是** | 可保留 |
| á | **否** | 去掉重音写 `a` |
| í | **否** | 去掉重音写 `i` |
| ó | **否** | 去掉重音写 `o` |
| ú | **否** | 去掉重音写 `u` |
| ü | **是** | 可保留 |

---

### 3.2 巴西葡萄牙语模板（GSM-7, 160字符/条）

**OTP 验证码：**
```
[Brand] Seu codigo de verificacao: 385291. Valido por 5 minutos. Nao compartilhe este codigo com ninguem.
```
（103字符，1条消息）

**订单确认：**
```
[Brand] Pedido #BR12345 confirmado. Valor: R$ 150,00. Entrega prevista: 10 Abr. Info: https://brand.com/o/BR12345
```
（113字符，1条消息）

**发货通知：**
```
[Brand] Pedido #BR12345 enviado. Rastreio: https://brand.com/track/BR12345 Previsao: 10 Abr.
```
（93字符，1条消息）

**派送中：**
```
[Brand] Seu pacote esta a caminho. Chegada prevista: hoje 14-17h. Rastreio: https://brand.com/track/BR12345
```
（107字符，1条消息）

**安全警告：**
```
[Brand] Alerta: Login detectado de novo dispositivo. Se nao foi voce: https://brand.com/security
```
（96字符，1条消息）

**营销消息（需 opt-in）：**
```
[Brand] 20% de desconto na proxima compra! Use o codigo PROMO20. Valido ate 15/04. Cancelar: https://brand.com/unsub/{TOKEN}
```
（125字符，1条消息）

**Opt-in 确认：**
```
[Brand] Bem-vindo! Voce recebera alertas de conta e notificacoes. Responda STOP para cancelar ou acesse https://brand.com/unsub/{TOKEN}
```
（135字符，1条消息）

**注意事项：**
- 使用巴西葡萄牙语，与欧洲葡萄牙语有显著差异
- 去掉 á í ó ú ã õ 等重音和波浪号以保持 GSM-7
- `ã` 和 `õ` 不在 GSM-7 中，写为 `a` 和 `o`
- 货币使用 R$（雷亚尔）
- 日期格式：DD/MM（日/月）

---

### 3.3 西班牙语模板（GSM-7, 160字符/条）

> 适用于：智利 (CL)、哥伦比亚 (CO)、哥斯达黎加 (CR)、多米尼加 (DO)、秘鲁 (PE)

**OTP 验证码：**
```
[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos. No comparta este codigo con nadie.
```
（101字符，1条消息）

**订单确认（各国货币版本）：**

智利：
```
[Brand] Pedido #CL12345 confirmado. Monto: CLP 50,000. Entrega estimada: 10 Abr. Info: https://brand.com/o/CL12345
```
（114字符，1条消息）

哥伦比亚：
```
[Brand] Pedido #CO12345 confirmado. Monto: COP 200,000. Info: https://brand.com/o/CO12345
```
（90字符，1条消息）

哥斯达黎加：
```
[Brand] Pedido #CR12345 confirmado. Monto: CRC 75,000. Entrega estimada: 10 Abr. Info: https://brand.com/o/CR12345
```
（114字符，1条消息）

多米尼加：
```
[Brand] Pedido #DO12345 confirmado. Monto: RD$ 5,000. Entrega estimada: 10 Abr. Info: https://brand.com/o/DO12345
```
（113字符，1条消息）

秘鲁：
```
[Brand] Pedido #PE12345 confirmado. Monto: S/ 500. Entrega estimada: 10 Abr. Info: https://brand.com/o/PE12345
```
（111字符，1条消息）

**发货通知（通用）：**
```
[Brand] Pedido #XX12345 enviado. Seguimiento: https://brand.com/track/XX12345 Entrega estimada: 10 Abr.
```
（103字符，1条消息）

**安全警告（通用）：**
```
[Brand] Alerta: Inicio de sesion desde nuevo dispositivo. Si no fue usted: https://brand.com/security
```
（101字符，1条消息）

**营销消息（需 opt-in，通用）：**
```
[Brand] 20% de descuento en su proxima compra. Use codigo PROMO20. Valido hasta 15/04. Cancelar: https://brand.com/unsub/{TOKEN}
```
（127字符，1条消息）

**营销消息（哥伦比亚，含退出链接 — 无双向 SMS）：**
```
[Brand] 20% de descuento en su proxima compra. Codigo PROMO20. Hasta 15/04. Cancelar SMS: https://brand.com/unsub/{TOKEN}
```
（121字符，1条消息）

**Opt-in 确认（支持双向的国家）：**
```
[Brand] Bienvenido! Recibira alertas de cuenta y notificaciones. Frecuencia variable. Responda STOP para cancelar.
```
（114字符，1条消息）

**Opt-in 确认（哥伦比亚，无双向 SMS）：**
```
[Brand] Bienvenido! Recibira alertas de cuenta y notificaciones. Cancelar: https://brand.com/unsub/{TOKEN}
```
（107字符，1条消息）

**注意事项：**
- 去掉 á í ó ú 重音以保持 GSM-7（160字符/条）
- 保留 é 和 ñ（GSM-7 兼容）
- 货币符号因国家而异（CLP, COP, CRC, RD$, S/）
- 哥伦比亚不支持双向 SMS，退出机制必须使用链接
- 哥斯达黎加不支持拼接，**务必控制在 160 字符以内**
- 智利长码也不支持拼接，**使用长码时务必控制在 160 字符以内**
- 日期格式：DD/MM 或 DD Mes（如 10 Abr）

---

### 3.4 英语模板（新西兰，GSM-7, 160字符/条）

> **新西兰特殊要求：** 每条消息须包含"回复可能被收费"的提示。

**OTP 验证码：**
```
[Brand] Your verification code: 385291. Valid for 5 min. Do not share this code. Reply charges may apply.
```
（103字符，1条消息）

**订单确认：**
```
[Brand] Order #NZ12345 confirmed. Amount: NZD 150. Delivery: 10 Apr. Info: https://brand.com/o/NZ12345 Msg charges may apply.
```
（125字符，1条消息）

**发货通知：**
```
[Brand] Order #NZ12345 shipped. Track: https://brand.com/track/NZ12345 Est. delivery: 10 Apr. Msg charges may apply.
```
（116字符，1条消息）

**安全警告：**
```
[Brand] Alert: Login from new device detected. If not you: https://brand.com/security Msg charges may apply.
```
（109字符，1条消息）

**营销消息（需 opt-in）：**
```
[Brand] 20% off your next purchase! Use code PROMO20. Valid until 15/04. Reply STOP to opt out. Msg charges may apply.
```
（118字符，1条消息）

**Opt-in 确认：**
```
[Brand] Welcome! You'll receive account alerts and notifications. Reply STOP to unsubscribe. Msg charges may apply.
```
（115字符，1条消息）

**注意事项：**
- 英语完全兼容 GSM-7（160字符/条）
- **每条消息须包含收费提示**：`Msg charges may apply` 或类似措辞
- **URL 须经白名单审批**，否则被运营商过滤
- 使用品牌自有域名，禁止 bit.ly 等短链接
- 货币使用 NZD 或 $
- 日期格式：DD Mon（如 10 Apr）

---

## 四、各国定价对比

> 以下价格为 AWS 消息费参考值，不含运营商附加费。实际费率请在 AWS 控制台确认或下载定价 CSV。

| 国家 | 发送方式 | 月租费用 | 编码 | 单条字符上限 | 备注 |
|------|---------|---------|------|------------|------|
| 巴西 BR | 短码 | 需确认（$800-$1,500/月） | GSM-7 | 160 | 仅短码，推荐 sa-east-1 |
| 智利 CL | 短码/长码 | 短码需确认 / 长码$21/月 | GSM-7 | 160（短码）/ 160（长码，不可拼接） | 长码限制多 |
| 哥伦比亚 CO | 短码 | 需确认 | GSM-7 | 160 | 仅短码，无双向 |
| 哥斯达黎加 CR | 短码/长码 | 需确认 | GSM-7 | 160（不可拼接） | 不支持拼接 |
| 多米尼加 DO | 短码 | 需确认 | GSM-7 | 160 | Sender ID 被覆写 |
| 秘鲁 PE | 短码 | 需确认 | GSM-7 | 160 | Sender ID 被覆写 |
| 新西兰 NZ | 专用短码 | 需确认 | GSM-7 | 160 | 共享池不可靠 |

### 成本优化建议

1. **使用 GSM-7 编码（去掉 á í ó ú 重音）**：西班牙语/葡萄牙语去掉部分重音后完全兼容 GSM-7，单条 160 字符 vs Unicode 的 70 字符，可节省约 50% 消息段数
2. **控制消息长度**：
   - 哥斯达黎加和智利长码**不支持拼接**，超过 160 字符可能投递失败
   - 其他国家虽支持拼接，但拼接后按多条计费
3. **精简 URL**：使用短路径（如 `/o/ID`）而非完整路径
4. **巴西使用 sa-east-1**：降低延迟，可能改善投递率
5. **维护号码列表**：定期清理无效号码，减少无效发送
6. **申请专用短码**：虽有月租费，但投递率和品牌识别度远优于共享池

### 费用结构

| 费用项 | 说明 |
|--------|------|
| 每条消息费 | 按消息片段（segment）计费，需下载 AWS 定价 CSV 确认各国费率 |
| 短码月租 | 因国家而异，通常 $800-$1,500 USD/月 |
| 长码月费（智利/哥斯达黎加） | 智利 $21/月，哥斯达黎加需确认 |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

> **重要：** 短码费用从向运营商提交申请那一刻开始计费，即使短码尚未配置完成。申请被拒绝已产生的费用也不退还。

---

## 五、合规清单

### 5.1 通用合规项（适用所有7国）

- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 实施有效的 opt-out 机制
- [ ] 消息中包含品牌标识（[Brand] 前缀）
- [ ] 发送时间限制在当地工作时间（通常 08:00-21:00）
- [ ] 正确格式化号码（E.164 格式）
- [ ] 保留同意和退出记录
- [ ] URL 使用品牌自有域名（禁止 bit.ly 等短链接）
- [ ] 事务性消息不混入营销内容
- [ ] 使用 GSM-7 编码（去掉 á í ó ú 重音）
- [ ] 消息控制在单条 160 字符以内（尤其哥斯达黎加和智利长码）
- [ ] 定期清理无效号码
- [ ] 准备备用通信渠道（WhatsApp/邮件）
- [ ] 在 AWS 控制台申请提高消费阈值（默认 $1.00）

### 5.2 各国特殊合规项

**巴西 (BR)：**
- [ ] 遵守 LGPD 数据保护要求
- [ ] 指定 DPO（Encarregado）
- [ ] 使用巴西葡萄牙语（非欧洲葡萄牙语）
- [ ] 优先使用 sa-east-1 区域
- [ ] 了解 ANATEL A2P 注册要求
- [ ] 跨境传输满足 LGPD 条件
- [ ] 发生数据泄露时通知 ANPD

**智利 (CL)：**
- [ ] 长码**禁止营销消息**，营销须使用短码
- [ ] 长码消息**不可拼接**，控制在 160 字符以内
- [ ] 60秒内不发送超 10 条相同消息
- [ ] 禁止政治消息

**哥伦比亚 (CO)：**
- [ ] **不支持双向 SMS**，必须提供替代退出机制（网页链接/邮箱）
- [ ] 每条营销消息包含退出链接
- [ ] 遵守 Ley 1581 数据保护要求
- [ ] 数据库在 SIC 注册
- [ ] 注意 Virgin Mobile 会替换短码

**哥斯达黎加 (CR)：**
- [ ] **运营商不支持拼接消息**，务必控制在单条以内
- [ ] 遵守 Ley 8968 数据保护要求
- [ ] 数据库在 PRODHAB 注册

**多米尼加 (DO)：**
- [ ] 注意 Sender ID / 号码会被覆写为随机短码
- [ ] 在消息正文中明确标识品牌
- [ ] 遵守 Ley 172-13 数据保护要求

**秘鲁 (PE)：**
- [ ] Sender ID 会被运营商覆写
- [ ] 在消息正文中明确标识品牌
- [ ] 遵守 Ley 29733 数据保护要求
- [ ] 数据库在 APDP 注册
- [ ] 跨境传输满足 Ley 29733 条件

**新西兰 (NZ)：**
- [ ] **申请专用短码**（共享池不可靠）
- [ ] **含 URL 的消息须白名单审批**
- [ ] **每条消息包含收费提示**（如 "Msg charges may apply"）
- [ ] 遵守 Unsolicited Electronic Messages Act 2007
- [ ] 遵守 Privacy Act 2020
- [ ] 发生数据泄露时强制通知隐私专员
- [ ] 禁止发送枪支/赌博/成人/贷款/政治/宗教内容

---

## 六、风险评估

| 风险 | 涉及国家 | 严重程度 | 缓解措施 |
|------|---------|---------|---------|
| 共享短码池投递率低 | NZ | **极高** | 申请专用短码（5-6周），切勿依赖共享池 |
| 不支持双向 SMS，无法 STOP 退订 | CO | **高** | 每条消息包含退出链接，提供邮箱退出选项 |
| URL 被运营商过滤 | NZ | **高** | 通过专用短码流程申请 URL 白名单 |
| 短码申请被拒 | BR, CL, CO, CR, DO, PE, NZ | 高 | 准备详尽用例说明，确保内容合规 |
| 短码申请周期长（6-15周） | BR, CO, PE | 中 | 提前规划，尽早提交申请 |
| Virgin Mobile 替换短码 | CO | 中 | 在消息正文明确标识品牌，告知用户可能显示不同号码 |
| Sender ID 被覆写 | DO, PE | 中 | 在消息正文开头加 [Brand] 标识 |
| 不支持拼接消息 | CR, CL（长码） | 中 | 严格控制消息长度在 160 字符以内 |
| 长码禁止营销 | CL | 中 | 营销消息使用短码发送 |
| LGPD 违规（巴西） | BR | **高** | 严格遵守 LGPD，指定 DPO，咨询巴西法律顾问 |
| 数据保护法违规 | 全部国家 | 高 | 遵守各国数据保护法，保留合规记录 |
| 运营商内容过滤 | BR, CL, CO, PE | 中 | 使用标准模板，避免可疑内容，使用品牌域名 |
| Unicode 触发双倍计费 | 全部国家 | 低 | 统一使用 GSM-7 编码（去掉 á í ó ú 重音） |
| 新西兰禁止内容范围广 | NZ | 中 | 严格审查消息内容，避免敏感话题 |
| 巴西时区差异 | BR | 低 | 根据用户 DDD 区号判断时区，调整发送时间 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。各国法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询当地法律顾问以确保合规。阿根廷相关信息请参见单独的《AWS SMS 阿根廷专题研究报告》。
