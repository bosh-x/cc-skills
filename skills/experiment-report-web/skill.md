# Skill: experiment-report-web

## Description

生成完整的实验结果汇报网页项目，包含实验设计、目的、结果、数据、分析等模块。

**核心特性**：
- 📁 自动在 `docs/` 下创建日期文件夹（如 `2026-03-26-experiment-name/`）
- 📋 自动维护索引页 `docs/index.html`，列出所有实验
- 🌐 URL 使用英文 slug，友好且易分享
- 🚀 自动部署到 Vercel（支持私有仓库）
- 🔄 支持多次运行，管理多个实验报告

**文件结构**：
```
docs/
├── index.html  ← 所有实验的索引
├── 2026-03-26-deep-learning/  ← 最新实验
├── 2026-03-25-data-augmentation/
└── 2026-03-24-model-compression/
```

## Quick Start

最快使用方式：
```bash
# 1. 调用 skill
/experiment-report-web

# 2. 回答几个问题
实验标题：深度学习优化
英文 slug：deep-learning-opt（用于 URL）
数据文件：（可选）提供现有数据路径

# 3. 自动生成并部署
# ✅ 创建 docs/2026-03-26-deep-learning-opt/
# ✅ 更新索引页 docs/index.html
# ✅ 部署到 Vercel

# 4. 获得 URL
# https://your-project.vercel.app/ （索引页）
# https://your-project.vercel.app/2026-03-26-deep-learning-opt/ （实验详情）
```

完成！每次运行都会创建新的日期文件夹，所有实验井然有序。

## Trigger Conditions

- 用户说 "生成实验报告网页"
- 用户说 "创建实验结果展示"
- 用户请求 "实验汇报网站"
- 用户说 "experiment report web"
- 用户说 "做实验展示网页"

## Prerequisites

使用此 skill 需要：
- **Vercel CLI**：`npm install -g vercel`
- **Vercel 账号**（免费，支持私有仓库）
- 已有实验数据和结果（或使用示例数据）

**首次使用 Vercel**：
```bash
# 1. 安装 Vercel CLI
npm install -g vercel

# 2. 登录
vercel login

# 3. 完成后就可以使用本 skill 了
```

**注意**：
- 默认生成纯 HTML 版本，无需构建工具
- 自动部署到 Vercel（支持私有仓库）
- 生成的文件也可以本地查看或部署到其他平台

## Instructions

### 步骤 1: 收集实验信息

使用 AskUserQuestion 询问用户以下信息：

1. **实验标题**（中文）：实验的完整标题
   - 示例：深度学习模型优化实验、BERT 微调、数据增强对比

2. **英文 slug**（用于 URL）：
   - 自动根据标题生成建议，例如：
     - "深度学习模型优化" → 建议：`deep-learning-optimization`
     - "BERT 微调实验" → 建议：`bert-fine-tuning`
     - "数据增强 v2" → 建议：`data-augmentation-v2`
   - 用户可以接受建议或自定义
   - 规则：小写字母、数字、连字符，不含空格和特殊字符

3. **是否已有实验数据**：如果有，提供数据文件路径

### 步骤 1.5: 自动创建文件夹

**自动在 docs/ 下创建子文件夹**，命名格式：`YYYY-MM-DD-english-title`

**重要**：文件夹名使用英文，作为 URL 路径。

示例：
```
docs/
├── 2026-03-26-deep-learning-optimization/
│   ├── index.html
│   ├── styles.css
│   ├── script.js
│   └── data/
│       └── experiments.json
├── 2026-03-25-dataset-benchmark/
│   └── ...
└── 2026-03-24-model-compression/
    └── ...
```

文件夹命名规则：
1. 使用当前日期：`YYYY-MM-DD`
2. 将实验标题转换为英文 slug（URL 友好格式）
3. 转换示例：
   - "深度学习模型优化" → `deep-learning-optimization`
   - "BERT 微调实验" → `bert-fine-tuning`
   - "数据增强 v2" → `data-augmentation-v2`
   - "Transformer 性能测试" → `transformer-performance`

4. 最终文件夹名：
   - "深度学习模型优化" → `2026-03-26-deep-learning-optimization`
   - "BERT 微调实验" → `2026-03-26-bert-fine-tuning`

**转换方法**：
- 如果标题已经是英文，直接使用（转小写，空格改为连字符）
- 如果是中文，询问用户提供英文 slug，或自动翻译关键词

如果用户提供了自定义路径（如 `results/`），则在该路径下创建日期子文件夹。

### 步骤 2: 准备部署

**默认部署到 Vercel**（支持私有仓库，免费）

不需要询问用户部署方式，直接准备：
1. 生成纯 HTML + CSS + JavaScript 文件
2. 自动部署到 Vercel
3. 返回部署 URL

如果用户明确说明不需要部署，则只生成文件。

### 步骤 3: 确认实验内容结构

询问用户需要包含哪些实验：
- 单个实验还是多个实验
- 每个实验的名称
- 是否需要实验对比功能

### 步骤 4: 创建项目结构

#### 纯 HTML 版本 - 多实验管理

**推荐结构**（支持多个实验报告）：
```
my-project/                 # 你的现有项目
├── src/                   # 实验代码
├── experiments/           # 实验脚本
├── docs/                  # 📍 报告中心
│   ├── index.html         # 🔥 所有实验的索引页（可选，自动生成）
│   ├── 2026-03-26-deep-learning-optimization/
│   │   ├── index.html
│   │   ├── styles.css
│   │   ├── script.js
│   │   └── data/
│   │       └── experiments.json
│   ├── 2026-03-25-dataset-benchmark/
│   │   ├── index.html
│   │   └── ...
│   └── 2026-03-24-model-compression/
│       └── ...
└── README.md
```

**优点**：
- ✅ 每个实验独立文件夹，结构清晰
- ✅ 按时间排序，易于查找
- ✅ 支持多个实验报告并存
- ✅ 可选的总索引页面（`docs/index.html`）
- ✅ Vercel 可以部署整个 docs/ 文件夹

