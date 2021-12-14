---
title: Linux
date: 2021-11-30 22:36:20
categories: 
- 测试开发
tags:
- linux
---

### Linux Command

#### 查看帮助常用命令：
*[help, whatis, info, which, whereis, man]*

1. help :该命令是bash内建命令，用于显示bash内建命令的帮助信息。
	- 格式语法
	```sh 
	help(选项)(参数)
	```
	-  常用参数：

| 参数  | 含义                                      |
| :---: | :---------------------------------------: |
| -d    | 显示内建命令的简要描述。                  |
| m     | 按照man手册的格式输出内建命令的帮助信息。 |
| s     | 仅输出内建命令的命令格式。                |

- 示例:
    - 显示cd命令的帮助信息：
```sh
    [root@text~]# help cd

    cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.
    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.

    The variable CDPATH defines the search path for the directory containing
    DIR.  Alternative directory names in CDPATH are separated by a colon (:).
    A null directory name is the same as the current directory.  If DIR begins
    with a slash (/), then CDPATH is not used.

    If the directory is not found, and the shell option `cdable_vars' is set,
    the word is assumed to be  a variable name.  If that variable has a value,
    its value is used for DIR.

    Options:
        -L	force symbolic links to be followed: resolve symbolic links in
        DIR after processing instances of `..'
        -P	use the physical directory structure without following symbolic
        links: resolve symbolic links in DIR before processing instances
        of `..'
        -e	if the -P option is supplied, and the current working directory
        cannot be determined successfully, exit with a non-zero status
        -@  on systems that support it, present a file with extended attributes
            as a directory containing the file attributes

    The default is to follow symbolic links, as if `-L' were specified.
    `..' is processed by removing the immediately previous pathname component
    back to a slash or the beginning of DIR.

    Exit Status:
    Returns 0 if the directory is changed, and if $PWD is set successfully when
    -P is used; non-zero otherwise.
```
    - 以短格式显示cd命令的帮助信息：
```sh
[root@text~]# help -s cd

cd: cd [-L|[-P [-e]] [-@]] [dir]
```
    - 输出cd命令的简短描述：
```sh
[root@text~]# help -d cd

