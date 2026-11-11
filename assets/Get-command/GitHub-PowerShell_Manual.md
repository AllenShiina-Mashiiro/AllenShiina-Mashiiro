# GitHub-PowerShell 学习笔记

## 简介

GitHub-PowerShell_Manual.md 是一份关于 GitHub 和本地项目提交、修改、分支管理及文件同步的操作知识。

## 学习清单

- [x] 第一次上传本地项目到 GitHub
- [x] 日常同步本地文件与 GitHub
- [x] 创建、切换、删除 Git 分支
- [x] Commit 提交管理
- [x] 本地新增、修改、删除文件并同步到 GitHub
- [x] 远程仓库管理
- [x] 表格
- [x] Mermaid
- [x] 数学公式
- [x] 脚注


## 代码示例

### 第一次上传本地项目到 GitHub

#### 1. 进入项目目录

```powershell
cd 你的项目目录
```

#### 2. 初始化 Git

```powershell
git init
```

#### 3. 设置主分支

```powershell
git branch -M main
```

#### 4. 关联 GitHub 远程仓库

```powershell
git remote add origin 你的仓库链接.git
```

> 如果已经关联过，不要重复添加。可以检查：

> ```powershell
> git remote -v
> ```

#### 5. 添加所有文件

```powershell
git add .
```

#### 6. 创建第一次提交

```powershell
git commit -m "Initial commit"
```

#### 7. 推送到 GitHub

```powershell
git push -u origin main
```

> 如果 GitHub 仓库已经存在 README、LICENSE 等文件，而本地又是独立初始化的仓库，需要先整合两边历史：

> ```powershell
> git pull origin main --allow-unrelated-histories
> ```

> 如果合并时出现冲突，先解决冲突，再执行 `git add .`、`git commit`，最后重新推送。不要随意使用 `git push --force`。

### 日常同步本地文件与 GitHub

#### 1. 本地修改后上传 GitHub

```powershell
cd 你的项目目录

git add .
git commit -m "Update profile"
git pull --rebase origin main
git push origin main
```

> 命令	作用

> `git add .`	将当前目录的修改加入暂存区

> `git commit -m`	创建本地提交

> `git pull --rebase`	获取并整合远程更新

> `git push`	上传本地提交

> 如果提示 `nothing to commit, working tree clean`，说明没有新的本地修改，可以跳过 `git add` 和 `git commit`，直接执行拉取和推送。

#### 2. 下载 GitHub 最新文件到本地

```powershell
git pull origin main
```

> 如果本地有尚未提交的修改，建议先提交或暂存，再拉取。

#### 3. 查看本地与远程同步状态

```powershell
git status
```

```powershell
git fetch origin
git log --oneline --graph --decorate --all -20
```

> `git fetch` 只获取远程更新，不会自动合并到当前分支。

#### 4. 查看文件修改内容

```powershell
git diff
```

> 查看已经加入暂存区的修改：

```powershell
git diff --cached
```

### 创建、切换、删除 Git 分支

#### 1. 查看所有本地分支

```powershell
git branch
```

#### 2. 查看本地和远程全部分支

```powershell
git branch -a
```

#### 3. 创建新分支

> 例如创建一个 dev 开发分支：

```powershell
git branch dev
```

#### 4. 创建并立即切换分支

```powershell
git switch -c dev
```

#### 5. 切换到已有分支

```powershell
git switch main
```

```powershell
git switch dev
```

#### 6. 将新分支推送到 GitHub

```powershell
git push -u origin dev
```

> 以后在 dev 分支上传更新：

> ```powershell
> git add .
> git commit -m "Update dev"
> git push
> ```

#### 7. 下载 GitHub 已有分支

```powershell
git fetch origin
git switch -c dev --track origin/dev
```

#### 8. 删除本地分支

> 先切换到其他分支：

```powershell
git switch main
```

> 然后删除分支：

```powershell
git branch -d dev
```

> 如果 Git 提示该分支尚未合并，且你确定不要其中的修改，可以强制删除：

```powershell
git branch -D dev
```

> `-D` 可能导致未合并提交失去分支引用，使用前请确认不再需要这些内容。

#### 9. 删除 GitHub 远程分支

```powershell
git push origin --delete dev
```

#### 10. 将开发分支合并到主分支

```powershell
git switch main
git pull origin main
git merge dev
git push origin main
```

> 合并完成后，可以保留 dev 分支继续开发，也可以按需删除。

### 本地新增、修改、删除文件并同步到 GitHub

#### 1. 新增文件并上传

> 例如新增一个 Projects.md 文件：

```powershell
New-Item Projects.md -ItemType File
```

> 编辑并保存文件后执行：

```powershell
git add Projects.md
git commit -m "Add Projects page"
git push origin main
```

#### 2. 新增文件夹并上传

```powershell
git add ./assets
git commit -m "Add assets"
git push origin main
```

> Git 不会跟踪空文件夹。如果需要保留空目录，可以在目录中放置一个 `.gitkeep` 文件。

#### 3. 删除本地文件并同步到 GitHub

> 例如删除 REDAME.txt：

```powershell
git rm REDAME.txt
git commit -m "Remove REDAME file"
git push origin main
```

