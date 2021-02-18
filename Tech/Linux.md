参考：Linux就该这么学  鸟哥的Linux私房菜 
[新手应该知道的26个命令](https://locez.com/Linux/common-command/)[Linux 基础](https://linuxtools-rst.readthedocs.io/zh_CN/latest/base/index.html#)

公网ip：47.93.247.69

连接方式

ssh root@47.93.247.69

Alanacc123!@#

# 错误记录

输入reboot时报错

## System has not been booted with systemd as init system (PID 1). Can't operate.

```bash
reboot -f          ##-f : 强迫重开机，不呼叫 shutdown 这个指令
```

## WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

```bash
ssh-keygen -R "you server hostname or ip"
```

# 为什么在更新了yum源之后需要yum -y update

**一般来说，各种博客的更新yum源步骤都是（这里以centos6.9 / ali yum源为例）**

**1：** `yum -y install wget`

**2：** `mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`

**3：** `cd /etc/yum.repos.d/`

**4：** `wget http://mirrors.aliyun.com/repo/Centos-6.repo`

**5：** `yum makecache`

问题来了，许多博客都有这一步

**6: `yum -y update`**

这一步更新了内核，软件以及rpm包。不明白意义何在？



其实我更推荐用<code>yum upgrade</code>取代<code>yum update</code>，<code>yum update</code>只更新系统中已有的软件包，不会更新内核软件包(<code>kernel-</code>这个包)，<code>yum upgrade</code>是更彻底的<code>update</code>，会分析包的废弃关系，可以跨小版本升级（比如从centos 7.1升级到centos 7.4），除了做了<code>yum update</code>完全相同的事之外，还会更新<code>kernel-</code>的包，也会卸载掉已经废弃的包。

新部署系统需要yum update/upgrade是因为yum不会给你解决依赖冲突(但是apt会)。

举个例子，你的系统中已经安装了<code>kernel-2.6.32.500</code>，但是你要安装的某个软件包依赖于<code>kernel-2.6.32.600</code>，此时yum会报错退出，告诉你依赖不满足，并不会升级kernel包(只是举个例子而已，实际上几乎没有软件包直接依赖于kernel包)，所以你只能yum update/upgrade一次，把系统中所有的软件包全部更新，这样满足新部署的软件包的依赖。

在debian/ubuntu的系统中，apt会对这种情况自动处理，会自动升级依赖的软件包。

换句话来说，对于新部署的服务器，也是推荐upgrade全部的软件包，已获得最新的安全补丁。即使对于已经上线的服务器，也是推荐定期打安全漏洞补丁，减少漏洞带来的侵害。

引自：https://segmentfault.com/q/1010000011264410

# 断点续传
`wget -c <downloadlink>`

# 删除文件和文件夹
删除文件 `rm <filename>`
删除文件夹 `rm -rf <dirname>`
# 压缩 查询 解压文件
压 缩：`tar -j<u>c</u>v -f filename.tar.bz2` 要被压缩的文件或目录名称
查 询：`tar -j<u>t</u>v -f filename.tar.bz2`
解压缩：`tar -j<u>x</u>v -f filename.tar.bz2 -C` 欲解压缩的目录
`tar -zxvf <filename>`

# 查看端口状态
netstat -anlp
# 杀进程
kill -9 <pid>
# 服务器启动frp
systemctl start frps # 启动frp
systemctl enable frps #开机自启frp
# 如何寻求帮助？
在 Linux 下遇到问题，最重要的是要自己寻求帮助，下面是三种寻求帮助的方法。

## man
man 是 Linux 的系统手册，即 manual 。因为大多数程序都会自带手册，所以可以通过 man 命令获取帮助。执行以后，在 man page 页面中按 q 退出。

获取 ls 的帮助

```s
$ man ls
```
查看有多少（针对不同方面的）同名的手册
```s
$ man -f ls
ls (1)               - list directory contents
ls (1p)              - list directory contents
```
查看特定的手册
```s
$ man 1p ls
```
## info
与 man 不同的是，可以像浏览网页一样在各个节点中跳转。
从文档首页开始浏览
```s
$ info
```
获取特定程序的帮助
```s
$ info program
```
## help
除了上面的两种方法外，还有一种简单使用的方法，那就是 --help 参数，一般程序都会有这个参数，会输出最简单有用的介绍。
```s
$ man --help       ### 获取 man 的帮助
$ info --help      ### 获取 info 的帮助
```
# 如何简单操作？
在 Terminal(终端) 中，有许多操作技巧，这里就介绍几个简单的。

## 光标
- up(方向键上)      可以调出输入历史执行记录，快速执行命令
- down(方向键下)    配合 up 选择历史执行记录
- Home             移动光标到本行开头
- End              移动光标到本行结尾
- PgUp             向上翻页
- PaDN             向下翻页
- ctrl + c         终止当前程序
## Tab 补全
Tab 补全是非常有用的一个功能，可以用来自动补全命令或文件名，省时准确。

未输入状态下连按两次 Tab 列出所有可用命令
已输入部分命令名或文件名，按 Tab 进行自动补全，多用你就肯定会喜欢的了。
## 常用命令
### cd
cd 是打开某个路径的命令，也就是打开某个文件夹，并跳转到该处。
```s
$ cd path      ### path 为你要打开的路径。
```
其中 path 有绝对路径和相对路径之分，绝对路径强调从 / 起，一直到所在路径。相对路径则，相对于当前路径来说，假设当前家目录有 etc 文件夹（绝对路径应为 /home/username/etc），如果直接 cd etc 则进入此文件夹，但若是 cd /etc/ 则是进入系统 etc ，多琢磨一下就可以理解了。另外在 Linux 中， . 代表当前目录， .. 代表上级目录，因此返回上级目录可以 cd .. 。

### ls

ls 即 list ，列出文件。
```s
$ ls       ### 仅列出当前目录可见文件
$ ls -l    ### 列出当前目录可见文件详细信息
$ ls -hl   ### 列出详细信息并以可读大小显示文件大小
$ ls -al   ### 列出所有文件（包括隐藏）的详细信息
```
注意： Linux 中 以 . 开头的文件或文件夹均为隐藏文件或隐藏文件夹。
### pwd
pwd 用于返回当前工作目录的名字，为绝对路径名。
```s 
$ pwd
/home
```
### mkdir
mkdir 用于新建文件夹。
```s
$ mkdir folder
$ mkdir -p folder/subfolder    ### -p 参数为当父目录存在时忽略，若不存在则建立，用此参数可建立多级文件夹
```
### rm
rm 即 remove ，删除文件。
```s
$ rm filename    ### 删除 filename
$ rm -i filename   ### 删除 filename 前提示，若多个文件则每次提示
$ rm -rf folder/subfolder/  ### 递归删除 subfolder 下所有文件及文件夹，包括 subfolder 自身
$ rm -d folder     ###  删除空文件夹
```
### cp
cp 即 copy ，复制文件。
```s
$ cp source dest            ### 将 source 复制到 dest
$ cp folder/*  dest         ### 将 folder 下所有文件(不含子文件夹中的文件)复制到 dest
$ cp -r folder  dest        ### 将 folder 下所有文件（包含子文件夹中的所有文件）复制到 dest
```
### mv
mv 即 move ，移动文件。
```s
$ mv source  folder        ### 将 source 移动到 folder 下，完成后则为  folder/source
$ mv -i source folder      ### 在移动时，若文件已存在则提示 **是否覆盖** 
$ mv source dest           ### 在 dest 不为目录的前提下，重命名 source 为 dest
```
### cat
cat 用于输出文件内容到 Terminal 。
```s
$ cat /etc/locale.gen     ### 输出 locale.gen 的内容 
$ cat -n /etc/locale.gen  ### 输出 locale.gen 的内容并显示行号
```
### more
more 与 cat 相似，都可以查看文件内容，所不同的是，当一个文档太长时， cat 只能展示最后布满屏幕的内容，前面的内容是不可见的。这时候可用 more 逐行显示内容。
```s
$ more /etc/locale.gen
$ more +100 /etc/locale.gen       ### 从 100 行开始显示
```
### less
less 与 more 相似，不过 less 支持上下滚动查看内容，而 more 只支持逐行显示。
```s
$ less /etc/locale.gen
$ less +100 /etc/locale.gen
```
### nano
nano 是一个简单实用的文本编辑器，入门简单。
```s
$ nano  filename       ### 编辑 filename 文件，若文件不存在，则新打开一个文件，若退出时保存，则创建该文件
```
编辑完后，ctrl + X 提示是否保存，按 y 确定保存即可。

注意：在使用过程中可用 ctrl + G 获取帮助。
### reboot
reboot 为重启命令。
```s
reboot         ### '$' 和 '#' 的区别在于 '$' 普通用户即可执行，而 '#' 为 root 用户才可执行，或普通用户使用 'sudo'
```
### poweroff
poweroff 为关机命令。
```s
poweroff  ### 马上关机
```
### ping
ping 主要用于测试网络连通，通过对目标机器发送数据包来测试两台主机是否连通，及延时情况。
```s
$ ping locez.com    ### 通过域名 ping，若 DNS 未设置好，可能无法 ping 通
$ ping linux.cn
PING linux.cn (211.157.2.94) 56(84) bytes of data.
64 bytes from 211.157.2.94.static.in-addr.arpa (211.157.2.94): icmp_seq=1 ttl=53 time=41.5 ms
64 bytes from 211.157.2.94.static.in-addr.arpa (211.157.2.94): icmp_seq=2 ttl=53 time=40.4 ms
64 bytes from 211.157.2.94.static.in-addr.arpa (211.157.2.94): icmp_seq=3 ttl=53 time=41.9 ms
--- linux.cn ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 40.406/41.287/41.931/0.644 ms

$ ping 211.157.2.94   ### 通过 IP 地址 ping ，若无法 ping 通可能是网络连接出现问题\
```
### grep
grep 主要用于返回匹配的项目，支持正则表达式。
```s
$ grep PATTERN filename      ### 返回所有含有 PATTERN 的行
$ grep zh_CN /etc/locale.gen ### 返回所有含 zh_CN 的行
```
### mount
mount 用于挂载一个文件系统，需要 root 用户执行。一个磁盘可分为若干个分区，在分区上面可以创建文件系统，而挂载点则是提供一个访问的入口，将一个分区的文件系统挂载到某个目录中，称这个目录为挂载点，并且可以通过这个挂载点访问该文件系统中的内容。
例如一块硬盘在 Linux 中表示为 /dev/sda 那么它上面的分区应该表示为 /dev/sda1 、/dev/sda2 。
```s
$ mount                       ### 输出系统目前的挂载信息
$ mount /dev/sda1 /mnt        ### 将 sda1 挂载到 /mnt 中
$ cd /mnt                     ### 直接通过 /mnt 访问内容
$ mount -o remount,rw  /mnt   ### 重新挂载 sda1 到 /mnt 并设置为 可读写 
$ mount -a                    ### 挂载 fstab 文件配置好的文件系统
```
### umount
umount 与 moung 相反，是卸载一个挂载点，即取消该入口。
```s
$ umount /mnt                 ### 卸载 /mnt 这个挂载点的文件系统
$ umount -a                   ### 卸载所有已挂载的文件系统
```
### tar
tar 主要用于创建归档文件，和解压归档文件，其本身是没有压缩功能的，但可以调用 gzip 、 bzip2 进行压缩处理。
参数解释：

-c 创建归档
-x 解压归档
-v 显示处理过程
-f 目标文件，其后必须紧跟 目标文件
-j 调用 bzip2 进行解压缩
-z 调用 gzip 进行解压缩
-t 列出归档中的文件
```s
$ tar -cvf filename.tar .       ### 将当前目录所有文件归档，但不压缩，注意后面有个 ’.‘ ，不可省略，代表当前目录的意思 
$ tar -xvf filename.tar         ### 解压 filename.tar 到当前文件夹
$ tar -cvjf filename.tar.bz2 .  ### 使用 bzip2 压缩
$ tar -xvjf  filename.tar.bz2   ### 解压 filename.tar.bz2 到当前文件夹
$ tar -cvzf filename.tar.gz     ### 使用 gzip  压缩
$ tar -xvzf filename.tar.gz     ### 解压 filename.tar.gz 到当前文件夹
$ tar -tf   filename            ### 只查看 filename 归档中的文件，不解压
```
### ln
ln 主要用于在两个文件中创建链接，链接又分为 Hard Links (硬链接)和 Symbolic Links (符号链接或软链接)，其中默认为创建硬链接，使用 -s 参数指定创建软链接。

硬链接主要是增加一个文件的链接数，只要该文件的链接数不为 0 ，该文件就不会被物理删除，所以删除一个具有多个硬链接数的文件，必须删除所有它的硬链接才可删除。
软链接简单来说是为文件创建了一个类似快捷方式的东西，通过该链接可以访问文件，修改文件，但不会增加该文件的链接数，删除一个软链接并不会删除源文件，即使源文件被删除，软链接也存在，当重新创建一个同名的源文件，该软链接则指向新创建的文件。
硬链接只可链接两个文件，不可链接目录，而软链接可链接目录，所以软链接是非常灵活的。
```s
$ ln source dest       ### 为 source 创建一个名为 dest 的硬链接
$ ln -s source dest    ### 为 source 创建一个名为 dest 的软链接
```
### chown
chown 用于改变一个文件的所有者及所在的组。
```s
$ chown user filename        ### 改变 filename 的所有者为 user
$ chown user:group filename  ### 改变 filename 的所有者为 user，组为 group
$ chown -R root folder      ### 改变 folder 文件夹及其子文件的所有者为 root
```
### chmod
chmod 永远更改一个文件的权限，主要有 读取 、 写入 、 执行 ，三种权限，其中 所有者 、 用户组 、 其他 各占三个，因此 ls -l 可以看到如下的信息

-rwxr--r-- 1 locez users   154 Aug 30 18:09 filename
其中 r=read ， w=write ， x=execute
```s
# chmod +x filename        ### 为 user ，group ，others 添加执行权限
# chmod -x filename        ### 取消 user ， group ，others 的执行权限
# chmod +w filename        ### 为 user 添加写入权限
# chmod ugo=rwx filename   ### 设置 user ，group ，others 具有 读取、写入、执行权限
# chmod ug=rw filename     ### 设置 user ，group 添加 读取、写入权限
# chmod ugo=--- filename   ### 取消所有权限
```
### useradd
useradd 用于添加一个普通用户。
```
# useradd -m -g users -G audio -s /usr/bin/bash newuser     
### -m 创建 home 目录， -g 所属的主组， -G 指定该用户在哪些附加组， -s 设定默认的 shell ，newuser 为新的用户名
```
### passwd
passwd 用于改变用户登录密码。
```S
$ passwd                 ### 不带参数更改当前用户密码
# passwd newuser       ### 更改上述新建的 newuser 的用户密码
```
### whereis
whereis 用于查找文件、手册等。
```
$ whereis bash 
bash: /usr/bin/bash /etc/bash.bashrc /etc/bash.bash_logout /usr/share/man/man1/bash.1.gz /usr/share/info/bash.info.gz
$ whereis -b bash       ### 仅查找 binary
bash: /usr/bin/bash /etc/bash.bashrc /etc/bash.bash_logout
$ whereis -m bash       ### 仅查找 manual
bash: /usr/share/man/man1/bash.1.gz /usr/share/info/bash.info.gz
```
### find
find 也用于查找文件，但更为强大，支持正则，并且可将查找结果传递到其他命令。
```S
$ find . -name PATTERN    ### 从当前目录查找符合 PATTERN 的文件
$ find /home -name PATTERN -exec ls -l {} \;  ### 从 /home 文件查找所有符合 PATTERN 的文件，并交由 ls 输出详细信息
```
### wget
wget 是一个下载工具，简单强大。
```
$ wget -O newname.md https://github.com/LCTT/TranslateProject/blob/master/README.md     ###下载 README 文件并重命名为 newname.md
$ wget -c url     ### 下载 url 并开启断点续传
```

`恭喜你，你已经学习了完了26 个基础的 Linux 命令。虽然这里只是一些最基础的命令，但是熟练使用这些命令就踏出了你从一位 Linux 新手成为 Linux 玩家的第一步！`










# 在shell中常用的特殊符号罗列如下：
```
#   ;   ;;      .      ,       /       \       'string'|       !   $   ${}   $?      $$   $*  "string"*     **   ?   :   ^   $#   $@    `command`{}  []   [[]]   ()    (())  ||   &&       {xx,yy,zz,...}~   ~+   ~-    &   \<...\>   +       -        %=   ==   != 
```


\# 井号 (comments)
这几乎是个满场都有的符号，除了先前已经提过的"第一行"
#!/bin/bash
井号也常出现在一行的开头，或者位于完整指令之后，这类情况表示符号后面的是注解文字，不会被执行。
\# This line is comments.
echo "a = $a" # a = 0
由于这个特性，当临时不想执行某行指令时，只需在该行开头加上 # 就行了。这常用在撰写过程中。
\# echo "a = \$a" # a = 0
如果被用在指令中，或者引号双引号括住的话，或者在倒斜线的后面，那他就变成一般符号，不具上述的特殊功能。


~ 帐户的 home 目录
算是个常见的符号，代表使用者的 home 目录：cd ~；也可以直接在符号后加上某帐户的名称：cd ~user或者当成是路径的一部份：~/bin
~+ 当前的工作目录，这个符号代表当前的工作目录，她和内建指令 pwd的作用是相同的。
\# echo ~+/var/log
~- 上次的工作目录，这个符号代表上次的工作目录。
\# echo ~-/etc/httpd/logs


; 分号 (Command separator)
在 shell 中，担任"连续指令"功能的符号就是"分号"。譬如以下的例子：cd ~/backup ; mkdir startup ;cp ~/.* startup/.


;; 连续分号 (Terminator)
专用在 case 的选项，担任 Terminator 的角色。
case "$fop" inhelp) echo "Usage: Command -help -version filename";;version) echo "version 0.1" ;;esac


