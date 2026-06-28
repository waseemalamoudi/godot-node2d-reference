# 08 — أخطاء والتباسات المبتدئين الشائعة

## 1. هدف الملف

هذا الملف يجمع أكثر الالتباسات التي تظهر عند بداية تعلم Godot 2D. الهدف ليس حفظ أسماء Nodes فقط، بل فهم لماذا يكون اختيار معين مناسبًا أو غير مناسب.

## 2. الالتباس الأول: Sprite2D يكفي للاعب

### الخطأ

```text
Player (Sprite2D)
```

أو:

```text
Player (Node2D)
└── Sprite2D
```

### المشكلة

`Sprite2D` يعرض صورة فقط. هو لا يعطيك حركة شخصية، ولا تصادمًا مناسبًا، ولا وظائف مثل التعامل مع الأرض والجدران.

### الصحيح غالبًا

```text
Player (CharacterBody2D)
├── AnimatedSprite2D
└── CollisionShape2D
```

## 3. الالتباس الثاني: Node2D يعني جسمًا فيزيائيًا

### الخطأ

الاعتقاد أن `Node2D` وحده يكفي لأي جسم داخل عالم 2D.

### المشكلة

`Node2D` يعطي position/rotation/scale، لكنه ليس جسمًا فيزيائيًا. لا يصطدم ولا يتحرك بطريقة فيزيائية إلا إذا استخدمت نوعًا متخصصًا.

### الصحيح

| الحاجة | استخدم |
|---|---|
| حركة لاعب بالكود | `CharacterBody2D` |
| أرض ثابتة | `StaticBody2D` |
| جسم يسقط ويتدحرج | `RigidBody2D` |
| كشف دخول/لمس | `Area2D` |
| تنظيم عناصر فقط | `Node2D` |

## 4. الالتباس الثالث: CollisionShape2D يعمل وحده

### الخطأ

```text
Player (Node2D)
└── CollisionShape2D
```

### المشكلة

`CollisionShape2D` هو Node يحدد شكل التصادم، لكنه يحتاج أن يكون تحت Node مناسب مثل `CharacterBody2D`, `StaticBody2D`, `RigidBody2D`, أو `Area2D`.

### الصحيح

```text
Player (CharacterBody2D)
└── CollisionShape2D
```

أو:

```text
Coin (Area2D)
└── CollisionShape2D
```

## 5. الالتباس الرابع: Area2D تمنع المرور

### الخطأ

استخدام `Area2D` كأرض أو جدار يمنع اللاعب من المرور.

### المشكلة

`Area2D` مصممة غالبًا للكشف: دخول جسم، خروج جسم، لمس عملة، دخول منطقة ضرر. هي ليست الخيار الطبيعي لأرض صلبة.

### الصحيح

| الحالة | الصحيح |
|---|---|
| أرض أو جدار | `StaticBody2D` |
| عملة | `Area2D` |
| منطقة ضرر | `Area2D` |
| باب يمنع المرور | `StaticBody2D` أو `AnimatableBody2D` |

## 6. الالتباس الخامس: StaticBody2D مناسب للشخصيات

### الخطأ

استخدام `StaticBody2D` للاعب أو عدو يتحرك ويقفز.

### المشكلة

`StaticBody2D` مخصص للأجسام الثابتة مثل الأرض والجدران. قد تستطيع تغيير position بالكود، لكنه ليس مصممًا لحركة شخصيات، وقد ينتج سلوك غير مناسب.

### الصحيح

```text
Player (CharacterBody2D)
├── AnimatedSprite2D
└── CollisionShape2D
```

استخدم `StaticBody2D` هنا:

```text
Ground (StaticBody2D)
├── Sprite2D
└── CollisionShape2D
```

## 7. الالتباس السادس: Root Node يجب أن يكون Node2D دائمًا

### الخطأ

بدء كل مشهد بـ `Node2D` بدون تفكير.

### المشكلة

بعض المشاهد تحتاج Root متخصصًا.

| المشهد | Root أفضل |
|---|---|
| Player | `CharacterBody2D` |
| Coin | `Area2D` |
| Ground | `StaticBody2D` |
| UI | `Control` |
| Game | `Node2D` |

## 8. الالتباس السابع: Control و Node2D نفس الشيء تقريبًا

### الخطأ

استخدام `Node2D` لبناء الواجهة أو `Control` لبناء اللاعب.

### المشكلة

رغم أن الاثنين مرتبطان بـ `CanvasItem`، إلا أن استخدامهما مختلف.

| الاستخدام | الصحيح |
|---|---|
| لاعب، عدو، عملة | `Node2D` branch |
| زر، نص، قائمة | `Control` branch |
| HUD فوق اللعبة | `CanvasLayer` + `Control` |

## 9. الالتباس الثامن: Scene تعني مرحلة كاملة فقط

### الخطأ

اعتبار أن Scene يجب أن تكون مستوى كاملًا فقط.

### الصحيح

Scene يمكن أن تكون:

- `Player.tscn`
- `Enemy.tscn`
- `Coin.tscn`
- `Bullet.tscn`
- `Door.tscn`
- `Level01.tscn`

أي شجرة Nodes قابلة للحفظ وإعادة الاستخدام يمكن أن تكون Scene.

## 10. الالتباس التاسع: الوراثة تعني نفس الاستخدام

### الخطأ

إذا كان `Sprite2D` و`CharacterBody2D` يرثان من `Node2D`، إذن يمكن استخدامهما لنفس الدور.

### المشكلة

الوراثة تعني مشاركة خصائص عامة، وليس نفس الوظيفة.

| Node | يشترك في | يختلف في |
|---|---|---|
| `Sprite2D` | Transform 2D | يعرض صورة فقط. |
| `CharacterBody2D` | Transform 2D | جسم حركة وتصادم. |
| `Area2D` | Transform 2D | منطقة كشف. |
| `StaticBody2D` | Transform 2D | جسم ثابت صلب. |

## 11. جدول تصحيح سريع

| أريد | لا تبدأ بـ | ابدأ غالبًا بـ |
|---|---|---|
| لاعب يتحرك | `Sprite2D` | `CharacterBody2D` |
| أرض | `Area2D` | `StaticBody2D` |
| عملة | `StaticBody2D` | `Area2D` |
| واجهة | `Node2D` | `Control` |
| صندوق فيزيائي | `Node2D` | `RigidBody2D` |
| مجموعة عناصر | `CharacterBody2D` | `Node2D` |

## 12. خلاصة سريعة

أغلب أخطاء البداية تأتي من سؤال ناقص: “أي Node أستخدم؟” السؤال الصحيح هو: “ما وظيفة هذا الشيء؟ هل يرسم؟ يتحرك؟ يصطدم؟ يكشف فقط؟ ينظم؟ أم هو واجهة؟”.

## 13. مصادر رسمية

- Node class reference: https://docs.godotengine.org/en/stable/classes/class_node.html
- Node2D class reference: https://docs.godotengine.org/en/stable/classes/class_node2d.html
- CharacterBody2D class reference: https://docs.godotengine.org/en/stable/classes/class_characterbody2d.html
- StaticBody2D class reference: https://docs.godotengine.org/en/stable/classes/class_staticbody2d.html
- Area2D class reference: https://docs.godotengine.org/en/stable/classes/class_area2d.html
- Control class reference: https://docs.godotengine.org/en/stable/classes/class_control.html
