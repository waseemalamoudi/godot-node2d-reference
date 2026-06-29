# الفصل الثاني: اختيار النود الصحيحة في ألعاب 2D

## حالة الفصل

**مغلق / Closed**

هذا الفصل يشرح كيف تختار النود المناسبة في Godot 2D حسب وظيفة الكائن داخل اللعبة.

---

## 1. الهدف من الفصل

بعد الفصل الأول عن ترتيب النود داخل المشهد، نحتاج الآن أن نعرف: أي نود نستخدم لكل كائن؟

بنهاية الفصل تعرف متى تستخدم:

- `Node2D`
- `CharacterBody2D`
- `StaticBody2D`
- `RigidBody2D`
- `Area2D`
- `Sprite2D`
- `CollisionShape2D`

---

## 2. خلاصة البحث الرسمي

توثيق Godot الرسمي يوضح الآتي:

- `Node2D`: كائن 2D عام له موقع ودوران وحجم، وترث منه أغلب نود 2D.
- `CharacterBody2D`: جسم مخصص للشخصيات التي تتحرك بالكود.
- `StaticBody2D`: جسم ثابت مناسب للأرضيات والجدران.
- `RigidBody2D`: جسم يتحرك بواسطة محاكاة الفيزياء.
- `Area2D`: منطقة كشف لدخول أو خروج الأجسام.
- `Sprite2D`: تعرض صورة فقط.
- `CollisionShape2D`: تضيف شكل تصادم أو كشف للأب.

---

## 3. القاعدة الذهبية

لا تسأل أولًا: ما اسم النود؟

اسأل: ما وظيفة هذا الشيء داخل اللعبة؟

```text
تنظيم أو جذر مشهد؟ -> Node2D
لاعب أو عدو يتحرك بالكود؟ -> CharacterBody2D
أرض أو جدار ثابت؟ -> StaticBody2D
صندوق أو كرة تتحرك بالفيزياء؟ -> RigidBody2D
عملة أو منطقة تفعيل؟ -> Area2D
صورة فقط؟ -> Sprite2D
شكل تصادم؟ -> CollisionShape2D
```

---

## 4. Node2D

`Node2D` مناسبة كأب عام أو منظم داخل عالم 2D.

أمثلة:

```text
Level
EnemySpawner
WeaponHolder
EffectsRoot
```

مثال:

```text
Level (Node2D)
├── Player
├── Ground
└── Coins
```

`Node2D` لا تعني وجود تصادم أو فيزياء.

---

## 5. CharacterBody2D

`CharacterBody2D` مناسبة للشخصيات التي تتحرك بالكود.

تستخدم مع:

- اللاعب
- عدو متحرك
- شخصية تقفز
- NPC يتحرك

تركيب لاعب بسيط:

```text
Player (CharacterBody2D)
├── Sprite2D
├── CollisionShape2D
└── Camera2D
```

مثال حركة مبسط:

```gdscript
extends CharacterBody2D

const SPEED = 250.0
const JUMP_VELOCITY = -400.0
const GRAVITY = 900.0

func _physics_process(delta: float) -> void:
	if not is_on_floor():
		velocity.y += GRAVITY * delta

	var direction := Input.get_axis("ui_left", "ui_right")
	velocity.x = direction * SPEED

	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	move_and_slide()
```

---

## 6. StaticBody2D

`StaticBody2D` مناسبة للأشياء الثابتة مثل:

- الأرض
- الجدار
- منصة ثابتة
- حدود المرحلة

مثال:

```text
Ground (StaticBody2D)
├── Sprite2D
└── CollisionShape2D
```

لا نستخدم `StaticBody2D` للاعب لأن اللاعب يحتاج حركة وقفز وسرعة واستجابة للمدخلات. هذه وظيفة `CharacterBody2D`.

---

## 7. RigidBody2D

`RigidBody2D` مناسبة للأجسام التي تتحرك بالفيزياء.

أمثلة:

- صندوق يدفعه اللاعب
- كرة تسقط
- حجر يتدحرج

مثال:

```text
Box (RigidBody2D)
├── Sprite2D
└── CollisionShape2D
```

الفرق المهم:

```text
CharacterBody2D: أنت تتحكم بالحركة بالكود.
RigidBody2D: الفيزياء تتحكم بالحركة غالبًا.
```

