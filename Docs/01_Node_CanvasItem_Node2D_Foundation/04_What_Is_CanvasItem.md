# 04 — ما هو CanvasItem؟

## 1. التعريف المختصر

`CanvasItem` هو أساس العناصر التي تُرسم في مساحة 2D أو تستخدم في واجهة المستخدم. هو ليس النود الذي تبدأ به غالبًا في مشاريعك، لكنه مهم لفهم لماذا يوجد فرعان كبيران:

```text
CanvasItem
├── Node2D    ← لعالم اللعبة 2D
└── Control   ← لواجهات المستخدم UI
```

## 2. لماذا CanvasItem مهم؟

لأنه يشرح العلاقة بين عالم اللعبة والواجهة.

| الفرع | الاستخدام |
|---|---|
| `Node2D` | كائنات عالم 2D مثل اللاعب، العدو، العملات، الأرض، الكاميرا. |
| `Control` | واجهات المستخدم مثل الأزرار، النصوص، القوائم، اللوحات. |

كلاهما يرث من `CanvasItem`، لكن استخدامهما مختلف جدًا.

## 3. شجرة الوراثة المبسطة

```text
Object
└── Node
    └── CanvasItem
        ├── Node2D
        └── Control
```

هذا يعني أن `Node2D` و `Control` يشتركان في بعض مفاهيم الرسم، لكنهما ليسا بديلين لبعضهما.

## 4. الفرق بين Node2D و Control

| المقارنة | `Node2D` | `Control` |
|---|---|---|
| المجال | عالم اللعبة 2D | واجهة المستخدم UI |
| أمثلة | `Sprite2D`, `CharacterBody2D`, `Camera2D` | `Button`, `Label`, `Panel` |
| التموضع | position/rotation/scale في عالم 2D | anchors, margins, containers |
| الاستخدام | لاعب، عدو، أرض، عملة | أزرار، قوائم، HUD، إعدادات |
| هل يصلح للاعب؟ | نعم عبر فروع مثل `CharacterBody2D` | لا |
| هل يصلح لزر قائمة؟ | ليس الأفضل | نعم |

## 5. مثال يوضح الفصل بين اللعبة والواجهة

```text
Game (Node2D)
├── World (Node2D)
│   ├── Player (CharacterBody2D)
│   └── Coins (Node2D)
└── UI (CanvasLayer)
    └── HUD (Control)
        ├── ScoreLabel (Label)
        └── PauseButton (Button)
```

هنا:

- عالم اللعبة يستخدم `Node2D` وفروعه.
- الواجهة تستخدم `Control` وفروعه.
- `CanvasLayer` يجعل الواجهة تظهر فوق عالم اللعبة ولا تتحرك معه بالطريقة نفسها.

## 6. ماذا يرث الأبناء من CanvasItem؟

في عناصر 2D، الأبناء يتأثرون بتحويلات الأب. إذا كان عندك `Node2D` أب وتحته `Sprite2D`، فإن تحريك الأب يحرك الطفل معه.

```text
Ship (Node2D)
├── Sprite2D
└── Marker2D
```

تحريك `Ship` يحرك الصورة ونقطة العلامة معها.

## 7. متى تحتاج معرفة CanvasItem؟

تحتاج فهمه عندما تسأل:

- لماذا `Node2D` و `Control` كلاهما يظهران في 2D؟
- لماذا لا أستخدم `Control` للاعب؟
- لماذا لا أستخدم `Node2D` كزر واجهة؟
- لماذا يتحرك الطفل مع الأب؟
- لماذا ترتيب الرسم والظهور مهم في 2D؟

## 8. أخطاء شائعة

| الخطأ | المشكلة | الصحيح |
|---|---|---|
| استخدام `Node2D` لبناء واجهة | لن تستفيد من anchors و containers. | استخدم `Control`. |
| استخدام `Control` للاعب | ليس مناسبًا لحركة عالم 2D وفيزياء اللعبة. | استخدم `CharacterBody2D`. |
| وضع HUD داخل World مباشرة | قد يتحرك مع الكاميرا أو العالم. | استخدم `CanvasLayer` ثم `Control`. |
| الخلط بين ما يظهر في اللعبة وما يظهر في الواجهة | ينتج تصميم مربك. | افصل World عن UI. |

## 9. خلاصة سريعة

`CanvasItem` هو الجذر المشترك لعالم 2D والواجهة. لكن في العمل العملي احفظ القاعدة:

```text
Node2D = عالم اللعبة
Control = واجهة المستخدم
```

## 10. مصادر رسمية

- CanvasItem class reference: https://docs.godotengine.org/en/stable/classes/class_canvasitem.html
- Node2D class reference: https://docs.godotengine.org/en/stable/classes/class_node2d.html
- Control class reference: https://docs.godotengine.org/en/stable/classes/class_control.html
