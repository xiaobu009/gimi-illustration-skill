---
name: gimi-illustration
description: >
  Quirky hand-drawn illustrations from Chinese article text (配图 / illustration),
  OR custom IP enrollment (自定义 IP / 录入 IP / 上传形象 / 新建 IP).
  When the user is enrolling a custom IP: do NOT generate a poster or illustration first —
  load references/ip/_template.md and run Step 0 (plan confirm before freeform image gen).
  Use for 配图, 给文章配图, 帮我配图, illustration, 自定义 IP, 录入 IP, 上传形象, 新建 IP.
---

# gimi-illustration · 配图 Skill

> **Public Release 2.0** · 内部迭代 4.7  
> 换窗口：有 `docs/roadmap.md` 时先读（tenp 内部）；公开仓以本文件 + `references/ip/_template.md` 为准

## ⚠ 路由分流（先读 · 防 ImageGen 抢跑）

触发含 **`自定义 IP` / `录入 IP` / `上传形象` / `新建 IP`**（可附图）时：

1. **立刻**加载 `references/ip/_template.md`，走 **Step 0**；**不要**先当「配图 / 海报」生图  
2. **本回合禁止**调用任何生图工具（`image_gen` / `GenerateImage` / ImageGen 等），直到用户已回答「按这个形象方案录入吗？」  
   - **半身/全身相同**：首轮**只**出形象方案 + 收尾问；**零例外**  
   - 确认后：半身才允许 0.2b 全身设定稿；再 0.4 校准  
3. **禁止**把录入理解成「生成一张 IP 主题海报 / 原创插画海报」  
4. 细则与话术以 `_template` 为准；本段与之冲突时，**以本段路由为准**（先拦抢跑）

触发仅为 **`配图` / `illustration`** 等、且非录入 IP → 走下方 Quick Mode / Step 1–5。

---

输入文章内容，生成怪诞手绘风格配图。单风格 `quirky-sketch`，IP 为 `gimi` / `none` / 自定义实例，默认 16:9。

---

## 触发词

配图：`配图` / `给文章配图` / `帮我配图` / `illustration`  
自定义 IP：`自定义 IP` / `录入 IP` / `上传形象` / `新建 IP`

---

## 参考文件（按需加载）

| 文件 | 何时加载 |
|------|---------|
| `docs/roadmap.md` | 换窗口 / 要改规则时 |
| `references/styles/quirky-sketch.md` | 生图时 |
| `references/composition-patterns.md` | 出策略时（**非** Step 0） |
| `references/shot-config.md` | 出策略时（**非** Step 0） |
| `references/prompt-template.md` | 组装 prompt 时 |
| `references/qa-checklist.md` | 质检时 |
| `references/ip/_template.md` | **自定义 IP / Step 0（优先于生图）** |
| `references/ip/gimi/ip.md` | `$IP` = gimi |
| `references/ip/none.md` | `$IP` = none |
| `references/ip/{id}/ip.md` | `$IP` 为已存在的自定义 id |

---

## 生图能力

调用当前 AI 软件**内置的生图工具**（如 Codex `@Image Gen`、Cursor `GenerateImage` 等）。Skill 负责策略、prompt 组装、参考图附件、QA；生图由平台工具执行。接口语义：文本 prompt + 可选图片附件。

**Step 0 录入中：** 遵守文首「路由分流」；未满足允许条件前**不得**调用生图工具。

平台适配优先级：

1. 支持显式图片附件的平台：把参考图作为 image reference 附件传入。
2. Codex `image_gen`：按该 IP 的 `ref_mode` 传入参考图（见 `_template.md` / 对应 `ip.md`）；再 `image_gen`
3. 不支持图片输入且无法把图片放入上下文时，暂停说明原因；只有用户接受降级时，才使用 `ip.md` 文字锚点描述。

---