---

## 8. Area2D

`Area2D` مناسبة للكشف والتفعيل، وليست أرضًا صلبة.

أمثلة:

- عملة
- منطقة ضرر
- Checkpoint
- باب يتفعل عند الاقتراب

مثال:

```text
Coin (Area2D)
├── Sprite2D
└── CollisionShape2D
```

---

## 9. Sprite2D

`Sprite2D` تعرض صورة 2D فقط.

مثال:

```text
Player (CharacterBody2D)
└── Sprite2D
```

لا تجعل اللاعب `Sprite2D` فقط، لأن الصورة لا تعطي تصادمًا ولا حركة فيزيائية.

---

## 10. CollisionShape2D

`CollisionShape2D` تعطي شكل التصادم أو الكشف للأب.

أمثلة صحيحة:

```text
Player (CharacterBody2D)
└── CollisionShape2D

Ground (StaticBody2D)
└── CollisionShape2D

Coin (Area2D)
└── CollisionShape2D
```

بدونها قد ترى الصورة، لكن التصادم أو الكشف لا يعمل كما تتوقع.

---

## 11. جدول القرار السريع

| الشيء داخل اللعبة | النود المناسبة |
|---|---|
| مشهد المرحلة | `Node2D` |
| اللاعب | `CharacterBody2D` |
| عدو يتحرك بالكود | `CharacterBody2D` |
| أرض | `StaticBody2D` |
| جدار | `StaticBody2D` |
| صندوق فيزيائي | `RigidBody2D` |
| كرة تسقط | `RigidBody2D` |
| عملة | `Area2D` |
| منطقة تفعيل | `Area2D` |
| صورة | `Sprite2D` |
| شكل تصادم | `CollisionShape2D` |

---

## 12. مثال مشهد كامل

```text
Level (Node2D)
├── Player (CharacterBody2D)
│   ├── Sprite2D
│   ├── CollisionShape2D
│   └── Camera2D
│
├── Ground (StaticBody2D)
│   ├── Sprite2D
│   └── CollisionShape2D
│
├── Coin (Area2D)
│   ├── Sprite2D
│   └── CollisionShape2D
│
└── Box (RigidBody2D)
    ├── Sprite2D
    └── CollisionShape2D
```

---

## 13. أخطاء شائعة

1. استخدام `Sprite2D` وحدها للاعب.
2. استخدام `StaticBody2D` للشخصيات المتحركة.
3. استخدام `Area2D` كأرض يقف عليها اللاعب.
4. نسيان `CollisionShape2D`.
5. اختيار النود حسب شكل الكائن بدل وظيفته.

---

## 14. تمرين الفصل

اختر النود المناسبة:

```text
1. لاعب يمشي ويقفز -> CharacterBody2D
2. أرض يقف عليها اللاعب -> StaticBody2D
3. عملة يجمعها اللاعب -> Area2D
4. صندوق يمكن دفعه -> RigidBody2D
5. منطقة إذا دخلها اللاعب يخسر -> Area2D
6. نقطة يظهر منها العدو -> Node2D أو Marker2D
7. صورة اللاعب -> Sprite2D
8. شكل تصادم اللاعب -> CollisionShape2D
```

---

## 15. ملخص الفصل

- `Node2D`: تنظيم أو جذر 2D.
- `CharacterBody2D`: شخصية تتحرك بالكود.
- `StaticBody2D`: أرض أو جدار ثابت.
- `RigidBody2D`: جسم يتحرك بالفيزياء.
- `Area2D`: منطقة كشف.
- `Sprite2D`: صورة فقط.
- `CollisionShape2D`: شكل تصادم أو كشف.

القاعدة النهائية:

> اختر النود حسب وظيفة الكائن داخل اللعبة، لا حسب شكله.

---

## 16. إغلاق الفصل الثاني

تم إغلاق الفصل الثاني لأنه يحتوي على:

- شرح اختيار النود الصحيحة.
- توضيح الفرق بين النود الأساسية.
- تفسير سبب عدم استخدام `StaticBody2D` للاعب.
- مثال مشهد كامل.
- جدول قرار سريع.
- أخطاء شائعة.
- تمرين مع الإجابة.

**الحالة النهائية: Chapter 02 Closed.**
