# README 设计系统

## 色彩主题

三套预置主题，根据项目性质选择：

### Dark Tech（默认，科技/开发工具类）
```css
--bg-primary:   #0d1117   /* GitHub 深色底 */
--bg-secondary: #161b22   /* 卡片底色 */
--border:       #21262d   /* 分割线 */
--text-primary: #e6edf3   /* 主文字 */
--text-muted:   #8b949e   /* 辅助文字 */
--accent:       #0ea5e9   /* 主色（科技蓝）*/
```

### Warm Editorial（出行/生活方式类）
```css
--bg-primary:   #1a1410   /* 暖棕深色 */
--bg-secondary: #241e18   /* 卡片底色 */
--border:       #3a3028   /* 分割线 */
--text-primary: #f0e6d3   /* 暖白文字 */
--text-muted:   #a09080   /* 辅助文字 */
--accent:       #d4a017   /* 主色（琥珀金）*/
```

### Nature Green（工具/效率类）
```css
--bg-primary:   #0f1a0f   /* 深绿底 */
--bg-secondary: #162016   /* 卡片底色 */
--border:       #2a3a2a   /* 分割线 */
--text-primary: #e8f5e8   /* 白绿文字 */
--text-muted:   #7a9a7a   /* 辅助文字 */
--accent:       #10b981   /* 主色（翠绿）*/
```

## 字体层级

```
H1 项目名：88px / 800 weight / -2px letter-spacing
H2 章节标题：40px / 800 weight / -1px letter-spacing
卡片标题：22px / 700 weight
正文：15-18px / 400-600 weight
标签/角标：11-13px / 600 weight / uppercase
```

## 16:9 信息图规格

| 图片 | 尺寸 | 用途 |
|------|------|------|
| banner.png | 1920×1080 | README 顶部 hero |
| features.png | 1920×1080 | 功能特性展示 |
| workflow.png | 1920×1080 | 工作流程图 |

所有图片存放在 `assets/` 目录，在 README 中以 `width="100%"` 嵌入。

## Badge 规范

```markdown
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)]()
[![Node](https://img.shields.io/badge/Node.js-≥18-6b7c5e.svg)](https://nodejs.org)
[![Stars](https://img.shields.io/github/stars/USER/REPO)](https://github.com/USER/REPO)
```

颜色建议：
- License：`#3b82f6`（蓝）
- Version：`#10b981`（绿）
- Platform/Runtime：`#6b7280`（灰）
- Status：`#f59e0b`（橙，beta）/ `#10b981`（绿，stable）

最多 5 个 badge，保持视觉整洁。

## 作者区块模板

```markdown
## 关于作者

| | |
|:---|:---|
| 个人主页 | [domain.dev](https://domain.dev) |
| GitHub | [username](https://github.com/username) |
| Twitter | [@handle](https://x.com/handle) |
| 公众号 | 微信搜「公众号名」 |
```

纯文字标签，不使用 emoji shortcode（与零 Emoji 原则一致）。
