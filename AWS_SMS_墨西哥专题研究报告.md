# 墨西哥 (MX, +52) AWS SMS 发送专题研究报告

> 调研日期：2026年4月7日
> 数据来源：AWS 官方文档、IFT/CRT 官网、墨西哥法律资料

---

## 一、AWS 上的墨西哥 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +52 |
| 短码 (Short Code) | **支持（申请约14周）** |
| 长码 (Long Code) | **不支持**（仅允许发送 OTP，见限制说明） |
| Sender ID | **支持（需注册，最低月发1000条）** |
| Toll-free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| MMS | 不支持（转换为 SMS+URL） |
| 国际发送 | 支持 |

**核心限制：**
1. Sender ID 仅支持 **Telcel、Movistar、AT&T** 三大网络
2. 长码**仅允许发送 OTP**，其他消息类型需使用短码或 Sender ID
3. 同一长码 **2分钟内超9条**消息将触发号码轮换
4. 含 URL 和品牌名的营销消息可能被运营商屏蔽
5. **政治推广会被运营商封锁**
6. 短码申请周期约14周

---

## 二、Sender ID 注册详细流程

### 2.1 注册方式

墨西哥 Sender ID 需通过 AWS 控制台或 Support Case 提交注册。

### 2.2 注册要求与限制

| 要求 | 说明 |
|------|------|
| **最低月发量** | **1,000 条/月**（必须达到此最低要求） |
| 支持的运营商 | **仅 Telcel、Movistar、AT&T** |
| Sender ID 格式 | 3-11个字母数字字符 |
| 注册审核 | 需 AWS 和运营商双重审核 |

> **重要：** 如果无法满足每月1,000条的最低发送量要求，建议改用短码。Sender ID 不覆盖小型 MVNO 运营商。

### 2.3 注册所需材料

| 材料 | 说明 |
|------|------|
| 公司法定名称 | 与注册文件一致 |
| 公司注册号 | 墨西哥公司：RFC（联邦纳税人注册号）；国际公司：EIN/VAT |
| 公司地址 | 完整注册地址 |
| 联系人信息 | 姓名、邮箱、电话 |
| 公司网站 | 官方网站 URL |
| Sender ID | 3-11个字母数字字符（如 "MyBrand"） |
| 消息样本 | 1-3条西班牙语消息模板 |
| 使用场景描述 | OTP / 事务性 / 营销等 |
| 预估月发送量 | 月度消息数量（须 ≥ 1,000 条） |

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
| Region | 你使用的 AWS 区域（如 us-east-1） |
| Resource Type | Sender ID Registration |
| Limit Type | 选择消息类型（见下方） |
| Limit Increase Value | **1**（即申请 1 个 Sender ID） |

**Limit Type 消息类型选项：**
- One Time Password/Two-Factor Authentication
- Promotional/Marketing
- Transactional
- Transactional/Notifications/OTP/2FA

### 2.5 Case Description 模板（Sender ID）

```
Subject: Request for Sender ID Registration - Mexico (MX)

We are requesting a Sender ID registration for Mexico (+52).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- RFC / EIN / VAT: [公司识别号]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Desired Sender ID: [品牌名，如 MyBrand]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)

Target Carriers: Telcel, Movistar, AT&T Mexico

Expected Monthly Volume: [预估月发送量，必须 ≥ 1,000 条]

Sample Messages (Spanish):
1. "[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos.
   No comparta este codigo."
2. "[Brand] Su pedido #MX12345 ha sido confirmado. Monto: MXN 2,500.
   Info: https://brand.com/orders/MX12345"
3. "[Brand] Alerta: Se detecto un inicio de sesion desde un nuevo
   dispositivo. Si no fue usted: https://brand.com/security"

Opt-in Process:
Users register on our website/app and provide their Mexican mobile number.
During registration, they check a consent box stating: "Acepto recibir
mensajes SMS de [Brand] para verificacion de cuenta y notificaciones
de transacciones."

Opt-out Mechanism:
Users can reply STOP to unsubscribe. They can also opt out via their
account settings or by emailing optout@brand.com.

We also request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.
```

