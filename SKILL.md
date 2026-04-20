---
name: readme-generator
description: 为 GitHub 项目生成符合最佳实践的专业 README.md，包含 SEO/GEO 优化标题、16:9 信息图三件套（banner/features/workflow）、结构化作者信息、MIT 许可证、GitHub description 和 Topics 推荐。当用户说「帮我写 README」「生成 README」「README 最佳实践」「README 信息图」「GitHub 项目首页」「readme maker」「readme 生成」「写一个专业的 README」「项目文档」「开源说明」时必须使用此 skill。也适用于 README 已存在但需要重构、补充信息图、添加作者信息、优化排版的场景。
---

# GitHub README Maker

> 零 ASCII 艺术 · 零 Emoji 堆砌 · 排版驱动设计 · 信息图精准插入

从第一性原理出发：README 是项目的门面，是 Google 和 AI 搜索的入口，是开发者决定是否 star/fork 的第一印象。一个好的 README 需要在 15 秒内传达三件事：**这是什么、能解决什么问题、怎么用**。

---

## 设计哲学

**反模式（绝对避免）：**
- ❌ 用 ASCII 艺术字幕开头（`╔══╗ ◇ Project ◇ ╚══╝`）
- ❌ 第一行就是图片（SEO 不友好，爬虫读不到）
- ❌ Emoji 装饰标题（`## 🚀 功能`——降低专业度）
- ❌ 大段文字没有信息图辅助（阅读疲劳）
- ❌ 缺少 GitHub Topics 和 Description（SEO 失分）

**正确模式：**
- `# 项目名` H1 + blockquote 标语 → 立即建立认知
- Badge 行 → 信息密度高，一眼扫描状态
- 16:9 Banner 信息图 → 视觉冲击，替代 ASCII
- Features 信息图 + Workflow 信息图 → 核心价值可视化
- 结构化作者区块 → 表格形式，统一风格

**零 Emoji 原则（README 及信息图均适用）：**

- README 中不用 emoji 装饰标题或作者表格（`:octocat:` `:globe_with_meridians:` 等 shortcode 同样禁止）
- 信息图 HTML 中不用 emoji 作为图标——改用编号（`01` `02`）、SVG 图形或几何色块
- 检查点提示文字中不用 ✅ 📋 ⚙️ 等符号，改用纯文字说明

---

## 总体流程

```
Phase 0    环境检查 (Node.js + Playwright)
Phase 0.5  模式识别 (新建 vs 升级)
Phase 1    项目信息采集 (6 个关键字段)
Phase 1.5  设计风格选择 (npx getdesign list → 提取 Token，默认 claude)
Phase 2    信息图生成 (3 张 HTML → PNG，应用风格 Token)
Phase 3    README 组装 (8 个标准区块)
Phase 4    GitHub 元信息建议 (description + topics)
Phase 5    Git 提交 (可选，用户确认后)
```

---

## Phase 0: 环境检查

在开始前验证：

```bash
node --version   # 要求 >= 18
```

检查 package.json：
```bash
ls package.json 2>/dev/null || npm init -y
```

检查 Playwright：
```bash
node -e "require('playwright')" 2>/dev/null && echo "OK" || (npm install playwright && npx playwright install chromium)
```

> **为什么需要 Playwright？** 信息图以 HTML 方式编写后通过 Playwright 截图为 PNG，这是在没有设计工具的情况下生成高质量信息图的最佳实践。详见 `scripts/gen_infographic.mjs`。

---

## Phase 0.5: 模式识别

在采集信息前，先判断这是哪种场景——**新建**还是**升级**，后续流程会有所不同。

### 判断规则

| 场景 | 判断条件 | 进入流程 |
|------|----------|----------|
| **全新创建** | 用户没有提到现有文件，或目录内无 README.md | Phase 1 → 2 → 3 → 4 → 5（完整流程） |
| **升级现有** | 用户说「我已有 README」「帮我优化/重构」，或当前目录存在 README.md | 先读取现有文件，再执行 **Phase 1 → 2 → 3（差量更新）→ 4 → 5** |

