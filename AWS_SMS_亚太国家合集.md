# AWS SMS 亚太国家合集报告

> 调研日期：2026年4月7日
> 覆盖国家：日本 (JP)、香港 (HK)、台湾 (TW)、马来西亚 (MY)、以色列 (IL)
> 数据来源：AWS End User Messaging SMS 官方文档、各国电信监管机构资料

---

## 一、各国能力对比表

| 项目 | 日本 JP +81 | 香港 HK +852 | 台湾 TW +886 | 马来西亚 MY +60 | 以色列 IL +972 |
|------|------------|-------------|-------------|----------------|--------------|
| 短码 | 支持 | 不支持 | 不支持 | **支持（唯一方式）** | 不支持 |
| 长码 | **不支持** | 支持 | **支持（唯一方式）** | 不支持 | 支持 |
| Sender ID | 支持 | 支持（无需注册） | 不支持 | 不支持 | 支持（无需注册） |
| Toll-Free | 不支持 | 不支持 | 不支持 | 不支持 | 不支持 |
| 双向 SMS | 支持 | 支持 | 支持 | 支持 | 支持 |
| 消息拼接 | 支持 | 支持 | 支持 | 支持 | 支持 |
| 国际发送 | 支持 | 支持 | 支持 | 支持 | 支持 |
| 注册要求 | 无需注册 | 无需注册 | 无需注册 | 短码需 Support Case | 无需注册 |
| 单价 (USD/条) | 需确认 | $0.036 | $0.075 | $0.065 | $0.014 |
| 监管机构 | MIC/总務省 | OFCA | NCC | MCMC | MOC |
| 数据保护法 | APPI | PDPO | PDPA | PDPA 2010 | PPL 5741 |
| 主要语言编码 | Unicode (70字符/条) | Unicode (70字符/条) | Unicode (70字符/条) | GSM-7 (160字符/条) | Unicode (70字符/条) |

---

## 二、各国详细信息

---

### 2.1 日本 (JP, +81)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +81 |
| 短码 (Short Code) | **支持** |
| 长码 (Long Code) | 不支持 |
| Sender ID | **支持（无需注册）** |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |

**核心特点：** 日本支持短码和 Sender ID 两种发送方式，但不支持长码。Sender ID 无需预注册，是最便捷的发送方式。短码需通过 AWS Support Case 申请。

#### 号码申请流程

**方式一：Sender ID（推荐，快速）**

无需注册，直接在 AWS 控制台设置即可使用。在发送时指定 SenderID 参数为品牌名（3-11个字母数字字符）。

**方式二：短码申请**

日本不在 AWS 控制台直接支持短码申请的国家列表中，需通过 **AWS Support Case** 手动申请。

**Support Case 填写指南：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域（如 ap-northeast-1） |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型（OTP / Transactional / Promotional） |
| Limit Increase Value | **1**（即申请 1 个专用短码） |

**Case Description 模板：**

