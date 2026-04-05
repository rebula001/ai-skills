---
name: release-versioning
description: 用于准备发布版本号：判断下一次发布应使用哪个版本、解释为什么是 major/minor/patch、确认是否创建独立 release commit，并在提交前更新版本相关文件。
disable-model-invocation: true
---

# Release Versioning

## When To Use
- 当用户说“请发布”时使用
- 当用户要准备一个新版本号时使用
- 当用户要判断下一次发布应使用 `MAJOR`、`MINOR` 还是 `PATCH` 时使用
- 当用户要确认是否应该从 `0.y.z` 进入 `1.0.0` 时使用
- 当用户要更新版本相关文件并创建独立 release commit 时使用

## Quick Start
- 先确认当前工作区是否存在准备发布的未提交改动
- 然后确定当前基准版本，而不是直接假设“上一个 commit”就是上一个发布版本
- 再读取 `references/semver-2.0.0-guide.md`
- 基于兼容性判断版本增量，而不是基于“改动看起来大不大”
- 默认先展示当前版本、建议版本号、升级原因，以及是否创建独立 release commit
- 需要提交时，再与 `/git-commit` 配合

## Workflow
1. 确认当前工作区存在准备发布的未提交改动
2. 确定当前基准版本，优先级如下：
   - 先查最近的 Git tag
   - 再查项目中声明的当前版本文件，例如：
     - `project.config.json`
     - 插件主文件中的 `Version`
   - 如果 Git tag 与项目版本文件都存在，必须检查两者是否一致
   - 如果两者不一致，必须暂停并提醒用户确认当前基准版本，不应自动继续
   - 如果没有 Git tag，则回退到项目版本文件
   - 如果以上都不可用，再回退到最近一次 release commit
3. 明确本次发布涉及的改动范围
4. 明确 public API / public contract：
   - 对外接口
   - 数据格式
   - 打包结构
   - 用户依赖的工作流
5. 判断本次发布是：
   - 向后兼容修复
   - 向后兼容新增
   - 不兼容变化
6. 计算建议版本号：
   - `PATCH`
   - `MINOR`
   - `MAJOR`
7. 向用户展示：
   - 当前版本
   - 建议版本号
   - bump 类型
   - 升级原因
   - 是否建议创建独立 release commit
8. 等待用户确认或异议说明：
   - 如果用户不同意，应继续解释原因或根据用户给出的理由重新判断
   - 如果用户同意，再继续后续步骤
9. 如项目存在版本文件，先更新版本相关文件，再进行 commit，例如：
   - `project.config.json`
   - 插件主文件中的 `Version`
10. 调用 `/git-commit` 创建独立 release commit，例如：
    - `chore(release): bump version to 0.2.0`
11. 在该 release commit 上创建 Git tag，例如：
    - `v0.2.0`

## Baseline Version Rules

- 不应默认把“上一个 commit”当作上一个发布版本
- 发布基准版本优先看最近的发布标记，而不是最近的提交顺序
- 如果最近一次普通 commit 不是 release commit，这种区别尤其重要
- 如果最近 Git tag 与项目版本文件同时存在，必须做一致性校验
- 如果 Git tag 是 `v0.1.0`，而项目版本文件是 `0.2.0`，应明确提示用户存在版本不一致
- 出现版本不一致时，默认暂停并等待用户确认以哪个版本作为当前基准

## Version File Rules

- 如果项目存在统一版本文件或配置文件，应在 release commit 之前先完成更新
- 否则 release commit 将无法真实包含版本变化
- 版本文件更新完成后，再执行 release commit 与 tag 流程

## Tag Rules

- Git tag 应创建在 release commit 之后
- tag 表示该 release commit 是某个发布版本的源码基线
- tag 不是普通 commit message 的一部分，而是独立的版本标记

## Confirmation Expectations

- 用户确认前，默认只展示建议，不直接修改版本文件、不直接 commit、不直接打 tag
- 展示内容至少应包括：
  - 最近 Git tag
  - 项目版本文件中的版本
  - 是否检测到不一致
  - 当前版本
  - 建议版本
  - bump 类型：`PATCH / MINOR / MAJOR`
  - 升级原因
  - 是否建议创建独立 release commit

## Example Flow

1. 用户说：`请发布`
2. AI 查找最近 Git tag 或项目版本文件，确定当前版本
3. AI 基于当前未提交改动判断建议版本
4. AI 展示：
   - 当前版本：`0.1.0`
   - 建议版本：`0.2.0`
   - bump 类型：`MINOR`
   - 原因：向后兼容地新增功能
   - 是否建议创建独立 release commit：`Yes`
5. 用户确认
6. AI 先更新版本文件
7. AI 调用 `/git-commit` 创建：
   - `chore(release): bump version to 0.2.0`
8. AI 为该 commit 创建 tag：
   - `v0.2.0`

## Inputs To Confirm
- 当前基准版本：
- 本次发布改动范围：
- 是否存在 public API / public contract：
- 是否要求向后兼容：
- 是否要创建独立 release commit：

## Output Expectations
- 必须明确给出建议版本号
- 必须明确说明为什么是 `PATCH`、`MINOR` 或 `MAJOR`
- 必须明确说明是否建议创建独立 release commit
- 必须优先说明当前基准版本是如何确定的
- 如进入版本更新步骤，应同步提醒用户检查插件版本、tag 与 package 名称是否一致

## References
- 详细规则见：
  - `references/semver-2.0.0-guide.md`

## Notes
- 本 skill 以 `https://semver.org/` 的 Semantic Versioning 2.0.0 为基线
- 本 skill 关注“发布前版本号判断与 release commit 准备”
- 本 skill 不直接负责具体项目的打包与发布实现
- 需要执行 commit 时应与 `/git-commit` 配合
