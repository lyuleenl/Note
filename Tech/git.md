
## git branch

### 删除本地分支
```
git branch -d <branch name>
```

## git stash 
可以将本地还没有提交的改动全部存储起来 并不提交
### git stash apply / git stash pop
这两个命令就可以将刚才暂存起来的内容还原了。但是这里有一个问题，就是 stash apply 和 pop 之间是不同的。
这里涉及到 stash 内部的实现机制，stash 内部其实是通过堆栈实现的。pop 对于堆栈而言很明确，就是弹出的意思。也就是说如果我们使用的是 pop，那么当我们 pop 之后，这条记录会在堆栈当中删除。而如果使用的是 apply 呢，记录不会从堆栈当中删除，仍然会保留下来。
一般情况下我使用 pop 多一些，但是 pop 也有缺点，比如 pop 没有办法选择应用的记录。我们可以使用 git stash list 来查看一下当前堆栈当中已经有的记录。
如果我们使用 git stash pop 的话，默认的是应用的栈顶的记录，也就是 stash@{0}。但如果我们使用 stash apply 的话，我们可以自由选择我们想要应用的记录。比如如果我们想要应用最后一条记录的话，我们可以这样：
```
git stash apply stash@{2}
```
## git push

### 撤销add
如果是某个文件回滚到上一次操作：  
```
git reset HEAD  文件名
```
### 撤销commit
```
git reset --soft HEAD~1  //windows的bash
```

#### 撤销push操作
重置至指定版本的提交，达到撤销提交的目的
git reset –-soft <版本号>


## git commit 

#### 使用amend命令修改commit信息（注： amend命令只会修改最后一次commit的信息，之前的commit需要使用rebase）
```
git commit --amend --reset-author
```
#### 查看提交历史记录
*git log*

查看所有的commit提交记录

加上参数  --pretty=oneline，只会显示版本号和提交时的备注信息

*git reflog*

 可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）

*git show*

 查看提交的详情

- 1.查看最新的commit

*git show*

- 2.查看指定commit hashID的所有修改：

*git show < commitId >*

- 3.查看某次commit中具体某个文件的修改：

*git show < commitId fileName >*

## git 其他

### git add . 与 git add * 的区别    
- 1
git add . 按.gitignore规则全部提交。不会提示.gitignore
git add * 与add .  不一样的在于会提示已被忽略的内容。

![](https://img2020.cnblogs.com/blog/2130168/202009/2130168-20200901152637965-560450371.png)
- 2
git add .默认添加. 开头的文件  例如.gitignore   git add * 忽略. 开头的文件

### git中出现 > 这个符号怎么退出？

ctrl + d 即可退出。

这个表示没有输入完成，输入没有闭合。比如，只输入了一边的双引号或单引号。
### 如何重置本项目用户信息：
```
git config user.name 'your name'
git config user.email xx@email.com
```
### git为本地分支设置对应的远程分支
当运行git pull时，如果本地分支没有绑定远程分支，将无法正常pull。
运行命令如下：
```
git branch --set-upstream-to=origin/branchName
```
### Git中用vim打开、修改、保存文件
一、vim 有两种工作模式： 
1.命令模式：接受、执行 vim操作命令的模式，打开文件后的默认模式； 

2.编辑模式：对打开的文件内容进行 增、删、改 操作的模式；

3.在编辑模式下按下ESC键，回退到命令模式；在命令模式下按i，进入编辑模式

二、创建、打开文件：

1.输入 touch 文件名 ，可创建文件。

2.使用 vim 加文件路径（或文件名）的模式打开文件，如果文件存在则打开现有文件，如果文件不存在则新建文件。 

3.键盘输入字母i进入插入编辑模式。

三、保存文件： 

1.在编辑模式下编辑文件 

2.按下ESC键，退出编辑模式，切换到命令模式。 

3.在命令模式下键入"ZZ"或者":wq"保存修改并且退出 vim。 

4.如果只想保存文件，则键入":w"，回车后底行会提示写入操作结果，并保持停留在命令模式。

四、放弃所有文件修改： 
1.放弃所有文件修改：按下ESC键进入命令模式，键入":q!"回车后放弃修改并退出vim。 

2.放弃所有文件修改，但不退出 vi，即回退到文件打开后最后一次保存操作的状态，继续进行文件操作：按下ESC键进入命令模式，键入":e!"，回车后回到命令模式。

五、查看文件内容：

在git窗口，输入命令：cat 文件名

 六、创建文件夹

在git窗口，输入命令：touch 文件夹名

### 使用Git上传项目时需要输入用户名和密码
这里简述一下主要步骤：

step1.将上传代码的方式从 https 改成 ssh
<img src="https://i.loli.net/2020/11/21/f6q1YtxQbuHjTUP.png" width = "30%" hight="30%" />
使用命令：`git remote set-url origin git@github.com:LawsonAbs/luogu.git` 后面的用户和项目名需要根据你自己的情况而改变。

step2.在自己本地生成ssh公钥并写在github中指定项目的key中
`ssh-keygen -rsa -C "LawsonAbs"` 生成LawsonAbs这个用户的公钥。

step3.执行命令 `git push -u origin master`


### git 报错
#### fatal: This operation must be run in a work tree 
可能原因： 1.路径不正确
          2.git仓库没有正确创建
#### warning: LF will be replaced by CRLF in ****. The file will have its original line endings in y
警告原因：可能是你的项目使用了GitHub的开源项目，这个开源项目上传的环境（Linux）和你本次上传GitHub仓库的环境（Windows）不同
例如：本机上传的环境是win10    ，而在你本机项目中引用的GitHub项目上传环境是Linux

windows中的换行符为 CRLF，而在Linux下的换行符为LF

就会产生如上换行符不同的警告
但是这个错误可以直接忽略，对项目影响不大，我觉着如果以后项目在其他环境上运行的话，还可以通过这个方法改变换行符，是项目在其他环境中正常运行！

#### Automatic merge failed; fix conflicts and then commit the result.
自动合并失败；修改冲突然后提交修改后的结果。
<<<<<<<< HEAD

         你写的代码

===============

          别人写的代码

>>>>>>>>>>>>>>> sdhqd128dqwenasjdq

#### Another git process seems to be running in this repository

windows对于进程的同步互斥管理，是有资源上锁机制的。猜测这里肯定是有进程对某资源进行了加锁，但是由于进程突然崩溃，未来得及解锁，导致其他进程访问不了。
进入工作区目录下的隐藏文件.git，其中的index.lock文件删除掉，然后重新打开git bash进程，问题解决。


### .gitignore用法
```
# 忽略 .a 文件
*.a
# 但否定忽略 lib.a, 尽管已经在前面忽略了 .a 文件
!lib.a
# 仅在当前目录下忽略 TODO 文件， 但不包括子目录下的 subdir/TODO
/TODO
# 忽略 build/ 文件夹下的所有文件
build/
# 忽略 doc/notes.txt, 不包括 doc/server/arch.txt
doc/*.txt
# 忽略所有的 .pdf 文件 在 doc/ directory 下的
doc/**/*.pdf
```

[模板](https://github.com/github/gitignore)