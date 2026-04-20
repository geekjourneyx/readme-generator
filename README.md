<div align="center">

# GitHub README Generator

**从项目信息到专业 README 的一站式生成 Skill**

<img src="assets/banner.png" alt="GitHub README Maker — AI Agent Skill for generating professional README with SEO optimization and 16:9 infographics" width="100%">

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Skill](https://img.shields.io/badge/Agent_Skill-Copilot_CLI-0ea5e9.svg)](#安装)
[![Node](https://img.shields.io/badge/Node.js-≥18-6b7c5e.svg)](https://nodejs.org)

</div>

---

## 这是什么

一个 AI Agent Skill，输入项目的基本信息，自动生成一份符合最佳实践的专业 README.md。

```
输入：项目名 + 标语 + 核心功能 + 作者信息
输出：README.md + 3 张 16:9 信息图 + LICENSE + GitHub 元信息建议
```

不是模板填充——而是从第一性原理出发，遵循 SEO / GEO 规范，生成**让用户在 15 秒内理解你项目价值**的专业文档。

---

## 解决什么问题

| 反模式 | 本 Skill 的做法 |
|--------|----------------|
| ASCII 艺术字幕开头 | `# H1 标题` + blockquote 标语，爬虫友好 |
| 大段文字无视觉辅助 | 3 张 16:9 信息图精准插入 |
| 缺失 GitHub Description/Topics | 提供 SEO 优化的元信息建议 |
| Emoji 装饰标题（降低专业度） | 零 Emoji 堆砌，排版驱动设计 |
| 作者信息格式混乱 | 标准化表格，纯文字标签，零 emoji |

---

## 核心特性

<img src="assets/features.png" alt="Core Features — SEO optimization, 16:9 infographics, typography-driven design, structured sections, author block, MIT license" width="100%">

---

## 工作流程

<img src="assets/workflow.png" alt="5-phase pipeline: environment check, info capture, infographic generation, README assembly, GitHub metadata" width="100%">

---

## 安装

### 前置依赖

```bash
# Node.js >= 18
node -v

# Playwright + Chromium（信息图截图引擎）
npm install playwright
npx playwright install chromium
```

### 安装 Skill

```bash
npx skills add https://github.com/geekjourneyx/readme-generator
```

> 安装后在对话中说「帮我写 README」即可触发。支持 Copilot CLI 和 Claude Code。

---

## 快速上手

安装完成后，在 Copilot CLI / Claude Code 中对话：

```
> 帮我为这个项目写一个专业的 README

> 生成 README，项目是一个 Node.js PDF 生成工具，MIT 协议

> README 最佳实践，帮我重构现有文档
```

Skill 会自动引导你完成 5 个阶段：环境检查 → 信息采集 → 信息图生成 → README 组装 → GitHub 元信息建议。

---

## 生成内容

```
project/
├── README.md                    # 符合最佳实践的专业文档
├── LICENSE                      # MIT 许可证
└── assets/
    ├── banner.png               # 16:9 Hero 信息图
    ├── features.png             # 核心特性可视化
    └── workflow.png             # 工作流程图
```

---

## 项目结构

```
readme-generator/
├── SKILL.md                     # Skill 定义（Agent 读取）
├── scripts/
│   └── gen_infographic.mjs      # Playwright 截图工具
├── templates/
│   ├── banner.html              # Banner 模板（含占位变量）
│   ├── features.html            # Features 模板
│   └── workflow.html            # Workflow 模板
└── references/
    ├── design-system.md         # 色彩/字体/规格规范
    └── seo-best-practices.md    # GitHub SEO / GEO 指南
```

---

## 设计原则

1. **第一性原理**：README 是项目门面和 AI 搜索入口，15 秒传达三件事
2. **排版即设计**：不依赖 AI 图片，通过 HTML + Playwright 生成专业信息图
3. **SEO 全链路**：H1 标题、Description、Topics 三位一体优化
4. **标准先行**：8 个黄金章节按最佳顺序，不可随意调换
5. **以身作则**：本 README 本身就是用这个 Skill 生成的

---

## 许可证

[MIT](./LICENSE) — 自由使用、修改、分发。

---

## 关于作者

| | |
|:---|:---|
| 个人主页 | [geekjourney.dev](https://geekjourney.dev) |
| GitHub | [geekjourneyx](https://github.com/geekjourneyx) |
| Twitter | [@seekjourney](https://x.com/seekjourney) |
| 公众号 | 微信搜「极客杰尼」 |

---

<div align="center">

*好的 README 不是写出来的，而是设计出来的。*

</div>
