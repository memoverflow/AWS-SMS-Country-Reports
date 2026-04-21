# AWS End User Messaging Notify 服务完全指南

> 调研日期：2026年4月21日
> 数据来源：AWS 官方文档、AWS End User Messaging SMS User Guide、AWS 定价页面
> 适用场景：OTP 验证码、身份验证短信的快速发送

---

## 目录

1. [服务概述](#一服务概述)
2. [Notify vs 标准 SMS 对比](#二notify-vs-标准-sms-对比)
3. [核心概念](#三核心概念)
4. [服务层级（Tiers）](#四服务层级tiers)
5. [支持的国家/地区](#五支持的国家地区)
6. [定价详解](#六定价详解)
7. [快速上手指南](#七快速上手指南)
8. [消息发送详解](#八消息发送详解)
9. [模板系统](#九模板系统)
10. [专用号码集成](#十专用号码集成)
11. [消费限额管理](#十一消费限额管理)
12. [合规要求](#十二合规要求)
13. [配额与限制](#十三配额与限制)
14. [事件追踪与监控](#十四事件追踪与监控)
15. [故障排查](#十五故障排查)
16. [最佳实践](#十六最佳实践)

---

## 一、服务概述

### 1.1 什么是 Notify？

**AWS End User Messaging Notify** 是 AWS 推出的**托管式 OTP/验证码消息服务**。它允许你通过 AWS 托管的号码和预审批模板发送 SMS 及语音验证消息，**无需自行购买号码、无需运营商注册、无需管理号码池**。

简单来说：一个 API 调用，验证码就发出去了。

### 1.2 核心特点

| 特性 | 说明 |
|------|------|
| **零号码管理** | AWS 自动选择最佳发送号码，无需购买或注册 |
| **预审批模板** | 内置多语言验证码模板，已通过各国运营商审核 |
| **分钟级上线** | 创建配置后即可发送，无需等待数周的号码注册 |
| **内置防欺诈** | 强制启用 SMS Protect，自动过滤 AIT（人为膨胀流量） |
| **双渠道支持** | 同时支持 SMS 短信和 Voice 语音验证码 |
| **简化 API** | 只需配置 ID + 目标号码 + 模板变量，一次调用完成发送 |

### 1.3 适用场景

- 用户注册时的手机号验证
- 登录时的一次性密码（OTP）
- 密码重置验证
- 交易确认验证
- 任何需要身份验证码的场景

### 1.4 不适用场景

以下场景应使用标准 SMS（`SendTextMessage`）而非 Notify：

- 营销推广消息
- 订单通知 / 物流更新
- 自定义消息内容（非模板化）
- MMS 多媒体消息
- 需要特定发送号码的场景
- 双向对话型消息

---

## 二、Notify vs 标准 SMS 对比

### 2.1 功能对比

| 对比项 | Notify | 标准 SMS（SendTextMessage） |
|--------|--------|---------------------------|
| **号码管理** | AWS 托管，无需购买 | 需自行购买和注册号码 |
| **消息内容** | 预审批模板，变量替换 | 完全自定义内容 |
| **上线周期** | 分钟级 | 数天至数周（需号码注册） |
| **消息类型** | 仅 OTP/验证码 | 任意类型（OTP、营销、通知等） |
| **渠道** | SMS + Voice | SMS + MMS + Voice |
| **双向消息** | 不支持（AWS 托管号码） | 支持 |
| **Sender ID** | AWS 自动选择 | 可指定自定义 Sender ID |
| **防欺诈** | 强制启用 SMS Protect | 可选配置 |
| **运营商注册** | AWS 代管 | 部分国家需自行注册 |
| **计费** | Notify 服务费 + 通道费 | 仅通道费 |

### 2.2 决策树

```
你的消息是 OTP/验证码吗？
├── 是 → 需要自定义发送号码吗？
│   ├── 是 → 标准 SMS（或 Notify + 专用号码池）
│   └── 否 → 需要快速上线吗？
│       ├── 是 → ✅ Notify（分钟级上线）
│       └── 否 → 成本敏感吗？
│           ├── 是 → 标准 SMS（无 Notify 服务费）
│           └── 否 → ✅ Notify（省去运维开销）
└── 否 → 标准 SMS
```

### 2.3 与 OTP 功能对比

AWS End User Messaging 还有一个独立的 **OTP（One-Time Password）功能**，与 Notify 有所不同：

| 对比项 | Notify | OTP 功能 |
|--------|--------|---------|
| **消息模板** | 预审批多语言模板 | 自动生成的 OTP 消息 |
| **验证逻辑** | 需自行实现代码校验 | AWS 生成并验证 OTP |
| **计费** | Notify 费 + 通道费 | $0.045/次成功验证 + 通道费 |
| **渠道** | SMS + Voice | 仅 SMS |
| **灵活性** | 可选模板和语言 | 固定格式 |

**关键区别：** Notify 只负责「发消息」，验证码的生成和校验需要你自己实现。OTP 功能则是「生成 + 发送 + 校验」一条龙服务。

---

## 三、核心概念

### 3.1 Notify Configuration（配置）

Notify 配置是核心资源，包含以下信息：

| 属性 | 说明 |
|------|------|
| **Display Name** | 品牌显示名称，最长 15 字符，支持字母数字、下划线、连字符、空格 |
| **Use Case** | 使用场景，当前仅支持 `CODE_VERIFICATION` |
| **Enabled Channels** | 启用的渠道：SMS、VOICE 或两者都启用 |
| **Enabled Countries** | 启用的目标国家（取决于 Tier） |
| **Tier** | 服务层级：BASIC（默认）或 ADVANCED |
| **Default Template** | 默认模板（可选，未指定模板时使用） |
| **Associated Pool** | 关联的号码池（可选，用于专用号码） |
| **Deletion Protection** | 删除保护（防止误删） |
| **Tags** | 最多 50 个键值对标签 |

**配置状态流转：**

```
创建 → PENDING → ACTIVE（可发送）
                → REQUIRES_VERIFICATION（需额外审核）
                → REJECTED（被拒绝）
```

### 3.2 Display Name 审核

Display Name 提交后会经过 **Amazon Bedrock AI 自动审核**，检查内容包括：

- 是否包含不当内容
- 是否包含 URL
- 是否包含其他不允许的内容

**审核失败时：**
- 可通过 AWS Support 申诉
- 如代表其他组织使用品牌名，需提供 **30 天内签署的授权信（Letter of Authorization）**

### 3.3 AWS 托管号码（Managed Identities）

Notify 使用 AWS 维护的共享号码池自动路由消息。系统根据目标国家自动选择最佳号码发送，你无法指定具体的发送号码（除非关联自有号码池）。

### 3.4 SMS Protect

每个 Notify 配置都**强制启用** SMS Protect，且不可由用户关闭。SMS Protect 提供：

- 国家级别的发送限制
- AIT（人为膨胀流量）检测与过滤
- 防止 SMS 泵水攻击

---

## 四、服务层级（Tiers）

### 4.1 层级对比

| 对比项 | Basic（基础版） | Advanced（高级版） |
|--------|---------------|-------------------|
| **开通方式** | 创建配置后立即可用 | 需提交审核，3-5 个工作日 |
| **每日消息上限** | 200 条/配置 | 无限制 |
| **TPS（每秒事务数）** | 1 TPS | 25 TPS |
| **支持国家数** | 30 个预审批国家 | 所有 AWS 支持的国家 |
| **号码类型** | 长码、免费号码、Sender ID | 额外支持短码 |
| **号码池关联** | 支持 | 支持 |
| **SMS Protect** | 强制启用，不可关闭 | 强制启用，不可关闭 |

### 4.2 每日消息限制详细说明

**Basic 层级的限制规则：**
- 每个 Notify 配置每天最多发送 **200 条**消息
- 每个目标号码每天最多接收 **10 条**（每配置）
- 每个目标号码每天最多接收 **10 条**（全账户）

**Advanced 层级：**
- 配置级别无每日上限
- 每个目标号码的每日限制依然为 **10 条**

### 4.3 升级到 Advanced

**提交材料：**

1. **Opt-in 方式** — 选择语音（电话/IVR）或数字（Web 表单/App）
2. **同意证明** — 提供截图、脚本或 URL，展示如何获取用户同意
3. **商业信息** — 组织详情和消息使用场景说明

**关键要求：**
- 所有 opt-in 流程必须包含**条款与条件**和**隐私政策**的链接
- 审核通常 **3-5 个工作日**完成
- 通过自动化和人工审核

**升级状态跟踪：**

```
BASIC → PENDING_UPGRADE → ADVANCED（成功）
                        → REJECTED（被拒绝，查看拒绝原因）
```

---

## 五、支持的国家/地区

### 5.1 Basic 层级支持的 30 个国家

| 国家/地区 | ISO 代码 | 国家/地区 | ISO 代码 |
|-----------|---------|-----------|---------|
| 美国 | US | 日本 | JP |
| 加拿大 | CA | 韩国 | KR |
| 英国 | GB | 新加坡 | SG |
| 德国 | DE | 香港 | HK |
| 法国 | FR | 澳大利亚 | AU |
| 意大利 | IT | 新西兰 | NZ |
| 西班牙 | ES | 印度 | IN |
| 荷兰 | NL | 巴西 | BR |
| 比利时 | BE | 墨西哥 | MX |
| 瑞典 | SE | 哥伦比亚 | CO |
| 葡萄牙 | PT | 爱尔兰 | IE |
| 芬兰 | FI | 瑞士 | CH |
| 丹麦 | DK | 波多黎各 | PR |
| 美属萨摩亚 | AS | 关岛 | GU |
| 北马里亚纳群岛 | MP | 美属维尔京群岛 | VI |

> **注意：** 最后 4 个为美国领地，共享 +1 国际区号。

### 5.2 Advanced 层级

Advanced 层级支持 **AWS End User Messaging SMS 所支持的全部国家**（240+ 国家/地区）。部分 Advanced 专属国家可能需要关联自有号码池才能发送。

### 5.3 查询最新国家列表

```bash
# 查询 Basic 层级支持的国家
aws pinpoint-sms-voice-v2 list-notify-countries \
  --channels SMS \
  --tier BASIC

# 查询 Advanced 层级支持的国家
aws pinpoint-sms-voice-v2 list-notify-countries \
  --channels SMS \
  --tier ADVANCED

# 按使用场景过滤
aws pinpoint-sms-voice-v2 list-notify-countries \
  --channels SMS \
  --use-cases CODE_VERIFICATION
```

---

## 六、定价详解

### 6.1 计费结构

每条 Notify 消息的费用由两部分组成：

```
总费用 = Notify 服务费 + 通道费（SMS 或 Voice）
```

### 6.2 Notify 服务费（阶梯定价）

采用**阶梯式递减定价**，每个层级仅适用于该区间内的增量消息量：

| 层级 | 月度消息量 | 每条费用 |
|------|-----------|---------|
| 第 1 档 | 前 25,000 条 | $0.045 |
| 第 2 档 | 25,001 – 100,000 条 | $0.040 |
| 第 3 档 | 100,001 – 250,000 条 | $0.035 |
| 第 4 档 | 250,001 条以上 | $0.030 |

### 6.3 通道费（SMS）

通道费按目标国家收取，与标准 SMS 费率一致：

| 目标国家 | 基础价格 | 运营商费 | 通道费合计 |
|---------|---------|---------|----------|
| 美国 | $0.00700 | $0.00302 | $0.01002 |
| 英国 | $0.04000 | — | $0.04000 |
| 德国 | $0.09410 | — | $0.09410 |
| 日本 | $0.06860 | — | $0.06860 |
| 新加坡 | $0.04100 | — | $0.04100 |
| 澳大利亚 | $0.05000 | — | $0.05000 |
| 印度 | $0.00328 | — | $0.00328 |
| 巴西 | $0.03170 | — | $0.03170 |

> 完整各国 SMS 通道费请参考 [AWS 定价页面](https://aws.amazon.com/end-user-messaging/pricing/) 或下载 CSV。

### 6.4 标准 SMS 号码固定成本参考

使用标准 SMS 发送时，除了每条消息的通道费外，**部分号码类型**还需承担号码租赁和注册的固定成本。

**号码类型与费用（美国）：**

| 号码类型 | 一次性费用 | 月度费用 | 备注 |
|---------|-----------|---------|------|
| Sender ID | **$0** | **$0** | AWS 官方文档："There's no additional charge for using a sender ID"。**但美国不支持 Sender ID** |
| 10DLC 号码 | $0 | $1/号码 + $10/Campaign | 需完成 10DLC 注册流程（4-8 周） |
| Toll-Free 号码 | $0 | $2/号码 | 注册周期约 15 个工作日 |
| Short Code（短码） | $650 | $995/月 | 个性化短码 $1,500/月；预置约 12 周 |

**10DLC 注册费用：**

| 费用项 | 金额 | 类型 | 备注 |
|--------|------|------|------|
| 品牌注册（Brand Registration） | $4.50 | 一次性 | — |
| 品牌审核（Brand Vetting） | $41.50 | 一次性 | 强烈推荐，提升吞吐量 |
| Campaign 激活费（T-Mobile） | $50 | 一次性 | 目前暂缓收取 |
| Campaign 月费（常规） | $10/月 | 月度 | 标准 Campaign |
| Campaign 月费（低流量） | $2/月 | 月度 | 低流量 Campaign |

**每条消息通道费（美国）：**

| 号码类型 | 基础价格（AWS） | 运营商费用 | 合计 |
|---------|---------------|-----------|------|
| 10DLC | $0.00700 | $0.00302 | $0.01002 |
| Short Code | $0.00700 | $0.00289 | $0.00989 |
| Toll-Free | $0.00700 | $0.00300 | $0.01000 |
| Long Code | $0.00700 | $0.00302 | $0.01002 |
| 入站 SMS（Inbound） | $0.00750 | — | $0.00750 |

> **重要提示：**
> - **Sender ID 本身没有任何费用**（无月租、无注册费），在支持 Sender ID 的国家使用标准 SMS 时，固定成本为零，总成本 = 纯通道费
> - **美国不支持 Sender ID**，必须使用 10DLC / Toll-Free / Short Code
> - 对于**支持 Sender ID 的国家**（如德国、日本、意大利等），标准 SMS 的成本仅为通道费，与 Notify 的差距更大
> - Notify 无号码月租费、无注册费，但每条消息需额外支付 Notify 服务费（$0.045-$0.030）

### 6.5 费用计算示例

**场景：每月向美国发送 50,000 条验证码短信**

**方案 A：Notify（AWS 官方示例）**

| 费用项 | 计算 | 金额 |
|--------|------|------|
| Notify 服务费（第 1 档） | 25,000 × $0.045 | $1,125.00 |
| Notify 服务费（第 2 档） | 25,000 × $0.040 | $1,000.00 |
| SMS 通道费 | 50,000 × $0.01 | $500.00 |
| 号码月租 | 无 | $0 |
| 注册费用 | 无 | $0 |
| **月度合计** | | **$2,625.00** |

> 以上 Notify 数据来自 AWS 官方定价页面示例。通道费 $0.01 为 AWS 官方简化值（精确值 $0.01002）。

**方案 B：标准 SMS — 10DLC（最经济）**

| 费用项 | 计算 | 金额 |
|--------|------|------|
| 号码月租 | 1 × $1 | $1.00 |
| Campaign 月费 | 1 × $10 | $10.00 |
| 注册费摊销 | ($4.50 + $41.50) / 12 | $3.83 |
| SMS 通道费 | 50,000 × $0.01002 | $501.00 |
| **月度合计** | | **$515.83** |

> 10DLC 通道费 $0.01002 = 基础价 $0.007 + 运营商费 $0.00302，含了运营商费部分，因此比 Notify 通道费（AWS 官方简化为 $0.01）略高。

**方案 C：标准 SMS — Toll-Free（快速上线）**

| 费用项 | 计算 | 金额 |
|--------|------|------|
| 号码月租 | 1 × $2 | $2.00 |
| SMS 通道费 | 50,000 × $0.01000 | $500.00 |
| **月度合计** | | **$502.00** |

**方案 D：标准 SMS — Short Code（高吞吐量）**

| 费用项 | 计算 | 金额 |
|--------|------|------|
| 号码月租 | 1 × $995 | $995.00 |
| 一次性费用摊销 | $650 / 12 | $54.17 |
| SMS 通道费 | 50,000 × $0.00989 | $494.50 |
| **月度合计** | | **$1,543.67** |

### 6.6 四种方案对比（美国，50,000 条/月）

**方案本质区别：**

| 方案 | 号码形式 | 运维责任 | 核心特点 |
|------|---------|---------|---------|
| **Notify** | AWS 托管的共享号码池，自动选择最佳号码发送（Basic 层级可能用 Long Code / Toll-Free / Sender ID；Advanced 层级还可能用 Short Code）。你看不到也不控制具体号码 | AWS 全托管（号码、路由、合规） | 零运维，按条付服务费 |
| **10DLC** | 你购买并拥有的 10 位本地号码（如 +1-206-555-0100） | 你负责注册 Brand + Campaign | 美国 A2P 主流方式，成本最低 |
| **Toll-Free** | 你购买并拥有的 800/888 免费号码 | 你负责号码管理和注册验证 | 注册快，用户拨打免费 |
| **Short Code** | 你购买并拥有的 5-6 位短号码（如 12345） | 你负责申请和维护 | 吞吐量最高，品牌辨识度强 |

> **简单理解：** Notify 是"打车"（按次付费，省心但贵，你不知道来的是什么车），其他三种是"买车"（有固定成本，但跑得越多越划算，你完全控制用哪辆）。

**月度总成本对比：**

| 方案 | 月度总成本 | vs Notify | 上线周期 | 吞吐量 |
|------|----------|-----------|---------|--------|
| **Notify** | **$2,625** | — | 分钟级 | 1-25 TPS |
| 10DLC | $516 | 便宜 $2,109 | 4-8 周 | 1 MPS/号码 |
| Toll-Free | $502 | 便宜 $2,123 | ~15 工作日 | 3 MPS |
| Short Code | $1,544 | 便宜 $1,081 | 8-12 周 | 100+ MPS |

### 6.7 不同量级下的费用参考（美国，按号码类型对比）

| 月发送量 | Notify | 10DLC | Toll-Free | Short Code |
|---------|--------|-------|-----------|------------|
| 1,000 条 | $55 | $25 | $12 | $1,059 |
| 5,000 条 | $275 | $65 | $52 | $1,099 |
| 10,000 条 | $550 | $115 | $102 | $1,148 |
| 50,000 条 | $2,625 | $516 | $502 | $1,544 |
| 100,000 条 | $5,125 | $1,017 | $1,002 | $2,038 |
| 250,000 条 | $11,875 | $2,520 | $2,502 | $3,522 |
| 500,000 条 | $21,875 | $5,025 | $5,002 | $5,994 |

> **计算说明：**
> - Notify = 阶梯服务费 + 消息量 × $0.01 通道费，无号码月租
> - 10DLC = $1 号码费 + $10 Campaign 月费 + $4 注册摊销 + 消息量 × $0.01002
> - Toll-Free = $2 号码月租 + 消息量 × $0.01000
> - Short Code = $995 号码月租 + $54 一次性设置摊销 + 消息量 × $0.00989
> - Notify 阶梯：前 25,000 条 $0.045，25,001-100,000 条 $0.040，100,001-250,000 条 $0.035，250,001+ 条 $0.030

### 6.8 Sender ID 国家的成本对比（德国示例）

对于支持 Sender ID 的国家，标准 SMS **没有任何号码固定成本**（Sender ID 免费），总费用 = 纯通道费。

**场景：每月向德国发送 50,000 条验证码短信**

| 方案 | Notify 服务费 | 通道费 | 号码/注册费 | 月度总成本 |
|------|-------------|--------|-----------|----------|
| **Notify** | $2,125 | 50,000 × $0.0941 = $4,705 | $0 | **$6,830** |
| **标准 SMS（Sender ID）** | $0 | 50,000 × $0.0941 = $4,705 | $0 | **$4,705** |

> **差额：$2,125/月**，完全是 Notify 服务费。在通道费高的国家，Notify 服务费占总成本的比例反而变小（德国约 31%），但绝对差额不变。

**不同国家 50,000 条/月的 Notify vs Sender ID 对比：**

| 国家 | 通道费/条 | 标准 SMS 月成本 | Notify 月成本 | Notify 溢价 | 溢价比例 |
|------|---------|--------------|-------------|-----------|---------|
| 印度 | $0.00328 | $164 | $2,289 | +$2,125 | 1,296% |
| 美国 | $0.01002 | $501 | $2,625 | +$2,124 | 424% |
| 巴西 | $0.03170 | $1,585 | $3,710 | +$2,125 | 134% |
| 英国 | $0.04000 | $2,000 | $4,125 | +$2,125 | 106% |
| 德国 | $0.09410 | $4,705 | $6,830 | +$2,125 | 45% |

> **规律：** Notify 服务费是固定的 $2,125（50,000 条场景），不因国家而变。通道费低的国家（如印度），Notify 溢价比例极高；通道费高的国家（如德国），溢价比例较低但绝对额不变。

### 6.9 成本分析与选型建议

> **关键发现：**
>
> 1. **Notify 在所有量级下都是最贵的方案**，因为 $0.030-$0.045 的 Notify 服务费叠加在通道费之上
> 2. **Short Code 在低量级（<24,000 条）时比 Notify 还贵**，因为 $995/月的固定号码月租
> 3. **Short Code 在 ~24,000 条/月以上时开始比 Notify 便宜**，高量级优势更明显
> 4. **Toll-Free 在所有量级下都是最便宜的**（仅 $2/月号码费 + 通道费，但仅限美国/加拿大）
> 5. **10DLC 成本接近 Toll-Free**，但需要 4-8 周注册周期（仅美国）
> 6. **Sender ID 国家（德国、日本等）的标准 SMS 成本更低**——Sender ID 免费，无任何号码固定成本，总费用 = 纯通道费
>
> **选型建议：**
>
> | 场景 | 推荐方案 | 理由 |
> |------|---------|------|
> | 需要今天就上线发 OTP | **Notify** | 分钟级部署，无需注册 |
> | 美国，预算敏感，可等 2 周 | **Toll-Free** | 最低成本，注册较快 |
> | 美国，预算敏感，需高吞吐 | **10DLC**（低量）/ **Short Code**（高量） | 低月租 / 高 MPS |
> | Sender ID 国家（德/日/意等） | **标准 SMS + Sender ID** | 零号码成本，总费用 = 纯通道费 |
> | 月发送 <5,000 条，不想管号码 | **Notify** | 绝对成本低，运维为零 |
> | 月发送 >50,000 条 | **标准 SMS** | Notify 服务费过高（溢价 $2,000+/月） |

---

## 七、快速上手指南

### 7.1 前置条件

- 已有 AWS 账户
- IAM 用户/角色具有相应权限

### 7.2 最小 IAM 权限（仅发送）

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "sms-voice:SendNotifyTextMessage",
        "sms-voice:SendNotifyVoiceMessage",
        "sms-voice:DescribeNotifyTemplates",
        "sms-voice:DescribeNotifyConfigurations",
        "sms-voice:ListNotifyCountries",
        "sms-voice:PutMessageFeedback"
      ],
      "Resource": "*"
    }
  ]
}
```

### 7.3 完整管理权限

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "sms-voice:CreateNotifyConfiguration",
        "sms-voice:UpdateNotifyConfiguration",
        "sms-voice:DeleteNotifyConfiguration",
        "sms-voice:DescribeNotifyConfigurations",
        "sms-voice:SendNotifyTextMessage",
        "sms-voice:SendNotifyVoiceMessage",
        "sms-voice:DescribeNotifyTemplates",
        "sms-voice:ListNotifyCountries",
        "sms-voice:PutMessageFeedback",
        "sms-voice:SetNotifyMessageSpendLimitOverride",
        "sms-voice:DeleteNotifyMessageSpendLimitOverride"
      ],
      "Resource": "*"
    }
  ]
}
```

### 7.4 第一步：创建 Notify 配置

**AWS CLI：**

```bash
aws pinpoint-sms-voice-v2 create-notify-configuration \
  --display-name "YourBrand" \
  --use-case CODE_VERIFICATION \
  --enabled-channels SMS
```

**控制台：**

1. 打开 [AWS End User Messaging SMS 控制台](https://console.aws.amazon.com/sms-voice/)
2. 导航到 **Notify configurations**
3. 选择 **Create Notify configuration**
4. 输入品牌名称（Display Name）
5. 默认使用场景为 Code verification
6. 选择渠道：SMS、Voice 或两者都选
7. 可选配置：启用的国家、默认模板、删除保护

### 7.5 第二步：浏览可用模板

```bash
# 列出所有 SMS 模板
aws pinpoint-sms-voice-v2 describe-notify-templates \
  --filters '[{"Name":"channels","Values":["SMS"]}]'

# 按语言过滤
aws pinpoint-sms-voice-v2 describe-notify-templates \
  --filters '[{"Name":"language-codes","Values":["en"]}]'
```

### 7.6 第三步：发送测试消息

**AWS CLI：**

```bash
aws pinpoint-sms-voice-v2 send-notify-text-message \
  --notify-configuration-id "nc-1234567890abcdef0" \
  --destination-phone-number "+12065550100" \
  --template-id "notify-code-verification-english-001" \
  --template-variables '{"code":"123456"}'
```

**Python (boto3)：**

```python
import boto3

client = boto3.client('pinpoint-sms-voice-v2')

response = client.send_notify_text_message(
    NotifyConfigurationId='nc-1234567890abcdef0',
    DestinationPhoneNumber='+12065550100',
    TemplateId='notify-code-verification-english-001',
    TemplateVariables={'code': '123456'}
)

print(f"Message ID: {response['MessageId']}")
```

### 7.7 第四步：验证发送结果

在控制台或通过 CloudWatch 查看送达状态。也可通过配置 Event Destination（SNS、CloudWatch、Data Firehose）接收投递事件。

---

## 八、消息发送详解

### 8.1 API 列表

| API | 说明 |
|-----|------|
| `SendNotifyTextMessage` | 发送 SMS 验证码 |
| `SendNotifyVoiceMessage` | 发送 Voice 语音验证码 |
| `PutMessageFeedback` | 报告验证码是否成功送达和使用 |

### 8.2 请求参数

| 参数 | 必填 | 说明 |
|------|------|------|
| `NotifyConfigurationId` | 是 | 配置 ID 或 ARN |
| `DestinationPhoneNumber` | 是 | E.164 格式的目标号码（如 +12065550100） |
| `TemplateVariables` | 是 | 模板变量键值对，所有值为字符串类型 |
| `TemplateId` | 否 | 模板 ID，省略时使用配置的默认模板 |
| `ConfigurationSetName` | 否 | 用于事件路由的配置集 |
| `Context` | 否 | 自定义键值对，包含在事件记录中 |
| `DryRun` | 否 | 设为 `true` 时仅验证请求，不实际发送 |
| `TimeToLive` | 否 | 消息有效期（秒），默认 72 小时 |
| `MessageFeedbackEnabled` | 否 | 启用消息反馈追踪 |
| `VoiceId`（仅 Voice） | 否 | Amazon Polly 语音（如 JOANNA、MATTHEW） |

### 8.3 DryRun 模式

在正式发送前，使用 DryRun 验证请求：

```bash
aws pinpoint-sms-voice-v2 send-notify-text-message \
  --notify-configuration-id nc-1234567890abcdef0 \
  --destination-phone-number +12065550100 \
  --template-id notify-code-verification-english-001 \
  --template-variables '{"code":"123456"}' \
  --dry-run
```

DryRun 模式会验证：
- 模板变量是否正确
- 目标国家是否启用
- 消费限额是否超出

但**不会实际发送消息**，也不会扣费。

### 8.4 消息路由逻辑

```
消息发送请求
├── 配置中关联了号码池？
│   ├── 是 → 池中有匹配目标国家的号码？
│   │   ├── 是 → 使用客户自有号码发送
│   │   └── 否 → 回退到 AWS 托管号码
│   └── 否 → 使用 AWS 托管号码
└── AWS 根据目标国家自动选择最佳号码
```

### 8.5 消息反馈（Message Feedback）

发送时启用 `MessageFeedbackEnabled` 后，可报告验证码的使用状态：

```python
client.put_message_feedback(
    MessageId='msg-1234567890abcdef',
    MessageFeedbackStatus='RECEIVED'  # 或 'FAILED'
)
```

这有助于 AWS 优化后续消息路由和投递质量。

### 8.6 Voice 消息注意事项

- 需在配置中启用 VOICE 渠道
- 验证码中的数字建议用 `.` 或空格分隔，如 `"1. 2. 3. 4. 5. 6."`，让 TTS 逐个读出
- 可通过 `VoiceId` 参数指定 Amazon Polly 语音角色

---

## 九、模板系统

### 9.1 模板概述

Notify 模板是 **AWS 预审批的托管模板**，用户不能创建或修改自定义模板，只能从现有模板中选择。

**当前支持的模板类型：** `CODE_VERIFICATION`（验证码）

### 9.2 模板属性

| 属性 | 说明 |
|------|------|
| Template ID | 唯一标识符（如 `notify-code-verification-english-001`） |
| Template Type | 模板类型（当前仅 `CODE_VERIFICATION`） |
| Channels | 支持的渠道：SMS、VOICE 或两者 |
| Language Code | 语言代码（如 `en`、`es`、`fr`、`ja`） |
| Supported Countries | 该模板支持的国家列表 |
| Tier Access | 可用的层级（BASIC、ADVANCED 或两者） |
| Content | 包含 `{{variable}}` 占位符的消息正文 |

### 9.3 模板变量

| 变量属性 | 说明 |
|---------|------|
| **Type** | `STRING`、`INTEGER` 或 `BOOLEAN`（API 中统一传字符串） |
| **Required** | 是否必填（缺少必填变量会报 ValidationException） |
| **Source** | `CUSTOMER`（用户提供）或 `SYSTEM`（系统自动填充） |
| **Constraints** | 验证规则：最大长度、最小/最大值、正则表达式 |

### 9.4 默认模板

可在 Notify 配置上设置默认模板。发送消息时如果不指定 `TemplateId`，会使用默认模板。如果既没有指定也没有默认模板，请求会失败。

### 9.5 浏览模板

```bash
# 列出所有可用模板
aws pinpoint-sms-voice-v2 describe-notify-templates

# 按渠道过滤
aws pinpoint-sms-voice-v2 describe-notify-templates \
  --filters '[{"Name":"channels","Values":["SMS"]}]'

# 按国家过滤
aws pinpoint-sms-voice-v2 describe-notify-templates \
  --filters '[{"Name":"supported-countries","Values":["US"]}]'

# 按语言过滤
aws pinpoint-sms-voice-v2 describe-notify-templates \
  --filters '[{"Name":"language-codes","Values":["ja"]}]'
```

---

## 十、专用号码集成

### 10.1 为什么需要专用号码？

虽然 Notify 默认使用 AWS 托管号码，但以下场景需要关联自有号码池：

- 某些国家 AWS 未提供托管号码，必须使用自有号码
- 需要使用特定的短码或免费号码发送
- 需要保持一致的发送号码（品牌认知）
- 需要双向消息能力

### 10.2 路由优先级

关联号码池后，消息路由遵循以下优先级：

1. 检查号码池中是否有支持目标国家的自有号码
2. 如果有 → 使用自有号码发送
3. 如果没有 → 回退到 AWS 托管号码

### 10.3 配置方法

**控制台：**

1. 进入 Notify 配置
2. 选择 **Use Dedicated Numbers** 标签
3. 选择 **Associate pool**
4. 选择已有的号码池并确认

**AWS CLI：**

```bash
aws pinpoint-sms-voice-v2 update-notify-configuration \
  --notify-configuration-id nc-1234567890abcdef0 \
  --pool-id pool-1234567890abcdef0
```

### 10.4 重要说明

- 关联的号码池必须已存在于你的 AWS 账户中
- 号码池中的 opt-out 列表在 Notify 中依然生效
- 可随时更改或移除号码池关联
- 使用自有号码时，该号码的所有注册和合规要求仍需你自行管理

---

## 十一、消费限额管理

### 11.1 限额类型

| 限额 | 说明 |
|------|------|
| **Account Limit** | 每月最大消费额度（美元），通过 Service Quotas 调整 |
| **Enforced Limit** | 可选的自定义消费上限，范围 $1 至 Account Limit |

**消费计算公式：**

```
消费额 = Notify 服务费 + 通道费（SMS/Voice）
```

### 11.2 查看和修改限额

**控制台：**

1. 进入 Notify 配置
2. 选择 **Spend limit** 标签
3. 查看当月消费和剩余额度
4. 选择 **Edit settings** 修改 Enforced Limit

**AWS CLI：**

```bash
# 设置消费上限
aws pinpoint-sms-voice-v2 set-notify-message-spend-limit-override \
  --monthly-limit 500

# 移除消费上限（恢复为 Account Limit）
aws pinpoint-sms-voice-v2 delete-notify-message-spend-limit-override
```

### 11.3 超限行为

当消费达到限额时，`SendNotifyTextMessage` 和 `SendNotifyVoiceMessage` 会返回 `ServiceQuotaExceededException`。

**提升 Account Limit：** 通过 Service Quotas 控制台或提交 AWS Support Case 申请。

---

## 十二、合规要求

### 12.1 合规责任

使用 Notify 时，**你仍需负责遵守目标国家的短信法规和运营商要求**。不合规可能导致运营商审计并移除你的 Notify 访问权限。

### 12.2 必须发布的条款与条件

你的条款页面必须至少包含以下内容：

```
Subscribers will opt-in via [YOUR_WEBSITE_URL] to receive verification
messages from [YOUR_COMPANY_NAME], powered by AWS Notify. Message
frequency may vary per user.

Text "HELP" for help. Text "STOP" to cancel.

Message and data rates may apply for any messages sent to you from us
and to us from you. Carriers are not liable for delayed or undelivered
messages.

If you have any questions about your text plan or data plan, contact
your wireless provider.

For all questions about the services provided, you can send an email
to [YOUR_SUPPORT_EMAIL] or call [YOUR_SUPPORT_PHONE_NUMBER].

If you have questions regarding privacy, please read our privacy policy
at [YOUR_PRIVACY_POLICY_URL].
```

### 12.3 必须发布的隐私政策

隐私政策页面必须至少包含：

- **不会将手机号码分享给第三方**用于营销或推广
- **短信 opt-in 数据和同意信息不会与任何第三方分享**
- 消息通过 AWS Notify 投递，AWS 不会将终端用户号码分享给第三方用于营销

### 12.4 必须实现的 Opt-in 流程

在通过 Notify 发送消息前，必须获取用户的明确同意：

**数字 Opt-in 示例（Web/App）：**

```
☐ By entering my phone number and clicking "Send Code," I consent to
receive an automated one-time verification code from [YOUR_BRAND] at
the number provided. Messages are powered by AWS Notify. Message and
data rates may apply. Message frequency varies. Reply HELP for help,
STOP to cancel.
Terms and conditions: [YOUR_TERMS_URL]
Privacy policy: [YOUR_PRIVACY_POLICY_URL]
```

**语音 Opt-in 示例（IVR/电话客服）：**

```
"To verify your identity, we'll send a one-time verification code to
your mobile phone, powered by AWS Notify. By providing your phone
number, you consent to receive this automated text message. Message
and data rates may apply. Message frequency varies. Reply HELP for
help or STOP to cancel. Do you consent to receive this verification
code?"
```

### 12.5 合规检查清单

在使用 Notify 发送消息前，确认已完成以下所有项目：

- [ ] 在公开 URL 发布了包含必要内容的**条款与条件**页面
- [ ] 在公开 URL 发布了包含必要声明的**隐私政策**页面
- [ ] 实现了获取明确同意的 **Opt-in 流程**
- [ ] Opt-in 流程中包含条款和隐私政策的**链接**
- [ ] 提供了**无需登录即可访问**的客户支持联系方式（邮箱和/或电话）
- [ ] 条款页面和隐私政策页面**互相链接**

### 12.6 Opt-out 行为说明

使用 AWS 托管号码发送 OTP 时：
- STOP 关键词回复是**信息性的**（informational only）
- **不会**维护持久性的 opt-out 列表
- 因为每次 OTP 请求本质上是用户的隐式 opt-in

如果关联了自有号码池，则号码池的 opt-out 列表会被**正常执行**。

---

## 十三、配额与限制

### 13.1 账户级别配额

| 配额项 | 默认值 | 可调整 |
|--------|--------|--------|
| 每账户 Notify 配置数 | 25 | 是 |
| 每目标号码每日消息数（每配置） | 10 | 否 |
| 每目标号码每日消息数（全账户） | 10 | 否 |

### 13.2 配置级别限制

| 限制项 | Basic | Advanced |
|--------|-------|----------|
| 每日消息数 | 200 | 无限制 |
| TPS（每秒事务） | 1 | 25 |
| 支持国家数 | 30 | 全部 |

### 13.3 API 速率限制（RPS）

| API | Basic | Advanced |
|-----|-------|----------|
| `SendNotifyTextMessage` | 1 RPS | 25 RPS |
| `SendNotifyVoiceMessage` | 1 RPS | 25 RPS |
| `CreateNotifyConfiguration` | 1 RPS | 1 RPS |
| `UpdateNotifyConfiguration` | 1 RPS | 1 RPS |
| `DeleteNotifyConfiguration` | 1 RPS | 1 RPS |
| `DescribeNotifyConfigurations` | 1 RPS | 1 RPS |
| `DescribeNotifyTemplates` | 1 RPS | 1 RPS |
| `ListNotifyCountries` | 1 RPS | 1 RPS |
| `SetNotifyMessageSpendLimitOverride` | 1 RPS | 1 RPS |
| `DeleteNotifyMessageSpendLimitOverride` | 1 RPS | 1 RPS |

### 13.4 提升配额

通过以下方式申请提升可调整的配额：
- **Service Quotas 控制台**
- **AWS Support Center** 提交工单

---

## 十四、事件追踪与监控

### 14.1 投递事件类型

| 事件 | 说明 |
|------|------|
| `PENDING` | 消息已排队，等待投递 |
| `DELIVERED` | 消息已成功投递到收件人设备 |
| `FAILED` | 投递失败，查看失败原因 |
| `BLOCKED` | 消息被 Protect 配置规则阻止 |

### 14.2 配置事件目标

Notify 通过 Configuration Set（配置集）将事件发送到以下目标：

- **Amazon CloudWatch** — 监控指标和告警
- **Amazon Data Firehose** — 流式存储到 S3 等
- **Amazon SNS** — 触发通知或 Lambda 函数

设置方法请参考 AWS End User Messaging SMS 文档中的 Configuration Sets 章节。

---

## 十五、故障排查

### 15.1 常见错误

| 错误 | 原因 | 解决方案 |
|------|------|---------|
| `ValidationException` | 缺少或无效的模板变量、号码格式错误、目标国家未启用 | 检查 TemplateVariables 是否完整、号码是否 E.164 格式、国家是否在 EnabledCountries |
| `ResourceNotFoundException` | 配置或模板不存在 | 确认 NotifyConfigurationId 和 TemplateId 正确 |
| `ServiceQuotaExceededException` | 每日消息上限（Basic）或月度消费限额已达 | 升级到 Advanced 或提升消费限额 |
| `ConflictException` | 配置不在 ACTIVE 状态 | 等待配置审核完成或解决审核问题 |

### 15.2 消息未投递

排查步骤：

1. 确认目标号码为 **E.164 格式**（以 `+` 开头加国家代码）
2. 检查目标国家是否在配置的 **EnabledCountries** 列表中
3. 确认配置状态为 **ACTIVE**
4. 检查**消费限额**是否已达上限
5. 查看事件日志中的投递状态和失败原因

### 15.3 消息被 BLOCKED

BLOCKED 表示消息被 SMS Protect 拦截，常见原因：

- 目标国家不在启用列表中
- 消息被识别为潜在 **AIT（人为膨胀流量）**
- 触发了国家级别的发送规则

### 15.4 配置审核被拒绝

- 检查 Display Name 是否包含不当内容、URL 或特殊字符
- 如确认名称无误，通过 AWS Support 申诉
- 如代表其他品牌使用，提供 30 天内的授权信

---

## 十六、最佳实践

### 16.1 安全性

- **始终在发送前验证用户输入的号码格式**（E.164）
- **设置合理的消费限额**防止意外超支或被攻击
- **利用 DryRun 模式**在生产环境前验证请求
- **启用 MessageFeedback**帮助检测验证码是否被真正使用
- Notify 的 SMS Protect 已强制开启，无需额外配置

### 16.2 可靠性

- **同时启用 SMS 和 Voice 渠道**，实现验证码投递的双通道兜底
- **监控投递事件**，对 FAILED 和 BLOCKED 事件设置告警
- **合理设置 TimeToLive**，OTP 通常 5-10 分钟有效期即可
- **对关键市场考虑关联自有号码池**，减少对 AWS 托管号码的依赖

### 16.3 成本优化

- **评估月发送量**：低量级（<5,000 条/月）时 Notify 的便利性可能值得溢价；高量级时标准 SMS 更经济
- **利用阶梯定价**：月发 250,000+ 条时，Notify 服务费降至 $0.030/条
- **设置 Enforced Limit** 防止意外超支
- **对高量级市场**使用标准 SMS，对低量级/快速上线市场使用 Notify

### 16.4 合规

- **务必在上线前完成合规检查清单**（见第十二章）
- **条款和隐私政策链接必须保持可访问**
- **保留用户同意记录**
- **遵守目标国家的具体法规**（如美国 TCPA、欧盟 GDPR）

### 16.5 Voice 验证码

- 验证码数字使用 `.` 或空格分隔（如 `"1. 2. 3. 4. 5. 6."`）
- 选择与目标市场语言匹配的 Polly Voice
- Voice 适合作为 SMS 投递失败的**备用渠道**

---

## 附录 A：API 快速参考

| 操作 | API | 说明 |
|------|-----|------|
| 创建配置 | `CreateNotifyConfiguration` | 创建新的 Notify 配置 |
| 查看配置 | `DescribeNotifyConfigurations` | 列出或过滤配置 |
| 更新配置 | `UpdateNotifyConfiguration` | 修改渠道、国家、模板、号码池 |
| 删除配置 | `DeleteNotifyConfiguration` | 删除配置（需先关闭删除保护） |
| 浏览模板 | `DescribeNotifyTemplates` | 按渠道、语言、国家过滤模板 |
| 查询国家 | `ListNotifyCountries` | 查看支持的国家列表 |
| 发送短信 | `SendNotifyTextMessage` | 发送 SMS 验证码 |
| 发送语音 | `SendNotifyVoiceMessage` | 发送 Voice 验证码 |
| 消息反馈 | `PutMessageFeedback` | 报告验证码使用状态 |
| 设置消费上限 | `SetNotifyMessageSpendLimitOverride` | 设置月度消费限额 |
| 移除消费上限 | `DeleteNotifyMessageSpendLimitOverride` | 恢复默认限额 |

## 附录 B：与本仓库其他文档的关联

| 如果你需要... | 参考文档 |
|-------------|---------|
| 了解标准 SMS 各国发送要求 | [各国发送要求研究报告](AWS_SMS_各国发送要求研究报告.md) |
| 查看特定国家的注册流程 | 各国专题报告（如 [美国](AWS_SMS_美国专题研究报告.md)、[英国](AWS_SMS_英国专题研究报告.md) 等） |
| 注册表单填写指导 | [注册表单填写指南与消息模板](AWS_SMS_注册表单填写指南与消息模板.md) |
| 编写自定义消息模板 | [消息模板最佳实践指南](AWS_SMS_消息模板最佳实践指南.md) |

> **提示：** Notify 的模板是 AWS 托管的预审批模板，不需要你自行编写。但如果你同时使用标准 SMS 发送非 OTP 消息（如营销、通知），则需参考上述模板指南。

---

## 免责声明

本报告基于 2026 年 4 月公开的 AWS 官方文档编写。AWS 服务功能、定价和可用性可能随时变更，实际使用前请以 [AWS 官方文档](https://docs.aws.amazon.com/sms-voice/latest/userguide/notify.html) 和 [AWS 控制台](https://console.aws.amazon.com/sms-voice/) 中的最新信息为准。
