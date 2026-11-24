# 交接笔记（给自己下次用）

## 这是什么

个人 GitHub 小仓库，放一个单文件网页小游戏。

- 仓库：https://github.com/Qian3324511/touhenying （**公开**）
- 在线地址：https://qian3324511.github.io/touhenying/ （GitHub Pages，main 分支根目录）
- 本地：`G:\touhenying`（WSL 里 `/mnt/g/touhenying`）
- 文件：`index.html`（游戏本体，无依赖无联网）、`README.md`

## 游戏

「小气象员 · 接雨滴」：canvas 手写引擎，接雨滴得分、躲冰雹扣命，3 条命，连击加成，最高分存 localStorage。
难度随时间上升（`hailChance = min(0.30, 0.08 + t*0.005)`，落速随 `S.t` 增长）。
想加东西的方向：音效、排行榜（要后端）、更多天气道具（雨伞护盾 / 双倍分云）、关卡。

## 提交怎么走（这台机器的网络很挑）

1. 先试 `cd /mnt/g/touhenying && git add -A && git commit -m "..." && git push` —— github.com:443 时通时断，失败就重试几次。
2. 还不行就用 API 直推（api.github.com 更稳）：
   `COMMIT_MSG="说明" python3 ~/.hermes/skills/software-development/github/scripts/git-data-api-push.py <文件绝对路径>`
   脚本里 OWNER/REPO 默认就是 Qian3324511/touhenying，可用 `GH_OWNER/GH_REPO/GH_BRANCH` 环境变量覆盖。
3. API 推完本地会分叉：先 `git hash-object 文件` 比对接口返回的 blob sha（一致＝字节相同），再 `git fetch && git reset --hard origin/main`。
4. 令牌：`~/.hermes/.env` 的 `GITHUB_TOKEN`，scope 只有 `repo`。

## 坑

- WSL 里的 `git` 是包装器 → Windows git.exe，**不认 `/mnt/...` 路径**：别用 `git -C /mnt/g/touhenying`，要先 `cd` 进去再裸调。
- Pages 首次构建要 1–3 分钟，期间返回 404 属正常（404 页面本身 9 KB，200 时是 index.html 的 19918 B，可据此判断）。
- 仓库是公开的：**别往里放密钥、内网数据、站点信息**。

## 下一步（用户可能想做的）

- 换个更好玩的题材或加音效
- 手机端体验微调（目前竖屏自适应，宽度上限 520px）
