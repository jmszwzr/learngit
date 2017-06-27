# 此文件为~目录下.gitconfig全局文件

> 罗列出自己个人的一些操作习惯

[color]  
	ui = true
[alias]  
	ss = status
	cc = checkout
	mm = commit
	bb = branch
	unstage = reset HEAD --
	last = log -1
	lg = log --color --graph --pretty=format:'%Cred%h%Creset - %C(yellow)%d% %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
	la = log --pretty=oneline
	cb = checkout -b
	ld = "log --color --graph --pretty=format:'%Cred%h%Creset - %C(yellow)%d% %s %Cgreen(%cr)%C(#00FFFF)(%ad) %C(bold blue)<%an>%Creset' --abbrev-commit --date=iso"
	s = status -s
	lh = log --oneline --graph
	ll = log --pretty=oneline --reverse --decorate --abbrev-commit
	lr = "log --color --reverse --pretty=format:'%Cred%h%Creset - %C(yellow)%d% %s %Cgreen(%cr)%C(#00FFFF)(%ad) %C(bold blue)<%an>%Creset' --abbrev-commit --date=iso"