## 核心变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `$STYLE` | `quirky-sketch` | v1.0 固定，不切换 |
| `$RATIO` | `16:9` | 宽高比；用户可指定任意比例 |
| `$IP` | `gimi` | `gimi` / `none` / 其他：若存在 `references/ip/{id}/`（非 `_template`）则加载该实例 |
| `$TITLE` | 无 | 用户要求时顶部手写标题 |
| `$COUNT` | 自动推断 | 配图张数 |
| `$OUTPUT_SIZE` | 无 | **可选**；仅用户明确要求具体像素时记录 |

**推荐像素（非强制）：** 16:9 → 建议 1920×1080；3:4 → 建议 1080×1440。

---

## 工作流

### Quick Mode

用户已给正文且变量可推断 → 不多问，直接生图。以下情况才确认：用户说「先别生图」、变量冲突、新 IP 尚无 `ip.md`（先走 Step 0）。  
**Step 0 进行中：关闭 Quick Mode**（不得跳过确认话术）。  
**录入触发词出现时：不适用 Quick Mode 生图。**

### Step 0 · 自定义 IP（按需）

用户要新建/录入 IP 时：加载 `references/ip/_template.md`，严格按其中 Step 0 填槽（用户只需附图 +「录入 IP」，不必写 Step 编号）。  
**双层：** 协议走 0.1–0.5；对用户只用「用户层话术」（见 `_template`），称风格为「怪诞手绘」，不暴露 Step 编号。  

**硬门禁（0.5 完成前）——与文首路由分流一致：**

1. **入口（半身/全身相同）**：首轮只出形象方案 +「按这个形象方案录入吗？」；**一律禁生图**；禁海报式成稿  
2. **禁止配图抢跑**：不得进 Step 1–5；不得写 `shot-config.md`  
3. **出图后必须提问**：同轮追加收尾问；**图 ≠ 闭合**（仅 0.2 确认之后才允许生图）  
4. 半身：确认后 0.2b 须头到脚；落盘 `reference-character.png` 一律 full  
5. 0.4 标准风格样板镜；「像吗？」未闭合前不得进配图  

`examples/{$STYLE}/` 空且要用该风格配图时 → lazy 补 0.4（细则 v3.0，见 `_template`）。  
完成后才进入配图；配图时 `$IP={id}`。

### Step 1 · 意图确认

记录：`$RATIO` · `$IP` · `$TITLE` · `$COUNT` ·（可选）`$OUTPUT_SIZE`

Quick Mode 推断：竖版/小红书 → 3:4；否则 → 16:9。

### Step 2 · 消化内容 + 出策略

> 策略细节以 `composition-patterns.md` 为准；落盘格式以 `shot-config.md` 为准。

1. 加载上述两文件
2. 按 `composition-patterns.md` 的 **Step 2 决策流程** 执行（二轴 → 创意生成法六步 → 构图库 → Shot Config）
3. Quick Mode：默认直接落盘并进入 Step 3；用户说「先看策略」时先展示摘要

### Step 3 · 组装 Prompt + 生图

1. 加载 `styles/{$STYLE}.md`、`prompt-template.md`、对应 `ip/*.md`
2. 读对应 `ip.md` 的 `ref_mode`：
   - `dual` → 设定图 + `examples/{$STYLE}/` 中 1 张校准图（缺文件则当次降级 single 并提示）
   - `single` → 仅设定图
   - `$IP=none` → 仍按 none 规则（无角色设定图）
   - 参考图未进上下文，不得声称已执行有 IP 流程
3. 填充变量槽，调用生图工具（`IP_DESC` 在 `STYLE_DNA` 之前，见 `prompt-template.md`）

### Step 4 · QA 质检

加载 `qa-checklist.md`。`$OUTPUT_SIZE` 有值时 → 等比缩放 + 白底留白，禁止裁切。不通过 → 重试，最多 2 次。

### Step 5 · 交付

```
outputs/{YYYYMMDD}-{slug}/
  shot-config.md      ← 配图策略 + 插入位置（与图片同时交付）
  01-{slug}.png
  02-{slug}.png       ← 多张时继续编号
```

---

## 版本

Public `2.0` · 内部 `4.7` · 2026-07-17
