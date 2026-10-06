# 项目交接

更新日期：2026-10-06

## 当前改动

- 音频渐入/渐出时长可配置，新增设置滑块和自然结束渐出（`dbb7c18`）。
- 清空旧音源前显式暂停；自然渐出时切歌直接停止旧源，处理缓冲等待期间尾段重复播放问题（`adcb326`）。
- beta 工作流汇总所有平台打包产物，上传带平台/架构说明的预发布 Release（`510b1e9`）。
- Release 标题与正文改为沿用用户历史 Release 的格式：标题 `beta-N — <本次提交标题>`，正文用 `## 标题` + `### 说明` + `### 致谢`，不再用之前的英文通用模板。
- `permissions: contents: write` 从工作流顶层移入 `Release` job，四个打包 job 不再持有仓库写权限。
- 删除过期的 `error.txt`（`PlayDetail/index.vue` 里 `ref<string[]>` 报错的旧日志，该问题已修，留着会误导接手人）。
- 新增 `AGENTS.md`（项目交接要求）与本文档。

## 验证与发布

- `npm run build:renderer` 本地实测通过（50.8s，0 error）。
- 工作流改动只做了 YAML 解析与 job/permissions 结构校验（js-yaml），**未跑线上**；新标题和正文要等下一次 beta 推送才能真正看到效果。
- 上一次 beta 线上运行 `37408428775` success，Release `beta-3`，15 个附件。
- 本次改动提交后推送到 `next` 的 `main`。**未推送 `beta` 分支**，所以没有触发新的打包。

## 接手注意

- 本次明确**不修**的两个已知 bug，都在 `src/renderer/plugins/player/index.ts` 的 `handleNaturalFade`：
  - 自然渐出没有恢复路径：播放中把进度条拖进最后 `fadeDuration` 内（触发渐出到 0），再拖回中段，`remaining > fadeDuration` 直接 return，音量永远停在 0，歌在放但没声，只能靠暂停/播放或切歌恢复。
  - 同一个函数缺 `isFadeInPending` 判断：音频时长 ≤ `fadeDuration` 时（例如 500ms 音效、fade 设 800ms），第一次 `timeupdate` 就会把还在跑的渐入清掉并渐向 0，整段基本没声。
- 想验证新版 Release 格式，推一次 `beta` 分支即可（会新建一个预发布 Release）。
- 用户已有未跟踪文件 `full.png` 和微信截图，应保留，不要误提交。
- 仓库：https://github.com/xingnengmao666/ikun-music-desktop-next
