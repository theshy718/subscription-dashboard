# 看板口径 Canonical Rules

> 规则变更时：所有 builder 必须对照本文件重算并同步 `subscription-dashboard-site/`。
> 单一真相源优先：`_build/part1_load.py` 的 `is_weekly_onetime` / `resolve_product`。

## 1. 周价单次 / 收单（剥离出周续订）

**判定（OR）**：`VIP套餐编码` 含 `ONETIME`（忽略大小写）**或** 订单 `摘要` 含「充值」。

- **进**：周价单次 bucket / 存量路径「收单」/ 财务「周价单次」档
- **不进**：周续订 cohort、周订混合、周档单位经济路径、周续订链锚点（周价单次自有路径/档）

实现：`part1_load.is_weekly_onetime` → cohort / payback / finance / stock 共用。

## 2. $99.9 拆分

- 订单「订阅周期」`YEAR` → **单次 99.9**
- 「订阅周期」`MONTH` → **半年包 99.9**（对齐 PayerMax Plan Name `6 MONTH VIP`）
- 周期缺失：UID ∈ 6 MONTH VIP 集合 → 半年包；否则单次
- 单次含平台误标「续费」一律计单次（不进续费成功路径）

## 3. 存量客户（customers）

- 一行 = **UID + VIP 路径**
- 订阅状态 = MMC 订阅计划 ∩ PayerMax `txn_type`
  - `PAYMENT` → 收单
  - `MIP` + 有匹配计划 → 订阅（生效中/已过期等）
- 空白订阅路径不乱填；收单路径不得抄 UID 级「生效中」

## 3b. 单位经济 / 回本粒度

| 表 | 粒度 | 说明 |
|----|------|------|
| 财务·分档明细 `by_tier` | **订单笔** | 首购笔/续费笔/GMV；周价单次 first_n≈笔数 |
| 财务·首购档单位经济 `unit_econ_full` | **首购路径 UID+VIP套餐编码** | 与存量客户同行；同 UID 周订+周价单次各开路径；路径 GMV=同 VIP 自首购起已支付 |
| 回本·实际路径 | **同上路径** | 周价单次/99.9 路径数对齐分档笔量（去重 uid+vip 后略少或相等）；**不**「一人只取最早首购」 |
| 续订·周订 cohort | 已按路径/套餐链 | 周价单次仍剥离，不进周订混合 |

错误口径（已废弃）：单位经济/回本按「用户最早一笔首购」定档 → 会把周价单次压成 ~40 人。

## 4. 财务 / 回本常数

- CAC = **$160**
- take 推荐：手续费+退款 **0.900** · 全成本 **0.865**（账单窗实测取整；公式不变）
- 存量净收益公式不变

## 5. 投流日界

- 西七区（America/Los_Angeles）切日；订单日报分档等既有规则保留

## 6. 订单源文件

- 一律 **自动选取** `/home/box/Downloads/orders-YYYY-MM-DD.csv` 中日期最新者
- 忽略 `orders-YYYY-MM-DD (1).csv` 这类副本
- Builder：`part1_load`、`rebuild_from_user_pack` / `refresh_overnight`、`build_stock_customers`

## Builders（必须遵守）

| Builder | 产出 |
|---------|------|
| `subscription-cohort/run_rebuild.py` | `index.html`（续订）+ `payback.html` |
| `finance-model/refresh_overnight.py` | `finance.html` + waterfall/summary |
| `finance-model/rebuild_from_user_pack.py` | 同上（对账包增量） |
| `customers/build_stock_customers.py` | `customers.html` + stock_customers.* |

规则或订单变更后：先跑上述 rebuild，再同步 `subscription-dashboard-site/` 打 tarball 推 Pages。