**优点**：
- ✅ 零依赖，无需构建
- ✅ 与实验代码放在同一仓库
- ✅ GitHub Pages 支持从 `docs/` 文件夹发布
- ✅ 加载速度快
- ✅ 易于维护和更新数据
- ✅ 一个仓库包含代码和报告

#### 选项 B：Next.js 版本（Vercel）

创建以下完整结构：
```
{project-name}/
├── package.json
├── next.config.js
├── vercel.json
├── tailwind.config.js
├── postcss.config.js
├── tsconfig.json
├── public/
│   └── data/
│       └── experiments.json
├── src/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── globals.css
│   ├── components/
│   │   ├── ExperimentCard.tsx
│   │   ├── ExperimentDetail.tsx
│   │   ├── DataVisualization.tsx
│   │   ├── Navigation.tsx
│   │   └── Footer.tsx
│   └── types/
│       └── experiment.ts
└── README.md
```

**优点**：
- ✅ 更强大的功能和扩展性
- ✅ 支持服务端渲染
- ✅ 更好的 SEO

### 步骤 5: 生成核心文件

#### 如果选择选项 A（纯 HTML 版本）：

创建以下文件：

**1. index.html** - 单页应用主文件
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>实验报告中心</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div id="app">
        <header>
            <h1>🧪 实验报告中心</h1>
            <div class="search-bar">
                <input type="text" id="searchInput" placeholder="搜索实验...">
            </div>
        </header>

        <main id="content">
            <!-- 动态内容将在这里插入 -->
        </main>

        <footer>
            <p>© 2026 实验报告系统</p>
        </footer>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="script.js"></script>
