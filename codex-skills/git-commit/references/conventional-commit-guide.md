# Conventional Commit Guide

## Goal
帮助为一次提交生成简洁、准确、可验证的英文 commit message。

## Preferred Format
```text
type(scope): subject
```

完整结构：
```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

示例：
```text
docs(codex): add project skill templates
fix(woocommerce): correct cart total calculation
feat(storefront): add featured products section
```

## Type Selection
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档变更
- `refactor`: 重构但不新增功能也不修复 bug
- `perf`: 性能优化
- `chore`: 杂项维护、工具或配置更新
- `style`: 纯格式调整
- `test`: 测试相关
- `build`: 构建系统或依赖相关
- `ci`: CI/CD 相关

说明：
- `feat` 和 `fix` 是规范中具有明确语义的核心类型
- 其他类型是允许的，但它们本身不自动对应 SemVer 级别

## Subject Rules
- 使用英文
- 紧跟在 `type` 或 `type(scope)` 后
- 使用简短、明确的短句
- 不要在末尾加句号
- 尽量描述“改动意图”而不是“操作动作列表”

## Body Rules
- 如果提交只对应一个简单任务，且没有子任务，通常不需要正文
- 如果提交包含多个任务，或一个任务中包含多个子任务，再写正文
- 正文使用英文完整句子或简洁短句
- 如果存在正文，必须与标题之间空一行
- 避免把 diff 逐条翻译成说明
- 如果正文同时说明多个不同任务，优先使用列表来提高可读性
- 多个任务应写在同一个正文中，作为多个列表项出现，而不是拆成多个分离段落

## Footer Rules
- footer 是可选的
- 如果存在 footer，必须与正文之间空一行；如果没有正文，则与标题之间空一行
- footer 可写附加元信息，例如：
  - `Refs: #123`
  - `Fixes: #45`
  - `Reviewed-by: name`

## Breaking Change Rules
- breaking change 可以出现在任何 type 中
- 可以在标题中使用 `!`，例如：
  - `feat(api)!: remove legacy endpoint`
- 但仍然必须在 body 或 footer 中写：
  - `BREAKING CHANGE: <description>`
- `BREAKING CHANGE` 必须全大写
- 其后必须跟清晰描述，说明破坏性变化是什么

## Scope Guidance
- `scope` 可选
- 有明显模块边界时再加，例如：`docs`、`woocommerce`、`blocksy`、`codex`
- 如果没有清晰 scope，可以省略

## SemVer Mapping
- `fix` 对应 `PATCH`
- `feat` 对应 `MINOR`
- 含有 `BREAKING CHANGE` 的提交对应 `MAJOR`

## Source
- Official spec: https://www.conventionalcommits.org/
