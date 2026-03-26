---
name: git-commit-helper
description: Intelligent Git commit assistant that analyzes staged changes and generates conventional commit messages following best practices.
---

# Skill: git-commit-helper

## Description

智能 Git Commit 助手，分析暂存的更改并生成符合规范的 commit message。

## Trigger Conditions

- 用户说 "生成 commit message"
- 用户说 "帮我写 commit"
- 用户请求 "git commit helper"
- 用户说 "提交信息"

## Prerequisites

- 当前目录是一个 Git 仓库
- 有已暂存（staged）的更改

## Instructions

### 步骤 1: 检查 Git 状态

使用 Bash 工具执行：
```bash
git status --short
```

检查是否有暂存的文件。如果没有暂存的更改：
- 提示用户先使用 `git add` 暂存文件
- 或询问是否需要帮助暂存所有更改

### 步骤 2: 查看具体更改

使用 Bash 工具执行：
```bash
git diff --staged
```

分析更改内容，识别：
- 新增的功能（feature）
- 修复的问题（fix）
- 代码重构（refactor）
- 文档更新（docs）
- 样式调整（style）
- 性能优化（perf）
- 测试相关（test）
- 构建或配置（build/chore）

### 步骤 3: 生成 Commit Message

根据分析结果，生成符合 [Conventional Commits](https://www.conventionalcommits.org/) 规范的消息：

格式：
```
<type>(<scope>): <subject>

<body>

<footer>
```

类型说明：
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档更新
- `style`: 代码格式调整（不影响功能）
- `refactor`: 重构代码
- `perf`: 性能优化
- `test`: 测试相关
- `build`: 构建系统或外部依赖
- `ci`: CI 配置
- `chore`: 其他杂项

### 步骤 4: 展示和确认

向用户展示生成的 commit message，包括：
1. 完整的 commit message
2. 简要说明为什么选择这个类型
3. 列出主要更改点

询问用户：
- 是否接受这个 message
- 是否需要修改
- 是否直接执行 commit

### 步骤 5: 执行 Commit（可选）

如果用户确认，使用 Bash 工具执行：
```bash
git commit -m "生成的 message"
```

## Output

控制台输出：
```
📝 分析了 3 个文件的更改
✨ 建议的 Commit Message:

feat(auth): add user authentication system

- Implement JWT token generation
- Add login and registration endpoints
- Create user authentication middleware

是否接受这个 commit message? (yes/no/edit)
```

## Examples

### Example 1: 基础使用

用户输入：
```
git add .
/git-commit-helper
```

系统分析更改并生成 commit message。

### Example 2: 修复 Bug

如果检测到 bug 修复，生成：
```
fix(api): resolve null pointer exception in user handler

- Add null check for user object
- Update error handling logic
```

### Example 3: 添加新功能

如果检测到新功能，生成：
```
feat(dashboard): add real-time data visualization

- Implement WebSocket connection
- Add chart component with live updates
- Update dashboard layout
```

## Error Handling

- **不是 Git 仓库**：提示用户当前目录不是 Git 仓库
- **没有暂存的更改**：提示使用 `git add` 或询问是否查看未暂存的更改
- **diff 过大**：总结主要更改，避免输出过长

## Notes

- 遵循 Conventional Commits 规范
- 生成的 message 应该清晰、简洁
- scope 是可选的，根据项目结构决定
- body 应该说明"做了什么"而不是"怎么做的"
- 如果有 breaking changes，在 footer 中标注 `BREAKING CHANGE:`

## Best Practices

生成 commit message 时应该：

1. **主题行（subject）**
   - 不超过 50 个字符
   - 使用祈使句（"add" 而不是 "added"）
   - 不要以句号结尾

2. **正文（body）**
   - 72 字符换行
   - 说明做了什么和为什么
   - 可以使用列表

3. **页脚（footer）**
   - 引用相关 issue：`Closes #123`
   - 标注破坏性变更：`BREAKING CHANGE: xxx`

## Related Skills

- `/simplify`: 简化代码后的提交
- `/code-review`: 审查后的修复提交

## Version History

- v1.0.0 (2026-03-24): 初始版本
  - 支持分析 git diff
  - 生成 Conventional Commits 格式消息
  - 支持多种 commit 类型识别
