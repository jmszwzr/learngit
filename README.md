# Git学习和使用

## 用户信息
### 设置
	git config --global user.name "John Doe"	设置全局用户名
	git config --global user.email johndoe@example.com	设置全局邮箱
	// 在某个项目下
	git config user.name "John Doe"	设置项目使用用户名
	git config user.email johndoe@example.com	设置项目使用邮箱
### 查看
	git config --list	查看所有配置
	cat ~/.gitconfig	查看部分关键配置
	git config user.name	查看用户名
	git config user.email	查看邮箱
## 获取帮助
	git help <verb>
	git <verb> --help

	git help config		获取config命令的手册

## Git基础
### <font color="#F44D27">获取Git仓库</font>
	git init	在现有目录中初始化仓库
	git init newrepo	对已存在的目录进行仓库初始化
	
	git add *.c		添加所有以.c结尾的文件
	git add LICENSE
	git commit -m 'initial project version'

	git clone [url]		克隆现有仓库
	git clone [url] [rename]	自定义本地仓库的名字
### <font color="#F44D27">记录每次更新到仓库</font>
#### 检查当前文件状态
	git status		状态详览
	git status -s	状态简览
	git status -short 同上
>`git status -s` 新添加的未跟踪文件前面有 ?? 标记，新添加到暂存区中的文件前面有 A 标记，修改过的文件前面有 M 标记。 你可能注意到了 M 有两个可以出现的位置，出现在右边的 M 表示该文件被修改了但是还没放入暂存区，出现在靠左边的 M 表示该文件被修改了并放入了暂存区。
#### 跟踪新文件
	git add README.md 添加内容到下一次提交中
>`git add` 可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。
#### 忽略文件
[GitHub忽略文件规则示例](https://github.com/github/gitignore)

**文件`.girignore`的格式规范如下：**

- 所有空行或者以 ＃ 开头的行都会被 Git 忽略。  
- 可以使用标准的 glob 模式匹配。  
- 匹配模式可以以（/）开头防止递归。  
- 匹配模式可以以（/）结尾指定目录。  
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。  
>所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。 使用两个星号（*) 表示匹配任意中间目录，比如`a/**/z` 可以匹配 a/z, a/b/z 或 `a/b/c/z`等。

我们再看一个 .gitignore 文件的例子：

	# no .a files
	*.a

	# but do track lib.a, even though you're ignoring .a files above
	!lib.a
	
	# only ignore the TODO file in the current directory, not subdir/TODO
	/TODO
	
	# ignore all files in the build/ directory
	build/
	
	# ignore doc/notes.txt, but not doc/server/arch.txt
	doc/*.txt
	
	# ignore all .pdf files in the doc/ directory
	doc/**/*.pdf
#### 查看已暂存和未暂存的修改
	git diff 	比较工作目录中当前文件和暂存区域快照之间的差异，也就是修改之后还没有暂存起来的变化
	git diff --cached	查看已暂存的将要添加到下次提交里的内容
	git diff --staged	同上，Git 1.6.1 及更高版本允许使用
>Git Diff 的插件版本  
在本书中，我们使用 git diff 来分析文件差异。 但是，如果你喜欢通过图形化的方式或其它格式输出方式的话，可以使用 git difftool 命令来用 Araxis ，emerge 或 vimdiff 等软件输出 diff 分析结果。 使用 git difftool --tool-help 命令来看你的系统支持哪些 Git Diff 插件。
#### 跳过使用暂存区域
	git commit -a -m 'description'	自动把所有已经跟踪过的文件暂存起来一起提交
	git commit -am 'description'	同上
#### 移除文件
	rm README.md	简单的从工作目录中删除文件
	git rm README.md	记录此次移除文件的操作，下一次提交时，该文件就不再纳入版本管理了、
	git rm -f README.md		如果删除之前修改过并且已经放到暂存区域的话，则必须强制删除选项`-f`(译注：即force的首字母)
	git rm --cached README.md	从Git仓库中删除(亦即从暂存区移除)，但人希望保留在当前工作目录中。（即文件仍存在磁盘上，但是Git不继续跟踪了）

>**使用glob模式删除文件**  
>
- git rm log/\*.log	删除`log/`目录下扩展名为`.log`的所有文件  
- git rm \*~	删除以`~`结尾的所有文件

