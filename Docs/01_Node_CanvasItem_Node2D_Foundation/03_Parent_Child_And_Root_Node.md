# 03 — Parent, Child, and Root Node

## 1. التعريف المختصر

في Godot، المشهد عبارة عن شجرة. أول Node في الشجرة يسمى `Root Node`. أي Node تحتوي Nodes أخرى تسمى `Parent`. وأي Node داخل Node أخرى تسمى `Child`.

## 2. مثال سريع

```text
Player (CharacterBody2D)      ← Root Node
├── AnimatedSprite2D          ← Child of Player
├── CollisionShape2D          ← Child of Player
└── Camera2D                  ← Child of Player
```

هنا `Player` هو الجذر، وبقية Nodes أبناء له.

## 3. المصطلحات الأساسية

| المصطلح | المعنى |
|---|---|
| `Root Node` | أول Node في Scene. يمثل أساس المشهد. |
| `Parent Node` | Node تحتوي Node أو أكثر داخلها. |
| `Child Node` | Node موجودة تحت Parent. |
| `Sibling Nodes` | Nodes لها نفس الأب. |
| `Scene Tree` | الشجرة الكاملة أثناء التشغيل. |

## 4. لماذا العلاقة بين Parent و Child مهمة؟

لأن الأبناء يتأثرون غالبًا بتحويلات الأب مثل المكان والدوران والحجم.

مثال:

```text
Ship (Node2D)
├── BodySprite (Sprite2D)
├── EngineFire (AnimatedSprite2D)
└── ShootPoint (Marker2D)
```

إذا تحركت `Ship`، يتحرك معها الشكل والنار ونقطة إطلاق الرصاص. هذا يجعل التركيب منطقيًا وسهلًا.

## 5. Root Node ليس دائمًا Node2D

خطأ شائع أن يظن المبتدئ أن أي مشهد 2D يجب أن يبدأ دائمًا بـ `Node2D`.

الصحيح: Root Node يختار حسب وظيفة المشهد.

| المشهد | Root Node مناسب |
|---|---|
| مشهد لعبة عام | `Node2D` |
| لاعب | `CharacterBody2D` |
| أرض | `StaticBody2D` |
| عملة | `Area2D` |
| واجهة | `Control` |
| نقطة تنظيم فقط | `Node` أو `Node2D` حسب السياق |

## 6. ماذا يحدث عند تحريك الأب؟

إذا كان لديك:

```text
PlatformGroup (Node2D)
├── PlatformA (StaticBody2D)
├── PlatformB (StaticBody2D)
└── Decoration (Sprite2D)
```

وتحرك `PlatformGroup`، ستتحرك معه العناصر تحته. لذلك نستخدم Parent Node لتجميع وتحريك عناصر مرتبطة ببعضها.

## 7. متى أضع Node كابن؟

ضع Node كابن عندما يكون تابعًا منطقيًا للأب.

| الأب | الطفل المناسب | السبب |
|---|---|---|
| `Player` | `AnimatedSprite2D` | الشكل يتبع اللاعب. |
| `Player` | `CollisionShape2D` | التصادم يجب أن يتحرك مع اللاعب. |
| `Player` | `Camera2D` | الكاميرا قد تتبع اللاعب. |
| `Weapon` | `Marker2D` | نقطة خروج الرصاصة تتبع السلاح. |
| `Coin` | `CollisionShape2D` | منطقة الالتقاط تتبع العملة. |

## 8. متى لا أضع Node كابن؟

لا تجعل Node ابنًا إذا لم يكن تابعًا منطقيًا للأب.

مثال: لا تجعل كل الأعداء أبناء للاعب؛ لأن الأعداء ليسوا جزءًا من اللاعب.

```text
Wrong:
Player
└── Enemy
```

الأفضل:

```text
Level (Node2D)
├── Player
└── Enemies (Node2D)
    └── Enemy
```

## 9. مثال GDScript

```gdscript
func _ready() -> void:
    var child := Marker2D.new()
    child.name = "ShootPoint"
    add_child(child)
```

`add_child()` تجعل النود الجديدة ابنًا للنود الحالية.

## 10. أخطاء شائعة

| الخطأ | المشكلة | الصحيح |
|---|---|---|
| وضع Nodes غير مرتبطة تحت بعضها | الحركة والتحكم يصبحان مربكين. | اجعل العلاقة منطقية. |
| نسيان أن الطفل يتبع Transform الأب | قد يتحرك شيء دون توقعك. | راقب Parent/Child عند التحريك. |
| جعل اللاعب ابنًا للكاميرا | غالبًا العلاقة معكوسة. | الكاميرا غالبًا ابن اللاعب أو مستقلة تتبعه بالكود. |
| وضع UI داخل Player | الواجهة لا تتبع اللاعب عادة. | ضع UI داخل `CanvasLayer`. |

## 11. خلاصة سريعة

الشجرة ليست ترتيبًا شكليًا فقط. هي تعبر عن علاقة ملكية وتبعية عملية. إذا كان الشيء يجب أن يتحرك أو يدور أو يعيش مع شيء آخر، غالبًا يكون ابنًا له.

## 12. مصادر رسمية

- Node class reference: https://docs.godotengine.org/en/stable/classes/class_node.html
- CanvasItem class reference: https://docs.godotengine.org/en/stable/classes/class_canvasitem.html
