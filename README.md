# 🎭 Kooky Skills

AI Agent Skills by Kooky — 让 AI 助手写出专业级 PRD / BRD 的技能包。

适用于任何支持读取技能文件的 AI 工具（QoderWork、Workbuddy、Codex、ChatGPT work、Trae work 等）。

---

## 我该安装哪个？

```
你能清晰描述需求的所有细节吗？（页面、字段、规则、边界情况）
│
├─ 能 → PRD Generator by Kooky
│        直接告诉 AI 你要什么，它生成带原型线框图的 PRD
│
└─ 不确定 / 不能 → BRD to PRD by Kooky
                    AI 会反复追问直到需求完全清晰，再输出 BRD 或 PRD
```

**简单来说：**
- **产品经理 / 需求描述清晰** → [PRD Generator ↓](#prd-generator-by-kooky)
- **非产品经理 / 需求还不完整** → [BRD to PRD ↓](#brd-to-prd-by-kooky)

---

> **AI 快速加载**
>
> 如果你是 AI，请根据用户需要读取对应文件并按 SKILL.md 中的指令执行：
>
> **PRD Generator：**
> - https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/prd-generator-by-kooky/SKILL.md
> - https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/prd-generator-by-kooky/assets/prd-template.html
>
> **BRD to PRD：**
> - https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/brd-to-prd-by-kooky/SKILL.md

---

## PRD Generator by Kooky

**适合人群：** 产品经理，或能清晰描述需求细节的人。

生成专业的产品需求文档（PRD），输出为独立的单文件 HTML，包含交互式原型线框图和详细需求说明。

**[查看在线 Demo →](https://kookyjinn.github.io/kooky-skills-prd/demo/prd-generator-by-kooky.html)**

### 生成的 PRD 包含什么？

- **左侧**：HTML/CSS 模拟的原型线框图（按钮、表格、表单、弹窗、流程图等）
- **右侧**：结构化的需求说明，带颜色标注（关键条件、警告、补充说明）
- **目录导航**：多页面目录，支持版本分组折叠
- **无限画布**：空格 + 鼠标拖拽平移（类似 Axure）
- **结构自校验**：自动检测 HTML 标签闭合，防止文件损坏

### 关于这个 Skill

这套 Skill 源于我日常工作中的真实需求文档积累。我基于历史 PRD 项目，通过 Codex 搭配 GPT-5.5 反复生成和优化，提炼出了当前的版本。Skill 主要定义的是**页面结构和布局规范**——这是我在实践中总结出的、开发和测试最容易理解的 PRD 呈现方式。仅代表个人观点，欢迎在使用过程中提出迭代建议。

**Demo 说明：** 在线 Demo 是使用 QoderWork 搭配 Qwen3.7-Max 基于真实业务需求生成的（已脱敏处理）。

**使用建议：** 建议先用基本的业务逻辑和功能要点让 AI 自由创作 1.0 版 PRD，再通过对话式迭代逐步打磨，最终得到满意的需求文档终稿。不要试图一次说清所有细节，让 AI 先搭框架，你再补充和修正。

**推荐模型：** Qwen3.7-Max 及以上水平的模型。模型能力直接影响输出质量，建议不要低于此水平。

### 快速加载

```
Fetch this skill and follow its instructions:
https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/prd-generator-by-kooky/SKILL.md
(template: https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/prd-generator-by-kooky/assets/prd-template.html)
```

### 安装方式

**方式一：一键安装**
下载 [`prd-generator-by-kooky-v1.1.0.skill`](./releases/prd-generator-by-kooky-v1.1.0.skill)，在你的 AI 工具中导入（通常在"技能"、"插件"或"扩展"菜单中）。

**方式二：手动安装**
下载 `prd-generator-by-kooky/` 目录，放到你 AI 工具的技能目录中，重启即可。

### 使用方式

```
"帮我写一个XX功能的PRD"
"生成需求文档"
"写个PRD"
/prd
```

---

## BRD to PRD by Kooky

**适合人群：** 非产品经理，或需求还不完整、需要 AI 帮你梳理和追问的人。

在 PRD 技能基础上增加了**需求澄清和结构化访谈**能力。AI 会通过反复追问确保每个模糊点都被澄清，然后输出业务需求文档（BRD），并可进一步转化为完整 PRD。

**核心原则：反复追问直到需求完全清晰，绝不跳过任何模糊点。**

### 它能做什么？

1. **结构化访谈**：逐层追问，直到每个细节都明确
2. **需求澄清**：识别遗漏的边界情况、隐藏假设
3. **BRD 输出**：生成结构化的业务需求文档
4. **升级为 PRD**：可选地将 BRD 转化为带原型线框图的完整 PRD

### 快速加载

```
Fetch this skill and follow its instructions:
https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/brd-to-prd-by-kooky/SKILL.md
```

### 安装方式

**方式一：一键安装**
下载 [`brd-to-prd-by-kooky-v1.0.0.skill`](./releases/brd-to-prd-by-kooky-v1.0.0.skill)，在你的 AI 工具中导入。

**方式二：手动安装**
下载 `brd-to-prd-by-kooky/` 目录，放到你 AI 工具的技能目录中，重启即可。

### 使用方式

```
"我要提个需求"
"我想加个功能"
"这个流程有问题，需要优化"
"帮我梳理一下XX业务的需求"
```

---

## 文件结构

```
kooky-skills-prd/
├── prd-generator-by-kooky/       # PRD Generator Skill
│   ├── SKILL.md
│   └── assets/
│       └── prd-template.html
├── brd-to-prd-by-kooky/          # BRD to PRD Skill
│   ├── SKILL.md
│   └── assets/
│       ├── brd-template.html
│       ├── prd-template.html
│       └── prd-guide.md
├── demo/                         # 在线 Demo（GitHub Pages）
│   └── prd-generator-by-kooky.html
├── releases/                     # 一键安装包
│   ├── prd-generator-by-kooky-v1.1.0.skill
│   └── brd-to-prd-by-kooky-v1.0.0.skill
└── README.md
```

## License

MIT

---

## English Version

### Which Skill Should I Use?

```
Can you clearly describe all the details of your requirements? (pages, fields, rules, edge cases)
│
├─ Yes → Install PRD Generator by Kooky
│         Tell AI what you need, it generates a PRD with interactive wireframes
│
└─ Not sure / No → Install BRD to PRD by Kooky
                    AI will keep asking until every ambiguity is resolved
```

**In short:**
- **Product manager / clear requirements** → [PRD Generator ↓](#prd-generator-by-kooky-1)
- **Non-PM / incomplete requirements** → [BRD to PRD ↓](#brd-to-prd-by-kooky-1)

---

> **AI Quick Load**
>
> If you are an AI reading this, fetch the skill file(s) the user needs:
>
> **PRD Generator:**
> - https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/prd-generator-by-kooky/SKILL.md
> - https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/prd-generator-by-kooky/assets/prd-template.html
>
> **BRD to PRD:**
> - https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/brd-to-prd-by-kooky/SKILL.md

---

### PRD Generator by Kooky

**Best for:** Product managers, or anyone who can clearly describe requirement details.

Generates professional Product Requirement Documents (PRD) as standalone HTML files with interactive wireframe mockups and detailed specifications.

**[View Live Demo →](https://kookyjinn.github.io/kooky-skills-prd/demo/prd-generator-by-kooky.html)**

#### What's in the generated PRD?

- **Left panel**: HTML/CSS wireframes (buttons, tables, forms, modals, flow charts, etc.)
- **Right panel**: Structured requirement descriptions with color-coded annotations (critical conditions, warnings, notes)
- **Sidebar navigation**: Multi-page directory with collapsible version groups
- **Infinite canvas**: Space + drag to pan (similar to Axure)
- **Self-validation**: Auto-checks HTML tag balance to prevent corrupted output

#### About This Skill

This skill is built from real-world PRD projects accumulated through my daily work. I iteratively generated and refined it using Codex with GPT-5.5 based on historical PRD documents. The skill primarily defines **page structure and layout conventions** — the format I've found most effective for developers and testers to understand. This is my personal perspective; feedback and improvement suggestions are welcome.

**About the Demo:** The live demo was generated using QoderWork with Qwen3.7-Max based on real business requirements (desensitized).

**Usage Tip:** Start by describing the basic business logic and key features, let AI freely create a v1.0 PRD, then iteratively refine through conversation to reach your final document. Don't try to describe everything at once — let AI build the framework first, then you supplement and correct.

**Recommended Model:** Qwen3.7-Max or above. Model capability directly impacts output quality.

#### Quick Load

```
Fetch this skill and follow its instructions:
https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/prd-generator-by-kooky/SKILL.md
(template: https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/prd-generator-by-kooky/assets/prd-template.html)
```

#### Installation

**Option 1: One-Click Install**
Download [`prd-generator-by-kooky-v1.1.0.skill`](./releases/prd-generator-by-kooky-v1.1.0.skill) and import it into your AI tool (via "Skills", "Extensions", or "Plugins" menu).

**Option 2: Manual Install**
Download the `prd-generator-by-kooky/` directory and place it in your AI tool's skill directory. Restart to activate.

#### Usage

```
"Help me write a PRD for XX feature"
"Generate a requirements document"
"Write a PRD"
/prd
```

---

### BRD to PRD by Kooky

**Best for:** Non-product managers, or anyone whose requirements are still incomplete and need AI to help clarify.

Builds on the PRD skill with **requirement elicitation and structured interview** capabilities. AI keeps asking probing questions until every ambiguity is resolved, then outputs a Business Requirements Document (BRD), which can optionally be converted into a full PRD.

**Core principle: Keep asking until every requirement is crystal clear. Never skip an ambiguous point.**

#### What It Does

1. **Structured Interview**: Layer-by-layer questioning until every detail is clear
2. **Requirement Clarification**: Identifies missing edge cases and hidden assumptions
3. **BRD Output**: Generates a structured Business Requirements Document
4. **Upgrade to PRD**: Optionally converts the BRD into a full PRD with wireframes

#### Quick Load

```
Fetch this skill and follow its instructions:
https://raw.githubusercontent.com/KookyJinn/kooky-skills-prd/main/brd-to-prd-by-kooky/SKILL.md
```

#### Installation

**Option 1: One-Click Install**
Download [`brd-to-prd-by-kooky-v1.0.0.skill`](./releases/brd-to-prd-by-kooky-v1.0.0.skill) and import it into your AI tool.

**Option 2: Manual Install**
Download the `brd-to-prd-by-kooky/` directory and place it in your AI tool's skill directory. Restart to activate.

#### Usage

```
"I want to propose a new requirement"
"I'd like to add a feature"
"This process has issues and needs optimization"
"Help me sort out the requirements for XX business"
```
