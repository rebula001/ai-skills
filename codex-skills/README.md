# Codex Skills Repository

这个目录用于集中维护可复用的 Codex skills，供多个项目通过子模块或同步方式共享使用。

## Purpose
- 沉淀可跨项目复用的工作流、约定和领域知识
- 让不同项目中的 Codex 会话更快进入稳定工作方式
- 将复杂或高频任务整理为标准化 skill

## Recommended Structure
```text
codex-skills/
  README.md
  <skill-name>/
    SKILL.md
    agents/
      openai.yaml
    scripts/
    references/
    assets/
  templates/
    basic-skill/
      SKILL.md.template
      agents/
        openai.yaml.template
```

## Naming Rules
- 文件夹名称使用英文
- 正式 skill 文件夹名称使用小写英文加连字符
- 例如：`wordpress-store-docs`、`woo-product-setup`

## Authoring Rules
- 每个 skill 必须包含 `SKILL.md`
- `SKILL.md` 顶部必须包含 `name` 和 `description` 的 YAML frontmatter
- 正文尽量精简，只写工作流、触发条件和必要约束
- 详细说明放到 `references/`
- 可复用脚本放到 `scripts/`
- 模板、示例资源放到 `assets/`

## Good Fit For Skills
- WordPress 安装与初始化检查清单
- WooCommerce 商品录入标准流程
- Blocksy 主题定制记录规范
- 站点迁移与上线检查
- 文档整理与维护手册更新流程

## Not A Good Fit
- 一次性的临时讨论
- 只适用于当前一天的待办事项
- 已经能在某个单独项目的本地文档中清楚记录的短期状态

## Suggested Workflow
1. 先确认这项规则是否值得跨项目复用
2. 当某项工作开始重复出现时，再抽成 skill
3. 从 `templates/basic-skill/` 复制一份作为起点
4. 根据用途补充 `references/`、`scripts/` 或 `assets/`
5. 在实际项目中通过子模块或同步方式引用这个目录

## Project Integration
- 如果项目使用 Codex，可将本仓库作为 Git submodule 引入，例如放在项目根目录的 `_ai-skills/`
- 在项目内只引用 Codex 所需的 skill，例如 `_ai-skills/codex-skills/git-commit/`
- 如果共享 skill 与项目本地规则冲突，以项目本地规则为准

## Security
- 不要在 skill 中写入密码、密钥、许可证等敏感信息
- 如需记录配置来源，只写位置和用途
