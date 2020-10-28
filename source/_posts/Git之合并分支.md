---
title: Git之合并分支
date: 2020-10-28 16:03:49
tags: Git 学习
---

## 分支的合并

[原文链接](https://backlog.com/git-tutorial/cn/stepup/stepup1_1.html)

完成作业后的分支，最后要合并回merge分支。合并分支有2种方法：使用merge或rebase。使用这2种方法，合并后分支的历史记录会有很大的差别。

## merge

使用merge可以合并多个历史记录的流程。

如下图所示，bugfix分支是从master分支分叉出来的。

![bugfix](/images/bugfix.png)

合并 bugfix分支到master分支时，如果master分支的状态没有被更改过，那么这个合并是非常简单的。 bugfix分支的历史记录包含master分支所有的历史记录，所以通过把master分支的位置移动到bugfix的最新分支上，Git 就会合并。这样的合并被称为fast-forward（快进）合并。

![bugfix1](/images/bugfix1.png)

但是，master分支的历史记录有可能在bugfix分支分叉出去后有新的更新。这种情况下，要把master分支的修改内容和bugfix分支的修改内容汇合起来。

![bugfix2](/images/bugfix2.png)

因此，合并两个修改会生成一个提交。这时，master分支的HEAD会移动到该提交上。

![bugfix3](/images/bugfix3.png)

执行合并时，如果设定了non fast-forward选项，即使在能够fast-forward合并的情况下也会生成新的提交并合并。

![bugfix4](/images/bugfix4.png)

执行non fast-forward后，分支会维持原状。那么要查明在这个分支里的操作就很容易了。

---

## rebase

跟merge的例子一样，如下图所示，bugfix分支是从master分支分叉出来的。

![rebase](/images/rebase.png)

如果使用rebase方法进行分支合并，会出现下图所显示的历史记录。现在我们来简单地讲解一下合并的流程吧。

![rebase1](/images/rebase1.png)

首先，rebase bugfix分支到master分支, bugfix分支的历史记录会添加在master分支的后面。如图所示，历史记录成一条线，相当整洁。

这时移动提交X和Y有可能会发生冲突，所以需要修改各自的提交时发生冲突的部分。

![rebase2](/images/rebase3.png)

rebase之后，master的HEAD位置不变。因此，要合并master分支和bugfix分支，即是将master的HEAD移动到bugfix的HEAD这里。

![rebase3](/images/rebase4.png)

Merge和rebase都是合并历史记录，但是各自的特征不同。

- merge
  保持修改内容的历史记录，但是历史记录会很复杂。
- rebase
  历史记录简单，是在原有提交的基础上将差异内容反映进去。
  因此，可能导致原本的提交内容无法正常运行。

---

上述是 merge 跟 rebase 的区别之处，接下来 ，我们通过实践来加深下体会。

## 实践

### 使用 merge 合并分支

    mkdir test
    cd test
    git init

使用 `vscode` 打开 test 文件夹后新建文件 `index.txt` , 输入 文字：

    master分支新建

然后 git 提交:

    git add index.txt
    git commit -m '第一次提交'

你可以使用 `vscode` 的git分支图形可视化来观察当前的分支状态

![merge-vscode](/images/merge-1.png)

目前的历史记录是这样的。

### 建立分支

命令: `git branch <branchname>`

创建名为dev的分支。

`git branch dev`

不指定参数直接执行branch命令的话，可以显示分支列表。 前面有*的就是现在的分支。

      git branch
        issue1
      * master

创建分支后,目前的历史记录是这样的：

![merge-vscode](/images/merge-2.png)

### 切换分支

若要在新建的dev分支进行提交，需要切换到dev分支。

要执行checkout命令以退出分支。

`git checkout <branch>`

    git checkout dev

创建分支并切换到该分支可以把上面创建和切换两个命令合并为一个：

    git checkout -b <branch>

在 dev 分支上 修改 index.txt 文件 , 新增内容：

    master分支新建
    dev分支新建  // 新增内容

然后提交。

目前的历史记录是这样的。

![alt](/images/merge-3.png)

### merge 合并分支

先切换至 `master`分支:

    git checkout master

然后执行命令合并dev分支：

    git merge dev

如图, `master`分支指向的提交移动到和dev同样的位置。
这个是fast-forward（快进）合并。

![alt](/images/merge-4.png)

### 删除分支

`git branch -d <branch>` 命令删除分支 , 现在我要删除 dev ：

    git branch -d dev

确认是否删除成功：

    git branch
    * master

### 并行操作

在日常开发中， 我们可能会存在多个分支，多个分支中可能会存在并行的情况， 接下来看看merge后的结果会是怎么样。

同时创建两个分支，一个 dev 分支， 一个 bugfix 分支：

    git branch dev
    git branch bugfix

现在是这个样子：

![alt](/images/more-merger.png)

假设我们现在 切换到 dev 上我们进行开发，在 index.txt 文件上新增内容并提交。

这个时候 分支变成这个样子：
![alt](/images/more-dev.png)

然后我们再切换到 bugfix 分支 ，进行一些代码的修改， 并且文件还是 index.txt。

这个时候 分支变成这个样子：
![alt](/images/more-bugfix.png)

现在 dev 跟 bugfix 分支的操作就并行进行了。

### 解决合并的冲突
  
  现在我们把 dev 跟 bugfix 分支 合并入master去。
  切换到 master 分支， merge dev分支。
  
    git checkout master
    git merge dev

执行fast-forward（快进）合并。

![alt](/images/dev-1.png)

接着再合并 bugfix :

    git merge bugfix

在 dev 上 和 bugfix 上 我们都修改了 index.txt 文件， 那么冲突就此产生了。

![alt](/images/bugfix-1.png)

解决冲突，重新提交代码(这里选择了保留两个的输入)：

  git add index.txt
  git commit -m '解决冲突'

历史记录如下图所示。因为在这次合并中修改了冲突部分，所以会重新创建合并修改的提交记录。这样，master的HEAD就移动到这里了。这种合并不是fast-forward合并，而是non fast-forward合并。

![alt](/images/dev-2.png)

至此 ，merge 讲解结束了。

---

### rebase 合并分支

我们在master 合并完dev分支的时候， 使用 rebase 来合并 bugfix 分支，看看会有什么样的效果。

使用命令 回退刚刚我们合并 bugfix 的历史记录， 让 master 回归到合并dev的记录那时候：

    git reset --hard HEAD~ 

回退后， 我们首先要切换到 bugfix 分支去。
对master执行rebase。

    git checkout bugfix
    git rebase master

和merge时的操作相同，修改在 index.txt 发生冲突的部分。
冲突解决完毕之后， 重新提交我们刚刚所修改的文件。

    git add index.txt

rebase的时候，修改冲突后的提交不是使用commit命令，而是执行rebase命令指定 --continue选项。若要取消rebase，指定 --abort选项。

    git rebase --continue

如图，历史记录很干净了。

![alt](/images/bugfix001.png)

这样，在master分支的bugfix分支就可以fast-forward合并了。切换到master分支后执行合并。

    git checkout master
    git merge bugfix

这就是 rebase 结合 merge 使 分支历史记录变得很干净了。
