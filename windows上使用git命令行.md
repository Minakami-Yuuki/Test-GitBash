# windows上使用git命令行

记录一下windows上git命令行，方便以后查看！

## 安装

在git官网上下载[安装程序](https://git-scm.com/downloads)，然后默认选项安装即可。
安装成功后，可以在桌面右面看到Git Bash Here。点击后跳出类似命令行窗口的东西，表示安装成功！

安装完成后，还需要最后一步设置，在命令行输入：

```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
12
```

注意：工作区，暂存区，本地仓库，远程仓库

## 创建版本库

```
git init
1
```

## 添加文件到Git仓库

添加file到暂存区（可反复多次使用，添加多个文件）

```
git add <file>
1
```

git add -A提交所有变化；

git add .提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件

git add -u提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)

文件提交到本地仓库（-m后面输入的是本次提交的说明，可以输入任意内容）

```
git commit -m "message"
1
```

每次使用git commit命令我们都会在本地版本库生成一个40位的哈希值，这个哈希值也叫commit-id，
commit-id在版本回退的时候是非常有用的，它相当于一个快照,可以在未来的任何时候通过与git reset的组合命令回到这里

## 查看本地信息

查看本地仓库与工作区，暂存区的区别（随时掌握工作区的状态）

```
git status
1
```

查看工作区(work dict)和暂存区(stage)的file文件改变内容

```
git diff <file>
1
```

查看暂存区(stage)和本地仓库的区别

```
git diff –cached 
1
```

查看工作区和本地仓库里面最新版本的区别（HEAD表示当前版本）

```
git diff HEAD -- readme.txt 
1
```

## 远程仓库

##### 先有本地库，后有远程库的时候，如何关联远程库。

关联远程库

```
 git remote add origin git@github.com:doc/abc.git
1
```

推送内容到远程库（第一次推送master分支时，加上了-u参数，关联本地的master分支和远程的master分支）

```
git push -u origin master
1
```

##### 先创建远程库，然后，从远程库克隆

克隆一个仓库

```
git clone git@github.com:doc/abc.git
1
```

##### 在本地创建和远程分支对应的分支

```
git checkout -b dev origin/dev
1
```

##### 查看信息

查看远程库的信息

```
git remote
1
```

显示更详细的信息

```
git remote -v
1
```

##### 拉取分支内容

从远程获取最新版本并merge到本地

```
 git pull
1
```

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建

```
git branch --set-upstream-to <branch-name> origin/<branch-name>
1
```

从远程获取最新到本地，不会自动merge

```
git fetch
1
```

##### 推送分支（推送时，要指定本地分支）

```
git push origin master
1
```

master分支是主分支，因此要时刻与远程同步

dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

## 分支管理

HEAD指向的就是当前分支

创建dev分支，然后切换到dev分支

```
git checkout -b dev
1
```

命令加上-b参数表示创建并切换，相当于以下两条命令

```
git branch dev
git checkout dev
12
```

查看当前分支（列出所有分支，当前分支前面会标一个*号）

```
git branch
1
```

把dev分支的工作成果合并到master分支上（合并指定分支dev到当前分支）

```
git merge dev
1
```

强制禁用Fast forward模式（因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。）

```
git merge --no-ff -m "merge with no-ff" dev
1
```

删除dev分支

```
git branch -d dev
1
```

强行删除

```
 git branch -D dev
1
```

##### Bug分支

把当前工作现场“储藏”起来，等以后恢复现场后继续工作

```
git stash
1
```

查看工作现场

```
git stash list
1
```

恢复工作现场（stash内容并不删除）

```
git stash apply
1
```

需要删除时

```
git stash drop
1
```

或则，恢复的同时删除stash内容

```
git stash pop
1
```

开发新功能时，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

## 解决冲突

Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容

查看分支的合并情况

```
git log
1
```

输出信息简化信息（一大串类似1094adb…的是commit id）

git log --pretty=oneline

## 版本切换

Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100

退回到上上一个版本

```
git reset --hard HEAD^
1
```

or（可以找回前面已经退回的版本，只要知道commit id）

```
git reset --hard 1094a
1
```

记录每一次命令（这样就可以找回不记得commit id的版本）

```
git reflog 
1
```

## 撤销

##### 丢弃工作区的修改

```
git checkout -- readme.txt
1
```

命令git checkout – readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。、

##### 暂存区的修改撤销掉（unstage），重新放回工作区

```
git reset HEAD readme.txt
1
```

##### 想要撤销commit 后的修改

参考版本切换

## 删除

## rebase操作

rebase操作可以把本地未push的分叉提交历史整理成直线

```
git rebase
1
```

## 标签管理

标签也是版本库的一个快照。(其实它就是指向某个commit的指针;但是分支可以移动，标签不能移动)

打一个新标签

```
git tag <tagname>
1
```

查看所有标签

```
git tag
1
```

针对某次commit打标签，f52c633为commit id

```
git tag <tagname> f52c633
1
```

创建带有说明的标签，用-a指定标签名，-m指定说明文字

```
git tag -a <tagname> -m "version 0.1 released" 1094adb
1
```

查看标签信息

```
git show <tagname>
1
```

删除标签

```
git tag -d <tagname>
1
```

推送某个标签到远程

```
git push origin <tagname>
1
```

或者，一次性推送全部尚未推送到远程的本地标签

```
git push origin --tags
1
```

##### 删除远程标签

先从本地删除

```
 git tag -d <tagname>
1
```

然后，远程删除（删除命令也是push）

```
git push origin :refs/tags/<tagname>
```