</body>
</html>
```

**2. styles.css** - 现代化响应式样式
```css
/* 使用 CSS Grid 和 Flexbox 实现响应式布局 */
/* 包含卡片样式、动画效果、暗色模式支持 */
/* 使用 CSS 变量方便主题定制 */
```

**3. script.js** - 核心 JavaScript 逻辑
```javascript
// 功能包括：
// - 加载 experiments.json 数据
// - 渲染实验卡片列表
// - 实验详情页面切换
// - 使用 Chart.js 绘制数据图表
// - 搜索和筛选功能
// - 路由管理（基于 hash）
```

**4. data/experiments.json** - 实验数据（中文）

**5. README.md** - GitHub Pages 部署说明

#### 如果选择选项 B（Next.js 版本）：

按顺序创建以下文件：

1. **package.json** - 包含项目依赖
   ```json
   {
     "name": "experiment-report",
     "version": "1.0.0",
     "private": true,
     "scripts": {
       "dev": "next dev",
       "build": "next build",
       "start": "next start",
       "lint": "next lint"
     },
     "dependencies": {
       "next": "^14.0.0",
       "react": "^18.2.0",
       "react-dom": "^18.2.0",
       "recharts": "^2.10.0",
       "lucide-react": "^0.300.0"
     },
     "devDependencies": {
       "@types/node": "^20.0.0",
       "@types/react": "^18.2.0",
       "@types/react-dom": "^18.2.0",
       "typescript": "^5.3.0",
       "tailwindcss": "^3.4.0",
       "postcss": "^8.4.0",
       "autoprefixer": "^10.4.0"
     }
   }
   ```

2. **vercel.json** - Vercel 部署配置
   ```json
   {
     "buildCommand": "npm run build",
     "outputDirectory": ".next",
     "framework": "nextjs"
   }
   ```

3. **tsconfig.json** - TypeScript 配置

4. **tailwind.config.js** - Tailwind CSS 配置

5. **src/types/experiment.ts** - TypeScript 类型定义
   ```typescript
   export interface ExperimentData {
     id: string;
     title: string;
     design: string;
     objective: string;
     methodology: string;
     results: {
       summary: string;
       data: any[];
     };
     analysis: string;
     strengths: string[];
     weaknesses: string[];
     improvements: string[];
     nextSteps: string[];
     timestamp: string;
   }
   ```

6. **public/data/experiments.json** - 实验数据文件

7. **src/components/** - React 组件
   - **ExperimentCard.tsx** - 实验卡片展示
   - **ExperimentDetail.tsx** - 实验详情页面
   - **DataVisualization.tsx** - 数据可视化组件（使用 recharts）
   - **Navigation.tsx** - 导航栏
   - **Footer.tsx** - 页脚

8. **src/app/page.tsx** - 主页面
   ```typescript
   // 展示所有实验的列表
   // 包含搜索和筛选功能
   ```

9. **src/app/layout.tsx** - 布局组件

10. **src/app/globals.css** - 全局样式

11. **README.md** - 项目文档
    包含：
    - 快速开始指南
    - 本地开发说明
    - Vercel 部署步骤
    - 数据更新方法

### 步骤 5: 生成示例数据（中文）

**重要**：所有网页内容和数据必须使用中文。

如果用户没有提供实验数据，生成一个中文示例 experiments.json：
```json
{
  "experiments": [
    {
      "id": "exp-001",
      "title": "深度学习模型性能优化实验",
      "design": "采用对照实验设计，设置基线模型和优化模型两组，在相同数据集上进行训练和测试。控制变量包括：学习率、批次大小、优化器类型等。",
      "objective": "验证新提出的优化策略能否显著提升深度学习模型的收敛速度和最终精度，同时降低训练时间成本。",
      "methodology": "使用 PyTorch 框架实现模型，在 ImageNet-1K 数据集上进行实验。训练周期为 100 个 epoch，使用 4 块 NVIDIA A100 GPU。记录每个 epoch 的训练损失、验证精度和训练时间。",
      "results": {
        "summary": "实验成功完成。优化模型在第 60 个 epoch 即达到基线模型 100 个 epoch 的精度，最终精度提升 2.3%，训练时间减少 35%。",
        "data": [
          {"name": "基线模型", "精度": 76.5, "训练时间": 48},
          {"name": "优化模型", "精度": 78.8, "训练时间": 31}
        ]
      },
      "analysis": "数据显示优化策略显著提升了模型性能。通过引入自适应学习率调度和混合精度训练，收敛速度大幅提升。精度提升主要来自于更好的梯度更新策略和正则化方法。训练时间的减少得益于混合精度训练和优化的数据加载流程。",
      "strengths": [
        "实验设计严谨，控制变量充分",
        "使用了业界标准数据集，结果可比性强",
        "详细记录了训练过程中的各项指标",
        "多次重复实验，结果稳定可靠"
      ],
      "weaknesses": [
        "仅在单一数据集上验证，泛化性有待进一步测试",
        "未测试在更大模型上的效果",
        "超参数搜索空间相对有限"
      ],
      "improvements": [
        "在多个数据集（COCO, CIFAR-100）上进行验证",
        "扩展到更大规模的模型（如 ResNet-152, ViT-Large）",
        "进行更系统的超参数搜索和消融实验",
        "增加不同 GPU 配置下的性能测试"
      ],
      "nextSteps": [
        "将优化策略应用到其他计算机视觉任务（目标检测、语义分割）",
        "探索在 NLP 任务上的迁移效果",
        "撰写论文并投稿到相关会议",
        "开源代码和预训练模型"
      ],
      "timestamp": "2026-03-26"
    }
  ]
}
```

### 步骤 6: 创建 UI 组件（中文界面）

**重要**：所有 UI 文本必须使用中文。

实现以下功能的组件：

1. **首页展示**
   - 实验卡片网格布局
   - 每个卡片显示：标题、日期、简要结果
   - 悬停效果和点击跳转
   - 顶部标题："实验报告中心"
   - 搜索框占位符："搜索实验..."

2. **实验详情页**
   - 顶部：实验标题和时间
   - 标签页切换（中文标签）：
     - "实验设计"
     - "实验目的"
     - "研究方法"
     - "实验结果"
     - "数据分析"
     - "优势与不足"
     - "改进方向"
     - "下一步计划"

3. **数据可视化**
   - 支持柱状图、折线图、饼图
   - 交互式图表（悬停显示数据）
   - 响应式设计

4. **导航系统**
   - 顶部导航栏
   - 面包屑导航
   - 实验间快速切换

5. **响应式设计**
   - 桌面端：三列卡片布局
   - 平板端：两列布局
   - 移动端：单列布局

### 步骤 7: 添加交互功能

实现以下交互：
- **搜索**：按标题或关键词搜索实验
- **筛选**：按日期、状态筛选
- **排序**：按时间、标题排序
- **导出**：支持导出 PDF（可选）
- **打印**：优化打印样式

### 步骤 8: 创建部署文档

在生成的文件夹中创建 `README.md`，包含：

1. **快速开始** - 如何本地查看
2. **部署选项** - 四种部署方式的详细说明：
   - 本地查看（最简单）
   - Vercel 部署（私有仓库友好）
   - Netlify 部署（私有仓库友好）
   - GitHub Pages（仅公开仓库）
3. **数据更新** - 如何编辑实验数据
4. **隐私提示** - 根据用户选择的部署方式提供安全建议

### 步骤 9: 生成或更新索引页面（可选）

**检查 docs/ 下是否存在其他实验报告**：

如果存在多个实验文件夹，生成/更新 `docs/index.html` 作为总索引：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>实验报告中心</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", sans-serif;
            max-width: 1000px;
            margin: 0 auto;
            padding: 40px 20px;
            background: #f9fafb;
        }
        h1 {
            color: #1f2937;
            margin-bottom: 40px;
        }
        .experiments {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
        }
        .card {
            background: white;
            padding: 24px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .card:hover {
            transform: translateY(-4px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        }
        .card h2 {
            margin: 0 0 8px 0;
            font-size: 1.25rem;
        }
        .card a {
            color: #3b82f6;
            text-decoration: none;
        }
        .card a:hover {
            text-decoration: underline;
        }
        .card .date {
            color: #6b7280;
            font-size: 0.875rem;
        }
    </style>
</head>
<body>
    <h1>🧪 实验报告中心</h1>
    <div class="experiments">
        <div class="card">
            <h2><a href="2026-03-26-deep-learning-optimization/">深度学习模型优化实验</a></h2>
            <p class="date">📅 2026-03-26</p>
        </div>
        <div class="card">
            <h2><a href="2026-03-25-dataset-benchmark/">数据集基准测试</a></h2>
            <p class="date">📅 2026-03-25</p>
        </div>
        <div class="card">
            <h2><a href="2026-03-24-model-compression/">模型压缩实验</a></h2>
            <p class="date">📅 2026-03-24</p>
        </div>
    </div>
</body>
</html>
```

**索引页面功能**：
- 自动扫描 docs/ 下的所有日期文件夹
- 按时间倒序排列（最新的在前）
- 提取实验标题（从文件夹内的 experiments.json）
- 点击卡片跳转到具体实验报告

这样访问 `https://your-project.vercel.app/` 就能看到所有实验的列表，点击进入具体实验。

### 步骤 10: 部署到 Vercel

**自动执行部署**：

1. 使用 Bash 工具执行：
   ```bash
   cd docs
   vercel --prod
   ```

2. 如果是首次使用 Vercel，会提示登录：
   - 告知用户需要登录 Vercel
   - 引导用户运行 `vercel login`
   - 然后重新运行部署命令

3. 部署成功后，获得 URL：
   - 索引页：`https://your-project.vercel.app/`
   - 具体实验：`https://your-project.vercel.app/2026-03-26-deep-learning-optimization/`

### 步骤 11: 输出总结

告知用户：

