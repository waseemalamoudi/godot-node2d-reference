# Chapter 01 — Node, CanvasItem, and Node2D Foundation

## هدف الفصل

هذا الفصل يضع الأساس العقلي لفهم Godot قبل الدخول في تفاصيل `Sprite2D`, `CharacterBody2D`, `Area2D`, `StaticBody2D`, وبقية Nodes.

الفكرة الأساسية: Godot لا يبدأ من كود فقط، بل من **شجرة Nodes**. كل كائن في اللعبة يكون Node أو Scene مبنية من عدة Nodes. لذلك اختيار Root Node الصحيح هو أول قرار تصميمي مهم في أي مشهد.

## ماذا ستتعلم؟

- ما هو `Node` ولماذا يعتبر وحدة البناء الأساسية في Godot.
- ما الفرق بين `Node` و `Scene`.
- ما معنى `Root Node`, `Parent Node`, و `Child Node`.
- ما هو `CanvasItem` ولماذا هو أصل مهم لعالم 2D وواجهات UI.
- ما هو `Node2D` وما الذي يضيفه فوق `CanvasItem`.
- كيف تفهم شجرة الوراثة: `Object → Node → CanvasItem → Node2D`.
- كيف تختار Root Node مناسبًا حسب وظيفة المشهد.
- ما الأخطاء الذهنية الشائعة التي يقع فيها المبتدئ.

## ترتيب القراءة المقترح

| الترتيب | الملف | الغرض |
|---:|---|---|
| 1 | `01_What_Is_A_Node.md` | فهم معنى Node كوحدة بناء. |
| 2 | `02_What_Is_A_Scene.md` | فهم Scene كشجرة Nodes قابلة للحفظ وإعادة الاستخدام. |
| 3 | `03_Parent_Child_And_Root_Node.md` | فهم العلاقات داخل الشجرة. |
| 4 | `04_What_Is_CanvasItem.md` | فهم أصل عناصر 2D و UI. |
| 5 | `05_What_Is_Node2D.md` | فهم Node2D كأصل أغلب عناصر عالم 2D. |
| 6 | `06_Inheritance_Tree.md` | قراءة شجرة الوراثة بطريقة صحيحة. |
| 7 | `07_How_To_Choose_Root_Node.md` | اختيار Root Node الصحيح عمليًا. |
| 8 | `08_Common_Beginner_Confusion.md` | تصحيح أكثر الالتباسات شيوعًا. |
| 9 | `09_Chapter_One_Summary.md` | ملخص سريع للحفظ والمراجعة. |

## صورة ذهنية سريعة

```text
Game (Node2D)
├── Player (CharacterBody2D)
│   ├── AnimatedSprite2D
│   ├── CollisionShape2D
│   └── Camera2D
├── Coins (Node2D)
│   └── Coin (Area2D)
└── UI (CanvasLayer)
    └── Control
```

في هذا المثال:

- `Game` ينظم مشهد اللعبة العام.
- `Player` يحتاج حركة وتصادم لذلك يبدأ غالبًا بـ `CharacterBody2D`.
- `Coin` تحتاج كشف لمس فقط لذلك تبدأ غالبًا بـ `Area2D`.
- `UI` لا تبدأ بـ `Node2D` بل تستخدم نظام واجهات `Control` داخل `CanvasLayer`.

## مصادر رسمية

- Godot Node class reference: https://docs.godotengine.org/en/stable/classes/class_node.html
- Godot CanvasItem class reference: https://docs.godotengine.org/en/stable/classes/class_canvasitem.html
- Godot Node2D class reference: https://docs.godotengine.org/en/stable/classes/class_node2d.html
- Godot Nodes and Scenes guide: https://docs.godotengine.org/en/stable/getting_started/step_by_step/nodes_and_scenes.html