### 2.6 注册时间线（Sender ID）

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 初审 | 1-3个工作日 |
| 运营商审核（Telcel/Movistar/AT&T） | 2-6周 |
| Sender ID 激活 | 1-2个工作日 |
| **总计** | **3-8周** |

---

## 三、短码申请详细流程

### 3.1 申请方式

墨西哥**不在** AWS 控制台直接支持短码申请的国家列表中，需通过 **AWS Support Case** 手动申请。

### 3.2 Support Case 填写指南

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
| Region | 你使用的 AWS 区域（如 us-east-1） |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1**（即申请 1 个专用短码） |

### 3.3 Case Description 模板（短码）

```
Subject: Request for Dedicated Short Code - Mexico (MX)

We are requesting a dedicated SMS short code for Mexico (+52).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- RFC / EIN / VAT: [公司识别号]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)
- Tertiary: Promotional offers (with explicit user consent)

Target Carriers: Telcel, Movistar, AT&T Mexico

Expected Monthly Volume: [预估月发送量，如 100,000 messages]

Sample Messages (Spanish):
1. "[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos.
   No comparta este codigo."
2. "[Brand] Su pedido #MX12345 ha sido confirmado. Monto: MXN 2,500.
   Entrega estimada: 10 Abr. Info: https://brand.com/o/MX12345"
3. "[Brand] Oferta especial: 20% de descuento en su proxima compra.
   Use codigo PROMO20. Valido hasta 15/04.
   Cancelar: Responda STOP"

Opt-in Process:
Users register on our website/app and provide their Mexican mobile number.
During registration, they check a consent box stating: "Acepto recibir
mensajes SMS de [Brand] para verificacion de cuenta, notificaciones
de transacciones y ofertas promocionales."

Opt-out Mechanism:
Users can reply STOP to unsubscribe. They can also opt out via their
account settings or by emailing optout@brand.com.

We understand that fees begin upon carrier initiation and are non-refundable.
We also request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.

Please provide the registration form and estimated timeline.
```

### 3.4 申请时间线（短码）

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 提供注册表格 | 1-3个工作日 |
| 填写并提交注册表格 | 取决于你 |
| 运营商审核 | 8-12周 |
| 短码配置完成 | 1-2周 |
| **总计** | **约14周** |

### 3.5 费用说明

| 费用项 | 说明 |
|--------|------|
| 短码月租 | 因国家而异，通常 $800-$1,500 USD/月（需向 AWS 确认） |
| 每条消息费 | 需下载 AWS 定价 CSV 或联系销售 |
| 一次性设置费 | 可能有（需确认） |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

> **重要：** 费用从向运营商提交申请那一刻开始计费，即使短码尚未配置完成。申请被拒绝已产生的费用也不退还。

---

## 四、长码限制详解

### 4.1 长码使用限制

墨西哥的长码有严格的使用限制：

| 限制项 | 详情 |
|--------|------|
| **允许的消息类型** | **仅限 OTP**（一次性密码） |
| 禁止的消息类型 | 事务性通知、营销消息、促销信息 |
| **频率限制** | 同一长码 2分钟内超9条消息触发号码轮换 |
| 号码轮换 | 超限后 AWS 自动切换到另一个号码发送 |

### 4.2 号码轮换的影响

| 影响 | 说明 |
|------|------|
| 发送者号码不一致 | 用户收到的消息可能来自不同号码 |
| 用户信任降低 | 多号码发送可能被视为可疑 |
| 双向 SMS 中断 | 用户回复可能发送到旧号码 |
| 运营商过滤风险增加 | 频繁轮换可能触发运营商反垃圾机制 |

> **建议：** 如果发送量较大或需要发送非 OTP 消息，强烈建议申请短码或注册 Sender ID。

