###git:
1. git init 把这个目录变成Git可以管理的仓库
2. git add xxx.txt 把文件添加到仓库 把文件修改添加到暂存区
3. git commit -m "xxx"把文件提交到仓库 把暂存区的所有内容提交到当前分支
每修改一次txt文件 都要重新进行add 和commit -m 命令
4. git status 查看仓库当前的状态 文件名为红色说明文件被修改过 但还没有提交
5. git diff xxx.txt 可以查看上次修改了什么
6. git log 可以查看历史记录
git log --pretty=oneline 可以查看每条历史记录的相应的commit id
7.  在Git中，用HEAD表示当前版本 上一个版本就是HEAD^ 上上一个版本就是HEAD^^ 也可以使用HEAD~（数字） git reset --hard HEAD^就是退回到上一个版本
也可以用git reset --hard 38d63（要返回的版本的commit id前五位）
在windows的cmd上操作 git reset --hard "HEAD^" (HEAD^要加双引号) 
8. git reflog可以查看命令历史 能回到未来的版本
Git自动创建了唯一一个master分支，master比较稳定，一般作为主支，git commit就是往master分支上提交更改

###暂存区
![暂存区](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)
git add命令就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支
就变成👇
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849077337835a877df2d26742b88dd7f56a6ace3ecf000/0)

1. git checkout -- xxx.txt 可以把文件在工作区的修改全部撤销
2. git reset HEAD xxx.txt可以把暂存区的修改撤销
删除 git rm xxx.txt  
git commit -m "remove xxx.txt"

###添加到远程仓库 
在GitHub上右上角找到并点击New repository 创建一个新的仓库 仓库名称
 git remote add origin git@github.com:(GitHub账号)/仓库名称.git
 git push -u origin master

 从远程仓库克隆git clone git@github.com:(GitHub账号)/仓库名称.git

要添加另一个本地仓库对应一个同一个远程仓库时要用 Git rebase origin/master 拉到同一线上再进行git push origin master


 ###分支：
 1. git checkout -b 分支名称(如dev) 创建并切换到分支
 2. git branch dev 创建分支
 3. git checkout dev 切换分支
 4. git branch 查看当前分支
 5. git merge dev 把dev分支的结果合并到master分支上
 6. git branch -d dev 删除分支
 7. git branch -D dev 强势删除 用于包含机密资料必须销毁的分支
 8. git merge --no-ff -m "merge with no-ff" dev 把原来的master和dev合并成一个新的master
 ![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909222841acf964ec9e6a4629a35a7a30588281bb000/0)
  
 9. git stash 可以把当前的工作现场隐藏起来 以后恢复后再工作
 10. git stash list 查看刚才的工作现场去哪了
 11. git stash apply 恢复工作现场 然后用git stash drop来删除stash的内容 也可以直接使用git stash pop 恢复的时候也删除了stash    上述指令能用在bug修复 （在工作时遇到修复bug的问题）
 当你多次stash时 恢复先用git stash list查看 然后恢复制定的stash用命令 git stash apply stash@{0} 

###多人开发
 1. git remote 查看远程仓库的信息
 2. git remote -v 查看更详细的信息
 3. git push origin master 把该分支上的所有本地推送到远程仓库
 4. 如果要推送其它分支例如(dev) git push origin dev 
 5. 当多人要在dev上开发时，就要建立远程origin的dev分支到本地 用命令创建本地dev：
 6. git checkout -b dev origin/dev 再用git push origin dev推送分支，多人推送的时候会产生冲突
解决冲突 先用git pull 把最新的提交从origin/dev抓下来 本地合并再推送
7. 如果git pull失败的话 用git branch --set-upstream-to=origin/dev dev 设置origin/dev和dev的链接 在用git pull 在本地解决冲突后在git push origin dev 推送


###标签
1. git tag 标签名  新建一个标签，默认为HEAD 
2. git tag 标签名 commit id  可以指定一个commit id给它加标签
3. git tag  -a 标签名 -m "标签信息..."  可以指定标签信息
4. git tag 可以查看所有标签
5. git tag -d 标签名 可以删除标签
6. git push origin 标签名 可以推送一个标签
7. git push origin --tags 可以推送所有未推送的标签名
8. git tag -d 标签名 可以删除一个本地标签
9. git push origin :refs/tags/标签名 可以删除一个远程标签 在删除前先要删除本地标签

10. git config --global alias.配置命令 原命令 


###Gitflow
gitflow是一个git的操作标准流程 包含如下几个关键分支：

1. master是主分支，产品功能全部实现后，在master分支对外发布，能从其他分支合并，不能在此分支修改

2. develop 主开发分支，基于master分支克隆，包含所有要发布到下一个release的代码，该分支为只读唯一分支，能从其他分支合并，feature功能分支完成，合并到develop（不推送）

3. feature功能开发分支，基于develop分支克隆，主要用于新需求新功能的开发，功能完毕后合并到develop分支（未正式上线之前不推送到远程中央仓库），feature分支可同时存在多个，用于多个功能同时开发，属于临时分支，功能完成后可选删除

4. release 测试分支，基于feature分支合并到develop之后，从develop分支克隆，主要用于功能测试，测试过程中发现的bug在本分支进行修复，修复完成上线后合并到develop/master分支并推送，打标签tag 临时分支，功能上线后可选删除

5. hotfix 补丁分支，基于master的分支克隆，主要用于对线上版本进行bug修复，修复完毕后合并到develop/master分支并推送，打tag，临时分支可选删除，所有hotfix分支的修改会进入到下一个release

主要工作流程:初始化项目为gitflow 默认创建master分支,从master拉取第一个分支develop分支，在develop拉取多个feature同时进行开发，开发完成后合并到develop,可以选择性删除feature,但不能更改,必要时从release分支进行修改，从develop拉取到release分支进行提测,在release分支上修复bug,release分支上线后合并release到develop/master并推送,合并后 可选择性删除当前release分支,不删除也不能修改,有问题也必须从master拉取hotfix分支进行修改，上线之后发现bug,也从master拉取hotfix进行修改,再合并hotfix到develop/master并推送,之后hotfix不能修改,若还有问题,还是从master拉取新的hotfix继续修复。

当在进行一个feature/develop时，若develop分支有变动，要将变动合并到自己的分支上 即 develop合并到feature分支上.


