# QA Checklist

## 必过项（通用，所有风格 / IP）

**输出规格**

- **比例正确**：画面横竖与 `$RATIO` 一致
- **像素尺寸**（仅 `$OUTPUT_SIZE` 有值时）：精确等于目标像素；禁止裁切
- 文字策略：默认只出现 shot-config 指定的短中文标注；不要文字时无任何可读文字

**构图基础**

- 主题相关、构图清晰、视线有落点

**内容与动线** — 3 秒内能说出具体物件名，并跟完「发生了什么」

- **表意明确**：能说出 2–3 个**具体物件名**（优先原文名词，非泛化图标）
- **物件表意**：画面物件能对应 shot-config「物件表意」列表
- **信息分工**：能说出 2–3 个主物件各自在讲什么（问题/动作/结果）
- **故事动线**：有箭头/路径/顺序，不是物件散落

**IP / 风格**

- **有 IP**（`$IP` ≠ none）：双参考已传入（若该 IP 协议要求）；锚点逐项可见 — **读 `ip/{$IP}/ip.md`「锚点」+「QA·走形失败信号」**；服装配色以 IP 文件为准；比例与设定图一致
- **无 IP**（`$IP=none`）：有明确视觉载体；**读 `ip/none.md`** 简笔角色规则
- 留白与色彩：**读 `styles/{$STYLE}.md`「QA 检查项」** 逐条执行

## 风格专属 QA

> 加载 `styles/{$STYLE}.md` →「QA 检查项」+「迭代 palette」（颜色失控时）

## IP 专属失败信号

> 加载 `ip/{$IP}/ip.md`；gimi 走形项见该文件锚点与负向约束。

- **none 出现 Gimi 品牌特征** → 改 prompt；改用 `ip/none.md` 简笔通用角色
- **gimi 背心黄/橙条纹或缺暖棕色** → 读 `ip/gimi/ip.md`「背心配色」+「迭代 palette（背心配色走形时）」后重试

## 失败信号（通用，宪法层）

**视觉体裁**

- 未授权文字 / 乱码 / 角色解剖错误 / 太写实 / 过满 / 太像 PPT

**表意与分工**

- 说不出具体物件名 / 只有泛化「文件页」→ 回 shot-config 补「物件表意」
- **说不清分工** → 隐喻补「信息分工」一行
- **太稀**（具名物件少于物件表意）→ 加具名物件 + 线稿层次，不加色
- **盲套象征物**（无原文语义却出现放大镜/灯泡/站牌）→ 回创意生成法 Step 3–4

**故事动线（序列）**

- **序列步数错位**（标题 N 步但画面 N+1 个等权主站）→ 回 shot-config，异常收入最后一环内部
- **回退箭头反了**（指向坏状态、或跳过坏状态直连终点）→ 改故事动线后重试
- **无故事动线**（物件散落、看不出顺序）→ 回 shot-config 补「故事动线」

**格式与指针**

- **风格 QA 未过**（色纪律、标签、背景等）→ 读 `styles/{$STYLE}.md` QA + 迭代 palette 后重试
- **IP QA 未过**（锚点缺失、走形）→ 读 `ip/{$IP}/ip.md` 后重试
- 标签超过 5 个 / 比例不对

## 迭代方法

**构图与密度**

- **太满** → 减元素，加强留白（或读 style 留白指令）
- **太稀/太泛** → `add more named objects from source list; hierarchy via size and line weight, not more color`

**故事动线（序列）**

- **无动线** → `clear story flow with arrows connecting objects in reading order`
- **序列箭头反了** → `rollback arrow from broken state to save box or clean file, never pointing back to broken state`
- **步数多了** → `exactly N numbered main steps; nest failure state inside the last step, not as a fourth equal station`
- **序列节点/箭头错** → 对照 `composition-patterns`「序列构图语法」改 shot-config 后重试

**风格**

- **颜色 / 线条 / 质感失控** → **读 `styles/{$STYLE}.md`「迭代 palette」**（不写死全局 prompt）
- **gimi 背心配色走形** → **读 `ip/gimi/ip.md`「迭代 palette（背心配色走形时）」**

**收束**

- **重试 2 次仍不过** → 告知原因；接受 / 简化构图

## 可选 · 尺寸后处理（仅 `$OUTPUT_SIZE` 有值时）

只缩放，不裁切；居中贴白底画布至目标像素。禁止 `sips -c` 中心裁切。

## 交付判断

好图：有点怪 → **1 秒懂意思** → **3 秒能点名物件并说清分工** → **能跟完故事动线**。

颜色花、标签带底、看不出顺序或分工，不合格。

---

`v1.9` · 2026-07-07 · IP 走形检查归 ip 文件；qa 不写死 gimi 背心到通用项
