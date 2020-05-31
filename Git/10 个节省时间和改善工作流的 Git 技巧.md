- [技巧1:优化配置](#技巧1优化配置)
  - [查找顺序](#查找顺序)
  - [修改配置](#修改配置)
  - [显示当前设置](#显示当前设置)
  - [一些有用的配置](#一些有用的配置)
- [技巧2:别名(alias)](#技巧2别名alias)
  - [一些有用的别名](#一些有用的别名)
- [技巧 3：查找 Commits 和更改](#技巧-3查找-commits-和更改)
  - [通过commits信息查找](#通过commits信息查找)
  - [通过更改查找](#通过更改查找)
  - [通过日期查找](#通过日期查找)
- [技巧4:添加hunk](#技巧4添加hunk)
- [技巧 5： 储藏（stash）更改而不提交](#技巧-5-储藏stash更改而不提交)
  - [创建](#创建)
  - [罗列](#罗列)
  - [浏览](#浏览)
  - [应用](#应用)
  - [清理](#清理)
- [技巧 6：空运行（Dry Run）](#技巧-6空运行dry-run)
- [技巧 7：安全强制推送](#技巧-7安全强制推送)
- [技巧 8：修改 commit 信息](#技巧-8修改-commit-信息)
- [技巧 9：修改历史](#技巧-9修改历史)
- [技巧 10：存档跟踪文件](#技巧-10存档跟踪文件)
- [额外提醒：单破折号](#额外提醒单破折号)

[Git Pro](https://git-scm.com/book/zh/v2)

# 技巧1:优化配置

Git 在全局、用户和本地级别上都是高度可配置的。

[git配置文档](https://git-scm.com/docs/git-config)

## 查找顺序

每个设置都可以被覆盖：

```
本地级别:
项目文件夹/.git/config
用户级别:
用户目录/.config/git
用户目录/.gitconfig
全局级别：
git目录/etc/gitconfig
```

## 修改配置

```bash
# 全局设置
git config --global <keypath> <value>
# 本地设置
git config <keypath> <value>
```

## 显示当前设置

```bash
# 显示当前设置及其来源
git config --list --show-origin
```

## 一些有用的配置

```bash
# 设定身份
git config --global user.name"<your name>"
git config --global user.email <your email>
```

# 技巧2:别名(alias)

[Git 别名]([https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-Git-%E5%88%AB%E5%90%8D](https://git-scm.com/book/zh/v2/Git-基础-Git-别名))

创建别名来保存常用的git命令：

```bash
# 创建别名
git config --global alias.<alias-name> "<git command>"
# 使用别名
git <alias-name> <more optional arguments>
```

## 一些有用的别名

```bash
# 撤销上次提交
git config --global alias.undo "reset --soft HEAD^"
# 将暂存区更新修订到上次提交 (不改变提交信息)
git config --global alias.amend "commit --amend --no-edit"
# 压缩的状态输出
git config --global alias.st "status -sb"
# 用 GRAPH 为日志着色
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset'"
# 删除所有已合并的分支
git config --global alias.rmb "!git branch --merged | grep -v '*' | xargs -n 1 git branch -d"
# 贡献排行
git config --global alias.rank "shortlog -n -s --no-merges"
```

# 技巧 3：查找 Commits 和更改

## 通过commits信息查找

```bash
# 通过 commit 信息查找 (所有分支)
git log --all --grep='<search term>'
# 通过 commit 信息查找 (包含 reflog)
git log-g --grep='<search term>'
```

## 通过更改查找

```bash
# 通过更新的内容查找
git log -S '<search term>'
```

## 通过日期查找

```bash
# 通过日期范围查找
git log --after='DEC 152019' --until='JAN 102020'
```

# 技巧4:添加hunk

git add <filepath> 不仅能添加文件的所有变更， --path / -p 参数还可以交互式暂存区块。

```bash
# 补丁命令
y = 暂存区块
n = 不暂存这个区块
q = 退出
a = 暂存当前文件的此区块以及所有剩余区块
d = 不暂存当前文件的此区块以及所有剩余区块
/ = 查找区块 (正则表达式)
s = 划分成更小的区块
e = 手动编辑区块
? = 打印帮助说明
g = 选择要前往的区块
j = 将区块设为未定，查看下一个未定区块
J = 将区块设为未定，查看下一个区块
k = 将区块设为未定，查看上一个未定区块
J = 将区块设为未定，查看下一个区块
```

# 技巧 5： 储藏（stash）更改而不提交

stash 将当前的更改临时搁置起来。在它的帮助下，可以返回当前状态的索引，并能在稍后应用已储藏的更改。

默认情况下，仅储藏当前跟踪文件中的更改，新文件将被忽略。

我们可以独立地创建和应用多个 stash。

[Git 工具 - 储藏与清理]([https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%82%A8%E8%97%8F%E4%B8%8E%E6%B8%85%E7%90%86](https://git-scm.com/book/zh/v2/Git-工具-储藏与清理))

## 创建

```bash
# 创建新的 STASH
git stash
# 创建新的 STASH (包含未追踪的更改)
git stash -u/--include-untracked
# 创建新的 STASH 并命名
git stash save"<stash name>"
# 交互式储藏
git stash -p
```

## 罗列

```bash
# 列出所有的 STASH (为其他命令提供 "n")
git stash list
```

## 浏览

```bash
# 浏览 STASH 内容
git stash show
# 浏览 STASH 差异
git stash show -p
```

## 应用

```bash
# 应用上一个 STASH (删除 stash)
git stash pop
# 应用上一个 STASH (保留 stash)
git stash apply
# 应用特定的 STASH (n = stash 列表序号)
git stash pop/apply stash@{n}
# 从 STASH 创建新的分支 (n = stash 列表序号)
git stash branch <newbranch name> stash@{n}
# 从 STASH 应用单个文件 (n = stash 列表序号)
git checkout stash@{n} -- <filepath>
```

## 清理

```bash
# 删除特定的 STASH (n = stash 列表序号)
git stash drop stash@{n}
# 删除所有的 STASH
git stash clear
```

# 技巧 6：空运行（Dry Run）

许多 git 操作可能具有破坏性，例如， git clean -f 将删除所有未跟踪的文件，而且无法恢复。

要避免出现这种灾难性的结果，许多命令都支持 *dry-run* ，可以在实际产生结果前对其进行检查。不过遗憾的是，使用的选项不完全一致：

```bash
git clean -n/--dry-run
git add -n/--dry-run
git rm -n/--dry-run
# GIT MERGE 模拟 DRY-RUN
git merge --no-commit --no-ff <branch>
git diff --cached
git merge --abort
```

# 技巧 7：安全强制推送

在处理旧的 commit、创建新的 head 等情况时时很容易弄乱分支。 git push --force 可以覆盖远程变更，但不应该这样做！

git push --force 是一种具有破坏性且危险的操作，因为它无条件生效，并且会破坏其他提交者已经推送的所有 commit。这对于其他人的代码仓库来说不一定是致命的，但是改变历史记录并影响其他人并不是一个好主意。

更好的选择是使用 git push --force-with-lease 。

git 不会无条件地覆盖上游的远程仓库，而是检查是否有本地不可用的远程更改。如果有，它会失败并显示一条“stale info”消息，并告诉我们需要先运行 git fetch 。

[git push](https://git-scm.com/docs/git-push#Documentation/git-push.txt---force-with-leaseltrefnamegt)

# 技巧 8：修改 commit 信息

Commit 是不可变的，且不能更改。不过可以用一条新的 commit 信息修订现有的 commit，这会覆盖原始 commit，因此请勿在已推送的 commit 中使用它。

```bash
git commit --amend -m "<new commit message>"
```

# 技巧 9：修改历史

修改代码仓库的历史不仅限于修改上次提交信息，使用 git rebase 可以修改多个提交：

```bash
# 提交的范围
git rebase -i/--interactive HEAD~<number of commits>
# 该 hash 之后的所有提交
git rebase -i/--interactive <commit hash>
```

在配置的编辑器中倒序列出所有的 commit，像这样：

```bash
#<command><commit hash><commit message>
pick5df8fbc revamped logic
pick ca5154e README typos fixed
pick a104aff added awesome new feature
```

通过更改编辑器中的实际内容，可以为 git 提供一个方案，来说明如何进行 rebase：

```bash
# p, pick = 使用提交而不更改
# r, reword = 修改提交信息
# e, edit = 编辑提交
# s, squash = 汇合提交
# f, fixup = 类似 "squash"，但是会丢弃提交信息
# x, exec = 运行命令 (其余行)
# d, drop = 移除提交
```

保存编辑器后，git 将运行该方案以重写历史记录。

e, edit 会暂停 rebase，就可以编辑代码仓库的当前状态。完成编辑后，运行 git rebase --continue 。

如果过程中出现问题（例如合并冲突），我们需要重新开始，可以使用 git rebase --abort 。

[git-rebase](https://git-scm.com/docs/git-rebase)

# 技巧 10：存档跟踪文件

可以使用不同格式（ zip 或 tar ）来压缩特定引用的跟踪文件：

```bash

git archive --format<format> --output<filename> <ref>
```

<ref> 可以是一个分支、commit hash 或者一个标签。

[git-archive](https://git-scm.com/docs/git-archive)

# 额外提醒：单破折号

有一个快捷方式可以表示刚用过的分支：一个单破折号 -

```bash
git checkout my-branch
# 当前分支：my-branch
<dosome git operations, e.g. adding/commiting>
git checkout develop
# 当前分支：develop
git merge -
# 将 my-branch 合并到 develop
```

单破折号等同于 @{-1} 。

[git-checkout](https://git-scm.com/docs/git-checkout#Documentation/git-checkout.txt-ltbranchgt)