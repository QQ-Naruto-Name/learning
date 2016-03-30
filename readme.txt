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
5. 在/home/zjh目前下执行git clone
git@localhost:sample.git后，发现sample目录，并且在sample下有readme.txt文件
