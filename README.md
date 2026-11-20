# lego-template-demo

Lego-Fullstack-Engine `template/` 里 Web 前端的**静态构建产物**，仅用于 GitHub Pages 预览。

- 源码不在本仓：源码仓 Lego-Fullstack-Engine 是私有仓，GitHub Free 计划下私有仓无法发布 Pages，故只把构建产物放进这个公开仓发布（产物上线后本来就人人可见，不额外泄密）。
- 本仓内容由构建生成，不要手改；页面里的 Backend health 显示 Unavailable 是预期行为（Pages 只托管静态文件，模板的 API 不在部署范围）。
- 重新发布：本地重新 build（`vite build --base=/lego-template-demo/`）后把 `apps/web/dist/` 的内容覆盖本仓根目录，commit + push 即可（Pages 设为从 main 分支根目录发布）。
- 方案说明见源码仓 `docs/deploy-github-pages.md`（ADR-10）。