---

## 五、墨西哥电信监管

### 5.1 监管机构

**注意：** 原监管机构 **IFT**（Instituto Federal de Telecomunicaciones，联邦电信研究所）于2025年10月17日被撤销，其职能由新成立的 **CRT**（Comision Reguladora de Telecomunicaciones，电信监管委员会）接管。

| 项目 | 详情 |
|------|------|
| 现任监管机构 | **CRT**（电信监管委员会） |
| 前监管机构 | IFT（2013-2025） |
| 消费者保护 | **PROFECO**（联邦消费者保护署） |

### 5.2 A2P SMS 监管现状

墨西哥的 A2P SMS 监管主要通过以下框架：
- 联邦电信和广播法（Ley Federal de Telecomunicaciones y Radiodifusion）
- 联邦个人数据保护法（LFPDPPP）
- 联邦消费者保护法
- 运营商自律机制（Telcel、Movistar、AT&T 各有内部过滤规则）

### 5.3 禁止/限制内容

| 类别 | 限制程度 | 说明 |
|------|---------|------|
| 未经同意的营销 | 禁止 | 必须获得明确同意 |
| **政治推广** | **严格禁止** | **运营商会直接封锁政治推广消息** |
| 含 URL 的营销消息 | **高风险** | URL + 品牌名组合可能被屏蔽 |
| 欺诈/误导性内容 | 禁止 | |
| 成人内容 | 限制 | |
| 赌博推广 | 限制 | 需合法许可 |
| 金融诈骗/钓鱼 | 禁止 | |

> **特别警告：** 墨西哥运营商对**政治推广消息**采取零容忍态度，会直接封锁发送号码。含 URL 和品牌名的营销消息也面临较高的屏蔽风险。

### 5.4 发送时间建议

虽无明确法定限制，行业最佳实践：
- **推荐发送时段：** 09:00-21:00 CST（UTC-6）
- 墨西哥横跨多个时区（UTC-5 至 UTC-7）
- 紧急/交易类消息除外
- 避免国定假日发送营销消息

---

## 六、墨西哥数据保护法 (LFPDPPP)

### 6.1 法律概述

**LFPDPPP**（Ley Federal de Proteccion de Datos Personales en Posesion de los Particulares，联邦个人数据保护法）于2010年颁布，是墨西哥规范私营部门处理个人数据的核心法律。由 **INAI**（国家透明、信息获取和个人数据保护研究所）负责执行。

### 6.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **隐私声明（Aviso de Privacidad）** | **强制要求**：收集个人数据前必须向数据主体提供隐私声明 |
| **同意** | 营销 SMS 必须获得明确同意 |
| **ARCO 权利** | 数据主体享有访问(Acceso)、更正(Rectificacion)、取消(Cancelacion)、反对(Oposicion)四项权利 |
| **目的限制** | 数据仅用于隐私声明中告知的目的 |
| **数据最小化** | 仅收集必要的个人数据 |
| **REVICA 注册表** | 消费者可登记号码拒绝商业营销 |

### 6.3 ARCO 权利详解

| 权利 | 说明 | 响应期限 |
|------|------|---------|
| **Acceso（访问）** | 了解其个人数据及处理方式 | 收到请求后20天内 |
| **Rectificacion（更正）** | 更正不准确或不完整的数据 | 收到请求后20天内 |
| **Cancelacion（取消/删除）** | 删除其个人数据 | 收到请求后20天内 |
| **Oposicion（反对）** | 反对出于特定目的的数据处理 | 收到请求后20天内 |

### 6.4 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| 未遵守隐私声明 | **100-160,000 天 UMA**（约 $1,500-$2,400,000 USD） |
| 未经同意处理数据 | **200-320,000 天 UMA**（约 $3,000-$4,800,000 USD） |
| 传输数据违规 | 双倍罚款 |
| 安全事故 | 视严重程度而定 |

