# lego-template-demo

Lego-Fullstack-Engine `template/` 里 Web 前端的**静态构建产物仓**，仅用于 GitHub Pages 预览。源码不在本仓（源码仓 Lego-Fullstack-Engine 是私有仓，GitHub Free 计划下私有仓无法发布 Pages，故只把构建产物放进这个公开仓发布；产物上线后本来就人人可见，不额外泄密）。方案说明见源码仓 `docs/deploy-github-pages.md`（ADR-10）。

本仓内容由构建生成，**不要手改**；要改页面，去源码仓改 `template/` 后重新发布。

## 访问

| 入口 | 地址 |
| --- | --- |
| 线上站点 | https://znbsys.github.io/lego-template-demo/ |
| 本仓（GitHub） | https://github.com/znbsys/lego-template-demo |

打开线上站点应看到模板前端页面，出现下面这些都是**预期状态**，不是故障：

- 标题显示 `__SERVICE_NAME__`（模板占位符，派生服务时才替换成真名）；
- Backend health 显示 Unavailable（Pages 只托管静态文件，模板的 API 不在部署范围）；
- Examples 区为空（没有 API，自然没有数据）。

## 本地预览（启动）

构建时资源路径带 `/lego-template-demo/` 前缀（`vite build --base=/lego-template-demo/`），所以必须按这个子路径托管；直接在仓根目录起服务器打开 `/` 会因资源路径对不上而白屏。用软链凑出子路径即可：

```bash
mkdir -p /tmp/pages-preview
ln -sfn /Users/znb/workspace/dev/lego-template-demo-site /tmp/pages-preview/lego-template-demo
cd /tmp/pages-preview && python3 -m http.server 8000
# 打开 http://127.0.0.1:8000/lego-template-demo/
```

## 重新发布（更新内容）

1. 在源码仓 Lego-Fullstack-Engine 里重新构建模板 Web：构建步骤照源码仓 `.github/workflows/pages.yml` 本地复现即可，关键只有一条 — `vite build` 的 base 必须是 `/lego-template-demo/`（与本仓名一致）。
2. 把 `template/apps/web/dist/` 的内容覆盖到本仓根目录（保留 `.nojekyll`），commit + push 到 main。
3. GitHub Pages 设置已配好：Settings → Pages → Source = Deploy from a branch、main + /(root)。分支模式**不需要**任何 workflow 文件。
4. 等 1–3 分钟后打开线上地址确认更新。

**坑：首次构建触发可能丢失** — push 成功、设置也对，但站点长时间 404 且 Actions 里没有任何构建记录时，推一个空提交重新触发：

```bash
git commit --allow-empty -m "触发 Pages 构建" && git push
```

**上线验证**：打开线上地址看到页面即算数；也可以在源码仓跑预检脚本（仓可见性 + 站点 200 轮询）：

```bash
node scripts/verify/pages-check.mjs --repo znbsys/lego-template-demo
```

退出码 0 = 已上线。
