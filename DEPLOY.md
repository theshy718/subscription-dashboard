# 部署到 GitHub Pages

## 目标

- 仓库：`theshy718/subscription-dashboard`（公开）
- 站点：https://theshy718.github.io/subscription-dashboard/
- 源：`main` 分支根目录（`/`）

## 一次性设置

1. 在 GitHub 创建公开空仓库 `theshy718/subscription-dashboard`（不要勾选自动加 README，若已有可忽略）。
2. 将本目录内容推到该仓库的 `main`：

```bash
cd /workspace/subscription-dashboard-site
git init
git checkout -b main
git add .
git commit -m "Initial subscription dashboard site"
git remote add origin https://github.com/theshy718/subscription-dashboard.git
git push -u origin main
```

3. 打开仓库 **Settings → Pages**：
   - Source：**Deploy from a branch**
   - Branch：**main**
   - Folder：**/ (root)**
   - Save

4. 等待 1–2 分钟，打开 https://theshy718.github.io/subscription-dashboard/

## 日常更新

改本地 HTML 后：

```bash
cd /workspace/subscription-dashboard-site
git add -A
git commit -m "Update dashboard data"
git push origin main
```

Pages 会自动发布新版本。同事刷新浏览器即可（必要时强刷）。

## 注意

- 站点公开，勿放入 Cookie、Token、密码或未脱敏的原始对账全量明细。
- 业务汇总数字（GMV、续费率、回本结论）按当前约定可公开。
- 本包为自包含静态页，无需构建步骤。
