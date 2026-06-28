# 09 — ملخص الفصل الأول

## 1. الخلاصة الكبرى

الفصل الأول يشرح كيف تفكر في Godot قبل اختيار أي Node.

القاعدة الأساسية:

```text
Godot = Scenes مبنية من Nodes داخل شجرة منظمة
```

اختيار Root Node الصحيح يعتمد على وظيفة المشهد، وليس على أن الاسم مألوف أو أن النود تظهر في قائمة 2D.

## 2. تعريفات سريعة

| المصطلح | المعنى المختصر |
|---|---|
| `Node` | وحدة بناء داخل Godot. |
| `Scene` | شجرة Nodes محفوظة ويمكن إعادة استخدامها. |
| `Root Node` | أول Node في Scene. |
| `Parent Node` | Node تحتوي أبناء. |
| `Child Node` | Node داخل Node أخرى. |
| `CanvasItem` | أصل عناصر 2D و UI. |
| `Node2D` | أصل أغلب كائنات عالم 2D. |
| `Control` | أصل واجهات المستخدم. |

## 3. الشجرة الأساسية التي يجب حفظها

```text
Object
└── Node
    └── CanvasItem
        ├── Node2D     ← عالم اللعبة 2D
        └── Control    ← واجهة المستخدم UI
```

## 4. ماذا أستخدم؟

| أريد إنشاء | أبدأ غالبًا بـ | السبب |
|---|---|---|
| مشهد لعبة 2D كامل | `Node2D` | تنظيم عناصر العالم. |
| لاعب | `CharacterBody2D` | حركة وتصادم. |
| عدو يتحرك بالكود | `CharacterBody2D` | حركة وسلوك. |
| أرض أو جدار | `StaticBody2D` | جسم ثابت يمنع المرور. |
| جسم يسقط بالفيزياء | `RigidBody2D` | المحرك الفيزيائي يتحكم به. |
| منصة أو باب متحرك | `AnimatableBody2D` | يتحرك يدويًا ويتفاعل مع الأجسام. |
| عملة أو Trigger | `Area2D` | كشف لمس أو دخول فقط. |
| واجهة مستخدم | `Control` | أزرار، نصوص، قوائم. |
| مجموعة عناصر 2D | `Node2D` | تنظيم وتحريك مجموعة. |
| صورة فقط | `Sprite2D` | عرض صورة. |

## 5. أمثلة حفظ سريعة

### Player

```text
Player (CharacterBody2D)
├── AnimatedSprite2D
├── CollisionShape2D
└── Camera2D
```

### Coin

```text
Coin (Area2D)
├── Sprite2D
└── CollisionShape2D
```

### Ground

```text
Ground (StaticBody2D)
├── Sprite2D
└── CollisionShape2D
```

### Game

```text
Game (Node2D)
├── Player
├── Enemies
├── Pickups
└── UI
```

### UI

```text
HUD (Control)
├── ScoreLabel
└── PauseButton
```

## 6. أهم قواعد الفصل

1. `Node` لا تعني صورة.
2. `Scene` لا تعني مرحلة كاملة فقط.
3. `Root Node` يختار حسب وظيفة المشهد.
4. `Node2D` ليس جسمًا فيزيائيًا وحده.
5. `Sprite2D` لا يكفي للاعب.
6. `CollisionShape2D` يحتاج جسمًا أو Area مناسبًا فوقه.
7. `Area2D` للكشف، وليس للأرض الصلبة غالبًا.
8. `StaticBody2D` للأرض والجدران، وليس للاعب المتحرك.
9. `Control` للواجهة، و`Node2D` لعالم اللعبة.
10. الوراثة لا تعني نفس الاستخدام.

## 7. أسئلة مراجعة

استخدم هذه الأسئلة للتأكد أنك فهمت الفصل:

- ما الفرق بين `Node` و `Scene`؟
- لماذا لا نستخدم `Sprite2D` وحده للاعب؟
- لماذا `Coin` غالبًا تبدأ بـ `Area2D`؟
- لماذا `Ground` غالبًا تبدأ بـ `StaticBody2D`؟
- ما الفرق بين `Node2D` و `Control`؟
- ماذا يعني أن `Sprite2D` و`CharacterBody2D` يرثان من `Node2D`؟
- هل الوراثة وحدها تكفي لاختيار Root Node؟

## 8. ما الذي سيأتي بعد هذا الفصل؟

بعد هذا الأساس، يمكن الانتقال إلى الفصل الثاني:

```text
Docs/02_Node2D_Derived_Nodes/
```

وفيه سنفصل Nodes المشتقة أو المرتبطة عمليًا بـ `Node2D` مثل:

- `Sprite2D`
- `AnimatedSprite2D`
- `Camera2D`
- `Marker2D`
- `Polygon2D`
- `Line2D`
- `Light2D`
- `TileMapLayer`
- `Path2D`
- وغيرها.

## 9. مصادر رسمية

- Node class reference: https://docs.godotengine.org/en/stable/classes/class_node.html
- Nodes and Scenes guide: https://docs.godotengine.org/en/stable/getting_started/step_by_step/nodes_and_scenes.html
- CanvasItem class reference: https://docs.godotengine.org/en/stable/classes/class_canvasitem.html
- Node2D class reference: https://docs.godotengine.org/en/stable/classes/class_node2d.html