```
✅ 实验报告网页已生成并部署！

📁 本地文件：docs/2026-03-26-{slug}/
  - index.html
  - styles.css
  - script.js
  - data/experiments.json

📋 索引页面：docs/index.html
  - 列出所有实验报告
  - 按时间倒序排列

🚀 在线访问：
  - 索引页：https://your-project.vercel.app/
  - 本次实验：https://your-project.vercel.app/2026-03-26-{slug}/

📝 更新数据：
  1. 编辑 docs/2026-03-26-{slug}/data/experiments.json
  2. cd docs && vercel --prod
  3. 自动重新部署

🔄 添加新实验：
  - 再次运行 /experiment-report-web
  - 自动创建新文件夹并更新索引

💡 提示：
  - Vercel 支持私有仓库
  - URL 使用英文 slug，更友好
  - 索引页会自动列出所有实验
  - 可在 Vercel 项目设置中启用密码保护（付费功能）
```

## Parameters

- `project_name`（可选）：项目目录名，默认为 "experiment-report"
- `experiments`（可选）：实验数据 JSON 文件路径

## Output

生成的项目包含：

1. **完整的 Next.js + TypeScript 项目**
2. **Tailwind CSS 样式系统**
3. **Recharts 数据可视化库**
4. **响应式设计**
5. **Vercel 部署配置**
6. **示例数据和文档**

项目结构：
```
experiment-report/
├── 📦 package.json         # 项目配置
├── ⚙️ next.config.js        # Next.js 配置
├── 🚀 vercel.json          # Vercel 部署配置
├── 🎨 tailwind.config.js   # 样式配置
├── 📂 public/
│   └── data/
│       └── experiments.json # 实验数据
├── 📂 src/
│   ├── app/               # Next.js App Router
│   ├── components/        # React 组件
│   └── types/            # TypeScript 类型
└── 📖 README.md          # 文档
```

## Examples

### Example 1: 基础使用（自动部署）

用户输入：
```
/experiment-report-web
```

交互过程：
```
Claude: 实验标题？
用户: 深度学习模型优化实验

Claude: 英文 slug（用于 URL）？
建议：deep-learning-optimization
用户: （直接回车使用建议）

Claude: 是否有现有实验数据文件？
用户: 没有，使用示例数据

Claude: 正在生成文件...
✅ 已创建 docs/2026-03-26-deep-learning-optimization/index.html
✅ 已创建 docs/2026-03-26-deep-learning-optimization/styles.css
✅ 已创建 docs/2026-03-26-deep-learning-optimization/script.js
✅ 已创建 docs/2026-03-26-deep-learning-optimization/data/experiments.json
✅ 已更新 docs/index.html（索引页）

正在部署到 Vercel...
✅ 部署成功！

在线访问：
- 索引页：https://your-project.vercel.app/
- 本次实验：https://your-project.vercel.app/2026-03-26-deep-learning-optimization/
```

### Example 2: 使用现有数据

用户输入：
```
/experiment-report-web
```

交互过程：
```
Claude: 实验标题？
用户: BERT 模型蒸馏实验

Claude: 英文 slug（用于 URL）？
建议：bert-distillation
用户: bert-distill

Claude: 是否有现有实验数据文件？
用户: 有，在 ./results/metrics.json

Claude: 正在读取数据...
✅ 已解析实验数据
正在生成文件...
✅ 已创建 docs/2026-03-26-bert-distill/index.html
✅ 已创建 docs/2026-03-26-bert-distill/styles.css
✅ 已创建 docs/2026-03-26-bert-distill/script.js
✅ 已导入数据到 docs/2026-03-26-bert-distill/data/experiments.json
✅ 已更新 docs/index.html（添加新实验到索引）

正在部署到 Vercel...
✅ 部署成功！

在线访问：
- 索引页：https://your-project.vercel.app/
- 本次实验：https://your-project.vercel.app/2026-03-26-bert-distill/
```

### Example 3: 多次使用，管理多个实验

**第一次使用**：
```
cd my-ml-project
/experiment-report-web

Claude: 实验标题？
用户: Transformer 性能优化

Claude: 英文 slug？
建议：transformer-optimization
用户: （回车）

✅ 已创建 docs/2026-03-26-transformer-optimization/
✅ 已创建 docs/index.html（索引页）
✅ 部署成功：https://my-project.vercel.app/
```

**一周后，第二次使用**：
```
/experiment-report-web

Claude: 实验标题？
用户: 数据增强对比实验

Claude: 英文 slug？
建议：data-augmentation-comparison
用户: data-aug

✅ 已创建 docs/2026-04-02-data-aug/
✅ 已更新 docs/index.html（现在有 2 个实验）
✅ 部署成功！

在线访问：
- 索引页：https://my-project.vercel.app/
  （显示 2 个实验，按时间排序）
- 新实验：https://my-project.vercel.app/2026-04-02-data-aug/
- 旧实验：https://my-project.vercel.app/2026-03-26-transformer-optimization/
```

**项目结构**：
```
my-ml-project/
├── src/
├── docs/
│   ├── index.html  ← 索引页（列出所有实验）
│   ├── 2026-04-02-data-aug/  ← 最新
│   │   └── ...
│   └── 2026-03-26-transformer-optimization/  ← 较早
│       └── ...
└── README.md
```

### Example 4: 多实验项目

项目会自动支持多个实验，数据格式：
```json
{
  "experiments": [
    { "id": "exp-001", ... },
    { "id": "exp-002", ... },
    { "id": "exp-003", ... }
  ]
}
```

## Features

生成的网页包含以下特性：

### 实验信息展示
- ✅ **实验设计**：详细的实验设计说明
- ✅ **实验目的**：明确的实验目标
- ✅ **实验方法**：方法论和流程
- ✅ **实验结果**：结果摘要和详细数据
- ✅ **数据可视化**：图表展示数据
- ✅ **结果分析**：深入的分析和讨论
- ✅ **优缺点**：实验的优势和不足
- ✅ **改进建议**：需要改进的地方
- ✅ **下一步计划**：后续工作安排