cd - Change the shell working directory.
```
	
	

2. whatis:查看命令的简要说明
3. info:查看命令的详细说明
4. which:查看命令的位置 
5. whereis:定位指令的二进制程序、源代码文件和 man 手册页等相关文件的路径
6. man:查看命令的帮助手册（包含说明、用法等信息）

#### 文件管理常用命令：
*[cd, ls, pwd, mkdir, rmdir, tree, touch, ln, rename, stat, file, chmod, chown, locate, find, cp, mv, rm]*
1. cd:切换目录
- 语法格式：
```sh
cd [参数] [目录名]
```
- 常用参数：
| 参数 | 含义                                                                 |
| ---- | -------------------------------------------------------------------- |
| -P   | 如果切换的目标目录是一个符号链接，则直接切换到符号链接指向的目标目录 |
| -L   | 如果切换的目标目录是一个符号链接，则直接切换到符号链接名所在的目录   |
| --   | 仅使用”-“选项时，当前目录将被切换到环境变量”OLDPWD”对应值的目录  |
| ~    | 切换至当前用户目录                                                   |
| ..   | 切换至当前目录位置的上一级目录                                       |
	- 示例：
		- 将当前工作目录切换到dir目录，并使用pwd命令查看当前目录：
		```sh
		[root@text~]# cd dir
		[root@text~]# cd pwd
	
		/home/text/桌面/linuxTest/dir
		```

	- 使用“cd ~ ”和“cd .. ”命令进行目录的切换操作，并使用pwd命令查看当前目录：
		```sh
		[root@text~]# cd ~
		[root@text~]# cd pwd
	
		/home/text

	  #cd ..表示退回到上一级目录
		[root@text~]# cd ..   
		[root@text~]# cd pwd
	
		/home/text/桌面/linuxTest

	  #退回多级目录，用 / 分隔开
	  #cd ../..  :表示退回到上两级目录
		[root@text~]# cd ../..   
		[root@text~]# cd pwd
	
		/home/text/桌面
		```

2. ls: 显示指定工作目录下的内容及属性信息
	- 语法格式：
	```sh
	ls [选项] [文件]
	```
	- 常用参数：

| 参数 | 含义                                                 |
| :--- | ---------------------------------------------------: |
| -a   | 显示所有文件及目录 (包括以“.”开头的隐藏文件)       |
| -l   | 使用长格式列出文件及目录信息                         |
| -r   | 将文件以相反次序显示(默认依英文字母次序)             |
| -t   | 根据最后的修改时间排序                               |
| -A   | 同 -a ，但不列出 “.” (当前目录) 及 “..” (父目录) |
| -S   | 根据文件大小排序                                     |
| -R   | 递归列出所有子目录                                   |
	- 示例:
		- 默认展示当前目录的内容
		```sh
		[root@text~]# ls
		
		_config.landscape.yml  _config.yml  package-lock.json  package.json  scaffolds/  source/  themes/
		```
		- 列出所有文件(包括隐藏文件)：
		```sh
		[root@text~]# ls -a
		
		./  ../  .git/  .github/  .gitignore  .idea/  _config.landscape.yml  _config.yml  package-lock.json  package.json  scaffolds/  source/  themes/
		```
		- 列出文件的详细信息
		```sh
		[root@text~]# ls -l  #可以简写为 ll
		
		$ ls -l
		total 84
		-rw-r--r-- 1 text 1049089     0 Dec  3 11:13 _config.landscape.yml
		-rw-r--r-- 1 text 1049089  3018 Dec  3 11:13 _config.yml
		-rw-r--r-- 1 text 1049089 75525 Dec  3 11:13 package-lock.json
		-rw-r--r-- 1 text 1049089   728 Dec  3 11:13 package.json
		drwxr-xr-x 1 text 1049089     0 Dec  3 11:13 scaffolds/
		drwxr-xr-x 1 text 1049089     0 Dec  3 11:13 source/
		drwxr-xr-x 1 text 1049089     0 Dec  3 11:13 themes/
		```
		 列出根目录(/)下的所有目录
		```sh
		[root@text~]# ls /
		
		$ ls /
		LICENSE.txt  ReleaseNotes.html  bin/  cmd/  dev/  etc/  git-bash.exe*  git-cmd.exe*  mingw64/  proc/  tmp/  unins000.dat  unins000.exe*  unins000.msg  usr/
		```
		列出当前工作目录下所有文件及目录并以文件的大小进行排序(从大到小):
		```sh
		[root@text~]# ls -lAS
		
		total 89
		-rw-r--r-- 1 hanhongli 1049089 75525 Dec  3 11:13 package-lock.json
		-rw-r--r-- 1 hanhongli 1049089  3018 Dec  3 11:13 _config.yml
		-rw-r--r-- 1 hanhongli 1049089   728 Dec  3 11:13 package.json
		-rw-r--r-- 1 hanhongli 1049089    71 Dec  3 11:13 .gitignore
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  6 10:15 .git/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 .github/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 .idea/
		-rw-r--r-- 1 hanhongli 1049089     0 Dec  3 11:13 _config.landscape.yml
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 scaffolds/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 source/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 themes/
		```
		列出当前工作目录下所有文件及目录并以文件的大小进行排序(从小到大，借助 参数 -r 进行反转):
		```sh
		[root@text~]# ls -lASr
		
		total 89
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 themes/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 source/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 scaffolds/
		-rw-r--r-- 1 hanhongli 1049089     0 Dec  3 11:13 _config.landscape.yml
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 .idea/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  3 11:13 .github/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  6 10:15 .git/
		-rw-r--r-- 1 hanhongli 1049089    71 Dec  3 11:13 .gitignore
		-rw-r--r-- 1 hanhongli 1049089   728 Dec  3 11:13 package.json
		-rw-r--r-- 1 hanhongli 1049089  3018 Dec  3 11:13 _config.yml
		-rw-r--r-- 1 hanhongli 1049089 75525 Dec  3 11:13 package-lock.json
		```
	
3. pwd: 显示当前工作目录的绝对路径
	- 语法格式：
		```sh
		pwd
		```
	- 示例:
		```sh
		[root@text~]# pwd
		
		/e/WorkSpace/honlicc.github.io
		```
4. mkdir: 创建目录
	- 语法格式：
		```sh
		 mkdir [参数] [目录]
		```
	- 常用参数：
	
|参数| 含义 | 
| ------ | 
| -p | 递归创建多级目录 | 
| -m | 建立目录的同时设置目录的权限 |
| -z| 设置安全上下文|
| -v | 显示目录的创建过程 |
	- 示例:
		在工作目录下，建立一个名为 dir 的子目录
		```sh
		[root@text~]# mkdir dir
		
		[root@text~]# ll
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  7 14:54 dir/
		```
		在工作目录下，建立一个名为 dir 的子目录
		```sh
		[root@text~]# mkdir dir
		
		[root@text~]# ll
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  7 14:54 dir/
		```
		在目录/usr/text下建立子目录dir，并且设置文件属主有读、写和执行权限，其他人无权访问:
		```sh
		[root@text~]# mkdir -m 700 /usr/text/dir
		```
		同时创建子目录dir1，dir2，dir3：
		```sh
		[root@text~]# mkdir dir1 dir2 dir3
		
		[root@text~]# ll
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  7 14:54 dir/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  7 14:57 dir1/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  7 14:57 dir2/
		drwxr-xr-x 1 hanhongli 1049089     0 Dec  7 14:57 dir3/
		```
		递归创建目录：
		```sh
		[root@text~]# mkdir -p dir4/test
		```
5. rmdir: 删除空目录
	- 语法格式：
	```sh
	 rmdir [参数] [目录]
	```
	- 常用参数：

| 参数                           | 含义                    |
| ------------------------------ | ----------------------- |
| -p                             | 所有应用                |
| -- -- ignore-fail-on-non-empty | 显示应用关联的 apk 文件 |
| -v                             | 只显示 disabled 的应用  |

	- 示例:
		删除空目录
		```sh
		[root@text~]# rmdir dir
		```
		递归删除指定的目录树：
		```sh
		[root@text~]# rmdir -p dir/dir_1/dir_2
		```
	    显示指令详细执行过程：
		```sh
		[root@text~]# rmdir -v dir
		rmdir: 正在删除目录 'dir'
		```
6. tree:以树状图列出目录内容
7. touch:
8. ln:
9. rename:
10. stat:
11. file:
12. chmod:
13. chown:
14. locate:
15. find:
16. cp:
17. mv:
18. rm:

#### 文件内容常用命令：
[cat, head, tail, more, less, sed, vi, grep]
1. cat:
2. head:
3. tail:
4. more:
5. less:
6. sed:
7. vi:
8. grep:

#### 文件压缩和解压：
[tar, gzip, zip, unzip]
1. tar:
2. gzip:
3. zip:
4. unzip:

#### 用户相关常用命令：
[groupadd, groupdel, groupmod, useradd, userdel, usermod, passwd, su, sudo]
1. groupadd:
2. groupdel:
3. groupmod:
4. useradd:
5. userdel:
6. usermod:
7. passwd:
8. su:
9. sudo:

#### 系统管理常用命令：
[eboot, exit, shutdown, date, mount, umount, ps, kill, systemctl, service, crontab]
1. eboot:
2. exit:
3. shutdown:
4. date:
5. mount:
6. umount:
7. ps:
8. kill:
9. systemctl:
10. service:
11. crontab:


#### 网络管理常用命令:
[curl, wget, telnet, ip, hostname, ifconfig, route, ssh, ssh-keygen, firewalld, iptables, host, nslookup, nc/netcat, ping, traceroute, netstat]
1. curl:
2. wget:
3. telnet:
4. ip:
5. hostname:
6. ifconfig:
7. route:
8. ssh:
9. ssh-keygen:
10. firewalld:
11. iptables:
12. host:
13. nslookup:
14. nc/netcat:
15. ping:
16. traceroute:
17. netstat:


#### 硬件管理常用命令:
[df, du, top, free, iotop]
1. df:
2. du:
3. top:
4. free:
5. iotop

#### 软件管理常用命令:
[rpm, yum, apt-get]
1. rpm:
2. yum:
3. apt-get:




