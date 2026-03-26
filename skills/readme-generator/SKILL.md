---
name: readme-generator
description: Intelligent README.md generator that analyzes project structure and code to automatically generate comprehensive project documentation with installation, usage, and contribution guidelines.
---

# Skill: readme-generator

## Description

智能 README.md 生成器，分析项目结构和代码，自动生成完整的项目文档。

## Trigger Conditions

- 用户说 "生成 README"
- 用户说 "创建项目文档"
- 用户请求 "generate readme"
- 用户说 "写 README.md"

## Instructions

### 步骤 1: 分析项目结构

使用 Glob 工具扫描项目文件：
```
- 查找主要代码文件（src/, lib/, app/ 等）
- 查找配置文件（package.json, requirements.txt, go.mod 等）
- 查找文档文件（docs/, LICENSE, CONTRIBUTING.md 等）
- 查找测试文件（tests/, __tests__/, *.test.* 等）
```

### 步骤 2: 识别项目类型和技术栈

根据发现的文件判断：
- **Node.js**: package.json 存在
- **Python**: requirements.txt 或 setup.py 存在
- **Go**: go.mod 存在
- **Rust**: Cargo.toml 存在
- **Java**: pom.xml 或 build.gradle 存在

读取配置文件获取：
- 项目名称
- 版本号
- 依赖包
- 脚本命令

### 步骤 3: 分析主要功能

使用 Read 工具读取主入口文件（如 main.js, app.py, main.go 等）：
- 识别主要模块和功能
- 查找导出的 API
- 识别命令行参数或配置选项

### 步骤 4: 生成 README 内容

创建包含以下部分的 README.md：

1. **项目标题和描述**
   - 项目名称
   - 简短描述（一句话概括）
   - 徽章（如果适用）

2. **功能特性**
   - 核心功能列表
   - 技术亮点

3. **安装说明**
   - 系统要求
   - 安装步骤
   - 依赖安装命令

4. **使用方法**
   - 快速开始示例
   - 基本用法
   - 配置选项

5. **API 文档**（如果是库项目）
   - 主要 API 列表
   - 使用示例

6. **开发指南**
   - 开发环境搭建
   - 运行测试
   - 构建命令

7. **贡献指南**
   - 如何贡献
   - 代码规范

8. **许可证**
   - 许可证信息

### 步骤 5: 询问用户定制

使用 AskUserQuestion 询问：
- 项目的主要目标是什么？
- 是否需要添加截图或示例？
- 是否有特殊的使用场景需要说明？
- 项目的独特卖点是什么？

### 步骤 6: 创建 README 文件

使用 Write 工具创建 `README.md` 文件，整合所有信息。

### 步骤 7: 输出总结

告知用户：
- README.md 已创建
- 包含的主要部分
- 建议检查和补充的内容

## Output

生成的 README.md 文件示例结构：

```markdown
# 项目名称

> 简短的项目描述

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)]()

## ✨ 特性

- 特性 1
- 特性 2
- 特性 3

## 📦 安装

\`\`\`bash
npm install project-name
\`\`\`

## 🚀 快速开始

\`\`\`javascript
const project = require('project-name');
project.run();
\`\`\`

## 📖 使用方法

详细的使用说明...

## 🛠️ 开发

\`\`\`bash
# 安装依赖
npm install

# 运行测试
npm test

# 构建
npm run build
\`\`\`

## 🤝 贡献

欢迎提交 Pull Request！

## 📄 许可证

MIT License
```

## Examples

### Example 1: Node.js 项目

用户输入：
```
/readme-generator
```

检测到 package.json，生成包含 npm 命令的 README。

### Example 2: Python 项目

检测到 requirements.txt 和 setup.py，生成包含 pip 安装说明的 README。

### Example 3: CLI 工具项目

检测到命令行工具，生成包含命令用法和选项说明的 README。

## Templates

根据项目类型使用不同模板：

### Node.js 库模板
```markdown
## 安装
\`\`\`bash
npm install package-name
\`\`\`

## 使用
\`\`\`javascript
const pkg = require('package-name');
\`\`\`
```

### Python 应用模板
```markdown
## 安装
\`\`\`bash
pip install -r requirements.txt
\`\`\`

## 运行
\`\`\`bash
python main.py
\`\`\`
```

### CLI 工具模板
```markdown
## 安装
\`\`\`bash
npm install -g tool-name
\`\`\`

## 命令
\`\`\`bash
tool-name --help
tool-name [options] <command>
\`\`\`
```

## Error Handling

- **无法识别项目类型**：生成通用模板，提示用户手动填充
- **缺少关键信息**：询问用户补充必要信息
- **README.md 已存在**：询问是否覆盖或创建 README.new.md

## Notes

- 生成的 README 是起点，需要人工审查和补充
- 对于复杂项目，可能需要多次迭代完善
- 建议添加实际的使用示例和截图
- 保持 README 简洁，详细文档放在 docs/ 目录

## Customization

可以通过以下方式定制：

1. **添加徽章**
   - GitHub Actions 状态
   - 代码覆盖率
   - npm 版本
   - 下载量统计

2. **包含示例**
   - 代码示例
   - 截图
   - GIF 演示

3. **多语言支持**
   - 提供多语言版本链接
   - 创建 README.zh-CN.md 等

## Best Practices

好的 README 应该：

1. **开头 30 秒原则**：用户应该在 30 秒内理解项目是什么
2. **清晰的安装步骤**：一步步说明，避免假设
3. **实际示例**：提供真实可运行的代码示例
4. **视觉元素**：适当使用截图、图表、GIF
5. **持续更新**：随项目发展保持 README 更新

## Related Skills

- `/init-project`: 初始化项目后生成 README
- `/code-review`: 审查代码后更新文档

## Version History

- v1.0.0 (2026-03-24): 初始版本
  - 支持多种项目类型识别
  - 自动生成标准 README 结构
  - 提供多种模板
