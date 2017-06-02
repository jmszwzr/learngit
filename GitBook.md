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
	git branch --track experimental origin/experimental	手动创建一个'追踪分支'
	git pull experimental	它会自动从‘origin'抓取(fetch)内容，再把远程的'origin/experimental'分支合并进(merge)本地的'experimental'分支

## 使用Git Grep进行搜索
	git grep xmmap
>查看仓库里每个使用'xmmap'函数的地方

	git grep -n xmmap
>显示行号

	git grep --name-only xmmap
>只显示文件名

	git grep -c xmmap
>查看每个文件里有多少行匹配内容

	git grep xmmap v1.5.0
>查找git仓库里某个特定版本里的内容, 我们可以像下面一样在命令行末尾加上标签名(tag reference)

	git grep -e '#define' --and -e SORT_DIRENT
>查找我们在仓库的哪个地方定义了'SORT_DIRENT'

	git grep --all-match -e '#define' -e SORT_DIRENT
>不但可以进行“与"(both)条件搜索操作，也可以进行"或"(either)条件搜索操作

	git grep -e '#define' --and \( -e PATH -e MAX \) 
>查找出符合一个条件(term)且符合两个条件(terms)之一的文件行.　例如我们要找出名字中含有‘PATH'或是'MAX'的常量定义

	git log [-p] | grep "Pro-git"
> 在提交说明中查找含有"Pro-git"的提交说明

## Git的撤销操作 - 重置(reset)，签出(checkout)和撤销(revert)

### 修复未提交文件中的错误(重置)
	git reset --hard HEAD
>让工作目录回到上次提交时的状态  
>这条命令会把你工作目录中所有未提交的内容清空(当然这不包括未置于版控制下的文件 untracked files). 从另一种角度来说, 这会让"git diff" 和"git diff --cached"命令的显示法都变为空

	git checkout -- hello.rb
	git checkout -- .
>前一句，恢复一个文件，如"hello.rb";后一句，恢复所有文件的修改

### 修复已提交文件中的错误

#### 创建新提交来修复错误
	
	git revert HEAD
>创建一个新的，撤消(revert)了前期修改的提交(commit)是很容易的; 只要把出错的提交(commit)的名字(reference)做为参数传给命令: git revert就可以了; 上面这条命令就演示了如何撤消最近的一个提交。  
>这样就创建了一个撤消了上次提交(HEAD)的新提交, 你就有机会来修改新提交(new commit)里的提交注释信息。

	git revert HEAD^
>- 撤消更早期的修改, 上面这条命令就是撤消“上上次”(next-to-last)的提交。  
>- 在这种情况下，<font color="#F44D27">**git尝试去撤消老的提交,然后留下完整的老提交前的版本**</font>。如果你最近的修改和要撤消的修改有重叠(overlap),那么就会被要求手工解决冲突(conflicts),　就像解决合并(merge)时出现的冲突一样。  
>- <font color="#F44D27">**git revert 其实不会直接创建一个提交(commit), 把撤消后的文件内容放到索引(index)里,你需要再执行git commit命令，它们才会成为真正的提交(commit)。**</font>  

	git revert --continue
>在撤销操作中，可能会存在冲突(conflicts)，解决掉冲突后，使用上面命令继续进行修复

	git revert --abort
>取消revert

#### 修改提交来修复错误
- 如果你刚刚做了某个提交(commit), 但是你又想马上修改这个提交; git commit 现在支持一个叫**--amend**的参数，它能让你修改刚才的这个提交(HEAD commit). 这项机制能让你在代码发布前,**添加一些新的文件** 或是 **修改你的提交注释**(commit message).  
>目前在Window7中，已commit但未push的只能只用`git commit --amend -m <message>`来修改最后一次提交的提示信息，而做不到像上文中说到的可以**添加一些新的文件**。

- 如果你在老提交(older commit)里发现一个错误, 但是现在还没有发布到代码服务器上. 你可以使用 git rebase命令的交互模式, "git rebase -i"会提示你在编辑中做相关的修改. 这样其实就是让你在rebase的过程来修改提交.

## 维护Git

### 保持良好的性能
	git gc
> 
- 在大的仓库中, git靠压缩历史信息来节约磁盘和内存空间。
- 压缩操作并不是自动进行的, 你需要手动执行 git gc。
- 压缩操作比较耗时, 你运行git gc命令最好是在你没有其它工作的时候

	git fsck
>git fsck 运行一些仓库的一致性检查, 如果有任何问题就会报告. 这项操作也有点耗时, 通常报的警告就是“悬空对象"(dangling objects)。“悬空对象"(dangling objects)并不是问题, 最坏的情况只是它们多占了一些磁盘空间. 有时候它们是找回丢失的工作的最后一丝希望。

## 创建新的空分支
>在偶尔的情况下，你可能会想要保留那些与你的代码没有共同祖先的分支。例如在这些分支上保留生成的文档或者其他一些东西。如果你需要创建一个不使用当前代码库作为父提交的分支，你可以用如下的方法创建一个空分支。

	git symbolic-ref HEAD refs/heads/newbranch 
	rm .git/index 
	git clean -fdx 
	<do work> 
	git add your files 
	git commit -m 'Initial commit'

### [高级分支与合并](http://gitbook.liuhui998.com/5_3.html)

### [查找问题的利器 - Git Bisect](http://gitbook.liuhui998.com/5_4.html)

## 查找问题的利器 - Git Blame
	git blame [filename]
>查看文件的每一行的详细修改信息:包括SHA串,日期和作者。

	git blame -L 160,+10 README.md
>用"-L"参数在命令(blame)中指定开始和结束行。

### [Git和Email](http://gitbook.liuhui998.com/5_6.html)

### [Git Hooks](http://gitbook.liuhui998.com/5_8.html)

### [找回丢失的对象-可练习](http://gitbook.liuhui998.com/6_1.html)

## 查看Git对象






