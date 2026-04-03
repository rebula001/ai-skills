---
name: submodule-sync
description: 用于在用户说“请同步子模块”时，检查当前项目中有改动的子模块，先将子模块改动提交并推送回子模块自己的主仓库，再回到主项目更新 submodule 指针。
disable-model-invocation: true
---

# Submodule Sync

## When To Use
- 当用户说“请同步子模块”时使用
- 当用户希望把当前项目里某个子模块中的改动，提交并推送回该子模块自己的主仓库时使用
- 当用户希望在子模块 push 成功后，再回到当前项目更新 submodule 指针时使用
- 当用户已经在项目里改动了 `.ai-skills/` 之类的子模块文件，并希望 AI 帮忙完成完整同步流程时使用

## Core Behavior
- 这个 skill 处理的是“子模块仓库 + 主项目仓库”两层 Git 流程，不是单一仓库提交
- 当用户说“请同步子模块”时，第一步先检查当前项目中哪些子模块存在改动
- 在识别出目标子模块后，第二步按 `git-commit` 的规则起草英文 Conventional Commit 标题和可选 `description`
- 默认先识别用户要同步的是哪个子模块；如果只有一个已修改子模块，可直接使用该子模块
- 先检查子模块当前分支、工作区状态，以及是否处于 detached HEAD
- 如果子模块处于 detached HEAD，不要直接在该状态下继续 push，先切回目标分支
- 起草标题和 `description` 时，应复用 `git-commit` 中关于范围检查、英文输出、Conventional Commits 和确认流程的规则
- 用户确认后，先提交子模块仓库，再确认并 push 到子模块自己的远程仓库
- 子模块 push 成功后，再回到主项目，起草 submodule 指针更新的 commit message
- 用户确认后，再提交主项目中的 submodule 指针更新
- 默认不自动 push 主项目仓库；是否 push 主项目仓库，必须单独再次确认

## Scope Defaults
- “子模块” 默认指当前项目中已存在 Git submodule 的目录
- 如果当前项目里只有一个已修改的子模块，默认使用该子模块
- 如果用户明确提到 `.ai-skills`，则固定处理 `.ai-skills`
- 如果存在多个已修改子模块而用户未明确指定，必须先让用户确认目标子模块
- “同步子模块” 默认包括：
  - 检查子模块状态
  - 在子模块仓库中提交改动
  - push 到子模块远程仓库
  - 回到主项目提交 submodule 指针更新
- “同步子模块” 不默认包括 push 主项目仓库，除非用户明确确认

## Workflow
1. 检查当前项目中哪些子模块存在改动
2. 识别目标子模块路径
3. 检查主项目当前分支名称
4. 检查目标子模块当前分支名称、工作区状态、远程状态
5. 如果子模块是 detached HEAD，明确告诉用户，并切回目标分支后继续
6. 检查子模块实际改动，并按 `git-commit` 的规则起草英文 Conventional Commit 标题和可选 `description`
7. 向用户展示子模块将提交的文件、子模块当前分支、拟定的 commit 标题和可选 `description`
8. 用户确认后，在子模块仓库内执行 `git add <paths>` 和 `git commit`
9. 提交成功后，明确展示将要 push 的目标仓库和目标分支，并再次请求确认
10. 用户确认后，执行子模块仓库的 push
11. push 成功后，回到主项目，检查 `.gitmodules` 与 submodule 指针状态
12. 起草主项目的 submodule 指针更新 commit message
13. 向用户展示主项目当前分支、将要提交的子模块路径、拟定的 commit message
14. 用户确认后，提交主项目中的 submodule 指针更新
15. 如需 push 主项目仓库，必须单独再次确认 push 目标仓库和目标分支

## Commit Message Rules
- 子模块仓库和主项目仓库的 commit message 都必须使用英文
- 默认使用 Conventional Commits 风格
- 子模块仓库的 commit message 应描述子模块内部真实改动
- 主项目仓库的 commit message 应描述“更新 submodule 指针”这件事
- 如果改动简单，通常只需要 `title`
- 如果需要 `description`，也必须使用英文
- 常用类型优先使用：`feat`、`fix`、`docs`、`refactor`、`chore`

