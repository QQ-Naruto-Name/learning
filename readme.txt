搭建git服务器（由于只有一台机器，故在本机上实验）
------------------克隆-----------------------------------
1. adduser git
2. cd /home/git
3. mkdir github
4. cd github
5. git init;vim readme.txt; git add readme.txt; git commit -m "create
readme.txt";
6. chown -R git:git .git
7. 本机上生成公私钥 ssh-keygen -t rsa -C "1643127918@qq.com"
（好像只能保存在/root/.ssh/id_rsa*, 过程中需要设定密码，后期会需要）
8. cd /home/other/
9. git clone git@localhost:github/.git
（会提示输入之前公私钥生成过程中设置的密码）
10. cd github 发现当前目录内容与/home/git/github相同


----------------拉取代码--------------------------------
1. cd /home/git/github
2. vim readme.txt; git add readme.txt; git commit -m "modify readme.txt";
3. cd /home/other/github
4. git pull (git
pull也失败了，原因可能是没有指定本地分支与远程origin相应分支的链接，例如git
branch --set-upstream dev origin/dev)
5. 查看当前目录下readme.txt是否发生改变


----------------提交代码(只能提交代码到空仓库，即无工作空间)--------------------------------
1. 先在/home/git下建立裸库 git init --bare sample.git
2. chown -R git:git sample.git
3. 在/home/mysql目前下执行 git clone git@localhost:sample.git
这样在/home/mysql目前下就出现sample目录
4. 在sample目录下添加readme.txt并提交到第一步的裸库（git push origin
master（因为clone时候已经建立了连接的对应关系））
5. 在/home/xxx目前下执行git clone
git@localhost:sample.git后，发现sample目录，并且在sample下有readme.txt文件




























安装完成后，还需要最后一步设置，在命令行输入：

$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
注意git
config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。
git init
git add readme.txt
git commit -m "comment for this operation"
git status
git log
git reflog
git reset --hard HEAD^/版本号前部分或整体
git diff HEAD -- readme.txt
git checkout -- readme.txt (回退到最近一次git commit或git add前的版本）

--从暂存区撤销修改后回退版本
git reset HEAD readme.txt  
git checkout -- readme.txt
git rm  readme.txt


要关联一个远程库，使用命令git remote add origin
git@server-name:path/repo-name.git；（git remote add origin
git@github.com:注册账号名/learngit.git）
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin
master推送最新修改；


远程克隆仓库到本地
git clone git@github.com:注册账号名/gitskills.git
git clone https://github.com/注册账号名/gitskills.git

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>

用git log --graph命令可以看到分支合并图： git log --graph --pretty=oneline
--abbrev-commit

分支的策略（非fast-forword）git merge --no-ff -m "merge with no-ff" dev

原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到dev分支干活了！

$ git checkout dev
Switched to branch 'dev'
$ git status
# On branch dev
nothing to commit (working directory clean)
工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：

$ git stash list
stash@{0}: WIP on dev: 6224937 add merge
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash
drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了：

$ git stash pop
再用git stash list查看，就看不到任何stash内容了：

$ git stash list
你可以多次stash，恢复的时候，先用git stash
list查看，然后恢复指定的stash，用命令：

$ git stash apply stash@{0}

小结

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git
stash pop，回到工作现场。

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

master分支是主分支，因此要时刻与远程同步；

dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。


多人协作的工作模式通常是这样：

首先，可以试图用git push origin branch-name推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果git pull提示“no tracking
information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch
--set-upstream branch-name origin/branch-name。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

小结

查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git
pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name
origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name
origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。


命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

git tag -a <tagname> -m "blablabla..."可以指定标签信息；

git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；

命令git tag可以查看所有标签。

命令git push origin <tagname>可以推送一个本地标签；

命令git push origin --tags可以推送全部未推送过的本地标签；

命令git tag -d <tagname>可以删除一个本地标签；

命令git push origin :refs/tags/<tagname>可以删除一个远程标签。


git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.lg "log --color --graph
--pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold
blue)<%an>%Creset' --abbrev-commit"

git config --global alias.last 'log -1'
这样，用git last就能显示最近一次的提交：

$ git last
