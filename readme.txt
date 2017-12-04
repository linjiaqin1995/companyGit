

初始化git仓库
git init 

上传文件
git add filename
git commit -m "描述"

查看仓库当前状态
git status
查看具体修改了什么
git diff filename

查看提交日志
git log
git log --pretty=oneline  每条输出为一行

回退上个版本
git reset --hard head^
head表示当前版本，上一个版本是head^，上上个版本就是head^^,往上100个版本就写head~100
注意：改完后用git log 看不到之前的版本了
使用reflog命令查看每一次的命令
git reflog

工作区即为文件夹，add命令把文件夹添加到暂存区（stage）, commit 把暂存区的所有文件提交到当前分支
所以可以看出三个区--工作区，暂存区，版本分支。
每次修改，不add到暂存区，就不会加入到commit中。

丢弃工作区的修改
git checkout -- filename
丢弃以发送到暂存区的修改
git reset head filename
git checkout -- filename
丢弃以发送到版本库（未发送到远程库）的修改
git reset --hard 版本号(操作同上回退版本)

删除工作区的文件后，用git status 发现工作区与版本库不一致，
rm filename
	1 确实要删除，
	git rm filename
	git commit -m "..."
	2 删错了，那就把版本库中的文件替换工作区的版本，即丢弃对工作区的操作（修改或者删除）
	git checkout -- filename

创建ssh key 
1 ssh-keygen -t rsa -C "yourEmail@example.com",一路回车.
2 登陆github，打开setting，ssh key 页面,add ssh key，填上title，在key文本框中粘贴id_rsa.pub文件的内容（在用户主目录下）,add key

添加远程库
假设本地有个git 仓库，想要在github上建立一个新的仓库并让两个仓库进行远程同步。
1 登陆github，create a new repository
2 填入repository name,点击创建
3 git remote add origin git@github.com:linjiaqin1995/filename.git
4 git push -u origin master 
注意： 第一次推送master分支，要加上-u 参数，关联两端的master分支。
之后的每次，本地坐了修改，就可以通过命令
git push origin master

从远程库克隆一个库到本地
git clone git@github.com:linjiaqin1995/learngit.git 

git 中的分支操作
	1 查看分支
	git branch   //当前分支前面带有*号
	2 创建分支
	git branch name
	3 切换分支
	git checkout name
	4 创建并切换
	git checkout -b name
	5 合并某分支到当前分支
	git merge name 
	6 删除分支
	git branch -d name
git无法自动合并分支时，先解决冲突，再提交。
查看分支合并图
git log --graph           加上--abbrev-commit 版本号不会太长

不带参数的merge，git会使用fast-forward模式，删除分支后，会丢掉分支信息
强制禁用fast-forword,git 会在merge时生成一个新的commit，这样，从分支历史上就可以看到分支信息
git merge --no-ff -m "..." branchName

手头上工作没完成，先把工作现场git stash 一下，然后去做别的，回来再git stash pop, 回到工作现场	
查看原来的工作现场 git stash list.

开发一个新feature,最好新建一个分支。
要丢弃一个没有被合并过的分支，git branch -D name

查看远程库的信息 
git remote   加上-v 参数显示更详细内容

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支。
git push origin 分支名
一般来说，master,dev需要推送，与远程同步，其他分支视情况而定。

假设多人协作，你的同伴在dev分支上做了推送。而你也要对该文件修改推送。
	先指定本地dev分支与远程origin/dev分支的链接
	git branch --set-upstream dev origin/dev
	再把最新的提交从origin/dev上抓取下来。
	git pull
	pull下来后还有冲突，手动解决。方法如上。解决后，提交，再push
	
通常的多人协作的模式
	1 git push origin branchName推送自己的修改。
	2 若推送失败，则因为远程分支比你本地更新，先用git pull 试图合并。
	3 若合并有冲突，解决冲突，在本地提交。
	4 没有冲突或冲突已解决，再用git push origin branchName.
	注意，若git pull 提示no tracking information,说明本地分支与远程分支的链接关系没有创建
	git branch --set-upstream branchName origin/branchName
	
给commit打标签名，默认打在最新的commit上，加上commit-id可指定
git tag tagName [commitId]
查看所有标签
git tag
创建带有说明的标签，-a指定标签名，-m指定说明文字
git tag -a v1.0 -m "version 1.0 released" 33213
查看某个标签具体
git show tagName
删除打错的标签名
git tag -d tagName   若是已经推送到远程，git push origin :refs/tags/tagName
推送某个标签到远程，
git push origin tagName
一次性推送所有尚未推送到远程的本地标签
git push origin --tags
	
配置别名，如把git status 改为输入git st 即可，
git config --global alias.st status	
 
