> UMA（Unidad de Medida y Actualizacion）是墨西哥的计量和更新单位，2026年每日约 $113.14 MXN（约 $6.50 USD）。

### 6.5 REVICA（拒绝商业营销注册表）

- 类似于 Do Not Call Register
- 消费者可注册号码拒绝商业营销电话和短信
- **发送营销 SMS 前建议检查号码是否在 REVICA 中**
- 由 PROFECO 管理

---

## 七、运营商与号码格式

### 7.1 三大运营商

| 运营商 | 母公司 | 市场份额（约） | 备注 |
|--------|--------|-------------|------|
| **Telcel** | America Movil (Carlos Slim) | ~60% | 最大运营商，覆盖最广 |
| **Movistar** | Telefonica (西班牙) | ~20% | 第二大运营商 |
| **AT&T Mexico** | AT&T Inc. (美国) | ~15% | 2015年收购 Iusacell + Nextel |
| 其他 MVNO | 各异 | ~5% | Sender ID 不一定覆盖 |

> **Sender ID 仅支持 Telcel、Movistar、AT&T 三家网络。** 发送到其他 MVNO 用户的消息可能无法投递或显示随机号码。

### 7.2 号码格式

#### 基本结构
- 国家代码：+52
- 手机号码：10位数字
- 2019年起墨西哥取消了移动号码前的 "1" 前缀

#### 格式示例

| 格式 | 示例 | 说明 |
|------|------|------|
| 国际格式（AWS 使用） | `+5215512345678` | 部分情况含15前缀 |
| 标准国际格式 | `+525512345678` | 10位手机号 |
| 国内格式 | `55 1234 5678` | 无前缀 |

#### 区号示例

| 城市 | 区号 | 示例号码 |
|------|------|---------|
| 墨西哥城 | 55 | +52 55 XXXX XXXX |
| 瓜达拉哈拉 | 33 | +52 33 XXXX XXXX |
| 蒙特雷 | 81 | +52 81 XXXX XXXX |
| 普埃布拉 | 222 | +52 222 XXX XXXX |
| 坎昆 | 998 | +52 998 XXX XXXX |

#### 正确的 AWS SMS 号码格式
```
+525512345678      ← 墨西哥城
+523312345678      ← 瓜达拉哈拉
+528112345678      ← 蒙特雷
+522221234567      ← 普埃布拉
```

> **注意：** 2019年墨西哥号码格式改革后，移动号码不再需要 "1" 前缀。但部分旧系统可能仍使用 +521XXXXXXXXXX 格式。建议同时支持 10位和 11位号码（含旧 "1" 前缀）的验证。

### 7.3 运营商技术细节

- Sender ID 仅支持三大运营商（Telcel、Movistar、AT&T）
- **同一长码2分钟内超9条触发号码轮换**
- 长码仅允许 OTP 消息
- 含 URL + 品牌名的营销消息可能被屏蔽
- 政治推广消息会被直接封锁
- 支持号码携带（携号转网）
- 固定电话不可达
- 支持消息拼接

---

## 八、消息模板范例（西班牙语）

### 8.1 字符编码说明

| 编码 | 单条上限 | 拼接每段 | 何时触发 |
|------|---------|---------|---------|
| GSM-7 | 160字符 | 153字符 | 仅基本字符 |
| UCS-2 (Unicode) | 70字符 | 67字符 | 含重音等特殊字符 |

**西班牙语重音字符与 GSM-7 兼容性：**

| 字符 | GSM-7 兼容 | 建议 |
|------|-----------|------|
| e (`é`) | **是** | 可保留 |
| n (`ñ`) | **是** | 可保留 |
| a (`á`) | **否** | 去掉重音写 `a` |
| i (`í`) | **否** | 去掉重音写 `i` |
| o (`ó`) | **否** | 去掉重音写 `o` |
| u (`ú`) | **否** | 去掉重音写 `u` |
| u (`ü`) | **是** | 可保留 |

