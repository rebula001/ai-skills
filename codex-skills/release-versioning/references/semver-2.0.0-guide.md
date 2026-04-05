# Semantic Versioning 2.0.0 Guide

## Source

- Official specification:
  - `https://semver.org/`

本文件是基于官方 SemVer 2.0.0 规则整理的工作参考，用于帮助判断发布版本如何递增。

## Core Model

标准版本格式：

- `MAJOR.MINOR.PATCH`

例如：

- `0.1.0`
- `0.1.1`
- `0.2.0`
- `1.0.0`
- `1.2.3`

规则：

- 三段都必须是非负整数
- 不允许前导零
- 版本一旦发布，该版本内容不应再被修改；修改应发布新版本

## Meaning Of Each Segment

### `PATCH`

适用于：

- 向后兼容的 bug 修复
- 不改变已声明 public API / public contract 的修正

### `MINOR`

适用于：

- 向后兼容地新增功能
- 向后兼容地扩展已有 public API
- 标记某个 public API 为 deprecated

规则：

- 提升 `MINOR` 时，`PATCH` 重置为 `0`

### `MAJOR`

适用于：

- 引入任何不兼容变化

规则：

- 提升 `MAJOR` 时，`MINOR` 与 `PATCH` 都重置为 `0`

## Major Zero Phase

`0.y.z` 表示初期开发阶段。

含义：

- 任意内容都可能变化
- public API 不应视为稳定
- 仍适合快速迭代和收敛设计

推荐理解：

- `0.1.0` 可以表示第一批可测试实现
- 后续开发期间可以继续使用 `0.x.y`
- 当项目开始强调稳定公开边界和兼容承诺时，再进入 `1.0.0`

## When To Move To `1.0.0`

适合进入 `1.0.0` 的常见信号：

- 项目已被真实使用
- public API / public contract 已较清晰
- 版本开始强调兼容性承诺
- 团队开始明确区分“兼容新增”和“破坏性变更”

如果仍然存在以下情况，通常还可以停留在 `0.y.z`：

- 核心结构仍在频繁变动
- 打包、接口、数据模型还没有稳定
- 对外使用边界尚未真正确定

## Public API / Public Contract

SemVer 的前提是项目已经声明“对外可依赖的边界”。

它不一定只指代码 API，也可以包括：

- 插件 REST API
- JSON 导入导出结构
- 打包产物结构
- 用户依赖的命令或工作流
- 其他被文档明确承诺的行为

如果这些边界发生不兼容变化，就应按 `MAJOR` 思考。

## Pre-release

预发布版本可在版本号后追加 `-<identifier>`。

示例：

- `1.0.0-alpha`
- `1.0.0-beta.1`
- `1.0.0-rc.1`

规则要点：

- 预发布版本优先级低于对应正式版本
- `1.0.0-rc.1 < 1.0.0`

## Build Metadata

可追加 `+<identifier>` 作为构建元数据。

示例：

- `1.0.0+20260405`
- `1.0.0-beta.1+exp.sha.1234567`

规则要点：

- build metadata 不影响版本优先级

## Practical Decision Checklist

判断版本时，优先按下面顺序：

1. 当前项目是否仍处于 `0.y.z` 初期阶段？
2. 本次变化是否改变了对外可依赖的边界？
3. 如果没有破坏兼容性，它是修复还是新增？
4. 是否需要同步更新：
   - 插件主版本号
   - Git tag
   - package 目录和 zip 文件名

## Relation To Conventional Commits

Conventional Commits 与 SemVer 的常见对应关系：

- `fix` 通常对应 `PATCH`
- `feat` 通常对应 `MINOR`
- `BREAKING CHANGE` 对应 `MAJOR`

注意：

- commit type 只能帮助理解版本意图
- 真正的版本判断仍应回到兼容性本身
