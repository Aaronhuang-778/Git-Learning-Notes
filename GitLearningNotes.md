# GIT Learning Notes

## 1.Git的简介

1、Git是由C语言写成的分布式版本控制系统。

2、集中式特点：需要从中央服务器取出和放入，安全性不高，需要联网。

3、分布式特点：各自电脑有版本库，修改后需要相互传送更新。

## 2.创建仓库（repository）

```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

1、`pwd`命令用于显示当前目录。

```
MECHREVO@AaronAurora MINGW64 ~/learngit
$ pwd
/c/Users/MECHREVO/learngit

```

2、以下代码（`git init`）可以将这个目录变成Git可以管理的仓库，瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

```git
MECHREVO@AaronAurora MINGW64 ~/learngit
$ git init
Initialized empty Git repository in C:/Users/MECHREVO/learngit/.git/

```

3、版本控制系统只能跟踪文本文件的改动，图片、音频无法知道改动内容。

### 1.写一个.txt文件

第一步：用 `git add`告诉Git，把文件添加到仓库

```
$ git add readme.txt
```

第二步：用`git commit`告诉Git，把文件提交到仓库

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

其中`-m`输入的是本次提交的说明，可以方便以后查阅历史记录

可以用`ls`或者`dir`查看当前目录的文件

## 3.对仓库文件的操作

### 1.状态查询

`git status`

`git diff`

### 2.版本回退

1、输入`git log`来查看修改的记录（由近到远）



    $ git log
    commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 21:06:15 2018 +0800
    
        append GPL
    
    commit e475afc93c209a690c39c13a46716e8fa000c366
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 21:03:36 2018 +0800
    
        add distributed
    
    commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
    Author: Michael Liao <askxuefeng@gmail.com>
    Date:   Fri May 18 20:59:18 2018 +0800
    
        wrote a readme file

2、如果只需要修改的信息则（只显示部分信息）

```
$ git log --pretty=oneline
```

3、查看当前文件内容

```
$ cat readme.txt
```

4、查看每一次命令

```
$ git reflog
```

5、回退一个版本

```
$ git reset --hard HEAD^
```

6、去到任意的版本

原理：调节head指针的位置

```
$ git reset --hard ’commit id'（其中为版本id）^
```

### 3.工作区和暂存区

git add用来将文件添加到暂存区



![git-stage](https://www.liaoxuefeng.com/files/attachments/919020074026336/0)

然后用git commit将暂存区中的文件提交到当前分支

![git-stage-after-commit](https://www.liaoxuefeng.com/files/attachments/919020100829536/0)

### 4.管理修改

​	每次git commit的只能是从 stage中来的版本，不能跳过add过程直接从工作区添加到仓库。

### 5.撤销修改

1、在添加到stage之前（git add操作之前）修改

原理：用版本库里的内容替换掉工作区里的内容。

```
$ git checkout -- readme.txt
```

2、在加入到stage之后未提交时修改

```
$ git reset HEAD readme.txt
```

### 6.删除文件

将已经add并commit到仓库的文件删除

1、直接将文件给删除

```
$ rm test.txt
```

2、从stage中删除

```
$ git rm test.txt
```

## 4.远程仓库

创建方式：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

输入自己的邮箱，一路回车（或设定密码），确定默认目录

### 1.添加远程仓库

第一步：现在Github上新建一个repository，其名称就输入我们新的仓库文件名如：learngit

第二步：按照github提示要求将本地仓库和github新建仓库关联起来

注意：第一次传送本地库的时候

```
$ git push -u origin main
```

`-u`可以将本地的main指针和远程的main指针关联起来

之后每次更新内容只需要以下操作即可

```
$ git push origin master
```

### 2.远程库克隆

第一步：首先在Github上新建一个库

第二步：进行本地克隆

```
MECHREVO@AaronAurora MINGW64 /g
$ git clone http://github.com/Aaronhuang-778/Git-Learning-Notes.git
```

## 5.分支管理

### 1.创建与合并分支

1、创建并切换到一个新的分支

```
$ git switch -c dev
```

2、直接切换到已有的分支

```
$ git switch master
```

3、将某个分支的内容合并到当前分支

```
$ git merge dev
```

4、删除某个分支 

```
$ git branch -d dev
```

5、查看分支分布

```
$ git branch
```

6、创建分支

```
$ git branch <name>
```

### 2.解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

查看分支合并图：

```
$ git log --graph --pretty=oneline --abbrev-commit
```

### 3.分支管理策略

使用`--no-ff`参数来进行分支的合并，强制禁用Fast Forward模式

```
$ git merge --no-ff -m "merge with no-ff" dev
```

### 4.BUG分支

1、`git stash`储藏当前工作现场的功能

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merg
```

第二步：确定在哪一个分支上进行修改

```
$ git checkout master
```

然后创建新的需要修改的分支

```
$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

之后提交

```
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-
```

然后切回到master分支，完成合并，之后删除`issue-101`分支：

```
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

最后回到原来的dev工作环境中

```
$ git switch dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean
```

用`git stash list`查看原来的工作现场

```
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

这时候我们原有的工作保存在了stash（藏匿处）当中

恢复的的办法有两种：

第一种：先用`git stash apply`恢复

​				然后再用`git stash drop`来删除

第二种：用`git stash pop`，恢复的同时把stash内容也删了：

```
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

此时再用`git stash list`查看的时候就看不到任何stash当中的内容了

### 5.Feature分支

对于软件开发中添加新功能的时候可以先新建一个feature分支，在其上面进行开发，完成后合并，最后删除feature分支。

第一步：首先新建一个分支并进入到这个分支当中

```
$ git switch -c feature-vulcan
Switched to a new branch 'feature-vulcan'
```

第二步：开发完毕后将这个分支添加入stage然后提交

```
$ git add vulcan.py

$ git status
On branch feature-vulcan
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   vulcan.py

$ git commit -m "add feature vulcan"
[feature-vulcan 287773e] add feature vulcan
 1 file changed, 2 insertions(+)
 create mode 100644 vulcan.py
```

第三步：切换回dev然后准备合并

```
$ git switch dev
```

此时如果发生意外要强制删除feature-vulcan则需要运用-D

```
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```

### 6.多人协作

查看远程信息，用`git remote`

```
$ git remote
origin
```

或者，用`git remote -v`显示更详细的信息：

```
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push
```

推送分支的过程：

将该分支的所有本地提交到库当中，选择需要推送进仓库的分支名然后操作

```
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```
$ git push origin dev
```