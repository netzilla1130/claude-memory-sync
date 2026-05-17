---
name: Step-2 任务交付清单格式
description: 每次完成 Step-2 任务后必须额外列出"需要覆盖的脚本清单"
type: project
originSessionId: fe8c8b18-2034-488a-922a-cd8a11b15bee
---
用户的工作流程：每次任务完成后，需要手动把脚本同步到工作工程里，所以每次任务完成时，除了列出 Inspector 操作清单，**还必须列出本次任务"需要覆盖的脚本清单"**，方便对照同步。

**Why:** 用户在不同位置维护工作工程；我做的修改写在 `C:\Users\netzi\Documents\Codex\Step-2-Workspace\Step-2`，他需要把这些 .cs 文件覆盖回工作工程。漏列脚本就漏同步，行为不一致。

**How to apply:**

每次完成任务时，在收尾报告里包含两个清单：

1. **Inspector 操作清单**（既有规则）
2. **要覆盖的脚本（共 N 个）**——按以下格式：
   ```
   1. `Assets/Scripts/.../X.cs` —— 改了什么（一行）
   2. `Assets/Scripts/.../Y.cs` —— 改了什么（一行）
   ```

只列**本次任务**实际新建/修改的脚本，不重复列以前任务的脚本。新建文件用"（新增）"标注，纯修改的不标。
