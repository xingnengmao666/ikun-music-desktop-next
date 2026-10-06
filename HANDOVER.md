# 项目交接

更新日期：2026-10-06

## 当前改动

- 音频渐入/渐出时长可配置，新增设置滑块和自然结束渐出（`dbb7c18`）。
- 清空旧音源前显式暂停；自然渐出时切歌直接停止旧源，处理缓冲等待期间尾段重复播放问题（`adcb326`）。
- beta 工作流汇总所有平台打包产物，上传带平台/架构说明的预发布 Release（`510b1e9`）。
- Release 标题与正文改为沿用用户历史 Release 的格式：标题 `beta-N — <本次提交标题>`，正文用 `## 标题` + `### 说明` + `### 致谢`，不再用之前的英文通用模板。
- `permissions: contents: write` 从工作流顶层移入 `Release` job，四个打包 job 不再持有仓库写权限。
- 修复 seek 离开自然渐出区间后永久静音；短音频渐入按音频长度截断，并避免自然渐出抢占。
- 删除过期的 `error.txt`（`PlayDetail/index.vue` 里 `ref<string[]>` 报错的旧日志，该问题已修，留着会误导接手人）。
- 新增 `AGENTS.md`（项目交接要求）与本文档。

## 验证与发布

- `npm run build:renderer` 本地实测通过（0 error）。
- 工作流改动只做了 YAML 解析与 job/permissions 结构校验（js-yaml），**未跑线上**；新标题和正文要等下一次 beta 推送才能真正看到效果。
- 上一次 beta 线上运行 `37408428775` success，Release `beta-3`，15 个附件。
- 本次提交已推送到 `next` 的 `main` 和 `beta`；beta 推送已触发新的打包和 Release 流程。

## 接手注意

- 此前确认的两个播放器 bug，均位于 `src/renderer/plugins/player/index.ts` 的 `handleNaturalFade`：
  - 已在本次修改修复：seek 离开尾段时取消自然渐出并恢复目标音量。
  - 已在本次修改修复：短音频优先完成渐入，渐入时长按音频长度截断，不再被自然渐出抢占。
  - 本次仍需用真实音频手动验证 seek 和短音频听感。
- 想验证新版 Release 格式，推一次 `beta` 分支即可（会新建一个预发布 Release）。
- 用户已有未跟踪文件 `full.png` 和微信截图，应保留，不要误提交。
- 仓库：https://github.com/xingnengmao666/ikun-music-desktop-next
