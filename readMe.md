# 创建版本库
- 版本库又名仓库，英文名是repository，你可以简单的理解是一个目录，这个目录里面的所有文件都可以被git管理起来，每个文件的修改删除git都可以跟踪.  
- 工作区(work directory)：就是你在电脑上看见的目录，出了.git以外的区域  
- 版本库(repository)：在工作区中有一个隐藏目录.git，这个不属于工作区，这是版	本库.  
	- 缓存区(stage)  
	- master分支以及指向master的一个指针HEAD.

>

**常用操作**:
  
		pwd//查看当前路径  
		cd path//访问路径文件  
		mkdir folder//创建路径文件
>  1.Desktop/web/git文件下创建git_test版本库 
 
		cd Desktop web git    
		mkdir git_test  	
>2.初始化版本库

		git init  
		这时候git_test版本库下多了一个.git的目录，这个目录是git		涌来跟踪管理版本的。  
>3.手动把文件添加到版本库中
  
		注:
		所有的版本控制系统，只能跟踪文本文件的改动，比如txt，网页		和所有的代码等。
>4.把文件田间到缓存区里面去
  
		git add readMe.md
>5.把文件提交到仓库当前分支上 
 
		git commit -m"commit description"  
		可以使用git status查看状态信息  
		可以使用git diff查看文件的修改  
		可以使用git log查看历史纪录  
		可以使用git reflog查看所有的历史操作  
		可以使用cat file查看file的内容  
		可以使用vi file查看文件的详细信息  
		可以使用:wq退出当前文件的查看状态  
**版本回退**  
  
		git reset --hard HEAD^//回到上一个版本  
		git reset --hard HEAD^^//回到上上一个版本  
		git reset --hard HEAD~100//回到100个版本以前  
		git reset --hard commit_id//回到commit_id的版本  
			可以使用git log查看所有的历史版本  
			可以使用git reflog查看进入撤销后的版本  
**Git撤销修改和删除文件操作**   
 
	修改了文件发现文件的修改有误需要撤销修改  
	1. 手动在文件中删除做出的修改  
	2. 使用git reset --hard HEAD^回到上一个版本  
	3. 使用git checkout --file丢弃在工作区中的修改    
		1>修改了文件还没有添加到缓存区，这就回到了版本库中的状态。  
		2>修改了文件并提交到了缓存区，回到了版本库缓存区的状态。  
			
	总结:
	只要没有commit，都可以使用git checkout --file恢复  
	commit了只能使用git reset --hard commit_id恢复到commit  
	
	总结:
	1.直接在目录下删掉文件
	2.使用命令 rm file
	3.使用commit提交删除修改或者使用git checkout --file恢复删除
		
**远程仓库**  
本地Git仓库和github仓库之间的传输是通过SSH加密的

	1.创建SSH Key  
		1>如果在用户主目录下有.ssh目录，查看有没有id_rsa和id_rsa.pub这两个文件
		2>没有的话，执行ssh-keygen -t rsa -C "email@example.com"，按照向导填写信息。
		3>id_rsa是私钥，不能泄漏出去；id_rsa.pub是公钥，可以放心的告诉别人。
		4>登陆github，在settings中SSH_Keys页面点击Add SSH Key，填上title在key文本里面粘贴id_rsa.pub文件的内容。
**本地Git仓库添加远程仓库**  
>已经在本地创建好了Git仓库，想在github上搭建一个远程Git仓库，希望这两个仓库远程同步，这样github仓库可以作为备份，又可以其他人进行仓库来协作  

	1.在github上创建一个git仓库git_test
	2.创建远程origin的git仓库  
		git remote add origin git:gitbub.com/unclepis/git_test.git
	3.把本地git仓库master分支的内容推送到远程仓库  
		git push -u origin master//第一次
		git push origin master//以后推送  
**远程库克隆**
>如果先有远程库，需要克隆到本地  
	
	1.登陆github创建一个新的仓库git_test
	2.克隆到本地  
		git clone git@github.com:unclepis/git_test.git
**创建与合并分支**

	git checkout -b dev//创建并切换到dev分支
	git branch //查看分支信息，当前分支前会有一个＊
	git checkout dev //切换到dev分支
	git merge dev //把dev分支合并到当前分支
	git branch -d dev//删除dev分支
**发生冲突**

	git status
	cat file//查看发生冲突的文件
	修改冲突的代码信息
	add并commit
	git merge dev//解决了冲突再次合并
	git branch -d dev//删除分支
**分支管理模式**
	
	Fast forward:快速模式，删除分支后会丢掉分支信息
	-no-ff：禁用ff模式

**分支管理流程**
	
	1.git checkout -b dev//创建一个dev分支。
	2.修改readme.txt内容。
	3.git add file
	git commit -m"change file"//添加到暂存区。
	4.git checkout master//切换回主分支(master)。
	5.git merge -no-ff -m"merge with no ff"dev//  
	合并dev分支  
	使用命令 git merge –no-ff  -m “注释” dev
	6.git branch -d dev//删除dev分支
	7.git log//查看历史记录
**分支管理策略**  
`master主分支比较稳定，一般是用于发布新版本；干活一半在dev分支，完成后合并到master上发布。`
**bug分支**  
临时保存没有提交到版本库的手头工作dev,修复bug分支。

	git stash//临时保存dev分支的工作
	git checkout -b bug//创建并切换到bug分支
	修改bug文件并add／commit
	git checkout master//切换到master分支
	git merge -no-ff -m"fix bug" bug//把bug分支合并到master上
	git branch -d bug//修复完bug删除bug分支
	git checkout dev //回到dev分支继续干活
	git stash list//查看stash list
	git stash apply //恢复stash内容并不删除
		git stash drop//强制删除
	git stash pop //恢复并删除stash内容
*多人协作*  
当从远程库克隆的时候，git会自动把本地的master分支和远程的master分支对应起来，并且默认的远程库是origin。

	git remote //产看远程库的信息
	git remote -v查看远程库的详细信息
	git push origin master//推送本地master到远程master
	git push origin dev//推送本地的dev到远程的dev  
*多人协作流程*  

	1.克隆git仓库到本地
		git clone git@github.com:unclepis/git_test.git
	2.进入本地库
		cd git_test
	3.创建分支开发dev
		git checkout -b dev origin/dev 
	4.修改项目文件
		git add file
		git commit -m"modify file"
	5.合并dev到本地的master
		git checkout master//切换到master
		git merge -no-ff -m"dev push" dev
	6.删除dev分支
		git branch -d dev//删除dev分支
	7.首先可以试图提交本地master
		git push origin master
	8.如果推送失败了，由于多人协作，因而远程分支比你本地的更加的新，需要使用git pull获取最新的版本。
		git pull
	9.如果pull的时候没有建立本地和远程库的关系，需要先建立联系
		git branch --set-upstream master origin/master
		//例子：建立本地master和远程master的联系
	10.再试着pull.pull成功后，如果别人没有和你修改相同的文件，可能就成功了；但是可能由于你和别人都修改了某个文件，所以引发了冲突，需要手动修复冲突。
		git status//查看冲突
		cat file／／查看发生冲突的文件并修改
	11.提交文件然后再push
		git add file
		git comit -m"fix conflict"
		git push origin master
*多人协作总结*

	因此：多人协作工作模式一般是这样的：
	1.首先，可以试图用git push origin branch-name推送自己的修改.  
	2.如果推送失败，则因为远程分支比你的本地更新早，需要先用git pull试图合并。  
	3.如果合并有冲突，则需要解决冲突，并在本地提交。再用git push origin branch-name推送。
		
	
	