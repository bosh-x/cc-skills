# Skill: modify-first

## Description

Prevent Claude from creating duplicate files. Always check if files exist before writing, and update existing files instead of creating new ones.
<!-- 防止 Claude 创建重复文件。写入前始终检查文件是否存在，更新现有文件而不是创建新文件 -->

## ⚠️ Critical Problem This Solves

User frequently experiences:
<!-- 用户经常遇到的问题： -->
- Claude generates "a bunch of markdown files" instead of updating existing ones
<!-- Claude 生成"一堆 markdown 文件"而不是更新现有文件 -->
- Multiple versions of same file (README-new.md, README-v2.md, etc.)
<!-- 同一文件的多个版本（README-new.md、README-v2.md 等） -->
- Recreating entire files when only sections need updates
<!-- 仅需更新部分内容时重建整个文件 -->
- File clutter and confusion about which file is current
<!-- 文件混乱，不知道哪个是最新的 -->

**Solution: ALWAYS update existing files, NEVER create duplicates**
<!-- 解决方案：总是更新现有文件，绝不创建重复文件 -->

## Trigger Conditions

- **ALWAYS active** when creating or writing ANY file (mandatory)
<!-- 创建或写入任何文件时始终激活（强制性） -->
- **ESPECIALLY critical** for markdown (.md) files
<!-- 对 markdown（.md）文件特别关键 -->
- User says "don't create duplicates" / "不要重复创建文件"
- User says "update existing files" / "更新现有文件"
- User says "stop making duplicate files"

## File Operation Rules

**CRITICAL - Apply to EVERY file operation:**
<!-- 关键规则 - 应用于每次文件操作 -->

1. **NEVER create duplicate files**
   <!-- 绝不创建重复文件 -->

2. **ALWAYS check if file exists before creating**
   <!-- 创建前始终检查文件是否存在 -->

3. **If exists → update instead of recreate**
   <!-- 如果存在 → 更新而不是重新创建 -->

4. **Use diff/patch style when modifying** (Edit tool, not Write)
   <!-- 修改时使用 diff/patch 风格（使用 Edit 工具，而非 Write） -->

5. **Keep single source of truth** (one README.md, not multiple versions)
   <!-- 保持单一事实来源（一个 README.md，而非多个版本） -->

6. **Do NOT create `*_v2`, `*_new`, `*_updated` files**
   <!-- 不要创建 `*_v2`、`*_new`、`*_updated` 文件 -->

## Core Principles

1. **Check First, Write Second**: Always use Glob/Read before Write
<!-- 先检查，后写入：Write 之前总是先 Glob/Read -->

2. **Edit Over Write**: If file exists, use Edit (diff-style) instead of Write (full overwrite)
<!-- Edit 优于 Write：文件存在时，使用 Edit（diff 风格）而非 Write（完全覆盖） -->

3. **Ask Before Overwrite**: If complete rewrite is necessary, ask user permission first
<!-- 覆盖前询问：如果必须完全重写，先征得用户许可 -->

4. **Single Source of Truth**: Consolidate into existing files, never create variants
<!-- 单一事实来源：整合到现有文件，绝不创建变体 -->

## Instructions

### Step 1: Before Creating ANY File

When you need to create or write a file, ALWAYS:
<!-- 当需要创建或写入文件时，务必： -->

```markdown
1. Use Glob to check if similar files exist:
   - Search for `filename.*`
   - Search for `*keyword*.md` if it's documentation
   - Check the target directory

2. If files found:
   - Read the existing file
   - Determine if you should:
     a) Edit the existing file (preferred)
     b) Ask user if they want to replace it
     c) Create with a different, specific name

3. If no files found:
   - Proceed to create the new file
```

### Step 2: Decision Tree for File Operations
<!-- 文件操作决策树 -->

```
Need to write content?
  │
  ├─ File exists?
  │   │
  │   ├─ YES → Can update part of it?
  │   │   │
  │   │   ├─ YES → Use Edit tool
  │   │   │         ✓ Update specific sections
  │   │   │         ✓ Keep existing content
  │   │   │
  │   │   └─ NO → Must completely rewrite?
  │   │             │
  │   │             ├─ Use AskUserQuestion:
  │   │             │   "File X exists. Replace it? (yes/no)"
  │   │             │
  │   │             ├─ User says YES → Use Write
  │   │             └─ User says NO → Ask for new filename
  │   │
  │   └─ NO → Use Write to create new file
```

