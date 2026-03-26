---
name: skill-writing-guide
description: Comprehensive guide for writing Claude Code skills based on official documentation
type: reference
---

# Claude Code Skill 编写指南

**来源**：https://code.claude.com/docs/en/skills (2026-03-26)

## 核心概念

### 什么是 Skill？

Skill 扩展 Claude 的能力。创建一个 `SKILL.md` 文件并添加指令，Claude 就会将其加入工具箱。Skill 可以：
- 由 Claude 自动加载（当相关时）
- 由用户通过 `/skill-name` 直接调用

### Skills vs Commands

- `.claude/commands/deploy.md` 和 `.claude/skills/deploy/SKILL.md` 都能创建 `/deploy` 命令
- Skills 是推荐的方式，支持更多功能：
  - 支持文件目录（supporting files）
  - Frontmatter 控制调用方式
  - 可以在 subagent 中运行

## 文件结构

### 基本结构

```
~/.claude/skills/my-skill/
├── SKILL.md           # 主文件（必需）
├── template.md        # 模板（可选）
├── examples/
│   └── sample.md      # 示例输出
└── scripts/
    └── validate.sh    # 可执行脚本
```

### Skill 存放位置

| 位置 | 路径 | 适用范围 |
|------|------|---------|
| Enterprise | 见 managed settings | 组织所有用户 |
| Personal | `~/.claude/skills/<name>/SKILL.md` | 所有项目 |
| Project | `.claude/skills/<name>/SKILL.md` | 当前项目 |
| Plugin | `<plugin>/skills/<name>/SKILL.md` | 插件启用处 |

**优先级**：enterprise > personal > project

## YAML Frontmatter 字段

### 完整字段列表

```yaml
---
name: my-skill                      # Skill 名称（小写字母、数字、连字符，最多64字符）
description: What this skill does   # 描述（推荐，帮助 Claude 决定何时使用）
argument-hint: [issue-number]       # 参数提示（在自动完成时显示）
disable-model-invocation: true      # 阻止 Claude 自动调用（默认：false）
user-invocable: true                # 是否在 / 菜单中显示（默认：true）
allowed-tools: Read, Grep, Glob     # Skill 激活时 Claude 可用的工具
model: sonnet                       # 使用的模型
effort: high                        # 努力级别：low, medium, high, max
context: fork                       # 在 subagent 中运行
agent: Explore                      # Subagent 类型
hooks: ...                          # Skill 生命周期钩子
paths: src/**/*.ts                  # 限制激活路径的 glob 模式
shell: bash                         # Shell 类型：bash 或 powershell
---

Skill 指令内容...
```

### 关键字段说明

#### `name`（可选）
- 显示名称
- 如果省略，使用目录名
- 规则：小写字母、数字、连字符，最多 64 字符

#### `description`（推荐）
- 描述 Skill 的功能和使用时机
- Claude 用这个决定何时应用 Skill
- 如果省略，使用 markdown 内容的第一段

#### `disable-model-invocation`（默认：false）
- 设为 `true`：只能手动调用（`/skill-name`），Claude 不会自动加载
- 用于有副作用的操作：部署、提交、发送消息等

#### `user-invocable`（默认：true）
- 设为 `false`：从 `/` 菜单隐藏
- 用于背景知识，用户不应直接调用

#### `allowed-tools`
- Claude 在此 Skill 激活时可以无需许可使用的工具
- 示例：`Read, Grep, Glob, Bash(git *)`

#### `context: fork`
- 在隔离的 subagent 中运行
- Skill 内容成为 subagent 的任务提示
- **重要**：只适用于包含明确任务的 Skill

#### `agent`
- 与 `context: fork` 配合使用
- 指定 subagent 类型：`Explore`, `Plan`, `general-purpose` 或自定义
- 如果省略，使用 `general-purpose`

#### `paths`
- Glob 模式限制激活时机
- 只在处理匹配文件时自动加载
- 格式：逗号分隔字符串或 YAML 列表

## 调用控制

### 谁可以调用 Skill？

