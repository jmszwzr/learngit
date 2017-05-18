# GPG签名

## 带GPG签名的Git tag

### 生成GPG Key
	gpg --gen-key 生成GPG key,然后根据提示选择你要的签名
	
	gpg (GnuPG) 1.4.11; Copyright (C) 2010 Free Software Foundation, Inc.
	This is free software: you are free to change and redistribute it.
	There is NO WARRANTY, to the extent permitted by law.
	
	请选择您要使用的密钥种类：
	   (1) RSA and RSA (default)
	   (2) DSA and Elgamal
	   (3) DSA (仅用于签名)
	   (4) RSA (仅用于签名)
	您的选择？  >选择加密种类
	
	RSA 密钥长度应在 1024 位与 4096 位之间。
	您想要用多大的密钥尺寸？(2048)
	
	请设定这把密钥的有效期限。
	         0 = 密钥永不过期
	      <n>  = 密钥在 n 天后过期
	      <n>w = 密钥在 n 周后过期
	      <n>m = 密钥在 n 月后过期
	      <n>y = 密钥在 n 年后过期
	密钥的有效期限是？(0) >选择密钥有效期****

**对以上信息进行确认，然后输入身份，最终确认，等待生成GPG Key。**

	您需要一个用户标识来辨识您的密钥；本软件会用真实姓名、注释和电子邮件地址组合
	成用户标识，如下所示：
	    “Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>”
	
	真实姓名： Keven Liu
	电子邮件地址： airk908@gmail.com
	注释： GPG key for Keven
	您选定了这个用户标识：
	    “Keven Liu (GPG key for Keven) <airk908@gmail.com>”
	
	更改姓名(N)、注释(C)、电子邮件地址(E)或确定(O)/退出(Q)？ O
	您需要一个密码来保护您的私钥。
	
	我们需要生成大量的随机字节。这个时候您可以多做些琐事(像是敲打键盘、移动
	鼠标、读写硬盘之类的)，这会让随机数字发生器有更好的机会获得足够的熵数。

**生成过程中可能出现类似提示：** 

	随机字节不够多。请再做一些其他的琐事，以使操作系统能搜集到更多的熵！
	(还需要174字节)

**疯狂的敲打键盘吧，不过看好还需要多少，悠着点。
稍等片刻，GPG Key就能生成好了，验证一下是否生成成功：**  

	gpg --list-keys

**如果输出类似信息就代表属于你的GPG Key生成成功了：**

	/home/keven/.gnupg/pubring.gpg
	------------------------------
	pub   2048R/AF26C87F 2013-09-30
	uid                  Keven Liu (Gpg key for Keven Liu<airk908@gmail.com>) <airk908@gmail.com>
	sub   2048R/C9A00F19 2013-09-30

**上边显示的是公钥，顺便也看一下与之匹配的私钥生成如何：**

	gpg --list-secret-keys

**不过，好像很多人会出现错误，比如：**

	gpg: WARNING: using insecure memory! 
	gpg: please see http://www.gnupg.org/faq.html for more information 
	gpg: skipped `Keven Liu <airk908@gmail.com>': secret key not available 
	gpg: signing failed: secret key not available 
	error: gpg failed to sign the tag 
	fatal: unable to sign the tag

## **出现以上错误有两种解决方式** 

+ **方式一**  
```
	git tag -u "Keven Liu" -s "My tag message" v1.0　需要输入密码  
```
>- Keven Liu即为在生成GPG时的Real Name  
>- git tag -v v1.0    查看一下项目的tag验证信息

+ **方式二**  
>**gpg --list-keys**　　显示公钥  
>**gpg --list-secret-keys**　　显示私钥  
	gpg --list-keys  
	~/.gnupg/pubring.gpg  
	\--------------------------------  
	pub   2048R/35F5FFB2 2016-04-23
	uid                  name (New key) <name@mail.com>
	sub   2048R/112A8C2D 2016-04-23  
**git config --global user.signingkey 35F5FFB2**  
**git tag -s v1.1 -m 'signed version 1.1 releases'**　　给tag打上GPG签名(需要输入生成GPG Key时的密码)  
**git commit -sm "XXX"**　　即可为提交也打上签名


**参考文章-链接**

<http://airk000.github.io/git/2013/09/30/git-tag-with-gpg-key>  
<http://stackoverflow.com/questions/36810467/git-commit-signing-failed-secret-key-not-available>  
<http://blog.csdn.net/chenjh213/article/details/47978199>  