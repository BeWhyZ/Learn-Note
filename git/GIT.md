# GIT
  在远端创建一个仓库。在仓库内创建分支master。  
  在本地创建分支master, 然后再创建分支dev
  在本地通过直接对文件进行内容修改，然后通过git add ''|<file\> 将修改的内容提交到暂存区。  
  而暂存区则会通过命令 git commit -m'<desc\>' 来将所有暂存区的修改提交到本地分支dev。
  确认好所有commit后，将dev的所有提交 通过先切换到master分支，再使用命令git merge dev来将内容同步到master分支。  
  最后通过git push <remote name:default_name-origin\> master
  来将本地master分支的所有commit提交内容提交到 远端master  
  值得注意的是，养成好习惯，在Push到远端操作前，一定要先进行一次Pull操作  

**关于Fork**

  fork则是在远端的项目A下，自己进行fork出另一个远程库name-A。所有的Push,pull操作仍然是对自己fork的远程库name-A进行操作。然后当把一些修改的内容想要同步给项目A时，则在fork的网站(github,gitee, gitbucket等等)上申请 创建一个新的pull request。  
  然后项目A的管理员来对这个pull request进行操作是否通过将其同步到项目A。(创建的pull request会有一个#xxx 值 创建好后可以给管理员说一下)  
  然后自己在操作自己的库 name-A时，为了对项目A的代码时时更新，最好创建一个分支用来每天pull项目A，然后再将分支代码整合到自己的master。也可以偷懒直接添加个远端 upstream，每次来

git pull upstream master 拉取最新的项目A代码。  

#### **添加远端**

```
git remote add xxx-name URL
```

#### **删除远端仓库文件**

```
git rm xxx
git commit 
git push
```

#### commit提交错误时

```
# 不删除本地修改，
git log
git reset HEAD~1
git log

# 删除本地修改
git reset --hard HEAD~
```

#### 修改很久以前的一次commit提交

  ```python
# 修改最近一次commit提交
# 对相应文件进行修改后，使用add
git add <file name>
git commit --amend # 该次commit提交会对最近一次的commit进行修改，不会产生新的log记录
# 对最近一次commit提交的-m"<desc>"描述进行修改
git commit --amend # 在打开的编辑器里直接对commit的comment进行修改

# 修改很久之前的一次提交，比如想要修改最近三次的提交或者是这三次提交中的最老的那一次的提交
# 同时，从别人pull merge进行的拉取在这个log中只算是一次commit。
git rebase -i HEAD~3 # 注意 -i 必须添加这个参数
# 然后进入文本编辑器，将要修改提交的pick改为edit，注意这里文本编辑器对Log记录的显示与git log相反
# 将pick修改为edit后 保存退出。
# 对文件进行修改，修改好后
git status # 来查看状态
git add <file name>
git commit --amend # 完成对edit 想要修改的那次提交
# 完成修改后
git status # 查看状态
git rebase --continue # 完成对历史的一条或者多条commit修改(多条便把想要修改的都pick 改为edit)

  ```

**注意：**对很久以前一次commit提交，这里使用到了rebase的操作。通过观看git log --pretty=format:"%h %s"看到一些pull merge历史被重新整合成新的提交。看起来很怪(因为本身已经在多人开发的过程中)。所以不推荐使用这种修改历史commit的方式。

**建议：** 

#### 忽略文件

```
# 使用下面命令查看.gitignore文件中的指令是否忽略了文件app.class
git check-ignore -v app.class
```

#### 工作区和暂存区
```
git add 将要提交的修改放到暂存区。

# 查看工作区状态
git status

再通过 git commit 一次性将暂存区的修改提交到分支。

# 查看工作区和版本库里面最新版本的区别
git diff HEAD

# 撤销工作区的修改
git checkout -- <file> # 让这个文件回到最近一次的commit或者add时的状态

# 将暂存区的修改撤销，重新放回到工作区
git reset HEAD <file> 
使用reset可以回退版本，同时可以将暂存区的修改回退到工作区。HEAD表示最新版本。

```

#### 版本回退
```
# 查看版本号
git log --pretty=oneline --graph --abbrev-commit 
git log --pretty=oneline 一行显示
git log --graph  显示分支合并情况
git --abbrev-commit 取少量commit_id

# 回退
git reset --hard 版本号
# 记录每一次git 命令， 来查找版本号
git reflog

git reset 版本号 默认软退回,将切换到的分支到当前分支所有修改在本地保存
```

#### 创建分支以及合并
```
# 创建一个分支，并且切换到该分支
git checkout -b dev
或者
git branch dev
git checkout dev

# 查看当前分支
git branch # 有*号标识的是当前分支

# 切换到需要进行合并的分支
git checkout master

# 进行将指定分支合并到当前分支,合并后会将本次合并做一次提交。
git merge dev
合并信息：Fast-forward表示这次合并是快进模式，直接将master指向dev

# 合并完成后可以进行删除分支
git branch -d dev

# 删除没有被合并的分支
git branch -D <branch name>
```

#### 关于冲突
```
git merge dev
出现冲突时
使用 git status 查看冲突
<<<HEAD === >>>dev

解决冲突 git add <file> 解决好的冲突文件
git commit 
```

#### 分支管理策略
```
git checkout master 
git merge dev
默认使用 fast-forward模式，该模式会将dev分支信息删除, 看不出有合并的历史
git log --graph --pretty=oneline --abbrec-commit

使用--no-ff来禁用fast-forward模式,可以看出有合并的历史
git merge dev --no-ff
git log --graph --pretty=oneline --abbrec-commit

```

#### BUG分支
正在工作手头上的工作时，出现一个需要立即解决的bug时，创建一个bug分支，并将Bug修复好后合并。然后继续进行手头上的工作。  

```
# 将当前所有进行的工作储藏
git stash
```
在bug出现的分支上创建bug分支(例如dev上的Bug则去到dev创建该分支。master则去master) issue-101 解决后将代码add commit。再切回分支，并将fixbug进行合并  
```
git merge --no-ff -m'merfe fix-bug-101' issue-101
```
然后删除 issue-101分支  
切回原来工作的分支  
```
# 查看stash存储的状态
git stash list

# 找到后，进行恢复,一共两种恢复.(在恢复的时候，stash list中stash 不会自动删除)
1.git stash apply <stash>  git stash drop <stash># 手动删除

2.git stash pop <stash> # 恢复并自动删除stash

```

#### Feature分支
开发的时候，出现新功能将创建一个新功能的分支，并将功能调试好后再合并到master。

#### 多人开协作开发
```
# 查看远程分支
git remote -v

# 推送本地分支master所有提交 到 远程分支(远程分支默认名字是origin)
git push origin master 

# 设置本地dev分支与远程origin/dev分支链接
git branch --set-upstream-to=origin/dev dev
```

多人开发时，在本地想要push到远程分支前，一定先要进行 git pull拉取操作之后再进行Push。有冲突时，按照上面的解决方法解决冲突。  

#### Rebase 变基

```
git rebase
```
rebase操作可以把本地未push的分叉提交历史整理成直线；  
rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。  

变基的一大好处，例如在修复一个Bug时所创建的 fix-101分支，通过git merge fix-101会有一个分叉的commit历史记录，看着会对本地的master log日志不是线性。为了使好看可以使用rebase。具体操作如下

```
git branch

git checkout master

git rebase dev

git log --pretty=oneline --graph --abbrev-commit
# 此时应该就是线性的log日志了
git branch -d dev
```



#### tag 标签管理
发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。  

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。  

```
# 创建标签(切换到要打标签的分支后再创建),默认打在最新的commit上
git tag v1.0

# 查看所有标签
git tag

# 打到某个commit上。先通过 git log --pretty=oneline --abbrev-commit 找到commit id
git tag v0.9 <commit id>

# 创建有说明desc的tag
git tag -a <tag name> -m '<desc>'

# 查看说明desc
git show <tag name>

# 推送本地标签到远端
git push origin <tag name>

# 一次性推送所有tag到远端
git push origin --tags

# 删除一个本地标签
git tag -d <tag name>

# 删除一个远程标签(先删除本地标签再删除remote)
git push origin :refs/tags/<tag name>

```

#### 搭建自己的Git服务器
这里有操作：<https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664>

#### 总结
笔记参考自：<https://www.liaoxuefeng.com/wiki/896043488029600>,

[https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E5%86%99%E5%8E%86%E5%8F%B2]()

