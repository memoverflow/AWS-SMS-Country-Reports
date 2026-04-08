# 阿根廷 (AR, +54) AWS SMS 发送专题研究报告

> 调研日期：2026年4月7日
> 数据来源：AWS 官方文档、ENACOM 官网、Twilio/Vonage 文档、阿根廷法律资料

---

## 一、AWS 上的阿根廷 SMS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +54 |
| 短码 (Short Code) | **支持（唯一发送方式）** |
| 长码 (Long Code) | 不支持 |
| Sender ID | 不支持（本地网络不支持动态 Sender ID） |
| Toll-free | 不支持 |
| 双向 SMS | **不支持** |
| 消息拼接 | 支持 |
| MMS | 不支持（转换为 SMS+URL） |
| 国际发送 | 支持 |

**核心限制：** 阿根廷在 AWS 上只能通过短码发送，且不支持双向短信。这意味着无法通过 STOP/HELP 关键字实现标准退订机制。

---

## 二、费用总览

### 2.1 号码费用

| 费用项目 | 一次性费用 | 月度费用 | 备注 |
|---------|-----------|---------|------|
| 短码 (Short Code) | $500 | $335 | 需通过 Support Case 确认，此为参考价格 |

### 2.2 每条消息费用

| 号码类型 | 基础价格/条 | 备注 |
|---------|-----------|------|
| 短码 | $0.09059 | 每条消息段（message segment）计费 |

### 2.3 月度消费阈值

新账户默认 $1.00 USD/月，需申请提高。

---

## 三、短码申请详细流程

### 3.1 申请方式

阿根廷**不在** AWS 控制台直接支持短码申请的国家列表中（控制台直接支持的仅有：智利、芬兰、德国、印度、荷兰、西班牙、英国、美国），需通过 **AWS Support Case** 手动申请。

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
| Limit Type | 选择消息类型（见下方） |
| Limit Increase Value | **1**（即申请 1 个专用短码） |

**Limit Type 消息类型选项：**
- One Time Password/Two-Factor Authentication
- Promotional/Marketing
- Transactional
- Transactional/Notifications/OTP/2FA

### 3.3 Case Description 模板

```
Subject: Request for Dedicated Short Code - Argentina (AR)

We are requesting a dedicated SMS short code for Argentina (+54).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications (order confirmations, delivery updates)

Target Carriers: Claro, Movistar (Telefonica), Personal (Telecom Argentina)

Expected Monthly Volume: [预估月发送量，如 50,000 messages]

Sample Messages (Spanish):
1. "[Brand] Su codigo de verificacion es: 385291. Valido por 5 minutos.
   No comparta este codigo."
2. "[Brand] Su pedido #AR12345 ha sido confirmado. Monto: ARS 15,000.
   Info: https://brand.com/orders/AR12345"
3. "[Brand] Alerta: Se detecto un inicio de sesion desde un nuevo
   dispositivo. Si no fue usted: https://brand.com/security"

Opt-in Process:
Users register on our website/app and provide their Argentine mobile number.
During registration, they check a consent box stating: "Acepto recibir
mensajes SMS de [Brand] para verificacion de cuenta y notificaciones
de transacciones."

Opt-out Mechanism:
Since two-way SMS is not supported in Argentina, we provide an unsubscribe
link in each message (https://brand.com/unsub/{token}). Users can also
opt out via email at optout@brand.com or through their account settings.

We understand that fees begin upon carrier initiation and are non-refundable.
We also request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.

Please provide the registration form and estimated timeline.
```

### 3.4 申请时间线

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 提供注册表格 | 1-3个工作日 |
| 填写并提交注册表格 | 取决于你 |
| 运营商审核 | 4-12周 |
| 短码配置完成 | 1-2周 |
| **总计** | **6-15周** |

### 3.5 费用说明

| 费用项 | 说明 |
|--------|------|
| 短码月租 | 因国家而异，通常 $800-$1,500 USD/月（需向 AWS 确认） |
| 每条消息费 | 需下载 AWS 定价 CSV 或联系销售 |
| 一次性设置费 | 可能有（需确认） |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

> **重要：** 费用从向运营商提交申请那一刻开始计费，即使短码尚未配置完成。申请被拒绝已产生的费用也不退还。