> **建议：** 墨西哥用户普遍习惯在短信中不使用重音符号。使用 GSM-7 兼容写法可将每条消息从70字符提升到160字符，显著降低成本。

### 8.2 OTP 验证码模板

**GSM-7 兼容版（推荐，160字符/条）：**
```
[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos.
No comparta este codigo con nadie.
```
(89字符，1条消息)

**含重音版（Unicode，70字符/条）：**
```
[Brand] Su código de verificación es: 385291. Válido por 5 minutos.
No comparta este código.
```
(79字符，2条消息 = 双倍费用)

### 8.3 交易通知模板

**订单确认（GSM-7）：**
```
[Brand] Su pedido #MX12345 ha sido confirmado. Monto: MXN 2,500.
Entrega estimada: 10 Abr. Info: https://brand.com/o/MX12345
```
(115字符，1条)

**配送通知 - 已发货（GSM-7）：**
```
[Brand] Su pedido #MX12345 ha sido enviado. Seguimiento:
https://brand.com/track/MX12345 Entrega estimada: 10 Abr.
```
(108字符，1条)

**配送通知 - 派送中（GSM-7）：**
```
[Brand] Su paquete esta en camino. Llegada estimada: hoy 14-17hrs.
Seguimiento: https://brand.com/track/MX12345
```
(103字符，1条)

**配送通知 - 已签收（GSM-7）：**
```
[Brand] Su pedido #MX12345 ha sido entregado.
Califique su experiencia: https://brand.com/review/MX12345
```
(97字符，1条)

**付款确认（GSM-7）：**
```
[Brand] Pago recibido: MXN 2,500 (pedido #MX12345).
Recibo: https://brand.com/receipt/MX12345
```
(87字符，1条)

**账户安全警报（GSM-7）：**
```
[Brand] Alerta: Inicio de sesion desde nuevo dispositivo.
Si no fue usted: https://brand.com/security
```
(93字符，1条)

### 8.4 营销消息模板（需 opt-in 同意，高屏蔽风险）

> **警告：** 墨西哥运营商可能屏蔽含 URL 和品牌名的营销消息。建议：（1）使用短码而非 Sender ID 发送营销消息；（2）避免在消息中同时包含品牌名和 URL；（3）测试不同运营商的投递率。

**促销活动（GSM-7，谨慎使用）：**
```
[Brand] 20% de descuento en su proxima compra.
Use codigo PROMO20. Valido hasta 15/04.
Cancelar: Responda STOP
```
(106字符，1条)

**新品通知（GSM-7，谨慎使用）：**
```
[Brand] Nueva coleccion disponible! Sea el primero en verla.
Compre ahora: https://brand.com/new
Cancelar: Responda STOP
```
(115字符，1条)

**会员积分提醒（GSM-7）：**
```
[Brand] Tiene 500 puntos por vencer el 30/04.
Canjee ahora: https://brand.com/rewards
Cancelar: Responda STOP
```
(110字符，1条)

### 8.5 Opt-in 确认模板

**首次订阅确认（GSM-7）：**
```
[Brand] Bienvenido! Recibira alertas de cuenta y notificaciones.
Frecuencia variable. Cancelar: Responda STOP
```
(104字符，1条)

### 8.6 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 使用西班牙语（墨西哥变体） |
| 编码 | 强烈建议 GSM-7（去掉 `á í ó ú` 重音） |
| `é` 和 `ñ` | 可保留（在 GSM-7 中） |
| 每条退出指引 | 营销消息必须包含 "Responda STOP" |
| **URL 风险** | 含 URL + 品牌名的营销消息可能被屏蔽 |
| **政治内容** | **绝对禁止** |
| 货币符号 | 使用 `MXN` 或 `$`（`$` 在 GSM-7 中） |
| 长码 OTP 限制 | 长码仅用于 OTP，其他消息用短码或 Sender ID |

