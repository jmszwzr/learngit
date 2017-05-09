![GitHub Mark](http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png "GitHub Mark")  
<div align=center>
<img src="http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png" title="GitHub Mark" style="width:30%;" /><br> 
<img src="http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png" title="GitHub Mark" width="100" />
</div>

**学习资料**  
[**Git Community Book 中文版**](http://gitbook.liuhui998.com/)    

[**Public Git hosting sites**](https://git.wiki.kernel.org/index.php/GitHosting)  

----------

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
	git log master..origin/master	master分时和本地最后一次连接服务器时的远程分支进行比较，查看有哪些变化


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
>
>>**例子**  
>>git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
   --before="2008-11-01" --no-merges -- t/  
>>查看Git仓库中，2008年10月期间，Junio Hamano 提交的但未合并的测试文件

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
>
- 这个命令列出了当你在特定的分支上执行 git push 会自动地推送到哪一个远程分支。 它也同样地列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了，还有当你执行 git pull 时哪些分支会自动合并。
#### 远程仓库的移除与重命名
	git remote rename origin coding		origin/master -->  coding/master
	git remote rm origin	移除名为origin的远程仓库

### <font color="#F44D27">打标签</font>
#### 列出标签
	git tag		列出所有标签
	git tag -l 'v1.8.5*'	列出1.8.5系列的版本
#### 创建标签
>Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）。一个轻量标签很像一个不会改变的分支 - 它只是一个特定提交的引用。然而，附注标签是存储在 Git 数据库中的一个完整对象。 它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；并且可以使用 GNU Privacy Guard （GPG）签名与验证。 通常建议创建附注标签，这样你可以拥有以上所有信息；但是如果你只是想用一个临时的标签，或者因为某些原因不想要保存那些信息，轻量标签也是可用的。 

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

	git remote prune origin	删除在远程仓库中已经不存在的分支
### <font color="#F44D27">分支开发工作流</font>

[**分支开发工作流**](https://www.git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E5%BC%80%E5%8F%91%E5%B7%A5%E4%BD%9C%E6%B5%81)

### <font color="#F44D27">远程分支</font>
\-- [远程分支详解](https://www.git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF>
)

- **远程引用**是对远程仓库的引用（指针），包括分支、标签等等。 你可以通过 git ls-remote (remote) 来显式地获得远程引用的完整列表，或者通过 git remote show (remote) 获得远程分支的更多信息。 然而，一个更常见的做法是利用远程跟踪分支。
- **远程跟踪分支**是远程分支状态的引用。 它们是你不能移动的本地引用，当你做任何网络通信操作时，它们会自动移动。 远程跟踪分支像是你上次连接到远程仓库时，那些分支所处状态的书签。它们以 (remote)/(branch) 形式命名。
- 例子：假设你的网络里有一个在 git.ourcompany.com 的 Git 服务器。 如果你从这里克隆，Git 的 clone 命令会为你自动将其命名为 origin，拉取它的所有数据，创建一个指向它的 master 分支的指针，并且在本地将其命名为 origin/master。 Git 也会给你一个与 origin 的 master 分支在指向同一个地方的本地 master 分支，这样你就有工作的基础。
- “origin” 并无特殊含义  
远程仓库名字 “origin” 与分支名字 “master” 一样，在 Git 中并没有任何特别的含义一样。 同时 “master” 是当你运行 git init 时默认的起始分支名字，原因仅仅是它的广泛使用，“origin” 是当你运行 git clone 时默认的远程仓库名字。 如果你运行 git clone -o booyah，那么你默认的远程分支名字将会是 booyah/master。　　

	git fetch origin　
>这个命令查找 “origin” 是哪一个服务器（在本例中，它是 git.ourcompany.com），从中抓取本地没有的数据，并且更新本地数据库，移动 origin/master 指针指向新的、更新后的位置。   
![remote -branch](https://www.git-scm.com/book/en/v2/images/remote-branches-3.png)

#### 推送
	git push origin dev		推送本地的 dev 分支来更新远程仓库上的 dev 分支。
	git push origin dev:dev		同上
	git push origin dev:server-dev		同上，只是将远程分支命名为 server-dev
#### 抓取
	git fetch origin	从服务器上抓取数据
	git merge origin/master	合并到当前所在的分支
	git checkout -b master origin/master	这会创建一个用于工作的本地分支，并且起点位于 origin/master

#### 跟踪分支
>
- 从一个远程跟踪分支检出一个本地分支会自动创建一个叫做 “跟踪分支”（有时候也叫做 “上游分支”）。
- 跟踪分支是与远程分支有直接关系的本地分支。 如果在一个跟踪分支上输入 git pull，Git 能自动地识别去哪个服务器上抓取、合并到哪个分支。

	git checkout -b dev origin/dev
	git checkout --track origin/dev	作用同上
	git checkout -b sf origin/dev	将本地分支与远程分支设置为不同的名字，本地分支 sf 会自动从 origin/dev拉取

	git branch -u origin/serverfix
- 设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支，你可以在任意时间使用 -u(--set-upstream 的简写) 或 --set-upstream-to 选项运行 git branch 来显式地设置。

>
上游快捷方式  
当设置好跟踪分支后，可以通过 @{upstream} 或 @{u} 快捷方式来引用它。 所以在 master 分支时并且它正在跟踪 origin/master 时，如果愿意的话可以使用 git merge @{u} 来取代 git merge origin/master。

	git branch -vv	查看本地设置的所有跟踪分支。这会将所有的本地分支列出来并且包含更多的信息，如每一个分支正在跟踪哪个远程分支与本地分支是否是领先、落后或是都有。
	git branch -av	查看包括远程在内的所有分支情况(数据中的数值来自于从每个服务器上最后一次抓取的数据，这些数据存在于本地缓存的服务器上)
	git fetch --all or git fetch or git fetch origin
	git branch -vv	统计最新的领先与落后数字

#### 抓取
>
- 当 git fetch 命令从服务器上抓取本地没有的数据时，它并不会修改工作目录中的内容。 它只会获取数据然后让你自己合并。然而，有一个命令叫作 git pull 在大多数情况下它的含义是一个 git fetch 紧接着一个 git merge 命令。 如果有一个像之前章节中演示的设置好的跟踪分支，不管它是显式地设置还是通过 clone 或 checkout 命令为你创建的，git pull 都会查找当前
- 分支所跟踪的服务器与分支，从服务器上抓取数据然后尝试合并入那个远程分支。
- 由于 git pull 的魔法经常令人困惑所以通常单独显式地使用 fetch 与 merge 命令会更好一些。

#### 删除远程分支
	git push origin --delete dev	从服务器上删除 dev 分支
	git push origin :dev	同上，只是推送一个空的分支到远程 dev 分支上，相当于删除

### <font color="#F44D27">[变基](https://www.git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA)</font>
- 在 Git 中整合来自不同分支的修改主要有两种方法：merge 以及 rebase。
- 变基使得提交历史更加整洁。你在查看一个经过变基的分支的历史记录时会发现，尽管实际的开发工作是并行的，但它们看上去就像是串行的一样，提交历史是一条直线没有分叉。
- 一般我们这样做的目的是为了确保在向远程分支推送时能保持提交历史的整洁
- 请注意，无论是通过变基，还是通过三方合并，整合的最终结果所指向的快照始终是一样的，只不过提交历史不同罢了。
- 变基是将一系列提交按照原有次序依次应用到另一分支上，而合并是把最终结果合在一起。

#### 变基的基本操作	
	git checkout dev
	git rebase master	以 master 所在的位置作为基底，将 dev 分支上的所有修改都移至 master 分支上
	git checkout master		回到 master 分支，进行一次快进合并
	git merge dev
>
- 它的原理是首先找到这两个分支的最近共同祖先，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底, 最后以此将之前另存为临时文件的修改依序应用。
- 请注意，无论是通过变基，还是通过三方合并，整合的最终结果所指向的快照始终是一样的，只不过提交历史不同罢了。 变基是将一系列提交按照原有次序依次应用到另一分支上，而合并是把最终结果合在一起。

#### 更有趣的变基例子
	git rebase --onto master server client
>
以上命令的意思是：“取出 client 分支，找出处于 client 分支和 server 分支的共同祖先之后的修改，然后把它们在 master 分支上重放一遍”。 这理解起来有一点复杂，不过效果非常酷。  

	git checkout master
	git merge client
>截取特性分支上的另一个特性分支，然后变基到其他分支,现在可以快进合并 master 分支了。快进合并 master 分支，使之包含来自 client 分支的修改  

	git rebase master server
	git checkout master
	git merge server
>
接下来你决定将 server 分支中的修改也整合进来。 使用 git rebase [basebranch] [topicbranch] 命令可以直接将特性分支（即本例中的 server）变基到目标分支（即 master）上。这样做能省去你先切换到 server 分支，再对其执行变基命令的多个步骤。

#### 变基的风险
>**遵守一条准则**
>>- 不要对在你的仓库外有副本的分支执行变基。

>变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交。

	git pull --rebase	此条命令同下面两条
	
	git fetch origin
	git rebase origin/master
>
- 如果你习惯使用 git pull ，同时又希望默认使用选项 --rebase，你可以执行这条语句 git config --global pull.rebase true 来更改 pull.rebase 的默认配置。  
- 只要你把变基命令当作是在推送前清理提交使之整洁的工具，并且只在从未推送至共用仓库的提交上执行变基命令，就不会有事。 假如在那些已经被推送至共用仓库的提交上执行变基命令，并因此丢弃了一些别人的开发所基于的提交，那你就有大麻烦了，你的同事也会因此鄙视你。  
- 如果你或你的同事在某些情形下决意要这么做，请一定要通知每个人执行 git pull --rebase 命令，这样尽管不能避免伤痛，但能有所缓解。

#### 变基 vs 合并
- 总的原则是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作

## [服务器上的Git](https://www.git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E5%8D%8F%E8%AE%AE)

### [配置服务器](https://www.git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1%E5%99%A8)


## 分布式Git

### <font color="#F44D27">向一个项目贡献</font>

#### 提交准则
	git diff --check	提交前，运行它，它将会找到可能的空白错误并将它们为你理列出来

#### 私有小型团队
![一个简单的多人 Git 工作流程的通常事件顺序](https://www.git-scm.com/book/en/v2/images/small-team-flow.png)  

#### 私有管理团队
![管理团队工作流程的基本顺序](https://www.git-scm.com/book/en/v2/images/managed-team-flow.png)


### <font color="#F44D27">维护项目</font>

#### 在特性分支中工作
	git branch featureA master	基于 master 分支建立特性分支
	git checkout -b featureA master	基于 master 分支建立特性分支，并立刻切换到新分支上
#### 检出远程分支
	git remote add jessica git://github.com/jessica/myproject.git
	git fetch jessica
	git checkout -b rubyclient jessica/ruby-client

	git pull https://github.com/onetimeguy/project 执行一个一次性的抓取，而不会将该 URL 存为远程引用

#### 确定引入了哪些东西
	git log contrib --not master
	git log -p contrib --not master	在每次提交后面附加对应的差异（diff）
>假设贡献者向你发送了两个补丁，为此你创建了一个名叫 contrib 的分支并在其上应用补丁,对该分支中所有 master 分支尚未包含的提交进行检查。通过在分支名称前加入 --not 选项，你可以排除 master 分支中的提交。

	git merge-base contrib master	手工找出contrib与master的公共祖先
	36c7dba2c95e6bbb78dfa822519ecfec6e1ca649
	git diff 36c7db

	git diff master...contrib	作用同上
>
- 三点语法。 对于 diff 命令来说，你可以通过把 ... 置于另一个分支名后来对该分支的最新提交与两个分支的共同祖先进行比较
- 该命令仅会显示自当前特性分支与 master 分支的共同祖先起，该分支中的工作。 这个语法很有用，应该牢记。

#### 变基与拣选工作流
	git cherry-pick e43a6fd3e94888d76779ad79fb568ed180e5fcdf
>只希望分支上的 e43a6 拉取到 master 分支，而不希望拉取 e43a6 之后的提交

#### Rerere
>Rerere 是“重用已记录的冲突解决方案（reuse recorded resolution）”的意思——它是一种简化冲突解决的方法。 当启用 rerere 时，Git 将会维护一些成功合并之前和之后的镜像，当 Git 发现之前已经修复过类似的冲突时，便会使用之前的修复方案，而不需要你的干预。  

	git config --global rerere.enabled true 现在每当你进行一次需要解决冲突的合并时，解决方案都会被记录在缓存中，以备之后使用

#### 生成一个构建号
	git describe master	由最近的标签名、自该标签之后的提交数目和你所描述的提交的部分 SHA-1 值构成
>注意 git describe 命令只适用于有注解的标签（即使用 -a 或 -s 选项创建的标签），所以如果你在使用 git describe 命令的话，为了确保能为标签生成合适的名称，打发布标签时都应该采用加注解的方式。 你也可以使用这个字符串来调用 checkout 或 show 命令，但是这依赖于其末尾的简短 SHA-1 值，因此不一定一直有效。 比如，最近 Linux 内核为了保证 SHA-1 值对象的唯一性，将其位数由 8 位扩展到了 10 位，导致以前的 git describe 输出全部失效。

#### 准备一次发布
	git archive master --prefix='project/' | gzip > `git describe master`.tar.gz

	git archive master --prefix='project/' --format=zip > `git describe master`.zip	创建一个 zip 压缩包

#### 查看提交简报
	git shortlog --no-merges master --not v1.0	给出上次发布v1.0以来所有提交的总结，并且已经按照作者分好组
	git shortlog --no-merges master --not a1beba6	给出自 a1beba6 提交以来的总结

## GitHub