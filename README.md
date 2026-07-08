# gimi-illustration

> 输入中文文章内容 → 输出怪诞手绘风格配图。开源 AI Agent Skill。
>
> Skill 代码采用 [MIT License](LICENSE)；Gimi 角色形象见 [IP-NOTICE.md](IP-NOTICE.md)。

把文章里的观点、流程和隐喻，变成一张张可读的手绘配图。默认 **16:9 横版**，支持 **Gimi IP** 或 **无角色** 两种模式。

---

## 适合谁用

| 场景 | 说明 |
|------|------|
| 图文创作者 | 小红书 / 公众号 / 博客，给文章穿插配图 |
| 职场写作者 | 汇报、方案、文档，用图提升可读性 |
| 口播 / 演讲视觉稿 | 每张图独立传达一个观点 |

---

## 快速开始

### 1. 安装

**Codex（推荐，主要测试环境）**

```bash
git clone https://github.com/GiMi-Xiaomi/gimi-illustration-skill.git
cd gimi-illustration-skill
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R . "${CODEX_HOME:-$HOME/.codex}/skills/gimi-illustration"
```

**Cursor（同样可用）**

```bash
mkdir -p ~/.cursor/skills
ln -s "$(pwd)" ~/.cursor/skills/gimi-illustration
```

或在项目内放置：`.cursor/skills/gimi-illustration/`

### 2. 使用

在对话中发送触发词 + 文章正文：

```
配图

[粘贴你的文章正文]
```

**触发词：** `配图` / `给文章配图` / `帮我配图` / `illustration`

AI 会按 `SKILL.md` 工作流自动完成：理解正文 → 规划配图策略 → 组装 prompt → 调用平台生图 → QA 质检 → 保存到 `outputs/`。

### 3. 常用选项（可在正文中说明）

| 选项 | 默认 | 说明 |
|------|------|------|
| 比例 | `16:9` | 可说「3:4 竖版」「小红书」等 |
| IP | `gimi` | 马帽女孩；说「不要人物」「纯物件」→ `none` |
| 张数 | 自动推断 | 可说「帮我配 3 张」 |
| 标题 | 无 | 可说「顶部加手写标题」 |

---

## 输出结构

```
outputs/{YYYYMMDD}-{slug}/
  shot-config.md    # 配图策略（与图片同时交付）
  01-{slug}.png
  02-{slug}.png     # 多张时继续编号
```

---

## 目录说明

| 路径 | 作用 |
|------|------|
| `SKILL.md` | Skill 主入口与工作流 |
| `references/` | 风格、构图、prompt、QA、IP 协议 |
| `assets/ip/gimi/` | Gimi 设定图与校准案例（`$IP=gimi` 时必用） |
| `assets/none/examples/` | 无角色模式正向案例 |

---

## 生图环境

Skill 负责策略与 prompt；**生图由当前 AI 平台的内置工具执行**：

| 平台 | 生图工具 |
|------|----------|
| **Codex** | `@Image Gen` / `image_gen`（推荐；Gimi IP 双参考图在此验证） |
| **Cursor** | `GenerateImage` |

Gimi IP 模式需将设定图 + 校准图放入上下文后再生图，详见 `references/ip/gimi/ip.md`。

---

## License

- **Skill 代码与文档结构：** [MIT License](LICENSE)
- **Gimi 角色形象（马帽女孩）：** 见 [IP-NOTICE.md](IP-NOTICE.md)，**不包含在 MIT 授权范围内**

---

**维护者：** [@GiMi-Xiaomi](https://github.com/GiMi-Xiaomi)