```
Subject: Request for Dedicated Short Code - Japan (JP)

We are requesting a dedicated SMS short code for Japan (+81).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Contact Phone: [联系电话]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes for user authentication
- Secondary: Transactional notifications

Target Carriers: NTT Docomo, au (KDDI), SoftBank, Rakuten Mobile

Expected Monthly Volume: [预估月发送量]

Sample Messages (Japanese):
1. "[Brand] 認証コード: 385291 有効期限5分 このコードを他者に教えないでください"
2. "[Brand] ご注文 #JP12345 を確認しました 配送予定: 4月10日"

Opt-in Process:
Users register on our website/app and provide their Japanese mobile number.
During registration, they check a consent box stating: "SMSによる認証コード
及び取引通知の受信に同意します。"

Opt-out Mechanism:
Users can reply STOP to unsubscribe, or opt out via account settings
at https://brand.com/settings or by emailing optout@brand.com.

We request an increase to our monthly SMS spend limit from $1.00 to
$[desired amount] USD.
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

**总務省（Ministry of Internal Affairs and Communications, MIC）** 是日本电信监管机构。

**主要法规：**
- 《特定電子メールの送信の適正化等に関する法律》（特定电子邮件传输规制法）— 2002年制定，2008年修订，涵盖 SMS 营销
- 《電気通信事業法》（电气通信事业法）— 基础电信法
- 《不正アクセス禁止法》— 禁止未经授权访问

**对 SMS 的关键规定：**
- 商业营销 SMS 必须获得事前同意（opt-in）
- 必须明确标识发送者信息
- 必须提供退订方式
- 违规可处 1年以下懲役 或 100万日元以下罰金（法人最高 3000万日元）

**发送时间建议：**
- 推荐发送时段：09:00-21:00 JST（UTC+9）
- 避免深夜和清晨发送
- 紧急/交易类消息除外

#### 数据保护法

**APPI（個人情報の保護に関する法律，个人信息保护法）** 是日本核心数据保护法律，2022年修订版已生效。

| 要求 | 详情 |
|------|------|
| 用户同意 | 处理个人信息需告知利用目的，敏感信息需明确同意 |
| 数据访问权 | 个人有权请求披露其个人数据 |
| 更正/删除权 | 可请求更正或停止使用 |
| 跨境传输 | 向境外传输需满足额外条件（同意/充分保护国/合同保障） |
| 数据泄露通知 | 发生泄露须通知个人信息保护委员会（PPC）和本人 |
| 违规处罚 | 最高 1亿日元罚金（法人），个人最高 1年监禁 |

#### 运营商与号码格式

**四大运营商：**

| 运营商 | 市场份额 | 备注 |
|--------|---------|------|
| NTT Docomo | 最大 | d系列服务 |
| au (KDDI) | 第二 | povo 子品牌 |
| SoftBank | 第三 | Y!mobile/LINEMO 子品牌 |
| Rakuten Mobile | 第四 | 2020年进入市场 |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +81 + 去掉首0的号码 | `+819012345678` |
| 固定电话 | +81 + 去掉首0的号码 | `+81312345678`（东京） |

- 手机号码通常以 070/080/090 开头，发送时去掉首位 0
- 正确格式：`+819012345678`（非 `+810901234578`）
- 固定电话不可达（SMS 仅限手机）

#### 特殊限制

- **Unicode 编码**：日语消息必须使用 Unicode，单条仅 70 字符，拼接时每段 67 字符
- 日本运营商对内容有额外过滤机制
- 高频发送相同内容可能触发限流
- Sender ID 可能在部分运营商上显示为数字号码而非品牌名

---

### 2.2 香港 (HK, +852)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +852 |
| 短码 (Short Code) | 不支持 |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持（无需注册）** |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |

**核心特点：** 香港支持长码和 Sender ID，且均无需注册，是进入门槛最低的亚太市场之一。

#### 号码申请流程

**方式一：Sender ID（推荐，最快）**

无需注册，直接在 AWS 控制台设置品牌名即可。

**方式二：长码申请**

在 AWS 控制台直接购买，或通过 Support Case 申请。

**Support Case 填写指南（如需申请长码）：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域 |
| Resource Type | Dedicated SMS Long Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

#### 监管机构与法规

**OFCA（通讯事務管理局，Office of the Communications Authority）** 是香港电信监管机构。

**主要法规：**
- 《非应邀电子讯息条例》（Unsolicited Electronic Messages Ordinance, UEMO）Cap.593 — 2007年生效，规范商业电子讯息（包括 SMS）
- 《电讯条例》（Telecommunications Ordinance）Cap.106

**UEMO 对 SMS 的关键规定：**
- 商业 SMS 必须包含发送者身份和联系方式
- 必须提供退订机制
- 退订请求须在 10 个工作日内处理
- 禁止在用户退订后继续发送
- 违规可处最高 **100万港元罚款**

**发送时间建议：**
- 推荐发送时段：09:00-21:00 HKT（UTC+8）
- 无法定发送时间限制，但遵循行业最佳实践

#### 数据保护法

**PDPO（個人資料（私隱）條例，Personal Data (Privacy) Ordinance）** Cap.486 是香港核心数据保护法律。

| 要求 | 详情 |
|------|------|
| 数据收集 | 须以合法、公平方式收集，告知用途 |
| 用途限制 | 仅限收集时声明的用途或直接相关用途 |
| 数据访问权 | 个人有权查阅和更正其个人数据 |
| 数据保留 | 不应超过实现目的所需期限 |
| 数据安全 | 须采取适当措施保障数据安全 |
| 直接营销 | 首次使用个人数据进行直接营销前须获得同意 |
| 违规处罚 | 最高 **100万港元罚款** + **5年监禁**（严重违规） |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 备注 |
|--------|------|
| CSL (HKT) | 最大运营商，1010/one2free |
| 3 Hong Kong | 和记电讯 |
| SmarTone | |
| China Mobile Hong Kong | 中国移动香港 |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +852 + 8位号码 | `+85291234567` |

- 香港号码统一 8 位，无区号
- 手机号通常以 5/6/7/9 开头
- 固定电话以 2/3 开头，不可达
- 正确格式：`+852XXXXXXXX`

#### 特殊限制

- 中文消息（繁体）使用 Unicode 编码，单条仅 70 字符
- 英文消息使用 GSM-7 编码，单条 160 字符
- Sender ID 无需注册，但部分运营商可能将字母 Sender ID 替换为数字号码
- 支持双向 SMS，可使用标准 STOP/HELP 退订

---

### 2.3 台湾 (TW, +886)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +886 |
| 短码 (Short Code) | 不支持 |
| 长码 (Long Code) | **支持（唯一支持类型）** |
| Sender ID | **不支持** |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |

**核心特点：** 台湾仅支持长码发送，不支持 Sender ID 和短码。消息将显示为长码号码而非品牌名。

#### 号码申请流程

台湾仅支持长码，可在 AWS 控制台直接购买或通过 Support Case 申请。

**Support Case 填写指南：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域 |
| Resource Type | Dedicated SMS Long Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

**Case Description 模板：**

```
Subject: Request for Dedicated Long Code - Taiwan (TW)

