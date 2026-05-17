---
name: Starling 短剧校对工作流
description: 字节 Starling 平台中→英短剧字幕 polish 任务的标准操作流程
type: project
originSessionId: 04cdaffc-f56c-45ea-bba2-58cd012393fe
---
无锡中电金信本地化短剧翻译项目 — 用户负责中→英方向的 Starling 校对工作。

**Why:** 用户在 starling.bytedance.com（字节内部翻译平台）上负责对已有的中→英 MT 译文做 polish 校对。批次单位是"order"（订单），每个 order 是一集短剧（约 60-70 条字幕），每天会派 10 个 order。

**How to apply:**

1. **Guide 文档**：https://kcnlgm4hyo6e.feishu.cn/docx/IxxpdIyYsoOceZxS5Q6chIChnTd — 中→英 polish 规则索引见 feedback_translation_style.md，关键点：
   - 货币 10:1 换算（中-英）
   - 25 字符/行 + 不超 2 行
   - 画面字加 `[]`（人物介绍：身份在上，姓名在下，整体一对 `[]`）
   - 人名地名本地化（如 刘军 → James Miller，刘云舒 → Sophia Miller，刘春燕 → Helen Miller）

2. **浏览器**：通过 Playwright MCP 或 Claude in Chrome MCP 控制 Chrome 操作 Starling。Claude in Chrome 的 `navigate` 有权限网关（即使设了 Always Allow 也拦），workaround：用户在 MCP 标签里手动输 URL 加载，之后 `screenshot/click/javascript_exec` 都能用。

3. **Order 处理流程**：
   - 在 my-task 列表点"去校对" → 进入编辑器
   - JS 抓取 `.subtitle-list .subtitle-item` 所有行（idx / start / end / src / tgt）
   - 按 Guide 生成 diff
   - 把所有改动列表发给用户审核（每条原 / 改 / 原因）
   - 用户确认后 JS 写回（contenteditable 用 execCommand('insertText') 触发 React 更新）
   - **不要点"完成校对"按钮 — 用户自己提交**

4. **每天默认 10 个 order**，每个 order 单独审核 diff 后再写回。