---

## 九、定价信息

### 9.1 AWS 定价

| 费用项 | 金额 |
|--------|------|
| 短码月租 | 通常 $800-$1,500 USD/月（需向 AWS 确认） |
| Sender ID 月费 | 通常免费（需确认） |
| 每条消息费 | 需下载 AWS 定价 CSV |
| 一次性短码设置费 | 可能有（需确认） |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

**获取完整定价：**
1. 下载 AWS 定价 CSV：`https://aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

### 9.2 成本优化建议

1. **使用 GSM-7 编码** — 去掉 `á í ó ú` 重音，每条160字符 vs Unicode 的70字符
2. **精简消息内容** — 保持单条消息在160字符内避免拼接
3. **OTP 用长码** — 长码月租低于短码，OTP 场景可用长码
4. **控制长码发送频率** — 2分钟内不超9条，避免触发号码轮换
5. **维护号码列表** — 定期清理无效号码，检查 REVICA
6. **避免 URL 被屏蔽** — 营销消息中 URL 使用品牌自有域名，测试投递率
7. **评估 Sender ID vs 短码** — 月发量 ≥ 1,000 可用 Sender ID，否则用短码

---

## 十、合规清单

在墨西哥发送 SMS 前，确保完成：

- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 发布隐私声明（Aviso de Privacidad）
- [ ] 注册 Sender ID（月发 ≥ 1,000 条）或申请短码
- [ ] 遵守 LFPDPPP 个人数据保护法
- [ ] 检查 REVICA（拒绝商业营销注册表）
- [ ] 消息内容使用西班牙语
- [ ] 每条营销消息包含退订方式（Responda STOP）
- [ ] **绝对不发送政治推广内容**
- [ ] 谨慎使用含 URL 和品牌名的营销消息
- [ ] 长码仅用于 OTP 消息
- [ ] 控制长码发送频率（2分钟内 ≤ 9条）
- [ ] 发送时间限制在 09:00-21:00 CST（UTC-6）
- [ ] 正确格式化号码（+52 + 10位号码）
- [ ] 使用 GSM-7 编码（去掉 `á í ó ú` 重音，保留 `é ñ`）
- [ ] 保留同意和退出记录
- [ ] 响应 ARCO 权利请求（20天内）
- [ ] 申请提高默认 $1.00 USD/月消费阈值
- [ ] 不在事务性消息中混入营销内容
- [ ] 确认 Sender ID 覆盖目标运营商（仅 Telcel/Movistar/AT&T）

---

## 十一、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| **政治推广被封锁** | **极高** | 绝对不发送任何政治推广内容 |
| 营销消息含 URL 被屏蔽 | **高** | 使用品牌域名，测试投递率，避免 URL + 品牌名组合 |
| Sender ID 仅覆盖三大运营商 | 高 | 接受 MVNO 用户可能无法收到消息 |
| 短码申请14周等待期 | 中 | 提前规划，尽早提交申请 |
| 长码 OTP 限制 | 中 | 非 OTP 消息使用短码或 Sender ID |
| 2分钟9条频率限制 | 中 | 实施发送速率控制，分散发送 |
| 号码轮换导致用户困惑 | 中 | 控制长码发送频率，使用 Sender ID 或短码 |
| LFPDPPP 违规罚款 | 高 | 严格遵守数据保护法，发布隐私声明 |
| 号码格式（旧 "1" 前缀） | 低 | 同时支持10位和11位号码验证 |
| Unicode 导致成本翻倍 | 低 | 统一使用 GSM-7 兼容写法 |
| Sender ID 月发1000条最低要求 | 低 | 评估发送量，不足1000条改用短码 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。墨西哥法规和 AWS 服务支持可能随时变更（注意原 IFT 已被 CRT 取代）。实际使用前请在 AWS 控制台确认最新信息，并咨询墨西哥当地法律顾问以确保合规。