We are requesting a dedicated SMS long code for Taiwan (+886).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes
- Secondary: Transactional notifications

Target Carriers: Chunghwa Telecom, Taiwan Mobile, Far EasTone

Expected Monthly Volume: [预估月发送量]

Sample Messages (Traditional Chinese):
1. "[Brand] 您的驗證碼為：385291，有效期5分鐘。請勿將此碼告知他人。"
2. "[Brand] 您的訂單 #TW12345 已確認。預計配送日期：4月10日。"

Opt-in/Opt-out:
Users opt in during registration. Reply STOP to unsubscribe.
```

#### 监管机构与法规

**NCC（國家通訊傳播委員會，National Communications Commission）** 是台湾电信监管机构。

**主要法规：**
- 《電信管理法》— 基础电信法
- 相关行政命令和管理办法

**对 SMS 的关键规定：**
- 禁止发送垃圾短信
- 运营商有权过滤可疑批量短信
- 营销类 SMS 需获得用户同意

**发送时间建议：**
- 推荐发送时段：09:00-21:00 CST（UTC+8）
- 避免深夜发送营销消息

#### 数据保护法

**PDPA（個人資料保護法，Personal Data Protection Act）** 是台湾核心数据保护法律。

| 要求 | 详情 |
|------|------|
| 用户同意 | 收集和使用个人资料需告知当事人并取得同意 |
| 目的限制 | 仅限特定目的范围内使用 |
| 数据访问权 | 当事人有权查阅、更正、删除其个人资料 |
| 跨境传输 | 主管机关可限制国际传输 |
| 违规处罚 | 行政罚锾最高 **500万新台币**；违反刑事规定者最高 **5年有期徒刑** + **100万新台币罚金** |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 备注 |
|--------|------|
| 中華電信 (Chunghwa Telecom) | 最大运营商 |
| 台灣大哥大 (Taiwan Mobile) | 第二大 |
| 遠傳電信 (Far EasTone) | 第三大 |
| 台灣之星 (Taiwan Star) | 已与台灣大哥大合并 |
| 亞太電信 (Asia Pacific Telecom) | 已与遠傳電信合并 |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +886 + 去掉首0的号码 | `+886912345678` |

- 手机号码以 09 开头（10位），发送时去掉首位 0
- 正确格式：`+886912345678`（非 `+8860912345678`）
- 固定电话不可达

#### 特殊限制

- **仅支持长码**，无法使用品牌名作为发送者
- 中文消息（繁体）使用 Unicode 编码，单条仅 70 字符
- 无 Sender ID 意味着用户看到的是一个国际号码，可能影响识别度和信任度
- 建议在消息开头加品牌标识，如 `[Brand]`

---

### 2.4 马来西亚 (MY, +60)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +60 |
| 短码 (Short Code) | **支持（唯一发送方式）** |
| 长码 (Long Code) | 不支持 |
| Sender ID | 不支持 |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |

**核心特点：** 马来西亚仅支持通过短码发送，需通过 AWS Support Case 申请。2019年起 MCMC 加强了对 A2P SMS 的监管。

#### 号码申请流程

马来西亚不在 AWS 控制台直接支持短码申请的国家列表中，需通过 **AWS Support Case** 手动申请。

**Support Case 填写指南：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域（如 ap-southeast-1） |
| Resource Type | Dedicated SMS Short Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

**Case Description 模板：**

```
Subject: Request for Dedicated Short Code - Malaysia (MY)