### 升级现有 README 的差量策略

当识别为「升级」场景时：

1. **读取现有 README**，提取已有内容（项目名、描述、安装方式等）
2. **诊断缺失项**，重点检查：
   - 是否缺少 Banner/Features/Workflow 信息图？→ Phase 2 补充生成
   - H1 标题是否以 ASCII 艺术或图片开头？→ Phase 3 重构开头
   - 是否缺少作者区块？→ Phase 3 补充
   - 是否缺少 GitHub 元信息建议？→ Phase 4 补充
3. **保留用户已有的独特内容**（如详细的 API 文档、贡献指南），不要因为升级而删除

> 升级时告诉用户：「我已读取你的现有 README，会保留原有内容并补充/重构以下部分：[列出差异]」，等用户确认后再继续。

### Fallback 场景

| 异常情况 | 处理方式 |
|----------|----------|
| 用户没有 Git 环境 | Phase 5 跳过，提醒用户手动复制文件 |
| 作者信息完全缺失 | Phase 3 使用占位符 `[your-homepage]`，在 README 末尾注释提醒用户填写 |
| Playwright 截图失败 | 检查 Chromium 路径 → 尝试 `npx playwright install chromium` → 仍失败则跳过信息图，Phase 3 保留 `![Banner](assets/banner.png)` 占位（后续可手动替换） |
| 用户只想要文字版（无信息图） | 跳过 Phase 2，Phase 3 中信息图位置改为简洁的功能列表 |

---

## Phase 1: 项目信息采集

向用户询问（或从上下文推断）以下 6 个字段：

| 字段 | 示例 | 说明 |
|------|------|------|
| `project_name` | `Travel Guidebook` | 项目名，首字母大写 |
| `tagline` | `从调研到成书的一站式旅行路书引擎` | 一句话价值主张，≤ 30 字 |
| `problem` | `用 AI 自动生成精排旅行 PDF，替代手动整理攻略` | 解决什么问题 |
| `features` | `并行调研 / 高德地图 MCP / Playwright PDF 导出` | 核心功能，3-6 条 |
| `tech_stack` | `Node.js / Playwright / Claude Code` | 主要技术栈 |
| `author` | 见下方作者结构 | 个人主页/GitHub/Twitter/公众号 |

**作者信息结构：**
```
author:
  homepage: https://example.dev
  github: username
  twitter: @handle
  wechat: 公众号名称（可选）
```

如果用户已在上下文中提供了这些信息，无需重复询问，直接推断并确认。

---

## Phase 1.5: 设计风格选择

> 使用 `getdesign` 工具从 60+ 大厂设计规范中选择风格，生成的信息图将自动应用对应的色彩和排版系统。

### Step 1：列出可用风格

```bash
npx getdesign list
```

> **如果 `npx getdesign list` 执行失败**（命令不存在、网络超时、npm registry 不可达）：跳过 Step 1-3，直接使用下方「claude 风格默认 Token」进入 Step 4。告知用户：「getdesign 工具暂不可用，将使用默认 claude 风格继续。」

展示完整列表后，向用户提示：

```
以上是 60+ 可用的大厂设计风格。

默认风格：claude — Anthropic's AI assistant. Warm terracotta accent, clean editorial layout.

请选择一种风格（直接回车使用默认 claude）：
```

### Step 2：下载风格文件

用户选择后（或按回车使用默认），执行：

```bash
# 将风格下载到临时目录，不污染项目目录
mkdir -p /tmp/getdesign-<style>
cd /tmp/getdesign-<style>
npx getdesign add <style>
# 生成 /tmp/getdesign-<style>/DESIGN.md
```

如果用户选择 `claude`（默认）：
```bash
mkdir -p /tmp/getdesign-claude && cd /tmp/getdesign-claude && npx getdesign add claude
```

### Step 3：从 DESIGN.md 提取设计 Token

读取 `/tmp/getdesign-<style>/DESIGN.md`，按以下规则提取 token：

