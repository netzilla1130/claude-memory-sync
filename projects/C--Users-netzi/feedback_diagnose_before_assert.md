---
name: 静态文件诊断不要早下结论
description: 排查 Unity 场景/预制体问题时，遇到被 Canvas/Animator/其它组件 "driven" 的字段，不能只看 YAML 序列化值就断言根因
type: feedback
originSessionId: fe8c8b18-2034-488a-922a-cd8a11b15bee
---
排查 Step-2 项目按键失灵问题时，我在 prefab YAML 里看到 UICanvas 根 `m_LocalScale: (0,0,0)` 就给用户提了"scale=0 让菜单不可见"的修复方案；用户进 Tuanjie 截图给我看，发现 Inspector 上写着 "Some values driven by Canvas."，运行时 scale 实际是 ~0.9953（Canvas 驱动），我那个假设完全错。真正根因是用户取消勾选了 UICanvas GameObject，导致单例 Awake 没跑。

**Why:** Unity 的 Canvas (Screen Space - Overlay/Camera 模式) 会在运行时驱动其 RectTransform 的 Position/Size/Scale，**prefab 序列化里的值只是"备份"，Canvas 活着时根本不会用**。Animator 驱动 Transform、Cinemachine 驱动 Camera 也是同一类机制。只看 .prefab / .scene YAML 文本，得到的可能不是运行时真实状态。

**How to apply:**
1. 给 Unity 场景/预制体问题下根因结论前，**先问自己**：这个字段是不是被同 GameObject 上某个组件 driven？Canvas、Animator、CinemachineBrain、Constraints 都属于这类
2. 怀疑某个数值异常时，**优先让用户在 Tuanjie Editor 里查实际 Inspector 值或 Play Mode 下的运行时值**，而不是基于 YAML 文本直接给修复方案
3. 静态分析能提出**候选假设**，但下结论前需要一次运行时确证（让用户截图 Inspector / Console / Hierarchy active 状态，或加一行 Debug.Log）
4. 如果先提了错误假设，要及时撤回并道歉，再继续排查；不要硬撑

这次的具体表现：用户给的 Console 截图里 PlayerMovement 日志说 `isBattleMode=False / isAnyMenuOpen=False`，我曾把它当成 "GeneralUIManager.instance 是活的" 的证据，但实际上 `instance == null` 时短路也会得到同样的 False，日志不能区分这两种情况。**布尔短路求值下的 false 日志不能反推前置条件**，下次要警惕这种"双重含义"的日志读数。
