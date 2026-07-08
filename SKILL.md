---
name: gimi-illustration
description: Generate quirky hand-drawn style illustrations from Chinese article text. Use when the user invokes 配图, 给文章配图, 帮我配图, illustration, or similar.
---

# gimi-illustration · 配图 Skill

> **Release v1.1** · 怪诞手绘单风格，IP：`gimi` / `none`，默认 16:9。

输入文章内容，生成怪诞手绘风格配图。单风格 `quirky-sketch`，IP 为 `gimi` / `none`，默认 16:9。

---

## 触发词

`配图` / `给文章配图` / `帮我配图` / `illustration`

---

## 参考文件（按需加载）

| 文件 | 何时加载 |
|------|---------|
| `references/styles/quirky-sketch.md` | 生图时 |
| `references/composition-patterns.md` | 出策略时 |
| `references/shot-config.md` | 出策略时（Shot List 模板） |
| `references/prompt-template.md` | 组装 prompt 时 |
| `references/qa-checklist.md` | 质检时 |
| `references/ip/gimi/ip.md` | `$IP` = gimi |
| `references/ip/none.md` | `$IP` = none |

---

## 生图能力

调用当前 AI 软件**内置的生图工具**（如 Codex `@Image Gen`、Cursor `GenerateImage` 等）。Skill 负责策略、prompt 组装、参考图附件、QA；生图由平台工具执行。接口语义：文本 prompt + 可选图片附件。

平台适配优先级：

1. 支持显式图片附件的平台：把参考图作为 image reference 附件传入。
2. Codex `image_gen`：先把 **设定图 + 1 张校准图**（见 `ip/gimi/ip.md` 双参考协议）展示进上下文；再 `image_gen`
3. 不支持图片输入且无法把图片放入上下文时，暂停说明原因；只有用户接受降级时，才使用 `ip.md` 文字锚点描述。

---

## 核心变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `$STYLE` | `quirky-sketch` | v1.0 固定，不切换 |
| `$RATIO` | `16:9` | 宽高比；用户可指定任意比例 |
| `$IP` | `gimi` | `gimi` / `none`（v1.0） |
| `$TITLE` | 无 | 用户要求时顶部手写标题 |
| `$COUNT` | 自动推断 | 配图张数 |
| `$OUTPUT_SIZE` | 无 | **可选**；仅用户明确要求具体像素时记录 |

**推荐像素（非强制）：** 16:9 → 建议 1920×1080；3:4 → 建议 1080×1440。

---

## 工作流

### Quick Mode

用户已给正文且变量可推断 → 不多问，直接生图。以下情况才确认：用户说「先别生图」、变量冲突。

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
2. `$IP` = gimi → **双参考**：`reference-character.png` + `examples/quirky-sketch/` 按主题选 1 张（见 `ip.md`）
   - 两张图都进上下文后再生图
   - 若参考图未进上下文，不得声称已执行有 IP 流程
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

`v1.1` · 2026-07-08
