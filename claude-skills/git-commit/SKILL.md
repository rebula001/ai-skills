---
name: git-commit
description: 用于检查指定改动范围，起草英文 Conventional Commit 文案，并在用户明确确认后执行提交。适用于提交全部改动、指定文件、当前文件、当前文件夹，或提交 skills 子模块相关改动。
disable-model-invocation: true
---

# Git Commit

## When To Use
- 当用户说“请提交”时使用
- 当用户说“请提交 skills”并希望提交当前项目中的 skills 相关改动时使用
- 当用户说“请提交这些”并指定了一组文件时使用
- 当用户说“请提交这个文件”并且当前文件上下文明确时使用
- 当用户说“请提交这个文件夹”并且当前文件夹上下文明确时使用
- 当用户希望先查看 commit 标题和描述，再决定是否提交时使用

## Core Behavior
- 提交前先检查变更范围，不要直接提交
- 一个 commit message 包括 `title` 和可选 `description`
- commit 的 `title` 和 `description` 必须使用英文
- commit message 必须遵循 Conventional Commits 风格
- 默认先检查未暂存和已暂存改动，再生成 commit 文案
- 默认先检查当前所在分支名称，并明确本次提交只会发生在当前分支
- 在用户确认前，只生成建议的 commit message 和待提交范围
- 即使用户的第一句话是“请提交”，也只表示“开始提交流程”，不表示“已经确认可以执行提交”
- 在任何情况下，都必须先把 `current branch`、`proposed title` 和 `proposed description` 展示给用户
- 在任何情况下，都必须明确询问用户是否确认提交到当前分支
- 只有在用户明确确认后，才执行 `git add` 和 `git commit`
- 默认提交到当前仓库当前分支
- 不自动 push 到远程仓库，push 由用户自行执行

## Confirmation Rules
- “请提交”“commit it”“提交一下”这类初始请求，不视为最终确认，只视为启动 commit 准备流程
- 以下输入默认视为确认提交：
  - `y`
  - `ok`
  - `go`
  - `提交`
  - `好的`
  - `好`
  - `确认`
- 以下输入默认视为取消或拒绝提交：
  - `n`
  - `no`
  - `cancel`
  - `stop`
  - `不要`
  - `不`
  - `取消`
  - `不提交`
- 只有当这些词明确针对当前 commit 确认流程时，才视为有效确认或取消
- 如果用户回复含义不明确，应继续等待明确确认，而不要直接提交
- 如果还没有先展示当前分支、拟提交范围和 commit message，则不得把任何简短回复解释为有效确认

## Scope Defaults
- “已更改的文件” 默认包括：
  - 已跟踪且已修改的文件
  - 已暂存的文件
  - 新增但尚未跟踪的文件
- `.gitignore` 中已定义排除的文件和文件夹，不纳入读取、分析或提交范围
- “提交 skills” 默认表示提交当前项目中的 skills 子模块改动，也就是提交 submodule 指针更新；如果用户明确进入子模块仓库上下文，则按该子模块仓库内的改动处理
- “当前文件” 默认指 IDE 当前活动文件
- “当前文件夹” 默认指当前活动文件所在目录；如果用户显式指定某个文件夹，则以用户指定为准
- 如果当前上下文无法唯一确定目标文件或文件夹，再向用户确认

## Scope Interpretation

### “请提交”
- 查看当前工作区中所有已更改的文件
- 这包括 tracked、staged 和 untracked 的改动
- 同时检查当前分支名称
- 总结本次变更涉及的主要内容
- 基于实际改动起草一个英文 commit 标题和描述
- 把拟提交范围和 commit message 给用户确认
- 明确告知提交只会发生在当前分支，且不会自动 push
- 用户确认后，再执行提交

### “请提交 skills”
- 固定表示：优先处理当前项目中的 skills 子模块变更
- 如果主项目中表现为 submodule 指针变化，则提交该 submodule 指针更新
- 如果当前上下文明确位于 skills 子模块仓库内部，则按该子模块仓库内部改动起草并提交
- 无论哪种情况，都要先检查当前分支名称
- 把将要提交的是“submodule 指针更新”还是“子模块仓库内部文件改动”明确告诉用户
- 明确告知提交只会发生在当前分支，且不会自动 push
- 用户确认后，再执行提交

