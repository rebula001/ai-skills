---
name: release-versioning
description: Use when the user wants to prepare a release version, decide the next release number under Semantic Versioning, explain why the version should be major, minor, or patch, update version-related files before committing, and add a version tag to the final commit.
---

# Release Versioning

## When To Use
- 当用户说“请发布”时使用
- 当用户要准备一个新版本号时使用
- 当用户要判断下一次发布应使用 `MAJOR`、`MINOR` 还是 `PATCH` 时使用
- 当用户要确认是否应该从 `0.y.z` 进入 `1.0.0` 时使用
- 当用户要更新版本相关文件，并在最终 commit 上打版本 tag 时使用

## Quick Start
- 先确认当前工作区是否存在准备发布的未提交改动
- 然后确定当前基准版本，而不是直接假设“上一个 commit”就是上一个发布版本
- 然后读取 `references/semver-2.0.0-guide.md`
- 基于兼容性判断版本增量，而不是基于“改动看起来大不大”
- 默认先展示当前版本、建议版本号、升级原因，以及是否继续进入发布提交流程
- 需要提交时，再与 `$git-commit` 配合

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
   - 是否继续进入发布提交流程
8. 等待用户确认或异议说明：
   - 如果用户不同意，应继续解释原因或根据用户给出的理由重新判断
   - 如果用户同意，再继续后续步骤
9. 用户确认发布版本号后，必须检查该版本号是否与 `project.config.json` 中的 `version` 一致
10. 如果不一致，必须先提醒用户版本不一致，并询问是否将 `project.config.json` 更新为确认后的版本号
11. 如果用户同意，先更新版本文件，再继续后续步骤
12. 对于本项目，版本更新应遵循单一来源规则：
    - 只手动修改 `project.config.json` 中的 `version`
    - 其余版本文件不得手动逐个修改，应通过同步命令统一更新，例如：
      - `frontend/package.json`
      - `frontend/package-lock.json`
      - `backend/wt-wp-multi-lang.php`
      - `sonar-project.properties`
13. 如果 AI 运行在 `WSL` 中，执行同步命令时应优先使用项目包装脚本，而不是直接运行原生 `npm`，例如：
    - `cd frontend && ../scripts/npmw.sh run sync:project-config`
14. 只有在同步命令执行完成后，才能继续最终 commit 与 tag 流程
15. 调用 `$git-commit`，由它按照实际改动范围起草并确认最终 commit 标题和描述
16. 在该最终 commit 上创建 Git tag，例如：
    - `v0.2.0`

## Baseline Version Rules

- 不应默认把“上一个 commit”当作上一个发布版本
- 发布基准版本优先看最近的发布标记，而不是最近的提交顺序
- 如果最近一次普通 commit 不是 release commit，这种区别尤其重要
- 如果最近 Git tag 与项目版本文件同时存在，必须做一致性校验
- 如果 Git tag 是 `v0.1.0`，而项目版本文件是 `0.2.0`，应明确提示用户存在版本不一致
- 出现版本不一致时，默认暂停并等待用户确认以哪个版本作为当前基准

## Version File Rules

- 用户一旦确认发布版本号，AI 必须检查该版本号是否与 `project.config.json` 中的 `version` 一致
- 如果不一致，必须先提醒用户并获得确认，再把 `project.config.json` 更新为一致值
- 对于本项目，`project.config.json.version` 是唯一允许手动修改的版本入口
- `frontend/package.json`
- `frontend/package-lock.json`
- `backend/wt-wp-multi-lang.php`
- `sonar-project.properties`
  这些文件的版本号不应手动逐个编辑，而应通过同步脚本统一生成
- 如果 AI 运行在 `WSL` 中，默认应使用：
  - `cd frontend && ../scripts/npmw.sh run sync:project-config`
  来完成版本同步
- 只有版本文件同步完成后，才能继续最终 commit 与 tag 流程
- 不应把“版本文件未同步”留到发布 commit 之后再补救，除非用户明确要求例外流程

## Commit Rules

- 不默认创建专门的 release commit
- 最终 commit 的标题和描述应完全交给 `$git-commit` 根据实际改动范围起草
- 如果实际改动本质是 `docs`、`chore`、`feat` 或其他类型，应按真实改动语义生成 commit message
- 版本号不应强行写进普通 commit 标题，除非用户明确要求
- release-versioning 负责确定版本号与更新版本文件，不负责替代 `$git-commit` 的文案判断

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
  - 是否继续进入发布提交流程

## Example Flow

1. 用户说：`请发布`
2. AI 查找最近 Git tag 或项目版本文件，确定当前版本
3. AI 基于当前未提交改动判断建议版本
4. AI 展示：
   - 当前版本：`0.1.0`
   - 建议版本：`0.2.0`
   - bump 类型：`MINOR`
   - 原因：向后兼容地新增功能
   - 是否继续进入发布提交流程：`Yes`
5. 用户确认
6. AI 检查 `project.config.json` 中的 `version` 是否已为 `0.2.0`
7. 如果不一致，先提醒用户并在用户同意后更新版本文件
8. AI 调用 `$git-commit`，由它根据实际改动生成最终 commit message
9. AI 为该 commit 创建 tag：
   - `v0.2.0`

## Inputs To Confirm
- 当前基准版本：
- 本次发布改动范围：
- 是否存在 public API / public contract：
- 是否要求向后兼容：
- 是否继续进入发布提交流程：

## Output Expectations
- 必须明确给出建议版本号
- 必须明确说明为什么是 `PATCH`、`MINOR` 或 `MAJOR`
- 必须明确说明是否继续进入发布提交流程
- 必须优先说明当前基准版本是如何确定的
- 用户确认发布版本号后，必须明确说明是否需要同步更新 `project.config.json`
- 如进入版本更新步骤，应同步提醒用户检查插件版本、tag 与 package 名称是否一致

## References
- 详细规则见：
  - `references/semver-2.0.0-guide.md`

## Notes
- 本 skill 以 `https://semver.org/` 的 Semantic Versioning 2.0.0 为基线
- 本 skill 关注“发布前版本号判断、版本文件更新与版本 tag 准备”
- 本 skill 不直接负责具体项目的打包与发布实现
- 需要执行 commit 时应与 `$git-commit` 配合
