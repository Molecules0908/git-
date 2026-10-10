# Git 使用入门

Git 常用命令速查，配合实际操作笔记整理。

## 初始化与连接远程仓库

```bash
git init                                   # 把当前目录变成 Git 仓库
git init -b main                           # 初始化时直接指定分支名为 main
git remote add origin <远程仓库地址>        # 关联远程仓库
git remote -v                              # 查看已关联的远程仓库
git remote set-url origin <新地址>          # 修改远程地址（HTTPS 换 SSH 时用）
```

## 提交更改

```bash
git status                                 # 查看哪些文件被改动了
git add <文件名>                            # 把某个文件加入暂存区
git add .                                  # 把当前目录所有改动加入暂存区
git commit -m "提交说明"                     # 提交到本地仓库
git commit --amend -m "新的提交说明"          # 修改上一次提交（还没 push 时用）
```

一次完整的提交流程：`git status` → `git add .` → `git commit -m "..."` → `git push`

## 推送与拉取

```bash
git push -u origin main                    # 首次推送并设置上游分支，之后只需 git push
git push                                   # 推送到已关联的上游分支
git pull                                   # 拉取远程最新代码并合并
git fetch                                  # 只下载远程更新，不自动合并
```

## 分支

```bash
git branch                                 # 查看本地分支
git branch -a                              # 查看所有分支（含远程）
git switch -c <分支名>                      # 新建并切换到该分支
git switch <分支名>                         # 切换分支
git merge <分支名>                          # 把某分支合并到当前分支
git branch -d <分支名>                      # 删除已合并的分支
```

## 查看历史

```bash
git log                                    # 查看提交历史
git log --oneline                          # 每条提交压缩成一行
git log --oneline --graph --all            # 带分支图的历史
git show <提交号>                           # 查看某次提交的具体改动
git diff                                   # 查看尚未暂存的改动
```

## 撤销操作

```bash
git restore <文件名>                        # 丢弃工作区的改动，回到上次提交的状态
git restore --staged <文件名>               # 把文件从暂存区撤回来（不改动文件内容）
git reset --soft HEAD^                     # 撤销上一次提交，改动保留在暂存区
git reset --hard HEAD^                     # 撤销上一次提交，改动全部丢弃（谨慎使用）
```

## 配置

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
git config --global --list                 # 查看全局配置
git config --global init.defaultBranch main  # 让新仓库默认分支为 main
```

## 忽略文件

在项目根目录建 `.gitignore`，每行写一个规则，被匹配的文件不会被 Git 跟踪：

```
node_modules/
.DS_Store
*.log
.env
```

> `.gitignore` 只对「还没被跟踪」的文件生效。如果文件已经被提交过，需要先 `git rm --cached <文件名>` 再提交。

## 常见坑

- **仓库名冲突**：`git remote add origin` 前先 `git remote -v` 确认是否已关联过，重复添加会报 `remote origin already exists`。
- **推送被拒绝**：本地落后于远程时先 `git pull` 再 `git push`。
- **HTTPS 密码认证已废弃**：GitHub 不再接受账号密码，需要用 Personal Access Token 或改用 SSH。
- **中文文件名乱码**：`git config --global core.quotepath false`。
