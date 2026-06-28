# 05 — ما هو Node2D؟

## 1. التعريف المختصر

`Node2D` هو أساس أغلب كائنات عالم 2D في Godot. هو Node يملك تحويلات 2D مثل:

- `position`
- `rotation`
- `scale`
- `skew`

ويستخدم كأب لتحريك أو تدوير أو تكبير مجموعة عناصر 2D معًا.

## 2. شجرة الوراثة المختصرة

```text
Object
└── Node
    └── CanvasItem
        └── Node2D
```

هذا يعني أن `Node2D` هو فرع من `CanvasItem`، ومخصص لعالم اللعبة ثنائي الأبعاد.

## 3. ما الذي يفعله Node2D؟

| الوظيفة | الشرح |
|---|---|
| تحديد مكان | يستطيع حمل position داخل عالم 2D. |
| تدوير | يستطيع تدوير نفسه وأبناءه. |
| تكبير/تصغير | يستطيع تغيير scale لنفسه وأبناءه. |
| تنظيم | يستخدم كمجموعة لعناصر مرتبطة. |
| ترتيب رسم | له خصائص تساعد في ترتيب الظهور داخل 2D. |

## 4. ما الذي لا يفعله Node2D وحده؟

`Node2D` لا يعني تلقائيًا أنه:

- يرسم صورة.
- يملك تصادمًا.
- يتحرك كجسم فيزيائي.
- يكشف لمس اللاعب.
- يصلح وحده كلاعب platformer.

لذلك هذه الشجرة غير كافية غالبًا للاعب:

```text
Player (Node2D)
├── Sprite2D
└── CollisionShape2D
```

المشكلة: `CollisionShape2D` يحتاج جسمًا مناسبًا مثل `CharacterBody2D` أو `Area2D` أو `StaticBody2D` لكي يكون له معنى فيزيائي واضح.

## 5. متى تستخدم Node2D كـ Root؟

استخدم `Node2D` كـ Root عندما يكون المشهد هدفه التنظيم أو تمثيل مجموعة عناصر 2D، وليس جسمًا فيزيائيًا محددًا.

أمثلة مناسبة:

```text
Game (Node2D)
├── World
├── Player
├── Enemies
└── Pickups
```

```text
EnemyGroup (Node2D)
├── Enemy
├── Enemy
└── Enemy
```

```text
DecorationCluster (Node2D)
├── Sprite2D
├── Sprite2D
└── PointLight2D
```

## 6. متى لا تستخدم Node2D كـ Root؟

| الحالة | الأفضل | السبب |
|---|---|---|
| لاعب يتحرك ويقفز | `CharacterBody2D` | يحتاج حركة وتصادم مناسبين. |
| عملة تجمع باللمس | `Area2D` | تحتاج كشف دخول اللاعب. |
| أرض أو جدار | `StaticBody2D` | يحتاج جسمًا صلبًا ثابتًا. |
| كرة تسقط بالفيزياء | `RigidBody2D` | تحتاج محرك الفيزياء. |
| واجهة مستخدم | `Control` | تحتاج نظام UI. |

## 7. مثال صحيح: مشهد لعبة عام

```text
MainGame (Node2D)
├── Level (Node2D)
│   ├── TileMapLayer
│   └── StaticBodies
├── Player (CharacterBody2D)
├── Enemies (Node2D)
├── Pickups (Node2D)
└── CameraRig (Node2D)
```

هنا `MainGame` يستخدم كحاوية منظمة، وليس كجسم فيزيائي.

## 8. مثال صحيح: تجميع عنصر بصري

```text
Torch (Node2D)
├── Sprite2D
├── PointLight2D
└── GPUParticles2D
```

`Torch` كـ `Node2D` منطقي لأنه يجمع شكل الشعلة، الضوء، والجسيمات في مكان واحد.

## 9. مثال غير مناسب: لاعب بـ Node2D فقط

```text
Player (Node2D)
├── Sprite2D
└── CollisionShape2D
```

هذا قد يبدو منطقيًا للمبتدئ، لكنه غير مناسب لأن اللاعب يحتاج جسمًا يتحرك ويتعامل مع التصادم. الأفضل:

```text
Player (CharacterBody2D)
├── AnimatedSprite2D
└── CollisionShape2D
```

## 10. أخطاء شائعة

| الخطأ | المشكلة | الصحيح |
|---|---|---|
| استخدام `Node2D` للاعب فقط | لا يوفر حركة فيزيائية جاهزة. | `CharacterBody2D`. |
| وضع `CollisionShape2D` تحت `Node2D` مباشرة | لا يعمل كجسم تصادم مستقل بالطريقة المتوقعة. | ضعه تحت جسم فيزيائي أو `Area2D`. |
| استخدام `Node2D` للواجهة | لا يملك نظام anchors/containers. | `Control`. |
| الاعتقاد أن كل ما يرث من Node2D له نفس الاستخدام | الوراثة لا تعني نفس الوظيفة. | اختر حسب الدور. |

## 11. خلاصة سريعة

استخدم `Node2D` للتنظيم أو كأصل عام لعناصر عالم 2D. لا تستخدمه وحده عندما تحتاج فيزياء أو تصادم أو كشف لمس. في هذه الحالات استخدم Node متخصصًا مثل `CharacterBody2D`, `StaticBody2D`, `RigidBody2D`, أو `Area2D`.

## 12. مصادر رسمية

- Node2D class reference: https://docs.godotengine.org/en/stable/classes/class_node2d.html
- CanvasItem class reference: https://docs.godotengine.org/en/stable/classes/class_canvasitem.html
