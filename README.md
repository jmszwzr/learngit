# Git学习和使用

## 用户信息
### 设置
	git config --global user.name "John Doe"				设置全局用户名
	git config --global user.email johndoe@example.com		设置全局邮箱
	// 在某个项目下
	git config user.name "John Doe"					设置项目使用用户名
	git config user.email johndoe@example.com		设置项目使用邮箱
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




## 分支简介

	git branch 查看本地分支  
	git branch -a 查看所有本地分支和远程分支  
	git branch -r Markdown中如何空格

### HEAD
***HEAD*** 指向当前所在的分支  
***HEAD*** 分支随着提交操作自动向前移动
### 分支创建
	git branch dev 在当前提交的对象上创建一个分支  
 

### 分支切换
	git checkout dev 切换到dev分支上
	git checkout -b dev 创建并切换到刚创建的dev分支上  
### 分支历史
	git log --oneline --reverse --decorate 查看各个分支当前所指的对象
	git log --oneline --decorate --graph --all 查看提交历史、各个分支的指向以及项目的分支分叉情况  






































![GitHub Mark](http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png "GitHub Mark")  