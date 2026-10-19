# 订阅数据看板

投流订阅业务的公开静态看板，方便同事用同一个链接查看、转发。

## 在线地址

部署到 GitHub Pages 后访问：

**https://theshy718.github.io/subscription-dashboard/**

## 页面说明

| 页面 | 说明 |
|------|------|
| [index.html](index.html) | 续订 Cohort 看板：按平台「首购」标识的总表、年包 99.9、日表向前多期、重复首购用户、回本（CAC $160） |
| [payback.html](payback.html) | 回本分析（同事分享版）：实际付款路径 vs 续费率连环推算对比 |
| [finance.html](finance.html) | 财务模型摘要：PayerMax 对账 × 订单的现金瀑布、take-rate、单位经济 |
| [customers.html](customers.html) | 存量客户：一人一行（首购时间、续费/失败次数、手续费/退款/拒付、净收益），可筛选；**不改 CAC** |

## 口径摘要

- **首购**：以平台订单「首购标记」为准；已支付重复首购用户单独列表。
- **续订第 N 期**：日历窗（首购日 + 7×(N−1) ±1 天内是否成功扣 39.9），不是用户 lifetime 续费序号。
- **回本**：推荐看「实际付款路径」口径；连环平均续费率仅作对照。净收默认扣手续费与退款（take ≈ 0.885；含拒付约 0.835）。
- **CAC**：获客成本按 **$160**。

## 更新方式

每日约 10:00（Asia/Shanghai）可从管理后台拉昨日订单并重算后，把本目录推送到仓库 `main`，Pages 会自动刷新。本仓库为纯静态 HTML，不含密钥与登录态。

## 仓库

- GitHub：`theshy718/subscription-dashboard`
- 分支：`main`，Pages 源选 **Deploy from a branch → main → / (root)**

详见 [DEPLOY.md](DEPLOY.md)。
