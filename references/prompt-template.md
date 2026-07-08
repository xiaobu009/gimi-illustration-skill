# Prompt Template（组装器）

> 本文件只定义组装逻辑。风格词从 `styles/{当前风格}.md` 获取，
> IP 从 `ip/{当前IP}/ip.md` 获取。不硬编码任何风格/IP 词。

每张图单独生成，不要把多张图拼在一张里。

---

## 组装顺序

```
{REF_IMAGE}
{IP_DESC}              ← $IP=gimi 时紧接参考图，先于风格段
{STYLE_DNA}
{WHITESPACE_DESC}
{STYLE_ADAPT}          ← 来自 styles/{$STYLE}.md，通用层
{IP_STYLE_ADAPT}       ← 来自 ip/{$IP}/ip.md（有则填，无则留空）
{COMPOSITION_DESC}
{MESSAGE_DESC}
{SUBJECT_DESC}
{TITLE_DESC}
{MOOD_DESC}
{RATIO_DESC}
{OUTPUT_SIZE_DESC}
{LABEL_DESC}
{NEGATIVE_DESC}
```

---

## 变量来源

| 变量 | 来源文件 | 说明 |
|------|----------|------|
| `{REF_IMAGE}` | `assets/ip/` 或 `assets/none/` | 作为 image reference 附件传入（见下方） |
| `{STYLE_DNA}` | `styles/{$STYLE}.md` → "Prompt 风格段" | 纯风格描述，不含 IP |
| `{WHITESPACE_DESC}` | `styles/{$STYLE}.md` → "留白指令" | 留白比例约束 |
| `{STYLE_ADAPT}` | `styles/{$STYLE}.md` → "风格适配指令" | 有 IP 时才加；**通用层**，不写死具体锚点 |
| `{IP_STYLE_ADAPT}` | `ip/{$IP}/ip.md` → "填入 `{IP_STYLE_ADAPT}`" | 有 IP 且该文件有此节时追加；无则留空 |
| `{IP_DESC}` | 见下方 | 按角色选择读取 |
| `{COMPOSITION_DESC}` | `shot-config.md` → Shot「构图」 | 从构图库选择或自定义 |
| `{MESSAGE_DESC}` | `shot-config.md` → Shot「核心信息」 | 先定义内容表意，再决定画面 |
| `{SUBJECT_DESC}` | `shot-config.md` → Shot「隐喻」 | 具象物件 + 结构；见下方填法 |
| `{MOOD_DESC}` | 可选 | 如不确定留空 |
| `{RATIO_DESC}` | 见下方 | 按 `$RATIO` 填入，**核心比例约束** |
| `{OUTPUT_SIZE_DESC}` | 见下方 | **可选**；仅 `$OUTPUT_SIZE` 有值时填入 |
| `{TITLE_DESC}` | 见下方 | 有标题时填 |
| `{LABEL_DESC}` | 见下方 | 文字标注（颜色从 style 文件读取） |
| `{NEGATIVE_DESC}` | `styles/{$STYLE}.md` → "绝对不要" + IP 负向约束 | 合并输出 |

---

## 各变量填法

### `{REF_IMAGE}`

**`$IP` = gimi（双参考协议）：**

1. **必传** `reference-character.png` — 锚点色、配饰、比例
2. **必传** `assets/ip/gimi/examples/quirky-sketch/` **1 张**校准图（只校准 IP，不抄场景）：
   - 默认 / 拿不准 → `gimi-writing-rules-checklist.png`
   - 职场 / 久坐 / 痛点 / 环境类 → `gimi-desk-shoulder-fatigue.png`
   - 产品界面 / 弹窗 / 功能讲解 → `gimi-exercise-modal-focus.png`
   - AI 改文件 / 焦虑 / 有图内标题 → `gimi-ai-change-chaos.png`
3. 两张都进上下文后再生图；prompt 开头：

```
Match Gimi to BOTH reference images (character sheet + sketch example): same horse hood, VERTICAL vest with deep-blue + sky-blue + warm-brown/tan stripes (NOT yellow/orange/gold on vest), pink blush, cube pendant, blue skirt, black chunky boots. Use the sketch example for line style only; do not change character identity or vest colors.
```

**`$IP` = none：** 不传角色图；质量不稳时可选 `assets/none/examples/{$STYLE}/` **1 张**风格参考：
- 路径/步骤/循环类 → `none-ai-practice-loop.png`
- 部署/分享链接类 → `none-shareable-portfolio-link.png`
- 情绪/改文件/AI介入类 → `none-ai-change-chaos.png`
- 演示/工具链主线类 → `none-ai-presentation-mainline.png`

**分工**：设定图 + 校准图 → IP；风格词 → 场景线稿与色纪律。禁止只传一张就当 gimi 流程完成。

### `{IP_DESC}`

- Gimi → 读 `ip/gimi/ip.md`「填入 `{IP_DESC}`」段落
- none → 读 `ip/none.md`
- 自定义 IP（v2.0）→ 读 `ip/{name}/ip.md`

### `{IP_STYLE_ADAPT}`

- 读 `ip/{$IP}/ip.md`「填入 `{IP_STYLE_ADAPT}`」— **有则填，无则留空**
- gimi → 背心三色锁色等 IP 专属约束
- none → 留空

> **锚点优先**：外貌/配饰/比例以参考图为准；`{STYLE_ADAPT}` 管线稿质感；`{IP_STYLE_ADAPT}` 管 IP 专属锁色。

