# Git Book 中文版

## 分支与合并
	gitk	以图形化的界面显示项目的提交历史
	git reset --hard HEAD	撤销一个合并，回到合并前的状态
	git reset --hard ORIG_HEAD	撤销已经合并后且已经提交的代码（另一个合并的分支不能被删除，否则，以后在合并相关的分支时会出错）

## 查看历史 —Git日志
	git log v2.5..Makefile fs/
>找出所有从`v2.5`开始在fs目录下的所有Makefile的修改

	git log -p	显示补丁(patchs)