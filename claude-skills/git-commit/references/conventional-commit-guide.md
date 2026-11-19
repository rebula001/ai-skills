# Conventional Commits 1.0.0 本地参考

本文件根据 Conventional Commits 1.0.0 整理，供日常提交离线使用；来源见文末。先遵守规范，再应用本 skill 的附加约定。

## Title 和 Description

- **Title**：提交的第一行，必须填写，例如 `fix(database): preserve row order`。
- **Description**：向用户展示的可选提交正文（Git 中的 body），用于补充改动原因、行为或限制；必要时填写，不能只重复标题。
- Title 和 Description 均使用英文。它们不同于规范结构中标题冒号后的 `<description>`，后者只是标题中的简短描述。

## 提交结构

```text
<type>[optional scope][optional !]: <description>

[optional body]

[optional footer(s)]
```

- 标题必须以类型开头，类型为名词；可附 scope 和破坏性变更标记 `!`，其后必须是冒号、空格和简短描述。
- `feat` 表示新增功能，`fix` 表示修复缺陷；也可以使用 `docs`、`refactor`、`perf`、`test`、`build`、`ci`、`style`、`chore` 等类型。
- scope 可选；如使用，必须用括号包围名词，表示受影响的代码范围，例如 `fix(database): preserve row order`。
- 正文可选，与标题之间必须有一个空行；正文可由多个段落组成。
- footer 可选，位于正文之后并以空行分隔；没有正文时与标题之间以空行分隔。每项由 token、分隔符和值组成：分隔符为冒号加空格或空格加 `#`，例如 `Reviewed-by: name`、`Refs #123`。
- footer 的 token 使用连字符替代空格；`BREAKING CHANGE` 是允许包含空格的例外。值可包含空格或换行，遇到下一个有效 token 和分隔符时结束。

## 破坏性变更

- 破坏性变更必须通过类型或 scope 后、冒号前的 `!`，或者通过 footer 表明；它可以属于任何类型。
- 官方规范允许只使用 `!`；此时标题中的描述必须说明破坏性变更。也可以使用 `BREAKING CHANGE: <description>` footer，其中标记必须大写，描述必须说明变化；footer token `BREAKING-CHANGE` 与 `BREAKING CHANGE` 等价。
- 本 skill 的附加约定：即使标题已经使用 `!`，也要在正文或 footer 中补充 `BREAKING CHANGE: <description>`，清楚解释影响。

## 本 skill 的写作约定

- 标题和正文使用英文；标题简短、明确，描述变更意图，末尾不加句号。标题足以说明简单变更时可省略正文；涉及复杂行为、原因或限制时补充正文。正文有多个适合并列说明的要点时，优先使用列表，提高可读性。
- 规范信息通常不区分大小写，但 `BREAKING CHANGE` 标记必须大写；本 skill 的类型名称统一使用小写。
- 按语义选择类型；`fix`、`feat` 和破坏性变更通常分别对应 SemVer 的补丁、次版本和主版本语义，其他类型不自动决定版本。是否发布或升级版本由项目决定。

来源：[Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/)。日常使用本地参考，无需为每次提交访问官网。
