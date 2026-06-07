# Taste Skill — Code Wiki

> **Anti-Slop Frontend Framework for AI Agents**
> 版本: v2 (experimental) | 许可证: MIT | 作者: Leonxlnx
> 仓库: https://github.com/Leonxlnx/taste-skill | 官网: https://tasteskill.dev

---

## 目录

1. [项目概述](#1-项目概述)
2. [项目整体架构](#2-项目整体架构)
3. [目录结构](#3-目录结构)
4. [核心机制：三旋钮系统](#4-核心机制三旋钮系统)
5. [技能模块详解](#5-技能模块详解)
   - [5.1 代码实现类技能](#51-代码实现类技能)
   - [5.2 图片生成类技能](#52-图片生成类技能)
6. [关键类与函数说明](#6-关键类与函数说明)
7. [辅助工具与脚本](#7-辅助工具与脚本)
8. [研究模块](#8-研究模块)
9. [项目运行方式](#9-项目运行方式)
10. [依赖关系图](#10-依赖关系图)
11. [版本历史](#11-版本历史)
12. [设计系统与参考附录](#12-设计系统与参考附录)

---

## 1. 项目概述

Taste Skill 是一个**可移植的 AI Agent 技能集合**，旨在升级 AI 生成的前端界面质量。它通过一组声明式的 `SKILL.md` 文件，为 AI 编程助手（如 Cursor、Claude Code、Codex、Copilot）提供设计指导，使其输出的 UI 不再具有"模板感"（anti-slop）。

### 核心理念

- **Anti-Slop**: 主动对抗 AI 默认生成的通用、模板化 UI 模式
- **Dial-Driven**: 通过三个可调旋钮（VARIANCE / MOTION / DENSITY）控制设计风格
- **Portable**: 每个技能是独立的 `SKILL.md` 文件，可跨平台、跨 Agent 使用
- **Framework-Agnostic**: 规则针对设计意图，不绑定特定框架 API

### 支持的 AI Agent

- GitHub Copilot
- Codex (OpenAI)
- Cursor
- Claude Code
- Google Stitch

---

## 2. 项目整体架构

```
taste-skill/
├── skills/                    # 核心：技能模块目录
│   ├── taste-skill/           # 默认设计技能 (v2 experimental)
│   ├── taste-skill-v1/        # 原始 v1 版本（向后兼容）
│   ├── gpt-tasteskill/        # GPT/Codex 加强版
│   ├── image-to-code-skill/   # 图片→分析→代码 工作流
│   ├── redesign-skill/        # 已有项目重设计
│   ├── soft-skill/            # 高端视觉设计
│   ├── output-skill/          # 完整输出强制执行
│   ├── minimalist-skill/      # 极简主义 UI
│   ├── brutalist-skill/       # 工业蛮横主义 UI
│   ├── stitch-skill/          # Google Stitch 兼容
│   ├── imagegen-frontend-web/ # 网页设计参考图生成
│   ├── imagegen-frontend-mobile/ # 移动端设计参考图生成
│   ├── brandkit/              # 品牌套件图片生成
│   └── llms.txt               # 技能索引文件
├── research/                  # 研究模块
│   └── laziness/              # LLM 输出截断研究
├── assets/                    # 静态资源
├── examples/                  # 示例截图
├── .github/                   # GitHub 配置
│   ├── copilot-instructions.md # Copilot 全局指令
│   └── FUNDING.yml
├── skill.sh                   # 本地技能注册脚本
├── CHANGELOG.md               # 变更日志
├── README.md                  # 项目说明
└── LICENSE                    # MIT 许可证
```

### 架构分层

| 层级 | 组件 | 职责 |
|------|------|------|
| **入口/分发层** | `README.md`, `llms.txt`, `skill.sh` | 技能发现、安装指引、本地注册 |
| **核心技能层** | `skills/*/SKILL.md` | 各技能的声明式设计规则 |
| **辅助规则层** | `.github/copilot-instructions.md` | Copilot 全局行为约束 |
| **研究/支撑层** | `research/` | LLM 行为研究，为技能设计提供理论依据 |
| **输出/示例层** | `assets/`, `examples/` | 品牌资源与效果展示 |

---

## 3. 目录结构

```
/workspace/
├── .github/
│   ├── FUNDING.yml                          # 赞助信息（GitHub: Leonxlnx）
│   └── copilot-instructions.md              # Copilot 全局反slop指令
├── assets/
│   ├── .gitkeep
│   ├── readme-banner.png                    # README 横幅
│   └── taste-skill-logo.webp                # 品牌 Logo
├── examples/
│   ├── floria-bottom.webp                   # 示例效果图
│   ├── floria-full.webp
│   └── floria-top.webp
├── research/
│   ├── README.md                            # 研究模块概述
│   └── laziness/
│       ├── README.md                        # LLM惰性研究概述
│       ├── findings/
│       │   ├── empirical-results.md         # 2025 学术实验数据
│       │   └── references.md                # 引用的研究文献
│       ├── remediation/
│       │   ├── architectural-patterns.md    # 架构级修复方案
│       │   ├── parameter-tuning.md          # 参数调优方案
│       │   ├── prompt-engineering.md        # 提示工程技巧
│       │   └── reference-prompts.md         # 参考提示模板
│       └── root-causes/
│           ├── cognitive-shortcuts.md       # 认知捷径原因
│           ├── output-limits.md             # 输出限制原因
│           ├── rlhf-and-compute.md          # RLHF与计算经济原因
│           └── training-data-bias.md        # 训练数据偏差原因
├── skills/
│   ├── llms.txt                             # 技能索引（供LLM发现）
│   ├── taste-skill/SKILL.md                 # 默认设计技能 v2 (核心)
│   ├── taste-skill-v1/SKILL.md              # 原始 v1 版本
│   ├── gpt-tasteskill/SKILL.md              # GPT/Codex 加强版
│   ├── image-to-code-skill/SKILL.md         # 图片→代码工作流
│   ├── redesign-skill/SKILL.md              # 已有项目重设计
│   ├── soft-skill/SKILL.md                  # 高端视觉设计
│   ├── output-skill/SKILL.md                # 完整输出强制执行
│   ├── minimalist-skill/SKILL.md            # 极简主义 UI
│   ├── brutalist-skill/SKILL.md             # 工业蛮横主义 UI
│   ├── stitch-skill/SKILL.md                # Google Stitch 兼容技能
│   ├── stitch-skill/DESIGN.md               # 示例 DESIGN.md 输出
│   ├── imagegen-frontend-web/SKILL.md       # 网页设计参考图生成
│   ├── imagegen-frontend-mobile/SKILL.md    # 移动端设计参考图生成
│   └── brandkit/SKILL.md                    # 品牌套件图片生成
├── CHANGELOG.md                             # 版本变更日志
├── LICENSE                                  # MIT 许可证
├── README.md                                # 项目主文档
└── skill.sh                                 # 本地技能注册脚本
```

---

## 4. 核心机制：三旋钮系统

Taste Skill 的核心设计哲学是通过三个可调参数（称为"旋钮/Dials"）来控制 AI 输出的设计风格。所有技能规则都基于这些旋钮的值进行条件化。

### 4.1 旋钮定义

| 旋钮 | 全称 | 范围 | 低值 | 高值 | 默认值 |
|------|------|------|------|------|--------|
| **VARIANCE** | `DESIGN_VARIANCE` | 1-10 | 完美对称、居中 | 艺术化混乱、不对称 | **8** |
| **MOTION** | `MOTION_INTENSITY` | 1-10 | 静态、无动画 | 电影级、物理动画 | **6** |
| **DENSITY** | `VISUAL_DENSITY` | 1-10 | 画廊级、大量留白 | 驾驶舱级、数据密集 | **4** |

### 4.2 设计意图推断表

| 用户描述信号 | VARIANCE | MOTION | DENSITY |
|-------------|----------|--------|---------|
| "极简 / 干净 / 平静 / Linear风格" | 5-6 | 3-4 | 2-3 |
| "高端消费 / Apple风格 / 奢侈品牌" | 7-8 | 5-7 | 3-4 |
| "有趣 / 疯狂 / Dribbble / Awwwards / 实验性" | 9-10 | 8-10 | 3-4 |
| "着陆页 / 作品集 / 营销站 (默认)" | 7-9 | 6-8 | 3-5 |
| "信任优先 / 公共部门 / 无障碍关键" | 3-4 | 2-3 | 4-5 |

### 4.3 使用场景预设

| 场景 | VARIANCE | MOTION | DENSITY |
|------|----------|--------|---------|
| 着陆页 (SaaS, 主流) | 7 | 6 | 4 |
| 着陆页 (代理商/创意) | 9 | 8 | 3 |
| 着陆页 (高端消费) | 7 | 6 | 3 |
| 作品集 (设计师/工作室) | 8 | 7 | 3 |
| 作品集 (开发者) | 6 | 5 | 4 |
| 编辑/博客 | 6 | 4 | 3 |
| 公共部门服务 | 3 | 2 | 5 |
| 重设计 - 保留 | 匹配现有 | +1 | 匹配现有 |
| 重设计 - 翻新 | +2 | +2 | 匹配现有 |

---

## 5. 技能模块详解

### 技能分类

```
技能体系
├── 代码实现类技能 (Implementation Skills)
│   ├── 通用设计方向
│   │   ├── taste-skill        (默认通用，v2 experimental)
│   │   ├── taste-skill-v1     (v1 原始版本)
│   │   └── gpt-tasteskill     (GPT/Codex 加强版)
│   ├── 特定风格
│   │   ├── soft-skill         (高端昂贵 UI)
│   │   ├── minimalist-skill   (极简编辑风)
│   │   └── brutalist-skill    (工业蛮横风)
│   ├── 工作流
│   │   ├── image-to-code-skill (图片→代码)
│   │   └── redesign-skill     (已有项目重设计)
│   └── 辅助
│       ├── output-skill       (完整输出强制执行)
│       └── stitch-skill       (Google Stitch 兼容)
└── 图片生成类技能 (Image Generation Skills)
    ├── imagegen-frontend-web   (网页设计参考图)
    ├── imagegen-frontend-mobile (移动端设计参考图)
    └── brandkit               (品牌套件图)
```

---

### 5.1 代码实现类技能

#### 5.1.1 taste-skill — 默认设计技能 (v2 experimental)

- **安装名**: `design-taste-frontend`
- **文件**: [skills/taste-skill/SKILL.md](file:///workspace/skills/taste-skill/SKILL.md)
- **定位**: 项目的核心技能，适用于着陆页、作品集、重设计。不适用于仪表盘、数据表、多步骤表单。

**核心结构 (14 个章节 + 3 个附录)**:

| 章节 | 名称 | 功能描述 |
|------|------|----------|
| §0 | Brief Inference | 在生成代码前推断用户意图，输出一行"设计解读" |
| §1 | The Three Dials | 三旋钮配置与推断表 |
| §2 | Brief → Design System Map | 根据设计解读选择设计系统（Material/Fluent/Carbon等）或美学方向 |
| §3 | Default Architecture & Conventions | 默认技术栈：React/Next.js、Tailwind v4、Motion、Phosphor Icons |
| §4 | Design Engineering Directives | 字体、色彩、布局、阴影、交互态、数据、视觉资产等严格要求 |
| §5 | Context-Aware Proactivity | 动画模式：GSAP Sticky-Stack、Horizontal-Pan、Scroll-Reveal等 |
| §6 | Performance & Accessibility | 硬件加速、reduced-motion、暗色模式、Core Web Vitals |
| §7 | Dial Definitions | 三个旋钮的技术参考定义 |
| §8 | Dark Mode Protocol | 双模式默认、Token策略、对比度要求 |
| §9 | AI Tells (Forbidden Patterns) | 禁止的AI设计模式（含完整的em-dash禁令） |
| §10 | Reference Vocabulary | 模式名称词汇表（Hero、导航、布局、卡片、动画等） |
| §11 | Redesign Protocol | 重设计模式检测、审计、现代化杠杆 |
| §12 | The Block Library | 可复用代码块的契约与实现规范 |
| §13 | Out of Scope | 明确不适用范围 |
| §14 | Final Pre-Flight Check | 强制性的57项发布前检查清单 |
| Appendix A | Install Commands | 各设计系统的安装命令 |
| Appendix B | Canonical Sources | 各设计系统的官方文档链接 |
| Appendix C | Apple Liquid Glass | Web端近似实现方案 |

**关键设计规则**:
- **Em-Dash 完全禁止** (§9.G): 全页面零 `—` 字符
- **色彩一致性锁定** (§4.2): 一个页面一个强调色，禁止中途切换
- **形状一致性锁定** (§4.4): 一个页面一个圆角系统
- **按钮对比度检查** (§4.5): WCAG AA 4.5:1 最低要求
- **Hero 纪律**: 标题≤2行，副文本≤20词，CTA无滚动可见
- **Section-Layout-Repetition 禁令**: 8个section至少4种不同布局
- **Bento Cell Count Rule**: N个内容=N个单元格，不允许空单元格
- **Serif 严格限制**: Fraunces 和 Instrument_Serif 被禁止作为默认字体

#### 5.1.2 taste-skill-v1 — 原始版本

- **安装名**: `design-taste-frontend-v1`
- **文件**: [skills/taste-skill-v1/SKILL.md](file:///workspace/skills/taste-skill-v1/SKILL.md)
- **定位**: 保留 v1 原始行为，供已有项目向后兼容

**v1 vs v2 主要差异**:
- v1: 较简单的规则结构，10个章节，无 §0 Brief Inference
- v1: 无设计系统映射、无暗色模式协议、无重设计协议
- v1: 无 Block Library、无 Pre-Flight Check
- v1: 较少硬性禁令，em-dash 未完全禁止
- v1: 使用 Framer Motion（v2 改用 Motion）

#### 5.1.3 gpt-tasteskill — GPT/Codex 加强版

- **安装名**: `gpt-taste`
- **文件**: [skills/gpt-tasteskill/SKILL.md](file:///workspace/skills/gpt-tasteskill/SKILL.md)
- **特色**: Python 驱动的真实随机化布局、AIDA 结构、无缝 Bento 网格、GSAP 动画

**关键特性**:
- 使用模拟 Python `random.choice()` 进行布局选择
- 强制 AIDA 结构（Attention / Interest / Desire / Action）
- Hero 2-3行铁律
- `grid-flow-dense` 零间隙 Bento 网格
- GSAP ScrollTrigger 滚动动画（pinning / stacking / scrubbing）

#### 5.1.4 image-to-code-skill — 图片→代码工作流

- **安装名**: `image-to-code`
- **文件**: [skills/image-to-code-skill/SKILL.md](file:///workspace/skills/image-to-code-skill/SKILL.md)
- **核心流程**: 生成图片 → 深度分析 → 编写代码

**强制工作流顺序**:
1. 推断 section 数量
2. 逐 section 生成参考图片
3. 生成额外的细节/提取图片
4. 深度分析所有图片（字体、间距、色彩、布局、按钮、组件）
5. 实现前端代码与图片匹配

**关键规则**:
- 每个 section 一个独立图片（Codex 内强制）
- 禁止裁剪旧图片，必须重新生成
- 禁止嵌套盒子（box-in-box-in-box）
- 禁止微型 UI 杂乱元素
- Hero 最小化：标题 1-3 行，小笔记本电脑可见

#### 5.1.5 redesign-skill — 已有项目重设计

- **安装名**: `redesign-existing-projects`
- **文件**: [skills/redesign-skill/SKILL.md](file:///workspace/skills/redesign-skill/SKILL.md)
- **工作流**: 扫描 → 诊断 → 修复

**审计维度**:
- 字体（浏览器默认字体、缺少层级、孤儿词）
- 色彩与表面（纯黑、过饱和、多强调色、混合灰度）
- 布局（居中对称、三列等宽卡片、100vh bug、缺少最大宽度容器）
- 交互与状态（缺少 hover/active/focus/loading/empty/error）
- 内容（通用名称、AI 文案陈词滥调、Lorem Ipsum）
- 组件模式（通用卡片外观、手风琴 FAQ、模态框过度使用）
- 图标（Lucide/Feather 默认、陈腐隐喻）
- 代码质量（div soup、内联样式、硬编码像素）

**修复优先级**: 字体替换 → 调色板清理 → 交互态 → 布局间距 → 组件替换 → 状态补充 → 字体微调

#### 5.1.6 soft-skill — 高端视觉设计

- **安装名**: `high-end-visual-design`
- **文件**: [skills/soft-skill/SKILL.md](file:///workspace/skills/soft-skill/SKILL.md)
- **定位**: 15万美元代理商级别的数字体验

**核心架构**:
- **Double-Bezel（双边框）嵌套架构**: 外框 + 内芯的同心结构
- **Button-in-Button**: CTA 按钮内嵌套圆形图标容器
- **Fluid Island Nav**: 浮动玻璃胶囊导航栏
- **Magnetic Button Hover**: 基于 group 工具类的磁吸悬停
- **Scroll Interpolation**: 模糊入场动画（800ms+）

**Vibe 原型**:
1. Ethereal Glass（SaaS/AI/Tech）
2. Editorial Luxury（生活方式/房地产/代理商）
3. Soft Structuralism（消费/健康/作品集）

#### 5.1.7 output-skill — 完整输出强制执行

- **安装名**: `full-output-enforcement`
- **文件**: [skills/output-skill/SKILL.md](file:///workspace/skills/output-skill/SKILL.md)
- **定位**: 覆盖 LLM 默认截断行为

**禁止的输出模式**:
- 代码中: `// ...`, `// rest of code`, `// TODO`, `/* ... */`
- 文本中: "Let me know if you want me to continue", "for brevity"
- 结构: 骨架替代完整实现、只展示首尾跳过中间

**长输出处理**: 使用 `[PAUSED — X of Y complete. Send "continue" to resume from: next section name]` 格式

#### 5.1.8 minimalist-skill — 极简主义 UI

- **安装名**: `minimalist-ui`
- **文件**: [skills/minimalist-skill/SKILL.md](file:///workspace/skills/minimalist-skill/SKILL.md)
- **定位**: Notion/Linear 风格的编辑式产品 UI

**核心约束**:
- 暖色单色调色板（禁止渐变、禁止重阴影、禁止霓虹色）
- 圆角最大 8px-12px（禁止 `rounded-full`）
- 卡片边框 `1px solid #EAEAEA`
- 淡化柔和的 pastel 强调色
- 标签页使用大写微字体 + 宽字符间距
- 禁止 emoji、禁止 Inter 字体

#### 5.1.9 brutalist-skill — 工业蛮横主义 UI

- **安装名**: `industrial-brutalist-ui`
- **文件**: [skills/brutalist-skill/SKILL.md](file:///workspace/skills/brutalist-skill/SKILL.md)
- **定位**: 瑞士印刷风格 + 军事终端美学

**两种视觉原型**:
1. **Swiss Industrial Print**: 高对比度亮色模式，巨大无衬线字体，可见网格线
2. **Tactical Telemetry & CRT Terminal**: 暗色模式，等宽字体，CRT 扫描线效果

**核心约束**: 绝对禁止圆角（90度直角）、禁止渐变、禁止阴影、禁止半透明

#### 5.1.10 stitch-skill — Google Stitch 兼容

- **安装名**: `stitch-design-taste`
- **文件**: [skills/stitch-skill/SKILL.md](file:///workspace/skills/stitch-skill/SKILL.md)
- **定位**: 生成 Google Stitch 兼容的 `DESIGN.md` 文件

**DESIGN.md 输出结构**:
1. Visual Theme & Atmosphere
2. Color Palette & Roles
3. Typography Rules
4. Component Stylings
5. Layout Principles
6. Motion & Interaction
7. Anti-Patterns (Banned)

### 5.2 图片生成类技能

这三个技能**仅生成图片**，不编写代码。用于 ChatGPT Images、Codex 图片模式等。

#### 5.2.1 imagegen-frontend-web

- **安装名**: `imagegen-frontend-web`
- **文件**: [skills/imagegen-frontend-web/SKILL.md](file:///workspace/skills/imagegen-frontend-web/SKILL.md)
- **核心规则**: **每个 section 生成一张独立的横向图片**（8个section=8张图）

**组合变异引擎**:
- 主题范式（4选1）
- 背景特征（4选1）
- 字体特征（6选1）
- Hero 架构（6选1）
- Section 系统（6选1）
- 签名组件集（4/12个）
- 运动暗示语言（2/6个）

#### 5.2.2 imagegen-frontend-mobile

- **安装名**: `imagegen-frontend-mobile`
- **文件**: [skills/imagegen-frontend-mobile/SKILL.md](file:///workspace/skills/imagegen-frontend-mobile/SKILL.md)
- **定位**: iOS/Android/跨平台移动端屏幕概念图

**平台模式**: iOS-native premium / Android-native premium / Cross-platform premium neutral

**核心要求**: 默认在手机 mockup 中展示，遵守安全区域，逻辑流连贯

#### 5.2.3 brandkit

- **安装名**: `brandkit`
- **文件**: [skills/brandkit/SKILL.md](file:///workspace/skills/brandkit/SKILL.md)
- **定位**: 品牌套件概览图（Logo、调色板、字体、应用场景）

**默认布局**: 3×3 网格面板系统
1. Logo Cover
2. Logo Construction
3. Digital Application
4. Brand Essence
5. Color System
6. Typography
7. Physical Application
8. Image Direction
9. System Detail

**视觉模式**: Dark Developer、Dark Product、Dark Nature、Dark Security、Light Editorial、Luxury/Beauty、Voice/Communication、Cultural/Experimental

---

## 6. 关键类与函数说明

Taste Skill 本身是一个声明式技能系统，不包含可执行代码类。但其核心技能中定义了规范化的代码骨架和 API。

### 6.1 GSAP Sticky-Stack 规范骨架

**定义位置**: [skills/taste-skill/SKILL.md#L367-L423](file:///workspace/skills/taste-skill/SKILL.md#L367-L423)

```tsx
// 组件: StickyStack
// 功能: 卡片在滚动时逐一固定并堆叠
// 关键参数:
//   - start: "top top" — 固定在视口顶部
//   - pin: true — 钉住卡片
//   - scrub: true — 动画与滚动同步
// 关键约束: 除最后一张外所有卡片都被 pin，scale/opacity 由下一张卡片的 trigger 驱动
```

### 6.2 GSAP Horizontal-Pan 规范骨架

**定义位置**: [skills/taste-skill/SKILL.md#L429-L470](file:///workspace/skills/taste-skill/SKILL.md#L429-L470)

```tsx
// 组件: HorizontalPan
// 功能: 垂直滚动转换为横向平移
// 关键参数:
//   - distance = track.scrollWidth - window.innerWidth
//   - start: "top top"
//   - end: `+=${distance}` — 滚动距离 = 横向移动距离
//   - scrub: 1
```

### 6.3 Scroll-Reveal Stagger 规范骨架

**定义位置**: [skills/taste-skill/SKILL.md#L477-L505](file:///workspace/skills/taste-skill/SKILL.md#L477-L505)

```tsx
// 组件: RevealStagger
// 功能: 元素进入视口时依次淡入
// 关键参数:
//   - whileInView: { opacity: 1, y: 0 }
//   - viewport: { once: true, amount: 0.3 }
//   - transition: { duration: 0.6, delay: i * 0.06, ease: [0.16, 1, 0.3, 1] }
```

### 6.4 Apple Liquid Glass Web 近似实现

**定义位置**: [skills/taste-skill/SKILL.md#L1142-L1200](file:///workspace/skills/taste-skill/SKILL.md#L1142-L1200)

```css
/* 类名: .liquid-glass-web-approx */
/* 关键属性:
   - backdrop-filter: blur(24px) saturate(180%) contrast(1.05)
   - 内阴影: inset 0 1px 0 rgba(255,255,255,0.48)
   - 外阴影: 0 18px 60px rgba(0,0,0,0.18)
   - ::before: 径向渐变高光
   - ::after: 内边框 1px
   - 暗色模式: 自动适配
   - 减少透明: 回退为不透明背景
*/
```

### 6.5 Block Library 契约

**定义位置**: [skills/taste-skill/SKILL.md#L841-L892](file:///workspace/skills/taste-skill/SKILL.md#L841-L892)

```yaml
# 每个 Block 文件的 Frontmatter 结构:
name: asymmetric-split-hero
category: hero
dial_compatibility:
  variance: [6, 10]
  motion: [3, 10]
  density: [2, 5]
when_to_use: "描述使用场景"
not_for: "描述不适用的场景"
stack: ["react", "next", "tailwind", "motion"]
```

### 6.6 技能 SKILL.md 文件 Frontmatter

每个技能文件都遵循统一的前置元数据格式：

```yaml
---
name: <安装名>          # 用于 npx skills add --skill 的参数
description: <描述>     # 技能的简短说明
---
```

---

## 7. 辅助工具与脚本

### 7.1 skill.sh — 本地技能注册表

**文件**: [skill.sh](file:///workspace/skill.sh)

功能：本地技能注册与路径查找，将技能名称映射到对应的 `SKILL.md` 文件路径。

```bash
# 用法
source ./skill.sh <skill-name>

# 支持的技能名称
taste-skill, taste-skill-v1, gpt-taste, image-to-code-skill,
imagegen-frontend-web, imagegen-frontend-mobile, brandkit,
redesign-skill, soft-skill, output-skill, minimalist-skill,
brutalist-skill, stitch-skill
```

### 7.2 skills/llms.txt — 技能索引

**文件**: [skills/llms.txt](file:///workspace/skills/llms.txt)

功能：为 LLM 提供技能发现索引，每个技能一行，包含名称和简短描述。

### 7.3 .github/copilot-instructions.md

**文件**: [.github/copilot-instructions.md](file:///workspace/.github/copilot-instructions.md)

功能：GitHub Copilot 自动读取此文件，全局应用"反 Slop"行为标准。
- 禁止通用 UI
- 要求高级留白
- 要求电影级动画（弹簧物理）
- 要求完整实现（无占位符）

---

## 8. 研究模块

### 8.1 研究概述

**目录**: [research/](file:///workspace/research/)

为技能设计提供理论依据的背景研究。目前主要研究 LLM 输出截断问题。

### 8.2 LLM 惰性研究

**目录**: [research/laziness/](file:///workspace/research/laziness/)

#### 根因分析 (root-causes/)

| 文件 | 研究内容 |
|------|----------|
| [rlhf-and-compute.md](file:///workspace/research/laziness/root-causes/rlhf-and-compute.md) | RLHF 和计算经济如何造成系统性简洁偏好 |
| [training-data-bias.md](file:///workspace/research/laziness/root-causes/training-data-bias.md) | 人类代码中的占位符模式如何传播到模型输出 |
| [cognitive-shortcuts.md](file:///workspace/research/laziness/root-causes/cognitive-shortcuts.md) | 模型在复杂或长任务中走捷径的经验证据 |
| [output-limits.md](file:///workspace/research/laziness/root-causes/output-limits.md) | 上下文窗口不对称和消费级截断机制 |

#### 修复方案 (remediation/)

| 文件 | 方案 |
|------|------|
| [parameter-tuning.md](file:///workspace/research/laziness/remediation/parameter-tuning.md) | Temperature、Top-p 和 Gemini 思维级别配置 |
| [prompt-engineering.md](file:///workspace/research/laziness/remediation/prompt-engineering.md) | 结构化提示技巧：语法绑定、XML 框架、验证循环 |
| [architectural-patterns.md](file:///workspace/research/laziness/remediation/architectural-patterns.md) | MCP 集成、懒加载技能、开发者平台访问 |
| [reference-prompts.md](file:///workspace/research/laziness/remediation/reference-prompts.md) | 即用型提示模板 |

#### 实证发现 (findings/)

| 文件 | 内容 |
|------|------|
| [empirical-results.md](file:///workspace/research/laziness/findings/empirical-results.md) | 2025 年学术研究中的受控实验数据 |
| [references.md](file:///workspace/research/laziness/findings/references.md) | 引用的研究和进一步阅读材料 |

---

## 9. 项目运行方式

### 9.1 安装全部技能

通过 Vercel Labs 的 `agent-skills` CLI 工具安装：

```bash
npx skills add https://github.com/Leonxlnx/taste-skill
```

### 9.2 安装单个技能

```bash
# 安装默认设计技能 (v2 experimental)
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"

# 安装 v1 原始版本
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend-v1"

# 安装 GPT/Codex 加强版
npx skills add https://github.com/Leonxlnx/taste-skill --skill "gpt-taste"

# 安装其他技能：将 --skill 参数替换为对应的安装名
```

### 9.3 手动使用

直接将任意 `SKILL.md` 文件内容复制到项目或粘贴到 AI Agent 对话中。

### 9.4 本地技能查找

```bash
source ./skill.sh taste-skill
# 输出: skills/taste-skill/SKILL.md
```

### 9.5 环境要求

- **无运行时依赖**：技能是纯声明式文本文件
- **支持的 AI Agent**：Codex、Cursor、Claude Code、GitHub Copilot、Google Stitch
- **推荐技术栈**（由技能指导）：React/Next.js、Tailwind CSS v4、Motion (motion/react)、Phosphor Icons
- **注意**：技能本身不包含任何需要安装的 npm 包或运行时

---

## 10. 依赖关系图

### 10.1 技能间依赖关系

```
                    ┌─────────────────────┐
                    │    taste-skill v2    │  ← 核心默认技能
                    │ (design-taste-frontend)│
                    └──────────┬──────────┘
                               │ 继承/演变
                    ┌──────────▼──────────┐
                    │   taste-skill-v1    │  ← 原始版本（向后兼容）
                    └─────────────────────┘
                    
                    ┌─────────────────────┐
                    │   gpt-tasteskill    │  ← 独立加强版
                    └─────────────────────┘
                    
                    ┌─────────────────────┐
                    │ image-to-code-skill │  ← 使用 imagegen-* 作为输入
                    └──────────┬──────────┘
                               │ 消费
                    ┌──────────▼──────────┐
                    │ imagegen-frontend-* │  ← 图片生成技能
                    └─────────────────────┘

    辅助技能: output-skill ──→ 可叠加到任何代码技能上
    风格技能: soft-skill / minimalist-skill / brutalist-skill ──→ 独立使用
    工作流技能: redesign-skill ──→ 独立使用
    平台技能: stitch-skill ──→ Google Stitch 专用
```

### 10.2 技能与外部工具的关系

```
SKILL.md 文件
    ├──→ npx skills add (Vercel agent-skills CLI) ──→ 安装到 AI Agent
    ├──→ 直接粘贴到对话 ──→ ChatGPT / Codex / Claude
    ├──→ GitHub Copilot (自动读取 .github/copilot-instructions.md)
    └──→ Google Stitch (使用 stitch-skill 生成 DESIGN.md)
```

### 10.3 推荐的技术栈依赖

| 类别 | 推荐 | 替代/备注 |
|------|------|-----------|
| 框架 | React / Next.js (RSC) | - |
| 样式 | Tailwind CSS v4 | v3 仅限已有项目 |
| 动画 | Motion (motion/react) | 原 Framer Motion |
| 滚动动画 | GSAP + ScrollTrigger | 仅用于 pin/scrub |
| 图标 | Phosphor / HugeIcons / Radix / Tabler | Lucide 不推荐 |
| 字体 | Geist / Outfit / Cabinet Grotesk / Satoshi | Inter 不推荐 |
| 设计系统 | shadcn/ui / Radix Themes / Fluent / Carbon / Material | 按需选择 |

---

## 11. 版本历史

### 当前版本

| 版本 | 安装名 | 状态 |
|------|--------|------|
| v2 (experimental) | `design-taste-frontend` | 当前默认，活跃迭代中 |
| v1 | `design-taste-frontend-v1` | 稳定，向后兼容保留 |

### v2 (experimental) 主要变更

**新增章节**:
- §0 Brief Inference — 生成前先推断设计意图
- §2 Brief → Design System Map — 设计系统映射
- §8 Dark Mode Protocol — 暗色模式协议
- §11 Redesign Protocol — 重设计协议
- §12 The Block Library — 可复用代码块
- §13 Out of Scope — 明确不适用的范围
- §14 Final Pre-Flight Check — 57项发布前检查

**硬性禁令强化**:
- Em-Dash 完全禁止（零容忍）
- Section 编号 eyebrows 禁止
- 装饰性状态点默认禁止
- 评分/进度条背景轨道禁止
- div 伪造产品 UI 禁止

**技术栈更新**:
- Tailwind v4 默认
- Motion 替代 Framer Motion
- Phosphor/HugeIcons/Radix/Tabler 图标优先级

---

## 12. 设计系统与参考附录

### 12.1 支持的设计系统

| 设计系统 | 适用场景 | 安装命令 |
|----------|----------|----------|
| Material Web (M3) | Google 风格产品 | `npm install @material/web` |
| Fluent UI React | 微软/企业 SaaS | `npm install @fluentui/react-components` |
| IBM Carbon | B2B/企业分析 | `npm install @carbon/react @carbon/styles` |
| Shopify Polaris | Shopify 应用 | CDN 加载 |
| Atlassian (Atlaskit) | Jira 风格产品 | `yarn add @atlaskit/*` |
| Primer | GitHub 风格开发工具 | `npm install @primer/css` |
| GOV.UK Frontend | 英国公共服务 | `npm install govuk-frontend` |
| USWDS | 美国公共服务 | `npm install uswds` |
| Radix Themes | 现代无障碍 React | `npm install @radix-ui/themes` |
| shadcn/ui | 现代 SaaS | `npx shadcn@latest add ...` |
| Bootstrap 5.3 | 快速本地业务 MVP | `npm install bootstrap` |

### 12.2 美学方向（非设计系统）

当没有官方设计系统时，使用以下美学方向：

| 美学 | 实现方式 |
|------|----------|
| Glassmorphism / 磨砂玻璃 | `backdrop-filter` + 分层边框 + 高光叠加 |
| Bento (Apple 风格磁贴) | CSS Grid 混合单元格大小 |
| Brutalism | 原生 CSS + 等宽字体 + 原始边框 |
| Editorial / Magazine | 衬线字体 + 非对称网格 + 大量留白 |
| Dark Tech / Hacker | 等宽字体 + 霓虹强调色 + 终端主题 |
| Aurora / Mesh Gradients | SVG 或分层径向渐变 |
| Kinetic Typography | 原生 CSS 动画 + 滚动驱动动画 + GSAP |
| Apple Liquid Glass | Web 近似：`backdrop-filter` + 分层边框 + 高光 |

### 12.3 完整的 AI Tells 禁止清单

来自 [skills/taste-skill/SKILL.md §9](file:///workspace/skills/taste-skill/SKILL.md#L596-L700)：

**视觉与 CSS**:
- 禁止霓虹/外发光阴影
- 禁止纯黑 `#000000`
- 禁止过饱和强调色
- 禁止过度渐变文字
- 禁止自定义鼠标光标

**字体**:
- 禁止 Inter 作为默认字体
- 禁止超大 H1
- 禁止 Fraunces 和 Instrument_Serif 作为默认衬线
- 禁止混合字体家族强调

**布局与间距**:
- 禁止数学完美但视觉不协调的对齐
- 禁止三列等宽特性卡片
- 禁止 em-dash 字符（`—` 和 `–`）

**内容与数据**:
- 禁止通用名称（John Doe, Sarah Chan, Acme, Nexus）
- 禁止通用头像
- 禁止虚假完美数字（99.99%, 50%）
- 禁止 AI 文案陈词滥调（Elevate, Seamless, Unleash, Next-Gen）

**生产测试 Tells**:
- 禁止 Hero 中的版本标签（V0.6, BETA）
- 禁止 Section 编号 eyebrows（00 / INDEX, 001 · Capabilities）
- 禁止 "Quietly in use at" 社交证明标题
- 禁止装饰性状态点
- 禁止 `border-t` + `border-b` 在长列表每一行
- 禁止滚动提示（Scroll, ↓ scroll）
- 禁止地区/时间/天气条
- 禁止 div 伪造产品 UI
- 禁止图片上的 pill/标签叠加
- 禁止装饰性照片署名
- 禁止营销页上的版本页脚
- 禁止 Hero 底部装饰文字条
- 禁止 Section 标题中的浮动右上角子文本
- 禁止评分/进度条作为比较视觉
- 禁止通用步骤标签（Stage 1/Step 1/Phase 01）

---

> **文档生成日期**: 2026-06-07
> **项目版本**: v2 (experimental)
> **许可证**: MIT License · Copyright (c) 2026 Leonxlnx