### “请提交这些”
- 固定表示：提交用户明确指定的文件
- 仅查看并处理用户明确指定的文件
- 如果用户指定的文件不清晰，先根据上下文列出将要提交的文件并请用户确认
- 如果指定对象中同时包含文件和文件夹，按用户给出的范围一起处理
- 只基于这些文件的改动起草 commit 标题和描述
- 用户确认后，只提交这些文件

### “请提交这个文件”
- 固定表示：提交当前活动文件这一个文件
- 以当前活动文件为默认提交范围
- 如果当前文件上下文不可用，再询问用户具体文件
- 只基于该文件改动起草 commit 标题和描述
- 用户确认后，只提交该文件

### “请提交这个文件夹”
- 固定表示：提交当前活动文件夹这一个目录下的所有相关改动
- 以当前活动文件夹为默认提交范围
- 提交范围包括该文件夹中的文件以及其子文件夹中的文件改动
- 如果该文件夹内存在 untracked 文件，也一并纳入候选提交范围
- 如果当前文件夹上下文不可用，再询问用户具体文件夹
- 只基于该文件夹范围内的改动起草 commit 标题和描述
- 用户确认后，只提交该文件夹范围内的改动

## Commit Message Rules
- 在本 skill 中：
  - `title` 指 Conventional Commits 的标题行
  - `description` 指标题之后的正文内容
- `title` 必须始终提供
- `description` 不是必填项
- 如果本次提交只对应一个简单任务，且没有可拆分的子任务，通常只需要 `title`
- 如果只提交一个文件，或提交多个文件但它们本质上是在完成同一个简单任务，通常只需要 `title`
- 如果一次提交同时包含多个任务，或一个任务内部还包含多个子任务，才添加 `description`
- 如果 `description` 中需要说明多个不同任务，使用一个 description，并在其中使用列表形式逐项说明
- 必须遵循 Conventional Commits 1.0.0 结构：
  - `<type>[optional scope]: <description>`
  - `[optional body]`
  - `[optional footer(s)]`
- `type` 是必填项，后面必须跟冒号和一个空格
- `scope` 是可选项，放在圆括号中，例如：`docs(skills):`
- `description` 是必填项，紧跟在 `type/scope` 后面
- `body` 是可选项；如果存在，必须与标题之间空一行
- 在本 skill 语境中，Conventional Commits 的 `body` 由 `description` 承载
- `footer` 是可选项；如果存在，必须与正文之间空一行；如果没有正文，则与标题之间空一行
- `feat` 必须用于“新增功能”
- `fix` 必须用于“修复 bug”
- 除 `feat` 和 `fix` 外，也允许使用其他类型
- 如果是 breaking change：
  - 可以在标题前缀中使用 `!`，例如：`feat(api)!: remove legacy endpoint`
  - 但仍然必须在 body 或 footer 中写 `BREAKING CHANGE: <description>`
  - `BREAKING CHANGE` 必须全大写，后跟冒号和空格
- footer 可用于附加元信息，并遵循类似 git trailer 的格式，例如：`Refs: #123`、`Fixes: #45`
- `subject` 使用英文、简短、明确
- 标题末尾不加句号
- 优先使用最贴近改动本质的类型，例如：`feat`、`fix`、`docs`、`refactor`、`perf`、`chore`、`style`、`test`、`build`、`ci`

## Workflow
1. 识别用户要提交的范围：全部、指定文件、当前文件、当前文件夹
2. 如果用户提到提交 skills，先判断是在提交主项目中的 submodule 指针，还是已经处于 skills 子模块仓库内部
3. 先检查当前分支名称，并明确记录本次提交只会发生在该分支
4. 先遵守 `.gitignore` 规则，排除所有被忽略的文件和文件夹
5. 用 `git status` 先列出候选改动范围，并明确区分 tracked、staged、untracked
6. 用 `git diff --staged`、`git diff`、`git diff -- <file>`、`git diff -- <directory>` 等方式检查实际改动
7. 如果工作区中存在超出目标范围的无关改动，要明确指出，并且不要混入本次提交
8. 提炼这次改动的主要意图，而不是逐行复述
9. 起草英文 Conventional Commit 标题和可选 `description`
10. 把以下信息展示给用户确认：
   - 将要提交的文件范围
   - 当前分支名称
   - 是否包含 untracked 文件
   - 是否是 submodule 指针更新
   - proposed title
   - proposed description（如需要）
   - 是否确认提交到“当前分支名称”