| Frontmatter | 你可以调用 | Claude 可以调用 | 何时加载到上下文 |
|-------------|-----------|----------------|----------------|
| （默认） | 是 | 是 | Description 始终在上下文，调用时加载完整 Skill |
| `disable-model-invocation: true` | 是 | 否 | Description 不在上下文，你调用时加载完整 Skill |
| `user-invocable: false` | 否 | 是 | Description 始终在上下文，调用时加载完整 Skill |

## Skill 内容类型

### 1. 参考内容（Reference Content）

添加 Claude 应用到工作中的知识：约定、模式、风格指南、领域知识。
内容在主会话中内联运行。

```yaml
---
name: api-conventions
description: API design patterns for this codebase
---

When writing API endpoints:
- Use RESTful naming conventions
- Return consistent error formats
- Include request validation
```

### 2. 任务内容（Task Content）

给 Claude 分步骤指令执行特定操作：部署、提交、代码生成。
通常需要直接调用，添加 `disable-model-invocation: true`。

```yaml
---
name: deploy
description: Deploy the application to production
context: fork
disable-model-invocation: true
---

Deploy the application:
1. Run the test suite
2. Build the application
3. Push to the deployment target
```

## 参数替换

### 可用变量

| 变量 | 说明 |
|------|------|
| `$ARGUMENTS` | 调用时传递的所有参数 |
| `$ARGUMENTS[N]` | 第 N 个参数（从 0 开始） |
| `$N` | `$ARGUMENTS[N]` 的简写（`$0`, `$1`, ...） |
| `${CLAUDE_SESSION_ID}` | 当前会话 ID |
| `${CLAUDE_SKILL_DIR}` | Skill 目录路径 |

### 示例

```yaml
---
name: migrate-component
description: Migrate a component from one framework to another
---

Migrate the $0 component from $1 to $2.
Preserve all existing behavior and tests.
```

调用：`/migrate-component SearchBar React Vue`
- `$0` = SearchBar
- `$1` = React
- `$2` = Vue

## 高级功能

### 1. 动态上下文注入

使用 `` !`<command>` `` 语法在 Skill 发送给 Claude 之前运行 shell 命令。

```yaml
---
name: pr-summary
description: Summarize changes in a pull request
context: fork
agent: Explore
allowed-tools: Bash(gh *)
---

## Pull request context
- PR diff: !`gh pr diff`
- PR comments: !`gh pr view --comments`
- Changed files: !`gh pr diff --name-only`

## Your task
Summarize this pull request...
```

执行流程：
1. 每个 `` !`<command>` `` 立即执行
2. 输出替换占位符
3. Claude 接收完整渲染后的提示

### 2. Subagent 执行

添加 `context: fork` 在隔离环境中运行：

```yaml
---
name: deep-research
description: Research a topic thoroughly
context: fork
agent: Explore
---

Research $ARGUMENTS thoroughly:
1. Find relevant files using Glob and Grep
2. Read and analyze the code
3. Summarize findings with specific file references
```

**关键点**：
- Skill 内容成为 subagent 的任务
- Subagent 没有主会话的历史记录
- 只适用于包含明确指令的 Skill

### 3. 扩展思考模式

在 Skill 内容中包含 "ultrathink" 关键词即可启用扩展思考。

### 4. 支持文件

保持 `SKILL.md` 简洁（< 500 行），将详细内容放到单独文件：

```markdown
## Additional resources

- For complete API details, see [reference.md](reference.md)
- For usage examples, see [examples.md](examples.md)
```

Claude 会在需要时加载这些文件。

### 5. 生成可视化输出

Skill 可以捆绑和运行任何语言的脚本，生成交互式 HTML 文件。

示例结构：
```
codebase-visualizer/
├── SKILL.md
└── scripts/
    └── visualize.py
```

在 SKILL.md 中调用脚本：
```bash
python ${CLAUDE_SKILL_DIR}/scripts/visualize.py .
```

## 最佳实践

### ✅ 推荐做法

