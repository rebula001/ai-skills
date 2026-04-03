# Claude Skills Repository

这个目录用于集中维护可复用的 Claude Code skills，供多个项目通过子模块或同步方式共享使用。

## Purpose
- 沉淀可跨项目复用的工作流、约定和领域知识
- 让不同项目中的 Claude Code 会话更快进入稳定工作方式
- 将复杂或高频任务整理为标准化 skill

## Recommended Structure
```text
claude-skills/
  README.md
  <skill-name>/
    SKILL.md
```

## Official Structure Notes
- Claude Code skill 的入口文件是 `SKILL.md`
- skill 可放在项目中的 `.claude/skills/<skill-name>/SKILL.md`
- 也可放在个人目录 `~/.claude/skills/<skill-name>/SKILL.md`
- `SKILL.md` 支持 YAML frontmatter
- `description` 用于帮助 Claude 判断何时自动加载该 skill
- 可通过 `/skill-name` 直接调用
- 除 `SKILL.md` 外，其余文件和目录都只是可选项

## Naming Rules
- 文件夹名称使用英文
- 正式 skill 文件夹名称使用小写英文加连字符
- 例如：`git-commit`、`project-deploy`

## Authoring Rules
- 每个 skill 必须包含 `SKILL.md`
- `SKILL.md` 顶部建议包含 `name` 和 `description`
- 如果该 skill 主要用于手动触发，可加 `disable-model-invocation: true`
- 正文尽量精简，只写工作流、触发条件和必要约束
- 只有在 `SKILL.md` 明显过长时，再拆分出额外文件

## Good Fit For Skills
- 提交流程与 commit message 规范
- 部署流程
- 项目级代码规范
- 文档维护流程
- 审核检查清单

## Not A Good Fit
- 一次性的临时讨论
- 只适用于当前一天的待办事项
- 仅属于某个单独项目、且不值得跨项目复用的短期状态

## Suggested Workflow
1. 先确认这项规则是否值得跨项目复用
2. 当某项工作开始重复出现时，再抽成 skill
3. 先尽量把规则收敛在单个 `SKILL.md` 内
4. 只有在必要时才补充额外文件
5. 在实际项目中通过子模块或同步方式引用这个目录

## Project Integration
- 如果项目使用 Claude Code，可将本仓库作为 Git submodule 引入，例如放在项目根目录的 `_ai-skills/`
- 在项目内只引用 Claude 所需的 skill，例如 `_ai-skills/claude-skills/git-commit/`
- 如果共享 skill 与项目本地规则冲突，以项目本地规则为准

## Security
- 不要在 skill 中写入密码、密钥、许可证等敏感信息
- 如需记录配置来源，只写位置和用途
