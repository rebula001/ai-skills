# Codex Skills

本目录维护供 Codex 使用的共享 skills。共享仓库的使用与编写原则见[根目录 README](../README.md)。

普通提交、分类提交和子模块提交的流程见 [git-commit](git-commit/SKILL.md)。

## 目录结构

```text
codex-skills/
  <skill-name>/
    SKILL.md
    agents/openai.yaml  # 可选的界面元数据及调用配置
    references/         # 可选的按需参考资料
    scripts/            # 可选的辅助脚本
    assets/             # 可选的输出资源
  templates/basic-skill/ # 本仓库提供的起始模板
```

skill 目录使用小写英文、数字及连字符；`SKILL.md` 的 YAML frontmatter 包含 `name` 和 `description`。正文保持聚焦，详细或条件性的资料可以放在 `references/`，并在入口说明何时读取；是否拆分取决于实际用途，而非文件长度。

## 在项目中引用

主项目可以将本仓库作为子模块放在 `.ai-skills/` 等路径，再根据项目及 Codex 的配置方式选择所需 skill，例如 `.ai-skills/codex-skills/git-commit/`。**加入子模块不等于 skill 已自动安装或可被 Codex 自动发现**；应根据当前环境的 skill 配置和项目规则确认实际可用方式。

如果维护 Codex 和 Claude Code 的同名版本，更新后核对共同流程，同时保留各自的元数据和调用方式。模板可作为起点，但不必为简单 skill 创建没有用途的文件。
