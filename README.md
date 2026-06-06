# AI Skills Submodule

本仓库作为共享的 AI skills 仓库使用，可通过 Git submodule 引入到不同项目中。

在当前项目中的配置是：

- 项目路径：`.ai-skills/`
- 远程仓库：`git@github.com:rebula001/ai-skills.git`
- Codex skills：`codex-skills/`
- Claude skills：`claude-skills/`

Submodule 的核心规则是：主项目只记录 `.ai-skills` 当前指向的某一个提交。也就是说，本仓库更新后，使用它的项目不会自动跟着变，需要在对应项目里手动更新并提交一次 submodule 指针。

## 新项目如何配置 Submodule

如果一个新项目还没有配置 `.ai-skills`，需要在项目根目录准备 `.gitmodules`，并让 Git 在主项目索引里登记 `.ai-skills` 的 submodule 指针。

`.gitmodules` 文件应放在项目根目录，内容如下：

```ini
[submodule ".ai-skills"]
	path = .ai-skills
	url = git@github.com:rebula001/ai-skills.git
```

其中：

- `path` 表示 submodule 在当前项目里的目录。
- `url` 表示 submodule 对应的远程仓库地址。

不过，仅仅创建 `.gitmodules` 还不够。`.gitmodules` 只是配置文件，Git 还需要在主项目里记录 `.ai-skills` 当前指向的提交。推荐直接在项目根目录执行：

```bash
git submodule add git@github.com:rebula001/ai-skills.git .ai-skills
```

这条命令会自动创建或更新 `.gitmodules`，并把 `.ai-skills` 登记为 submodule。

如果 `.ai-skills/` 已经存在，但只是一个空目录，可以先删除空目录：

```bash
rmdir .ai-skills
git submodule add git@github.com:rebula001/ai-skills.git .ai-skills
```

添加完成后检查：

```bash
git status
git submodule status
git ls-files --stage .ai-skills .gitmodules
```

如果 `git ls-files --stage .ai-skills` 的输出以 `160000` 开头，说明 `.ai-skills` 已经被 Git 正确登记为 submodule。

然后在主项目中提交：

```bash
git add .gitmodules .ai-skills
git commit -m "chore(skills): add ai-skills submodule"
```

最后再 push 当前项目仓库：

```bash
git push
```

注意，这一步 push 的是当前项目仓库，不是公共 `ai-skills` 仓库。

## 克隆项目后如何初始化 Submodule

其他人首次克隆使用 `.ai-skills` 的项目后，需要初始化 submodule：

```bash
git submodule update --init --recursive
```

如果只想初始化 `.ai-skills`：

```bash
git submodule update --init .ai-skills
```

初始化后检查：

```bash
git submodule status --recursive
```

如果看到 `.ai-skills` 对应的提交号，说明初始化成功。

也可以在克隆时一次性带上 submodule：

```bash
git clone --recurse-submodules <project-repo-url>
```

## 在项目中修改 Submodule 后如何更新仓库

如果你在项目里修改了 `.ai-skills/` 里面的文件，例如：

```text
.ai-skills/codex-skills/git-commit/SKILL.md
```

需要提交两次：

1. 先在 `.ai-skills/` 子模块仓库里提交并 push 到公共 `ai-skills` 仓库。
2. 再回到主项目仓库，提交 `.ai-skills` 的 submodule 指针更新。

这两次提交属于两个不同仓库，不能混在一起。

### 1. 进入子模块仓库

```bash
cd .ai-skills
```

### 2. 确认当前在 main 分支

```bash
git branch --show-current
git status
```

如果分支名为空，或者状态里出现 `HEAD detached from ...`，说明当前处于 detached HEAD。先切回 `main`：

```bash
git switch main
```

然后同步远程最新提交：

```bash
git pull --ff-only origin main
```

建议在开始修改 `.ai-skills/` 文件之前就完成这一步，避免把修改提交到 detached HEAD。

### 3. 在子模块里提交修改

先查看这次在子模块里改了哪些文件：

```bash
git status
```

然后用 `git add` 把要提交的文件加入暂存区。`git add <changed-path>` 里的 `<changed-path>` 是占位写法，实际使用时要替换成具体文件或目录。

只提交某一个文件：

```bash
git add <changed-path>
```

例如：

```bash
git add codex-skills/git-commit/SKILL.md
```

提交某一个目录下的全部改动：

```bash
git add codex-skills/git-commit
```

提交当前目录下的全部改动：

```bash
git add .
```

`git add .` 会把当前所在目录及其子目录里的新增、修改、删除都加入暂存区。因为这一步是在 `.ai-skills/` 目录里执行，所以它只会暂存 `.ai-skills` 子模块仓库里的改动，不会暂存主项目根目录里的文件。

暂存完成后再提交：

```bash
git commit -m "chore(skills): update shared skill"
```

完整例子：

```bash
git status
git add codex-skills/git-commit/SKILL.md
git commit -m "chore(skills): refine git commit workflow"
```

### 4. 推送公共 ai-skills 仓库

```bash
git push origin main
```

这一步推送的是公共 `ai-skills` 仓库。

如果当前执行环境无法访问你的 SSH 凭据，而公共仓库又是私有 SSH 仓库，这一步需要你在自己的本机终端或 SourceTree 中完成。

### 5. 回到主项目并提交 submodule 指针

```bash
cd ..
git status
git submodule status
git add .ai-skills
git commit -m "chore(skills): update ai-skills submodule"
```

这一步提交的是主项目仓库里的 submodule 指针。它告诉主项目：“以后使用公共 `ai-skills` 仓库里的这个新提交。”

最后 push 主项目仓库：

```bash
git push
```

## 只同步公共 ai-skills 的新提交

如果公共 `ai-skills` 仓库已经有新提交，而你只是想让某个项目使用那个最新提交，可以在主项目根目录执行：

```bash
git submodule update --remote .ai-skills
```

然后提交主项目里的 submodule 指针：

```bash
git status
git add .ai-skills
git commit -m "chore(skills): update ai-skills submodule"
git push
```

也可以显式进入子模块拉取：

```bash
git -C .ai-skills pull --ff-only origin main
```

拉取后同样需要回到主项目提交 `.ai-skills` 指针更新。

## 常见问题

### git submodule update --init --recursive 没有反应

这个命令只会初始化或更新已经被主项目登记过的 submodule。如果没有输出，常见原因是：

- 当前仓库还没有登记任何 submodule。
- `.gitmodules` 文件存在，但还没有提交到主项目。
- `.ai-skills` 只是普通目录，还没有通过 `git submodule add` 登记成 submodule。

检查命令：

```bash
git submodule status --recursive
git ls-files --stage .ai-skills .gitmodules
```

如果 `git submodule status --recursive` 没有输出，通常表示 Git 当前没有识别到可更新的 submodule。

### 误在 detached HEAD 里提交了怎么办

如果你在 `.ai-skills/` 里看到：

```text
HEAD detached from <commit>
```

并且已经误提交了一个 commit，不要直接 reset。先记住这个误提交的 commit id，例如 `4418e38`，然后：

```bash
git switch main
git pull --ff-only origin main
git cherry-pick 4418e38
git push origin main
```

成功后，再回到主项目根目录提交 `.ai-skills` 的 submodule 指针更新。

### 主项目里看到 .ai-skills 已修改

通常表示两种情况之一：

- 子模块内部有未提交改动。
- 子模块已经切换到了新的提交，但主项目还没有提交这个指针变化。

可以分别检查：

```bash
git status
git -C .ai-skills status
git submodule status
```
