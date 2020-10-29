---
title: Git之多个commit合并
date: 2020-10-28 14:57:58
categories: Git
tags:  Git 学习
---

## 净化 commit 提交

把多次的 commit 合并成一个commit 记录。

### 场景描述

有时候我们修改一个 `Bug` 或者一段代码的时候， `commit` 一次之后，发现 `Bug` 没改对或者这段代码需要再优化之类的，改完之后又 `commit` 了一次或多次，这样就会感觉提交历史不太美观（有点强迫症），这个时候我们就希望只想保留一次提交历史记录，合并为一个完整的提交，该怎么办呢？`git rebase` 应运而生！

### 步骤

#### 查看历史，确定要合并的提交

首先你要先确定你要修改的提交 `commit id` , 这个我们可以使用 `git log --oneline` 打印查看。

> 注意： 不要合并其他人的提交

#### 命令合并

命令行输入：`git rebase -i HEAD~6` 或者 `git rebase -i ***`

> `***` 指的是你的某个提交 `commit id`
> **注意:** 这个`commit id` 不包含在内
> `HEAD~6` 指的是最近的 6 次提交 , 这个数字 6 代表几次， 如下图 ，我是修改前3次，所以是 3

输入 以上命令后,命令行输出如下图内容:

![内容输出](/images/rebase-one.png)

键盘 i 进入 vim 编辑模式, 把 除了第一个 `commit message` 之外的 `pick` 修改为 `squash` or `s` , 然后 esc 退出 vim 编辑模式后键盘输入 `:wq` or `x` 保存退出，如下图：

![s](/images/rebase2.png)

这里说明下， 为什么除了第一个不用改 是因为要把下面的两次 commit 合并到第一个 commit 去。

- `pick` 的意思是要执行这个 commit
- `squash` 的意思是这个 commit 会被合并到前一个 commit

#### 中间可能会出现的情况

git 会压缩提交历史，若有冲突，需要进行修改，修改的时候保留最新的历史记录，修改完之后输入以下命令：

    git add .
    git rebase --continue

若想退出放弃此次压缩，执行命令：

    git rebase --abort

若无冲突 or 冲突已 fix，则会出现一个 `commit message` 编辑页面，修改 `commit message` ，然后 输入`:wq` or `x` 保存退出。

#### 同步到远程 git 仓库

输入：`git push -f` or `git push --force`

查看远程仓库效果，多次 commit 已被合并成一次 commit。
