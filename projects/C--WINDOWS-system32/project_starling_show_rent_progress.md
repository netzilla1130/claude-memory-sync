---
name: 《房东涨房租》校对进度状态
description: 该剧 10 个 order 的当日进度，明日继续从这里接力
type: project
originSessionId: 04cdaffc-f56c-45ea-bba2-58cd012393fe
---
剧名：《房东恶意涨房租，我转身租下隔壁》（She Raised My Rent）  
任务批次：2026-05-16，60 个进行中 order 中的前 10 个

## 已完成（4/10）

| Order # | Task ID | 行数 | 改动数 | 改动类型 |
|---|---|---|---|---|
| 1 | 151171840002 | 65 | 20 | 1 格式/一致；7 货币 10:1；11 断行/缩短；1 标点歧义 |
| 2 | 151171841026 | 108 | 26 | 11 货币 10:1；13 断行；1 标点；1 画面字 `[]` |
| 3 | 151171842050 | 77 | 20 | 11 货币 10:1；8 断行；1 画面字 `[]` |
| 4 | 151171843074 | 73 | 22 | 17 断行；1 画面字格式 `[]`；1 画面字加 `[]`；1 用词缩短；1 内容微调+断行；1 去标点 |

**所有已完成 order 未点"完成校对"**，等用户手动提交。

## 待完成（6/10）

| Order # | Task ID |
|---|---|
| 5 | 151171844098 |
| 6 | 151171845122 |
| 7 | 151171846146 |
| 8 | 151171847170 |
| 9 | 151171848194 |
| 10 | 151171849218 |

## 关键规则速查（应用到每个 order）

1. **术语库**：see `project_starling_show_rent_glossary.md` — 刘军=James Miller / 春燕=Helen Miller / 春梅=Mary Miller / 航天=Tony Lee / 云舒=Sophia / 三姑=Aunt Helen 等
2. **货币 10:1**：80万→80k / 8万→8k / 6万→6k 等
3. **25 字符限制**：单行 ≤25，cue ≤2 行；超就 `\n` 断行或缩短
4. **画面字加 `[]`**：`[未完待续]→[To be continued]`、人物介绍 `[身份\n姓名]`
5. **多 cue 续句**：中间 cue 末尾不加句号，续行首字母小写
6. **文化梗**：不要硬译，转化为英语自然表达（不补"亲姑"的"亲"、"明白明白"的叠词等）

## 操作模式（明日恢复时直接重启）

1. 打开 Chrome（MCP 控制 → Browser 1）
2. 在 MCP 标签栏粘贴 `https://starling.bytedance.com/#/my-task?pageNum=1&pageSize=10&progress=doing&translateTypeList=%5B%5D&sortType=2`
3. 点 Order 4 的"去校对"
4. JS 抓全部行 → diff → 用 `computer.key 'ctrl+a'` + `Delete` + `type` 写入（**不是 execCommand，是真键盘事件，因为 Slate.js**）
5. 写完不点"完成校对"