### 3.6 需要准备的材料

1. 公司基本信息（名称、地址、联系方式、网站）
2. 详细的使用场景描述
3. 预估月发送量
4. 西班牙语消息样本（至少2-3条）
5. 用户 opt-in 流程描述
6. opt-out 机制说明
7. 是否涉及 PHI（受保护健康信息）的声明
8. AWS 发送的短码注册表格（收到后填写回传）

---

## 四、阿根廷电信监管 (ENACOM)

### 4.1 监管机构

**ENACOM**（Ente Nacional de Comunicaciones，国家通信管理局）是阿根廷通信监管机构，2016年成立，合并了前 AFSCA 和 AFTIC。

### 4.2 A2P SMS 监管现状

**ENACOM 目前没有针对 A2P SMS 的专门法规。** 监管主要通过：
- 运营商自律机制
- 数据保护法（Ley 25.326）
- 消费者保护法
- Registro Nacional No Llame（国家谢绝来电登记）

### 4.3 禁止/限制内容

| 类别 | 限制程度 |
|------|---------|
| 未经同意的营销 | 禁止 |
| 欺诈/误导性内容 | 禁止 |
| 成人内容 | 需年龄验证 |
| 赌博推广 | 仅合法运营商 |
| 政治宣传 | 选举期间有特殊限制 |
| 金融诈骗/钓鱼 | 禁止 |

### 4.4 发送时间建议

虽无明确法定限制，行业最佳实践：
- **推荐发送时段：** 08:00-21:00 ART（UTC-3）
- 紧急/交易类消息除外
- 避免周日和国定假日发送营销消息

---

## 五、阿根廷数据保护法 (Ley 25.326)

### 5.1 法律概述

**Ley 25.326**（个人数据保护法）是阿根廷核心数据保护法律，被欧盟认定为提供"充分"数据保护水平。

### 5.2 对 SMS 业务的关键要求

| 要求 | 详情 |
|------|------|
| **用户同意** | 必须获得明确的、知情的同意才能发送营销 SMS |
| **数据访问权** | 用户每6个月可免费请求获取其个人数据 |
| **更正/删除权** | 必须在5个工作日内响应删除请求 |
| **退出权** | 用户有权要求从营销数据库中删除其信息 |
| **数据库注册** | 允许个人数据归档的数据库必须在国家登记处注册 |
| **数据保留** | 不应超过必要期限 |

### 5.3 违规处罚

| 处罚类型 | 详情 |
|---------|------|
| 行政处罚 | 警告至罚款 1,000-100,000 比索，可能关闭数据库 |
| 刑事 - 插入虚假数据 | 最高 **2年监禁** |
| 刑事 - 披露虚假信息 | **6个月至3年监禁** |
| 公职人员 | 加重处罚 |
| 罚款缴纳期限 | 通知后10个工作日内 |

### 5.4 Registro Nacional No Llame（谢绝来电登记）

- 公民可免费注册号码拒绝商业广告
- 虽主要针对电话，但精神上延伸至 SMS
- **建议发送营销 SMS 前检查号码是否在该名单中**

---

## 六、运营商与号码格式

### 6.1 三大运营商

| 运营商 | 母公司 | 备注 |
|--------|--------|------|
| Claro | America Movil (墨西哥) | 三大之一 |
| Movistar | Telefonica (西班牙) | 覆盖南部 |
| Personal | Telecom Argentina | 覆盖北部 |

### 6.2 号码格式（极其重要）

**阿根廷号码格式是最容易出错的之一：**

#### 基本结构
- 国家代码：+54
- 国内号码长度：10位（区号 + 用户号码）

#### 区号示例

| 城市 | 区号 | 示例号码 |
|------|------|---------|
| 布宜诺斯艾利斯 | 11 | +54 11 XXXX XXXX |
| 科尔多瓦 | 351 | +54 351 XXX XXXX |
| 马德普拉塔 | 223 | +54 223 XXX XXXX |
| 里奥加列戈斯 | 2966 | +54 2966 XX XXXX |

#### SMS 与语音呼叫的关键区别