## Detached HEAD Rules
- 如果子模块当前处于 detached HEAD，必须先明确告知用户
- 在 detached HEAD 状态下，可以检查改动，但不要直接默认继续 push
- 如果用户要继续同步，应先切换到目标分支，例如 `main`
- 切换到目标分支后，先同步远程最新提交，再继续提交流程
- 如果已经在 detached HEAD 上误提交，优先通过 `cherry-pick` 把该提交接回真实分支，而不是重做提交

## Push Rules
- 子模块仓库的 push 和主项目仓库的 push 是两件独立操作
- 每次 push 前都必须明确展示：
  - `Push repository:`
  - `Push branch:`
- 只有在用户明确确认后，才执行 push
- 不要把子模块仓库的 push 和主项目仓库的 push 混为一谈
- 如果子模块 push 失败，应先解释失败原因，再决定是否继续
- 如果子模块 push 失败，则不要继续提交主项目中的 submodule 指针更新
- AI 可以完成本地检查、改动范围确认、commit message 起草、本地提交，以及主项目中的 submodule 指针更新
- 如果目标是私有 SSH 仓库，而当前 AI 执行环境无法访问用户本机的 SSH 凭据，push 这一步可能需要用户在自己的本机终端或 SourceTree 中执行
- 一旦用户在本机完成子模块仓库的 push，AI 应继续完成主项目中的 submodule 指针更新

## Safety Rules
- 不要在未识别清楚目标子模块时执行同步
- 不要在存在多个候选子模块时自行猜测
- 不要在未确认当前分支名称的情况下提交
- 不要在未确认 push 目标仓库和目标分支的情况下 push
- 不要跳过子模块仓库提交，直接提交主项目 submodule 指针
- 不要在子模块仓库未 push 成功前，就把主项目的 submodule 指针更新当作完成
- 不要在 detached HEAD 状态下默认继续长期工作流
- 如果子模块工作区中混有无关改动，要明确指出并缩小范围

## Output Format Before Submodule Commit
- `Target submodule:` 列出目标子模块路径
- `Submodule branch:` 列出子模块当前分支；如果是 detached HEAD，明确写出
- `Files to commit:` 列出子模块将要提交的文件
- `Proposed title:` 给出英文 commit 标题
- `Proposed description:` 给出英文正文描述；如果不需要，明确写 `None`
- `Please confirm:` 请用户确认是否在子模块仓库执行提交

## Output Format Before Submodule Push
- `Push repository:` 列出子模块远程仓库地址或名称
- `Push branch:` 列出子模块将要 push 的分支
- `Submodule commit:` 列出刚刚创建的子模块提交标题或提交号
- `Please confirm:` 请用户确认是否 push 到该子模块仓库

## Output Format Before Parent Commit
- `Parent repository branch:` 列出主项目当前分支
- `Submodule pointer update:` 列出将更新的子模块路径
- `Proposed title:` 给出主项目提交标题
- `Proposed description:` 给出主项目正文描述；如果不需要，明确写 `None`
- `Please confirm:` 请用户确认是否提交主项目中的 submodule 指针更新

## Output Format Before Parent Push
- `Push repository:` 列出主项目远程仓库地址或名称
- `Push branch:` 列出主项目将要 push 的分支
- `Please confirm:` 请用户确认是否 push 到主项目仓库

## Notes
- 这个 skill 的核心目标是：把子模块改动安全地推回子模块自己的仓库，并让主项目正确记录新的 submodule 指针
- 如果用户只是想普通提交单一仓库改动，应优先使用 `git-commit`
- 如果用户说“请同步子模块”，优先理解为执行这一整套同步流程
- 在私有 SSH 仓库场景中，允许用户手动完成 push，而 AI 继续完成后续的主项目 submodule 更新步骤