### 技术特性
- ⚡ **快速**：Next.js 14 App Router
- 🎨 **美观**：Tailwind CSS 现代 UI
- 📱 **响应式**：支持所有设备
- 📊 **可视化**：Recharts 交互图表
- 🔍 **可搜索**：实验搜索和筛选
- 🚀 **易部署**：Vercel 一键部署
- 📝 **TypeScript**：类型安全
- ♿ **无障碍**：遵循 WCAG 标准

## UI Design（中文界面）

页面设计参考：

### 首页布局
```
┌───────────────────────────────────────────────┐
│   🧪 实验报告中心                                │
│   [搜索实验...] [按日期筛选▼] [排序▼]            │
├───────────────────────────────────────────────┤
│ ┌──────────────┐ ┌──────────────┐ ┌──────────┐│
│ │ 深度学习优化   │ │ 数据集评估    │ │ 模型压缩 ││
│ │ 精度提升2.3%  │ │ 完成基准测试  │ │ 进行中   ││
│ │ 2026-03-26   │ │ 2026-03-25   │ │ 2026-03-24││
│ └──────────────┘ └──────────────┘ └──────────┘│
│ ┌──────────────┐ ┌──────────────┐ ┌──────────┐│
│ │ 迁移学习      │ │ 参数调优      │ │ 消融实验 ││
│ └──────────────┘ └──────────────┘ └──────────┘│
└───────────────────────────────────────────────┘
```

### 实验详情页
```
┌───────────────────────────────────────────────┐
│ ← 返回列表   深度学习模型性能优化    2026-03-26  │
├───────────────────────────────────────────────┤
│ [实验设计] [实验目的] [研究方法] [实验结果]      │
│ [数据分析] [优势与不足] [改进方向] [下一步]      │
├───────────────────────────────────────────────┤
│                                               │
│  📋 实验设计                                   │
│  采用对照实验设计，设置基线模型和优化模型...     │
│                                               │
│  📊 数据可视化                                 │
│  ┌─────────────────────────────────┐         │
│  │    [柱状图：精度对比]              │         │
│  │    基线: 76.5%  优化: 78.8%       │         │
│  └─────────────────────────────────┘         │
│                                               │
│  📈 训练曲线                                   │
│  ┌─────────────────────────────────┐         │
│  │    [折线图：训练过程]              │         │
│  └─────────────────────────────────┘         │
│                                               │
└───────────────────────────────────────────────┘
```

## Data Schema

实验数据的完整 JSON Schema：

```typescript
interface ExperimentData {
  id: string;                    // 唯一标识符
  title: string;                 // 实验标题
  design: string;                // 实验设计
  objective: string;             // 实验目的
  methodology: string;           // 实验方法
  results: {
    summary: string;             // 结果摘要
    data: Array<{                // 数据点
      name: string;
      value: number;
      [key: string]: any;
    }>;
    visualizations?: {           // 可视化配置
      type: 'bar' | 'line' | 'pie';
      xAxis?: string;
      yAxis?: string;
    }[];
  };
  analysis: string;              // 结果分析
  strengths: string[];           // 优点列表
  weaknesses: string[];          // 缺点列表
  improvements: string[];        // 改进建议
  nextSteps: string[];          // 下一步计划
  timestamp: string;            // 实验日期
  tags?: string[];              // 标签（可选）
  status?: 'completed' | 'in-progress' | 'planned'; // 状态
}

interface ExperimentCollection {
  experiments: ExperimentData[];
  metadata?: {
    projectName: string;
    author: string;
    lastUpdated: string;
  };
}
```

## Deployment Steps

### 方法 1: 本地查看（最简单，私有仓库适用）

**无需部署，直接在浏览器中打开！**

#### 直接打开

```bash
# 方法 1：直接双击 index.html 文件
# 在 Finder/资源管理器中找到 docs/index.html，双击打开

# 方法 2：使用命令行
cd docs
open index.html  # macOS
# 或
start index.html  # Windows
# 或
xdg-open index.html  # Linux
```

#### 本地服务器（推荐，避免 CORS 问题）

```bash
# Python 3
cd docs
python3 -m http.server 8000
# 访问 http://localhost:8000

# Node.js
npx http-server docs -p 8000
# 访问 http://localhost:8000

# Python 2
python -m SimpleHTTPServer 8000
```

**优点**：
- ✅ 完全私密，数据不离开本地
- ✅ 无需账号和配置
- ✅ 即时查看，无需等待部署
- ✅ 适合敏感数据和内部实验

---

### 方法 2: Vercel（私有仓库免费，推荐）

**支持私有 GitHub 仓库，完全免费！**

#### 一键部署

1. 访问 https://vercel.com/new
2. 用 GitHub 账号登录
3. 选择你的**私有仓库**
4. Vercel 会自动检测为静态网站
5. 设置：
   - **Root Directory**: `docs`（如果报告在 docs 文件夹）
   - **Output Directory**: `.`（留空或当前目录）
6. 点击 **Deploy**
7. 获得 URL：`https://your-project.vercel.app`

#### CLI 部署

```bash
# 安装 Vercel CLI
npm i -g vercel

# 部署（首次会要求登录）
cd docs
vercel

# 生产环境
vercel --prod
```

**优点**：
- ✅ 支持私有仓库
- ✅ 自动 HTTPS
- ✅ 全球 CDN 加速
- ✅ 每次推送自动部署
- ✅ 免费额度足够个人使用

**隐私设置**：
- Vercel 部署的网站默认是**公开可访问**的
- 如需限制访问，可以：
  1. 在 Vercel 项目设置中启用 **Password Protection**
  2. 使用 Vercel 的 **Authentication** 功能（Pro 版）

---

### 方法 3: Netlify（私有仓库免费）

**另一个支持私有仓库的选择**

1. 访问 https://app.netlify.com
2. 连接 GitHub 账号
3. 选择私有仓库
4. 设置：
   - **Base directory**: `docs`
   - **Publish directory**: `docs`（或留空）
5. 点击 **Deploy**

