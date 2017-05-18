# Git Book 中文版

## 分支与合并
	gitk	以图形化的界面显示项目的提交历史
	git reset --hard HEAD	撤销一个合并，回到合并前的状态
	git reset --hard ORIG_HEAD	撤销已经合并后且已经提交的代码（另一个合并的分支不能被删除，否则，以后在合并相关的分支时会出错）

## 查看历史 —Git日志
	git log v2.5..Makefile fs/
>找出所有从`v2.5`开始在fs目录下的所有Makefile的修改

	git log -p	显示补丁(patchs)
	git log --stat	显示每个提交文件的修改情况

## 比较提交 —Git Diff
	git diff master..test	比较项目中任意两个版本的差异
	git diff master...test	找出master与test的共有父分支和test分支之间的差异
	git diff HEAD	显示工作目录与上次提交时之间的所有差别，显示的内容都会在执行'git commit -a'命令时被提交
	git diff test	查看当前的工作目录与另外一个分支的差别(也可以加上路径限定符，来只比较某一个文件或目录)
	git diff HEAD -- ./lib	显示你当前工作目录下的lib目录与上次提交之间的差别(或者更准确的 说是在当前分支)
	git diff --stat	统计哪些文件被改动，有多少行被改动
	git diff 3acf338..aad96 --stat	比较两个提交中文件的改动情况

## 分布式的工作流程
	git log -p master..origin/master	显示本地主分支和缓存中的远程分支的修改
	git push origin +master	强制git push在上传修改时先更新

### [Git签名的标签](https://github.com/jmszwzr/learngit/blob/master/GPG.md)

### [Rebase](http://gitbook.liuhui998.com/4_2.html)

## 储藏
	git stash "work in progress for foo feature"
>上面这条命令会保存你的本地修改到储藏(stash)中, 然后将你的工作目录和索引里的内容全部重置, 回到你当前所在分支的上次提交时的状态.  

	git stash apply	恢复到以前的工作状态

### 储藏队列
	git stash list	查看所有保存的stashes
	git stash apply stash@{1}	使用队列中的任意一个stashes

	git stash clear	清空这个队列

### [Git树名](http://gitbook.liuhui998.com/4_6.html)

## 追踪分支




