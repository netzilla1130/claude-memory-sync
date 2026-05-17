---
name: Step-2 UI/Canvas 隐藏与运行时激活模式
description: 用户希望部分 UI 根对象（BattleCanvas、ShopSystem）默认在 Inspector 里不勾选，仅在条件触发时由代码激活；但 UICanvas 必须保持勾选
type: project
originSessionId: fe8c8b18-2034-488a-922a-cd8a11b15bee
---
用户曾让 Codex 实现"BattleCanvas / ShopSystem 在编辑器可取消勾选，进入战斗或打开商店时由代码自动激活"的模式。Codex 实现位置：

- `BattleController.ResolveBattleUIManager(bool activateIfNeeded)`：用 `Resources.FindObjectsOfTypeAll<BattleUIManager>` 找到 inactive 的实例并按需 `SetActive(true)`。`StartBattle` 和 `EndBattle` 都走这个解析器
- `ShopManager.Awake/OpenShop`：检测 inactive 的 `shopSystemRoot` 并激活，`shopSystemRoot` 为空时回退到 `gameObject`

**关键边界：这套"按需激活"机制只覆盖 BattleCanvas 和 ShopSystem，没有覆盖通用 UICanvas（持有 InventoryManager / EquipmentManager / PlayerInfoManager / SkillUIManager / SystemManager 等单例）。**

**Why:** UICanvas 持有的那 5 个单例 Awake 不跑 → `xxx.instance == null` → `GeneralUIManager.Update` 里所有 `Input.GetKeyDown(KeyCode.I/C/E/S/Esc)` 后面的 `&& xxx.instance != null` 判断全部短路 → 按键全部静默失效。

**How to apply:**
1. 用户报"按 I/C/E/S/ESC 打不开菜单"或"商店里按 T 呼不出 Trade UI" 时，**第一时间问** UICanvas 在场景里是否勾选 active。这一次的元凶就是 UICanvas 被用户误以为也被覆盖了所以取消勾选，结果整片 manager 没 Awake
2. 不要主动给"BattleCanvas / ShopSystem 取消勾选"扩展到 UICanvas，除非用户明确再下指令并接受要在 GeneralUIManager / 其余 5 个 manager 上都加按需激活逻辑
3. 如果未来用户确实要 UICanvas 也支持取消勾选，正确做法是 GeneralUIManager.Awake 加 `Resources.FindObjectsOfTypeAll<...>` + 主动 `SetActive(true)` 风格的解析器，对每个 manager 单例都加同样的兜底；不可只在 GeneralUIManager 上做