| Token | 提取位置 | 映射到模板变量 |
|-------|----------|---------------|
| 主色（Accent） | Section 2 "Primary" 中第一个品牌强调色 | `{{PRIMARY_COLOR}}` |
| 背景色 | Section 2 "Surface & Background" page background | 模板 `background-color` / `{{BG_COLOR}}` |
| 标题字体 | Section 3 "Font Family" Headline 字体（无法加载自定义字体时用 fallback） | 模板 `font-family` |
| 暗色文字 | Section 2 "Neutrals & Text" 最深的文字色 | 模板 `color` 主文字 |

**claude 风格默认 Token（用户不选 / getdesign 不可用时直接使用）：**

| Token | 值 | 说明 |
|-------|-----|------|
| `{{PRIMARY_COLOR}}` | `#c96442` | Terracotta Brand — 暖赤陶橙 |
| 背景色 | `#f5f4ed` | Parchment — 羊皮纸暖白 |
| 标题字体 | `Georgia, serif` | Anthropic Serif 的 fallback |
| 主文字色 | `#141413` | Anthropic Near Black — 暖黑 |

> **提取失败时的 fallback：** 在 DESIGN.md 的 `Key Characteristics` 列表中找第一个 hex 颜色值，作为 `{{PRIMARY_COLOR}}`。背景色默认 `#0d1117`（GitHub 深色），字体默认 `Inter`。

### Step 4：更新总体流程中的变量声明

将提取到的 Token 记录为本次生成的变量，供 Phase 2 使用：

```
PRIMARY_COLOR = <从DESIGN.md提取>
BG_COLOR      = <从DESIGN.md提取>
HEADING_FONT  = <从DESIGN.md提取>
TEXT_COLOR    = <从DESIGN.md提取>
STYLE_NAME    = <用户选择的风格名>
```

---

## Phase 2: 信息图生成（核心）

生成 3 张 16:9 HTML 信息图，**截图引擎使用 `scripts/gen_infographic.mjs`**。

### 生成流程（每张图）