#### 4. 删除文件夹并同步到 GitHub

```powershell
git rm -r assets
git commit -m "Remove assets directory"
git push origin main
```

#### 5. 只删除 Git 跟踪，不删除本地文件

```powershell
git rm --cached config.json
git commit -m "Stop tracking config"
git push origin main
```

> 如果希望 Git 忽略这个文件，还需要将它添加到 `.gitignore`。

#### 6. 修改文件并上传

> 例如使用记事本修改 README.md：

```powershell
notepad README.md
```

> 保存后执行：

```powershell
git add README.md
git commit -m "Update README"
git push origin main
```

#### 7. 在 GitHub 网页直接新增文件

1. 打开 GitHub 仓库。
2. 点击 `Add file`。
3. 选择 `Create new file`。
4. 输入文件名和内容。
5. 点击 `Commit changes`。

> 随后在本地执行：

```powershell
git pull origin main
```

#### 8. 在 GitHub 网页直接删除文件

1. 打开仓库中的目标文件。
2. 点击文件右上角的删除按钮。
3. 填写 Commit 信息。
4. 点击 `Commit changes`。

> 随后在本地执行：

```powershell
git pull origin main
```

> 如果本地有未提交的修改，先提交或暂存，再执行 Pull，避免冲突。

### Commit 提交管理

#### 1. 查看提交历史

```powershell
git log --oneline
```

> 查看图形化提交历史：

```powershell
git log --oneline --graph --decorate --all
```

#### 2. 修改最近一次提交说明

```powershell
git commit --amend -m "New commit message"
```

> 如果这次提交已经推送到 GitHub，需要谨慎处理历史改写，不要直接对共享分支强制推送。

#### 3. 撤销尚未提交的暂存

```powershell
git restore --staged .
```

#### 4. 撤销文件尚未提交的修改

```powershell
git restore README.md
```

> 这会丢弃该文件尚未提交的修改，执行前请确认不再需要这些内容。

#### 5. 撤销最近一次提交，但保留文件修改

```powershell
git reset --soft HEAD~1
```

#### 6. 撤销最近一次提交，并取消暂存但保留文件修改

```powershell
git reset HEAD~1
```

#### 7. 安全撤销已经推送的提交

```powershell
git revert HEAD
git push origin main
```

> `git revert` 会创建一个新的反向提交，通常比改写已经共享的提交历史更安全。

### 远程仓库管理

#### 1. 查看远程仓库地址

```powershell
git remote -v
```

#### 2. 修改远程仓库地址

```powershell
git remote set-url origin 你的仓库链接.git
```

#### 3. 删除远程关联

```powershell
git remote remove origin
```

> 这只会删除本地的远程关联，不会删除 GitHub 上的仓库。

#### 4. 重新添加远程关联

```powershell
git remote add origin 你的仓库链接.git
```

#### 5. 获取远程更新，但不合并

```powershell
git fetch origin
```

#### 6. 查看远程仓库分支

```powershell
git branch -r
```

#### 7. 推送所有本地分支

```powershell
git push --all origin
```

#### 8. 推送所有标签

```powershell
git push --tags origin
```

### AboutME 日常一键同步流程

> 适用于本地 AboutME 文件修改完成后，提交并同步到 GitHub 的情况。

```powershell
cd D:\Project\AboutME

git add .
git commit -m "Update AboutME"
git pull --rebase origin main
git push origin main
git status
```

> 如果提示 `nothing to commit, working tree clean`，说明没有新的本地修改，可以跳过 Commit，继续执行 Pull 和 Push。

## 常用命令速查表

| 目的 | 命令 |
|---|---|
| 初始化仓库 | `git init` |
| 查看状态 | `git status` |
| 添加全部文件 | `git add .` |
| 添加指定文件 | `git add README.md` |
| 创建提交 | `git commit -m "说明"` |
| 上传主分支 | `git push origin main` |
| 首次推送分支 | `git push -u origin 分支名` |
| 下载远程更新 | `git pull origin main` |
| 获取远程信息 | `git fetch origin` |
| 查看分支 | `git branch -a` |
| 创建分支 | `git branch 分支名` |
| 创建并切换 | `git switch -c 分支名` |
| 切换分支 | `git switch 分支名` |
| 删除本地分支 | `git branch -d 分支名` |
| 删除远程分支 | `git push origin --delete 分支名` |
| 合并分支 | `git merge 分支名` |
| 删除文件并提交 | `git rm 文件名` |
| 删除文件夹并提交 | `git rm -r 文件夹名` |
| 查看提交记录 | `git log --oneline` |
| 查看远程地址 | `git remote -v` |

### 数学公式

>行内公式：$E=mc^2$

块级公式：
$$
\frac{a}{b} = c
$$

### 脚注

这里有一个脚注[^1]。

[^1]: 脚注内容。

## 参考资料

- [CommonMark 规范](https://commonmark.org/)
- [GitHub Markdown 指南](https://docs.github.com/zh/get-started/writing-on-github)
- [Git 官方文档](https://git-scm.com/docs)

> 提示：多写多预览，语法很快就能掌握。
