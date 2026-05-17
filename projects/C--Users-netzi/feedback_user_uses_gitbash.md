---
name: 用户用 Git Bash (MINGW64)，不要给 PowerShell 风格命令
type: feedback
originSessionId: fe8c8b18-2034-488a-922a-cd8a11b15bee
---
用户的 shell 环境是 Git Bash (MINGW64)，不是 PowerShell。我曾经给过一条 `cd E:\Unity\Step2 ; git add . ; git commit -m ...` 的命令，结果 git bash 把 `\` 当转义字符吃掉，`E:\Unity\Step2` 变成 `E:UnityStep2`，命令失败。

**Why:** Git Bash 用 Unix 风格路径解析，`\` 是转义符；PowerShell 用 Windows 原生路径，`\` 是分隔符。两者命令语法也有差异（PowerShell 的 `;` vs Bash 的 `&&` / `;`）。

**How to apply:**

给用户跑 shell 命令时，**默认假设是 Git Bash**，按 Unix 语法出：

- 路径用正斜杠：`/e/Unity/Step2` 或 `"E:/Unity/Step2"`（不要写 `E:\Unity\Step2`）
- 串联命令用 `&&`（"前一条成功再跑后一条"，更严谨），少用 `;`
- 引号优先双引号；中文路径和带空格的路径要加双引号
- Tool 调用层面：Bash 工具本身在 Windows 上用 bash 而不是 powershell，对应也是 Unix 语法

**反例 (PowerShell 风格，在 git bash 里失败)：**
```
cd E:\Unity\Step2 ; git add . ; git commit -m "..."
```

**正例 (Git Bash 兼容)：**
```bash
cd /e/Unity/Step2 && git add -A && git commit -m "..."
```

或：

```bash
cd "E:/Unity/Step2" && git add -A && git commit -m "..."
```