**优点**：
- ✅ 支持私有仓库
- ✅ 简单易用
- ✅ 免费 SSL
- ✅ 支持密码保护（免费版）

---

### 方法 4: GitHub Pages（仅公开仓库，免费）

**如果你愿意公开实验报告**

#### 步骤 1：推送到 GitHub

```bash
# 1. 初始化 Git 仓库
cd experiment-report
git init

# 2. 添加文件
git add .
git commit -m "初始化实验报告网站"

# 3. 连接到 GitHub 仓库
git remote add origin https://github.com/yourusername/experiment-report.git
git branch -M main
git push -u origin main
```

#### 步骤 2：启用 GitHub Pages

1. 访问你的 GitHub 仓库（例如：`my-project`）
2. 点击 **Settings** → **Pages**
3. 在 "Source" 下选择：
   - Branch: `main`（或 `master`）
   - Folder: **`/docs`**（如果你生成在 docs 文件夹）
   - 或 Folder: `/ (root)`（如果是独立仓库）
4. 点击 **Save**
5. 等待几分钟，你的网站将发布在：
   - `https://yourusername.github.io/my-project/`（从 docs 发布）
   - 或 `https://yourusername.github.io/experiment-report/`（独立仓库）

#### 步骤 3：更新数据

```bash
# 编辑 data/experiments.json 后
git add data/experiments.json
git commit -m "更新实验数据"
git push

# GitHub Pages 会自动重新部署（约 1-2 分钟）
```

**优点**：
- ✅ 完全免费
- ✅ 无需额外账号
- ✅ 自动 HTTPS
- ✅ 自定义域名支持
- ✅ 推送即部署

---

### 方法 2: Vercel（Next.js 版本）

**适合需要高级功能的用户**

#### Vercel CLI

```bash
# 1. 安装 Vercel CLI
npm i -g vercel

# 2. 进入项目目录
cd experiment-report

# 3. 部署
vercel

# 4. 生产环境部署
vercel --prod
```

#### Vercel Dashboard

1. 访问 https://vercel.com/new
2. 导入 Git 仓库或上传项目文件夹
3. Vercel 自动检测 Next.js
4. 点击 "Deploy"
5. 几分钟后获得部署 URL

#### GitHub 自动部署

1. 将项目推送到 GitHub
2. 在 Vercel 连接 GitHub 仓库
3. 每次 push 自动重新部署

**注意**：GitHub Pages 仅支持公开仓库！私有仓库无法使用 GitHub Pages（需要 GitHub Pro）。

---

### 部署方式对比

| 特性 | 本地查看 | Vercel | Netlify | GitHub Pages |
|------|---------|--------|---------|--------------|
| **费用** | 完全免费 | 免费 | 免费 | 免费 |
| **私有仓库** | ✅ 支持 | ✅ 支持 | ✅ 支持 | ❌ 不支持* |
| **部署难度** | ⭐ 最简单 | ⭐⭐ 简单 | ⭐⭐ 简单 | ⭐⭐⭐ 需公开 |
| **网络访问** | ❌ 仅本地 | ✅ 全球访问 | ✅ 全球访问 | ✅ 全球访问 |
| **HTTPS** | ❌ 无 | ✅ 自动 | ✅ 自动 | ✅ 自动 |
| **CDN 加速** | ❌ 无 | ✅ 有 | ✅ 有 | ✅ 有 |
| **访问控制** | ✅ 完全私密 | ⭐ 付费功能 | ✅ 免费密码 | ❌ 公开 |
| **自动部署** | ❌ 手动 | ✅ Git 推送 | ✅ Git 推送 | ✅ Git 推送 |
| **构建需求** | ❌ 无 | ❌ 无（静态） | ❌ 无（静态） | ❌ 无 |
| **自定义域名** | ❌ 无 | ✅ 支持 | ✅ 支持 | ✅ 支持 |

\* GitHub Pages 在免费账户下仅支持公开仓库。需要私有仓库 + GitHub Pages 需要升级到 GitHub Pro。

### 推荐选择

- **实验数据敏感** → 选择 **本地查看** 或 **Netlify**（有免费密码保护）
- **私有仓库 + 需要分享** → 选择 **Vercel** 或 **Netlify**
- **公开仓库** → 选择 **GitHub Pages**（最简单）
- **团队内部使用** → 选择 **本地查看** + 共享 HTML 文件

## Customization

用户可以自定义：

### 主题颜色
编辑 `tailwind.config.js`：
```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: '#your-color',
        secondary: '#your-color',
      }
    }
  }
}
```

### 布局
修改 `src/app/layout.tsx` 调整整体布局

### 数据格式
在 `src/types/experiment.ts` 中扩展类型定义

### 图表样式
在 `src/components/DataVisualization.tsx` 中自定义图表

## Error Handling

常见问题和解决方案：

- **部署失败**：检查 `vercel.json` 配置
- **图表不显示**：确认 `experiments.json` 数据格式正确
- **样式错误**：运行 `npm run build` 检查 Tailwind 配置
- **TypeScript 错误**：检查 `tsconfig.json` 配置

## Notes

### 在现有项目中使用的最佳实践

1. **推荐文件夹结构**
   ```
   my-project/
   ├── src/              # 实验代码
   ├── docs/             # 📍 GitHub Pages 报告（推荐）
   ├── experiments/      # 实验脚本
   ├── data/            # 原始数据
   └── README.md
   ```

2. **文件夹选择建议**
   - **`docs/`** - GitHub Pages 默认支持，推荐
   - **`results/report/`** - 语义清晰
   - **`experiments/report/`** - 与实验代码接近

3. **数据同步**
   - 可以使用脚本自动将实验结果转换为 JSON
   - 示例：`python scripts/export_results.py > docs/data/experiments.json`

4. **版本控制**
   - 将报告网页和代码一起提交
   - 使用 `.gitignore` 排除临时文件
   - 每次实验后更新 `experiments.json`

