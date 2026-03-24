# Claude Skills 仓库

这是一个自定义 Claude Code Skills 的仓库，用于存放和管理个人的 Claude 技能脚本。

## 什么是 Claude Skills？

Claude Skills 是 Claude Code 中的可复用任务模板，通过 `/skill-name` 的方式调用。Skills 可以封装复杂的工作流程，实现自动化任务。

## 使用方法

### 1. 克隆仓库

```bash
git clone https://github.com/yourusername/skills.git
cd skills
```

### 2. 复制 Skills 到 Claude 配置目录

```bash
# 复制单个 skill
cp skills/your-skill-name ~/.claude/skills/

# 或复制所有 skills
cp -r skills/* ~/.claude/skills/
```

### 3. 在 Claude Code 中使用

在 Claude Code 中直接调用：

```
/your-skill-name
```

## 目录结构

```
skills/
├── README.md                 # 本文件
├── template/                 # Skill 模板
│   └── skill-template.md     # 基础模板
└── skills/                   # 自定义 Skills 目录
    └── example-skill/        # 示例 skill
        └── skill.md          # Skill 定义文件
```

## 创建新 Skill

### 方法一：使用模板

1. 复制模板目录：
```bash
cp -r template/skill-template skills/my-new-skill
```

2. 编辑 `skills/my-new-skill/skill.md`，填写技能内容

3. 将 skill 复制到 Claude 配置目录

### 方法二：手动创建

创建一个新的 Markdown 文件 `skills/my-skill/skill.md`，包含以下基本结构：

```markdown
# Skill: my-skill

## Description
简短描述这个 skill 的功能

## Trigger Conditions
- 关键词 1
- 关键词 2

## Instructions
详细的执行步骤和逻辑
```

## Skill 文件格式

一个标准的 skill 文件应包含：

1. **标题**：清晰的 skill 名称
2. **Description**：功能描述
3. **Trigger Conditions**（可选）：触发条件，帮助 Claude 判断何时使用
4. **Instructions**：详细的执行指令
5. **Examples**（可选）：使用示例

## 示例 Skills

查看 `skills/example-skill/` 目录获取完整示例。

## 最佳实践

1. **命名规范**：使用小写字母和连字符，如 `my-skill-name`
2. **清晰的触发条件**：明确说明何时应该使用这个 skill
3. **模块化**：每个 skill 专注于单一任务
4. **文档完整**：提供清晰的说明和示例
5. **版本控制**：使用 git 管理 skill 的版本

## 更新 Skills

修改 skill 后，重新复制到 Claude 配置目录：

```bash
cp skills/your-skill-name/skill.md ~/.claude/skills/your-skill-name.md
```

## 贡献

欢迎提交 Pull Request 分享你的 skills！

## 许可

MIT License