We are requesting a dedicated SMS short code for Malaysia (+60).

Company Information:
- Company Name: [公司法定名称]
- Company Address: [公司地址]
- Contact Email: [联系邮箱]
- Website: [公司网站]

Use Case:
- Primary: OTP verification codes
- Secondary: Transactional notifications

Target Carriers: Maxis, Celcom, Digi, U Mobile

Expected Monthly Volume: [预估月发送量]

Sample Messages (Malay/English):
1. "[Brand] Kod pengesahan anda: 385291. Sah selama 5 minit.
   Jangan kongsi kod ini."
2. "[Brand] Pesanan anda #MY12345 telah disahkan.
   Penghantaran dijangka: 10 Apr."

Opt-in Process:
Users opt in during registration by checking a consent box.

Opt-out Mechanism:
Reply STOP to unsubscribe, or manage preferences at
https://brand.com/settings
```

**申请时间线：**

| 阶段 | 预计时间 |
|------|---------|
| AWS 确认收到 | 24小时内 |
| AWS 提供注册表格 | 1-3个工作日 |
| 运营商审核 | 4-12周 |
| 短码配置完成 | 1-2周 |
| **总计** | **6-15周** |

**费用说明：**

| 费用项 | 说明 |
|--------|------|
| 短码月租 | 需向 AWS 确认（通常 $800-$1,500 USD/月） |
| 每条消息费 | $0.065/条 |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

#### 监管机构与法规

**MCMC（Malaysian Communications and Multimedia Commission，马来西亚通讯及多媒体委员会）** 是电信监管机构。

**主要法规：**
- Communications and Multimedia Act 1998（CMA 1998）
- 2019年 MCMC 加强 A2P SMS 监管，要求运营商更严格审查商业短信

**对 SMS 的关键规定：**
- 商业 SMS 须获得用户同意
- 禁止发送欺诈、误导性内容
- 运营商有权过滤未经授权的批量短信
- 2019年后运营商实施更严格的 A2P 过滤

**发送时间建议：**
- 推荐发送时段：09:00-21:00 MYT（UTC+8）
- 避免在伊斯兰节日期间发送大量营销消息

#### 数据保护法

**PDPA 2010（Personal Data Protection Act 2010）** 是马来西亚核心数据保护法律。

| 要求 | 详情 |
|------|------|
| 用户同意 | 处理个人数据须获得数据主体同意 |
| 告知义务 | 须告知数据收集目的、类型、权利等 |
| 数据访问权 | 个人有权查阅和更正其个人数据 |
| 数据安全 | 须采取适当措施保障数据安全 |
| 数据保留 | 不应超过实现目的所需期限 |
| 跨境传输 | 仅限已获批准的国家/地区 |
| 违规处罚 | 最高 **50万令吉罚款** 及/或 **3年监禁** |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 备注 |
|--------|------|
| Maxis | 最大运营商，Hotlink 预付子品牌 |
| Celcom Axiata | Axiata 集团 |
| Digi (CelcomDigi) | 与 Celcom 合并为 CelcomDigi |
| U Mobile | 第四大运营商 |
| YES (YTL) | 较小运营商 |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +60 + 去掉首0的号码 | `+60121234567` |

- 手机号码以 01 开头（10-11位），发送时去掉首位 0
- 正确格式：`+60121234567` 或 `+601112345678`
- 固定电话不可达

#### 特殊限制

- **仅支持短码**，申请周期较长
- 马来语消息使用 GSM-7 编码（160字符/条），因马来语使用拉丁字母
- 如消息包含中文或阿拉伯文字符则切换为 Unicode（70字符/条）
- 2019年后 MCMC 加强监管，运营商过滤更严格
- 建议提供马来语和英语双语消息选项

---

### 2.5 以色列 (IL, +972)

#### AWS 能力总览

| 项目 | 详情 |
|------|------|
| 国家代码 | +972 |
| 短码 (Short Code) | 不支持 |
| 长码 (Long Code) | **支持** |
| Sender ID | **支持（无需注册）** |
| Toll-Free | 不支持 |
| 双向 SMS | **支持** |
| 消息拼接 | 支持 |
| 国际发送 | 支持 |

**核心特点：** 以色列支持长码和 Sender ID，且均无需注册，进入门槛极低。单价仅 $0.014/条，是本报告中最经济的国家。

#### 号码申请流程

**方式一：Sender ID（推荐，最快）**

无需注册，直接在 AWS 控制台设置品牌名即可。

**方式二：长码申请**

可在 AWS 控制台直接购买或通过 Support Case 申请。

**Support Case 填写指南（如需申请长码）：**

| 字段 | 填写值 |
|------|-------|
| Issue Type | Service Limit Increase |
| Service | Service Quotas |
| Category | AWS End User Messaging SMS (Pinpoint) |
| Region | 使用的 AWS 区域 |
| Resource Type | Dedicated SMS Long Codes |
| Limit Type | 选择消息类型 |
| Limit Increase Value | **1** |

#### 监管机构与法规

**MOC（Ministry of Communications，通信部）** 是以色列电信监管机构。

**主要法规：**
- 《通信法》（Communications Law, Bezeq and Broadcasts）
- 《反垃圾短信法》（修正案第40号，Amendment 40 to the Communications Law）— 2008年生效

**修正案第40号关键规定：**
- 未经同意发送商业短信视为违法
- 每条违规短信可被罚款最高 **1,000 新谢克尔**（无需证明损害）
- 营销短信仅限 **工作时间** 发送
- 必须提供退订机制
- 禁止周五日落后至周六日落期间发送（安息日，Shabbat）

**发送时间建议：**
- 推荐发送时段：08:00-21:00 IST（UTC+2/+3 夏令时）
- **避免安息日**：周五日落至周六日落
- 避免犹太节日期间发送营销消息

#### 数据保护法

**PPL 5741-1981（Protection of Privacy Law，隐私保护法）** 是以色列核心数据保护法律。以色列被欧盟认定为提供"充分"数据保护水平。

| 要求 | 详情 |
|------|------|
| 数据库注册 | 包含500人以上个人数据的数据库须向 ILITA 注册 |
| 用户同意 | 处理敏感数据须获得同意 |
| 数据访问权 | 个人有权查阅其个人数据 |
| 更正权 | 可请求更正不准确的数据 |
| 数据安全 | 须遵守信息安全条例（2017年） |
| 违规处罚 | 最高 **5年监禁**（严重违规） |

#### 运营商与号码格式

**主要运营商：**

| 运营商 | 备注 |
|--------|------|
| Cellcom | 最大运营商之一 |
| Partner (Orange) | |
| Pelephone (Bezeq) | |
| HOT Mobile | |
| Golan Telecom | 低价运营商 |

**号码格式：**

| 类型 | 格式 | 示例 |
|------|------|------|
| 手机号 | +972 + 去掉首0的号码 | `+972501234567` |

- 手机号码以 05 开头（10位），发送时去掉首位 0
- 正确格式：`+972501234567`（非 `+9720501234567`）
- 固定电话不可达

#### 特殊限制

- **希伯来语为 RTL（从右到左）语言**，使用 Unicode 编码，单条仅 70 字符
- 英文消息使用 GSM-7 编码，单条 160 字符
- **安息日限制**：周五日落至周六日落避免发送营销消息
- 犹太节日期间同样建议避免营销消息
- 被欧盟认定为充分保护水平，有利于与欧盟企业合作

---

## 三、各语言消息模板

### 3.1 字符编码总览

| 语言 | 编码 | 单条上限 | 拼接每段 | 备注 |
|------|------|---------|---------|------|
| 日语 | Unicode (UCS-2) | **70字符** | 67字符 | 假名/汉字强制 Unicode |
| 繁体中文（台湾/香港） | Unicode (UCS-2) | **70字符** | 67字符 | 中文强制 Unicode |
| 马来语 | GSM-7 | **160字符** | 153字符 | 拉丁字母，成本最优 |
| 希伯来语 | Unicode (UCS-2) | **70字符** | 67字符 | RTL 方向，希伯来字母 |
| 英语 | GSM-7 | **160字符** | 153字符 | 成本最优 |

---

### 3.2 日语模板（Unicode, 70字符/条）

> **重要：** 日语消息全部使用 Unicode 编码，单条上限仅 70 字符。务必精简消息内容以控制成本。

**OTP 验证码：**
```
[Brand] 認証コード: 385291 有効期限5分 他者に教えないでください
```
（34字符，1条消息）

**订单确认：**
```
[Brand] 注文#JP12345確認済 配送予定:4/10 詳細: https://brand.com/o/JP12345
```
（52字符，1条消息）

**发货通知：**
```
[Brand] 注文#JP12345発送済 追跡: https://brand.com/track/JP12345
```
（43字符，1条消息）

**安全警告：**
```
[Brand] 新端末からのログイン検出 心当たりがない場合: https://brand.com/security
```
（50字符，1条消息）

**营销消息（需 opt-in）：**
```
[Brand] 全品20%OFF コード:PROMO20 4/15迄 停止: https://brand.com/unsub/{TOKEN}
```
（52字符，1条消息）

**Opt-in 确认：**
```
[Brand] SMS通知の登録完了 停止はSTOPと返信 または https://brand.com/unsub/{TOKEN}
```
（53字符，1条消息）

**注意事项：**
- 使用半角数字和半角英文字母可节省字符
- 尽量使用缩写（例如「確認済」代替「確認しました」）
- 避免使用 emoji（占2字符且强制 Unicode，但日语已经是 Unicode）
- URL 使用短路径

---

### 3.3 繁体中文模板（台湾/香港，Unicode, 70字符/条）

> **重要：** 繁体中文消息使用 Unicode 编码，单条上限仅 70 字符。台湾和香港用语略有差异，以下分别标注。

**OTP 验证码（台湾）：**
```
[Brand] 您的驗證碼為：385291，有效期5分鐘。請勿告知他人。
```
（33字符，1条消息）

**OTP 验证码（香港）：**
```
[Brand] 驗證碼：385291，5分鐘內有效。請勿向他人透露。
```
（31字符，1条消息）

**订单确认（通用）：**
```
[Brand] 訂單#TW12345已確認 預計配送:4/10 詳情: https://brand.com/o/TW12345
```
（51字符，1条消息）

**发货通知（台湾）：**
```
[Brand] 訂單#TW12345已出貨 追蹤: https://brand.com/track/TW12345
```
（43字符，1条消息）

**安全警告（通用）：**
```
[Brand] 偵測到新裝置登入 若非本人操作請至: https://brand.com/security
```
（42字符，1条消息）

**营销消息（需 opt-in，台湾）：**
```
[Brand] 全館8折 優惠碼PROMO20 至4/15 取消訂閱: https://brand.com/unsub/{TOKEN}
```
（52字符，1条消息）

**营销消息（需 opt-in，香港）：**
```
[Brand] 全場8折 優惠碼PROMO20 至4/15 退訂: https://brand.com/unsub/{TOKEN}
```
（49字符，1条消息）

**注意事项：**
- 台湾用「出貨」，香港用「發貨」或「寄出」
- 台湾用「取消訂閱」，香港用「退訂」
- 货币：台湾用 NT$ / 新台幣，香港用 HK$ / 港幣
- 使用半角数字和半角标点可节省字符

---

### 3.4 马来语模板（GSM-7, 160字符/条）

> **优势：** 马来语使用拉丁字母，GSM-7 编码可用，单条 160 字符，成本最优。

**OTP 验证码：**
```
[Brand] Kod pengesahan anda: 385291. Sah selama 5 minit. Jangan kongsi kod ini dengan sesiapa.
```
（93字符，1条消息）

**订单确认：**
```
[Brand] Pesanan #MY12345 anda telah disahkan. Jumlah: MYR 150. Penghantaran dijangka: 10 Apr. Info: https://brand.com/o/MY12345
```
（127字符，1条消息）

**发货通知：**
```
[Brand] Pesanan #MY12345 telah dihantar. Jejak: https://brand.com/track/MY12345 Dijangka tiba: 10 Apr.
```
（103字符，1条消息）

**安全警告：**
```
[Brand] Amaran: Log masuk dari peranti baharu dikesan. Jika bukan anda: https://brand.com/security
```
（98字符，1条消息）

**营销消息（需 opt-in）：**
```
[Brand] Diskaun 20% untuk pembelian seterusnya! Guna kod PROMO20. Sah sehingga 15/04. Berhenti: https://brand.com/unsub/{TOKEN}
```
（127字符，1条消息）

**Opt-in 确认：**
```
[Brand] Selamat datang! Anda akan menerima pemberitahuan SMS. Balas STOP untuk berhenti atau layari https://brand.com/unsub/{TOKEN}
```
（129字符，1条消息）

**注意事项：**
- 马来语基于拉丁字母，完全兼容 GSM-7
- 避免使用马来语特殊引号或 Unicode 字符
- 马来西亚为多语言社会，部分用户偏好英语，可考虑双语消息
- 货币使用 MYR 或 RM

---

### 3.5 希伯来语模板（Unicode, 70字符/条, RTL）

> **重要：** 希伯来语为 **RTL（从右到左）** 书写方向，使用 Unicode 编码，单条仅 70 字符。在包含数字和 URL 时会出现双向文本（Bidi），需注意排版。

**OTP 验证码：**
```
[Brand] קוד האימות שלך: 385291 תקף ל-5 דקות. אל תשתף קוד זה.
```
（40字符，1条消息）

**订单确认：**
```
[Brand] הזמנה #IL12345 אושרה. סכום: ₪150. משלוח: 10/04. https://brand.com/o/IL12345
```
（56字符，1条消息）

**发货通知：**
```
[Brand] הזמנה #IL12345 נשלחה. מעקב: https://brand.com/track/IL12345
```
（44字符，1条消息）

**安全警告：**
```
[Brand] התראה: כניסה ממכשיר חדש. אם לא את/ה: https://brand.com/security
```
（47字符，1条消息）

**营销消息（需 opt-in）：**
```
[Brand] 20% הנחה! קוד PROMO20 עד 15/04. ביטול: https://brand.com/unsub/{TOKEN}
```
（51字符，1条消息）

**注意事项：**
- 希伯来语 + 数字/URL 混排时，手机上的显示方向可能出乎意料（Bidi 问题）
- 建议将 URL 放在消息末尾以减少 Bidi 混乱
- `₪`（新谢克尔符号）占 Unicode 字符
- **安息日限制：** 避免周五日落至周六日落发送营销消息
- 犹太节日期间同样建议避免发送
- 部分以色列用户偏好英语，可考虑语言偏好设置
- 性别敏感：希伯来语有性别变位，使用 `את/ה`（你）等中性写法

---

## 四、各国定价对比

> 以下价格为 AWS 消息费参考值，不含运营商附加费。实际费率请在 AWS 控制台确认。

| 排名 | 国家 | 单价 (USD/条) | 编码类型 | 实际成本/70字符消息 | 备注 |
|------|------|-------------|---------|-------------------|------|
| 1 | 台湾 TW | $0.075 | Unicode | $0.075 | 仅长码 |
| 2 | 马来西亚 MY | $0.065 | GSM-7 | $0.065 | 仅短码，160字符/条 |
| 3 | 香港 HK | $0.036 | Unicode | $0.036 | 长码 + Sender ID |
| 4 | 日本 JP | 需确认 | Unicode | 需确认 | 短码 + Sender ID |
| 5 | 以色列 IL | $0.014 | Unicode | $0.014 | 长码 + Sender ID，最便宜 |

### 成本优化建议

1. **马来西亚最具成本优势**：马来语使用 GSM-7 编码，单条 160 字符，相同信息量下成本仅为 Unicode 国家的约一半
2. **以色列单价最低**：$0.014/条，即使 Unicode 限制为 70 字符，成本仍极低
3. **日语/繁体中文/希伯来语务必精简**：70 字符上限意味着超出即拼接计费（2条 = 双倍费用）
4. **避免 emoji**：虽然 Unicode 国家的消息本来就是 Unicode 编码，但 emoji 占额外字符空间
5. **URL 使用短路径**：品牌自有域名 + 短路径（如 `/o/ID`）比长 URL 节省大量字符

### 费用结构

| 费用项 | 说明 |
|--------|------|
| 每条消息费 | 按消息片段（segment）计费，见上表 |
| 短码月租（日本/马来西亚） | 通常 $800-$1,500 USD/月（需向 AWS 确认） |
| 长码月费（台湾/香港/以色列） | 因国家而异（约 $2-$27 USD/月） |
| Sender ID 费用 | 通常免费 |
| 默认消费阈值 | $1.00 USD/月（需申请提高） |

> **重要：** 短码费用从向运营商提交申请那一刻开始计费，即使短码尚未配置完成。

---

## 五、合规清单

### 5.1 通用合规项（适用所有5国）

- [ ] 获取所有接收者的明确 opt-in 同意
- [ ] 实施有效的 opt-out 机制（支持 STOP 回复或提供退出链接）
- [ ] 消息中包含品牌标识
- [ ] 发送时间限制在当地工作时间（通常 09:00-21:00）
- [ ] 正确格式化号码（E.164 格式）
- [ ] 保留同意和退出记录
- [ ] URL 使用品牌自有域名（避免 bit.ly 等短链接）
- [ ] 事务性消息不混入营销内容
- [ ] 实施号码格式验证
- [ ] 定期清理无效号码列表
- [ ] 准备备用通信渠道

### 5.2 各国特殊合规项

**日本 (JP)：**
- [ ] 遵守《特定电子邮件传输规制法》
- [ ] 遵守 APPI 数据保护要求
- [ ] 消息使用日语
- [ ] 跨境传输数据满足 APPI 额外条件
- [ ] 发生数据泄露时通知 PPC

**香港 (HK)：**
- [ ] 遵守 UEMO 非应邀电子讯息条例
- [ ] 退订请求 10 个工作日内处理
- [ ] 遵守 PDPO 数据保护要求
- [ ] 直接营销首次使用前获得同意
- [ ] 消息包含发送者身份和联系方式

**台湾 (TW)：**
- [ ] 遵守 NCC 电信管理规定
- [ ] 遵守 PDPA 个人资料保护法
- [ ] 消息使用繁体中文
- [ ] 消息开头加品牌标识（无 Sender ID）

**马来西亚 (MY)：**
- [ ] 遵守 CMA 1998 及 MCMC 相关规定
- [ ] 遵守 PDPA 2010 数据保护要求
- [ ] 跨境数据传输仅限已获批准的国家/地区
- [ ] 注意 2019 年后更严格的 A2P 过滤规则
- [ ] 提供马来语消息选项

**以色列 (IL)：**
- [ ] 遵守修正案第40号反垃圾短信法
- [ ] **安息日不发送营销消息**（周五日落至周六日落）
- [ ] 犹太节日期间避免营销消息
- [ ] 营销短信仅限工作时间
- [ ] 遵守 PPL 5741 隐私保护法
- [ ] 500人以上数据库须在 ILITA 注册

---

## 六、风险评估

| 风险 | 涉及国家 | 严重程度 | 缓解措施 |
|------|---------|---------|---------|
| Unicode 70字符限制导致成本增加 | JP, HK, TW, IL | 中 | 精简消息内容，使用短路径 URL，控制在单条以内 |
| 短码申请周期长（6-15周） | JP, MY | 中 | 提前规划，尽早提交申请；JP 可先用 Sender ID |
| 短码申请被拒 | JP, MY | 高 | 准备详尽用例说明，确保内容合规 |
| Sender ID 被运营商替换为数字号码 | JP, HK, IL | 低 | 在消息正文开头加 [Brand] 标识 |
| 台湾无 Sender ID，用户信任度低 | TW | 中 | 在消息开头明确标识品牌，配合 App/邮件通知 |
| 马来西亚仅支持短码，无替代方案 | MY | 高 | 提前申请，同时准备备用通信渠道 |
| 运营商内容过滤导致投递失败 | JP, MY | 中 | 避免可疑内容，使用标准模板，使用品牌域名 |
| 日本 APPI 跨境传输限制 | JP | 中 | 确保 AWS Region 选择符合 APPI 要求，或获得用户同意 |
| 以色列安息日发送限制 | IL | 低 | 在发送调度中排除安息日和犹太节日 |
| 希伯来语 RTL/Bidi 显示问题 | IL | 低 | URL 放消息末尾，充分测试各运营商显示效果 |
| PDPA/PDPO 数据保护违规 | HK, TW, MY | 高 | 严格遵守当地数据保护法，咨询当地法律顾问 |
| 马来西亚跨境数据传输限制 | MY | 中 | 确认目标国家在 PDPA 2010 获批名单中 |

---

> **免责声明：** 本报告基于2026年4月公开资料编写。各国法规和 AWS 服务支持可能随时变更，实际使用前请在 AWS 控制台确认最新信息，并咨询当地法律顾问以确保合规。