#### 移动文件
	git mv file_from file_to	Git中对文件改名，相当于下面三条命令

	mv file_from file_to
	git rm file_from
	git add file_to
>更具体的`git log --pretty=format`使用，请查看[Git-log.md](https://github.com/jmszwzr/learngit/blob/master/git-log.md)

### <font color="#F44D27">查看提交历史</font>

	git log -p	用来显示每次提交的内容差异
	git log -p -2	用来显示最近两次提交差异
	git log --stat	显示每次提交的简略的统计信息


>**git log 的常用选项**
>
	-p 按补丁格式显示每个更新之间的差异。
	--stat 显示每次更新的文件修改统计信息。
	--shortstat 只显示 --stat 中最后的行数修改添加移除统计。
	--name-only 仅在提交信息后显示已修改的文件清单。	
	--name-status 显示新增、修改、删除的文件清单。	
	--abbrev-commit 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。	
	--relative-date 使用较短的相对时间显示（比如，“2 weeks ago”）。	
	--graph 显示 ASCII 图形表示的分支合并历史。	
	--pretty 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。
#### 限制输出长度
	git log --since=2.weeks		列出所有最近两周内的提交
	git log -Sfunction_name		找出添加或移除某一个特定函数的引用的提交
>**限制 git log 输出的选项**  
>
- -(n) 仅显示最近的 n 条提交  
- --since, --after 仅显示指定时间之后的提交。  
- --until, --before 仅显示指定时间之前的提交。  
- --author 仅显示指定作者相关的提交。  
- --committer 仅显示指定提交者相关的提交。  
- --grep 仅显示含指定关键字的提交  
- -S 仅显示添加或移除了某个关键字的提交  

>>**例子**  
>git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
   --before="2008-11-01" --no-merges -- t/  
>查看Git仓库中，2008年10月期间，Junio Hamano 提交的但未合并的测试文件

### <font color="#F44D27">撤销操作</font>
	git add <file>	
	git commit --amend	添加文件，尝试重新提交，并且调用自身的编辑器修改修改提交信息

	git reset HEAD <file>	取消暂存文件
	git checkout -- <file>	撤销文件所做的修改
>**Important**  
>你需要知道 git checkout -- [file] 是一个危险的命令，这很重要。 你对那个文件做的任何修改都会消失 - 你只是拷贝了另一个文件来覆盖它。 除非你确实清楚不想要那个文件了，否则不要使用这个命令。

### <font color="#F44D27">远程仓库的使用</font>
#### 查看远程仓库
	git remote	列出指定的每一个远程服务器的简写
	git remote -v	显示需要读写远程仓库使用的Git保存的简写与其对应的URL
#### 添加远程仓库
	git remote add <shortname> <url>
#### 从远程仓库中抓取与拉取
	git fetch [remote-name]		从远程仓库中获得数据
>这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。
	git fetch origin	会抓取克隆(或上一次抓取)后新推送的所有工作，必须手动合并
	git pull	通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支
#### 推送到远程仓库
	git push origin master		将master分支推送到origin服务器
	git push origin master:master	同上
>只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。 当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。 阅读 Git 分支 了解如何推送到远程仓库服务器的详细信息。
#### 查看远程仓库
	git remote show origin	
>- 它同样会列出远程仓库的 URL 与跟踪分支的信息。 这些信息非常有用，它告诉你正处于 master 分支，并且如果运行 git pull，就会抓取所有的远程引用，然后将远程 master 分支合并到本地 master 分支。 它也会列出拉取到的所有远程引用。  
- 这个命令列出了当你在特定的分支上执行 git push 会自动地推送到哪一个远程分支。 它也同样地列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了，还有当你执行 git pull 时哪些分支会自动合并。
#### 远程仓库的移除与重命名
	git remote rename origin coding		origin/master -->  coding/master
	git remote rm origin	移除名为origin的远程仓库

### <font color="#F44D27">打标签</font>
#### 列出标签
	git tag		列出所有标签
	git tag -l 'v1.8.5*'	列出1.8.5系列的版本
#### 创建标签
>Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）。  
>
一个轻量标签很像一个不会改变的分支 - 它只是一个特定提交的引用。

>然而，附注标签是存储在 Git 数据库中的一个完整对象。 它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；并且可以使用 GNU Privacy Guard （GPG）签名与验证。 通常建议创建附注标签，这样你可以拥有以上所有信息；但是如果你只是想用一个临时的标签，或者因为某些原因不想要保存那些信息，轻量标签也是可用的。 

#### 附注标签
	git tag -a v1.0 -m 'version 1.0'
>-m 选项指定了一条将会存储在标签中的信息。 如果没有为附注标签指定一条信息，Git 会运行编辑器要求你输入信息。
	git show v1.0	查看标签信息与对应的提交信息
#### 附注标签
	git tag v1.0	给当前最后一次的提交打上一个标签
>轻量标签本质上是将提交校验和存储到一个文件中 - 没有保存任何其他信息。 创建轻量标签，不需要使用 -a、-s 或 -m 选项，只需要提供标签名字
#### 后期打标签
	git log --pretty=oneline	列出提交历史
	git tag -a v1.2 9fceb02		给指定的commit打上标签
#### 共享标签
>默认情况下，git push 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。 这个过程就像共享远程分支一样 - 你可以运行 git push origin [tagname]。

	git push origin v1.0
	git push origin --tags	一次性推送所有不存在远程仓库中的本地标签
#### 检出标签
>在 Git 中你并不能真的检出一个标签，因为它们并不能像分支一样来回移动。 如果你想要工作目录与仓库中特定的标签版本完全一样，可以使用 git checkout -b [branchname] [tagname] 在特定的标签上创建一个新分支

	git checkout -b version1.0 v1.0
	
### <font color="#F44D27">Git别名</font>

**常用别名参考**  

	git config --global alias.ss status
>**[alias]**
>
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
	ll = log --oneline
	lr = "log --color --reverse --pretty=format:'%Cred%h%Creset - %C(yellow)%d% %s %Cgreen(%cr)%C(#00FFFF)(%ad) %C(bold blue)<%an>%Creset' --abbrev-commit --date=iso"



## Git分支
### <font color="#F44D27">分支简介</font>
>
- Git 保存的不是文件的变化或者差异，而是一系列不同时刻的文件快照
- 在进行提交操作时，Git 会保存一个提交对象（commit object）。该提交对象会包含一个指向暂存内容快照的指针。该提交对象还包含了作者的姓名和邮箱、提交时输入的信息以及指向它的父对象的指针
- 首次提交产生的提交对象没有父对象，普通提交操作产生的提交对象有一个父对象，而由多个分支合并产生的提交对象有多个父对象

	git branch 查看本地分支  
	git branch -a 查看所有本地分支和远程分支  
	git branch -r Markdown中如何空格

### HEAD
- ***HEAD*** 指向当前所在的分支  
- ***HEAD*** 分支随着提交操作自动向前移动
### 分支创建
	git branch dev 在当前提交的对象上创建一个分支(HEAD仍然指向master)  

### 分支切换
	git checkout dev 切换到dev分支上(HEAD指向了dev分支)
	git checkout -b dev 创建并切换到刚创建的dev分支上  
### 分支历史
	git log --oneline --reverse --decorate 查看各个分支当前所指的对象
	git log --oneline --decorate --graph --all 查看提交历史、各个分支的指向以及项目的分支分叉情况  

### <font color="#F44D27">分支的创建与合并</font>
>由于这部分非常重要，请穿越阅读  

[**分支的创建与合并**](https://www.git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)

- `git mergetool`	使用图形化工具来解决冲突

### <font color="#F44D27">分支管理</font>
	git branch -v	查看本地每一个分支的最后一次提交
	git branch -av	查看包括远程在内的每一个分支的最后一次提交
	git branch --merged		查看哪些分支已经合并到当前分支
	git branch --no-merged	查看所有包含未合并工作的分支
	git branch -d dev	删除已经合并到当前HEAD上的dev分支
	git branch -D dev	强制删除dev分支并丢掉那些工作

### <font color="#F44D27">分支开发工作流</font>

[**分支开发工作流**](https://www.git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E5%BC%80%E5%8F%91%E5%B7%A5%E4%BD%9C%E6%B5%81)

### <font color="#F44D27">远程分支</font>






![GitHub Mark](http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png "GitHub Mark")  