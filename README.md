# AI Skills

本仓库维护可在不同项目中复用的 AI skills：

- [Codex skills](codex-skills/README.md)
- [Claude Code skills](claude-skills/README.md)
- [标准工作时段流程](workflows/work-session.md)：由各项目的协作入口明确引用，并在各自仓库维护任务记录。

不同助手的版本分别维护；更新共同工作流程时，检查两个版本是否需要同步，同时保留各自的配置差异。skill 的入口是 `SKILL.md`，按需使用同目录下的参考资料、脚本或资源。

普通提交、分类提交及子模块提交统一由对应版本的 `git-commit` skill 处理：[Codex](codex-skills/git-commit/SKILL.md) / [Claude](claude-skills/git-commit/SKILL.md)。

## 编写原则

- 优先为反复出现、值得跨项目复用的流程编写 skill；项目状态和一次性事项放在对应项目文档中。
- 入口说明适用场景、必要流程和约束；较长或只在特定情况下使用的资料放入 `references/`，并从入口链接。避免多处重复维护同一规则。
- 不在仓库中保存密码、密钥或其他敏感信息；引用外部内容和资源时核实来源及许可。
- 项目应明确选择实际需要的 skills；将本仓库加入项目不代表自动启用全部 skills。项目的明确指令和规则仍然适用。

## 在项目中使用 Git 子模块

以下命令假设主项目位于当前目录，并以 `.ai-skills` 为子模块路径。实际路径和远程地址由项目自行配置；示例使用本仓库的 SSH 地址。

在尚未添加子模块的项目中：

```bash
git submodule add git@github.com:rebula001/ai-skills.git .ai-skills
git status
git submodule status
```

`git submodule add` 会登记子模块并创建或更新 `.gitmodules`。检查结果后，在主项目提交 `.gitmodules` 和子模块指针；**子模块路径可用，并不代表助手已经发现或启用其中的 skills**。

克隆已有项目后，在主项目根目录初始化已登记的子模块：

```bash
git submodule update --init --recursive
```

也可以克隆时使用 `git clone --recurse-submodules <project-url>`。如需检查 Git 是否已登记子模块，可使用 `git ls-files --stage .ai-skills`；输出以 `160000` 开头表示该路径是子模块指针。

## 修改共享 skill

主项目记录的是子模块的提交指针，子模块内部改动与主项目是两个独立仓库。操作前分别检查两者，确认分支、远程地址、已暂存内容和未提交改动：

```bash
git status
git submodule status
git -C .ai-skills status
git -C .ai-skills branch --show-current
git -C .ai-skills remote -v
```

子模块处于 detached HEAD 时，先确认要保留的提交及工作区改动，再选择合适的分支或其他处理方式；不要盲目切换、重置或拉取。具体流程：

1. 在子模块内检查并提交需要共享的改动，保留不属于本次提交的文件和原有暂存内容。
2. 如其他人需要访问该提交，将子模块提交推送到可访问的远程仓库；先确认目标分支与远程地址。暂不推送也可保留本地提交，但其他人可能无法检出它。
3. 回到主项目，检查 `.ai-skills` 指向预期提交，再提交主项目中的指针变更。主项目的推送是独立操作。

提交和推送由用户决定；助手按项目指令及所用 skill 的授权流程执行。不要假定固定使用 `main` 或 `origin`，也不要将子模块内部提交误认为主项目指针已更新。

## 更新已有项目的子模块

如果要让项目使用共享仓库中的另一个提交，先检查主项目与子模块工作区并确认目标版本，再在子模块中切换到该提交。操作前保留已有改动；完成后在主项目检查并提交指针。`git submodule update --remote .ai-skills` 会依据子模块配置获取远程版本，使用前应先确认目标及工作区状态，不应默认视为安全的最新版本更新。
