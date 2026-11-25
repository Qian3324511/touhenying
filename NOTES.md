# 交接笔记（给自己下次用）

## 这是什么

个人 GitHub 小仓库，放一个单文件网页桌游。

- 仓库：https://github.com/Qian3324511/touhenying （**公开**）
- 在线地址：https://qian3324511.github.io/touhenying/ （GitHub Pages，main 分支根目录）
- 本地：`G:\touhenying`（WSL 里 `/mnt/g/touhenying`）
- 文件：`index.html`（游戏本体，无依赖无联网）、`README.md`

## 游戏：《布站记 · 县域气象观测网》

致敬《勃艮第城堡》，六要素全部换成气象：观测站（城堡）、科研课题（修道院）、雷达站（矿山）、
数据链（船）、业务楼（建筑）、农业示范（牧场）。

- 图板：37 个六角格，6 色地类，每格 1–6 点。`ROWS` 里每行是 `{r, q0, s}`，字母 A–F 对应 `COLS` 顺序。
  现在的图板是用脚本生成的（区域 2–5 格、每色 5–8 格、起始格邻居 ≥3 种颜色），改图板必须重新校验区域大小，
  否则会出现 7 格连通大区、永远铺不满。
- 回合：每人 2 骰 → 4 种行动（取/布/传/调）→ 每阶段 5 轮 × 5 阶段。
- 计分：区域完成（`REGION_SCORE` + `PHASE_BONUS`）、颜色奖励、终局四项。
- 暂停/联机都没有；单人练习记 `localStorage.buzhanji_best`。
- 图标是内联 SVG（`ICONS`），不要用 emoji——Linux/Chromium 会渲染成方框。

## 测试方法

`index.html` 里函数都在 IIFE 闭包里，外部只能点 DOM。做自动化验证时临时加过
`window.__T = { G, cur, canPlaceAt, place, take, tech, upload, adjust, endTurn, render }`
调试钩子直接驱动逻辑层，验证完删掉（交付版没有）。DOM 层用 `.die:not(.used) / .wh.can / .slot.f / .cell.hl` 探针。

## 还没做

- 房间号联机（要后端：WebSocket 或 Cloudflare Durable Objects；用户说留到下一阶段）
- 简单 AI 对手（规则都集中在 `canPlaceAt / placeTile`，枚举合法行动即可接）
- 音效

## 提交怎么走（这台机器的网络很挑）

1. 先试 `cd /mnt/g/touhenying && git add -A && git commit -m "..." && git push` —— github.com:443 时通时断，失败就重试几次。
2. 还不行就用 API 直推（api.github.com 更稳）：
   `COMMIT_MSG="说明" python3 ~/.hermes/skills/software-development/github/scripts/git-data-api-push.py <文件绝对路径>`
   脚本里 OWNER/REPO 默认就是 Qian3324511/touhenying，可用 `GH_OWNER/GH_REPO/GH_BRANCH` 环境变量覆盖。
3. API 推完本地会分叉：先 `git hash-object 文件` 比对接口返回的 blob sha（一致＝字节相同），再 `git fetch && git reset --hard origin/main`。
4. 令牌：`~/.hermes/.env` 的 `GITHUB_TOKEN`，scope 只有 `repo`。

## 坑

- WSL 里的 `git` 是包装器 → Windows git.exe，**不认 `/mnt/...` 路径**：别用 `git -C /mnt/g/touhenying`，要先 `cd` 进去再裸调。
- Pages 首次构建要 1–3 分钟，期间返回 404 属正常（404 页面本身约 9 KB，200 时是 index.html 的大小）。
- 仓库是公开的：**别往里放密钥、内网数据、站点信息**。

## 历史

旧版「小气象员 · 接雨滴」（canvas 接雨滴）在 commit `2a08617`，需要时 `git show 2a08617:index.html > rain.html` 取回。
