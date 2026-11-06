# 用 nano-vllm 练习 Git 与 GitHub

这份指南配合真实的分支、提交和 Pull Request 使用。第一课只添加学习文档，不需要安装模型依赖，也不需要 GPU。

## 1. 先认清三个位置

| 位置 | 地址或名称 | 用途 |
| --- | --- | --- |
| 原作者的 GitHub 仓库 | `GeeeekExplorer/nano-vllm`，本地别名 `upstream` | 阅读原始代码、获取上游更新 |
| 你的 GitHub fork | `JohnUtah1997/nano-vllm`，本地别名 `origin` | 发布练习分支、向自己的主分支提 PR |
| 你的本地仓库 | `D:\Data\GitRepo\nano-vllm` | 编辑文件、运行 Git 命令 |

Git 管理本地版本；GitHub 托管远程仓库，并提供 PR、讨论和自动检查等协作功能。

`origin`、`upstream` 都是本地保存的远程地址别名，可以自定义。这里按上表使用，便于区分两个 GitHub 仓库。

在 PowerShell 中查看实际配置：

```powershell
Set-Location 'D:\Data\GitRepo\nano-vllm'
git remote -v
git status --short --branch
git branch -vv
```

## 2. 一次改动怎样进入主分支

```text
编辑文件 → git add → git commit → git push → 创建 PR → 检查差异 → 合并 PR
工作区      暂存区     本地提交      GitHub 分支                 GitHub main
```

| 操作 | 实际作用 |
| --- | --- |
| Fork | 在 GitHub 账号下建立一个与原仓库有关联的仓库 |
| Clone | 把远程仓库复制到本机 |
| Branch | 为一项改动建立单独的开发分支 |
| `git add` | 选择哪些文件内容进入下一次提交 |
| `git commit` | 在本地保存一份可追踪的版本记录 |
| `git push` | 把本地提交发送到远程仓库，并创建或更新远程分支 |
| Pull Request（PR） | 请求将来源分支的改动合入目标分支 |
| Merge | 把分支的改动合入目标分支 |
| `git fetch` | 获取远程对象和分支信息，不合并到当前本地分支 |
| `git pull --ff-only` | 获取远程更新，并仅在能快进时更新当前分支；出现分叉时停止 |

例如：`git commit` 成功后，GitHub 上还不会出现这次提交；`git push` 成功后，提交才会出现在远程分支；创建 PR 后，目标 `main` 仍需经过合并才会包含改动。

## 3. 示范：添加这份指南

这次示范的来源分支是 `practice/demo-first-pr`，目标是你自己仓库的 `main`。新增文件只有 `practice/README.md`。

本地使用的关键命令如下。此处用于回看示范，不要在已有同名分支时重复执行创建命令。

```powershell
git fetch origin
git switch --no-track -c practice/demo-first-pr origin/main

# 编辑并保存 practice/README.md 后：
git add -- practice/README.md
git diff --cached --stat
git diff --cached
git diff --cached --check
git commit -m "docs: add GitHub practice guide"
git log -1 --oneline

# 发布到自己的 fork：
git push -u origin practice/demo-first-pr
```

`-u` 会将当前本地分支与对应的远程分支关联。后续在这个分支继续提交时，通常直接运行 `git push` 即可。

本仓库的提交署名已在本地配置为 GitHub 用户名 `JohnUtah1997` 和对应的 GitHub noreply 邮箱。配置只作用于这个仓库，可以查看：

```powershell
git config --local user.name
git config --local user.email
```

Git 提交署名与 GitHub 登录是两回事：署名用于提交记录，推送还需要 GitHub 认证和目标仓库的写入权限。

## 4. 你来做：再添加一份自己的笔记

目标：新建 `practice/my-first-pr.md`，用自己的话回答三个问题，再将它通过 PR 合入自己的 `main`。

1. 这里的 `origin` 和 `upstream` 分别指向哪里？
2. `git commit` 与 `git push` 有什么区别？
3. PR 的来源分支和目标分支分别是什么？

先等待示范 PR 合并完成，再从更新后的 `main` 开始。每组命令执行后都观察结果，不需要一次性粘贴全部步骤。