| 场景 | 格式 | 示例 |
|------|------|------|
| **SMS 发送（正确）** | +54 + 区号 + 用户号码 | `+5411XXXXXXXX` |
| 语音呼叫（国际） | +54 **9** + 区号 + 用户号码 | `+54911XXXXXXXX` |
| 国内拨打 | 0 + 区号 + **15** + 用户号码 | `011-15-XXXX-XXXX` |

> **SMS 不加 `9` 前缀！不加 `15` 前缀！** 这是最常见的格式错误。

#### 正确的 AWS SMS 号码格式
```
+5411XXXXXXXX      ← 布宜诺斯艾利斯
+54351XXXXXXX      ← 科尔多瓦
+54223XXXXXXX      ← 马德普拉塔
+542966XXXXXX      ← 里奥加列戈斯
```

### 6.3 运营商技术细节

- 动态 Sender ID 不被支持
- 运营商有垃圾信息自动过滤
- 高频发送可能触发限流
- 重复内容可能被标记
- 支持号码携带（携号转网）
- 固定电话不可达

---

## 七、不支持双向 SMS 的替代退出方案

### 7.1 问题

AWS 上阿根廷不支持双向 SMS，用户无法回复 STOP 退订。必须提供替代 opt-out 机制。

### 7.2 四种替代方案

#### 方案一：短链接退出（推荐）

每条 SMS 中包含唯一退出链接：

```
[Brand] Su codigo: 385291. Valido 5 min. Cancelar SMS: https://brand.com/unsub/abc123
```

**实现要点：**
- 为每个用户生成唯一 token
- 退出页面使用西班牙语
- 一键完成退出，无需登录
- 退出后24小时内停止发送
- 保留退出记录作为合规证据

**退出页面文案范例：**
```
Cancelar suscripcion SMS

Confirmar que desea dejar de recibir mensajes SMS de [Brand].

[Boton: Confirmar cancelacion]

Su numero: +54 XX XXXX-XXXX
Al confirmar, dejara de recibir mensajes en un plazo de 24 horas.
```

#### 方案二：邮件退出

```
Cancelar SMS: envie email a optout@brand.com con su numero de telefono.
```

#### 方案三：客服电话退出

```
Cancelar SMS: llame al +54-XXX-XXX-XXXX
```

#### 方案四：App 内设置

在 App 设置中提供 SMS 偏好管理页面。

### 7.3 最佳实践

1. 每条营销消息**必须**包含退出指引
2. 使用品牌自有域名短链接（避免 bit.ly 等被运营商过滤）
3. 退出应即时生效，24小时内停止发送
4. 保留所有退出记录
5. 在首次 opt-in 时明确告知退出方式
6. 定期发送提醒告知如何退出

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