**Step 1：从 templates/ 读取模板，替换占位符，写入 /tmp/**

`templates/` 目录下有三个基础模板，包含 `{{PROJECT_NAME}}`、`{{PRIMARY_COLOR}}`、`{{TAGLINE}}` 等占位符。操作方式：

```bash
# 读取 templates/banner.html 内容后，将以下占位符替换为实际值，写入 /tmp/
# {{PROJECT_NAME}}     → 实际项目名（如 "codeflow"）
# {{TAGLINE}}          → 一句话标语
# {{PRIMARY_COLOR}}    → 主色 hex（如 "#0ea5e9"）
# {{CATEGORY}}         → 项目类别（如 "CLI Tool" / "Agent Skill"）
# {{PLATFORM}}         → 运行平台（如 "Node.js" / "Python"）
# {{LANGUAGE}}         → 主要语言（如 "TypeScript"）
# {{VERSION_INFO}}     → 版本信息（如 "v1.0 · 2026"）
# {{TECH_CARDS}}       → banner 右侧技术卡片（见下方 HTML 结构）
# {{FEATURE_CARDS}}    → features 功能卡片（见下方 HTML 结构）
# {{FEATURE_COUNT}}    → 功能数量数字（如 "6"）
# {{PIPELINE_STAGES}}  → workflow 流程阶段（见下方 HTML 结构）
# {{STAGE_COUNT}}      → 阶段数量（如 "5"）
```

**卡片 HTML 结构参考（从 templates/ 提取，直接复制并填入内容）：**

`{{TECH_CARDS}}`（banner.html 右侧，每条技术一个）：
```html
<div class="tech-card">
  <div class="tech-icon">01</div>
  <div class="tech-info">
    <div class="tech-name">Node.js</div>
    <div class="tech-desc">Runtime environment</div>
  </div>
</div>
<!-- 重复 .tech-card，编号递增：02, 03... 建议 3-5 条 -->
```

`{{FEATURE_CARDS}}`（features.html 网格，每个功能一个，需设置 `--accent`）：
```html
<div class="card" style="--accent: {{PRIMARY_COLOR}}">
  <div class="card-icon">01</div>
  <div class="card-tag">Core</div>
  <div class="card-title">并行调研</div>
  <div class="card-desc">多个 Agent 同时搜索，汇总后去重排序</div>
</div>
<!-- 重复 .card，编号递增，card-tag 可选：Core / Integration / Output 等 -->
```

`{{PIPELINE_STAGES}}`（workflow.html 流水线，.stage 和 .arrow 交替）：
```html
<div class="stage">
  <div class="stage-num">STEP 01</div>
  <div class="stage-title">信息采集</div>
  <ul class="stage-items">
    <li>读取项目源码</li>
    <li>提取 package.json</li>
  </ul>
  <span class="stage-badge">Input</span>
</div>
<div class="arrow"></div>
<!-- 重复 .stage + .arrow，最后一个 .stage 后不加 .arrow -->
```

**Step 2：截图**

```bash
node scripts/gen_infographic.mjs /tmp/readme-banner.html assets/banner.png 1920 1080
node scripts/gen_infographic.mjs /tmp/readme-features.html assets/features.png 1920 1080
node scripts/gen_infographic.mjs /tmp/readme-workflow.html assets/workflow.png 1920 1080
```

**Step 3：清理临时文件**

```bash
rm /tmp/readme-banner.html /tmp/readme-features.html /tmp/readme-workflow.html
```

> ⚠️ **关键：** HTML 临时文件只写 `/tmp/`，截图 PNG 保存到 `assets/`。不要把 HTML 提交到 git（`.gitignore` 已包含 `*.html` 排除规则）。

### 主色选择指南

**优先使用 Phase 1.5 提取的 `PRIMARY_COLOR`**。如果用户跳过了风格选择，则按下表根据项目性质手动选择：

| 项目类型 | 推荐主色 | Hex |
|----------|----------|-----|
| 开发工具 / CLI | 科技蓝 | `#0ea5e9` |
| AI / Agent | 紫色 | `#a78bfa` |
| 出行 / 生活 | 琥珀金 | `#f59e0b` |
| 效率 / 自动化 | 翠绿 | `#10b981` |
| 设计 / 创意 | 玫瑰 | `#f43f5e` |
| 数据 / 分析 | 青色 | `#06b6d4` |

### 设计系统（保持一致）

参考 `references/design-system.md`，默认采用：
- **背景色**：`#0d1117`（GitHub 深色）
- **字体**：`Inter`（西文）+ `Noto Sans SC`（中文），均通过 Google Fonts CDN 加载
- **圆角**：`12px`，卡片边框：`1px solid #21262d`

> 三张图的主色 `{{PRIMARY_COLOR}}` 必须填写相同的值——视觉一致性是专业度的核心。

### Phase 2 检查点（生成后必须暂停）

三张 PNG 生成完毕后，向用户汇报：

```
信息图生成完成：
  - assets/banner.png   (1920×1080, ~500KB)
  - assets/features.png (1920×1080, ~120KB)
  - assets/workflow.png (1920×1080, ~100KB)

请在你的文件管理器中预览这三张图，确认：
1. 项目名/标语是否正确？
2. 配色是否符合项目风格？
3. 功能卡片的描述是否准确？

确认后输入「继续」进入 README 组装。如需调整，告诉我哪里需要修改。
```

不要在用户确认之前进入 Phase 3。

---

## Phase 3: README 组装

### 标准结构（8 个区块，顺序不可随意调换）

```markdown
<div align="center">

# 项目名

**一句话标语**

<img src="assets/banner.png" alt="[项目名] — [功能描述]" width="100%">

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![...其他 badge...]

</div>

---

## 这是什么

[2-3 句话，说清楚：这是什么、解决什么问题、最大亮点]

```
输入：[示例输入]
输出：[示例输出]
```

---

## 核心特性

<img src="assets/features.png" alt="[alt 文本]" width="100%">

[可选：补充 1-2 条文字说明]

---

## 工作流程

<img src="assets/workflow.png" alt="[alt 文本]" width="100%">

---

## 安装

[依赖 + 安装命令]

---

## 快速上手

[最简示例，让用户 5 分钟内跑起来]

---

## 许可证

[MIT](./LICENSE) — 自由使用、修改、分发。

---

## 关于作者

| | |
|:---|:---|
| 个人主页 | [domain.dev](https://domain.dev) |
| GitHub | [username](https://github.com/username) |
| Twitter | [@handle](https://x.com/handle) |
| 公众号 | 微信搜「公众号名」 |
```

### 区块规则

1. **H1 必须在 `<div align="center">` 内**，但 H2 及以下在外面（便于 anchor 链接）
2. **Banner 紧跟 H1**，不超过 3 行文字就出现图
3. **Badge 行**放在 banner 下方，不超过 5 个（保持整洁）
4. **Features 和 Workflow 用纯图片**，不需要再重复文字列表
5. **关于作者用表格**，不用列表，不用 blockquote
6. **最后一定有许可证**，默认 MIT

---

## Phase 4: GitHub 元信息建议

### Repository Description（≤ 160 字符）

格式：`[一句话功能] — [核心技术关键词]`

示例：
```
AI Agent Skill for generating beautifully typeset travel guidebook PDFs with parallel research and Playwright export
```

规则：
- 英文优先（GitHub 国际受众）
- 包含 2-3 个核心技术关键词（供搜索引擎索引）
- 不用 emoji，不用感叹号
- 说「做什么」不说「很厉害」

### Topics（7-10 个）

推荐策略：
1. **技术类**（语言/框架）：如 `nodejs`, `typescript`, `react`
2. **领域类**（应用场景）：如 `travel`, `pdf-generation`, `ai-agent`
3. **工具类**（用到的工具）：如 `playwright`, `claude`, `mcp`
4. **受众类**（目标用户）：如 `developer-tools`, `automation`

示例（travel-guidebook）：
```
agent-skill  claude-code  travel  pdf-generation  playwright
nodejs  mcp  copilot-cli  ai-agent  itinerary
```

### Phase 4 检查点（给出建议后暂停）

生成元信息建议后，向用户展示并等待确认：

```
GitHub 元信息建议（请在 repo Settings 中填写）：

Description:
  [生成的英文描述，≤160字符]

Topics（在 repo 主页点击齿轮图标添加）:
  [tag1]  [tag2]  [tag3]  ...

以上建议是否合适？如需调整描述或 Topics，直接告诉我。
确认后继续 → Git 提交（Phase 5）。
```

---

## Phase 5: Git 提交（可选）

用户确认 README 满意后，询问是否初始化 git 并提交：

```bash
cd [项目目录]
git init
git add README.md assets/ LICENSE
git commit -m "docs: add README with infographics and author section

- Banner, features, workflow 三张 16:9 信息图
- SEO 优化标题和描述
- 结构化作者区块
- MIT 许可证"
```

> 提醒用户：将 HTML 模板文件加入 `.gitignore`（如 `*.html` 临时文件），只提交 PNG 截图结果。

---

## 常见问题处理

**信息图字体不显示？**
HTML 模板中使用了 Google Fonts CDN，需要联网。截图时 Playwright 会自动等待字体加载。如果断网，在 `<style>` 中换成系统字体：`font-family: -apple-system, 'PingFang SC', sans-serif`。

**截图比例不对？**
`gen_infographic.mjs` 的 `--width` 和 `--height` 参数控制视口大小。16:9 = 1920×1080（标准）或 1280×720（轻量）。

**Playwright 找不到 Chromium？**
```bash
npx playwright install chromium
# 或指定路径：
PLAYWRIGHT_BROWSERS_PATH=~/.cache/ms-playwright npx playwright install chromium
```

**如何适配暗色/亮色主题？**
参见 `references/design-system.md` 的主题切换方案。Banner 默认深色背景，如项目风格偏向亮色（如教育类），可切换为 `#fafafa` 背景 + 深色文字。