. 逗号 (dot,就是“点”)
在 shell 中，使用者应该都清楚，一个 dot 代表当前目录，两个 dot 代表上层目录。
CDPATH=.:~:/home:/home/web:/var:/usr/local
在上行 CDPATH 的设定中，等号后的 dot 代表的就是当前目录的意思。
如果档案名称以 dot 开头，该档案就属特殊档案，用 ls 指令必须加上 -a 选项才会显示。除此之外，在 regularexpression 中，一个 dot 代表匹配一个字元。


'string' 单引号 (single quote)
被单引号用括住的内容，将被视为单一字串。在引号内的代表变数的$符号，没有作用，也就是说，他被视为一般符号处理，防止任何变量替换。
heyyou=homeecho '$heyyou' # We get $heyyou


"string" 双引号 (double quote)
被双引号用括住的内容，将被视为单一字串。它防止通配符扩展，但允许变量扩展。这点与单引数的处理方式不同。
heyyou=homeecho "$heyyou" # We get home

`command` 倒引号 (backticks)
在前面的单双引号，括住的是字串，但如果该字串是一列命令列，会怎样？答案是不会执行。要处理这种情况，我们得用倒单引号来做。
fdv=`date +%F`echo "Today $fdv"
在倒引号内的 date +%F 会被视为指令，执行的结果会带入 fdv 变数中。