### Step 3: Handling Markdown Files (CRITICAL)
<!-- 处理 Markdown 文件（关键） -->

**⚠️ SPECIAL ATTENTION: User frequently gets unwanted duplicate markdown files**
<!-- 特别注意：用户经常收到不需要的重复 markdown 文件 -->

For markdown files (README.md, docs/*.md, *.md):
<!-- 对于 markdown 文件： -->

**MANDATORY checks before ANY markdown file operation:**
<!-- 任何 markdown 文件操作前的必需检查： -->

1. **ALWAYS check first** (non-negotiable):
```bash
# Check patterns
Glob: "README*.md"        # Find all README variants
Glob: "docs/**/*.md"       # Find all docs
Glob: "**/GUIDE*.md"       # Find guides
Glob: "**/*{keyword}*.md"  # Find by keyword
```

2. **If ANY markdown file exists**:
   - ✅ **DO**: Read it first, then Edit specific sections
   - ✅ **DO**: Use diff-style updates (Edit tool)
   - ✅ **DO**: Preserve existing structure and content
   - ❌ **DON'T**: Create README-new.md, README-v2.md, README_updated.md
   - ❌ **DON'T**: Use Write to recreate the entire file
   - ❌ **DON'T**: Create parallel versions

3. **Default action for markdown**:
   - File exists? → **ALWAYS Edit, NEVER Write**
   <!-- 文件存在？→ 总是 Edit，绝不 Write -->
   - Need new section? → **Add via Edit, don't recreate file**
   <!-- 需要新章节？→ 通过 Edit 添加，不要重建文件 -->
   - Need to reorganize? → **Edit section by section**
   <!-- 需要重组？→ 逐节 Edit -->

4. **If must ask user**:
   - "I found existing {filename}. I'll update it with Edit instead of creating a new file. OK?"
   <!-- "发现现有 {文件名}。我将用 Edit 更新它而不是创建新文件。可以吗？" -->
   - Default assumption: user wants updates, not duplicates
   <!-- 默认假设：用户想要更新，而非重复文件 -->

### Step 4: Handling Code Files
<!-- 处理代码文件 -->

For source code files (.js, .py, .go, etc.):
<!-- 对于源代码文件： -->

1. **Check if file exists**:
```bash
# Use Glob to find
Glob: "src/**/*.js"
Glob: "**/*component*.tsx"
```

2. **If exists**:
   - Read the file to understand current structure
   - Use Edit to add/modify functions or classes
   - Preserve existing code unless explicitly asked to replace

3. **If adding new functionality**:
   - Add to existing file if related
   - Only create new file if it's a new module/component

### Step 5: Prevention Strategies
<!-- 预防策略 -->

**Before ANY Write operation**:
<!-- 在任何 Write 操作之前： -->

```markdown
STOP and ask yourself:
1. Did I check if this file exists? (Use Glob/Read)
   <!-- 我检查过这个文件是否存在了吗？ -->

2. If it exists, can I Edit instead of Write?
   <!-- 如果存在，我能用 Edit 而不是 Write 吗？ -->

3. Is this truly a NEW file or an update to existing?
   <!-- 这真的是新文件还是对现有文件的更新？ -->

4. Have I asked the user if overwrite is needed?
   <!-- 如果需要覆盖，我询问过用户了吗？ -->
```

**Use this checklist**:
```
[ ] Searched for existing files with Glob
[ ] Read existing file if found
[ ] Determined: Edit vs Write vs Ask User
[ ] If Write: Confirmed file is truly new OR user approved overwrite
```

## Special Cases
<!-- 特殊情况 -->

### Case 1: Multiple Similar Files Already Exist
<!-- 情况 1：已存在多个相似文件 -->

Example: README.md, README-old.md, README-backup.md
<!-- 示例：README.md, README-old.md, README-backup.md -->

**Action**:
```
1. List all found files to user
2. Ask: "I found multiple README files. Which one should I update?"
   <!-- 询问："发现多个 README 文件。我应该更新哪一个？" -->
3. Wait for user decision
4. Update the chosen file, optionally offer to clean up others
   <!-- 更新选定的文件，可选择性地清理其他文件 -->
```

### Case 2: User Explicitly Wants a New File
<!-- 情况 2：用户明确想要新文件 -->

Example: "Create a NEW design.md, don't touch the existing one"
<!-- 示例："创建一个新的 design.md，不要动现有的" -->

**Action**:
```
1. Acknowledge user's explicit request
2. Ask for specific filename: "What should I name the new file?"
   <!-- 询问具体文件名："新文件应该叫什么名字？" -->
3. Suggest: design-v2.md, design-2026.md, design-proposal.md
4. Create with the specified name
```

### Case 3: Batch File Creation
<!-- 情况 3：批量创建文件 -->

Example: Creating multiple documentation files
<!-- 示例：创建多个文档文件 -->

**Action**:
```
1. Before starting batch creation, check ALL target files
   <!-- 开始批量创建前，检查所有目标文件 -->
2. Report: "Found X existing files, Y will be new"
   <!-- 报告："发现 X 个现有文件，Y 个将是新文件" -->
3. Ask: "Update existing files? (yes/no/ask-each)"
   <!-- 询问："更新现有文件吗？（是/否/逐个询问）" -->
4. Proceed based on user choice
```

## Output Format
<!-- 输出格式 -->

When handling files, always communicate:
<!-- 处理文件时，始终传达： -->

```markdown
📁 File Check: filename.md
  ├─ Status: ✓ Exists / ✗ Not found
  ├─ Action: Edit / Write / Skip
  └─ Reason: [explanation]
  <!-- 状态：存在/未找到；操作：编辑/写入/跳过；原因：[说明] -->
```

Example output:
```
📁 File Check: README.md
  ├─ Status: ✓ Exists
  ├─ Action: Edit (updating "Installation" section)
  └─ Reason: File already has good structure, only updating one section

📁 File Check: docs/API.md
  ├─ Status: ✗ Not found
  ├─ Action: Write (creating new file)
  └─ Reason: No existing API documentation found
```

## Common Mistakes to Avoid
<!-- 要避免的常见错误 -->

### ❌ **NEVER DO THESE** (Especially with Markdown):
<!-- 绝对不要做这些（特别是 Markdown 文件） -->

**Creating duplicate/versioned files:**
- ❌ `README-new.md`, `README-updated.md`, `README_v2.md`
- ❌ `GUIDE-2.md`, `API-revised.md`, `CHANGELOG-latest.md`
- ❌ `docs-new/`, `documentation_backup/`
- ❌ ANY file with suffix: `_new`, `_v2`, `_updated`, `-revised`, `-latest`, `-copy`

**Writing without checking:**
- ❌ Use Write without Glob/Read first
- ❌ Overwrite files without asking
- ❌ Generate "a bunch of markdown files" when one update would work
<!-- 当一次更新就够时生成"一堆 markdown 文件" -->

**Recreating instead of updating:**
- ❌ Use Write when Edit would work
- ❌ Delete and recreate instead of modify
- ❌ Assume fresh file is better than updating existing

### ✅ **ALWAYS DO THESE**:
<!-- 总是这样做 -->

**For every file operation:**
- ✅ Glob/Read before Write (100% of the time)
- ✅ Use Edit for existing files (especially .md)
- ✅ Keep single source of truth
- ✅ Ask user before destructive operations
- ✅ Clean up duplicates when found

**For markdown files specifically:**
- ✅ Always update existing, never create variants
- ✅ Use Edit to modify sections incrementally
- ✅ Preserve existing structure and content
- ✅ Consolidate related content into one file

## Integration with Other Operations
<!-- 与其他操作的集成 -->

This skill should be active during:
<!-- 此 skill 应该在以下情况下激活： -->

- `/readme-generator` - Check if README.md exists first
<!-- 先检查 README.md 是否存在 -->
- `/init-project` - Verify directory is truly empty
<!-- 验证目录确实为空 -->
- Documentation generation - Update existing docs
<!-- 文档生成 - 更新现有文档 -->
- Code scaffolding - Check for existing components
<!-- 代码脚手架 - 检查现有组件 -->

## Examples

### Example 1: Updating README
<!-- 示例 1：更新 README -->

```
User: "Add installation instructions to the README"

Claude:
1. Glob: "README*.md" → Found: README.md
2. Read: README.md → Has structure, missing "Installation" section
3. Edit: Add "Installation" section between "Features" and "Usage"
✓ Updated existing README.md (not created new file)
```

### Example 2: Creating New Documentation
<!-- 示例 2：创建新文档 -->

```
User: "Create API documentation"

Claude:
1. Glob: "docs/**/API*.md" → Not found
2. Glob: "API*.md" → Not found
3. Write: docs/API.md
✓ Created new file (verified it doesn't exist)
```

### Example 3: Handling Duplicates
<!-- 示例 3：处理重复 -->

```
User: "Generate contributing guide"

Claude:
1. Glob: "*CONTRIB*.md" → Found: CONTRIBUTING.md, CONTRIBUTING-old.md
2. Ask: "Found 2 files:
   - CONTRIBUTING.md (modified 2 days ago)
   - CONTRIBUTING-old.md (modified 30 days ago)

   Should I update CONTRIBUTING.md? (yes/no/delete-old)"

User: "yes, and delete the old one"

3. Edit: CONTRIBUTING.md
4. Bash: rm CONTRIBUTING-old.md
✓ Updated main file and cleaned up duplicate
```

### Example 4: User Wants Separate File
<!-- 示例 4：用户想要独立文件 -->

```
User: "Create a new design document, separate from existing one"

Claude:
1. Glob: "design*.md" → Found: design.md
2. Ask: "design.md exists. What should I name the new one?
   Suggestions:
   - design-v2.md
   - design-proposal-2026.md
   - design-alternative.md"

User: "design-alternative.md"

3. Write: design-alternative.md
✓ Created with specific name per user request
```

## Error Recovery
<!-- 错误恢复 -->

If you accidentally created a duplicate:
<!-- 如果意外创建了重复文件： -->

```markdown
1. Acknowledge the mistake:
   "I apologize, I created X when Y already existed."
   <!-- 承认错误："抱歉，我在 Y 已存在时创建了 X。" -->

2. Offer to fix:
   "Should I:
   a) Merge X into Y and delete X
   b) Keep both files
   c) Replace Y with X"
   <!-- 提供修复选项 -->

