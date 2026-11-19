# Claude Code Skills

本目录维护供 Claude Code 使用的共享 skills。共享仓库的使用与编写原则见[根目录 README](../README.md)。

普通提交、分类提交和子模块提交的流程见 [git-commit](git-commit/SKILL.md)。

## 目录结构

```text
claude-skills/
  <skill-name>/
    SKILL.md        # skill 入口
    references/     # 可选的按需参考资料
    scripts/        # 可选的辅助脚本
    assets/         # 可选的输出资源
```

skill 目录使用小写英文、数字及连字符。入口 `SKILL.md` 可通过 YAML frontmatter 定义 `name`、`description` 和调用方式；如果要求仅由用户手动调用，可设置 `disable-model-invocation: true`。支持文件是否拆分取决于用途，不以入口文件是否过长作为唯一标准；参考资料应从入口链接。

## 在项目中使用

Claude Code 可从项目的 `.claude/skills/<skill-name>/SKILL.md` 或用户目录 `~/.claude/skills/<skill-name>/SKILL.md` 等位置发现 skill；可用 skill 也可通过 `/skill-name` 调用。具体发现规则和配置方式以 [Claude Code 官方文档](https://code.claude.com/docs/en/skills)为准。

将本仓库作为 `.ai-skills/` 子模块后，`.ai-skills/claude-skills/` 只是共享文件路径，**不意味着 Claude Code 会自动发现或启用其中的 skills**。按当前环境和项目规则配置所需的引用或安装方式，避免重复维护副本。

如果维护 Claude Code 和 Codex 的同名版本，更新后核对共同流程，同时保留各自的 frontmatter 和调用方式。