5. **团队协作**
   - 一个仓库包含代码、数据和报告
   - 团队成员可以直接访问 GitHub Pages 查看最新结果
   - 无需额外的文档系统

### 私有仓库最佳实践

如果你的实验项目在私有仓库中，推荐以下方案：

#### 方案 A：完全私密（本地查看）
```bash
# 1. 生成报告到 docs/
/experiment-report-web

# 2. 本地启动服务器
cd docs
python3 -m http.server 8000

# 3. 浏览器访问 http://localhost:8000
```

**适用场景**：
- 实验数据敏感，不能上传到任何云服务
- 仅个人查看，不需要分享
- 内网环境

#### 方案 B：私有分享（Vercel）
```bash
# 1. 生成报告到 docs/
/experiment-report-web

# 2. 部署到 Vercel（支持私有仓库）
cd docs
vercel

# 3. 获得私有 URL（只有知道链接的人才能访问）
# https://your-experiment-abc123.vercel.app
```

**适用场景**：
- 需要分享给导师、同事
- 数据不是极度敏感
- 愿意使用第三方服务

**安全提示**：
- Vercel 默认生成的 URL 很难猜测（包含随机字符串）
- 可以在 Vercel 项目设置中启用密码保护（需要付费账号）
- 定期检查谁有访问权限

#### 方案 C：团队内部（文件共享）
```bash
# 1. 生成报告到 docs/
/experiment-report-web

# 2. 打包整个 docs/ 文件夹
zip -r experiment-report.zip docs/

# 3. 通过企业网盘/邮件分享 zip 文件
# 同事下载后双击 index.html 即可查看
```

**适用场景**：
- 团队内部分享
- 不依赖外部服务
- 符合企业安全政策

### 其他注意事项

- 生成的是生产就绪的项目，可直接部署
- 支持暗色模式（可选功能）
- 所有数据存储在 JSON 文件中，易于编辑
- 支持 SEO 优化（公开部署时）
- 可以扩展添加数据库支持
- 建议定期备份 `experiments.json`
- 部署从推送到生效约需 1-2 分钟
- **重要**：确认实验数据的隐私要求后再选择部署方式

## Related Skills

- `/run-experiment`: 运行实验后可用此 skill 生成报告
- `/analyze-results`: 分析结果后生成可视化报告
- `/readme-generator`: 为实验项目生成 README

## 完整代码示例（纯 HTML 版本）

### index.html 完整代码

生成的 index.html 包含完整的单页应用结构：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>实验报告中心</title>
    <meta name="description" content="机器学习实验结果展示与分析">
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div id="app">
        <!-- 导航栏 -->
        <header class="header">
            <div class="container">
                <h1 class="logo">🧪 实验报告中心</h1>
                <nav class="nav">
                    <a href="#" class="nav-link active" onclick="showHome()">首页</a>
                    <a href="#" class="nav-link" onclick="showAbout()">关于</a>
                </nav>
            </div>
        </header>

        <!-- 主内容区 -->
        <main class="main">
            <div class="container">
                <!-- 搜索栏 -->
                <div class="search-section" id="searchSection">
                    <input type="text" id="searchInput" class="search-input"
                           placeholder="🔍 搜索实验..."
                           oninput="filterExperiments()">
                    <select id="sortSelect" class="sort-select" onchange="sortExperiments()">
                        <option value="date-desc">最新优先</option>
                        <option value="date-asc">最早优先</option>
                        <option value="title">按标题</option>
                    </select>
                </div>

                <!-- 实验列表 -->
                <div id="experimentList" class="experiment-grid"></div>

                <!-- 实验详情 -->
                <div id="experimentDetail" class="experiment-detail" style="display: none;"></div>
            </div>
        </main>

        <!-- 页脚 -->
        <footer class="footer">
            <div class="container">
                <p>© 2026 实验报告系统 | 基于 GitHub Pages 部署</p>
            </div>
        </footer>
    </div>

    <!-- Chart.js 用于数据可视化 -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <script src="script.js"></script>
</body>
</html>
```

### styles.css 样式代码

```css
:root {
    --primary-color: #3b82f6;
    --secondary-color: #8b5cf6;
    --success-color: #10b981;
    --danger-color: #ef4444;
    --text-color: #1f2937;
    --bg-color: #f9fafb;
    --card-bg: #ffffff;
    --border-color: #e5e7eb;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
                 "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
    line-height: 1.6;
    color: var(--text-color);
    background: var(--bg-color);
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
}

/* 导航栏 */
.header {
    background: var(--card-bg);
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    position: sticky;
    top: 0;
    z-index: 100;
}

/* 实验卡片网格 */
.experiment-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 24px;
    margin-top: 32px;
}

.experiment-card {
    background: var(--card-bg);
    border-radius: 12px;
    padding: 24px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    transition: transform 0.2s, box-shadow 0.2s;
    cursor: pointer;
}

.experiment-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 12px rgba(0,0,0,0.15);
}

/* 响应式设计 */
@media (max-width: 768px) {
    .experiment-grid {
        grid-template-columns: 1fr;
    }
}
```

### script.js 核心逻辑

```javascript
// 全局变量
let experiments = [];
let currentExperiment = null;

// 页面加载时初始化
document.addEventListener('DOMContentLoaded', async () => {
    await loadExperiments();
    renderExperimentList();
    setupRouter();
});

// 加载实验数据
async function loadExperiments() {
    try {
        const response = await fetch('data/experiments.json');
        const data = await response.json();
        experiments = data.experiments || [];
    } catch (error) {
        console.error('加载数据失败:', error);
        experiments = [];
    }
}

// 渲染实验列表
function renderExperimentList() {
    const listElement = document.getElementById('experimentList');

    const html = experiments.map(exp => `
        <div class="experiment-card" onclick="showExperiment('${exp.id}')">
            <h3 class="card-title">${exp.title}</h3>
            <p class="card-summary">${exp.results.summary}</p>
            <div class="card-footer">
                <span class="card-date">📅 ${exp.timestamp}</span>
                <span class="card-link">查看详情 →</span>
            </div>
        </div>
    `).join('');

    listElement.innerHTML = html;
}