3. Execute user's choice
4. Remember to check next time
```

## Metrics
<!-- 指标 -->

Track and minimize:
<!-- 跟踪并最小化： -->
- Number of duplicate files created per session
<!-- 每次会话创建的重复文件数 -->
- Number of unnecessary overwrites
<!-- 不必要的覆盖次数 -->
- User corrections about file handling
<!-- 用户关于文件处理的更正次数 -->

## Related Skills

- `/simplify` - May generate files, apply this skill first
- `/readme-generator` - Must check before creating
- Any code generation skill - Check before creating components

## Quick Reference: File Operation Workflow
<!-- 快速参考：文件操作工作流 -->

```
Before ANY file operation:
  │
  1. Glob/Read → Check if file exists
  │
  ├─ Exists?
  │  │
  │  ├─ YES → Use Edit (diff/patch style)
  │  │        ✓ Update only what's needed
  │  │        ✓ Preserve existing content
  │  │        ✓ Single source of truth
  │  │
  │  └─ Must completely replace?
  │     └─ Ask user first → then Write
  │
  └─ NO → OK to Write (new file)
           ✓ Verified file doesn't exist
```

**Remember: Edit > Write (for existing files)**
<!-- 记住：Edit > Write（对于现有文件） -->

## Version History

- v1.1.0 (2026-03-24): Enhanced for markdown files
  - Added 6 critical file operation rules
  <!-- 添加 6 条关键文件操作规则 -->
  - Special emphasis on markdown file handling
  <!-- 特别强调 markdown 文件处理 -->
  - Explicit prohibition of *_v2, *_new, *_updated patterns
  <!-- 明确禁止 *_v2、*_new、*_updated 模式 -->
  - Added quick reference workflow
  <!-- 添加快速参考工作流 -->

- v1.0.0 (2026-03-24): Initial version
  - File existence checking workflow
  - Edit-over-Write preference
  - Duplicate prevention strategies
  - User confirmation patterns