, 逗点 (comma，标点中的逗号)
这个符号常运用在运算当中当做"区隔"用途。如下例
\#!/bin/bashlet "t1 = ((a = 5 + 3, b = 7 - 1, c = 15 / 3))"echo "t1= $t1, a = $a, b = $b"


/ 斜线 (forward slash)
在路径表示时，代表目录。
cd /etc/rc.dcd ../..cd /
通常单一的 / 代表 root 根目录的意思；在四则运算中，代表除法的符号。
let "num1 = ((a = 10 / 2, b = 25 / 5))"


\ 倒斜线
在交互模式下的escape 字元，有几个作用；放在指令前，有取消 aliases的作用；放在特殊符号前，则该特殊符号的作用消失；放在指令的最末端，表示指令连接下一行。
\# type rmrm is aliased to `rm -i'# \rm ./*.log
上例，我在 rm 指令前加上 escape 字元，作用是暂时取消别名的功能，将 rm 指令还原。
\# bkdir=/home# echo "Backup dir, \$bkdir = $bkdir"Backup dir,$bkdir = /home
上例 echo 内的 \$bkdir，escape 将 $ 变数的功能取消了，因此，会输出 $bkdir，而第二个 $bkdir则会输出变数的内容 /home。


| 管道 (pipeline)
pipeline 是 UNIX 系统，基础且重要的观念。连结上个指令的标准输出，做为下个指令的标准输入。
who | wc -l
善用这个观念，对精简 script 有相当的帮助。