### `{COMPOSITION_DESC}`

从 `composition-patterns.md` 构图库选择（或根据核心信息自定义），转英文：

| 构图 | 填入内容 |
|------|---------|
| 单主体居中 | `single centered subject, generous negative space on all sides` |
| 对比并置 | `two contrasting elements side by side, visual tension` |
| 行动中人物 | `character in mid-action, dynamic pose` |
| 物件特写 | `close-up single object, exaggerated scale` |
| 环境暗示 | `tiny figure in vast environment, scale contrast` |
| 图标级单主体 | `ultra-simple visual symbol, large whitespace, room for sparse labels` |
| 隐喻场景 mini | `minimal scene with 3 elements or fewer, one complete metaphor` |
| 序列逻辑 | `2-4 numbered main steps in left-to-right order; failure/rollback nested inside the last step, not as an extra equal station; rollback arrow from broken state to save point or clean file` |
| 信息聚焦 | `one clear visual focus, supporting elements are smaller and only serve the main message` |
| 情绪面孔 | `expressive face close-up, exaggerated emotion` |

### `{MESSAGE_DESC}`

`Reader takeaway: <one sentence describing the key idea this image must communicate>.`

### `{SUBJECT_DESC}`

从 shot-config「物件表意」+「隐喻」填入，包一层英文引导：

```
Draw these specific named objects from the source text: <物件表意>.
Scene layout: <隐喻内容，含信息分工>.
Clear story flow: <故事动线>, connected by arrows or path in reading order.
Each main object has a clear information role (problem source / action entry / result / state).
Allow 4-6 named objects when needed; use size and line weight for hierarchy, not extra color fills.
For sequential diagrams: exactly N numbered main steps if title says N steps;
nest failure/broken state inside the last step (before → action → after), not as a fourth equal station;
rollback arrow must go from broken state or rollback button TO save box or clean file, never back to broken state.
Viewer can name 2-3 concrete objects in 3 seconds.
Do not use magnifying glass, lightbulb, or generic file icons unless listed in source nouns.
Use soft blue as main scene accent on at most 1-4 objects.
Soft orange on at most 2 small highlights only (not object fills).
Show changes with line marks, not red-green color fills.
No colored sticky notes or label background fills.
```

### `{COMPOSITION_DESC}` 补充

序列逻辑构图时，在 `{COMPOSITION_DESC}` 末尾加：`numbered main steps with clear left-to-right story flow; failure state nested inside final step`

### `{RATIO_DESC}`

按 `$RATIO` 填入（**默认 16:9**）：

| `$RATIO` | 填入内容 |
|----------|---------|
| 16:9 | `aspect ratio 16:9, horizontal composition, landscape orientation, wide frame, NOT portrait, NOT square` |
| 3:4 | `aspect ratio 3:4, vertical composition, portrait orientation, tall frame, NOT landscape` |
| 1:1 | `aspect ratio 1:1, square composition, balanced framing` |
| 9:16 | `aspect ratio 9:16, vertical composition, tall mobile frame` |
| 其它 | 按用户给定比例动态生成，写明 `aspect ratio W:H` 及横/竖/方构图方向 |

推荐像素（16:9 → 1920×1080；3:4 → 1080×1440）**仅作参考**，不写入 `{RATIO_DESC}`，除非用户同时指定了 `$OUTPUT_SIZE`。

### `{OUTPUT_SIZE_DESC}`（可选）

**仅当用户明确要求具体像素（`$OUTPUT_SIZE` 有值）时填入，否则整段留空。**

示例（16:9 + 1920×1080）：
```
CRITICAL: target output 1920x1080 pixels, landscape 16:9 frame
```

示例（3:4 + 1080×1440）：
```
CRITICAL: target output 1080x1440 pixels, portrait 3:4 frame
```

### `{TITLE_DESC}`

- 有标题 → `centered handwritten Chinese title at top: "标题内容"`
- 无标题 → 留空

### `{LABEL_DESC}`

- 默认：`sparse handwritten Chinese labels only, 2-5 labels total: "锚点词1", "锚点词2"; exact labels from shot-config; black text and black arrows only; {LABEL_COLOR}; no English except product names`
- `{LABEL_COLOR}` 从 `styles/{$STYLE}.md` → "标注颜色指令" 读取
- 用户明确不要文字时：`no text, no letters, no words, no watermark`

### `{NEGATIVE_DESC}`

从 `styles/{$STYLE}.md`「绝对不要」段落提取，加上 IP 负向约束（如有）。默认附加：

```
avoid equal-weight process diagrams unless the core idea is truly a process;
avoid formal workflow chart or dense tutorial page;
do not optimize for fewer words at the expense of meaning;
no colored sticky notes, no label background fills, no red-green diff color blocks;
soft blue main scene accent; soft orange at most 2 small highlights only; no orange object fills or label backgrounds
```

**`$IP` = gimi 时追加：**

```
NOT yellow/orange/gold stripes on Gimi vest; NOT gray or monochrome vest;
NOT horizontal vest stripes; scene accent colors must NOT tint Gimi clothing;
vest must keep deep-blue + sky-blue + warm-brown/tan vertical stripes from character sheet
```

---

`v1.7` · 2026-07-07 · 新增 `{IP_STYLE_ADAPT}` 变量；IP 专属锁色与 style 分层
