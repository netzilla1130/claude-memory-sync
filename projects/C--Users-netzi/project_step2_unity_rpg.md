---
name: Step-2 Unity 2D 横版回合制 RPG
description: 用户在 Tuanjie Unity (2022.3.62t7) 上开发的 2D 横版回合制 RPG 项目；工作目录与关键管理者位置
type: project
originSessionId: fe8c8b18-2034-488a-922a-cd8a11b15bee
---
实际工作工程根目录：`E:\Unity\Step2`（**已授权直接读写**；用户启用了 git，每 2-3 天推送一次远端仓库作为兜底）
克隆备份工程根目录：`C:\Users\netzi\Documents\Codex\Step-2-Workspace\Step-2`（保留作历史快照，**不再作为主工作目录**）

**Why direct edit:** 走克隆 + 用户手动复制的旧流程已发生一次贴反文件事故（把 stub 贴到了新版位置，导致 SkillData class 消失、CS0246 雪崩）。直接改源工程避免人手中转误差，git 是回滚兜底。

**How to apply:** 写入前先用 Read/Bash 确认当前状态，写入后立刻 brace + class 计数自检，关键时刻提醒用户 `git status` / `git commit` 存档。

引擎：Tuanjie 1.8.5（基于 Unity 2022.3.62t7）。.meta 用 Tuanjie Base64 GUID，prefab/scene 里用 Unity 旧式 hex GUID，由 Library/ 做映射；`Library/` 在 `ignore.conf` 里被忽略。

**Why:** 该项目场景/预制体修复必须意识到 GUID 双形态共存，不能只看 .meta hex 找不到就断言 "missing"。

**How to apply:** 找一个 script 的 GUID 时，先在 .csproj `<Compile Include>` 列表里确认它现在的实际文件路径；prefab/scene 里 `m_Script: {guid: xxx, type: 3}` 的 hex 形式如果没出现在 .meta 里，多数时候不是真的缺失，而是 GUID 形态不同。

## 关键管理者及位置

- 战斗：`Assets/Scripts/战斗系统/`（BattleController, BattleUnit, BattleUIManager, BattleCalculator, DamageNumber, SkillManager, EnemyAI, EnemyEncounter, EnemyWeaknessShieldUI, BattleHitStopManager, BattleCameraShake, BattleRewardManager）
- 玩家：`Assets/Scripts/玩家管理/`（PlayerStats, PlayerMovement, PlayerPersist）
- 系统/UI：`Assets/Scripts/系统&UI/`（GeneralUIManager, InventoryManager, EquipmentManager, PlayerInfoManager, SkillUIManager, SystemManager, UniversalCursorController, MusicManager, MainMenuManager, CameraFollow2D, SceneTeleporter）
- 装备道具：`Assets/Scripts/装备道具/`（ItemData, ItemObject, AllItemsLibrary）
- 数据 SO：`Assets/SO卡片/`（玩家技能卡/SkillData.cs, 敌人卡/EnemyData.cs + EnemySkillData.cs, 道具卡, 道具库）
- 预制体：`Assets/预制体/`（DamageNumber, Player, ShopSystem_Root, Enemies/, 战斗相关/, 技能/, 系统/UICanvas.prefab）
- 场景：`Assets/Scenes/Level01.scene`, `StartMenu.scene`

## 已知重复/历史包袱

- 两份同 GUID 的 `预制体/系统/UICanvas.prefab` 与 `预制体/系统相关/UICanvas.prefab`（同 base64 GUID，同 hex GUID），重复文件待清理
- 两份 `SkillData.cs`（`玩家技能/` 旧 + `玩家技能卡/` 新）、两份 `EnemyData.cs`（`敌人/` 旧 + `敌人卡/` 新），csproj 只编译新版；旧 .cs 是定时炸弹，未来 Unity 重导可能撞 `CS0101`
- Level01.scene 里有大量指向 GeneralUIManager 但属性是 `subMenu/equipBodyText/equipTechText` 的历史 override 残留（脚本被拆分后未迁移），目前 Unity 静默丢弃，不影响运行，留作清理任务

## AGENTS.md 强制规则（项目根有）

- 改动需考虑全局影响、最小安全改动
- 不破坏 UTF-8 编码、不动 Chinese Header/Tooltip/注释，除非用户明确要求
- 已批准的修改范围内可继续做，但若超出范围（涉及战斗流、存档、装备、UI、输入等主系统）需重新确认
- 修改 .cs 后做语法/字符串/括号/属性的自检