! 惊叹号(negate or reverse)
通常它代表反逻辑的作用，譬如条件侦测中，用 != 来代表"不等于"
if [ "$?" != 0 ]thenecho "Executes error"exit 1fi
在规则表达式中她担任 "反逻辑" 的角色
ls a[!0-9]
上例，代表显示除了a0, a1 .... a9 这几个文件的其他文件。


: 冒号
在 bash 中，这是一个内建指令："什么事都不干"，但返回状态值 0。
:
echo \$? # 回应为 0
: > f.\$\$
上面这一行，相当于 cat /dev/null >f.\$\$。不仅写法简短了，而且执行效率也好上许多。
有时，也会出现以下这类的用法
: \${HOSTNAME?} \${USER?} \${MAIL?}
这行的作用是，检查这些环境变数是否已设置，没有设置的将会以标准错误显示错误讯息。像这种检查如果使用类似 test 或 if这类的做法，基本上也可以处理，但都比不上上例的简洁与效率。
除了上述之外，还有一个地方必须使用冒号
PATH=\$PATH:\$HOME/fbin:\$HOME/fperl:/usr/local/mozilla
在使用者自己的HOME 目录下的 .bash_profile或任何功能相似的档案中，设定关于"路径"的场合中，我们都使用冒号，来做区隔。


