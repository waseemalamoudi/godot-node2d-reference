# 02 — ما هي Scene؟

## 1. التعريف المختصر

`Scene` في Godot هي شجرة Nodes محفوظة كملف يمكن إعادة استخدامها. المشهد ليس دائمًا “مرحلة كاملة” أو “شاشة كاملة”. يمكن أن يكون المشهد لاعبًا، عدوًا، عملة، رصاصة، بابًا، أو مستوى كاملًا.

## 2. الفرق بين Node و Scene

| المقارنة | Node | Scene |
|---|---|---|
| المعنى | عنصر واحد داخل الشجرة | شجرة Nodes محفوظة |
| مثال | `Sprite2D`, `Camera2D` | `Player.tscn`, `Coin.tscn` |
| إعادة الاستخدام | يستخدم داخل مشهد | يمكن نسخه/استدعاؤه كـ instance |
| التركيب | قد يحتوي أبناء | غالبًا يحتوي Root Node وأبناء |

## 3. لماذا Scene مهمة؟

لو صممت اللاعب كمجموعة Nodes داخل مشهد واحد ضخم فقط، سيكون من الصعب نسخه أو تعديله أو إعادة استخدامه. أما إذا جعلته Scene مستقلة:

```text
Player.tscn
└── Player (CharacterBody2D)
    ├── AnimatedSprite2D
    ├── CollisionShape2D
    └── Camera2D
```

يمكنك إضافته إلى أي مستوى بسهولة.

## 4. Scene يمكن أن تكون صغيرة جدًا

مثال عملة:

```text
Coin.tscn
└── Coin (Area2D)
    ├── Sprite2D
    └── CollisionShape2D
```

هذه Scene صغيرة، لكنها مفيدة لأنك تستطيع وضع 100 عملة في المستوى من نفس الملف.

## 5. Scene يمكن أن تكون كبيرة

مثال مستوى كامل:

```text
Level01.tscn
└── Level01 (Node2D)
    ├── TileMapLayer
    ├── Player
    ├── Enemies
    ├── Coins
    └── UI
```

هنا Scene تمثل مرحلة كاملة.

## 6. مبدأ إعادة الاستخدام

Godot يشجع على بناء اللعبة من Scenes صغيرة قابلة للتركيب:

```text
Game
├── Player.tscn
├── Enemy.tscn
├── Coin.tscn
├── Door.tscn
└── Bullet.tscn
```

بدلًا من جعل كل شيء داخل ملف واحد ضخم.

## 7. مثال GDScript لاستدعاء Scene

```gdscript
const CoinScene := preload("res://Coin.tscn")

func spawn_coin(position: Vector2) -> void:
    var coin := CoinScene.instantiate()
    coin.position = position
    add_child(coin)
```

هذا المثال يوضح أن Scene يمكن تحميلها ثم إنشاء نسخة منها داخل المشهد الحالي.

## 8. أخطاء شائعة

| الخطأ | المشكلة | الصحيح |
|---|---|---|
| أظن أن Scene تعني شاشة كاملة فقط | ستجعل كل شيء في ملف واحد ضخم. | Scene يمكن أن تكون لاعبًا أو عملة أو رصاصة. |
| أضع كل الأعداء يدويًا داخل مشهد واحد | يصعب إعادة استخدام العدو. | اصنع `Enemy.tscn` مستقل. |
| أخلط بين Scene و Node | Scene هي شجرة Nodes، وليست Node واحدة فقط. | افهم أن كل Scene لها Root Node. |
| أغير Instance دون فهم المصدر | قد تخلط بين المشهد الأصلي والنسخة. | تعلم الفرق بين Scene resource و instance. |

## 9. متى تجعل الشيء Scene مستقلة؟

| الحالة | اجعله Scene؟ | السبب |
|---|---:|---|
| اللاعب | نعم | كائن مركب ومهم. |
| العدو | نعم | يتكرر ويحتاج سلوك مستقل. |
| العملة | نعم | تتكرر كثيرًا. |
| الأرضية البسيطة داخل مستوى واحد | ليس دائمًا | قد تكفي داخل Level أو TileMapLayer. |
| الرصاصة | نعم غالبًا | تتولد وتتكرر. |
| زر UI خاص يتكرر | نعم أحيانًا | إذا له تصميم وسلوك خاص. |

## 10. خلاصة سريعة

`Scene` هي طريقة Godot في تنظيم وإعادة استخدام الكائنات. فكر في كل شيء متكرر أو مستقل كمرشح لأن يكون Scene خاصة به.

## 11. مصادر رسمية

- Nodes and Scenes guide: https://docs.godotengine.org/en/stable/getting_started/step_by_step/nodes_and_scenes.html
- Node class reference: https://docs.godotengine.org/en/stable/classes/class_node.html