> **建议：** 阿根廷用户普遍习惯在短信中不使用重音符号。使用 GSM-7 兼容写法可将每条消息从 70 字符提升到 160 字符，显著降低成本。

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
[Brand] Su pedido #AR12345 ha sido confirmado. Monto: ARS 15,000.
Entrega estimada: 10 Abr. Info: https://brand.com/o/AR12345
```
(119字符，1条)

**配送通知 - 已发货（GSM-7）：**
```
[Brand] Su pedido #AR12345 ha sido enviado. Seguimiento:
https://brand.com/track/AR12345 Entrega estimada: 10 Abr.
```
(108字符，1条)

**配送通知 - 派送中（GSM-7）：**
```
[Brand] Su paquete esta en camino. Llegada estimada: hoy 14-17hs.
Seguimiento: https://brand.com/track/AR12345
```
(101字符，1条)

**配送通知 - 已签收（GSM-7）：**
```
[Brand] Su pedido #AR12345 ha sido entregado.
Califique su experiencia: https://brand.com/review/AR12345
```
(97字符，1条)

**付款确认（GSM-7）：**
```
[Brand] Pago recibido: ARS 15,000 (pedido #AR12345).
Recibo: https://brand.com/receipt/AR12345
```
(87字符，1条)

**账户安全警报（GSM-7）：**
```
[Brand] Alerta: Inicio de sesion desde nuevo dispositivo.
Si no fue usted: https://brand.com/security
```
(93字符，1条)

### 8.4 营销消息模板（需 opt-in 同意）

**促销活动（GSM-7）：**
```
[Brand] Oferta: 20% de descuento en su proxima compra.
Use codigo PROMO20. Valido hasta 15/04.
Cancelar: https://brand.com/unsub/{TOKEN}
```
(127字符，1条)

**新品通知（GSM-7）：**
```
[Brand] Nueva coleccion disponible! Sea el primero en verla.
Compre ahora: https://brand.com/new
Cancelar: https://brand.com/unsub/{TOKEN}
```
(126字符，1条)

**会员积分提醒（GSM-7）：**
```
[Brand] Tiene 500 puntos por vencer el 30/04.
Canjee ahora: https://brand.com/rewards
Cancelar: https://brand.com/unsub/{TOKEN}
```
(120字符，1条)

### 8.5 Opt-in 确认模板

**首次订阅确认（GSM-7）：**
```
[Brand] Bienvenido! Recibira alertas de cuenta y notificaciones.
Frecuencia variable. Cancelar: https://brand.com/unsub/{TOKEN}
```
(114字符，1条)

### 8.6 注意事项汇总

| 要点 | 说明 |
|------|------|
| 语言 | 使用西班牙语（阿根廷变体） |
| 编码 | 强烈建议 GSM-7（去掉 `á í ó ú` 重音） |
| `é` 和 `ñ` | 可保留（在 GSM-7 中） |
| 每条退出指引 | 营销消息必须包含 |
| 退出方式 | 使用网页链接（无双向 SMS） |
| 货币符号 | 使用 `ARS` 或 `$`（`$` 在 GSM-7 中） |

---

## 九、定价信息

### 9.1 AWS 定价

AWS 未在公开文档直接列出阿根廷每条费率，需通过：
1. 下载 AWS 定价 CSV：`aws.amazon.com/end-user-messaging/pricing/`
2. 联系 AWS 销售获取报价
3. 登录 AWS 控制台查看

**已知费用结构：**
- 短码月租：通常 $800-$1,500 USD/月（需向 AWS 确认）
- 每条消息费：需参考 CSV
- 默认消费阈值：$1.00 USD/月（需申请提高）

### 9.2 成本优化建议

1. **使用 GSM-7 编码** — 每条160字符 vs Unicode 的70字符，可减少约50%消息段数
2. **精简消息内容** — 保持单条消息在160字符内避免拼接
3. **维护号码列表** — 定期清理无效号码，减少无效发送
4. **低峰时段批量发送** — 可能获得更好的投递率

---

## 十、合规清单

在阿根廷发送 SMS 前，确保完成：

- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 实施有效的 opt-out 机制（网页链接，因 AWS 不支持双向）
- [ ] 遵守 Ley 25.326 数据保护要求
- [ ] 检查 Registro Nacional No Llame（谢绝来电名单）
- [ ] 消息内容使用西班牙语
- [ ] 发送时间限制在 08:00-21:00 ART（UTC-3）
- [ ] 每条营销消息包含退出链接
- [ ] 数据库在国家登记处注册
- [ ] 保留同意和退出记录（至少保留法定要求期限）
- [ ] 正确格式化号码（+54 + 区号 + 号码，**SMS 不加 9 前缀**）
- [ ] 使用 GSM-7 编码（去掉 `á í ó ú` 重音，保留 `é ñ`）
- [ ] 准备备用通信渠道（WhatsApp/邮件）
- [ ] 响应数据删除请求（5个工作日内）

---

## 十一、风险评估

| 风险 | 严重程度 | 缓解措施 |
|------|---------|---------|
| 短码申请被拒 | 高 | 准备详尽的使用说明，确保内容合规 |
| 6-15周等待期 | 中 | 提前规划，尽早提交申请 |
| 运营商内容过滤 | 中 | 避免可疑内容，使用标准模板，使用品牌域名 |
| 单点故障（AWS 单一运营商合作伙伴） | 中 | 实施备用通信渠道 |
| 数据保护违规 | 高 | 严格遵守 Ley 25.326，咨询当地法律顾问 |
| 号码格式错误 | 中 | 实施严格的号码格式验证（不加9前缀） |
| Unicode 导致成本翻倍 | 低 | 统一使用 GSM-7 兼容写法 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。阿根廷法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询阿根廷当地法律顾问以确保合规。
