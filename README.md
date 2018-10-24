# LearnGit
测试Git使用的练习册

## 简介

git的理论学习了很多。但是在实际使用过程中仍有很多未能解决的谜题。究其原因，git的项目管理能力太过于强大，又太过于灵活。导致面临的复杂情况比之svn有指数级别的增长。想要像学习svn一样学习git，通过一段时间的理论学习来理解其理论基础，然后就期望能做到一法通万法通的效果。这是不现实的，如果说svn的版本复杂度是树，枝枝杈杈就那么多，一目了然；那么git的版本复杂度兼职就是竹，看起来是一根一根的，实际上整片竹林都是同一条根生长出来的。要想透彻的理解git，除了需要对git版本管理的主体思想了然于胸之外。还需要进行大量的实践联系，并在实践中不断与理论理解相互印证。如此一段时间之后，才能做到真正入门。

## 第一天

### 创建仓库

github创建仓库的操作通常是在服务器端通过页面创建。具体操作只需要在个人的默认页面：`https://github.com`中，在Repositories面板中，选择【New repository】按钮即可。具体操作非常简单，此处不再赘述。创建仓库成功之后我们就有了一个远程仓库,仓库的地址是`https://github.com/dfiver/learngit.git`。远程仓库的主分支通常被称为`origin\master`。

### 根据远程仓库创建本地仓库

在希望创建创建本地仓库的目录下。使用命令：`git clone https://github.com/dfiver/learngit.git`即可创建本地仓库。本地仓库创建成功后，当前目录下会有一个`.git`的文件夹，这个文件夹里面存储的是仓库的信息。另外会有一个名为learngit的文件夹（与仓库的名字同名），这个是工作空间。本地仓库创建成功后，同步会创建一个与`origion/master`远程主分支对应的本地主分支`master`。

### 添加文件

通过`touch README.md`为项目添加一个文件。在README.md中，录入文档标题等说明性文字并保存。

> 此时通过'git checkout -b 1stday'命令创建并切换到新分支，由于master分支上存在未暂存的修改。那切换能否成功呢？如果成功了，此时1stday分支上是否有README.md这个文件呢？
>
> 答：由于1stday分支是从master分支创建出来的。因此，master上未暂存、未提交的修改都可以直接被带到1stday分支上。并且，可以在1stday分支上进行提交。如果这些修改在1stday分支上提交了。则master分支的log中是看不到这些修改的。之在1stday分支上能看到。这对于同时创建2个分支，用于开发两个不同的功能来说，是不能接受的。因此，如果在开发时本地同时创建了2个分支，用于开发不相干的两个功能。那么在进行分支切换之前，请务必确保当前分支已经提交。

### 提交本地修改

> 我们仍然回到`master`分支进行提交。如果之前切换到了1stday分支。只需要通过`git checkout master`命令，即可切换回master分支。之气在1stday分支上未暂存和未提交的修改，都可以直接代回到master分支。当然，如果已经提交了，那就带不回来了。好了，假设目前我们已经回到了master分支上。

使用命令`git commit -m "修改说明"`可以将本地修改提交到本地仓库。提交成功之后，本地仓库`origin/master`的版本会领先远程仓库1个提交。通常可以通过`push`命令来将本地仓库的提交推送到远程仓库中。

### 推送到远程仓库

如果本地分支的版本领先与远程分支，可以通过`git push`命令将本地分支的修改推送至远程分支。如何查看本地分支和远程分支的版本呢？可以通过'git log'命令。

## 第二天

### 创建分支

创建分支有两种方式：本地创建、远程创建。先看一下本地创建的方法。

#### 创建本地分支

通过命令`git checkout -b 分支名`可以在本地仓库上基于当前分支创建一个新分支。并且切换到该分支上。此处可以扩展思考的地方有两点。
1. 此前在`添加文件`这一小节中，曾经提到过：基于当前分支创建分支，并切换到新分支上之后。原有分支上为暂存和未提交的修改，均会被带到新分支上。
2. 由于我们之前的master分支是通过远程创建，然后clone到本地的，因此master分支存在其remote分支:`origin/master`，而新创建的分支(如果我们称其为2edday)由于是在本地创建，因此是没有远程分支与其对应的。此时，在2edday分支上，是不能进行push操作的。如果一定要操作，系统会提示先创建远程分支。并给出命令建议类似这样：`git push --set-upstream origin 2edday`。通过这样一个命令，实际上是给当前分支设置了远程分支，并进行了push操作。
> 设置远程分支的命令是:`git branch --set-upstream-to origion/分支名`

> `git push --set-upstream origin 2edday`还有一个简化的写法：`git push -u origin 2edday`

### 创建远程分支

这里说的创建远程分支是指直接从远程的origin/master上clone出一个远程分支。这种操作通常需要通过git服务提供商的页面，例如github或者gitlab都能够提供该功能。这个功能有一个更加耳熟能详的名字`fork`。实际上，fork并不是一个git命令。只是git服务提供商用来描述在远程直接clone出分支这一行为而创建出的一个概念。

#### pull-request工作流

无论github还是gitlab，都支持名为`pull-request`（或者简称PR）的git版本管理流程（也是开发流程或者多人协作流程）。该流程的主要步骤如下：
1. 开发人员基于主分支(origin/master)创建自己的远程分支。这一步骤通常被称为`fork`操作，无论github还是gitlab，都支持在界面上进行fork操作。
2. 开发人员将fork出来的分支clone到本地。
3. 开发人员在本地分支上完成开发工作