// 显示实验详情
function showExperiment(id) {
    const exp = experiments.find(e => e.id === id);
    if (!exp) return;

    currentExperiment = exp;

    // 隐藏列表，显示详情
    document.getElementById('experimentList').style.display = 'none';
    document.getElementById('searchSection').style.display = 'none';

    const detailElement = document.getElementById('experimentDetail');
    detailElement.style.display = 'block';

    // 渲染详情内容
    detailElement.innerHTML = renderExperimentDetail(exp);

    // 渲染图表
    setTimeout(() => renderCharts(exp), 100);

    // 更新 URL
    window.location.hash = `#/experiment/${id}`;
}

// 渲染实验详情
function renderExperimentDetail(exp) {
    return `
        <button class="back-button" onclick="showHome()">← 返回列表</button>

        <div class="detail-header">
            <h1>${exp.title}</h1>
            <span class="detail-date">${exp.timestamp}</span>
        </div>

        <div class="tabs">
            <button class="tab active" onclick="switchTab(event, 'design')">实验设计</button>
            <button class="tab" onclick="switchTab(event, 'objective')">实验目的</button>
            <button class="tab" onclick="switchTab(event, 'method')">研究方法</button>
            <button class="tab" onclick="switchTab(event, 'results')">实验结果</button>
            <button class="tab" onclick="switchTab(event, 'analysis')">数据分析</button>
            <button class="tab" onclick="switchTab(event, 'evaluation')">优劣评估</button>
            <button class="tab" onclick="switchTab(event, 'next')">后续计划</button>
        </div>

        <div class="tab-content">
            <div id="design" class="tab-pane active">
                <h2>📋 实验设计</h2>
                <p>${exp.design}</p>
            </div>

            <div id="objective" class="tab-pane">
                <h2>🎯 实验目的</h2>
                <p>${exp.objective}</p>
            </div>

            <div id="method" class="tab-pane">
                <h2>🔬 研究方法</h2>
                <p>${exp.methodology}</p>
            </div>

            <div id="results" class="tab-pane">
                <h2>📊 实验结果</h2>
                <p>${exp.results.summary}</p>
                <canvas id="resultsChart" width="400" height="200"></canvas>
            </div>

            <div id="analysis" class="tab-pane">
                <h2>📈 数据分析</h2>
                <p>${exp.analysis}</p>
            </div>

            <div id="evaluation" class="tab-pane">
                <h2>⚖️ 优劣评估</h2>
                <div class="evaluation-grid">
                    <div class="strengths">
                        <h3>✅ 优势</h3>
                        <ul>${exp.strengths.map(s => `<li>${s}</li>`).join('')}</ul>
                    </div>
                    <div class="weaknesses">
                        <h3>⚠️ 不足</h3>
                        <ul>${exp.weaknesses.map(w => `<li>${w}</li>`).join('')}</ul>
                    </div>
                </div>
                <div class="improvements">
                    <h3>🔧 改进方向</h3>
                    <ul>${exp.improvements.map(i => `<li>${i}</li>`).join('')}</ul>
                </div>
            </div>

            <div id="next" class="tab-pane">
                <h2>🚀 下一步计划</h2>
                <ul class="next-steps">${exp.nextSteps.map(n => `<li>${n}</li>`).join('')}</ul>
            </div>
        </div>
    `;
}

// 渲染图表
function renderCharts(exp) {
    const ctx = document.getElementById('resultsChart');
    if (!ctx) return;

    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: exp.results.data.map(d => d.name),
            datasets: [{
                label: '实验结果',
                data: exp.results.data.map(d => d.value),
                backgroundColor: 'rgba(59, 130, 246, 0.8)',
                borderColor: 'rgba(59, 130, 246, 1)',
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            plugins: {
                legend: { display: true },
                title: { display: true, text: '实验数据对比' }
            }
        }
    });
}

// 标签页切换
function switchTab(event, tabId) {
    // 移除所有激活状态
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.tab-pane').forEach(p => p.classList.remove('active'));

    // 激活当前标签
    event.target.classList.add('active');
    document.getElementById(tabId).classList.add('active');
}

// 返回首页
function showHome() {
    document.getElementById('experimentList').style.display = 'grid';
    document.getElementById('searchSection').style.display = 'flex';
    document.getElementById('experimentDetail').style.display = 'none';
    window.location.hash = '';
}

// 搜索过滤
function filterExperiments() {
    const query = document.getElementById('searchInput').value.toLowerCase();
    const filtered = experiments.filter(exp =>
        exp.title.toLowerCase().includes(query) ||
        exp.results.summary.toLowerCase().includes(query)
    );

    // 重新渲染
    const listElement = document.getElementById('experimentList');
    listElement.innerHTML = filtered.map(exp => `
        <div class="experiment-card" onclick="showExperiment('${exp.id}')">
            <h3 class="card-title">${exp.title}</h3>
            <p class="card-summary">${exp.results.summary}</p>
            <div class="card-footer">
                <span class="card-date">📅 ${exp.timestamp}</span>
                <span class="card-link">查看详情 →</span>
            </div>
        </div>
    `).join('');
}

// 路由管理
function setupRouter() {
    window.addEventListener('hashchange', handleRoute);
    handleRoute();
}

function handleRoute() {
    const hash = window.location.hash;
    if (hash.startsWith('#/experiment/')) {
        const id = hash.replace('#/experiment/', '');
        showExperiment(id);
    }
}
```

## Version History

- v1.0.0 (2026-03-26): 初始版本
  - ✅ 支持纯 HTML + GitHub Pages（推荐）
  - ✅ 支持 Next.js 14 + Vercel（可选）
  - ✅ 完整的中文界面
  - ✅ Chart.js 数据可视化
  - ✅ 响应式设计
  - ✅ 零依赖部署
  - ✅ 完整的实验信息展示