1. **描述要具体**：包含用户自然会说的关键词
2. **单一职责**：每个 Skill 专注一个任务
3. **文档完整**：提供清晰的说明和示例
4. **路径限制**：使用 `paths` 只在相关文件时激活
5. **合理拆分**：SKILL.md < 500 行，详细内容用支持文件
6. **命名规范**：小写字母、连字符，语义清晰

### ❌ 避免的做法

1. 不要在 description 中包含不相关的关键词
2. 不要让一个 Skill 做太多事情
3. 不要忘记添加 frontmatter
4. 有副作用的操作要设置 `disable-model-invocation: true`
5. 背景知识类 Skill 设置 `user-invocable: false`

## Skill 模式示例

### 1. 只读探索 Skill

```yaml
---
name: safe-reader
description: Read files without making changes
allowed-tools: Read, Grep, Glob
---

Explore the codebase without modifying anything...
```

### 2. 手动部署 Skill

```yaml
---
name: deploy
description: Deploy to production
disable-model-invocation: true
context: fork
---

1. Run tests
2. Build application
3. Deploy to target
4. Verify deployment
```

### 3. 自动加载参考 Skill

```yaml
---
name: coding-standards
description: Coding standards and best practices for this project
paths: src/**/*.ts
---

When writing TypeScript code in this project:
- Use strict type checking
- Follow naming conventions...
```

### 4. 研究型 Skill（Subagent）

```yaml
---
name: research-topic
description: Deep dive into a specific topic
context: fork
agent: Explore
---

Research $ARGUMENTS:
1. Search for relevant files
2. Read and analyze
3. Provide detailed summary with references
```

### 5. PR 总结 Skill（动态上下文）

```yaml
---
name: pr-summary
description: Summarize a pull request
allowed-tools: Bash(gh *)
---

PR Diff:
!`gh pr diff`

Comments:
!`gh pr view --comments`

Summarize the changes and their impact.
```

## 故障排除

### Skill 不触发

1. 检查 description 是否包含用户会说的关键词
2. 验证 Skill 出现在 `/` 菜单中
3. 尝试直接调用 `/skill-name`
4. 重新表述请求以匹配 description

### Skill 触发太频繁

1. 使 description 更具体
2. 添加 `disable-model-invocation: true` 只允许手动调用

### Claude 看不到所有 Skill

Skill descriptions 占用上下文预算（2% 的上下文窗口，最少 16,000 字符）。
运行 `/context` 检查是否有警告。

可以设置环境变量覆盖限制：
```bash
export SLASH_COMMAND_TOOL_CHAR_BUDGET=32000
```

## 限制 Skill 访问

三种方式控制 Claude 调用哪些 Skill：

### 1. 禁用所有 Skill

在 `/permissions` 中拒绝 Skill 工具：
```
Skill
```

### 2. 允许/拒绝特定 Skill

```
# 只允许特定 Skill
Skill(commit)
Skill(review-pr *)

# 拒绝特定 Skill
Skill(deploy *)
```

### 3. 隐藏单个 Skill

在 frontmatter 中添加：
```yaml
disable-model-invocation: true
```

## Why: 选择合适的 Frontmatter

### 何时使用 `disable-model-invocation: true`？

- 有副作用的操作（部署、提交、发送消息）
- 需要明确用户控制时机的操作
- 不希望 Claude 自动决定执行的任务

### 何时使用 `user-invocable: false`？

- 纯背景知识（系统约定、遗留系统文档）
- 用户不应直接调用的参考内容
- Claude 应该知道但不是可执行命令的信息

### 何时使用 `context: fork`？

- 需要隔离执行环境的任务
- 长时间运行的研究或分析
- 不需要访问主会话历史的操作
- 明确的自包含任务

### 何时使用 `paths`？

- 只与特定文件类型相关的 Skill
- 语言或框架特定的约定
- 项目特定区域的规则

## 相关资源

- **Subagents**：委托任务给专门的 agent
- **Plugins**：打包和分发 Skill 及其他扩展
- **Hooks**：围绕工具事件自动化工作流
- **Memory**：管理 CLAUDE.md 文件以获得持久上下文
- **Permissions**：控制工具和 Skill 访问