? 问号 (wild card)
在文件名扩展(Filename expansion)上扮演的角色是匹配一个任意的字元，但不包含 null 字元。
\# ls a?a1
善用她的特点，可以做比较精确的档名匹配。


* 星号 (wild card)

相当常用的符号。在文件名扩展(Filename expansion)上，她用来代表任何字元，包含 null 字元。
\# ls a*a a1 access_log
在运算时，它则代表 "乘法"。
let "fmult=2*3"
除了内建指令 let，还有一个关于运算的指令expr，星号在这里也担任"乘法"的角色。不过在使用上得小心，他的前面必须加上escape 字元。


** 次方运算
两个星号在运算时代表 "次方" 的意思。
let "sus=2**3"echo "sus = $sus" # sus = 8


$ 钱号(dollar sign)
变量替换(Variable Substitution)的代表符号。
vrs=123echo "vrs = $vrs" # vrs = 123
另外，在 Regular Expressions 里被定义为 "行" 的最末端 (end-of-line)。这个常用在grep、sed、awk 以及 vim(vi) 当中。


${} 变量的正规表达式
bash 对 ${} 定义了不少用法。以下是取自线上说明的表列
   ${parameter:-word}   ${parameter:=word}   ${parameter:?word}   ${parameter:+word}   ${parameter:offset}   ${parameter:offset:length}   ${!prefix*}   ${#parameter}   ${parameter#word}   ${parameter##word}   ${parameter%word}   ${parameter%%word}   ${parameter/pattern/string}   ${parameter//pattern/string}


$*
$* 引用script的执行引用变量，引用参数的算法与一般指令相同，指令本身为0，其后为1，然后依此类推。引用变量的代表方式如下：
$0, $1, $2, $3, $4, $5, $6, $7, $8, $9, ${10}, ${11}.....
个位数的，可直接使用数字，但两位数以上，则必须使用 {} 符号来括住。
$* 则是代表所有引用变量的符号。使用时，得视情况加上双引号。
echo "$*"
还有一个与 $* 具有相同作用的符号，但效用与处理方式略为不同的符号。


$@
$@ 与 $* 具有相同作用的符号，不过她们两者有一个不同点。
符号 $* 将所有的引用变量视为一个整体。但符号 $@ 则仍旧保留每个引用变量的区段观念。

$#
这也是与引用变量相关的符号，她的作用是告诉你，引用变量的总数量是多少。
echo "$#"


$? 状态值 (status variable)
一般来说，UNIX(linux) 系统的进程以执行系统调用exit()来结束的。这个回传值就是status值。回传给父进程，用来检查子进程的执行状态。
一般指令程序倘若执行成功，其回传值为 0；失败为 1。
tar cvfz dfbackup.tar.gz /home/user > /dev/nullecho"$?"$$
由于进程的ID是唯一的，所以在同一个时间，不可能有重复性的 PID。有时，script会需要产生临时文件，用来存放必要的资料。而此script亦有可能在同一时间被使用者们使用。在这种情况下，固定文件名在写法上就显的不可靠。唯有产生动态文件名，才能符合需要。符号$$或许可以符合这种需求。它代表当前shell 的 PID。
echo "$HOSTNAME, $USER, $MAIL" > ftmp.$$
使用它来作为文件名的一部份，可以避免在同一时间，产生相同文件名的覆盖现象。
ps: 基本上，系统会回收执行完毕的 PID，然后再次依需要分配使用。所以 script 即使临时文件是使用动态档名的写法，如果script 执行完毕后仍不加以清除，会产生其他问题。

(   ) 指令群组 (command group)
用括号将一串连续指令括起来，这种用法对 shell 来说，称为指令群组。如下面的例子：(cd ~ ; vcgh=`pwd` ;echo $vcgh)，指令群组有一个特性，shell会以产生 subshell来执行这组指令。因此，在其中所定义的变数，仅作用于指令群组本身。我们来看个例子
\# cat ftmp-01#!/bin/basha=fsh(a=incg ; echo -e "\n $a \n")echo $a#./ftmp-01incgfsh
除了上述的指令群组，括号也用在 array 变数的定义上；另外也应用在其他可能需要加上escape字元才能使用的场合，如运算式。


((  ))
这组符号的作用与 let 指令相似，用在算数运算上，是 bash 的内建功能。所以，在执行效率上会比使用 let指令要好许多。
#!/bin/bash(( a = 10 ))echo -e "inital value, a = $a\n"(( a++))echo "after a++, a = $a"

{  } 大括号 (Block of code)
有时候 script 当中会出现，大括号中会夹着一段或几段以"分号"做结尾的指令或变数设定。
\# cat ftmp-02#!/bin/basha=fsh{a=inbc ; echo -e "\n $a \n"}echo $a#./ftmp-02inbcinbc
这种用法与上面介绍的指令群组非常相似，但有个不同点，它在当前的 shell 执行，不会产生 subshell。
大括号也被运用在 "函数" 的功能上。广义地说，单纯只使用大括号时，作用就像是个没有指定名称的函数一般。因此，这样写 script也是相当好的一件事。尤其对输出输入的重导向上，这个做法可精简 script 的复杂度。

此外，大括号还有另一种用法，如下
{xx,yy,zz,...}
这种大括号的组合，常用在字串的组合上，来看个例子
mkdir {userA,userB,userC}-{home,bin,data}
我们得到 userA-home, userA-bin, userA-data, userB-home, userB-bin,userB-data, userC-home, userC-bin,userC-data，这几个目录。这组符号在适用性上相当广泛。能加以善用的话，回报是精简与效率。像下面的例子
chown root /usr/{ucb/{ex,edit},lib/{ex?.?*,how_ex}}
如果不是因为支援这种用法，我们得写几行重复几次呀！


[   ] 中括号
常出现在流程控制中，扮演括住判断式的作用。if [ "$?" != 0 ]thenecho "Executes error"exit1fi
这个符号在正则表达式中担任类似 "范围" 或 "集合" 的角色
rm -r 200[1234]
上例，代表删除 2001, 2002, 2003, 2004 等目录的意思。


[[     ]]
这组符号与先前的 [] 符号，基本上作用相同，但她允许在其中直接使用 || 与&& 逻辑等符号。
\#!/bin/bashread akif [[ $ak > 5 || $ak< 9 ]]thenecho $akfi


|| 逻辑符号
这个会时常看到，代表 or 逻辑的符号。


&& 逻辑符号
这个也会常看到，代表 and 逻辑的符号。


& 后台工作
单一个& 符号，且放在完整指令列的最后端，即表示将该指令列放入后台中工作。
tar cvfz data.tar.gz data > /dev/null&

\<...\> 单字边界
这组符号在规则表达式中，被定义为"边界"的意思。譬如，当我们想找寻 the 这个单字时，如果我们用
grep the FileA
你将会发现，像 there 这类的单字，也会被当成是匹配的单字。因为 the 正巧是 there的一部份。如果我们要必免这种情况，就得加上 "边界" 的符号
grep '\' FileA


+ 加号 (plus)
在运算式中，她用来表示 "加法"。
expr 1 + 2 + 3
此外在规则表达式中，用来表示"很多个"的前面字元的意思。
\# grep '10\+9' fileB109100910000910000931010009#这个符号在使用时，前面必须加上escape 字元。


- 减号 (dash)
在运算式中，她用来表示 "减法"。
expr 10 - 2
此外也是系统指令的选项符号。
ls -expr 10 - 2
在 GNU 指令中，如果单独使用 - 符号，不加任何该加的文件名称时，代表"标准输入"的意思。这是 GNU指令的共通选项。譬如下例
tar xpvf -
这里的 - 符号，既代表从标准输入读取资料。
不过，在 cd 指令中则比较特别
cd -
这代表变更工作目录到"上一次"工作目录。


% 除法 (Modulo)
在运算式中，用来表示 "除法"。
expr 10 % 2
此外，也被运用在关于变量的规则表达式当中的下列
${parameter%word}${parameter%%word}
一个 % 表示最短的 word 匹配，两个表示最长的 word 匹配。


= 等号 (Equals)
常在设定变数时看到的符号。
vara=123echo " vara = $vara"
或者像是 PATH 的设定，甚至应用在运算或判断式等此类用途上。


== 等号 (Equals)
常在条件判断式中看到，代表 "等于" 的意思。
if [ $vara == $varb ]
...下略

!= 不等于
常在条件判断式中看到，代表 "不等于" 的意思。
if [ $vara != $varb ]
...下略


^
这个符号在规则表达式中，代表行的 "开头" 位置，在[]中也与"!"(叹号)一样表示“非”


输出/输入重导向
>      >>   <   <<   :>   &>   2&>   2<>>&   >&2   

文件描述符(File Descriptor)，用一个数字（通常为0-9）来表示一个文件。
常用的文件描述符如下：
文件描述符          名称         常用缩写     默认值
     0               标准输入      stdin            键盘
     1               标准输出      stdout         屏幕
     2            标准错误输出   stderr          屏幕
我们在简单地用<或>时，相当于使用 0< 或 1>（下面会详细介绍）。
* cmd > file
把cmd命令的输出重定向到文件file中。如果file已经存在，则清空原有文件，使用bash的noclobber选项可以防止复盖原有文件。
* cmd >> file
把cmd命令的输出重定向到文件file中，如果file已经存在，则把信息加在原有文件後面。
* cmd < file
使cmd命令从file读入
* cmd << text
从命令行读取输入，直到一个与text相同的行结束。除非使用引号把输入括起来，此模式将对输入内容进行shell变量替换。如果使用<<- ，则会忽略接下来输入行首的tab，结束行也可以是一堆tab再加上一个与text相同的内容，可以参考後面的例子。
* cmd <<< word
把word（而不是文件word）和後面的换行作为输入提供给cmd。
* cmd <> file
以读写模式把文件file重定向到输入，文件file不会被破坏。仅当应用程序利用了这一特性时，它才是有意义的。
* cmd >| file
功能同>，但即便在设置了noclobber时也会复盖file文件，注意用的是|而非一些书中说的!，目前仅在csh中仍沿用>!实现这一功能。
: > filename      把文件"filename"截断为0长度.# 如果文件不存在, 那么就创建一个0长度的文件(与'touch'的效果相同).
cmd >&n 把输出送到文件描述符n
cmd m>&n 把输出 到文件符m的信息重定向到文件描述符n
cmd >&- 关闭标准输出
cmd <&n 输入来自文件描述符n
cmd m<&n m来自文件描述各个n
cmd <&- 关闭标准输入
cmd <&n- 移动输入文件描述符n而非复制它。（需要解释）
cmd >&n- 移动输出文件描述符 n而非复制它。（需要解释）
注意： >&实际上复制了文件描述符，这使得cmd > file 2>&1与cmd 2>&1 >file的效果不一样。