### 第一步：检查状态并建立自己的分支

```powershell
Set-Location 'D:\Data\GitRepo\nano-vllm'
git status --short --branch
git switch main
git pull --ff-only origin main
git switch -c practice/my-first-pr
```

如果有需要保留的已跟踪文件改动，先停下来检查。你原有的 `docs/` 学习文档可能显示为 `??`，表示 Git 尚未跟踪；后面只暂存练习文件即可。

### 第二步：创建并填写文件

```powershell
@'
# 我的第一次 PR

## origin 和 upstream

在这里写下我的理解。

## commit 和 push

在这里写下我的理解。

## 这次 PR 的方向

在这里写下来源分支、目标仓库和目标分支。
'@ | Set-Content -LiteralPath 'practice/my-first-pr.md' -Encoding utf8

notepad practice/my-first-pr.md
```

在记事本中填写并保存，再继续下一步。

### 第三步：只提交这一个文件

```powershell
git status --short
git add -- practice/my-first-pr.md
git diff --cached --name-only
git diff --cached
git diff --cached --check
git commit -m "docs: add my first GitHub practice notes"
git log -1 --oneline
git status --short --branch
```

`git diff --cached --name-only` 应只列出 `practice/my-first-pr.md`。定点暂存可以让你在保留其他学习文件的同时，明确控制本次提交的范围。新文件在暂存前通常不会出现在普通的 `git diff` 中，可以先直接打开阅读。

### 第四步：推送并创建 PR

```powershell
git push -u origin practice/my-first-pr
```

如果首次推送要求登录，在 GitHub 的正常登录流程中使用自己的账号完成认证。若推送失败，保留本地提交，记录报错后再处理即可。

推送成功后，打开[你自己的 Pull Requests 页面](https://github.com/JohnUtah1997/nano-vllm/pulls)，点击 **New pull request**，确认以下方向：

| 页面字段 | 应选择的值 |
| --- | --- |
| base repository（目标仓库） | `JohnUtah1997/nano-vllm` |
| base（目标分支） | `main` |
| head repository（来源仓库，如页面显示） | `JohnUtah1997/nano-vllm` |
| compare（来源分支） | `practice/my-first-pr` |

标题可以写“docs: 添加我的第一次 GitHub 练习笔记”。描述写清新增文件及检查结果。创建后查看 **Files changed**，确认只有自己的练习文件。

### 第五步：合并，再同步本地 main

查看差异后，在自己的 PR 页面点击 **Merge pull request** 并确认合并。随后回到本地：

```powershell
git switch main
git pull --ff-only origin main
git log --oneline --graph --decorate -8
git status --short --branch
Get-Content -LiteralPath 'practice/my-first-pr.md' -Encoding utf8
```

验收标准：PR 显示 **Merged**；GitHub 的 `main` 和本地 `main` 都能看到你的笔记；本地原有学习文档仍然保留。

## 5. 后续练习路线

| 顺序 | 练习 | 完成时应能解释 |
| --- | --- | --- |
| 1 | 上面的第一次 PR | 本地提交、远程分支、PR、合并分别发生在哪里 |
| 2 | 在同一 PR 中继续提交一次修改 | 为什么不用为每次小修改重建 PR |
| 3 | 制造并解决一次文档冲突 | 冲突两边各是谁的改动，解决后保留了什么 |
| 4 | 用 `git revert` 撤销一次练习提交 | 为什么撤销可以作为新的提交保留在历史中 |
| 5 | 比较 merge、squash、rebase | 主分支的提交历史分别发生什么变化 |
| 6 | 获取并合入 upstream 更新 | `upstream/main`、本地 `main`、`origin/main` 的区别 |
| 7 | Issue 与 PR 关联 | 如何描述任务、关联改动、关闭已完成任务 |
| 8 | 增加一个简单的 GitHub Actions 检查 | PR 的自动检查在何时运行、失败日志在哪里 |

这个 fork 在准备第一课时未启用 Issues；练到第 7 项时再处理该设置。文档练习只验证 Git/GitHub 工作流，模型功能和性能需要在对应运行环境中另行验证。
