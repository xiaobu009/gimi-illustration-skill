# Style · Quirky Sketch（怪诞手绘）

> 当前默认风格预设。定义「怎么画」，不定义「画谁」。

---

## 一句话

怪诞手绘，隐喻叙事，留白透气，不说明书。物件具体、有故事动线，靠线稿和大小区分，不靠铺满色块。

## 画法

**线条** — 粗细不均、带抖动感、可不封口；不矢量、不厚重描边

**背景与留白**

- 白底：白色或轻微米白；无纹理 / 噪点 / 渐变 / 深色背景
- 主体占 50–75%，至少 25% 空白
- **留白 ≠ 少画**：同屏可 **4–8** 个具名物件；靠大小、线稿粗细、前后层次区分，不靠铺色填密

**内容与动线**（字段详则见 shot-config / composition-patterns）

- **具象优先**：画可辨认的具体物件（优先原文名词）；纯抽象象征不能替代
- **故事动线**：箭头 / 路径 / 视线 / 动作连成可读顺序，一眼跟完「发生了什么」

**双色层** — IP 角色跟参考图全色；场景物件线稿为主 + 少量点色（见「颜色」）

## 颜色

| 层 | 规则 |
|----|------|
| **IP 角色** | 锚点色跟 **当次 IP 设定图**（读 `ip/{$IP}/ip.md`）；线条可 sketchy，**颜色不减、不被场景色染色** |
| **场景物件** | 线稿为主；**软蓝主强调** + **软橙可选 ≤2 处点睛**（见「场景色纪律」） |

**场景色纪律**

> 场景以线稿为主；颜色只强调关键信息，不替代物件表意和故事动线。

**软蓝（主强调）** — 路径、按钮、关键物件

- 有填色的场景物件 **≤ 4**（IP 不算）；单物件填色 **≤ 30%**
- 禁止文件夹 / 屏幕 / 标签底大面积填色；不得每个元素都上色

**软橙（点睛）** — **≤ 2 处**

- 仅用于：问句引线、警示线、编号圈、小箭头
- 禁止：橙物件填色、橙标签底

**标注** — 黑色手写 + 黑色箭头

- 禁止：便利贴底色、标签背景色块、彩色下划线

**改动 / 对比** — 删除线、波浪线等线稿符号

- 禁止：红绿 diff 填色；强调优先蓝，橙只作上述点睛

## 审美方向

要：怪诞、创意、有意思、简洁清爽、有故事动线  
不要：可爱、幼稚、死板、彩虹色

## 绝对不要

> 视觉体裁禁令；颜色细则见「场景色纪律」。

- 商业插画 / PPT 信息图 / 正式流程图 / 课程课件
- 儿童插画 / 精致扁平插画 / 科技感 UI
- 复杂背景、渐变、阴影、纹理
- 在图上写结构类型名称

## 留白指令（填入 `{WHITESPACE_DESC}`）

```
subject occupies 50-75% of frame, at least 25% white space,
preferably one continuous empty block, not filled edge to edge;
whitespace does not mean fewer objects — allow 4-8 named objects when the message needs it;
use size, line weight, and depth layering instead of color blocks to create hierarchy
```

## 标注颜色指令（填入 `{LABEL_COLOR}`）

```
black handwritten Chinese labels and black arrows only;
no colored sticky notes, no label background fills, no colorful underlines;
character keeps full reference colors; scene objects stay mostly black line art;
soft blue as the main scene accent (paths, buttons, key object fills);
soft orange for at most 2 small highlights only (question cues, warning lines, step numbers) — no orange object fills or label backgrounds;
blue fills on at most 1-4 key scene objects maximum
```

## Prompt 风格段（填入 `{STYLE_DNA}`）

```
quirky hand-drawn illustration, wobbly ink lines, expressive rough sketch,
naive art style, black hand-drawn line structure for scene objects,
soft blue as main scene accent color,
soft orange on at most 2 small highlight touches (not object fills),
blue local accent fills on at most 1-4 key scene objects;
recognizable concrete objects connected by clear story flow arrows or path;
multiple named objects allowed (typically 4-8) — hierarchy via size and line weight, not more color fills;
changes shown with line marks not red-green color fills;
white background, editorial doodle feeling,
no photorealistic, no smooth vector, no 3D render, no PPT infographic,
no gradient, no cute cartoon, no colored label backgrounds, no large color-filled shapes
```

## 风格适配指令（填入 `{STYLE_ADAPT}`）

当有 IP 角色时（**通用层**，不写死具体锚点名称）：

```
IMPORTANT style adaptation for character:
- Keep ALL identity anchors (clothing, accessories, colors, proportions) exactly as defined in the IP file and reference images
- Only change outline quality to wobbly sketch; do NOT change colors, proportions, or outfit items
- Scene soft-blue and soft-orange accents apply to OBJECTS ONLY — must NOT tint, replace, or bleed into character clothing or accessories
- Scene objects: black line art; soft blue main accents on 1-4 objects; soft orange at most 2 small highlights only
```

> IP 专属锁色（如 Gimi 背心三色条纹）→ 读 `ip/{$IP}/ip.md`「填入 `{IP_STYLE_ADAPT}`」，组装在 `{STYLE_ADAPT}` 之后。

## QA 检查项

> 仅当 `$STYLE=quirky-sketch` 时加载本节。

**构图与留白**

- 主体 50–75%，空白 ≥ 25%；白底，无纹理 / 渐变 / 深色背景

**场景色** — 逐项对照「场景色纪律」

**内容与动线**

- 物件具体可辨认；不像 PPT
- 故事动线可读（箭头 / 路径能跟完）
- 需要信息密度时：具名物件够、层次清，**不等于**过满或铺色

**失败信号（本风格）**

- 填色物件 > 4；单物件填色 > 30%
- 橙 > 2 处，或橙用于物件填底 / 标签底
- **场景点睛色渗入角色服装**（读 `ip/{$IP}/ip.md` 判断具体走形项）
- 红绿 diff 色块；标签 / 便利贴有底色

## 迭代 palette（颜色过多时用）

```
keep character reference colors; scene stays black line art;
soft blue main accent on 1-4 objects max; soft orange at most 2 small highlights;
remove orange object fills, colored label backgrounds, red-green diff fills
```

---

`v1.7` · 2026-07-07 · 收层：IP 专属锁色归 ip 文件；风格层只保留场景色禁染服装
