---
title: Git之修改commit的message
date: 2020-10-28 16:02:39
categories: Git
tags: Git 学习
---

## 修改commit message

上面我们讲的是如何在自己的分支 净化 commit 提交记录， 接下来 讲讲如何 利用 `rebase` 修改 commit message 的信息。

### 场景描述

在使用git提交代码的时候，可能会出现message写错的情况，
如果此时commit已经push到远程服务器了，
修改起来就比较麻烦了。

**注**：
在修复历史commit message的时候，请确保当前分支是最新代码，
且已经提交了所有本地修改。

### 步骤

#### 查看历史，确定要修改的提交

首先你要先确定你要修改的提交 commit id , 这个我们可以使用 `git log --oneline` 打印查看。

#### 命令

使用`git rebase -i HEAD~5`确定要修改哪些commit

跟上篇文章 《Git之多个commit合并.md》 其实一样的步骤。

但是 需要把 之前的 `squash` 更改为 `edit`

假设现在你要更改最近的 5次提交message ， 那么在这最近5次的 pick 更改 为 edit , 然后保存退出后， 你会注意到 git 分支会变成 第一个edit的 commit id, 例如：

    test-rebase git:(master) > // 这个是之前在master
    test-rebase git:(f429786) >  // 保存后会变成 commit id

然后在这个 id 下 ，我们输入命令： `git commit --amend`
回车后会进入 vim 模式就可以修改了。

修改保存之后， 再次在命令行输入: `git rebase --continue`

命令执行完 git 分支会进入第二个（如果有的话）, 重复上面的操作：

- 先用git commit --amend修改message，然后保存，
- 再执行，git rebase --continue。

我们标记了几个edit，这个过程就需要重复执行几次。
全部修改完成后，会提示，

    Successfully rebased and updated refs/heads/master.

#### 使用git push -f强制更新远程服务器

    git push -f

切记一定要加`-f`，否则我们edit的commit会添加到commit后面，
而不是更新原commit。