11. 在展示完这些信息后，明确暂停并等待用户确认
12. 只有在用户明确确认后，才执行提交
13. 提交时只添加确认范围内的文件，不扩大提交范围
14. 提交后回报最终 commit 标题、description 是否使用、提交所在分支，以及提交结果

## Execution Rules
- 用户确认前，不执行 `git add` 或 `git commit`
- 用户第一次说“请提交”时，不执行 `git add` 或 `git commit`
- 用户确认后，仅对已确认范围执行 `git add <paths>`，不要使用会扩大范围的提交方式
- “请提交” 可以使用当前工作区全部改动作为确认范围
- “请提交 skills” 默认优先理解为提交主项目中的 skills 子模块变更
- “请提交这些”“请提交这个文件”“请提交这个文件夹” 必须使用精确路径提交
- 被 `.gitignore` 排除的文件和文件夹，不读取、不分析、不提交
- 如果 `description` 包含多个任务，提交时将这些任务写成同一个 commit body 中的列表项，而不是多个分开的 body 段落
- 如果目标范围内没有实际改动，要明确告知用户，而不是生成空提交
- 不创建空提交，除非用户明确要求
- 提交前必须先检查当前分支名称
- 必须明确询问用户是否确认提交到当前分支
- 不自动推送远程仓库，除非用户明确要求；默认只创建本地提交，由用户自行 push

## Safety Rules
- “请提交” 默认表示提交当前工作区中所有已更改的文件
- “请提交 skills” 默认表示提交 skills 相关变更，并优先按主项目里的 submodule 变更理解
- “请提交这些” 默认表示只提交用户指定的那些文件
- “请提交这个文件” 默认表示只提交当前活动文件
- “请提交这个文件夹” 默认表示只提交当前活动文件夹及其内部内容的改动
- “请提交” 默认包含 untracked 文件；如果用户不希望包含，需要明确说明
- 所有被 `.gitignore` 排除的文件和文件夹，都不得读取或加入提交范围
- 不要把用户未指定的文件加入“请提交这些”或“请提交这个文件”的提交范围
- 不要把文件夹外部的改动加入“请提交这个文件夹”的提交范围
- 不要自动修改用户写好的代码或文档，只为了让 commit 更“干净”
- 不要在未经确认的情况下提交
- 不要把用户最初的“请提交”误判为最终确认
- 不要在未确认当前分支名称的情况下提交
- 不要把“提交”误解为 push
- 不要使用 `git commit --amend`，除非用户明确要求
- 如果工作区中混有无关改动，要明确指出并缩小提交范围

## Output Format Before Commit
- `Files to commit:` 列出将被提交的文件
- `Current branch:` 列出当前分支名称
- `Untracked included:` 明确是否包含新增未跟踪文件
- `Submodule update:` 明确是否为 submodule 指针更新；如果不是，写 `No`
- `Proposed title:` 给出英文 commit 标题
- `Proposed description:` 给出英文正文描述；如果不需要，明确写 `None`
- `Push after commit:` 明确写 `No`
- `Confirm branch target:` 询问用户是否确认提交到“当前分支名称”
- `Please confirm:` 请用户确认是否执行提交

## Conventional Commit Guide
- 优先格式：`type(scope): subject`
- 完整结构：
  - `<type>[optional scope]: <description>`
  - `[optional body]`
  - `[optional footer(s)]`
- 常用类型：
  - `feat`：新功能
  - `fix`：修复 bug
  - `docs`：文档变更
  - `refactor`：重构但不新增功能也不修复 bug
  - `perf`：性能优化
  - `chore`：维护、工具或配置类调整
  - `style`：纯格式调整
  - `test`：测试相关
  - `build`：构建系统或依赖相关
  - `ci`：CI/CD 相关
- subject 使用英文、简短、明确，末尾不加句号
- 如果改动小且单一，通常不需要 body
- 如果改动涉及多个任务、重要上下文或明显权衡，再添加 body
- body 与标题之间保留一个空行
- 如果需要说明多个事项，放在同一个 body 中，并使用清晰列表
- footer 是可选项，可用于 `Refs: #123`、`Fixes: #45` 等元信息
- 如果是 breaking change：
  - 可以在标题中使用 `!`
  - 仍然必须在 body 或 footer 中写 `BREAKING CHANGE: <description>`
