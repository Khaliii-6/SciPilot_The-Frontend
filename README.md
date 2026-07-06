# SciCopilot — 前端实现方案

<p align="center">
  <strong>面向软件工程科研训练的垂类智能体平台 · Frontend</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white" alt="React">
  <img src="https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white" alt="Vite">
  <img src="https://img.shields.io/badge/TailwindCSS-3-06B6D4?logo=tailwindcss&logoColor=white" alt="TailwindCSS">
  <img src="https://img.shields.io/badge/ECharts-5-E43961?logo=apache-echarts&logoColor=white" alt="ECharts">
  <img src="https://img.shields.io/badge/Zustand-4-FF6B6B?logo=zustand&logoColor=white" alt="Zustand">
</p>

---

## 目录

- [项目简介](#项目简介)
- [核心功能模块](#核心功能模块)
- [技术栈](#技术栈)
- [项目结构](#项目结构)
- [页面路由](#页面路由)
- [核心组件设计](#核心组件设计)
- [UI 设计规范](#ui-设计规范)
- [状态管理](#状态管理)
- [API 对接规范](#api-对接规范)
- [本地开发指南](#本地开发指南)
- [构建与部署](#构建与部署)
- [开发里程碑](#开发里程碑)
- [团队分工](#团队分工)

---

## 项目简介

**SciPilot** 是一个面向软件工程（SE）科研训练的垂类智能体平台，聚焦 SE 专业学生在科研入门阶段的真实痛点，构建"助研 + 助学"双轮驱动的智能辅助系统。

### 目标用户

| 用户类型 | 特征 | 核心诉求 |
|---------|------|---------|
| 初级科研者（研一/本科高年级） | 刚进入课题组，需要快速理解研究方向 | 降低论文阅读门槛、结构化解读、明确研究切入点 |
| 实验执行者（研一~研二） | 已有研究问题，需要设计实验 | 快速搭建实验框架、避免常见坑、理解结果含义 |
| 课程学习者（本科 SE 专业） | 正在学习软件工程核心课程 | 概念讲解、案例关联、知识图谱导航 |

### 前端职责

前端负责 SciPilot 的全部用户交互界面，包括五大核心功能模块的可视化呈现、实时对话交互、数据可视化图表渲染、知识图谱探索等。

---

## 核心功能模块

### 模块总览

| # | 功能模块 | 页面路由 | 核心交互 |
|---|---------|---------|---------|
| 1 | 论文精读 | `/paper/read` | 上传 PDF → 解析 → 结构化精读报告 → 多轮追问 |
| 2 | 研究问题拆解 | `/research/decompose` | 输入方向 → 交互式问题树 → 可行性评估 |
| 3 | 实验路线生成 | `/experiment/roadmap` | 选择研究问题 → 完整实验方案 → 导出 |
| 4 | 代码复现辅助 | `/code/reproduce` | 输入仓库地址 → 文件树 → 复现步骤 → 错误诊断 |
| 5 | 结果解释 | `/result/analyze` | 上传 CSV/JSON → 统计分析 → 图表可视化 → 洞察 |

### 模块 1：论文精读（Paper Deep Read）

用户上传 PDF 或输入论文标题/arXiv ID，系统自动解析并生成结构化精读报告。

**精读报告结构：**
```
1. 研究背景与动机（200 字摘要）
2. 核心研究问题（明确的问题陈述）
3. 方法与创新点（技术贡献，标新点编号）
4. 实验设计（数据集、baseline、评价指标）
5. 主要结果（关键数据与发现）
6. 局限性与未来工作（作者提及 + AI 分析）
7. 对我的启发（面向初学者的学习建议）
```

**前端交互要点：**
- 左侧：论文章节导航树（可点击跳转对应章节）
- 中间：精读报告渲染（Markdown + LaTeX 公式 + 引用卡片）
- 右侧：知识图谱关联面板（当前论文涉及的概念节点）
- 底部：对话输入框（支持针对任意章节追问）
- 引用溯源：点击引用编号 [1][2] 弹出原文片段 + 来源信息

### 模块 2：研究问题拆解（Research Question Decomposition）

用户输入宽泛的研究方向，系统输出结构化研究问题树。

**前端交互要点：**
- 交互式树形图（可折叠/展开节点）
- 节点颜色编码：可行性高 = 绿色，中 = 黄色，低 = 红色
- 点击节点弹出详情抽屉：相关论文、数据集、工具推荐
- 支持"收藏子问题"到个人研究计划
- 一键跳转论文精读（问题树中引用的论文可直接阅读）

### 模块 3：实验路线生成（Experiment Roadmap Generation）

基于研究问题和已精读论文，自动生成完整实验方案。

**前端交互要点：**
- 步骤时间线（甘特图风格，含预估天数）
- Baseline 对比卡片（含性能数据、论文链接、GitHub 地址）
- 数据集信息面板（规模、语言、下载按钮）
- 评价指标说明（公式、计算方式、适用场景）
- 工具链推荐列表
- 一键导出 Markdown / PDF

### 模块 4：代码复现辅助（Code Reproduction Assistant）

用户输入 GitHub 仓库地址，系统自动分析并生成逐步复现指南。

**前端交互要点：**
- 仓库基本信息卡片（名称、语言、Star 数、描述）
- 文件树展示（可展开查看目录结构）
- 依赖列表（包名、版本、用途）
- 关键文件标注（入口文件、配置文件、核心模块）
- 复现步骤检查清单（可逐项勾选）
- 错误诊断对话区（粘贴报错 → 获取修复建议）

### 模块 5：结果解释（Result Interpretation）

用户上传实验结果文件，系统自动解析数据并生成可视化分析。

**前端交互要点：**
- 拖拽上传区（支持 CSV / JSON / Excel）
- 自动图表画廊（柱状图、箱线图、折线图、热力图、雷达图）
- 统计摘要面板（均值、标准差、置信区间、显著性检验结果）
- 分析文本编辑器（AI 生成 + 人工修改）
- 论文写作建议（Results 段落草稿、图表标题）
- 导出图表 PNG/SVG + 分析文本 Markdown

---

## 技术栈

| 层级 | 技术选型 | 版本 | 用途 |
|------|---------|------|------|
| 框架 | React | 18.x | 组件化 UI 开发 |
| 语言 | TypeScript | 5.x | 类型安全 |
| 构建工具 | Vite | 5.x | 快速冷启动、HMR、优化构建 |
| 样式方案 | TailwindCSS | 3.x | 原子化 CSS |
| 组件库 | shadcn/ui | latest | 高质量可定制组件 |
| 状态管理 | Zustand | 4.x | 轻量全局状态管理 |
| 路由 | React Router | v6 | SPA 路由管理 |
| HTTP 客户端 | Axios | 1.x | API 请求、拦截器、错误处理 |
| 实时通信 | WebSocket API | 原生 | 流式对话输出 |
| 图表可视化 | ECharts | 5.x | 统计图表渲染 |
| 知识图谱 | D3.js | 7.x | 图谱可视化 |
| 公式渲染 | KaTeX | latest | LaTeX 数学公式渲染 |
| Markdown | react-markdown | latest | Markdown 内容渲染 |
| 表格扩展 | remark-gfm | latest | GFM 表格、任务列表支持 |
| 代码高亮 | PrismJS / highlight.js | latest | 代码块语法高亮 |

---

## 项目结构

```
frontend/
├── public/
│   ├── favicon.ico
│   └── logo.svg
│
├── src/
│   ├── app/                          # 应用入口与全局配置
│   │   ├── App.tsx                   # 根组件
│   │   ├── routes.tsx                # 路由配置
│   │   └── providers.tsx            # 全局 Provider（Zustand、Theme 等）
│   │
│   ├── pages/                        # 页面级组件（对应路由）
│   │   ├── Home/                     # 首页（产品介绍 + 快速入口）
│   │   │   └── index.tsx
│   │   ├── Login/                    # 登录页
│   │   │   └── index.tsx
│   │   ├── Register/                 # 注册页
│   │   │   └── index.tsx
│   │   ├── Dashboard/               # 用户仪表盘
│   │   │   └── index.tsx
│   │   ├── PaperRead/               # 论文精读页
│   │   │   ├── index.tsx
│   │   │   ├── SectionNav.tsx        # 章节导航树
│   │   │   ├── DeepReadReport.tsx    # 精读报告渲染
│   │   │   ├── CitationCard.tsx      # 引用卡片弹窗
│   │   │   └── PaperChat.tsx         # 论文追问对话
│   │   ├── PaperLibrary/            # 论文库
│   │   │   ├── index.tsx
│   │   │   ├── PaperCard.tsx         # 论文卡片
│   │   │   └── PaperFilter.tsx       # 筛选/搜索
│   │   ├── ResearchDecompose/       # 研究问题拆解页
│   │   │   ├── index.tsx
│   │   │   ├── ResearchTree.tsx      # 交互式问题树
│   │   │   ├── TreeNodeDetail.tsx    # 节点详情抽屉
│   │   │   └── FeasibilityBadge.tsx   # 可行性标签
│   │   ├── ExperimentRoadmap/       # 实验路线生成页
│   │   │   ├── index.tsx
│   │   │   ├── Timeline.tsx          # 步骤时间线
│   │   │   ├── BaselineCard.tsx       # Baseline 对比卡片
│   │   │   ├── DatasetPanel.tsx       # 数据集面板
│   │   │   └── ToolChainList.tsx     # 工具链推荐
│   │   ├── CodeReproduce/           # 代码复现辅助页
│   │   │   ├── index.tsx
│   │   │   ├── RepoInfoCard.tsx       # 仓库信息
│   │   │   ├── FileTree.tsx           # 文件树
│   │   │   ├── DependencyList.tsx      # 依赖列表
│   │   │   ├── ReproductionChecklist.tsx  # 复现步骤清单
│   │   │   └── ErrorDiagnosis.tsx     # 错误诊断对话
│   │   ├── ResultAnalyze/           # 结果分析页
│   │   │   ├── index.tsx
│   │   │   ├── DataUploader.tsx       # 拖拽上传
│   │   │   ├── ChartGallery.tsx       # 图表画廊
│   │   │   ├── StatsSummary.tsx       # 统计摘要
│   │   │   ├── AnalysisEditor.tsx      # 分析文本编辑器
│   │   │   └── WritingSuggestion.tsx   # 论文写作建议
│   │   ├── KnowledgeGraph/           # 知识图谱探索页
│   │   │   ├── index.tsx
│   │   │   └── GraphCanvas.tsx         # D3.js 图谱画布
│   │   └── Profile/                  # 个人中心
│   │       └── index.tsx
│   │
│   ├── components/                   # 可复用 UI 组件
│   │   ├── ui/                       # 基础 UI 组件（shadcn/ui 封装）
│   │   │   ├── Button.tsx
│   │   │   ├── Card.tsx
│   │   │   ├── Input.tsx
│   │   │   ├── Dialog.tsx
│   │   │   ├── Drawer.tsx
│   │   │   ├── Tabs.tsx
│   │   │   ├── Badge.tsx
│   │   │   ├── Tooltip.tsx
│   │   │   ├── Skeleton.tsx
│   │   │   └── ScrollArea.tsx
│   │   ├── layout/                   # 布局组件
│   │   │   ├── AppLayout.tsx          # 主布局（Header + Sidebar + Content）
│   │   │   ├── Header.tsx              # 顶部导航栏
│   │   │   ├── Sidebar.tsx            # 侧边栏
│   │   │   ├── Footer.tsx
│   │   │   └── MobileNav.tsx           # 移动端导航
│   │   ├── chat/                     # 对话相关组件
│   │   │   ├── ChatBubble.tsx         # 聊天气泡
│   │   │   ├── ChatInput.tsx          # 输入框
│   │   │   ├── ChatSidebar.tsx        # 会话列表侧边栏
│   │   │   ├── MessageList.tsx        # 消息列表
│   │   │   └── StreamingText.tsx       # 流式文本渲染
│   │   ├── markdown/                 # Markdown 渲染组件
│   │   │   ├── MarkdownRenderer.tsx
│   │   │   ├── CodeBlock.tsx          # 代码块（带高亮 + 复制）
│   │   │   └── LaTeXBlock.tsx         # LaTeX 公式渲染
│   │   ├── chart/                    # 图表组件
│   │   │   ├── BarChart.tsx
│   │   │   ├── LineChart.tsx
│   │   │   ├── BoxPlotChart.tsx
│   │   │   ├── HeatmapChart.tsx
│   │   │   ├── RadarChart.tsx
│   │   │   └── ChartWrapper.tsx       # ECharts 通用包装
│   │   └── common/                   # 通用业务组件
│   │       ├── LoadingSpinner.tsx
│   │       ├── EmptyState.tsx
│   │       ├── ErrorBoundary.tsx
│   │       ├── ConfirmDialog.tsx
│   │       ├── FileDropZone.tsx
│   │       ├── SearchBar.tsx
│   │       └── Pagination.tsx
│   │
│   ├── hooks/                        # 自定义 React Hooks
│   │   ├── useAuth.ts                # 用户认证状态
│   │   ├── useWebSocket.ts           # WebSocket 连接管理
│   │   ├── useChat.ts               # 对话逻辑
│   │   ├── usePaperRead.ts          # 论文精读流程
│   │   ├── useResearchTree.ts       # 问题树交互
│   │   ├── useExperiment.ts         # 实验方案管理
│   │   ├── useCodeAnalysis.ts       # 代码分析
│   │   ├── useResultAnalysis.ts     # 结果分析
│   │   ├── useFileUpload.ts         # 文件上传
│   │   └── useDebounce.ts           # 防抖
│   │
│   ├── services/                     # API 服务层
│   │   ├── api.ts                   # Axios 实例配置（拦截器、baseURL）
│   │   ├── auth.service.ts          # 登录/注册/登出
│   │   ├── paper.service.ts         # 论文上传/精读/追问
│   │   ├── research.service.ts      # 研究问题拆解
│   │   ├── experiment.service.ts    # 实验方案生成
│   │   ├── code.service.ts          # 代码复现辅助
│   │   ├── result.service.ts        # 结果分析
│   │   ├── kg.service.ts           # 知识图谱查询
│   │   └── user.service.ts          # 用户信息/历史记录
│   │
│   ├── store/                        # Zustand 状态管理
│   │   ├── authStore.ts             # 用户认证状态
│   │   ├── chatStore.ts             # 对话状态
│   │   ├── paperStore.ts            # 论文相关状态
│   │   └── uiStore.ts               # UI 状态（主题、侧边栏等）
│   │
│   ├── types/                        # TypeScript 类型定义
│   │   ├── api.ts                   # API 请求/响应类型
│   │   ├── paper.ts                 # 论文相关类型
│   │   ├── research.ts             # 研究问题相关类型
│   │   ├── experiment.ts            # 实验方案相关类型
│   │   ├── code.ts                  # 代码分析相关类型
│   │   ├── result.ts               # 结果分析相关类型
│   │   ├── chat.ts                 # 对话相关类型
│   │   └── user.ts                 # 用户相关类型
│   │
│   ├── utils/                        # 工具函数
│   │   ├── format.ts               # 格式化工具（日期、数字等）
│   │   ├── validators.ts           # 表单验证
│   │   ├── constants.ts            # 常量定义
│   │   └── helpers.ts             # 通用辅助函数
│   │
│   ├── styles/                       # 全局样式
│   │   ├── globals.css             # 全局 CSS 变量 + Tailwind 入口
│   │   ├── themes.css              # 主题变量（亮色/暗色）
│   │   └── animations.css          # 动画定义
│   │
│   └── assets/                       # 静态资源
│       ├── images/
│       ├── icons/
│       └── fonts/
│
├── index.html
├── package.json
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
├── tailwind.config.ts
├── postcss.config.js
├── .env.local                        # 环境变量（不提交到 Git）
├── .env.example                      # 环境变量示例（提交到 Git）
├── .eslintrc.cjs
├── .prettierrc
└── README.md
```

---

## 页面路由

```
/                       → 首页（产品介绍 + 快速入口）
/login                  → 登录页
/register               → 注册页
/dashboard              → 用户仪表盘（最近会话、收藏论文、学习进度）
/paper/read             → 论文精读页（上传/选择论文 → 查看报告 → 对话）
/paper/library          → 论文库（浏览、搜索、筛选、收藏）
/research/decompose     → 研究问题拆解页（输入方向 → 查看问题树）
/experiment/roadmap     → 实验路线生成页（选择问题 → 查看方案）
/code/reproduce         → 代码复现辅助页（输入仓库 → 查看指南）
/result/analyze         → 结果分析页（上传数据 → 查看图表 + 分析）
/kg/explore             → 知识图谱探索页（可视化浏览概念关系）
/profile                → 个人中心（历史记录、设置）
```

### 路由守卫规则

```
公开路由：/, /login, /register
需登录路由：/dashboard, /paper/*, /research/*, /experiment/*, /code/*, /result/*, /kg/*, /profile
```

未登录用户访问需登录路由 → 自动重定向到 `/login`

---

## 核心组件设计

### PaperReader 组件（论文精读）

```
┌─────────────────────────────────────────────────────────────┐
│  Header: [论文标题] [会议/年份标签] [收藏] [导出]            │
├──────────┬──────────────────────────┬───────────────────────┤
│ 章节导航  │  精读报告主体             │  知识图谱关联          │
│          │                          │                       │
│ 1. 背景   │  ## 研究背景与动机        │  相关概念节点：         │
│ 2. 核心问题│  ...内容...[1][2]        │  ● 自动程序修复        │
│ 3. 方法   │                          │    └─ ● 生成验证        │
│ 4. 实验   │  ## 核心研究问题          │    └─ ● 语义分析        │
│ 5. 结果   │  ...内容...               │  ● 缺陷预测            │
│ 6. 局限性  │                          │                       │
│ 7. 启发   │  [引用卡片弹窗]           │  [点击节点查看详情]     │
│          │                          │                       │
├──────────┴──────────────────────────┴───────────────────────┤
│  对话输入区：[输入追问...] [发送] [推荐问题 1] [推荐问题 2]   │
└─────────────────────────────────────────────────────────────┘
```

### ResearchTree 组件（研究问题拆解）

```
┌─────────────────────────────────────────────────────────────┐
│  输入区：[请输入研究方向，如"代码自动生成"]  [开始分析]       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🟢 核心问题 RQ1：如何提升大模型在代码缺陷修复中的准确率？      │
│  │                                                          │
│  ├─ 🟢 RQ1.1：哪种 prompting 策略更有效？                    │
│  │   └─ 数据集: Defects4J | 预期贡献: 方法创新              │
│  │                                                          │
│  ├─ 🟡 RQ1.2：如何结合静态分析信息增强修复定位？              │
│  │   └─ 数据集: Bugs.jar | 预期贡献: 工具集成               │
│  │                                                          │
│  └─ 🔴 RQ1.3：修复结果的可解释性如何量化评估？               │
│      └─ 挑战: 缺乏统一评估标准 | 预期贡献: 评估框架          │
│                                                             │
│  [收藏到研究计划]  [跳转论文精读]  [生成实验方案]             │
└─────────────────────────────────────────────────────────────┘
```

### ExperimentPlanner 组件（实验路线）

```
┌─────────────────────────────────────────────────────────────┐
│  实验方案：基于大模型的代码缺陷修复                             │
├─────────────────────────────────────────────────────────────┤
│  📋 实验目标                                                 │
│  验证 CoT prompting 策略对代码缺陷修复准确率的影响              │
├─────────────────────────────────────────────────────────────┤
│  📅 步骤时间线                                               │
│  ├─ Step 1: 数据准备（2天）████░░░░  [详情]                  │
│  ├─ Step 2: Baseline 复现（5天）░░░░░░░░  [详情]            │
│  ├─ Step 3: 方法实现（7天）░░░░░░░░░░  [详情]                │
│  └─ Step 4: 评估与对比（3天）░░░░░░░░  [详情]                │
├─────────────────────────────────────────────────────────────┤
│  ⚔️ 推荐 Baseline          │  📊 数据集        │  🛠️ 工具链  │
│  ┌─────────────────┐      │  ┌──────────────┐ │  ┌─────────┐ │
│  │ CoCoNut (ICSE'20)│     │  │ Defects4J    │ │  │ PyTorch │ │
│  │ ★ 150 GitHub    │      │  │ 835 bugs     │ │  │ HF      │ │
│  │ [代码] [论文]   │      │  │ [下载]       │ │  │ Docker  │ │
│  └─────────────────┘      │  └──────────────┘ │  └─────────┘ │
│  ┌─────────────────┐      │                    │              │
│  │ CURE (ICSE'21)   │     │  ┌──────────────┐ │              │
│  │ ★ 89 GitHub     │      │  │ QuixBugs     │ │              │
│  │ [代码] [论文]   │      │  │ 160 bugs     │ │              │
│  └─────────────────┘      │  └──────────────┘ │              │
├──────────────────────────┴────────────────────┴──────────────┤
│  [导出 Markdown]  [导出 PDF]  [保存到我的方案]                   │
└─────────────────────────────────────────────────────────────┘
```

### ResultAnalyzer 组件（结果分析）

```
┌─────────────────────────────────────────────────────────────┐
│  📁 数据上传                                                 │
│  ┌─────────────────────────────────────────┐                │
│  │     拖拽 CSV / JSON / Excel 到这里       │                │
│  │            或 [点击选择文件]               │                │
│  └─────────────────────────────────────────┘                │
├─────────────────────────────────────────────────────────────┤
│  📈 图表画廊                                                 │
│  ┌────────────────┐ ┌────────────────┐                     │
│  │  柱状图          │ │  箱线图          │                     │
│  │  (方法对比)      │ │  (分布差异)      │                     │
│  └────────────────┘ └────────────────┘                     │
│  ┌────────────────┐ ┌────────────────┐                     │
│  │  折线图          │ │  热力图          │                     │
│  │  (参数趋势)      │ │  (敏感性分析)    │                     │
│  └────────────────┘ └────────────────┘                     │
├─────────────────────────────────────────────────────────────┤
│  📊 统计摘要          │  📝 AI 分析文本                        │
│  均值: 0.85          │  根据实验结果，本文方法在                  │
│  标准差: 0.05        │  Defects4J 数据集上取得了                 │
│  95% CI: [0.82, 0.88]│  85% 的准确率，相比 baseline...          │
│  显著性: p < 0.05    │  [编辑] [重新生成]                       │
├─────────────────────────────────────────────────────────────┤
│  [导出图表 PNG]  [导出 SVG]  [导出分析报告 Markdown]           │
└─────────────────────────────────────────────────────────────┘
```

---

## UI 设计规范

### 设计原则

- **简洁专业**：面向学术用户，界面清爽、信息密度适中
- **结构优先**：所有输出内容结构化呈现（表格、树形、时间线）
- **溯源可见**：引用标注贯穿所有模块，点击即可查看来源
- **渐进披露**：复杂信息分层展示，默认折叠，按需展开

### 配色方案

```css
:root {
  /* 主色调 */
  --primary: #2563eb;        /* 蓝色 — 主按钮、链接、活跃状态 */
  --primary-hover: #1d4ed8;
  --primary-light: #dbeafe;

  /* 辅助色 */
  --accent: #0ea5e9;         /* 青色 — 强调元素、标签 */
  --success: #10b981;        /* 绿色 — 成功、可行性高 */
  --warning: #f59e0b;        /* 黄色 — 警告、可行性中 */
  --danger: #ef4444;         /* 红色 — 错误、可行性低 */
  --purple: #8b5cf6;         /* 紫色 — 知识图谱、AI 相关 */

  /* 中性色 */
  --bg: #ffffff;
  --bg-secondary: #f8fafc;
  --border: #e2e8f0;
  --text-primary: #0f172a;
  --text-secondary: #64748b;
  --text-muted: #94a3b8;
}
```

### 字体

- 中文：`"PingFang SC", "Microsoft YaHei", "Noto Sans CJK SC", sans-serif`
- 英文：`Inter` 或 `SF Pro`
- 代码：`"JetBrains Mono", "Consolas", "Courier New", monospace`

### 间距系统

基于 4px 网格：
- 组件内间距：`4px / 8px / 12px / 16px`
- 组件间间距：`16px / 24px / 32px`
- 区块间间距：`48px / 64px`

### 响应式断点

```
Mobile:  < 640px   (单列布局，侧边栏折叠为抽屉)
Tablet:  640-1024px (双列布局)
Desktop: > 1024px   (完整三列布局)
```

---

## 状态管理

使用 Zustand 进行全局状态管理，按功能模块拆分 store。

### authStore — 用户认证

```typescript
interface AuthState {
  user: User | null;
  token: string | null;
  isAuthenticated: boolean;
  login: (email: string, password: string) => Promise<void>;
  register: (data: RegisterData) => Promise<void>;
  logout: () => void;
  checkAuth: () => Promise<void>;
}
```

### chatStore — 对话状态

```typescript
interface ChatState {
  sessions: ChatSession[];
  currentSessionId: string | null;
  messages: Message[];
  isStreaming: boolean;
  createSession: (module: string) => Promise<string>;
  sendMessage: (content: string) => Promise<void>;
  loadHistory: () => Promise<void>;
  clearCurrentSession: () => void;
}
```

### paperStore — 论文状态

```typescript
interface PaperState {
  currentPaper: Paper | null;
  deepReadReport: DeepReadReport | null;
  isLoading: boolean;
  uploadPaper: (file: File) => Promise<Paper>;
  getDeepRead: (paperId: string) => Promise<DeepReadReport>;
  clearPaper: () => void;
}
```

### uiStore — UI 状态

```typescript
interface UIState {
  sidebarOpen: boolean;
  theme: 'light' | 'dark';
  toggleSidebar: () => void;
  toggleTheme: () => void;
}
```

---

## API 对接规范

### 基础配置

```typescript
// services/api.ts
const api = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL, // http://localhost:8000
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// 请求拦截器 — 自动附加 JWT Token
api.interceptors.request.use((config) => {
  const token = useAuthStore.getState().token;
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// 响应拦截器 — 统一错误处理
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      useAuthStore.getState().logout();
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);
```

### 接口列表

| 方法 | 端点 | 说明 |
|------|------|------|
| `POST` | `/api/v1/auth/login` | 用户登录 |
| `POST` | `/api/v1/auth/register` | 用户注册 |
| `GET` | `/api/v1/users/me` | 获取当前用户信息 |
| `POST` | `/api/v1/papers/upload` | 上传 PDF 论文（multipart） |
| `GET` | `/api/v1/papers/{id}/deep-read` | 获取精读报告 |
| `POST` | `/api/v1/papers/{id}/chat` | 论文追问对话 |
| `POST` | `/api/v1/research/decompose` | 研究问题拆解 |
| `POST` | `/api/v1/experiments/generate-roadmap` | 生成实验方案 |
| `POST` | `/api/v1/code/analyze-repo` | 分析 GitHub 仓库 |
| `POST` | `/api/v1/code/diagnose-error` | 错误诊断 |
| `POST` | `/api/v1/results/analyze` | 分析实验结果（multipart） |
| `GET` | `/api/v1/kg/concepts` | 获取知识图谱概念节点 |
| `GET` | `/api/v1/kg/concepts/{id}/relations` | 获取概念关联关系 |

### WebSocket 流式输出

```typescript
// hooks/useWebSocket.ts
const ws = new WebSocket(`ws://${host}/ws/chat/${sessionId}`);

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  switch (data.type) {
    case 'stream_chunk':   // 流式文本片段
      appendToCurrentMessage(data.content);
      break;
    case 'stream_end':      // 生成完毕
      finalizeMessage();
      break;
    case 'citation':        // 引用信息
      attachCitation(data.citation);
      break;
    case 'error':           // 错误
      showError(data.content);
      break;
  }
};
```

---

## 本地开发指南

### 环境要求

- Node.js >= 18.0
- npm >= 9.0（或 pnpm >= 8.0）
- 后端服务运行在 `http://localhost:8000`

### 安装步骤

```bash
# 1. 克隆仓库
git clone https://github.com/Khaliii-6/SciCopilot_The-Fronted-Portion.git
cd SciCopilot_The-Fronted-Portion

# 2. 安装依赖
npm install

# 3. 配置环境变量
cp .env.example .env.local
# 编辑 .env.local 填入实际值

# 4. 启动开发服务器
npm run dev
```

### 环境变量

```bash
# .env.local
VITE_API_BASE_URL=http://localhost:8000
VITE_WS_BASE_URL=ws://localhost:8000
```

### 可用脚本

```bash
npm run dev          # 启动开发服务器（http://localhost:5173）
npm run build        # 生产构建
npm run preview      # 预览生产构建
npm run lint         # ESLint 检查
npm run format       # Prettier 格式化
npm run type-check   # TypeScript 类型检查
```

---

## 构建与部署

### 构建配置

```typescript
// vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
  server: {
    port: 5173,
    proxy: {
      '/api': {
        target: 'http://localhost:8000',
        changeOrigin: true,
      },
      '/ws': {
        target: 'ws://localhost:8000',
        ws: true,
      },
    },
  },
  build: {
    outDir: 'dist',
    sourcemap: true,
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom', 'react-router-dom'],
          charts: ['echarts'],
          markdown: ['react-markdown', 'remark-gfm', 'katex'],
        },
      },
    },
  },
});
```

### GitHub Pages 部署

```bash
# 构建后部署到 GitHub Pages
npm run build
# Settings → Pages → Branch: main /root
```

---

## 开发里程碑

| 周次 | 里程碑 | 前端具体任务 | 验收标准 |
|------|--------|-------------|---------|
| W1 | 项目初始化 | 初始化 React + Vite + TailwindCSS；配置路由；搭建主布局组件 | 首页、登录页、Dashboard 骨架可用 |
| W2 | 论文库 + 精读 UI | 实现论文上传、论文库列表、精读报告渲染（Markdown + 引用卡片） | 论文可上传、精读报告正确渲染 |
| W3 | 论文追问 + 知识图谱 | 实现论文追问对话（WebSocket 流式）、知识图谱关联面板、引用溯源弹窗 | 追问实时流式输出、引用可点击查看 |
| W4 | 研究问题拆解 | 实现交互式问题树组件（D3.js / 自定义树）、节点颜色编码、详情抽屉 | 问题树可折叠/展开、节点可交互 |
| W5 | 实验路线 | 实现步骤时间线、Baseline 对比卡片、数据集面板、导出功能 | 完整实验方案页面可用 |
| W6 | 代码复现 | 实现仓库信息卡、文件树、依赖列表、复现清单、错误诊断对话 | 输入仓库地址后完整展示 |
| W7 | 结果分析 | 实现拖拽上传、ECharts 图表画廊（>=4 种）、统计摘要、分析编辑器 | 上传 CSV 后自动生成图表和分析 |
| W8 | 集成优化 | 全流程联调、Loading 状态、Error 状态、Empty 状态、响应式适配、性能优化 | 核心流程无阻断 Bug、页面加载 < 2s |
| W9 | 用户验证 + 提交 | 2+ 真实用户试用、收集反馈、修复问题、录制演示视频 | 用户反馈记录完整、演示视频 <= 3 分钟 |

---

## 团队分工

### 前端开发职责

| 负责模块 | 核心工作 |
|---------|---------|
| **登录/注册** | 登录页、注册页、认证状态管理 |
| **Dashboard** | 仪表盘页、最近会话列表、收藏论文、学习进度 |
| **论文精读** | 论文上传、精读报告渲染（Markdown + LaTeX）、引用卡片、追问对话 |
| **论文库** | 论文列表、搜索筛选、论文卡片、收藏功能 |
| **研究问题拆解** | 交互式问题树可视化、节点详情抽屉、可行性标签 |
| **实验路线** | 步骤时间线、Baseline 卡片、数据集面板、导出功能 |
| **代码复现** | 仓库信息、文件树、依赖列表、复现清单、错误诊断 |
| **结果分析** | 数据上传、ECharts 图表、统计摘要、分析编辑器 |
| **知识图谱** | D3.js 图谱可视化、概念导航、关联关系展示 |
| **UI 基础设施** | 布局组件、通用 UI 组件、主题系统、响应式适配 |

---

> 本 README 基于 SciPilot 详细实现方案编制，所有页面结构、组件设计、技术选型均严格对应实现方案文档。
>
> **Start small. Build the loop. Then scale the intelligence.**
