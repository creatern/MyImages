# 安装

## 分区

**挂载点**

- 指定该分区对应Linux文件系统的哪个目录。
- Linux允许将不同的物理磁盘上的分区映射到不同的目录，
   - 可以实现将不同的服务框架放在不同的物理磁盘上，当一个物理磁盘损坏时，不会影响其他物理磁盘上的数据。

**SWAP 虚拟内存交换区**

- 原理：用硬盘模拟的虚拟内存，当系统内存使用率比较高时，内核会自动使用SWAP分区来存取数据。
- 生产环境中：
   - 物理内存 < 4G 建议设置为物理内存的2倍
   - 物理内存  4~16G 建议设置为与物理内存相等
   - 物理内存 > 16G 建议设置为物理内存的一半

**/ 根分区**

**/boot 引导分区**

### 文件系统类型

**XFS**

- 高度可扩高性能文件系统
- 支持的最大的文件系统为：500TB，最大文件：16TB
- 支持元数据日志，可以加快崩溃时的恢复速度
- 当挂载使用时，仍可以进行清理碎片和控制文件系统。

**ext**

**LVM 缓存**

- 允许用户创建逻辑卷(LV)，以小型快速设备作为更大、速度更慢的设备的缓存

## KDUMP 

- KDUMP开启后，将会使用一部分内存空间，当系统崩溃时KDUMP会捕获系统的关键信息，以便分析查找出系统崩溃的原因。
- 此功能主要是系统相关的程序员使用，对普通用户而言意义不大，建议关闭。

## NETWORK 网络设置

- 默认关闭，需要打开

## 登录

### 运行级别

| 参数 | 说明                                                |
| :--- | :-------------------------------------------------- |
| 0    | 停机                                                |
| 1    | 单用户模式                                           |
| 2    | 多用户模式                                           |
| 3    | 完全多用户模式，服务器一般运行载此级别                 |
| 4    | 一般不用，在一些特殊情况使用                          |
| 5    | X11模式。一般发行版默认的运行级别，可以启动图形桌面系统 |
| 6    | 重新启动                                            |

**修改运行级别**

```shell
init 运行级别参数
```

**重启**

```shell
reboot
```

**关机**

```shell
poweroff
```

### 远程登录

**查看网络**

```shell
ifconfig
```

```
[root@bogon ~]# ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.186.144  netmask 255.255.255.0  broadcast 192.168.186.255
# inet 192.168.186.144  这个是Xshell等软件需要的主机地址
        inet6 fe80::17f1:ef1c:aad4:3bff  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:ab:56:b2  txqueuelen 1000  (Ethernet)
        RX packets 49  bytes 9077 (8.8 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 68  bytes 8388 (8.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1  (Local Loopback)
        RX packets 12  bytes 1404 (1.3 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 12  bytes 1404 (1.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:19:78:d6  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

**sshd服务**

- 检查sshd服务是否开启

```shell
ps -ef | grep sshd
```

```
[root@bogon ~]# ps -ef | grep sshd
root       1433      1  0 13:30 ?        00:00:00 /usr/sbin/sshd -D
root       2952   2828  0 13:36 pts/0    00:00:00 grep --color=auto sshd
```

-检查sshd服务开启的端口

```shell
netstat -plnt | grep sshd
```

```
[root@bogon ~]# netstat -plnt | grep sshd
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1433/sshd           
tcp6       0      0 :::22                   :::*                    LISTEN  
```

**软件**

- Xshell 终端

![](c:/notebook/pictures/149153913227439.png =292x)

![](c:/notebook/pictures/375583913240274.png =292x)

- Xftp 文件传输

![](c:/notebook/pictures/293774313236829.png =298x)

# 图形化界面

## X Window系统

- 以位图方式显示的软件窗口系统
- 用来创建图形化界面，在X Window基础上开发出GNOME、KDE、CDE

![](c:/notebook/pictures/308405213232583.png =407x)

**进入X Window环境**

```shell
startx
```

## KDE桌面环境和GNOME桌面环境**

- 默认安装GNOME桌面环境

- 两者都采用GPL公约发行，
   - 不同之处在于KDE基于双重授权的Qt，
   - 而GNOME采用遵循GPL的GTK库开发，拥有更广泛的支持。
- KDE包含大量的应用软件、项目规模庞大，自带软件众多，操作习惯接近Windows。
  - KDE不足之处在于其运行速度相对较慢，且部分程序容易崩溃
- GNOME项目由于专注于桌面环境本身，软件较少、运行速度快，并具有出色的稳定性，GNOM受到了大多数公司的青睐，成为多个企业发行版的默认桌面。

### GNOME

#### 输入法设置

- setting -> Region&Language

![](c:/notebook/pictures/247450614250463.png =295x)
![](c:/notebook/pictures/389750714248067.png =293x)

**切换输入法**

![](c:/notebook/pictures/416270814245569.png =141x)

#### 配置网卡和有线

![](c:/notebook/pictures/463371114226810.png =294x)
![](c:/notebook/pictures/23051214249250.png =298x)

#### 使用U盘、光盘和移动硬盘

- 在Linux中，U盘、光盘和移动硬盘等可移动的存储介质都会以文件系统的方式挂载到本地目录上进行访问。
- 图形化界面自动识别
- 命令行界面 会提示，但不会自动挂载，需要mount命令

# 命令行界面

**Linux系统结构**

![](c:/notebook/pictures/440202214237932.png =459x)

**切换至命令行**

- 图形化：终端
- `Ctrl + Windows + Alt + F3` 切换到终端3 （命令行界面)
- `Ctrl + Windows + Alt + F1` 切换到终端1 （图形化界面)


# Linux的目录结构

| 目录         | 说明                                                                                                                                                                                                               |
| :---------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /           | 根目录。文件的最顶端，/ect、/bin、/dev、/lib、/sbin应该和根目录放置在一个分区中，而类似/usr/local可以单独位于另一个分区                                                                                                   |
| /bin        | 存放系统所需要的重要命令，比如文件或目录操作的命令ls、cp、mkdir等。另外/usr/bin也存放了一些系统命令，这些命令对应的文件都是可执行的，普通用户可以使用大部分的命令                                                              |
| /boot       | 这是存放Linux启动时内核及引导系统程序所需要的核心文件，内核文件和gub系统引导管理器（或称引导装载程序）都位于此目录                                                                                                          |
| /dev        | 存放Linux系统下的设备文件，如光驱、磁盘等。访问该目录下某个文件相当于访问某个硬件设备，常用的是挂载光驱                                                                                                                    |
| /etc        | 一般存放系统的配置文件，作为一些软件启动时默认配置文件读取的目录，如/etc/fstab存放系统分区信息                                                                                                                            |
| /home       | 系统默认的用户主目录。如果添加用户时不指定用户的主目录，默认在/home下创建与用户名同名的文件夹。代码中可以用HOME环境变量表示当前用户的主目录                                                                                   |
| /lib        | 64位系统有/lib64文件夹，主要存放动态链接库。类似的目录有/usr/1ib、/usr/local/lib等                                                                                                                                     |
| /lost+found | 存放一些当系统意外崩溃或机器意外关机时产生的文件碎片                                                                                                                                                                   |
| /mnt        | 用于存放挂载储存设备的挂载目录，如光驱等                                                                                                                                                                              |
| /proc       | 存放操作系统运行时的信息，如进程信息、内核信息、网络信息等。此目录的内容存在于内存中，实际不占用磁盘空间，如/etc/cpuinfo存放CPU的相关信息                                                                                    |
| /root       | Linux超级权限用户root的主目录                                                                                                                                                                                       |
| /sbin       | 存放一些系统管理的命令，一般只能由超级权限用户root执行。大多数命令普通用户一般无权限执行，类似/sbin/ifconfig，普通用户使用绝对路径也可执行，用于查看当前系统的网络配置。类似的目录有usr/sbin、usr/local/sbin                     |
| /tmp        | 临时文件目录，任何人都可以访问。系统软件或用户运行程序（如MySQL)时产生的临时文件存放到这里。此目录数据需要定期清除。重要数据不可放置在此目录下，此目录空间不易过小                                                              |
| /usr        | 应用程序存放目录，如命令、帮助文件等。安装Linux软件包时默认安装到/usr/local目录下。比如/usr/share/fonts存放系统字体，/usr/share/man存放帮助文档，/usr/include存放软件的头文件等。/usr/local目录建议单独分区并设置较大的磁盘空间 |
| /var        | 这个目录的内容是经常变动的，如/var/log用于存放系统日志、/var/lib用于存放系统库文件等                                                                                                                                     |
| /sys        | 目录与/proc类似，是一个虚拟的文件系统，主要记录与系统核心相关的信息，如系统当前已经载入的模块信息等。这个目录实际不占硬盘容量                                                                                                |



# Shell

- 默认通常是bash(Bourne Again Shell)
- 命令本身是一个函数

**功能**

- 提供用户与操作系统进行交互操作的接口，方便用户使用系统中的软硬件资源。
- 提供脚本语言编程环境，方便用户完成简单到复杂的任务调度。

**过程**

1. Linux启动时，最先进入内存的是内核，并常驻内存，
2. 然后进行系统引导，引导过程中启动所有进程的父进程在后台运行，直到相关的系统资源初始化完毕后，等待用户登录。
4. 用户登录时，通过登录进程验证用户的合法性。
5. 用户验证通过后根据用户的设置启动相关的Shell，以便接收用户输入的命令并返回执行结果。

![](c:/notebook/pictures/320973214224100.png =198x)

## 常用快捷键和通配符

| 快捷键        | 说明                                                                                                      |
| :----------- | :-------------------------------------------------------------------------------------------------------- |
| history      | 显示命令历史列表                                                                                           |
| ↓            | 显示上一条命令                                                                                             |
| ↑            | 显示下一条命令                                                                                             |
| !num         | 执行命令历史列表的第num条命令                                                                               |
| !!           | 执行上一条命令                                                                                             |
| Ctrl + r     | 按键后输入若干字符，会向上搜索包含该字符的命令，继续按此键搜索上一条匹配的命令 ;相当于`history | grep 特点字符串` |
| ls !$        | 执行命令ls,并以上一条命令的参数为其参数                                                                      |
| Ctrl + a     | 移动到当前行的开头                                                                                          |
| Ctrl + e     | 移动到当前行的结尾                                                                                          |
| Esc + b      | 移动到当前单词的开头                                                                                        |
| Esc + f      | 移动到当前单词的结尾                                                                                        |
| Ctrl + l     | 清除屏幕内容                                                                                               |
| Ctrl +u      | 删除命令行中光标所在处之前的所有字符，不包括自身                                                              |
| Ctrl + k     | 删除命令行中光标所在处之后的所有字符，包括自身                                                                |
| Ctrl + d     | 删除光标所在处字符                                                                                          |
| Ctrl + h     | 删除光标所在处前一个字符                                                                                    |
| Ctrl + y     | 粘贴刚才所删除的字符                                                                                        |
| Ctrl + w     | 删除光标所在处之前的字符至其单词头，以空格、标点等为分隔符                                                     |
| Ctrl + t     | 颠倒光标所在处及其之前的字符位置，并将光标移动到下一个字符                                                     |
| Esc + t      | 颠倒光标所在处及其相邻单词的位置                                                                             |
| Ctrl + x + u | 撤销刚才的操作                                                                                             |
| Ctrl + s     | 挂起当前Shell，不接收任何输入                                                                               |
| Ctrl + q     | 重新启用挂起的Shell，接收用户输入                                                                           |



**自动补齐 Tab键**

| 通配符 | 说明                                       |
| :----- | :----------------------------------------- |
| ?      | 匹配任意一个字符                            |
| *      | 匹配任意多个字符                            |
| []     | 或，如[123]表示123中的任意一个               |
| -      | 代表一个范围：如a-z表示26个小写字母中任意一个 |

## alias命令 别名(自定义命令)

```shell
alias
```

- 用来设置指令的别名。使用该命令可以将一些较长的命令进行简化。
- 使用alias时，用户必须使用单引号''将原来的命令引起来，防止特殊字符导致错误。


### 查看系统已经设置的自定义命令(别名)

```shell
alias
```

```shell
[root@localhost ~]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```

### type命令 检查自定义命令（别名）是否已被使用

```shell
type 自定义命令
```

```shell
[root@bogon ~]# type ls
ls is aliased to `ls --color=auto'
# 已被使用
[root@bogon ~]# type myls
-bash: type: myls: not found
# 未被使用
```

### 设置自定义命令(别名)

- alias命令的作用只局限于该次登入的操作。

```shell
alias 自定义别名='命令'
# 可以是多条命令，之间使用分号;分隔
```

```shell
[root@bogon ~]# alias myls='ls -l;ls -d'
[root@bogon ~]# myls
total 8
-rw-------. 1 root root 1823 Nov  5 13:04 anaconda-ks.cfg
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Desktop
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Documents
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Downloads
-rw-r--r--. 1 root root 1899 Nov  5 13:30 initial-setup-ks.cfg
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Music
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Pictures
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Public
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Templates
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Videos
```

### 撤销自定义命令(别名)

```shell
unalias 自定义命令
```

```shell
[root@bogon ~]# unalias myls
```

### 命令别名永久生效

- alias命令的作用只局限于该次登入的操作（直接在shell里设定的命令别名，在终端关闭或者系统重新启动后都会失效）
- 若要每次登入都能够使用这些命令别名，则可将相应的alias命令存放到bash的初始化文件`/etc/bashrc`中
- 通过`unalias`命令删除的只是本次操作中的（实际上，bashrc文件中未被删除）

```shell
vim ~/.bashrc
# 在打开的文件最后面加入别名设置，保存后重新载入：
# 添加的格式：alias 自定义命令= '命令'
source ~/.bashrc
```


```shell
[root@localhost ~]# vim ~/.bashrc

###以下是文件内容---------

# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
# 在这里添加要加入的别名设置，格式如上

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
# 这段if代码的作用是:
# 即：加载.bash_aliases文件，所以也可以在用户根目录下新建一个文件.bash_aliases存放命令别名设置。

###以上是文件内容-------

[root@localhost ~]# source ~/.bashrc
```

```shell
[root@bogon ~]# vim ~/.bashrc
[root@bogon ~]# source ~/.bashrc
[root@bogon ~]# myls
total 8
-rw-------. 1 root root 1823 Nov  5 13:04 anaconda-ks.cfg
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Desktop
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Documents
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Downloads
-rw-r--r--. 1 root root 1899 Nov  5 13:30 initial-setup-ks.cfg
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Music
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Pictures
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Public
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Templates
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Videos

# 以下是修改的文件内容:
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
alias myls='ls -l'

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
```

## history命令 历史命令的使用

**或者上下光标**

```shell
history
```

**选项**

| 参数 | 作用                                      |
| :--- | :--------------------------------------- |
| -c   | 清空当前历史命令                           |
| -a   | 将历史命令缓冲区中命令写入历史命令文件中     |
| -r   | 将历史命令文件中的命令读入当前历史命令缓冲区 |
| -w   | 将当前历史命令缓冲区命令写入历史命令文件中   |

- n：打印最近的n条历史命令

### 查看最近n条历史命令

```shell
history n
```

```shell
[root@bogon ~]# history 10
   20  myls
   21  unalias myls
   22  vim ~/.bashrc
   23  source ~/.bashrc
   24  ls
   25  ls -l
   26  history | grep start
   27  history | grep ls
   28  ls
   29  history 10
```

### 查找包含特定字符串的历史命令

```shell
history | grep 特点字符串
```

```shell
[root@bogon ~]# history | grep start
    1  startx
   26  history | grep start
[root@bogon ~]# history | grep ls
    5  type ls
    6  type myls
    7  alias myls='ls -l'
    8  mls
    9  alias myls='ls -l'
   10  myls
   11  unalias myls
   12  unalias mylsss
   15  myls
   17  unalias myls
   18  myls
   19  alias myls='ls -l;ls -d'
   20  myls
   21  unalias myls
   24  ls
   25  ls -l
   27  history | grep ls
 
# 历史命令序号 历史命令（执行过的命令）
```

### !!命令 

#### !!命令 执行上一次执行的命令**

```shell
[root@bogon ~]# ls -l
[root@bogon ~]# !!
ls -l
total 8
-rw-------. 1 root root 1823 Nov  5 13:04 anaconda-ks.cfg
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Desktop
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Documents
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Downloads
-rw-r--r--. 1 root root 1899 Nov  5 13:30 initial-setup-ks.cfg
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Music
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Pictures
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Public
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Templates
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Videos
```

##### !num命令 执行指定序号的历史命令**

```shell
!历史命令序号
```

```shell
[root@bogon ~]# !24
ls
anaconda-ks.cfg  Desktop  Documents  Downloads  initial-setup-ks.cfg  Music  Pictures  Public  Templates  Videos
```

#### !字符命令 执行以指定字符开头的历史命令

```shell
!字符
```

```shell
[root@bogon ~]# !l
ls
anaconda-ks.cfg  Desktop  Documents  Downloads  initial-setup-ks.cfg  Music  Pictures  Public  Templates  Videos
```

## 管道与重定向

**文件描述符**

- 大多数Linux进程运行时需要3个文件描述符：

| 文件描述符 | 说明         |
| :-------: | :---------- |
|     0     | 标准输入     |
|     1     | 标准输出     |
|     2     | 标准错误输出 |

**管道符 |**

**管道和重定向的目的：**

- 重定向这些描述符。
- 管道一般为输入和输出的结合，一个进程向管道的一端发送数据，而另一个进程从该管道的另一端读取数据。

## 标准输入与输出

**执行一个Shell命令通常会自动打开3个标准文件**

- 标准输入文件stdin 对应终端的键盘
- 标准输出文件stout和标准错误输出文件stderr 对应终端的屏幕

进程从标准输入文件得到输入数据，将正常输出数据输出到标准输出文件，而错误信息则打印到标准错误文件。

![](c:/notebook/pictures/153845716239573.png =369x)

### echo 输出指定的字符串或者变量

- 用于在shell中打印shell变量的值，或者直接输出指定的字符串。linux的echo命令，在shell编程中极为常用, 在终端下打印变量value的时候也是常常用到的，因此有必要了解下echo的用法echo命令的功能是在显示器上显示一段文字，一般起到一个提示的作用。

**选项 -e 激活转义符**

| 转义符    | 说明    |
| :-- | :-- |
| `\e`或 `\033`    |设置终端的颜色    |

#### `\e` 设置终端颜色

```shell
echo -e "\e[编码1;编码2;编码3m内容"
# 文字色：
#颜色码：重置=0，黑色=30，红色=31，绿色=32，黄色=33，蓝色=34，洋红=35，青色=36，白色=37
echo -e "\e[1;31m内容\e[0m"
#\e[1;31m 将颜色设置为红色
#\e[0m 将颜色重新置回
# 背景色 ：
#颜色码：重置=0，黑色=40，红色=41，绿色=42，黄色=43，蓝色=44，洋红=45，青色=46，白色=47
echo -e "\e[1;42m内容\e[0m"
# 文字闪动：
# 0 关闭所有属性、1 设置高亮度（加粗）、4 下划线、5 闪烁、7 反显、8 消隐
echo -e "\033[37;31;5mMySQL Server Stop...\033[39;49;0m"
```

### printf 格式化并输出结果

![](c:/notebook/pictures/Snipaste_2022-12-17_16-07-55.png =400x)

```shell

```

### cat 连接文件并打印到标准输出设备上

- 从命令行给出的文件中读取数据，并将这些数据直接送到标准输出文件。
- cat经常用来显示文件的内容
- 当文件较大时，文本在屏幕上迅速闪过（滚屏），用户往往看不清所显示的内容。因此，一般用more等命令分屏显示。
   - 为了控制滚屏，可以按`Ctrl+S键`，停止滚屏；
   - 按`Ctrl+Q键`可以恢复滚屏。
   - 按`Ctrl+C（中断）键`可以终止该命令的执行，并且返回Shell提示符状态。

| 选项                   | 说明                                           |
| :-------------------- | :--------------------------------------------- |
| -n或--number          | 从1开始对所有输出的行数编号                      |
| -b或--number-nonblank | 和-n相似，只不过对于空白行不编号                  |
| -s或--squeeze-blank   | 当遇到有连续两行以上的空白行，就代换为一行的空白行 |
| -A                    | 显示不可打印字符，行尾显示“$”                    |
| -e                    | 等价于"-vE"选项                                 |
| -t                    | 等价于"-vT"选项                                 |

**语法**

```shell
cat 文件
```

```shell
cat m1 （在屏幕上显示文件m1的内容）
cat m1 m2 （同时显示文件m1和m2的内容）
cat m1 m2 > file （将文件m1和m2合并后放入文件file中）
```

```shell
[root@bogon ~]# cat initial-setup-ks.cfg anaconda-ks.cfg
```

**cat命令 不带参数时**

- 从标准输入中读取数据并显示到标准输出文件中
- 此时，每一行键盘（标准输入文件）输入的数据，都会立刻被输出在屏幕上（标准输出文件）。
- 直到`Ctrl + d键`终止

```shell
[root@bogon ~]# cat
test
test
```

### tail 在屏幕上显示指定文件的末尾若干行

- 用于输入文件中的尾部内容。tail命令默认在屏幕上显示指定文件的末尾10行。
    - 如果给定的文件不止一个，则在显示的每个文件前面加一个文件名标题。
    - 如果没有指定文件或者文件名为“-”，则读取标准输入。
- 注意：如果表示字节或行数的N值之前有一个”+”号，则从文件开头的第N项开始显示，而不是显示文件的最后N项。N值后面可以有后缀：b表示512，k表示1024，m表示1048576(1M)。

![](c:/notebook/pictures/Snipaste_2022-12-01_17-01-43.png =1100x)

### head

### more

### less

### 输入重定向

**主要用于改变一个命令的输入源**

- 把命令或可执行程序的标准输入重定向到指定的文件中
- 即：将输入从键盘（标准输入文件）改为一个指定的文件。

#### < 输入重定向符 

**语法**

```shell
命令 < 文件
```

```shell
[root@bogon ~]# cat < initial-setup-ks.cfg
```

#### << 文档的重定向操作符

```shell
命令 <<标识符

> 标识符
# 以此结束输入的操作，并使标准输出文件输出。
```

```shell
[root@bogon ~]# cat <<END
> test
> END
test
```

### 输出重定向

- 把命令或可执行程序的标准输出或标准错误输出重新定向到指定文件中。
- 可以将命令的输出从标准输出文件（屏幕）改为指定的文件中。
- 可以将一个命令的输出当作另一个命令的输入

#### > 输出重定向符

**语法**

```shell
命令 > 文件
```

- 如果> 后面的文件不存在，则创建该文件。
- 如果> 后面的文件已经存在，则会被覆盖。

```shell
[root@localhost ~]# ls -l >lsdir.txt

# 或

[root@localhost ~]# ls -l >lsdir.txt
```

#### >> 输出重定向的追加方法

**语法**

```shell
命令 >>文件
```

- 如果> 后面的文件不存在，则创建该文件。
- 如果> 后面的文件已经存在，则将内容追加到该文件。

```shell
[root@localhost ~]# ls -l >>lsdir.txt
```

#### 2> 错误输出重定向

**语法**

```shell
命令 2>文件
```

- 如果有错误信息：（错误信息不会在屏幕（标准输出文件）输出，而会保存在指定的文件中。
   - 如果> 后面的文件不存在，则创建该文件。
   - 如果> 后面的文件已经存在，则会被覆盖。
- 如果没有错误信息，仍然会创建/覆盖。

```shell
[root@localhost ~]# ls -l ./不存在的路径 2>myerr.txt
# 该文件不存在，在myerr.txt中会记录此次错误信息
```

#### 2>> 错误输出重定向的追加方法

```shell
命令 2>>文件
```

- 是`2>`的追加模式
- 如果有错误信息：（错误信息不会在屏幕（标准输出文件）输出，而会保存在指定的文件中。
   - 如果> 后面的文件不存在，则创建该文件。
   - 如果> 后面的文件已经存在，则将内容追加到该文件。
- 如果没有错误信息，仍然会创建/覆盖。


```shell
[root@localhost ~]# ls ./不存在的路径 2>>myerr.txt
```

### 混合使用

```shell
命令 >输出 2> 错误 
命令 >输出 2>>错误
命令 >>输出 2>错误
命令 >>输出 2>>错误

命令 <输入 >输出
命令 <<标识符 >输出
```

```
[root@localhost ~]# ls >myout.txt 2>myerr.txt
[root@localhost ~]# ls <<over >myout.txt
```

#### 将标准输出和标准错误输出重定向到同一个文件

```shell
命令 >输出 2>&1
```

```shell
[root@localhost ~]# ls >myOutAndErr.txt 2>&
```

## | 管道 

- 将一个程序或命令的输出作为另一个程序或命令的输入；
  - 管道符 | 连接两个命令。
  - 或通过临时文件将两个命令或程序结合在一起。
  
```shell
命令1 | 命令2
```

```shell
[root@localhost ~]# ls -l | grep Downloads
drwxr-xr-x. 2 root root    6 Nov  5 13:31 Downloads
```

## grep 过滤文本

- 支持正则。
- 若不指定任何文件名称/给予的文件名为"-"，则grep命令从标准输入设备读取数据。

### 正则

| 参数        | 说明                                       |
| :--------- | :---------------------------------------- |
| \          | 转义符                                     |
| ^          | 指定匹配字符串的行首 ；指定行的开始                |
| $          | 指定匹配字符串的结尾  ； 指定行的结束              |
| *          | 表示0个以上的字符 ；匹配0个或多个先前的字符        |
| +          | 表示1个以上的字符 ；匹配1个或多个先前的字符        |
| ?          | 匹配0个或多个先前的字符                       |
| .          | 匹配一个非换行符的字符                        |
| []         | 匹配一个指定范围内的字符                       |
| [^]        | 匹配一个非指定范围内的字符                     |
| `\(..\)`   | 标记匹配字符                                |
| <          | 指定单词的开始                               |
| >          | 指定单词的结束                               |
| x{m}       | 重复字符x，m次                               |
| x{m,}      | 重复字符x，至少m次                           |
| x{m,n}     | 重复字符x，至少m次，至多n次                    |
| w          | 匹配单词和数字 即：[A-Z] [a-z] [0-9]             |
| W         | w的反置形式，匹配一个或多个非单词字符，如点号句号等 |
| b          | 单词锁定符                                  |
| `|`        | 或                                        |
| ()         | 分组符                                     |
| [:alnum:]  | 文字数字字符                                |
| [:alpha:]  | 文字字符                                   |
| [:digit:]  | 数字字符                                   |
| [:graph:]  | 非空格、控制字符                             |
| [:lower:]  | 小写字符                                   |
| [:upper:]  | 大写字符                                   |
| [:cntrl:]  | 控制字符                                   |
| [:print:]  | 非空字符（包括空格）                          |
| [:punct:]  | 标点符号                                   |
| [:space:]  | 所有空白字符（换行、空格、制表符）                |
| [:xdigit:] | 十六进制数字 0-9 a-f A-F                      |


### 使用

**语法**

```shell
grep 选项 指定字符串 文件路径
```

**查找指定字符串在文件中的匹配内容**

```shell
grep [选项] 指定字符串 文件路径

# 等同于

grep [选项] "指定字符串" 文件路径
```

- 默认区分大小写
- 指定字符串可以使用正则表达式

```shell
[root@bogon ~]# grep root /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
[root@bogon ~]# grep "root" /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
```

**结合管道的使用**

```shell
命令 | grep 指定字符串
```
- 管道前的命令返回的数据作为grep的参数使用，即作为要被查找的内容

```shell
[root@bogon ~]# ifconfig | grep inet
        inet 192.168.186.144  netmask 255.255.255.0  broadcast 192.168.186.255
        inet6 fe80::17f1:ef1c:aad4:3bff  prefixlen 64  scopeid 0x20<link>
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
```

**递归查询的使用**

## awk

**格式**

```shell
awk [options] 'script' var=value file(s)
awk [options] -f scriptfile var=value file(s)
```

**选项**


| 选项    | 说明    |
| :-- | :-- |
|  -F fs    |  fs指定输入分隔符，fs可以是字符串或正则表达式   |
|   -v var=value  |   赋值一个用户定义变量，将外部变量传递给awk  |
|  -f scripfile  | 从脚本文件中读取awk命令    |
|  -m[fr] val   |  对val值设置内在限制，-mf选项限制分配给val的最大块数目；-mr选项限制记录的最大数目。   |

```shell
[root@localhost ~]# awk '{print $0}' /etc/passwd | head
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
```

## 自动任务

### at 定时任务

- 在指定时间执行任务，只执行一次。

![](c:/notebook/pictures/Snipaste_2022-11-27_23-24-32.png =300x)

**格式**

```shell
at [选项] [时间]
```

- 使用`Ctrl + D`结束任务的指定，等待时间执行

**指定时间的格式**

- 绝对计时法
1. 当天的hh:mm，假如该时间已过去，那么就放在第二天执行。
2. midnight（深夜），noon（中午），teatime（一般是下午4点）等比较模糊的词语来指定时间。
3. 12小时计时制，时间后面加上AM（上午）或PM（下午）。
4. 具体日期，指定格式为month day或mm/dd/yy或dd.mm.yy。指定的日期必须跟在指定时间的后面。
- 相对计时法
1. now + count time-units
   - now就是当前时间
   - time-units是时间单位 minutes（分钟）、hours（小时）、days（天）、weeks（星期）
   - count是时间的数量，究竟是几天，还是几小时，等等。 
2. 直接使用today（今天）、tomorrow（明天）。

```shell
[root@localhost ~]# at 23:43 today
at> date >/root/today.log            
at> <EOT>
job 3 at Sun Nov 27 23:43:00 2022
```

![](c:/notebook/pictures/Snipaste_2022-11-27_23-44-25.png =400x)

#### atq 显示当前所有定时任务

```shell
[root@localhost ~]# atq
4       Wed Nov 30 08:40:00 2022 a root
```

#### atrm 删除指定行号的定时任务

**格式**

```shell
atrm 任务序号
```

![](c:/notebook/pictures/Snipaste_2022-11-27_23-49-33.png =300x)

### crontab 周期任务

- 用来提交和管理用户的需要周期性执行的任务。
- 当安装完成操作系统后，默认会安装此服务工具，并且会自动启动crond进程，crond进程每分钟会定期检查是否有要执行的任务，如果有要执行的任务，则自动执行该任务。

**格式**

```shell
crontab [选项] [参数]
```

![](c:/notebook/pictures/Snipaste_2022-11-27_23-55-50.png =550x)

#### 系统任务调度 和 用户任务调度 。

**系统任务调度**

- 系统周期性所要执行的工作，比如写缓存数据到硬盘、日志清理等。
- 系统任务调度的配置文件`/etc/crontab`

![](c:/notebook/pictures/Snipaste_2022-11-28_00-01-43.png =600x)

```shell
[root@localhost etc]# ll | grep cron
-rw-------.  1 root root      541 Mar 30  2017 anacrontab
drwxr-xr-x.  2 root root       54 Nov 13 21:16 cron.d
drwxr-xr-x.  2 root root       70 Nov 13 21:16 cron.daily
-rw-------.  1 root root        0 Mar 30  2017 cron.deny
drwxr-xr-x.  2 root root       41 Nov 13 21:16 cron.hourly
drwxr-xr-x.  2 root root        6 Dec 28  2013 cron.monthly
-rw-r--r--.  1 root root      451 Dec 28  2013 crontab
drwxr-xr-x.  2 root root        6 Dec 28  2013 cron.weekly
```

**用户任务调度**

- 用户定期要执行的工作，比如用户数据备份、定时邮件提醒等。
- 所有用户定义的crontab文件都被保存在`/var/spool/cron`目录中。其文件名与用户名一致，
- 使用者权限文件如下：
  - `/etc/cron.deny `     该文件中所列用户不允许使用crontab命令
  - `/var/spool/cron/`   所有用户crontab文件存放的目录,以用户名命名 

#### 使用crontab

**格式**

```shell
crontab [选项] [crontab 的任务列表文件]
```

![](c:/notebook/pictures/Snipaste_2022-11-28_00-30-55.png =400x)

![](c:/notebook/pictures/Snipaste_2022-11-28_00-12-54.png =800x)

**常用`crontab -e`进行设置和删除**

```shell
# 设置新的周期任务
[root@localhost etc]# crontab -e
no crontab for root - using an empty one
crontab: installing new crontab
# 在vim内编辑：
#* * * * * date >> /root/Test/myDays.log


[root@localhost Test]# cat myDays.log
Mon Nov 28 00:39:01 CST 2022
Mon Nov 28 00:40:01 CST 2022

# 删除周期任务
[root@localhost Test]# crontab -e
crontab: installing new crontab
# 在vim中编辑
#删除周期任务
```

##### 实例

```shell
# 每1分钟执行一次command
* * * * * command

# 每小时的第3和第15分钟执行
3,15 * * * * command

# 在上午8点到11点的第3和第15分钟执行
3,15 8-11 * * * command

# 每隔两天的上午8点到11点的第3和第15分钟执行
3,15 8-11 */2 * * command
# 每个星期一的上午8点到11点的第3和第15分钟执行
3,15 8-11 * * 1 command

# 每晚的21:30重启smb 
30 21 * * * /etc/init.d/smb restart

# 每月1、10、22日的4 : 45重启smb 
45 4 1,10,22 * * /etc/init.d/smb restart

# 每周六、周日的1:10重启smb
10 1 * * 6,0 /etc/init.d/smb restart

# 每天18 : 00至23 : 00之间每隔30分钟重启smb 
0,30 18-23 * * * /etc/init.d/smb restart

# 每星期六的晚上11:00 pm重启smb 
0 23 * * 6 /etc/init.d/smb restart

# 每一小时重启smb 
* */1 * * * /etc/init.d/smb restart

# 晚上11点到早上7点之间，每隔一小时重启smb
* 23-7/1 * * * /etc/init.d/smb restart

# 每月的4号与每周一到周三的11点重启smb 
0 11 4 * mon-wed /etc/init.d/smb restart

# 一月一号的4点重启smb
0 4 1 jan * /etc/init.d/smb restart

# 每小时执行/etc/cron.hourly目录内的脚本
01 * * * * root run-parts /etc/cron.hourly
```

#### crond服务

```shell
service crond status # 查看crond服务状态
/sbin/service crond start    # 启动服务
/sbin/service crond stop     # 关闭服务
/sbin/service crond restart  # 重启服务
/sbin/service crond reload   # 重新载入配置
ntsysv # 查看crond服务是否已经设置为开机启动
chkconfig –level 35 crond on # 加入开机自动启动
```

# 系统管理

## 性能检测与优化

### CPU相关

- uptime 查看Linux系统负载信息
- vmstat 显示虚拟内存状态

#### uptime 查看系统负载

- 能够打印系统总共运行了多长时间和系统的平均负载。
- uptime命令可以显示的信息显示依次为：现在时间、系统已经运行了多长时间、目前有多少登陆用户、系统在过去的1分钟、5分钟和15分钟内的平均负载。

**格式**

```shell
uptime
```

```shell
[root@bogon Test]# uptime
 20:41:51 up  1:21,  3 users,  load average: 0.00, 0.01, 0.05
# 现在时间、系统已运行时间、登陆用户数、系统在过去的1分钟、5分钟和15分钟内的平均负载。
```

#### vmstat

![](C:/notebook/pictures/Snipaste_2022-12-12_15-05-21.png =300x)

```shell
[root@bogon ipv4]# vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 4128428   2116 428040   0    0     3     0   14   11  0  0 100 0  0
```

**Procs（进程）**

- r: 运行队列中进程数量，这个值也可以判断是否需要增加CPU。（长期大于1）
- b: 等待IO的进程数量。

**Memory（内存）**

- swpd: 使用虚拟内存大小，如果swpd的值不为0，但是SI，SO的值长期为0，这种情况不会影响系统性能。
- free: 空闲物理内存大小。
- buff: 用作缓冲的内存大小。
- cache: 用作缓存的内存大小，如果cache的值大的时候，说明cache处的文件数多，如果频繁访问到的文件都能被cache处，那么磁盘的读IO bi会非常小。

**Swap**

- si: 每秒从交换区写到内存的大小，由磁盘调入内存。
- so: 每秒写入交换区的内存大小，由内存调入磁盘。
- 注意：内存够用的时候，这2个值都是0，如果这2个值长期大于0时，系统性能会受到影响，磁盘IO和CPU资源都会被消耗。如果free很少，但是si和so也很少（大多时候是0），那么不用担心，系统性能这时不会受到影响。

**IO（现在的Linux版本块的大小为1kb）**

- bi: 每秒读取的块数
- bo: 每秒写入的块数
- 注意：随机磁盘读写的时候，这2个值越大（如超出1024k)，能看到CPU在IO等待的值也会越大。

**system（系统）**

- in: 每秒中断数，包括时钟中断。
- cs: 每秒上下文切换数。
- 注意：上面2个值越大，会看到由内核消耗的CPU时间会越大。

**CPU（以百分比表示）**

- us: 用户进程执行时间百分比(user time)
   - us的值比较高时，说明用户进程消耗的CPU时间多，但是如果长期超50%的使用，那么我们就该考虑优化程序算法或者进行加速。
- sy: 内核系统进程执行时间百分比(system time)
   - sy的值高时，说明系统内核消耗的CPU资源多，这并不是良性表现，我们应该检查原因。
- wa: IO等待时间百分比
   - wa的值高时，说明IO等待比较严重，这可能由于磁盘大量作随机访问造成，也有可能磁盘出现瓶颈（块操作）。
- id: 空闲时间百分比

###  内存相关

- top
- free
- vmstat

#### free 显示系统内存状态

**格式**

```shell
free [选项]
```

**常用组合**

| 组合          | 作用                    |
| :------------ | :---------------------- |
| free -s 秒    | 每几秒刷新一次           |
| free -b\|k\|m | 以Byte或KB或MB的单位显示 |
| free -t       | 显示内存总和             |

```shell
# free  -mts 10
[root@bogon Test]# free -mts 10
              total        used        free      shared  buff/cache   available
Mem:           4943         911        3037          10         994        3701
Swap:          5119           0        5119
Total:        10063         911        8157
```

**解释**

![](c:/notebook/pictures/Snipaste_2022-11-27_20-59-05.png =700x)


### IO

- iostat

#### iostat 监视系统输入输出设备和CPU的使用情况

- iostat命令 被用于监视系统输入输出设备和CPU的使用情况。它的特点是汇报磁盘活动统计情况，同时也会汇报出CPU使用情况。同vmstat一样，iostat也有一个弱点，就是它不能对某个进程进行深入分析，仅对系统的整体情况进行分析。

![](C:/notebook/pictures/Snipaste_2022-12-12_15-13-00.png =300x)

![](C:/notebook/pictures/Snipaste_2022-12-12_15-14-15.png =430x)

```shell
[root@bogon ipv4]# iostat -x /dev/sda1
Linux 3.10.0-693.el7.x86_64 (bogon)     12/12/2022      _x86_64_        (12 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.01    0.00    0.03    0.00    0.00   99.96

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda1              0.00     0.00    0.18    0.00     0.56     0.20     8.34     0.00    0.13    0.12    3.00   0.12   0.00
```

### 网络性能评估

- netstat

## 运行级别

**启动**

![](c:/notebook/pictures/Snipaste_2022-12-01_16-10-41.png =700x)

![](c:/notebook/pictures/Snipaste_2022-12-01_16-13-01.png =700x)

![](c:/notebook/pictures/Snipaste_2022-12-01_16-14-09.png =800x)

**紧急模式 systemctl rescue**

### runlevel 查看当前用户运行级别

![](c:/notebook/pictures/Snipaste_2022-12-01_16-22-29.png =300x)

### init 改变系统运行级别

- init进程是所有Linux进程的父进程，它的进程号为1。init命令是Linux操作系统中不可缺少的程序之一，init进程是Linux内核引导运行的，是系统中的第一个进程。

```shell
init [选项] 运行级别
```

![](c:/notebook/pictures/Snipaste_2022-12-01_16-23-31.png =300x)

### /etc/inittab init配置文件

### systemctl rescue 紧急模式

### systemctl set-default 指向 更改默认运行级别

- 实际上是修改 /etc/systemd/ssytem/default.target 的指向
- 重启后生效

| 参数               | 指向                                       | 运行级别 |
| :---------------- | :---------------------------------------- | :------- |
| default.target    | /etc/systemd/system/default.target        | -        |
| multi-user.target | /usr/lib/systemd/system/multi-user.target | 3        |
| graphical.target  | /usr/lib/systemd/system/graphical.target  | 5        |

- 查看可选的指向： `ll /usr/lib/systemd/system | grep .target`


```shell
# 查看指向
[root@bogon ~]# ll /etc/systemd/system/default.target
lrwxrwxrwx. 1 root root 36 Nov 13 21:20 /etc/systemd/system/default.target -> /lib/systemd/system/graphical.target
# 更改指向
[root@bogon ~]# systemctl set-default multi-user.target
Removed symlink /etc/systemd/system/default.target.
Created symlink from /etc/systemd/system/default.target to /usr/lib/systemd/system/multi-user.target.
```

## systemctl systemd服务单元控制

**扩展与单元类型**

![](c:/notebook/pictures/Snipaste_2022-12-01_16-33-29.png =750x)

- systemctl命令 是系统服务管理器指令，它实际上将 service 和 chkconfig 这两个命令组合到一起。

![](c:/notebook/pictures/Snipaste_2022-12-01_16-30-36.png =1100x)

### 对服务单元操作

```shell
systemctl start nfs-server.service . # 启动nfs服务
systemctl restart nfs-server.service # 重新启动某服务
systemctl stop firewalld.service # 停止某服务
systemctl enable nfs-server.service # 设置开机自启动
systemctl disable nfs-server.service # 停止开机自启动
systemctl status nfs-server.service # 查看服务当前状态

# 以上等效于：systemctl firewalld stop 
# 或 systemctl status firewalld

systemctl list-units --type=service # 查看所有已启动的服务
systemctl list-units-files # 查看系统中安装的服务
```

### 电源控制

```shell
systemctl poweroff # 关机
systemctl reboot # 重启
systemctl suspend # 待机

# 以上等效于 poweroff
```

### 单元配置文件

**存放位置**

|                                 |                          |
| ------------------------------- | ------------------------ |
| 存放软件包安装的单元              | /usr/lib/systemd/system/ |
| 由系统管理员安装的与系统密切的单元 | /etc/systemd/system      |


**分为三个小节**

| 小节     | 说明                                                                 |
| :------ | :------------------------------------------------------------------- |
| Unit    | 主要是单元的描述和依赖                                                 |
| Service | 单元的最主要内容，主要定义了服务的类型、启动、停止的命令、杀死服务的信号等 |
| Install | 安装单元                                                             |

- 以vmtoolsd.service为例

```shell
[root@bogon ~]# cat /usr/lib/systemd/system/vmtoolsd.service
```

![](c:/notebook/pictures/Snipaste_2022-12-02_17-45-08.png =900x)

#### 添加单元配置文件

1. 将单元配置文件放入相应的目录位置
2. 执行`systemctl daemon-relod`

## 进程管理

**分类**

| 进程           | 启动                                      |       运行        |      状态       |
| :------------ | :---------------------------------------- | ----------------- | --------------- |
| **交互进程**   |                                           |                   |                 |
| **批处理进程** |                                           |                   |                 |
| **守护进程**   | 系统开机时通过脚本自动激活启动或root用户启动 | 后台运行，一直运行 | 等待请求处理任务 |

**进程的属性**

| 属性              | 说明                          |
| :---------------- | :---------------------------- |
| PID               | 进程ID，唯一值，区分进程        |
| PPID              | 父进程和父进程的PID            |
| UID、GID          | 启动进程的用户ID和所归属的组ID  |
| 进程状态          | R（运行）、S（休眠）、Z（僵尸） |
| 进程执行的优先级   |                               |
| 进程所连接的终端名 |                               |
| 进程资源占用       |                               |

### ps 进程监视

- 用于报告当前系统的进程状态（非动态）
- 可以搭配kill指令随时中断、删除不必要的程序。

```shell
ps [选项]
```

![](c:/notebook/pictures/Snipaste_2022-12-03_14-39-40.png =600x)
![](c:/notebook/pictures/Snipaste_2022-12-03_14-40-11.png =600x)

**常用**

```shell
ps axo pid,comm,pcpu # 查看进程的PID、名称以及CPU 占用率
ps aux | sort -rnk 4 # 按内存资源的使用量对进程进行排序
ps aux | sort -nk 3  # 按 CPU 资源的使用量对进程进行排序
ps -A # 显示所有进程信息
ps -u root # 显示指定用户信息
ps -efL # 查看线程数
ps -e -o "%C : %p :%z : %a"|sort -k5 -nr # 查看进程并按内存使用大小排列
ps -ef # 显示所有进程信息，连同命令行
ps -ef | grep ssh # ps 与grep 常用组合用法，查找特定进程
ps -C nginx # 通过名字或命令搜索进程
ps aux --sort=-pcpu,+pmem # CPU或者内存进行排序,-降序，+升序
ps -f --forest -C nginx # 用树的风格显示进程的层次关系
ps -o pid,uname,comm -C nginx # 显示一个父进程的子进程
ps -e -o pid,uname=USERNAME,pcpu=CPU_USAGE,pmem,comm # 重定义标签
ps -e -o pid,comm,etime # 显示进程运行的时间
ps -aux | grep named # 查看named进程详细信息
ps -o command -p 91730 | sed -n 2p # 通过进程id获取服务名称
```

![](c:/notebook/pictures/Snipaste_2022-12-03_14-55-49.png =1100x)

![](c:/notebook/pictures/Snipaste_2022-12-03_14-57-52.png =700x)

```shell
[root@bogon ~]# ps aux | head -2
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          1  0.0  0.0 191700  4760 ?        Ss   13:15   0:04 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
```

### top 系统实时状态监视

- 动态交互命令
- 默认每5秒刷新一次

![](c:/notebook/pictures/Snipaste_2022-12-03_15-02-00.png =800x)

![](c:/notebook/pictures/Snipaste_2022-12-03_15-20-11.png =1000x)

- 应用程序实际占用的内存：MemTotal - MemFree - Buffer - Cached (KB)而不是MemUsed

**常用**

```shell
top -n 1 # 显示一次结果就退出
top -p 5 # 显示指定PID的进程当前状态
top -u oracle # 显示某一用户的进程信息
```

### 前台进程和后台进程

- 前台进程会占用终端窗口，后台进程不会
- 启动前台进程：正常启动即可；启动后台进程：在启动命令后加上`$`

```shell
[root@bogon ~]# ps -ef $
error: garbage option

Usage:
 ps [options]

 Try 'ps --help <simple|list|output|threads|misc|all>'
  or 'ps --help <s|l|o|t|m|a>'
 for additional help text.

For more details see ps(1).
```

**从前台进程切换到后台**

1. Ctrl + Z 让正在执行的前台进程暂停
2. `jobs` 获取当前的后台作业号
3. `bg 作业号` 将进程放入后台执行

**从后台到前台**

1. `jobs` 获取当前的后台作业号
2. `fg 作业号` 将进程放入前台执行

```shell
[root@bogon ~]# top
#前台显示。。。
# Ctrl + Z
[1]+  Stopped                 top
# 查看作业号 
[root@bogon ~]# jobs
[1]+  Stopped                 top
# 放入后台
[root@bogon ~]# bg 1
[1]+ top &

[1]+  Stopped                 top
# 放入前台
[root@bogon ~]# fg 1
#前台显示。。。
```

### kill / killall 杀死进程

- <mark>当父进程被终止时，子进程也被终止；而子进程的终止不会导致父进程的终止</mark>。

- 用来删除执行中的程序或工作。kill可将指定的信息送至程序。
- 预设的信息为SIGTERM(15),可将指定程序终止。若仍无法终止该程序，可使用SIGKILL(9)信息尝试强制删除程序。
- 程序或工作的编号可利用ps指令或jobs指令查看。
- <mark>kill 按PID杀死单个进程，killall 按进程名杀死同名的所有进程</mark>。

![](c:/notebook/pictures/Snipaste_2022-12-03_15-44-29.png =500x)

**信号 默认15**

![](c:notebook/pictures/Snipaste_2022-12-03_15-45-41.png =500x)

```shell
# ps/top 找出进程号PID
[root@bogon ~]# ps -ef | pgrep -l top
7260 top
# kill进行杀死
# kill 按PID杀死，信号9 #在使用该进程的会显示killed
[root@bogon ~]# kill -9 7260
# killall 按进程名杀死同名的所有，信号9
[root@bogon ~]# killall -9 top
```

### nice / renice 进程优先级

**优先级**

- 优先级的数值为-20~19
- 负值和0是高优先级
- 启动进程时，其默认优先级是0
- <mark>进程的优先级是父进程Shell优先级的值与所指定优先级的**相加和**</mark>

#### nice 

- 在创建进程（可以是命令的执行）时指定进程的优先级
- 优先级是父进程和nice指定的优先的相加和，即：<mark>nice指定的并不就是该进程的优先级，而是一个**增量**</mark>。

![](c:/notebook/pictures/Snipaste_2022-12-03_15-58-39.png =300x)

- 注：指定优先级时：`nice -19 进程`表示优先级是19而不是-19，如果要-19则：`nice --19 进程`；也可以使用`nice -n -19 进程`表示-19.

```shell
[oracle@bogon Test]$ nice -19 tar zcf packTest.tar.gz test.txt
[oracle@bogon Test]$ nice -n 19 rm packTest.tar.gz
```

## 端口管理

### 修改默认端口

### 以ssh为例

```shell
# 修改修改配置文件 /etc/ssh/sshd_config
#Port 22 取消注释改为 #Port 10222
# 先临时关闭SELinux，SELinux会阻止端口的修改而导致sshd服务重启失败
[root@bogon ~]# setenforce 0
setenforce: SELinux is disabled
# 重启sshd服务
[root@bogon ~]# systemctl restart ssh
# 查看端口
[root@bogon ~]# netstat -plnt | grep 10222
tcp        0      0 0.0.0.0:10222           0.0.0.0:*               LISTEN      8553/sshd           
tcp6       0      0 :::10222                :::*                    LISTEN      8553/sshd
# 此时远程连接的软件ssh端口号需要改为10222
# 如果有问题，对SELinux和firewalld操作
[root@bogon ~]# systemctl stop firewalld
[root@bogon ~]# systemctl disable firewalld
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
```

# 文件管理

## 文件信息

![](c:/notebook/pictures/Snipaste_2022-11-28_20-30-27.png =700x)

**文件类型**

| 参数    | 说明    |
| :-- | :-- |
| -     |  普通文件   |
|  d   |  目录文件   |
|   l  | 符号链接文件    |
|   b/c  |    设备文件；b-块设备文件；c-字符设备文件 |
|  p   |  管道文件   |

**文件权限**

| 权限 | 说明     |
| :--- | :------- |
| r    | 读权限   |
| w    | 写权限   |
| x    | 执行权限 |

- 数字对应的权限

| 权限 | 说明               |
| :--- | :----------------- |
| 777  | 绝对权限 rwxrwxrwx |
|      |                    |

## 文件权限

### chown 改变文件所有者

![](c:/notebook/pictures/Snipaste_2022-11-28_20-35-01.png =700x)

**常用**

| 命令                       | 说明                                             |
| :------------------------- | :----------------------------------------------- |
| chown -R 用户 文件/目录文件 | 将目录及其下面的所有文件、子目录的文件主改成指定用户 |

```shell
[root@bogon Test]# ll
total 8
-rw-r--r--. 1 root root  116 Nov 28 00:42 myDays.log
[root@bogon Test]# chown zjk myDays.log
[root@bogon Test]# ll
total 8
-rw-r--r--. 1 zjk  root  116 Nov 28 00:42 myDays.log

[root@bogon Test]# ll
total 8
-rw-r--r--. 1 zjk  root  116 Nov 28 00:42 myDays.log
drwxr-xr-x. 3 root root   78 Nov 27 20:35 zipTest
-rw-r--r--. 1 root root 1588 Nov 27 20:29 zipTest1.zip
[root@bogon Test]# chown -R zjk zipTest
[root@bogon Test]# ll
total 8
-rw-r--r--. 1 zjk  root  116 Nov 28 00:42 myDays.log
drwxr-xr-x. 3 zjk  root   78 Nov 27 20:35 zipTest
-rw-r--r--. 1 root root 1588 Nov 27 20:29 zipTest1.zip
[root@bogon Test]# cd zipTest
[root@bogon zipTest]# ll
total 12
-rw-r--r--. 1 zjk root 424 Nov 27 20:27 test1.zip
-rw-r--r--. 1 zjk root 323 Nov 27 20:25 testZip1.txt
-rw-r--r--. 1 zjk root 323 Nov 27 20:26 testZip2.txt
drwxr-xr-x. 2 zjk root  63 Nov 27 20:27 zipTest
```

### chgrp 改变文件所属组

- 组名可以是用户组的id，也可以是用户组的组名

**格式**

```shell
chgrp [选项][组群][文件|目录]
```

![](c:/notebook/pictures/Snipaste_2022-11-28_20-41-22.png =700x)

```shell
[root@bogon zipTest]# chgrp -R root /root/Test/zipTest
```

### chmod 变更文件或目录的权限

![](c:/notebook/pictures/Snipaste_2022-12-09_16-46-13.png =650x)

**权限**

## 文件操作

### touch 创建新的空文件

- 一是用于把已存在文件的时间标签更新为系统当前的时间（默认方式），它们的数据将原封不动地保留下来；二是用来创建新的空文件。

![](c:/notebook/pictures/Snipaste_2022-12-19_16-48-24.png =600x)


### sed 流编辑器

- 流编辑器，
- 处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有 改变，除非使用重定向存储输出。

**命令格式**

```shell
sed [options] 'command' file(s)
sed [options] -f scriptfile file(s)
```

```shell
[root@localhost ~]# head -3 /etc/passwd |sed -n 2p
bin:x:1:1:bin:/bin:/sbin/nologin
[root@localhost ~]# head -3 /etc/passwd |sed -n 2,3p
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

**选项**

| options                              | 说明                                      |
| :----------------------------------- | :---------------------------------------- |
| `-e<script>或--expression=<script>`   | 以选项中的指定的script来处理输入的文本文件   |
| `-f<script文件>或--file=<script文件>` | 以选项中指定的script文件来处理输入的文本文件 |
| -h或--help                            | 显示帮助                                   |
| -n或--quiet或--silent                 | 仅显示script处理后的结果                    |
| -V或--version                         | 显示版本信息                               |

**sed替换标记**

| 命令 | 说明 |
| :--- | :--- |
|g | 表示行内全面替换。  |
|p | 表示打印行。  |
|w | 表示把行写入一个文件。  |
|x | 表示互换模板块中的文本和缓冲区中的文本。|  
|y | 表示把一个字符翻译为另外的字符（但是不用于正则表达式）|
|\1 | 子串匹配标记|
|& | 已匹配字符串标记|

**sed元字符集**

| 命令      | 说明                                                                                    |
| :------- | :-------------------------------------------------------------------------------------- |
| ^        | 匹配行开始，如：/^sed/匹配所有以sed开头的行。                                              |
| $        | 匹配行结束，如：/sed$/匹配所有以sed结尾的行。                                              |
| .        | 匹配一个非换行符的任意字符，如：/s.d/匹配s后接一个任意字符，最后是d。                        |
| *        | 匹配0个或多个字符，如：/*sed/匹配所有模板是一个或多个空格后紧跟sed的行。                     |
| []       | 匹配一个指定范围内的字符，如/[ss]ed/匹配sed和Sed。                                         |
| [^]      | 匹配一个不在指定范围内的字符，如：/[^A-RT-Z]ed/匹配不包含A-R和T-Z的一个字母开头，紧跟ed的行。 |
| \(..\)   | 匹配子串，保存匹配的字符，如s/\(love\)able/\1rs，loveable被替换成lovers。                  |
| &        | 保存搜索字符用来替换其他字符，如s/love/ **&** /，love这成 **love** 。                      |
| \<       | 匹配单词的开始，如:/\<love/匹配包含以love开头的单词的行。                                   |
| \>       | 匹配单词的结束，如/love\>/匹配包含以love结尾的单词的行。                                    |
| x\{m\}   | 重复字符x，m次，如：/0\{5\}/匹配包含5个0的行。                                             |
| x\{m,\}  | 重复字符x，至少m次，如：/0\{5,\}/匹配至少有5个0的行。                                      |
| x\{m,n\} | 重复字符x，至少m次，不多于n次，如：/0\{5,10\}/匹配5~10个0的行。                             |

### tar 打包/解包

```shell
tar 选项 参数
```

**扩展名**

- `.tar`
- `.tar.gz`
- `.tar.bz2`

**常用组合**

| 组合                                                        | 作用                    |
| :---------------------------------------------------------- | :--------------------- |
| tar -cvf 输出文件 目标文件                                   | 仅打包                  |
| tar -zcvf 输出文件 目标文件                                  | 打包，gzip压缩          |
| tar -jcvf 输出文件 目标文件                                  | 打包，bzip2压缩         |
| tar -ztvf 目标文件                                           | 查看压缩包文件列表       |
| tar -zxvf 目标文件                                           | 解压压缩包到当前路径     |
| tar -zxvf 目标文件 指定文件                                  | 只压缩指定文件          |
| tar -zxvpf 输出文件 目标文件                                 | 建立压缩包时保留文件属性 |
| tar --exclude 排除的文件1 排除的文件2 ... 执行的选项/组合 参数 | 排除某些文件            |

```shell
tar -cvf /tmp/etc.tar /etc
tar -zcvf /tmp/etc.tar.gz /etc
tar -zcvf /tmp/etc.tar.bz2 /etc
tar -ztvf /tmp/etc.tar.bz2
tar -zxvf /tmp/etc.tar.gz etc/passwd
tar --exclude /home/*log -zcvf /tmp/etc2.tar.gz etc/passwd
```

### zip / unzip 压缩/解压缩

**格式**

```shell
zip [参数] [打包后的文件名] [打包的目录路径]
```

**压缩常用组合**

| 组合                      | 作用       |
| :------------------------ | :-------- |
| zip 压缩文件名.zip 文件名   | 压缩文件   |
| zip -r 压缩包名.zip 文件夹 | 压缩文件夹 |

**解压缩常用组合**

| 组合                                  | 作用                    |
| :----------------------------------- | :--------------------- |
| unzip 压缩文件名.zip                  | 解压文件                |
| unzip 压缩文件名.zip -d 目标文件夹路径 | 解压到目标文件夹         |
| unzip -v 压缩包名.zip                 | 查看压缩包内容，而不解压 |

```shell
# zip 压缩包.zip 文件
[root@bogon zipTest]# zip test1.zip testZip1.txt
updating: testZip1.txt (deflated 23%)
[root@bogon zipTest]# ll
total 12
-rw-r--r--. 1 root root 424 Nov 27 20:27 test1.zip
-rw-r--r--. 1 root root 323 Nov 27 20:25 testZip1.txt
-rw-r--r--. 1 root root 323 Nov 27 20:26 testZip2.txt

# zip -r 压缩包.zip 文件夹
[root@bogon Test]# zip -r zipTest1.zip zipTest
  adding: zipTest/ (stored 0%)
  adding: zipTest/testZip1.txt (deflated 23%)
  adding: zipTest/testZip2.txt (deflated 23%)
  adding: zipTest/test1.zip (stored 0%)
[root@bogon Test]# ll
total 4
drwxr-xr-x. 2 root root   63 Nov 27 20:27 zipTest
-rw-r--r--. 1 root root 1588 Nov 27 20:29 zipTest1.zip

# unzip 压缩包.zip
[root@bogon Test]# unzip zipTest1.zip
Archive:  zipTest1.zip
replace zipTest/testZip1.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: y
  inflating: zipTest/testZip1.txt    
replace zipTest/testZip2.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: y
  inflating: zipTest/testZip2.txt    
replace zipTest/test1.zip? [y]es, [n]o, [A]ll, [N]one, [r]ename: y
 extracting: zipTest/test1.zip       
[root@bogon Test]# ll
total 4
drwxr-xr-x. 2 root root   63 Nov 27 20:32 zipTest
-rw-r--r--. 1 root root 1588 Nov 27 20:29 zipTest1.zip

# unzip 压缩包.zip -d ./Test
[root@bogon Test]# unzip zipTest1.zip -d /root/Test/zipTest
Archive:  zipTest1.zip
   creating: /root/Test/zipTest/zipTest/
  inflating: /root/Test/zipTest/zipTest/testZip1.txt  
  inflating: /root/Test/zipTest/zipTest/testZip2.txt  
 extracting: /root/Test/zipTest/zipTest/test1.zip  
[root@bogon Test]# ll
total 4
drwxr-xr-x. 3 root root   78 Nov 27 20:35 zipTest
-rw-r--r--. 1 root root 1588 Nov 27 20:29 zipTest1.zip
[root@bogon Test]# cd zipTest
[root@bogon zipTest]# ll
total 12
-rw-r--r--. 1 root root 424 Nov 27 20:27 test1.zip
-rw-r--r--. 1 root root 323 Nov 27 20:25 testZip1.txt
-rw-r--r--. 1 root root 323 Nov 27 20:26 testZip2.txt
drwxr-xr-x. 2 root root  63 Nov 27 20:27 zipTest

# unzip -v 压缩包.zip
[root@bogon Test]# unzip -v zipTest1.zip
Archive:  zipTest1.zip
 Length   Method    Size  Cmpr    Date    Time   CRC-32   Name
--------  ------  ------- ---- ---------- ----- --------  ----
       0  Stored        0   0% 11-27-2022 20:27 00000000  zipTest/
     323  Defl:N      250  23% 11-27-2022 20:25 eb905491  zipTest/testZip1.txt
     323  Defl:N      250  23% 11-27-2022 20:26 eb905491  zipTest/testZip2.txt
     424  Stored      424   0% 11-27-2022 20:27 a68b676d  zipTest/test1.zip
--------          -------  ---                            -------
    1070              924  14%                            4 files
```

### mv 移动/重命名

- 用来对文件或目录重新命名，或者将文件从一个目录移到另一个目录中。
- 如果将一个文件移到一个已经存在的目标文件中，则目标文件的内容将被覆盖。

**源文件被移至目标文件有两种不同的结果：**

1. 如果目标文件是到某一目录文件的路径，源文件会被移到此目录下，且文件名不变。
2. 如果目标文件不是目录文件，则源文件名（只能有一个）会变为此目标文件名，并覆盖己存在的同名文件。

![](c:/notebook/pictures/Snipaste_2022-12-03_16-13-44.png =1100x)


**常用**

| 组合           | 说明                |
| :------------ | :------------------ |
| mv 源文件 目标 | 将源文件移到目标位置 |

### rm 删除

### cp 复制

**常用**

```shell
cp 复制的文件路径 目标
```

### mkdir 新建文件夹

![](c:/notebook/pictures/Snipaste_2022-12-09_16-43-44.png =500x)


```shell
# 在当前路径下创建目录
mkdir 目录
# 创建目录及其上层目录
mkdir /目录1/目录2
# 创建目录的同时指定权限
mkdir -m 777 目录
```

# 磁盘管理

- Linux的所有文件和目录都存在于根分区/中
- Linux系统中的每一个硬件设备都映射到系统的一个文件

**硬盘分区类型主要有：主分区、扩展分区、逻辑分区。**

- 每一个硬盘设备最多只能由4个主分区构成，任何一个扩展分区都要占用一个主分区号码（即：主分区和扩展分区数量最多为4），占用分区号1~4。当分区数量大于4时，要用到扩展分区和逻辑分区，先划分扩展分区，再在扩展分区的基础上建立逻辑分区。
- 在进行系统分区时，主分区一般设置为激活状态，用于在系统启动时引导系统。
- 分区时，每个分区的大小可以由用户指定。

## df 查看磁盘占用情况

**格式**

```shell
df [选项][参数]
```

![](c:/notebook/pictures/Snipaste_2022-11-28_20-45-10.png =700x)

**常用组合**

| 组合       | 说明                         |
| :-------- | :-------------------------- |
| df -ah    | 查看当前系统所有分区使用情况   |
| df -i     | 查看各个分区inode节点占用情况  |
| df -T     | 显示分区类型                 |
| df -t xfs | 显示指定文件类型的磁盘使用情况 |

```shell
# 仅展示部分

# df
[root@bogon zipTest]# df
Filesystem            1K-blocks    Used Available Use% Mounted on
/dev/mapper/rhel-root  46110724 3259840  42850884   8% /
devtmpfs                2515252       0   2515252   0% /dev

# df -ah
[root@bogon zipTest]# df -ah
Filesystem             Size  Used Avail Use% Mounted on
rootfs                    -     -     -    - /
sysfs                     0     0     0    - /sys
tmpfs                  495M   12K  495M   1% /run/user/42
tmpfs                  495M     0  495M   0% /run/user/0

# df -i
[root@bogon zipTest]# df -i
Filesystem              Inodes  IUsed    IFree IUse% Mounted on
/dev/mapper/rhel-root 23066624 119968 22946656    1% /
devtmpfs                628813    447   628366    1% /dev

# df -T
[root@bogon zipTest]# df -T
Filesystem            Type     1K-blocks    Used Available Use% Mounted on
/dev/mapper/rhel-root xfs       46110724 3259844  42850880   8% /
devtmpfs              devtmpfs   2515252       0   2515252   0% /dev

# DF -T xfs 
[root@bogon zipTest]# df -t xfs
Filesystem            1K-blocks    Used Available Use% Mounted on
/dev/mapper/rhel-root  46110724 3259844  42850880   8% /
/dev/sda1               1038336  163996    874340  16% /boot
```

## du 查看文件/目录占用空间

![](c:/notebook/pictures/Snipaste_2022-11-28_20-54-15.png =700x)

**常用**

| 组合                 | 说明                             |
| :------------------ | :------------------------------ |
| du -sh              | 统计当前文件大小，默认不统计软链接 |
| du --max-depth=1 -h | 按层级统计文件夹大小              |

```shell
# du -sh
[root@bogon ~]# du -sh /root/Test
32K     /root/Test

# du --max-depth=1-- -h
[root@bogon ~]# du --max-depth=1-- -h /root/Test
24K     /root/Test/zipTest
32K     /root/Test
```

## tune2fs 调整查看文件系统参数

- tune2fs允许系统管理员在Linux ext2、ext3或ext4文件系统上调整各种可调的文件系统参数。

**常用**

| 组合                       | 说明                         |
| :------------------------- | :--------------------------- |
| tune2fs -l 磁盘            | 查看分区信息                  |
| tune2fs -i 时间 磁盘        | 设置多久之后自检              |
| tune2fs -e remount-ro 磁盘 | 设置当磁盘出错时重新挂载为只读 |
| tune2fs -c -l -i 磁盘      | 设置磁盘永久不自检            |

## mkfs 格式化文件系统

**查看当前系统中支持mkfs的文件系统格式**

```shell
ls -l /usr/sbin/mkfs.*

[root@localhost ~]# ll /usr/sbin/mkfs.*
-rwxr-xr-x. 1 root root 375240 Mar  8  2017 /usr/sbin/mkfs.btrfs
-rwxr-xr-x. 1 root root  36984 Jun  1  2017 /usr/sbin/mkfs.cramfs
-rwxr-xr-x. 4 root root  96304 Mar 16  2017 /usr/sbin/mkfs.ext2
-rwxr-xr-x. 4 root root  96304 Mar 16  2017 /usr/sbin/mkfs.ext3
-rwxr-xr-x. 4 root root  96304 Mar 16  2017 /usr/sbin/mkfs.ext4
-rwxr-xr-x. 1 root root  28624 Mar  5  2014 /usr/sbin/mkfs.fat
-rwxr-xr-x. 1 root root  37096 Jun  1  2017 /usr/sbin/mkfs.minix
lrwxrwxrwx. 1 root root      8 Nov 13 21:14 /usr/sbin/mkfs.msdos -> mkfs.fat
lrwxrwxrwx. 1 root root      8 Nov 13 21:14 /usr/sbin/mkfs.vfat -> mkfs.fat
-rwxr-xr-x. 1 root root 368464 May 10  2017 /usr/sbin/mkfs.xfs
```

![](c:/notebook/pictures/Snipaste_2022-11-28_23-27-57.png =400x)

**常用**

| 组合                         | 说明                               |
| :--------------------------- | :-------------------------------- |
| mkfs -t 文件系统格式 磁盘分区 | 将指定分区格式化为指定的文件系统格式 |

```shell
mkfs -t ext4 /dev/sdb1
```

## mount 挂载/卸载文件系统

- 挂载点必须是一个目录，如果挂载点原本有内容，则在挂载时看不到，卸载后才能重新看到。

![](c:/notebook/pictures/Snipaste_2022-11-28_23-34-30.png =1100x)

**常用**

| 组合                           | 说明                                                  |
| :----------------------------- | :--------------------------------------------------- |
| mount                          | 查看系统的挂载                                         |
| mount -a                       | 挂载/ect/fstab里的所有分区                             |
| mount 磁盘分区 目录             | 挂载指定分区到指定目录                                  |
| mount -o re 磁盘分区 目录       | 以只读的方式挂载                                       |
| mount -t iso9660 磁盘分区 目录  | 挂载光驱，使用ISO文件避免将文件解压，可以在挂载后直接访问 |
| mount -t nfs 地址:磁盘分区 目录 | NFS挂载                                               |
| mount -t ntfs 磁盘分区 目录     | 挂载Windows下分区格式的分区                            |

- 光盘的磁盘分区通常是/dev/cdrom
- 挂载到的目录可以是/mnt中新建。

```shell
# 新建文件目录
mkdir -p /mnt/room
# 光盘挂载磁盘分区一般是 /dev/cdrom
mount -t iso9660 /dev/cdrom /mnt/room
```

### /etc/fstab文件 自动挂载

![](c:/notebook/pictures/Snipaste_2022-11-29_00-08-22.png =850x)

## fdisk 基本磁盘管理

![](c:/notebook/pictures/Snipaste_2022-11-29_14-57-50.png =900x)

**常用**

```
[root@bogon ~]# fdisk /dev/sda
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel 
   c   toggle the dos compatibility flag
   d   delete a partition # 删除一个磁盘分区
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition # 添加分区
   o   create a new empty DOS partition table
   p   print the partition table # 查看分区信息
   q   quit without saving changes # 不保存退出
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit # 保存变更信息
   x   extra functionality (experts only)
```

## SWAP空间管理

## RAID 磁盘冗余阵列

# 日志系统

## 日志文件

- Linux日志一般放在/var/log下，需要root用户权限

**日志类型**

| 类型         | 说明                                                             |
| :---------- | :-------------------------------------------------------------- |
| 系统连接日志 | 记录系统的登录记录和用户名，写录到/var/log/wtmp和/var/log/utmp      |
| 进程统计     | 系统内核执行，当一个进程终止时为每个进程往进程统计文件中写一个记录    |
| 错误日志     | 各种系统守护进程、用户程序和内核，通过syslog向/var/log/messages写录 |

**常用日志文件**

| 日志        | 说明                                     | 文件类型   |
| :--------- | :--------------------------------------- | :-------- |
| access-log | 记录Web服务访问日志，错误信息存于error-log |           |
| acct/pacct | 记录用户命令                              |           |
| btmp       | 记录失败的记录                            |           |
| messages   | 服务器的系统日志                          | 文本       |
| sudolog    | 记录使用sudo发出的命令                    |           |
| sulog      | 记录su命令的使用                          |           |
| syslog     | 从syslog中记录信息                        |           |
| secure     | 记录系统登录行为                          | 文本       |
| utmp       | 记录当前登录的每个用户                     | 二进制文件 | 
| wtmp       | 一个用户每次登录和退出时间的永久记录        | 二进制文件 |
| lastlog    | 记录最后一次失败的登录和最近几次成功的登录  |           |


![](c:/notebook/pictures/Snipaste_2022-11-29_15-22-13.png =900x)

###  查看日志文件

**常用命令**

- 对一般的文本日志文件：cat、tail、head
   - /var/log/messages
- 对二进制日志文件：who、w、users、last、ac、
   - /var/log/wtmp
   - /var/log/utmp

#### who 显示目前登录系统的用户信息

![](c:/notebook/pictures/Snipaste_2022-11-29_15-31-13.png =900x)

- 参数：/var/log/wtmp

```shell
[root@bogon ~]# who 
root     pts/0        2022-11-29 14:58 (192.168.186.1)
[root@bogon ~]# who /var/log/wtmp
zjk      :0           2022-11-13 21:34 (:0)
zjk      pts/0        2022-11-13 21:34 (:0)
root     :1           2022-11-13 21:34 (:1)
root     pts/0        2022-11-13 21:36 (:1)
root     pts/1        2022-11-13 21:38 (:1)
root     pts/1        2022-11-13 21:42 (192.168.186.1)
root     pts/0        2022-11-27 20:23 (192.168.186.1)
root     pts/1        2022-11-27 20:24 (192.168.186.1)
root     pts/0        2022-11-27 23:21 (192.168.186.1)
root     pts/0        2022-11-27 23:35 (192.168.186.1)
root     pts/0        2022-11-28 19:48 (192.168.186.1)
root     pts/0        2022-11-28 23:18 (192.168.186.1)
root     pts/0        2022-11-29 14:58 (192.168.186.1)
[root@bogon ~]# who /var/log/utmp
[root@bogon ~]# who -q
root
# users=1
[root@bogon ~]# who -H
NAME     LINE         TIME             COMMENT
root     pts/0        2022-11-29 14:58 (192.168.186.1)
```

#### w 显示目前登入系统的用户信息

![](c:/notebook/pictures/Snipaste_2022-11-29_15-34-45.png =400x)


```shell
[root@bogon ~]# w
 15:35:12 up  3:02,  1 user,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    192.168.186.1    14:58    8.00s  0.22s  0.05s w
[root@bogon ~]# w /var/log/utmp
 15:35:27 up  3:02,  1 user,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
```

#### users 显示当前登录系统的所有用户

- 如果一个用户有不止一个登录会话，那他的用户名将显示相同的次数

![](c:/notebook/pictures/Snipaste_2022-11-29_15-36-53.png =300x)

```shell
[root@bogon ~]# users
root
```

#### last 列出目前与过去登入系统的用户相关信息

- 用于显示用户最近登录信息。单独执行last命令，它会读取/var/log/wtmp的文件，并把该给文件的内容记录的登入系统的用户名单全部显示出来

![](c:/notebook/pictures/Snipaste_2022-11-29_15-38-09.png =400x)


```shell
[root@bogon ~]# last -3
root     pts/0        192.168.186.1    Tue Nov 29 14:58   still logged in   
root     pts/0        192.168.186.1    Mon Nov 28 23:18 - 00:18  (00:59)    
root     pts/0        192.168.186.1    Mon Nov 28 19:48 - 21:08  (01:19) 
```

#### dmesg 显示Linux系统启动信息

- 被用于检查和控制内核的环形缓冲区。
- 内核会将开机信息存储在ring buffer中。若是开机时来不及查看信息，可利用dmesg来查看。
- 开机信息保存在/var/log/dmesg文件里。

![](c:/notebook/pictures/Snipaste_2022-11-29_15-42-37.png =400x)

**常用**

| 组合                    | 说明            |
| :--------------------- | :-------------- |
| dmesg \| grep -i error | 查看系统异常情况 |

## rsyslog日志系统

**配置文件 /etc/rsyslog.conf rsyslog**

![](c:/notebook/pictures/Snipaste_2022-11-29_19-47-09.png =800x)

**设备**

![](c:/notebook/pictures/Snipaste_2022-11-29_19-48-07.png =800x)

**优先符限定符**

| 限定符 | 说明                                                                                |
| :----- | :--------------------------------------------------------------------------------- |
| *      | 服务生成的<mark>所有日志消息</mark>都发送到操作动作指定的地点                           |
| =      | 服务生成的<mark>本优先级的日志消息</mark>都发送到操作动作指定的地点                     |
| !      | 服务生成的所有日志消息都发送到操作动作指定的地点，但本<mark>优先级的消息不包括在内</mark> |

**操作动作**

- 每条消息都会经过所有的规则，并非唯一匹配的。

![](c:/notebook/pictures/Snipaste_2022-11-29_19-52-55.png =800x)

```shell
# 编辑/etc/rsyslog.conf

# 把邮件中除info级别外都写入/var/log/maillog文件
mail.*;mail.!=info                          /var/log/maillog

# 仅仅把邮件的通知消息发送给tty12终端设备
main.=info                       /dev/tty12

# 如果root或zjk用户登入，则把紧急消息通知给这些用户
*.alert root,zjk

# 把所有的信息发送给127.0.0.1主机
*.* @127.0.0.1
```

## logrotate 日志轮转

- 用于对系统日志进行轮转、压缩和删除，也可以将日志发送到指定邮箱。
- 每个记录文件都可被设置成每日，每周或每月处理，也能在文件太大时立即处理。
- 必须自行编辑，指定配置文件，预设的配置文件存放在 **/etc/logrotate.conf** 文件中。

![](c:/notebook/pictures/Snipaste_2022-11-29_20-05-50.png =600x)

```shell
[root@bogon ~]# vim /etc/logrotate.conf

# see "man logrotate" for details
# rotate log files weekly 每周轮转
weekly

# keep 4 weeks worth of backlogs 保存过去四周的文件
rotate 4

# create new (empty) log files after rotating old ones 轮转以后创建新的空白日志文件
create

# use date as a suffix of the rotated file 轮转的文件以日期结尾
dateext

# uncomment this if you want your log files compressed 如果需要对轮转后的日志压缩，去掉此行注释
#compress

# RPM packages drop log rotation information into this directory 其他配置存放的文件目录;即用户自定义的配置
include /etc/logrotate.d

# no packages own wtmp and btmp -- we'll rotate them here 一些系统日志的轮转规则
/var/log/wtmp {
    monthly
    create 0664 root utmp
        minsize 1M
    rotate 1
}

/var/log/btmp {
    missingok
    monthly
    create 0600 root utmp
    rotate 1
}

# system-specific logs may be also be configured here.
```

### 自定义配置轮转

**配置的文件主目录：/etc/logrotate.d**

**logrotate配置文件参数**

![](c:/notebook/pictures/Snipaste_2022-11-29_20-11-41.png =800x)

### 测试轮转

```shell
/user/sbin/logrotate -vf /etc/logrotate.conf
```

- 无error日志，则正常生成轮转配置文件。

### 查看每日执行的轮转

```shell
[root@bogon ~]# cat -n /etc/cron.daily/logrotate
     1  #!/bin/sh
     2
     3  /usr/sbin/logrotate -s /var/lib/logrotate/logrotate.status /etc/logrotate.conf
     4  EXITVALUE=$?
     5  if [ $EXITVALUE != 0 ]; then
     6      /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
     7  fi
     8  exit 0
```

# 用户和组

## 用户

| 类型     | 说明                                                                                              |
| :------- | :------------------------------------------------------------------------------------------------ |
| 超级用户 | root 或 UID为0 的用户                                                                              |
| 系统用户 | 正常运行时系统使用的用户，每个进程在系统中都有一个相应的属主。系统用户不能用来登录（bin、daemon、mail等） |
| 普通用户 | user                                                                                              |

### 用户管理机制

| 文件          | 说明         |
| :----------- | :---------- |
| /etc/passswd | 用户账号文件 |
| /etc/shadow  | 用户密码文件 |
| /etc/group   | 用户组文件   |

#### /etc/passwd

![](c:/notebook/pictures/Snipaste_2022-11-30_13-43-52.png =500x)

![](c:/notebook/pictures/Snipaste_2022-11-30_14-12-25.png =800x)

- 普通用户的UID默认1000以上的数字

#### /etc/shadow

![](c:/notebook/pictures/Snipaste_2022-11-30_16-21-03.png =800x)

![](c:/notebook/pictures/Snipaste_2022-11-30_16-35-15.png =1000x)

#### /etc/group

- 用户登录时默认的组放在/etc/passwd中

![](c:/notebook/pictures/Snipaste_2022-11-30_18-46-41.png =200x)

![](c:/notebook/pictures/Snipaste_2022-11-30_18-52-02.png =280x)

### 用户管理命令

#### useradd 添加用户

![](c:/notebook/pictures/Snipaste_2022-11-30_18-55-07.png =400x)

```shell
[root@bogon ~]# useradd user1
[root@bogon ~]# cat /etc/passwd | grep user1
user1:x:1001:1001::/home/user1:/bin/bash
[root@bogon ~]# cat /etc/shadow | grep user1
user1:!!:19326:0:99999:7:::
[root@bogon ~]# cat /etc/group | grep user1
user1:x:1001:
```

#### usermod 修改用户

- usermod 命令不允许你改变正在线上的使用者帐号名称。当 usermod 命令用来改变user id，必须确认这名user没在电脑上执行任何程序。你需手动更改使用者的 crontab 档。也需手动更改使用者的 at 工作档。采用 NIS server 须在server上更动相关的NIS设定。

![](c:/notebook/pictures/Snipaste_2022-11-30_19-01-28.png =400x)

```shell
[root@bogon ~]# usermod -l newuser user1 
[root@bogon ~]# cat /etc/passwd | grep newuser
newuser:x:1001:1001::/home/user1:/bin/bash
[root@bogon ~]# cat /etc/passwd | grep user1
newuser:x:1001:1001::/home/user1:/bin/bash
```

#### userdel 删除用户

![](c:/notebook/pictures/Snipaste_2022-11-30_19-04-45.png =300x)

- 不加`-r`选项的userdel只是对用户删除，而不包含该用户的相关任务和文件。

```shell
[root@bogon ~]# userdel -r newuser
```

#### passwd 用户密码

- 用于设置用户的认证信息，包括用户密码、密码过期时间等。
- 系统管理者则能用它管理系统用户的密码。只有管理者可以指定用户名称，一般用户只能变更自己的密码。

![](C:notebook/pictures/Snipaste_2022-11-30_19-08-46.png =350x)

```shell
[root@bogon ~]# passwd
Changing password for user root.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@bogon ~]# passwd zjk
Changing password for user zjk.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
```

#### su 切换用户

![](c:/notebook/pictures/Snipaste_2022-11-30_19-13-25.png =700x)

**常用**

| 组合            | 说明                                          |
| :-------------- | :------------------------------------------- |
| su 用户         | 切换用户，不改变环境变量（不指定用户则默认root） |
| su - 用户       | 切换用户，改变环境变量（不指定用户则默认root）   |
| su -c 命令 用户 | 切换用户执行命令之后，回原来用户                |

```shell
[root@bogon ~]# su
[root@bogon ~]# su zjk
[zjk@bogon root]$ pwd
/root
[root@bogon ~]# su - zjk
Last login: Wed Nov 30 19:16:03 CST 2022 on pts/0
[zjk@bogon ~]$ pwd
/home/zjk
[zjk@bogon ~]$ su -c ls root
Password: 
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

#### sudo 普通用户获取超级权限

- 用来以其他身份来执行命令，预设的身份为root。
- 在**/etc/sudoers**中设置了可执行sudo指令的用户。若其未经授权的用户企图使用sudo，则会发出警告的邮件给管理员。
- 用户使用sudo时，必须先输入密码，之后有5分钟的有效期限，超过期限则必须重新输入密码。

![](c:/notebook/pictures/Snipaste_2022-11-30_19-21-53.png =600x)

```shell
[zjk@bogon ~]$ fdisk -l
fdisk: cannot open /dev/sda: Permission denied
fdisk: cannot open /dev/sr0: Permission denied
fdisk: cannot open /dev/mapper/rhel-root: Permission denied
fdisk: cannot open /dev/mapper/rhel-swap: Permission denied
[zjk@bogon ~]$ sudo fdisk -l

Disk /dev/sda: 53.7 GB, 53687091200 bytes, 104857600 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x0009ca0e

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200   104857599    51379200   8e  Linux LVM

Disk /dev/mapper/rhel-root: 47.2 GB, 47240445952 bytes, 92266496 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/rhel-swap: 5368 MB, 5368709120 bytes, 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

#### id 查看用户信息

- 可以显示真实有效的用户ID(UID)和组ID(GID)。
- id命令已经默认预装在大多数Linux系统中。

![](c:/notebook/pictures/Snipaste_2022-11-30_19-44-47.png =400x)

```shell
[root@bogon ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[root@bogon ~]# id root
uid=0(root) gid=0(root) groups=0(root)
```

## 用户组

- 每个用户都有一个用户组，一个用户可以属于多个用户组，一个用户组可以有多个用户。
- 系统对一个用户组的用户集中管理，赋予用户组的权限可以被该用户获取。用户的权限为所在的所有用户组的权限之和。
- /etc/passwd内定义的用户组为基本组，其他的为附加组。
- /etc/group文件的更新实现对用户组的操作

### 用户组管理命令

#### groupadd 添加用户组

![](c:/notebook/pictures/Snipaste_2022-11-30_19-33-36.png =400x)

```shell
[root@bogon ~]# groupadd group1 -p tiger
[root@bogon ~]# cat /etc/group | grep group1
group1:x:1002:
```

#### groupdel 删除用户组

- 命令要修改的系统文件包括/ect/group和/ect/shadow。
- 若该群组中仍包括某些用户，则必须先删除这些用户后，方能删除群组。

![](c:/notebook/pictures/Snipaste_2022-11-30_19-35-45.png =200x)

```shell
[root@bogon ~]# groupadd group1 -p tiger
[root@bogon ~]# useradd user1 -g group1
[root@bogon ~]# groupdel group1
groupdel: cannot remove the primary group of user 'user1'
[root@bogon ~]# userdel -r user1
[root@bogon ~]# groupdel group1
```

#### groupmod 修改用户组

![](c:/notebook/pictures/Snipaste_2022-11-30_19-39-45.png =300x)

```shell
[root@bogon ~]# groupadd group0
[root@bogon ~]# groupmod -g 1033 group0
[root@bogon ~]# groupmod -n group2 group0
[root@bogon ~]# cat /etc/group | grep group2
group2:x:1033:
[root@bogon ~]# cat /etc/group | grep group0
[root@bogon ~]# groupdel group2
```

#### groups 查看用户所在用户组

```shell
[root@bogon ~]# groups zjk
zjk : zjk wheel
[root@bogon ~]# groups root
root : root
```

# 应用程序管理

## rpm RPM包

![](c:/notebook/pictures/Snipaste_2022-11-30_19-47-03.png =700x)

**常用**

| 组合                            | 说明                                                   |
| :------------------------------ | :---------------------------------------------------- |
| rpm -ivh RPM包                  | 安装时显示信息                                         |
| rpm -ivh --force RPM包          | 强制安装                                               |
| rpm -ivh --nodeps --force RPM包 | 无视依赖，强制安装                                      |
| rpm -qpl RPM包                  | 查看软件包的详细信息                                    |
| rpm -Uvh RPM包                  | 更新已安装的软件包                                      |
| rpm -qa                         | 查看已安装的软件包                                      |
| rpm -e                          | 卸载软件包，如果存在依赖关系，需要先卸载需要其依赖的软件包 |
| rpm -qf                         | 查看文件所属软件包                                      |
| rpm -qip                        | 获取说明信息                                           |

```shell
# 1.挂载光盘
[root@bogon Packages]# mkdir -p /cdrom
[root@bogon Packages]# mount -t iso9660 /dev/cdrom /cdrom

# 2.安装 需要依赖关系时
[root@bogon Packages]# rpm -ivh glibc-devel-2.17-196.el7.x86_64.rpm
warning: glibc-devel-2.17-196.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
error: Failed dependencies:
        glibc-headers is needed by glibc-devel-2.17-196.el7.x86_64
        glibc-headers = 2.17-196.el7 is needed by glibc-devel-2.17-196.el7.x86_64
[root@bogon Packages]# rpm -ivh glibc-devel-2.17-196.el7.x86_64.rpm glibc-headers-2.17-196.e17.x86_64.rpm

# 3.先安装依赖关系
[root@bogon Packages]# rpm -ivh glibc-headers-2.17-196.el7.x86_64.rpm
warning: glibc-headers-2.17-196.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
error: Failed dependencies:
        kernel-headers is needed by glibc-headers-2.17-196.el7.x86_64
        kernel-headers >= 2.2.1 is needed by glibc-headers-2.17-196.el7.x86_64
[root@bogon Packages]# rpm -ivh kernel-headers-3.10.0-693.el7.x86_64.rpm
warning: kernel-headers-3.10.0-693.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:kernel-headers-3.10.0-693.el7    ################################# [100%]
[root@bogon Packages]# rpm -ivh kernel-headers-3.10.0-693.el7.x86_64.rpm
warning: kernel-headers-3.10.0-693.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:kernel-headers-3.10.0-693.el7    ################################# [100%]
[root@bogon Packages]# rpm -ivh glibc-headers-2.17-196.el7.x86_64.rpm
warning: glibc-headers-2.17-196.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:glibc-headers-2.17-196.el7       ################################# [100%]
[root@bogon Packages]# rpm -ivh glibc-devel-2.17-196.el7.x86_64.rpm
warning: glibc-devel-2.17-196.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:glibc-devel-2.17-196.el7         ################################# [100%]
# 4.检查
```

```shell
[root@bogon Packages]# rpm -ivh ftp-0.17-67.el7.x86_64.rpm
warning: ftp-0.17-67.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:ftp-0.17-67.el7                  ################################# [100%]
[root@bogon Packages]# rpm -ivh --force ftp-0.17-67.el7.x86_64.rpm
warning: ftp-0.17-67.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:ftp-0.17-67.el7                  ################################# [100%]
[root@bogon Packages]# rpm -ivh --nodeps --force ftp-0.17-67.el7.x86_64.rpm
warning: ftp-0.17-67.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:ftp-0.17-67.el7                  ################################# [100%]
[root@bogon Packages]# rpm -Uvh ftp-0.17-67.el7.x86_64.rpm
warning: ftp-0.17-67.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
```

```shell
[root@bogon Packages]# rpm -e glibc-headers
error: Failed dependencies:
        glibc-headers = 2.17-196.el7 is needed by (installed) glibc-devel-2.17-196.el7.x86_64
```

```shell
[root@bogon Packages]# rpm -qip glibc-devel-2.17-196.el7.x86_64.rpm
warning: glibc-devel-2.17-196.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Name        : glibc-devel
Version     : 2.17
Release     : 196.el7
Architecture: x86_64
Install Date: (not installed)
Group       : Development/Libraries
Size        : 1066324
License     : LGPLv2+ and LGPLv2+ with exceptions and GPLv2+
Signature   : RSA/SHA256, Sat 17 Jun 2017 03:48:44 AM CST, Key ID 199e2f91fd431d51
Source RPM  : glibc-2.17-196.el7.src.rpm
Build Date  : Sat 17 Jun 2017 12:59:25 AM CST
Build Host  : x86-017.build.eng.bos.redhat.com
Relocations : (not relocatable)
Packager    : Red Hat, Inc. <http://bugzilla.redhat.com/bugzilla>
Vendor      : Red Hat, Inc.
URL         : http://www.gnu.org/software/glibc/
Summary     : Object files for development using standard C libraries.
Description :
The glibc-devel package contains the object files necessary
for developing programs which use the standard C libraries (which are
used by nearly all programs).  If you are developing programs which
will use the standard C libraries, your system needs to have these
standard object files available in order to create the
executables.

Install glibc-devel if you are going to develop programs which will
use the standard C libraries.
```

### 需要的依赖问题

```shell
[root@localhost Packages]# rpm -ivh unbound-1.4.20-34.el7.x86_64.rpm
warning: unbound-1.4.20-34.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
error: Failed dependencies:
        libunbound.so.2()(64bit) is needed by unbound-1.4.20-34.el7.x86_64
        # 需要unbound库的2个64位的rpm包
        # lib库名.so.需要的包的数量()(位数x86_64或.i686)
        unbound-libs(x86-64) = 1.4.20-34.el7 is needed by unbound-1.4.20-34.el7.x86_64
        # 需要unbound-libs(x86-64)包
```

## 源码包

- Linux 系统中用于保存源代码的位置主要有 2 个，分别是 "/usr/src" 和 "/usr/local/src"，其中 "/usr/src" 用来保存内核源代码，"/usr/local/src" 用来保存用户下载的源代码。

**流程**

1. 下载源码包
2. tar 解压缩 
3. cd 进入解压目录
4. ./configure 软件配置与检查 
5. make 编译
6. make install 正式开始安装软件 

### 安装

#### ./configure

- configure 是源码包软件自带的一个脚本程序，必须采用 ./configure 方式执行。
- configure 通过系统环境的检测结果和定义好的功能选项生成编译规则文件 Makefile ，后续的编译和安装需要依赖这个文件的内容。

**常用**

| 命令                          | 说明            |
| :---------------------------- | :-------------- |
| ./configure --prefix=安装路径 | 指定安装路径。   |
| ./configure --help            | 查询其支持的功能 |

#### make 

- make根据Makefile文件生成目标文件，如二级制程序等。

#### make install

### 测试

#### vim源码安装

**1.检查并卸载vim**

```shell
[root@localhost ~]# vim --version
     
[root@localhost ~]# which vim
/usr/bin/vim
[root@localhost ~]# rpm -qf /usr/bin/vim
vim-enhanced-7.4.160-2.el7.x86_64
[root@localhost ~]# rpm -e vim-enhanced
[root@bogon ~]# which vim
/usr/bin/which: no vim in (/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin)
```

**2. 使用wget工具下载Vim http://www.vim.org**

- vim下载查看地址：`http://ftp.vim.org/pub/vim/unix/`
- vim9.0 ftp: `ftp://ftp.vim.org/pub/vim/unix/vim-9.0.tar.bz2`

- 先到安装目录下

```shell
[root@localhost ~]# mkdir soft
[root@localhost ~]# cd ./soft
[root@bogon soft]# wget ftp://ftp.vim.org/pub/vim/unix/vim-7.4.tar.bz2
```

**3. 解压缩**

```shell
[root@localhost soft]# ll
[root@localhost soft]# tar xvf vim-7.4.tar.bz2
```

**4. ./configure**

- 需要进入软件安装目录内部

```shell
[root@localhost soft]# cd vi74
[root@localhost vim74]# ./configure
configure: creating cache auto/config.cache
checking whether make sets $(MAKE)... yes
checking for gcc... no
checking for cc... no
checking for cl.exe... no
configure: error: in `/root/soft/vim74/src':
configure: error: no acceptable C compiler found in $PATH
See `config.log' for more details.
```

- 进行补充，gcc等配置

```shell
[root@localhost Packages]# ll | grep gcc
-r--r--r--.  953 root root 10529884 Apr  1  2014 compat-gcc-44-4.4.7-8.el7.x86_64.rpm
-r--r--r--.  953 root root  6636564 Apr  1  2014 compat-gcc-44-c++-4.4.7-8.el7.x86_64.rpm
-r--r--r--.  203 root root 16957744 Jun  6  2017 gcc-4.8.5-16.el7.x86_64.rpm
-r--r--r--.  203 root root  7525624 Jun  6  2017 gcc-c++-4.8.5-16.el7.x86_64.rpm
-r--r--r--.  203 root root  6977048 Jun  6  2017 gcc-gfortran-4.8.5-16.el7.x86_64.rpm
-r--r--r--.  203 root root 13562880 Jun  6  2017 gcc-gnat-4.8.5-16.el7.x86_64.rpm
-r--r--r--.  203 root root  6020924 Jun  6  2017 gcc-objc-4.8.5-16.el7.x86_64.rpm
-r--r--r--.  203 root root  6444876 Jun  6  2017 gcc-objc++-4.8.5-16.el7.x86_64.rpm
-r--r--r--.  203 root root   108440 Jun  6  2017 libgcc-4.8.5-16.el7.i686.rpm
-r--r--r--.  212 root root   100552 Jun  6  2017 libgcc-4.8.5-16.el7.x86_64.rpm
[root@localhost Packages]# rpm -ivh gcc-4.8.5-16.el7.x86_64.rpm
warning: gcc-4.8.5-16.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
error: Failed dependencies:
        cpp = 4.8.5-16.el7 is needed by gcc-4.8.5-16.el7.x86_64
        libmpc.so.3()(64bit) is needed by gcc-4.8.5-16.el7.x86_64
[root@localhost Packages]# ll | grep cpp
-r--r--r--.  271 root root   195708 May 30  2017 abrt-addon-ccpp-2.1.11-48.el7.x86_64.rpm
-r--r--r--.  203 root root  6230984 Jun  6  2017 cpp-4.8.5-16.el7.x86_64.rpm
[root@localhost Packages]# ll | grep libmpc.so
[root@localhost Packages]# ll | grep libmpc
-r--r--r--. 1642 root root    52120 Apr  2  2014 libmpc-1.0.1-3.el7.i686.rpm
-r--r--r--. 1646 root root    51788 Apr  2  2014 libmpc-1.0.1-3.el7.x86_64.rpm
-r--r--r--. 1637 root root    29364 Apr  2  2014 libmpcdec-1.2.6-12.el7.i686.rpm
-r--r--r--. 1637 root root    28992 Apr  2  2014 libmpcdec-1.2.6-12.el7.x86_64.rpm
[root@localhost Packages]# rpm -ivh libmpc-1.0.1-3.el7.x86_64.rpm
warning: libmpc-1.0.1-3.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:libmpc-1.0.1-3.el7               ################################# [100%]
[root@localhost Packages]# rpm -ivh cpp-4.8.5-16.el7.x86_64.rpm
warning: cpp-4.8.5-16.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:cpp-4.8.5-16.el7                 ################################# [100%]
[root@localhost Packages]# rpm -ivh gcc-4.8.5-16.el7.x86_64.rpm
warning: gcc-4.8.5-16.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:gcc-4.8.5-16.el7                 ################################# [100%]
```

- 安装 ncurses库

- 继续完成配置

```shell
[root@localhost Packages]# cd /root/soft/vim74
[root@localhost vim74]# ./configure
# ...略
no terminal library found
checking for tgetent()... configure: error: NOT FOUND!
      You need to install a terminal library; for example ncurses.
      Or specify the name of the library with --with-tlib.
[root@localhost vim74]# echo $?
0
```

- 安装依赖包

```shell
[root@localhost vim74]# cd /cdrom/Packages
[root@localhost Packages]# ll | grep ncurses-devel
-r--r--r--. 1633 root root   729968 Apr  3  2014 ncurses-devel-5.9-13.20130511.el7.i686.rpm
-r--r--r--. 1633 root root   729972 Apr  3  2014 ncurses-devel-5.9-13.20130511.el7.x86_64.rpm
[root@localhost Packages]# rpm -ivh ncurses-devel-5.9-13.20130511.el7.x86_64.rpm
warning: ncurses-devel-5.9-13.20130511.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:ncurses-devel-5.9-13.20130511.el7################################# [100%]
```

- 再次继续配置

```shell
[root@localhost Packages]# cd ~/soft/vim74
[root@localhost vim74]# ./configure --prefix=/usr/local/vim74 | head
configure: loading cache auto/config.cache
checking whether make sets $(MAKE)... yes
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
```

**5. make进行编译**

```shell
[root@localhost vim74]# make
# 略...
```

**6. make install 继续安装**

```shell
[root@localhost vim74]# make install
# 略...

# 此时还未添加到环境，不能使用
[root@localhost vim74]# vim
-bash: /usr/bin/vim: No such file or directory
```

**7. PATH 添加到环境变量**

```shell
[root@localhost vim74]# /usr/local/vim74/bin/vim /etc/profile
# 或 vi /etc/profile
# 添加以下到末尾
PATH=$PATH:/usr/local/vim74/bin/
export PATH
# 结束
[root@localhost vim74]# source /etc/profile
[root@localhost vim74]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/usr/local/vim74/bin/

# 至此 安装成功
```

##### 静默安装

- RHEL74

```shell
# 删除vim
rpm -e vim-enhanced
# 下载
cd /root
mkdir soft
cd ./soft
wget ftp://ftp.vim.org/pub/vim/unix/vim-9.0.tar.bz2
# 解压
tar -xvf vim-9.0.tar.bz2
# 安装依赖
# 1. 挂载光盘
cd ~
mkdir -p /room
mount -t iso9660 /dev/cdrom /room
# 2. 安装rpm包
cd /room/Packages
rpm -ivh libmpc-1.0.1-3.el7.x86_64.rpm
rpm -ivh cpp-4.8.5-16.el7.x86_64.rpm
rpm -ivh kernel-headers-3.10.0-693.el7.x86_64.rpm
rpm -ivh glibc-headers-2.17-196.el7.x86_64.rpm
rpm -ivh glibc-devel-2.17-196.el7.x86_64.rpm
rpm -ivh gcc-4.8.5-16.el7.x86_64.rpm
# 3. 安装ncurses库
# yum的方式 yum install ncurses ncurses-devel

# 源码安装 ncurses
# 下载路径 https://invisible-island.net/ncurses/ncurses.html#download_ncurses
wget https://invisible-island.net/datafiles/release/ncurses.tar.gz
tar -vxf ncurses.tar.gz
cd /soft/ncurses-6.3
./configure
make
make install

# 进入/root/soft/vim90 ./configure 配置软件
cd /root/soft/vim90
./configure
make
make install

# 结束
```

#### nginx安装

- 下载路径 `https://nginx.org/en/download.html`

```shell
mkdir /root/soft
cd /root/soft
wget https://nginx.org/download/nginx-1.22.1.tar.gz
tar -xvf nginx-1.22.1.tar.gz
cd /root/soft/nginx-1.22.1

# gcc配置 在vim安装时已经配置

# pcre模块 
cd /room/Packages
rpm -ivh pcre-devel-8.32-17.el7.x86_64.rpm
rpm -ivh pcre-8.32-17.el7.x86_64.rpm

# zlib开发包
cd /room/Packages
rpm -ivh zlib-devel-1.2.7-17.el7.x86_64.rpm
rpm -ivh zlib-1.2.7-17.el7.x86_64.rpm

cd /root/soft/nginx-1.22.1
./configure
make
make install
```

```shell
[root@bogon ~]# cd /usr/local/nginx/sbin
[root@bogon sbin]# ./nginx
[root@bogon sbin]# netstat -plnt | grep 80
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      50771/nginx: master 
[root@bogon sbin]# echo "welcome" > /usr/local/nginx/html/index.html
[root@bogon sbin]# curl http://127.0.0.1
welcome
```

### Linux源码包卸载

- 通过源码包方式安装的各个软件，其安装文件独自保存在 /usr/local/ 目录下的各子目录中
- 例如，apache 所有的安装文件都保存在 /usr/local/apache2 目录下。这就为源码包的卸载提供了便利。
- 源码包的卸载，只需要找到软件的安装位置，直接删除所在目录即可，不会遗留任何垃圾文件。
- 在删除软件之前，应先将软件停止服务。

- 以删除 apache 为例，只需关闭 apache 服务后执行如下命令即可：

```shell
[root@localhost ~]# rm -rf /usr/local/apache2/
```

## Linux函数库

- 函数库是一个文件，包含已经编译好的代码和数据，供程序使用。

**静态函数库** 
                                       
- 在编译程序时，如果指定静态函数库文件，则编译时将这些静态函数库一起加载到最终的可执行文件中，在程序执行前加入目标程序。
- 文件后缀 .a
- 所在目录 /usr/lib  
- 通常只有一个程序使用 

**共享函数库**

- 在程序启动时加载到程序，可以被不同的程序共享
- 当一个可执行程序启动时被加载
- 所在目录 /lib 和 /usr/lib 
- 被多个程序使用

**动态加载函数库**

- 可以在程序运行的任何时候动态的加载
- 文件后缀 .so
- 一般都是共享函数库 

### 寻找函数库

#### 1. 缓存文件 /etc/ld.so.cache**

- 从缓存文件 /etc/ld.so.cache中可以找到相关的库文件信息

```shell
# 修改 /etc/ld.so.conf 文件，将该库的文件路径添加到文件
echo "/usr/local/ssl/lid/" >> /etc/ld.so.conf

# 让系统升级ld.so.cache文件
ldconfig
```

#### LD_LIBRARY_PATH 环境变量

```shell
export LD_LIBRARY_PATH=/usr/local/ssl/lib:$LD_LIBRARY_PATH:.
```

### ldd 查看程序使用了哪些动态库文件

## yum



# 网络管理

## 网络管理命令

### ip 网络配置工具

![](c:/notebook/pictures/Snipaste_2022-12-12_00-40-46.png =500x)

### ping

- 用来测试主机之间网络的连通性。执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。

![](c:/notebook/pictures/Snipaste_2022-12-04_16-47-45.png =400x)

**常用**

```shell
ping 域名/IP地址
ping -c 指定次数 域名/IP # 指定次数
ping -c 指定次数 -i 间隔秒数 域名/IP # 指定次数和每次间隔时间
```

![](c:/notebook/pictures/Snipaste_2022-12-04_16-57-38.png =800x)

### ifconfig 配置和显示Linux系统网卡的网络参数

- 用ifconfig命令配置的网卡信息，在网卡重启后机器重启后，配置就不存在。要想将上述的配置信息永远保存电脑，要修改网卡的配置文件。

![](c:/notebook/pictures/Snipaste_2022-12-04_16-59-24.png =500x)

![](c:/notebook/pictures/Snipaste_2022-12-04_17-23-14.png =1000x)

**常用**

```shell
# 显示信息
ifconfig   #处于激活状态的网络接口
ifconfig -a  #所有配置的网络接口，不论其是否激活
ifconfig eth0  #显示eth0的网卡信息
# 启动关闭网卡eth0
ifconfig eth0 up
ifconfig eth0 down
# 为网卡eth0配置和删除IPv6地址：
ifconfig eth0 add 33ffe:3240:800:1005::2/64  #为网卡eth0配置IPv6地址
ifconfig eth0 del 33ffe:3240:800:1005::2/64  #为网卡eth0删除IPv6地址
# 用ifconfig修改eth0网卡MAC地址：
ifconfig eth0 hw ether 00:AA:BB:CC:dd:EE
# 配置网卡eth0的IP地址：
ifconfig eth0 192.168.2.10
ifconfig eth0 192.168.2.10 netmask 255.255.255.0
ifconfig eth0 192.168.2.10 netmask 255.255.255.0 broadcast 192.168.2.255
# 启用和关闭arp协议：
ifconfig eth0 arp    #开启网卡eth0 的arp协议
ifconfig eth0 -arp   #关闭网卡eth0 的arp协议
# 设置最大传输单元：
ifconfig eth0 mtu 1500    #设置能通过的最大数据包大小为 1500 bytes
```

### ifdown 禁用指定的网络接口

### route 显示添加/修改路由器

- 用来显示并设置Linux内核中的网络路由表，route命令设置的路由主要是静态路由。要实现两个不同的子网之间的通信，需要一台连接两个网络的路由器，或者同时位于两个网络的网关来实现。
- 在Linux系统中设置路由通常是为了解决以下问题：该Linux系统在一个局域网中，局域网中有一个网关，能够让机器访问Internet，那么就需要将这台机器的ip地址设置为Linux机器的默认路由。
- 要注意的是，直接在命令行下执行route命令来添加路由，不会永久保存，当网卡重启或者机器重启之后，该路由就失效了；可以在**/etc/rc.local**中添加route命令来保证该路由设置永久有效。

![](c:/notebook/pictures/Snipaste_2022-12-05_14-39-35.png =300x)

![](c:/notebook/pictures/Snipaste_2022-12-11_23-07-07.png =750x)

```shell
# 显示
route # 显示当前路由器
route -n # 显示所有路由器
# 添加网关/设置网关： route add -net IP地址 netmask 子网掩码 gw 网关 dev 网络接口
route add -net 224.0.0.0 netmask 240.0.0.0 dev ens33 #增加一条到达244.0.0.0的路由。
# 删除路由记录: 不会删除被屏蔽的路由（需要reject选项）
route del -net 224.0.0.0 netmask 240.0.0.0 #删除指定IP和子网掩码的路由
# 删除和添加设置默认网关：
route del default gw 192.168.120.240
route add default gw 192.168.120.240
# 删除和添加屏蔽的路由：
route add -net 224.0.0.0 netmask 240.0.0.0 reject #增加屏蔽的路由，目的地址为224.x.x.x将被拒
route add -net 224.0.0.0 netmask 240.0.0.0 reject #删除屏蔽的路由
```

### scp 加密的方式在本地主机和远程主机之间复制文件

- 用于在Linux下进行远程拷贝文件的命令，和它类似的命令有cp，不过cp只是在本机进行拷贝不能跨服务器，而且scp传输是加密的，可能会稍微影响速度。当服务器硬盘变为只读read only system时，用scp可以把文件移出来。另外，scp还非常不占资源，不会提高多少系统负荷，在这一点上，rsync就远远不及它了。虽然 rsync比scp会快一点，但当小文件众多的情况下，rsync会导致硬盘I/O非常高，而scp基本不影响系统正常使用。

![](c:/notebook/pictures/Snipaste_2022-12-05_15-01-17.png =500x)

```shell
# scp 源文件 目标文件

# scp 远程主机用户@远程主机IP地址:/要下载的完整文件路径 /下载到本机的文件路径
#从远程主机复制文件到本地 
scp root@192.168.186.137:/root/install/test.txt  /root/install/
#从远程主机复制文件目录到本地
scp -r zjk@192.168.186.137:/home/zjk/Downloads /root/install/

# scp /本地文件路径 远程主机用户@远程主机IP地址:/要上传到的文件路径
#上传本地文件目录到远程机器指定目录
scp -r /root/install zjk@192.168.186.137:/home/zjk/Downloads
#指定端口 即使用ssh端口默认22
scp -rp -P 22 /root/install/test.txt zjk@192.168.186.137:/home/zjk/Downloads
```

```shell
[root@bogon install]# scp root@192.168.186.137:/root/install/test.txt  /root/install/
The authenticity of host '192.168.186.137 (192.168.186.137)' can't be established.
ECDSA key fingerprint is SHA256:pqc8Zaq6EU1PrS4M/FGOkfQ3T0gvP+4dwglHsrq0r4g.
ECDSA key fingerprint is MD5:8c:cf:1e:3c:c9:aa:4b:32:32:5d:e6:8c:19:e2:cc:37.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.186.137' (ECDSA) to the list of known hosts.
root@192.168.186.137's password: 
test.txt                                                                                 100%    5     2.1KB/s   00:00    
[root@bogon install]# ll
total 12
drwxr-xr-x. 18 oracle wheel 4096 Dec  4 16:39 ncurses-6.3
-rw-r--r--.  1 root   root     5 Dec  5 15:12 test.txt
drwxrwxr-x.  9 zjk    zjk   4096 Jun 28 18:40 vim90

[root@bogon install]# scp -r zjk@192.168.186.137:/home/zjk/Downloads /root/install/
zjk@192.168.186.137's password: 
hello.txt                                                                                100%    6     1.8KB/s   00:00    
[root@bogon install]# ll
total 12
drwxr-xr-x.  2 root   root    23 Dec  5 15:22 Downloads
drwxr-xr-x. 18 oracle wheel 4096 Dec  4 16:39 ncurses-6.3
-rw-r--r--.  1 root   root     5 Dec  5 15:12 test.txt
drwxrwxr-x.  9 zjk    zjk   4096 Jun 28 18:40 vim90
```

#### Linux 与 Windows的传输

- 需要在Windos安装SSH功能。**OPENSSH服务器和OPENSSH客户端**
![](c:/notebook/pictures/Snipaste_2022-12-05_15-40-47.png =500x)
- 需要启动SSH服务

##### Windows到Linux

**Windows需要SSH服务器，启动相应的服务**

```shell
[root@bogon ~]# scp -r zjk10@10.22.114.22:/g:/SCPtest /root/install/
zjk10@10.22.114.22's password: 
test.txt                                                                                 100%    0     0.0KB/s   00:00   
```

##### Linux到Windows

**Windows需要SSH客户端，启动相应的服务OpenSSH Authentication Agent**

```shell
C:\Users\zjk10>scp -r root@192.168.186.154:/root/install /G:/test

The authenticity of host '192.168.186.154 (192.168.186.154)' can't be established.
ED25519 key fingerprint is SHA256:gPlDhf9CboGI3JUnxNVrltOGAjUYX9GJyypCQulMDb0.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])?
Warning: Permanently added '192.168.186.154' (ED25519) to the list of known hosts.
root@192.168.186.154's password:
```

### rsync 远程数据同步工具

- rsync命令 是一个远程数据同步工具，可通过LAN/WAN快速同步多台主机间的文件。rsync使用所谓的“rsync算法”来使本地和远程两个主机之间的文件达到同步，这个算法只传送两个文件的不同部分，而不是每次都整份传送，因此速度相当快。 

![](c:/notebook/pictures/Snipaste_2022-12-05_15-33-10.png =900x)
![](c:/notebook/pictures/Snipaste_2022-12-05_15-34-06.png =900x)
![](c:/notebook/pictures/Snipaste_2022-12-05_15-34-52.png =500x)

```shell
# 传送本地到远程主机

```

### netstat 查看网络系统状态信息

![](c:/notebook/pictures/Snipaste_2022-12-05_19-32-19.png =500x)

```shell
# 列出所有端口 (包括监听和未监听的)
netstat -a     #列出所有端口
netstat -at    #列出所有tcp端口
netstat -au    #列出所有udp端口                             
# 列出所有处于监听状态的 Sockets
netstat -l        #只显示监听端口
netstat -lt       #只列出所有监听 tcp 端口
netstat -lu       #只列出所有监听 udp 端口
netstat -lx       #只列出所有监听 UNIX 端口
# 显示每个协议的统计信息
netstat -s    #显示所有端口的统计信息
netstat -st   #显示TCP端口的统计信息
netstat -su   #显示UDP端口的统计信息
# 在netstat输出中显示 PID 和进程名称
netstat -p
# 在netstat输出中不显示主机，端口和用户名(host, port or user)
#当不想让主机，端口和用户名显示，使用netstat -n。将会使用数字代替那些名称。同样可以加速输出，因为不用进行比对查询。
netstat -an
# 如果只是不想让这三个名称中的一个被显示，使用以下命令:
netstat -a --numeric-ports
netstat -a --numeric-hosts
netstat -a --numeric-users
# 持续输出netstat信息
netstat -c   #每隔一秒输出网络信息
# 显示系统不支持的地址族(Address Families)
netstat --verbose
# 显示核心路由信息
netstat -r
# 找出程序运行的端口
#并不是所有的进程都能找到，没有权限的会不显示，使用 root 权限查看所有的信息。
netstat -ap | grep ssh
# 找出运行在指定端口的进程：
netstat -an | grep ':80'
# 通过端口找进程ID
netstat -anp|grep 8081 | grep LISTEN|awk '{printf $7}'|cut -d/ -f1
# 显示网络接口列表
netstat -i

# IP和TCP分析
# 查看连接某服务端口最多的的IP地址：
netstat -ntu | grep :80 | awk '{print $5}' | cut -d: -f1 | awk '{++ip[$1]} END {for(i in ip) print ip[i],"\t",i}' | sort -nr
# TCP各种状态列表：
netstat -nt | grep -e 127.0.0.1 -e 0.0.0.0 -e ::: -v | awk '/^tcp/ {++state[$NF]} END {for(i in state) print i,"\t",state[i]}'
# 查看phpcgi进程数，如果接近预设值，说明不够用，需要增加：
netstat -anpo | grep "php-cgi" | wc -l
```

### traceroute 显示数据包到主机间的路径

- traceroute命令 用于追踪数据包在网络上的传输时的全部路径，它默认发送的数据包大小是40字节。
- 通过traceroute可以知道信息从计算机到互联网另一端的主机是走的什么路径。每次数据包由某一同样的出发点（source）到达某一同样的目的地(destination)走的路径可能会不一样，但大部分时候所走的路由是相同的。
- 记录按序列号从1开始，每个纪录就是一跳 ，每跳表示一个网关。最多有30跳(即最多可经过30个路由)，如果超过还未到达目标机，则终止traceroute命令。每行有三个时间（即-q的默认参数），单位是ms是探测数据包向每个网关发送三个数据包后，网关响应后返回的时间。每个数据包都对应一个Max_ttl值，同一跳步的数据包的该值相同。不同跳步的数据包的值从1开始，每一跳加1。当本地机发出的数据包到达路由器时，路由器就响应一个ICMPTimeExceed消息，于是traceroute命令就显示这一跳的消息。如果在指定的时间内未收到响应包（可能是防火墙封掉了ICMP的返回信息），则traceroute命令显示星号`*`，直到收到一个ICMPPORT_UNREACHABLE的消息表示已经到达目标机，或到达指定的最多跳步数。
- 有时我们在某一网关处延时比较长，有可能是某台网关比较阻塞，也可能是物理设备本身的原因。当然如果某台DNS出现问题时，不能解析主机名、域名时，也会有延时长的现象；您可以加-n参数来避免DNS解析，以IP格式输出数据。
- 如果在局域网中的不同网段之间，可以通过traceroute 来排查问题所在，是主机的问题还是网关的问题。如果通过远程来访问某台服务器遇到问题时，我们用到traceroute 追踪数据包所经过的网关，提交IDC服务商，也有助于解决问题。

![](c:/notebook/pictures/Snipaste_2022-12-05_20-14-21.png =400x)

```shell
traceroute 域名/IP地址
traceroute -m 10 www.baidu.com # 跳数设置 默认最大30跳
traceroute -n www.baidu.com    # 显示IP地址，不查主机名
traceroute -p 6888 www.baidu.com  # 探测包使用的基本UDP端口设置6888
traceroute -q 4 www.baidu.com  # 把探测包的个数设置为值4
traceroute -r www.baidu.com    # 绕过正常的路由表，直接发送到网络相连的主机
traceroute -w 3 www.baidu.com  # 把对外发探测包的等待响应时间设置为3秒
traceroute -I 10.22.114.22 # 强制使用ICMP
```

```shell
[root@bogon ~]# traceroute -I 10.22.114.22
traceroute to 10.22.114.22 (10.22.114.22), 30 hops max, 60 byte packets
 1  bogon (192.168.186.2)  0.100 ms  0.055 ms  0.034 ms
 2  bogon (10.22.114.22)  0.542 ms  0.529 ms  0.776 ms
```

### telnet 登录远程主机和管理(测试ip端口是否连通)

- telnet命令用于登录远程主机，对远程主机进行管理，可以用来确认远程服务端口的状态。
- telnet程序基于TELNET协议，TELNET协议是TCP/IP协议族中的一员，是Internet远程登录服务的标准和主要方式。
- telnet因为采用明文传送报文，安全性不好。

![](c:/notebook/pictures/Snipaste_2022-12-05_20-48-25.png =550x)

**需要安装该程序telnet**

```shell
yum list telnet*    # 列出telnet相关的安装包
yum install telnet-server    # 安装telnet服务
yum install telnet.*    # 安装telnet客户端
```

```shell
telnet www.php.net 80 # 以80端口连接www.php.net
```

### wget 下载网络文件

- wget命令 用来从指定的URL下载文件。wget非常稳定，它在带宽很窄的情况下和不稳定网络中有很强的适应性，如果是由于网络的原因下载失败，wget会不断的尝试，直到整个文件下载完毕。如果是服务器打断下载过程，它会再次联到服务器上从停止的地方继续下载。这对从那些限定了链接时间的服务器上下载大文件非常有用。
- wget支持HTTP，HTTPS和FTP协议，可以使用HTTP代理。所谓的自动下载是指，wget可以在用户退出系统的之后在后台执行。这意味这你可以登录系统，启动一个wget下载任务，然后退出系统，wget将在后台执行直到任务完成，相对于其它大部分浏览器在下载大量数据时需要用户一直的参与，这省去了极大的麻烦。
- 用于从网络上下载资源，没有指定目录，下载资源会默认为当前目录。
1. 支持断点下传功能
2. 同时支持FTP和HTTP下载方式
3. 支持代理服务器 对安全强度很高的系统而言，一般不会将自己的系统直接暴露在互联网上，所以，支持代理是下载软件必须有的功能；
4. 设置方便简单
5. 程序小，完全免费

```shell
wget [参数] [URL地址]
```

**选项**

```
启动参数：

-V, –-version 显示wget的版本后退出
-h, –-help 打印语法帮助
-b, –-background 启动后转入后台执行
-e, –-execute=COMMAND 执行`.wgetrc’格式的命令，wgetrc格式参见/etc/wgetrc或~/.wgetrc

记录和输入文件参数：

-o, –-output-file=FILE 把记录写到FILE文件中
-a, –-append-output=FILE 把记录追加到FILE文件中
-d, –-debug 打印调试输出
-q, –-quiet 安静模式(没有输出)
-v, –-verbose 冗长模式(这是缺省设置)
-nv, –-non-verbose 关掉冗长模式，但不是安静模式
-i, –-input-file=FILE 下载在FILE文件中出现的URLs
-F, –-force-html 把输入文件当作HTML格式文件对待
-B, –-base=URL 将URL作为在-F -i参数指定的文件中出现的相对链接的前缀
–-sslcertfile=FILE 可选客户端证书
–-sslcertkey=KEYFILE 可选客户端证书的KEYFILE
–-egd-file=FILE 指定EGD socket的文件名

下载参数：

–-bind-address=ADDRESS 指定本地使用地址(主机名或IP，当本地有多个IP或名字时使用)
-t, –-tries=NUMBER 设定最大尝试链接次数(0 表示无限制).
-O –-output-document=FILE 把文档写到FILE文件中
-nc, –-no-clobber 不要覆盖存在的文件或使用.#前缀
-c, –-continue 接着下载没下载完的文件
–progress=TYPE 设定进程条标记
-N, –-timestamping 不要重新下载文件除非比本地文件新
-S, –-server-response 打印服务器的回应
–-spider 不下载任何东西
-T, –-timeout=SECONDS 设定响应超时的秒数
-w, –-wait=SECONDS 两次尝试之间间隔SECONDS秒
–waitretry=SECONDS 在重新链接之间等待1…SECONDS秒
–random-wait 在下载之间等待0…2*WAIT秒
-Y, –-proxy=on/off 打开或关闭代理
-Q, –-quota=NUMBER 设置下载的容量限制
–limit-rate=RATE 限定下载输率

目录参数：

-nd –-no-directories 不创建目录
-x, –-force-directories 强制创建目录
-nH, –-no-host-directories 不创建主机目录
-P, –-directory-prefix=PREFIX 将文件保存到目录 PREFIX/…
–cut-dirs=NUMBER 忽略 NUMBER层远程目录

HTTP 选项参数：

-–http-user=USER 设定HTTP用户名为 USER.
-–http-passwd=PASS 设定http密码为 PASS
-C, –-cache=on/off 允许/不允许服务器端的数据缓存 (一般情况下允许)
-E, –-html-extension 将所有text/html文档以.html扩展名保存
-–ignore-length 忽略 `Content-Length’头域
-–header=STRING 在headers中插入字符串 STRING
-–proxy-user=USER 设定代理的用户名为 USER
-–proxy-passwd=PASS 设定代理的密码为 PASS
-–referer=URL 在HTTP请求中包含 `Referer: URL’头
-s, –-save-headers 保存HTTP头到文件
-U, –-user-agent=AGENT 设定代理的名称为 AGENT而不是 Wget/VERSION
-–no-http-keep-alive 关闭 HTTP活动链接 (永远链接)
–-cookies=off 不使用 cookies
–-load-cookies=FILE 在开始会话前从文件 FILE中加载cookie
-–save-cookies=FILE 在会话结束后将 cookies保存到 FILE文件中

FTP 选项参数：

-nr, -–dont-remove-listing 不移走 `.listing’文件
-g, -–glob=on/off 打开或关闭文件名的 globbing机制
-–passive-ftp 使用被动传输模式 (缺省值).
-–active-ftp 使用主动传输模式
-–retr-symlinks 在递归的时候，将链接指向文件(而不是目录)

递归下载参数：

-r, -–recursive 递归下载－－慎用!
-l, -–level=NUMBER 最大递归深度 (inf 或 0 代表无穷)
–-delete-after 在现在完毕后局部删除文件
-k, –-convert-links 转换非相对链接为相对链接
-K, –-backup-converted 在转换文件X之前，将之备份为 X.orig
-m, –-mirror 等价于 -r -N -l inf -nr
-p, –-page-requisites 下载显示HTML文件的所有图片

递归下载中的包含和不包含(accept/reject)：

-A, –-accept=LIST 分号分隔的被接受扩展名的列表
-R, –-reject=LIST 分号分隔的不被接受的扩展名的列表
-D, –-domains=LIST 分号分隔的被接受域的列表
–-exclude-domains=LIST 分号分隔的不被接受的域的列表
–-follow-ftp 跟踪HTML文档中的FTP链接
–-follow-tags=LIST 分号分隔的被跟踪的HTML标签的列表
-G, –-ignore-tags=LIST 分号分隔的被忽略的HTML标签的列表
-H, –-span-hosts 当递归时转到外部主机
-L, –-relative 仅仅跟踪相对链接
-I, –-include-directories=LIST 允许目录的列表
-X, –-exclude-directories=LIST 不被包含目录的列表
-np, –-no-parent 不要追溯到父目录
wget -S –-spider url 不下载只显示过程
```

```shell
# 下载某个文件
wget URL地址
# 断点续传
wget -c URL地址
# 批量下载 其中download.list是一系列网址(每行一个网址)，将依次下载
wget -i download.list

使用wget下载单个文件

wget http://www.jsdig.com/testfile.zip
以下的例子是从网络下载一个文件并保存在当前目录，在下载的过程中会显示进度条，包含（下载完成百分比，已经下载的字节，当前下载速度，剩余下载时间）。

下载并以不同的文件名保存

wget -O wordpress.zip http://www.jsdig.com/download.aspx?id=1080
wget默认会以最后一个符合/的后面的字符来命令，对于动态链接的下载通常文件名会不正确。

错误：下面的例子会下载一个文件并以名称download.aspx?id=1080保存:

wget http://www.jsdig.com/download?id=1
即使下载的文件是zip格式，它仍然以download.php?id=1080命令。

正确：为了解决这个问题，我们可以使用参数-O来指定一个文件名：

wget -O wordpress.zip http://www.jsdig.com/download.aspx?id=1080
wget限速下载

wget --limit-rate=300k http://www.jsdig.com/testfile.zip
当你执行wget的时候，它默认会占用全部可能的宽带下载。但是当你准备下载一个大文件，而你还需要下载其它文件时就有必要限速了。

使用wget断点续传

wget -c http://www.jsdig.com/testfile.zip
使用wget -c重新启动下载中断的文件，对于我们下载大文件时突然由于网络等原因中断非常有帮助，我们可以继续接着下载而不是重新下载一个文件。需要继续中断的下载时可以使用-c参数。

使用wget后台下载

wget -b http://www.jsdig.com/testfile.zip

Continuing in background, pid 1840.
Output will be written to `wget-log'.
对于下载非常大的文件的时候，我们可以使用参数-b进行后台下载，你可以使用以下命令来察看下载进度：

tail -f wget-log
伪装代理名称下载

wget --user-agent="Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.204 Safari/534.16" http://www.jsdig.com/testfile.zip
有些网站能通过根据判断代理名称不是浏览器而拒绝你的下载请求。不过你可以通过--user-agent参数伪装。

测试下载链接

当你打算进行定时下载，你应该在预定时间测试下载链接是否有效。我们可以增加--spider参数进行检查。

wget --spider URL
如果下载链接正确，将会显示:

Spider mode enabled. Check if remote file exists.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Remote file exists and could contain further links,
but recursion is disabled -- not retrieving.
这保证了下载能在预定的时间进行，但当你给错了一个链接，将会显示如下错误:

wget --spider url
Spider mode enabled. Check if remote file exists.
HTTP request sent, awaiting response... 404 Not Found
Remote file does not exist -- broken link!!!
你可以在以下几种情况下使用--spider参数：

定时下载之前进行检查
间隔检测网站是否可用
检查网站页面的死链接
增加重试次数

wget --tries=40 URL
如果网络有问题或下载一个大文件也有可能失败。wget默认重试20次连接下载文件。如果需要，你可以使用--tries增加重试次数。

下载多个文件

wget -i filelist.txt
首先，保存一份下载链接文件：

cat > filelist.txt
url1
url2
url3
url4
接着使用这个文件和参数-i下载。

镜像网站

wget --mirror -p --convert-links -P ./LOCAL URL
下载整个网站到本地。

--miror开户镜像下载。
-p下载所有为了html页面显示正常的文件。
--convert-links下载后，转换成本地的链接。
-P ./LOCAL保存所有文件和目录到本地指定目录。
过滤指定格式下载

wget --reject=gif ur
下载一个网站，但你不希望下载图片，可以使用这条命令。

把下载信息存入日志文件

wget -o download.log URL
不希望下载信息直接显示在终端而是在一个日志文件，可以使用。

限制总下载文件大小

wget -Q5m -i filelist.txt
当你想要下载的文件超过5M而退出下载，你可以使用。注意：这个参数对单个文件下载不起作用，只能递归下载时才有效。

下载指定格式文件

wget -r -A.pdf url
可以在以下情况使用该功能：

下载一个网站的所有图片。
下载一个网站的所有视频。
下载一个网站的所有PDF文件。
FTP下载

wget ftp-url
wget --ftp-user=USERNAME --ftp-password=PASSWORD url
可以使用wget来完成ftp链接的下载。

使用wget匿名ftp下载：

wget ftp-url
使用wget用户名和密码认证的ftp下载：

wget --ftp-user=USERNAME --ftp-password=PASSWORD url
```

## 网络配置

**相关文件**

| 文件                                    | 说明                                              |
| :------------------------------------- | :------------------------------------------------ |
| /etc/hostname                          | 修改主机名称                                       |
| /etc/sysconfig/network-scripts/ifcfg-* | 设置网卡参数，*为网卡编号/回环网卡                   |
| /etc/resolv.conf                       | 设置DNS的相关信息，用于将域名解析到IP                |
| /etc/hosts                             | 计算机IP对应的主机名称或域名对应的IP                 |
| /etc/nsswitch.conf                     | 名字服务切换配置（设置DNS解析优先还是本地设置优先等） |

- 通过对配置文件的修改，重启后依然有效

### 网络接口文件 /etc/sysconfig/network-scripts/ifcfg-* 

- 可以配置IP地址，修改之后需要重启网络服务才开始生效

```shell
# 查看配置文件
cat /etc/sysconfig/network-scripts/ifcfg-ens33
# 重启network服务后生效
systemctl restart network
```

![](c:/notebook/pictures/Snipaste_2022-12-06_23-25-20.png =700x)

#### 子接口

- 同一个网络接口可以设置多个IP地址时，可使用子接口。
- 在/etc/rc.local中可以添加，使重启后仍然生效。不建议。

**设置子接口**

```shell
# 子接口的设置
ifconfig 网络接口:子接口编号 IP地址 netmask 子网掩码
```

```shell
[root@localhost network-scripts]# ifconfig ens33:11 192.168.186.153 netmask 255.255.255.0
[root@localhost network-scripts]# ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.186.154  netmask 255.255.255.0  broadcast 192.168.186.255
        inet6 fe80::9de5:f5e7:1447:1951  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:fa:d4:2e  txqueuelen 1000  (Ethernet)
        RX packets 1990829  bytes 2488531319 (2.3 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 485327  bytes 711271182 (678.3 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens33:11: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.186.153  netmask 255.255.255.0  broadcast 192.168.186.255
        ether 00:0c:29:fa:d4:2e  txqueuelen 1000  (Ethernet)
```

### 设置主机名 hostname 或 /etc/hostname /etc/hosts

- 可修改/etc/hostname配置文件，使重启仍然生效。
- 在修改完主机名后，还需要修改相应的/etc/hosts文件，使主机能够顺利地解析该主机名。

```shell
# 设置主机名 并不修改/etc/hostname文件和/etc/hosts文件，能够被ping通
hostname 新的主机名
# 查看主机名
hostname
```

```shell
# 通过/etc/hostname修改
vim /etc/hostname #将主机名改为myhost
# 修改/ect/host添加该主机地域名解析
vim /etc/hosts #添加 127.0.0.1 myhost
# 检查能否ping通
ping myhost
```

```shell
[root@localhost ~]# hostname
localhost.localdomain
[root@localhost ~]# hostname mylinux
[root@localhost ~]# hostname
mylinux
[root@localhost ~]# ping mylinux
PING mylinux(mylinux (fe80::20c:29ff:fe00:6254%ens160)) 56 data bytes
64 bytes from mylinux (fe80::20c:29ff:fe00:6254%ens160): icmp_seq=1 ttl=64 time=0.146 ms
64 bytes from mylinux (fe80::20c:29ff:fe00:6254%ens160): icmp_seq=2 ttl=64 time=0.069 ms
64 bytes from mylinux (fe80::20c:29ff:fe00:6254%ens160): icmp_seq=3 ttl=64 time=0.084 ms
^C
--- mylinux ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 0.069/0.099/0.146/0.035 ms
[root@localhost ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
```

```shell
[root@localhost ~]# vim /etc/hostname #此时修改为myLinuxHost，但此时ping不通
[root@localhost ~]# hostname
mylinux
[root@localhost ~]# vim /etc/hosts #添加 127.0.0.1 myLinuxHost
[root@localhost ~]# hostname #hostname应该是重启后正常
mylinux
[root@localhost ~]# ping myLinuxHost
PING mylinuxHost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.061 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.059 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.054 ms
64 bytes from localhost (127.0.0.1): icmp_seq=4 ttl=64 time=0.054 ms
64 bytes from localhost (127.0.0.1): icmp_seq=5 ttl=64 time=0.097 ms
^C
--- mylinuxHost ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4084ms
rtt min/avg/max/mdev = 0.054/0.065/0.097/0.016 ms
```

### 默认网关 route 或 /etc/sysconfig/network

- 在设置好IP地址以后，如果要访问其他的子网或Internet，用户还需要设置路由，或使用默认网关。


**route命令**

```shell
route add default gw IP地址
```

- 可以将执行的命令写入到/etc/rc.local文件中。

```shell
# 修改/etc/sysconfig/network文件
vim /etc/sysconfig/network #添加内容 GATEWAY=IP地址
# 重启
systemctl restart network
# 如果没有network服务
yum install network-service
```

### 设置DNS服务器

#### 在接口配置文件/etc/sysconfig/network-scipts/ifcfg.* 中设置DNSn

#### /etc/resolv.conf  当接口配置文件中的DEFROUTE=yes时，失效。

![](c:/notebook/pictures/Snipaste_2022-12-07_00-21-46.png =700x)

## DHCP 动态主机配置协议

- DHCP是用来自动给客户端分配TCP/IP信息的网络协议。
- DHCP基于CS模式，DHCP提供一组动态指定IP地址和相关网络配置参数的机制。

**工作原理**

- 每个DHCP客户端通过广播连接到区域内的DHCP服务器，该服务器响应请求，返回包括IP地址、网关和其他网络配置信息。

![](c:/notebook/pictures/Snipaste_2022-12-07_00-49-08.png =500x)

![](c:/notebook/pictures/Snipaste_2022-12-07_00-50-46.png =650x)

### 配置DHCP服务器

**1.rpm安装**

- dhcp-libs
- dhcp-common
- dhcp

```shell
[root@bogon ~]# rpm -aq | grep dhcp
dhcp-libs-4.2.5-58.el7.x86_64
dhcp-common-4.2.5-58.el7.x86_64
[root@bogon ~]# cd /mnt/room/Packages
[root@bogon Packages]# ll | grep dhcp
-r--r--r--.  383 root root   525388 May 16  2017 dhcp-4.2.5-58.el7.x86_64.rpm
-r--r--r--.  394 root root   177744 May 16  2017 dhcp-common-4.2.5-58.el7.x86_64.rpm
-r--r--r--.  383 root root   132816 May 16  2017 dhcp-libs-4.2.5-58.el7.i686.rpm
-r--r--r--.  394 root root   133128 May 16  2017 dhcp-libs-4.2.5-58.el7.x86_64.rpm
[root@bogon Packages]# rpm -ivh dhcp-4.2.5-58.el7.x86_64.rpm
warning: dhcp-4.2.5-58.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:dhcp-12:4.2.5-58.el7             ################################# [100%]
```

**2.编辑配置文件 /etc/dhcp/dhcpd.conf**

- /etc/dhcp/dhcpd.conf DHCP主配置文件
- /etc/init.d/dhcpd DHCP服务启停文件 

**DHCP服务器配置**

```shell
[root@bogon ~]# cat /etc/dhcp/dhcpd.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp*/dhcpd.conf.example
#   see dhcpd.conf(5) man page

# 以上是格式说明和实例文件所在位置
# 以下为添加的内容

# 定义所支持的DNS动态更新类型。
# none表示不支持动态更新，interim表示DNS互动更新模式，ad-hoc表示特殊DNS更新模式
ddns-update-style none;

# 指定接收DHCP请求的网卡的子网地址(自己计算)
subnet 192.168.186.0 netmask 255.255.255.0{
        # 指定默认网关
        option routers 192.168.186.1;
        # 指定默认子网掩码
        option subnet-mask 255.255.255.0;
        # 指定DNS服务器地址
        option domain-name-servers 61.139.2.69;
        # 最大租用周期 秒
        max-lease-time 172800;
        # 此DHCP服务分配的IP地址范围
        range 192.168.186.230 192.168.186.240;
}
```

**DHCP客户端**

```shell
[root@mylinuxHost ~]# cat /etc/sysconfig/network-scripts/ifcfg-ens160
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp  # 该项启用DHCP服务
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=ens160
UUID=8369a359-ed22-4998-be21-5d73d3793256
DEVICE=ens160
ONBOOT=yes
[root@mylinuxHost ~]# ifdown ens160
WARN      : [ifdown] You are using 'ifdown' script provided by 'network-scripts', which are now deprecated.
WARN      : [ifdown] 'network-scripts' will be removed in one of the next major releases of RHEL.
WARN      : [ifdown] It is advised to switch to 'NetworkManager' instead - it provides 'ifup/ifdown' scripts as well.
[root@mylinuxHost ~]# ifconfig ens160
ens160: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        ether 00:0c:29:00:62:54  txqueuelen 1000  (Ethernet)
        RX packets 73058  bytes 103609333 (98.8 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 20692  bytes 1263403 (1.2 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
[root@mylinuxHost ~]# ifup ens160
WARN      : [ifup] You are using 'ifup' script provided by 'network-scripts', which are now deprecated.
WARN      : [ifup] 'network-scripts' will be removed in one of the next major releases of RHEL.
WARN      : [ifup] It is advised to switch to 'NetworkManager' instead - it provides 'ifup/ifdown' scripts as well.
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/8)
[root@mylinuxHost ~]# ifconfig ens160
ens160: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.186.137  netmask 255.255.255.0  broadcast 192.168.186.255
        inet6 fe80::20c:29ff:fe00:6254  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:00:62:54  txqueuelen 1000  (Ethernet)
        RX packets 101538  bytes 144975827 (138.2 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 28945  bytes 1716123 (1.6 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

## DNS 域名服务

- 在使用域名访问网络时，DNS负责将其解析为IP地址。
- DNS是一个分布式数据库系统，扩充性好。新加入的网络应用可以通过DNS负责将新主机的信息传播到网络中的其他部分。

**域名查询的方式**

1. 递归查询：由最初的域名服务器代替客户端进行域名查询。若该域名服务器不能直接回答，则会在域中的各分支的上下进行递归查询，最终将返回查询结果给客户端，在服务器查询期间，客户端将完全处于等待状态。
2. 迭代查询：每次由客户端发起请求，若请求的域名服务器能提供需要查询的信息则返回主机地址信息，若不能则引导客户端到其他域名服务器查询。

**DNS服务器类型**

| 服务器类型     | 说明                           |
| :------------ | :---------------------------- |
| 高速缓存服务器 | 将每次域名查询的结果缓存到本机   |
| 主DNS服务器    | 提供特定域的权威信息，是可信赖的 |
| 辅助DNS服务器  | 信息来源与主DNS服务器           |

### DNS服务器配置 BIND软件

#### 1.安装

```shell
[root@bogon ~]# rpm -aq | grep bind
bind-utils-9.9.4-50.el7.x86_64
keybinder3-0.3.0-1.el7.x86_64
bind-libs-lite-9.9.4-50.el7.x86_64
bind-license-9.9.4-50.el7.noarch
bind-libs-9.9.4-50.el7.x86_64
rpcbind-0.2.0-42.el7.x86_64
[root@bogon ~]# cd /mnt/room/Packages
[root@bogon Packages]# ll | grep bind
-r--r--r--.  383 root root  1865044 May 23  2017 bind-9.9.4-50.el7.x86_64.rpm
-r--r--r--.  383 root root    87964 May 23  2017 bind-chroot-9.9.4-50.el7.x86_64.rpm
-r--r--r--.  356 root root   124676 Apr 28  2017 bind-dyndb-ldap-11.1-3.el7.x86_64.rpm
-r--r--r--.  383 root root  1033984 May 23  2017 bind-libs-9.9.4-50.el7.i686.rpm
-r--r--r--.  394 root root  1050544 May 23  2017 bind-libs-9.9.4-50.el7.x86_64.rpm
-r--r--r--.  383 root root   739580 May 23  2017 bind-libs-lite-9.9.4-50.el7.i686.rpm
-r--r--r--.  394 root root   749880 May 23  2017 bind-libs-lite-9.9.4-50.el7.x86_64.rpm
-r--r--r--.  764 root root    86208 May 23  2017 bind-license-9.9.4-50.el7.noarch.rpm
-r--r--r--.  383 root root   303928 May 23  2017 bind-pkcs11-9.9.4-50.el7.x86_64.rpm
-r--r--r--.  383 root root  1181628 May 23  2017 bind-pkcs11-libs-9.9.4-50.el7.i686.rpm
-r--r--r--.  383 root root  1202276 May 23  2017 bind-pkcs11-libs-9.9.4-50.el7.x86_64.rpm
-r--r--r--.  383 root root   202108 May 23  2017 bind-pkcs11-utils-9.9.4-50.el7.x86_64.rpm
-r--r--r--.  394 root root   207972 May 23  2017 bind-utils-9.9.4-50.el7.x86_64.rpm
-r--r--r--. 1641 root root   121980 Apr  1  2014 cmpi-bindings-pywbem-0.9.5-6.el7.x86_64.rpm
-r--r--r--. 1652 root root    14680 Apr  2  2014 keybinder3-0.3.0-1.el7.i686.rpm
-r--r--r--. 1679 root root    14628 Apr  2  2014 keybinder3-0.3.0-1.el7.x86_64.rpm
-r--r--r--.  269 root root    60564 Jun  2  2017 rpcbind-0.2.0-42.el7.x86_64.rpm
-r--r--r--.  134 root root   528072 Jun 22  2017 samba-winbind-4.6.2-8.el7.x86_64.rpm
-r--r--r--.  134 root root   131280 Jun 22  2017 samba-winbind-clients-4.6.2-8.el7.x86_64.rpm
-r--r--r--.  134 root root   113500 Jun 22  2017 samba-winbind-modules-4.6.2-8.el7.i686.rpm
-r--r--r--.  134 root root   113472 Jun 22  2017 samba-winbind-modules-4.6.2-8.el7.x86_64.rpm
-r--r--r--.  186 root root   113988 Jun 22  2017 sssd-winbind-idmap-1.15.2-50.el7.x86_64.rpm
-r--r--r--.  388 root root    63960 Mar 29  2017 ypbind-1.37.1-9.el7.x86_64.rpm
[root@bogon Packages]# rpm -ivh bind-9.9.4-50.el7.x86_64.rpm
warning: bind-9.9.4-50.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:bind-32:9.9.4-50.el7             ################################# [100%]
```

#### 2.编辑配置文件 /etc/named/conf

- /etc/named.conf DNS主配置文件
- /usr/lib/systemd/named.service DNS服务器的控制单元文件

![](c:/notebook/pictures/Snipaste_2022-12-07_14-56-54.png =1100x)

```shell
[root@mylinuxHost Packages]# cat /etc/named.conf
//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

options {
        listen-on port 53 { any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { any; };

        /* 
         - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
         - If you are building a RECURSIVE (caching) DNS server, you need to enable 
           recursion. 
         - If your recursive DNS server has a public IP address, you MUST enable access 
           control to limit queries to your legitimate users. Failing to do so will
           cause your server to become part of large scale DNS amplification 
           attacks. Implementing BCP38 within your network would greatly
           reduce such attack surface 
        */
        recursion yes;

        dnssec-enable yes;
        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
        type hint;
        file "named.ca";
};


# 添加：搭建一个域名访问器，位于192.168.186.137，
# 其他主机可以通过该域名服务器解析已经注册的以oa.com结尾的域名
zone "oa.com" IN {
        type master;
        file "oa.com.zone";
        allow-update { none; };
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```

#### 3.编辑DNS数据文件 /var/named/oa.com.zone

- 该文件可以配置每个域名指向的实际IP，在配置此文件时注意此文件的权限要属于named组，否则服务不能正常启动。\

```shell
vim /var/named/oa.com.zone
chmod 777 /var/named/oa.com/zone
```

![](c:/notebook/pictures/Snipaste_2022-12-07_15-22-09.png =1000x)

#### 4.启动域名服务

```shell
systemctl start named
```

#### DNS服务测试

##### 客户端需要的配置

**1. /etc/resolv.conf**

- 客户端设置DNS服务器地址

```shell
[root@bogon Packages]# vim /etc/resolv.conf
[root@bogon Packages]# cat /etc/resolv.conf
# Generated by NetworkManager
search localdomain
# nameserver 192.168.186.2
nameserver 192.168.186.137
```

**2. nslookup命令检查**

```shell
[root@bogon Packages]# nslookup
> server 192.168.186.137
Default server: 192.168.186.137
Address: 192.168.186.137#53
> bbs.oa.com
;; connection timed out; trying next origin
;; connection timed out; no servers could be reached
```

## 路由

### 配置网络接口地址

#### ifconfig 临时修改

```shell
ifconfig 网络接口 选项
# up 启动该网络接口
# down 关闭该网络接口
# netmask 子网掩码
# broadcast 广播地址
# address IP地址
```

#### 网络接口配置文件 永久修改

**/etc/sysconfig/network-scripts/ifcfg-网络接口名**

- 需要`systemctl restart network`之后生效

```shell
[root@localhost ~]# vim /etc/sysconfig/network-scripts/ifcfg-ens33
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="dhcp" # 地址分配方式
# dhcp 表示从DHCP服务器动态获取
# none 表示静态路由
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33" # 网络接口名
UUID="a6bb1ccf-854f-4179-8245-ebcd4c8946cf"
DEVICE="ens33" # 网络接口名
ONBOOT="yes" # 是否在主机启动时启动该接口
```

#### route 修改静态路由

```shell
# 添加静态路由
route add [-net | host] 目标网络/主机 [netmask] [gw] [matric] [dev]
# 删除静态路由
route del [-net | host] 目标网络/主机 [netmask] [gw] [matric] [dev]
```

### 策略路由

![](c:/notebook/pictures/Snipaste_2022-12-12_00-12-12.png =600x)

#### 路由表的管理

- RHEL系统中最多可以同时存在256个路由表（0~255）。每个路由表都各自独立，数据包传输时根据路由策略数据库中的策略决定数据包应该由哪个路由表传输。
- Linux系统负责维护4个路由表：0、253、254、255。
    - 0号表unspec是系统保留表；
    - 253号表default是默认路由表，一般来说，默认的路由都存放在253号表；
    - 254号表main是主路由表，如果没有指明路由所属的表，所有的路由都默认放在这个表；
    - 255号表local是本地路由表，由系统自动维护，管理员不能直接修改，本地接口地址、广播地址、NAT地址都放在这。
    
**/etc/iproute2/rt_tables**

- 表号和表名的对应关系

```shell
[root@localhost ~]# cat /etc/iproute2/rt_tables
#
# reserved values
#
255     local
254     main
253     default
0       unspec
#
# local
#
#1      inr.ruhep
```

#### ip route命令 路由管理

```shell
ip rotue list SELECTOR
ip route {change | del | add | append | replace | monitor} 路由表名/路由表号
```

```shell
# 查看所有路由表信息
ip route
# 查看main路由表的信息
ip route list table main
# 在主路由表中添加一条路由信息：
ip route add 192.168.186.0/24 dev ens33 table main
# 删除到网络192.168.186.0/24的路由信息
ip route del 192.168.186.0/24
```

#### ip rule命令 路由策略管理

```shell
# 查看当前系统存在的路由策略
ip rule
# 添加一条路由策略，匹配规则：所有来自192.168.186.0/24子网的数据包,使用12号路由表
ip rule add from 192.168.186.0/24 table 12
# 添加一条路由策略，匹配规则：所有发送192.168.186.0/24子网的数据包,使用13号路由表
ip rule add to 192.168.186.0/24 table 13
# 删除某条路由策略
ip route del to 192.168.186.0/24
# 清空路由策略数据库
ip rule flush
```

## NAT

- NAT网络地址转换，将IP数据包头中的IP地址转换为另外一个IP地址，以实现私有IP地址访问公有网络。
   - 私有IP地址：A类10.0.0.0~10.255.255.255; B类172.16.0.0~172.31.255.255；C类192.168.0.0~192.168.255.255
- NAT会自动修改IP报文的源IP地址和目的IP地址，IP地址校验在NAT处理过程中自动完成。

![](c:/notebook/pictures/Snipaste_2022-12-12_14-33-36.png =300x)

**NAT的实现：**

1. 静态转换：某个私有IP地址只能转换为某个指定的公有IP地址。
2. 动态转换：IP地址是随机的，系统会自动随机选择一个没有被使用的公有IP地址进行转换。
3. **端口多路复用**：修改数据包的源端口并进行端口转换，使内部网络的多台主机可以共享一个公有IP地址。

### 配置NAT服务

```shell
# 开启转发功能
echo 1 > /proc/sys/net/ipv4/ip_forward
# 配置NAT规则
```

#### firewall-cmd

**将网络接口加入到外部区域**

```shell
# 查看网络接口ens33所属区域
firewall-cmd --get-zone-of-interface=ens33
# 改变区域为external
firewall-cmd --permanent --zone=external --change-interface=ens33
# 查看外部区域配置
firwall-cmd --zone=external --list-all
# 打开外部区域的伪装
firewall-cmd --permanent --zone=external --add-masquerade
```

**配置内部接口**

```shell
firewall-cmd --get-zone-of-interface=ens101
firewall-cmd --permanent --zone=internal --change-interface=ens101
```

**配置上网**

- 将所有主机的网关设置为RHEL主机的网络接口ens101（内部接口）的IP地址。
- 内部网络的主机的DNS服务器设置为运营商提供的DNS服务器的地址。

# 网络共享NFS、Samba、FTP

## NFS 网络文件系统

- NFS是一种分布式文件系统，允许网络中不同操作系统的计算机间共享文件，其通信协议基于TCP/IP协议层，可以将远程计算机磁盘挂载（mount）到本地并进行读写。
- NFS在文件传送/信息传送过程中依赖于RPC协议，该协议可以在不同的系统之间使用，此通信协议设计于主机及操作系统无关。

### 配置NFS服务器

#### 1.安装rpm

- nfs-utils 基本的NFS命令与脚本
- rpcbind 管理RPC连接

##### nfs 主要文件和进程

| 文件和进程   | 说明                                                                                                                  |
| :---------- | :-------------------------------------------------------------------------------------------------------------------- |
| nfs.service | NFS服务启停控制单元，位于/usr/lib/systemd/system/nfs.service                                                            |
| rpc.nfsd    | NFS守护进程，控制客户端是否可以登录服务器，是nfs-utils的一部分存放在usr/sbin目录，可以结合/etc/hosts.allow和/etc/hosts.deny |
| rpc.mountd  | RPC安装守护进程，管理NFS的文件系统，初始化客户端的mount请求，是nfs-utils的一部分存放在usr/sbin目录                          |
| rpc.statd   | 抓取文件锁，是nfs-utils的一部分存放在usr/sbin目录                                                                        |
| rpc.rquotad | 对客户文件的磁盘配额限制，是nfs-utils的一部分存放在usr/sbin目录                                                           |
| rpcbind     | 管理RPC连接，对NFS是必需的。因为是NFS的动态端口分配守护进程，如果rpcbind不启动，NFS服务无法启动                              |
| exportfs    | 如果修改了**/etc/exports**，则不需要重新激活NFS，只需要重新扫描一遍/etc/exports文件，并重新将设定加载即可                    |

###### exportfs 管理NFS共享文件系统列表

![](c:/notebook/pictures/Snipaste_2022-12-07_17-00-45.png =1100x)

###### showmount 查询“mountd”守护进程，以显示NFS服务器加载的信息。

![](c:/notebook/pictures/Snipaste_2022-12-07_17-01-59.png =300x)

#### 2.配置文件/etc/ports

- 在/etc/ports文件中，每一行代表一个共享目录，并且描述了该目录如何被共享。

![](c:/notebook/pictures/Snipaste_2022-12-07_17-09-36.png =800x)

```shell
[root@bogon /]# mkdir /root/nfsshare
[root@bogon /]# chmod 777 /root/nfsshare
[root@bogon /]# cat /etc/exports
# <共享目录> [客户端1(选项)] [客户端2(选项)]
/root/nfsshare *(rw,all_squash,sync,anonuid=1001,anongid=1001)
```

#### 3.启动服务

```shell
[root@bogon /]# systemctl start rpcbind
[root@bogon /]# systemctl start nfs
[root@bogon /]# systemctl status nfs
```

```shell
# 检查是否正确加载
[root@localhost ~]# showmount -e localhost
Export list for localhost:
/root/nfsshare *
```

#### 4. 配置NFS客户端

```shell
[root@mylinuxHost ~]# mkdir /test
[root@mylinuxHost ~]# mount -t nfs -o rw 192.168.186.154:/root/nfsshare /test
[root@mylinuxHost ~]# cd /test
[root@mylinuxHost ~]# ll
```

- 如果mount.nfs: No route to host，则关闭服务器的防火墙和SELinux

[nfs错误合集](https://developer.aliyun.com/article/47560#)

## Samba 文件服务器

- Samba基于Server Message Block协议（SMB信息服务块），SMB是客户机/服务器型协议，是在局域网上共享文件和打印的通信协议，客户通过SMB可访问服务器上的共享文件系统和打印机等资源。

### Samba 服务配置

#### 安装

```shell
# 安装依赖
cd /mnt/room/Packages
rpm -aq gcc
rpm -ivh python-devel-2.7.5-58.el7.x86_64.rpm
rpm -ivh gnutls-3.3.26-9.el7.x86_64.rpm
rpm -ivh gnutls-c++-3.3.26-9.el7.x86_64.rpm
rpm -ivh libtasn1-devel-4.10-1.el7.x86_64.rpm
rpm -ivh libtasn1-4.10-1.el7.x86_64.rpm
rpm -ivh zlib-devel-1.2.7-17.el7.x86_64.rpm
rpm -ivh zlib-1.2.7-17.el7.x86_64.rpm
rpm -ivh gmp-devel-6.0.0-15.el7.x86_64.rpm
rpm -ivh nettle-devel-2.7.1-8.el7.x86_64.rpm
rpm -ivh p11-kit-devel-0.23.5-3.el7.x86_64.rpm
rpm -ivh libattr-devel-2.4.46-12.el7.x86_64.rpm
rpm -ivh libacl-devel-2.2.51-12.el7.x86_64.rpm
rpm -ivh cyrus-sasl-devel-2.1.26-21.el7.x86_64.rpm
rpm -ivh openldap-devel-2.4.44-5.el7.x86_64.rpm
rpm -ivh ldns-1.6.16-10.el7.x86_64.rpm
rpm -ivh unbound-libs-1.4.20-34.el7.x86_64.rpm
rpm -ivh unbound-1.4.20-34.el7.x86_64.rpm
rpm -ivh gnutls-dane-3.3.26-9.el7.x86_64.rpm
rpm -ivh gnutls-devel-3.3.26-9.el7.x86_64.rpm
rpm -ivh pytalloc-2.1.9-1.el7.x86_64.rpm
rpm -ivh samba-libs-4.6.2-8.el7.x86_64.rpm
rpm -ivh samba-common-libs-4.6.2-8.el7.x86_64.rpm
rpm -ivh samba-common-tools-4.6.2-8.el7.x86_64.rpm
rpm -ivh samba-4.6.2-8.el7.x86_64.rpm
```

##### 包含的程序

| 程序       | 说明                                                      |
| :-------- | :-------------------------------------------------------- |
| smbd      | SMB服务器，为客户机提供文件和打印服务                        |
| nmbd      | NetBIOS名字服务器，可以提供浏览器支持                        |
| smbclient | SMB客户程序，用于从Linux或其他操作系统上访问SMB服务器上的资源 |
| smbmount  | 挂载SMB文件系统的工具，对应的卸载工具：smbunmount            |
| smbpasswd | 用户增删服务器的用户和密码                                             |

#### 配置文件 /etc/samba/samba.conf

**配置文件/etc/samba/samba.conf**

```shell
[root@localhost ~]# vim /etc/samba/smb.conf

[global] 
# 表示全局配置
    # 与主机名相关的设置
    
    workgroup = sambausers  
    # 工作组名称

    netbios name = mySamba   
    # 主机名称，不是hostname，在同一个组中，netbios name必须唯一

    serverstring = this is a test samba server 
    # 说明性文字，内容无关紧要

    # 与登录文件有关的设置
    #log file = /var/log/samba/log.%m   # 日志文件的存储文件名，%m代表的是client端Internet主机名，就是hostname
    #max log size = 50      # 日志文件最大的大小为50Kb

    # 与密码相关的设置
    security = user       
    # share表示不需要密码,user需要用户名和密码，可设置的值为share、user和server

    #passdb backend = tdbsam
    # 打印机加载方式

    load printer = no # 不加载打印机
# 删除[home][printer]
[sambaTest]       
# 共享资源名称
    comment = Temporary file space 
    # 简单的解释，内容无关紧要
    path = /root/sambaTest       
    # 实际的共享目录
    writeable = yes    
    # 设置为可写入
    browseable = yes  
    # 可以被所有用户浏览到资源名称，
    guest ok = yes    
    # 是否允许匿名用户以guest身份登入
```

**配置登入用户**

```shell
# smbpasswd 命令的用法 
smbpasswd -a # 增加用户（要增加的用户必须以是系统用户）   
smbpasswd -d # 冻结用户，就是这个用户不能在登录了   
smbpasswd -e # 恢复用户，解冻用户，让冻结的用户可以在使用   
smbpasswd -n # 把用户的密码设置成空.   
                # 要在global中写入 null passwords -true   
smbpasswd -x # 删除用户  
```

```shell
mkdir /root/sambaTest
groupadd sambausers
useradd -g sambausers sambauser #该用户或组的权限就是登录samba的权限
chown -R sambauser.sambausers /root/sambaTest #没有root用户的权限，无法访问该文件，但是可以看见有这个文件
smbpasswd -a sambauser

#添加一个root用户的smbpasswd
smbpasswd -a root
```

#### 启动服务

```shell
/usr/sbin/smbd
/usr/sbin/nmbd
```

##### testparm命令 查看是否无误

```shell
testparm
echo $?
```

**可能需要关闭防火墙**

#### Windows使用

- 在Windows打开相关的服务
![](c:/notebook/pictures/Snipaste_2022-12-08_19-09-06.png =500x)
- 在Windows的运行或文件资源管理器，输入`\\192.168.186.155`（Samba对应的IP地址）

## FTP服务器

- FTP服务器中的文件按目录结构进行组织，用户通过网络与服务器建立连接。FTP仅基于TCP的服务，不支持UDP。
- FTP使用两个端口，通常：命令端口(21)，数据端口(20)

**主动FTP和被动FTP**

![](c:/notebook/pictures/Snipaste_2022-12-08_19-27-20.png =800x)


### ftp

#### ftp命令

![](c:/notebook/pictures/Snipaste_2022-12-08_22-58-25.png =500x)

| 常用命令     | 说明                                                                                                                                                                                                                                                               |
| :---------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| cd          | 切换路径                                                                                                                                                                                                                                                            |
| dir 和 ls   | dir查看FTP服务器中的文件及目录，ls仅查看文件                                                                                                                                                                                                                          |
| get 和 mget | get一次只下载一个文件；mget一次可以下载多个文件，而且支持通配符，需要注意的是在mget的时侯，需要对每一个文件都选择y/n，如果想不交互的下载全部的文件，可以先用prompt命令关掉交互方式（关闭：prompt off；打开：prompt on）。 获得的文件默认在当前的工作目录下（即执行加入FTP服务器的目录） |
| bye         | 退出                                                                                                                                                                                                                                                               |

#### 安装ftp

```shell
cd /mnt/room/Packages
rpm -ivh ftp-0.17-67.el7.x86_64.rpm
```

#### ftp访问方式

1. cmd中：ftp FTP服务器地址
2. 浏览器访问(已弃用)。使用命令`ftp://username:password@hostname:port`
3. 资源管理器访问。打开我的电脑，在文件路径中，输入ftp://网址
4. 客户端工具访问
5. 在桌面上新建快捷方式。方便需要时不时连接FTP的用户，右键单击选择新建快捷方式，直接访问快捷方式地址：`%SystemRoot%/explorer.exe ftp://用户名:密码@服务器网址`；如`D:\software\firefox\firefox.exe ftp:admin:admin@127.0.0.1:1990`。

```
C:\Users\zjk10>ftp 192.168.186.155
连接到 192.168.186.155。
220 (vsFTPd 3.0.2)
200 Always in UTF8 mode.
用户(192.168.186.155:(none)): anonymous
331 Please specify the password.
密码:
230 Login successful.
ftp>
```

- 对于cmd方式：匿名用户使用 anonymous,无密码登录

``` 
# 浏览器登录
ftp://user1:pass1@192.168.186.155:20
```

#### 系统用户和虚拟用户

**ftp的登录方式分为系统用户和虚拟用户**

- **系统用户**是指使用当前shell中的系统用户登录FTP服务器，用户登录后对于主目录具有和Shell中相同的权限。
- **虚拟用户**只能访问服务器为其提供的FTP服务，而不能访问系统的其他资源。在vsftp中，认证这些虚拟用户使用的是单独的口令库文件（pam_userdb），由可插入认证模块（PAM）认证，更安全灵活。

### vsftp的安装与配置

#### 安装

```shell
# 安装
rpm -ivh vsftpd-3.0.2-22.el7.x86_64.rpm
# 查看主要文件和安装路径
rpm -qpl vsftpd-3.0.2-22.el7.x86_64.rpm
```

```shell
# vsftp启停控制单元
/usr/lib/systemd/system/vsftpd.service
/usr/lib/systemd/system/vsftpd.target
/usr/lib/systemd/system/vsftpd@.service
# 保存认证用户
/etc/vsftpd/ftpusers
/etc/vsftpd/user_list
# 主配置文件
/etc/vsftp/vsftpd.conf
# 主程序
/usr/bin/vsftpd
```

#### 匿名FTP设置

```shell
# FTP服务器中可以访问的文件
chown -R ftp.users /var/ftp/pub
# 修改配置文件
vim /etc/vsftpd/vsftpd.conf
```

**配置文件/ect/vsftpd/vsftpd.conf**

```shell
# 修改：/etc/vsftpd/vsftpd.conf
listen=YES
# 允许匿名登录
anonymous_enable=YES
# 允许上传文件
write_enable=YES
anon_upload_enable=YES
# 启用日志
xferlog_enable=YES
# 日志路径
vsftpd_log_file=/var/log/vsftpd.log
# 使用匿名用户登录时，映射到的用户名
ftp_username=ftp
# 允许本地登录
local_enable=YES

# 即如下：
listen=YES
anonymous_enable=YES
write_enable=YES
anon_upload_enable=YES
xferlog_enable=YES
vsftpd_log_file=/var/log/vsftpd.log
ftp_username=ftp
local_enable=YES
```

**重启服务**

```shell
systemctl restart vsftpd
```

##### 登录

#### 实名FTP设置

##### 1. 修改/etc/vsftpd/vsftpd.conf

```shell
vim /etc/vsftpd/vsftpd.conf
# 修改如下
listen=YES
# 绑定本地主机IP
listen_address=192.168.186.155
# 禁止匿名用户登录
anonymous_enable=NO
anon_upload_enable=NO
anon_mkdir_write_enable=NO
anon_other_write_enable=NO
# 不允许FTP用户离开自己的主目录
chroot_list_enable=NO
# 虚拟用户列表，每行一个用户名
chroot_list_file=/etc/vsftpd.chroot_list
# 允许本地访问
local_enable=YES
# 允许写入
write_enable=YES
allow_writeable_chroot=YES
# 上传的文件默认的权限掩码
local_umask=022
# 禁止本地用户离开自己的FTP主目录
chroot_local_user=YES
# 权限验证需要的加密文件
pam_service_name=vsftpd.vu
# 开启虚拟用户的性能
guest_enable=YES
# 虚拟用户的宿主目录
guest_username=ftp
# 用户登录后操作主目录和本地用户具有同样的权限
virtual_use_local_privs=YES
# 虚拟用户主目录设置文件
user_config_dir=/etc/vsftpd/vconf

# 即如下：
listen=YES
listen_address=192.168.186.155
anonymous_enable=NO
anon_upload_enable=NO
anon_mkdir_write_enable=NO
anon_other_write_enable=NO
chroot_list_enable=NO
chroot_list_file=/etc/vsftpd.chroot_list
local_enable=YES
write_enable=YES
allow_writeable_chroot=YES
local_umask=022
chroot_local_user=YES
pam_service_name=vsftpd.vu
guest_enable=YES
guest_username=ftp
virtual_use_local_privs=YES
user_config_dir=/etc/vsftpd/vconf
```

##### 2.编辑用户文件/etc/vsftpd.chroot_list

- 一行一个用户名

```shell
vim /etc/vsftpd.chroot_list
#user1
#user2
```

##### 3.增加用户并指定主目录

```shell
mkdir -p /data/{user1,user2}
chmod -R 777 /data/user1 /data/user2
```

##### 4.设置用户密码数据库

```shell
echo -e "user1\npass1\nuser2\npass2">/etc/vsftpd/vusers.list
cd /etc/vsftpd
db_load -T -t hash -f vusers.list vusers.db
chmod 600 vusers.*
```

##### 5.指定认证方式

```shell
echo -e "#%PAM-1.0\n\nauth    required pam_userdb.so    db=/etc/vsftpd/vusers\naccount    required pam_userdb.so    db=/etc/vsftpd/vusers">/etc/pam.d/vsftpd.vu
mkdir vconf
```

##### 6.编辑对用户的用户文件，指定主目录

```shell
cd /etc/vsftpd/vconf
vim user1
# local_root=/data/user1
vim user2
# local_root=/data/user2
```

##### 7.创建标识文件

```shell
touch /data/user1/user1
touch /data/user2/user2
```

##### 8.重启服务

```shell
systemctl restart vsftpd
```

##### 使用

- 如匿名，只不过需要用户密码

### proftpd的安装配置 

#### 安装

```shell
cd /root/install
# 速度过慢，下不动
#wget ftp://ftp.proftpd.org/distrib/source/proftpd-1.3.8.tar.gz
#tar -xvf proftpd-1.3.8.tar.gz
# 使用git clone https://github.com/proftpd/proftpd.git 得到已经解压的
cd /mnt/room/Packages
rpm -ivh m4-1.4.16-10.el7.x86_64.rpm
rpm -ivh autoconf-2.69-11.el7.noarch.rpm
rpm -ivh perl-CPAN-Meta-YAML-0.008-14.el7.noarch.rpm
rpm -ivh perl-JSON-PP-2.27202-2.el7.noarch.rpm
rpm -ivh perl-Parse-CPAN-Meta-1.4404-5.el7.noarch.rpm
rpm -ivh perl-Thread-Queue-3.02-2.el7.noarch.rpm
rpm -ivh perl-Test-Harness-3.28-3.el7.noarch.rpm
rpm -ivh automake-1.13.4-3.el7.noarch.rpm
rpm -ivh libtool-2.4.2-22.el7_3.x86_64.rpm
rpm -ivh libtool-ltdl-2.4.2-22.el7_3.x86_64.rpm
rpm -ivh libtool-ltdl-devel-2.4.2-22.el7_3.x86_64.rpm
cd /root/install/proftp
# 替换该解压目录下的config.sub和config.guess文件
cp /usr/share/libtool/config/config.guess .
cp /usr/share/libtool/config/config.sub .
./configure --prefix=/usr/local/proftp
# 出问题
make
make install
```

#### 匿名FTP设置

- 配置文件 /usr/local/proftp/etc/proftpd.conf

```shell
chown -R ftp.users /var/ftp
vim /usr/local/proftp/etc/proftpd.conf
#修改内容：
```

```
ServerName    "ProFTP Default Installation"
ServerType    standalone
DefaultServer    on
Port    21
Umask    022
# 最大实例数
MaxInstances    30
# FTP启动后将切换到此用户和组运行
User    myftp
Group    myftp
AllowOverwrite    on
# 匿名服务器配置
<Anonymous ~>
    User    ftp
    Group    ftp
    UserAlias    anonymous ftp
    MaxClients    10
# 控制权限
<Limit WRITE>
    AllowAll
</Limit>
</Anonymous>
```

##### 启动服务

```shell
# 添加用户 useradd myftp
# 启动服务
/usr/local/proftp/sbin/proftpd
# 关闭SELinux
setenforce 0
# 检查是否成功
ps -ef | grep proftpd
```

##### 匿名用户登录

- 使用的用户名为 anonymous，无密码登录。

```shell
ftp 192.168.186.155
```

#### 实名FTP设置 

```shell
useradd -d /data/proftpuser1 -m proftpuser1
useradd -d /data/proftpuser2 -m proftpuser2
passwd proftpuser1
passwd proftpuser2
```

**编辑  /usr/local/proftp/etc/proftpd.conf**

```shell
# 添加以下：
<VirtualHost 192.168.186.155>
    DefaultRoot    /data/guest
    AllowOverwrite    no
    <Limit STOR MKD RETR>
        AllowAll
    </Limit>
    <Limit DIRS WRITE READ DELE RMD>
        AllowUser proftpuser1 proftpuser2
        DenyAll
    </Limit>
</VirtualHost>
```

##### 启动服务

```shell
/usr/local/proftp/sbin/proftpd
mkdir /data/guest
chmod -R 777 /data/guest/
```

##### 登录

# 防火墙管理

## 工作原理

- Linux内核提供的防火墙功能通过netfiter框架实现，并提供了iptables、firewalld工具配置和修改防火墙的规则。

### netfiter

- netfiter的通用框架不依赖于具体的协议，而是为每种网络协议定义一套**钩子函数**，在数据包经过协议栈的几个关键点时，调用这些钩子函数，同时协议栈将数据包和钩子函数作为参数传递给netfiter框架。
- 对于每种网络协议定义的钩子函数，任何内核模块可以对每种协议的一个或多个钩子函数进行注册，实现挂接：当某个数据包被传递给netfiter框架时，内核能检测到是否有相关模块对该协议和钩子函数进行了注册，若发现注册信息，则调用该模块注册时使用的回调函数，然后对应模块去检查、修改、丢弃该数据包并指示netfiter将该数据包传入用户空间的队列。

### netfiter体系结构

**数据包通信步骤**

![](c:/notebook/pictures/Snipaste_2022-12-12_19-10-31.png =400x)

![](C:/notebook/pictures/Snipaste_2022-12-12_19-13-32.png =700x)

### 包过滤

- 每个函数都可以对数据包进行处理，最基本的操作就是对数据包进行过滤。root用户可以通过iptables工具向内核模块注册多个过滤规则，并且指明过滤规则的优先权，每个钩子函数按照规则进行匹配，如果匹配则执行过滤操作。

| 过滤操作   | 说明                  |
| :-------- | :------------------- |
| NF_ACCEPT | 继续正常地传送包       |
| NF_DROP   | 丢弃包，停止传送       |
| NF_STOLEN | 已经接管包，不继续传送 |
| NF_REPEAT | 再次使用该钩子函数     |

### 包选择

- 在netfiter框架上已经创建了一个包选择系统，这个包选择工具默认注册了3个表：过滤filter表、网络地址转换NAT表：mangle表。

![](c:/notebook/pictures/Snipaste_2022-12-12_19-42-30.png =600x)

- 在调用钩子函数时，按照如上的顺序来调用需要的表。
- 包过滤表只是过滤包，而不改变包，实际中由网络过滤框架来通过NF_IP_FORWARD钩子的输入和输出接口。NF_IP_LOCAL_IN和NF_IP_LOCAL_OUT也可以过滤，但只对本机。
- NAT表分别服务于两套不同的网络过滤挂钩的包，对于非本地包，NF_IP_PRE_ROUTING和NF_IP_POST_ROUTING挂钩可以很好地解决源地址和目标地址的变更问题。
- NAT表与Filter表的区别在于NAT表只有新建连接的第一个包会在表中传送，结果将被用于以后所有来自这一连接的包。如：某一连接的第一个数据包在这个表中被替换了源地址，则接下来的这条连接的所有包都将被替换源地址。
- Mangle表用于真正改变包的信息，Mangle表和所有的5个网络过滤的钩子都有关。

## firewalld

### Zone 防火墙区域/网络区域

- Zone：防火墙区域/网络区域，是一系列可以被快速执行到网络接口的预设置。

![](c:/notebook/pictures/Snipaste_2022-12-12_19-52-57.png =630x)

- 一个网络接口只能与一个网络区域对应。当数据包进入区域后，防火墙会依据区域内的规则进行逐一过滤，只有符合规则的数据包才能通过区域到达本机应用。

**共9个区域，从信任到不信任 如下：**

| 区域                   | 说明                                                            |
| :--------------------- | :------------------------------------------------------------- |
| trusted （信任）       | 信任所有的网络连接                                               |
| internal （内部网络）   | 用于企业等的内部网络，可基本信任内部网络中的计算机不会威胁计算机安全 |
| home （家庭网络）       | 可基本信任家庭网络中的计算机不会危害计算机安全                     |
| work （工作网络）       | 可基本信任工作网络中的计算机不会危害计算机的安全                    |
| dmz （非军事区/隔离区） | 此区域内的电脑可以公开访问，可以有限的进入内部网络                  |
| external  （外部网络）  | 通常是使用了伪装的外部网络，该区域内的计算机可能会危害计算机安全     |
| public （公共区域）     | 在公共区域使用，该区域内的计算机可能会危害计算机安全                |
| block （阻塞/拒绝）     | 任何进入的网络连接都会被拒绝，并返回IPv4或IPv6的拒绝报文            |
| drop （丢弃）          | 任何进入的网络连接都会被丢弃，没有任何回复                         |

- 无论处于哪个区域，防火墙都不会拒绝由本机主动发起的网络连接，即：本地发起的数据包（包含对方响应或返回的数据包）将通过该区域。虽然这些区域已经有所描述，但实际通行规则由区域内的规则决定，最终决定连接是否被放行的是规则而不是区域的描述。

### firewall-cmd / firewall-config 配置工具

- firewall-cmd 是 firewalld的字符界面管理工具，firewalld支持动态更新，不用重启服务。

**firewalld跟iptables比起来至少有两大好处：**

- firewalld可以动态修改单条规则，而不需要像iptables那样，在修改了规则后必须得全部刷新才可以生效。
- firewalld自身并不具备防火墙的功能，而是和iptables一样需要通过内核的netfilter来实现，也就是说firewalld和 iptables一样，他们的作用都是用于维护规则，而真正使用规则干活的是内核的netfilter。


#### 配置firewalld

```shell
firewall-cmd --version  # 查看版本
firewall-cmd --help     # 查看帮助

# 查看设置：
firewall-cmd --state  # 显示状态
firewall-cmd --get-active-zones  # 查看区域信息
firewall-cmd --get-zone-of-interface=eth0  # 查看指定接口所属区域
firewall-cmd --panic-on  # 拒绝所有包
firewall-cmd --panic-off  # 取消拒绝状态
firewall-cmd --query-panic  # 查看是否拒绝

# 更新防火墙规则
firewall-cmd --reload
firewall-cmd --complete-reload
# 两者的区别就是第一个无需断开连接，就是firewalld特性之一动态添加规则，第二个需要断开连接，类似重启服务

# 将接口添加到区域，默认接口都在public
firewall-cmd --zone=public --add-interface=eth0
# 永久生效再加上 --permanent 然后reload防火墙

# 设置默认接口区域，立即生效无需重启
firewall-cmd --set-default-zone=public

# 查看所有打开的端口：
firewall-cmd --zone=dmz --list-ports

# 加入一个端口到区域：
firewall-cmd --zone=dmz --add-port=8080/tcp
# 若要永久生效方法同上

# 打开一个服务，类似于将端口可视化，服务需要在配置文件中添加，/etc/firewalld 目录下有services文件夹，这个不详细说了，详情参考文档
firewall-cmd --zone=work --add-service=smtp

# 移除服务
firewall-cmd --zone=work --remove-service=smtp

# 显示支持的区域列表
firewall-cmd --get-zones

# 设置为家庭区域
firewall-cmd --set-default-zone=home

# 查看当前区域
firewall-cmd --get-active-zones

# 设置当前区域的接口
firewall-cmd --get-zone-of-interface=enp03s

# 显示所有公共区域（public）
firewall-cmd --zone=public --list-all

# 临时修改网络接口（enp0s3）为内部区域（internal）
firewall-cmd --zone=internal --change-interface=enp03s

# 永久修改网络接口enp03s为内部区域（internal）
firewall-cmd --permanent --zone=internal --change-interface=enp03s
```


#### 服务管理

```shell
# 显示服务列表  
Amanda, FTP, Samba和TFTP等最重要的服务已经被FirewallD提供相应的服务，可以使用如下命令查看：

firewall-cmd --get-services

# 允许SSH服务通过
firewall-cmd --enable service=ssh

# 禁止SSH服务通过
firewall-cmd --disable service=ssh

# 打开TCP的8080端口
firewall-cmd --enable ports=8080/tcp

# 临时允许Samba服务通过600秒
firewall-cmd --enable service=samba --timeout=600

# 显示当前服务
firewall-cmd --list-services

# 添加HTTP服务到内部区域（internal）
firewall-cmd --permanent --zone=internal --add-service=http
firewall-cmd --reload     # 在不改变状态的条件下重新加载防火墙
```

#### 端口管理

```shell
# 打开443/TCP端口
firewall-cmd --add-port=443/tcp

# 永久打开3690/TCP端口
firewall-cmd --permanent --add-port=3690/tcp

# 永久打开端口好像需要reload一下，临时打开好像不用，如果用了reload临时打开的端口就失效了
# 其它服务也可能是这样的，这个没有测试
firewall-cmd --reload

# 查看防火墙，添加的端口也可以看到
firewall-cmd --list-all
```


#### 直接模式

```shell
# firewalld包括一种直接模式，使用它可以完成一些工作，例如打开TCP协议的9999端口
firewall-cmd --direct -add-rule ipv4 filter INPUT 0 -p tcp --dport 9000 -j ACCEPT
firewall-cmd --reload
```

#### 控制端口 / 服务

- 可以通过两种方式控制端口的开放，一种是指定端口号，另一种是指定服务名。虽然开放 http 服务就是开放了 80 端口，但是还是不能通过端口号来关闭，也就是说通过指定服务名开放的就要通过指定服务名关闭；通过指定端口号开放的就要通过指定端口号关闭。还有一个要注意的就是指定端口的时候一定要指定是什么协议，tcp 还是 udp。知道这个之后以后就不用每次先关防火墙了，可以让防火墙真正的生效。

```shell
firewall-cmd --add-service=mysql        # 开放mysql端口
firewall-cmd --remove-service=http      # 阻止http端口
firewall-cmd --list-services            # 查看开放的服务
firewall-cmd --add-port=3306/tcp        # 开放通过tcp访问3306
firewall-cmd --remove-port=80tcp        # 阻止通过tcp访问3306
firewall-cmd --add-port=233/udp         # 开放通过udp访问233
firewall-cmd --list-ports               # 查看开放的端口
```

#### 伪装 IP

```shell
firewall-cmd --query-masquerade # 检查是否允许伪装IP
firewall-cmd --add-masquerade   # 允许防火墙伪装IP
firewall-cmd --remove-masquerade# 禁止防火墙伪装IP
```

#### 端口转发

- 端口转发可以将指定地址访问指定的端口时，将流量转发至指定地址的指定端口。转发的目的如果不指定 ip 的话就默认为本机，如果指定了 ip 却没指定端口，则默认使用来源端口。 如果配置好端口转发之后不能用，可以检查下面两个问题：
1. 比如我将 80 端口转发至 8080 端口，首先检查本地的 80 端口和目标的 8080 端口是否开放监听了
2. 其次检查是否允许伪装 IP，没允许的话要开启伪装 IP

```shell
firewall-cmd --add-forward-port=port=80:proto=tcp:toport=8080   # 将80端口的流量转发至8080
firewall-cmd --add-forward-port=proto=80:proto=tcp:toaddr=192.168.1.0.1 # 将80端口的流量转发至192.168.0.1
firewall-cmd --add-forward-port=proto=80:proto=tcp:toaddr=192.168.0.1:toport=8080 # 将80端口的流量转发至192.168.0.1的8080端口
```

- 当我们想把某个端口隐藏起来的时候，就可以在防火墙上阻止那个端口访问，然后再开一个不规则的端口，之后配置防火墙的端口转发，将流量转发过去。
- 端口转发还可以做流量分发，一个防火墙拖着好多台运行着不同服务的机器，然后用防火墙将不同端口的流量转发至不同机器。


```shell
[root@bogon ~]# firewall-cmd --get-zones
block dmz drop external home internal public trusted work
[root@bogon ~]# firewall-cmd --get-default-zone
public
[root@bogon ~]# firewall-cmd --get-active-zones
public
  interfaces: ens33
[root@bogon ~]# firewall-cmd --zone=internal --change-interface=ens33
The interface is under control of NetworkManager, setting zone to 'internal'.
success
[root@bogon ~]# firewall-cmd --get-active-zones
internal
  interfaces: ens33
[root@bogon ~]# firewall-cmd --get-zone-of-interface=ens33
internal
[root@bogon ~]# firewall-cmd --reload
success
[root@bogon ~]# firewall-cmd --get-zone-of-interface=ens33
internal
[root@bogon ~]# firewall-cmd --get-services
RH-Satellite-6 amanda-client amanda-k5-client bacula bacula-client bitcoin bitcoin-rpc bitcoin-testnet bitcoin-testnet-rpc ceph ceph-mon cfengine condor-collector ctdb dhcp dhcpv6 dhcpv6-client dns docker-registry dropbox-lansync elasticsearch freeipa-ldap freeipa-ldaps freeipa-replication freeipa-trust ftp ganglia-client ganglia-master high-availability http https imap imaps ipp ipp-client ipsec iscsi-target kadmin kerberos kibana klogin kpasswd kshell ldap ldaps libvirt libvirt-tls managesieve mdns mosh mountd ms-wbt mssql mysql nfs nrpe ntp openvpn ovirt-imageio ovirt-storageconsole ovirt-vmconsole pmcd pmproxy pmwebapi pmwebapis pop3 pop3s postgresql privoxy proxy-dhcp ptp pulseaudio puppetmaster quassel radius rpc-bind rsh rsyncd samba samba-client sane sip sips smtp smtp-submission smtps snmp snmptrap spideroak-lansync squid ssh synergy syslog syslog-tls telnet tftp tftp-client tinc tor-socks transmission-client vdsm vnc-server wbem-https xmpp-bosh xmpp-client xmpp-local xmpp-server
```

# MySQL

## 安装 mariadb

```shell
cd /mnt/Packages
rpm -ivh perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64.rpm
rpm -ivh perl-Compress-Raw-Zlib-2.061-4.el7.x86_64.rpm
rpm -ivh perl-DBD-MySQL-4.023-5.el7.x86_64.rpm
rpm -ivh perl-Data-Dumper-2.145-3.el7.x86_64.rpm
rpm -ivh perl-IO-Compress-2.061-2.el7.noarch.rpm
rpm -ivh perl-Net-Daemon-0.48-5.el7.noarch.rpm
rpm -ivh perl-PlRPC-0.2020-14.el7.noarch.rpm
rpm -ivh mariadb-server-5.5.56-2.el7.x86_64.rpm mariadb-5.5.56-2.el7.x86_64.rpm
```

### 配置文件

```shell
/etc/my.cnf
```

# Apache服务和LAMP

- 镜像下载地址[apache](https://mirrors.tuna.tsinghua.edu.cn/apache/)

## Apache服务的安装配置

### 安装OpenSSL

```shell
cd /root/install
wget --no-check-certificate https://www.openssl.org/source/old/1.0.1/openssl-1.0.1s.tar.gz
tar -xvf openssl-1.0.1s.tar.gz
cd openssl-1.0.1s
./config --prefix=/usr/local/ssl --shared
make
make install
echo /usr/local/ssl/lib/ >> /etc/ld.so.conf
ldconfig
```

### 安装Apache

```shell
# 安装apr
cd /root/install
wget https://archive.apache.org/dist/apr/apr-1.5.2.tar.gz
tar -xvf apr-1.5.2.tar.gz
cd apr-1.5.2
./configure --prefix=/usr/local/apr
make
make install
# 安装apr-util
cd /root/install
wget https://archive.apache.org/dist/apr/apr-util-1.5.4.tar.gz
tar -xvf apr-util-1.5.4.tar.gz
cd apr-util-1.5.4
./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
make
make install
# 安装 pcre
cd /root/install
wget --no-check-certificate  https://sourceforge.net/projects/pcre/files/pcre/8.38/pcre-8.38.tar.gz
tar -xvf pcre-8.38.tar.gz
cd pcre-8.38
./configure --prefix=/usr/local/pcre
make
make install
# 安装源码包
cd /root/install
wget http://archive.apache.org/dist/httpd/httpd-2.4.18.tar.gz
tar -xvf
cd httpd-2.4.18
./configure --prefix=/usr/local/apache2 \
--enable-so \
--enable-rewrite \
--enable-ssl \
--with-ssl=/usr/local/ssl \
--with-apr=/usr/local/apr \
--with-apr-util=/usr/local/apr-util \
--with-pcre=/usr/local/pcre
make
make install
```

**Apache是模块化的服务器，服务器中只包含常用的模块，扩展功能需要由其他模块提供**

**使用模块的方式**

- 方法1：静态编译二进制文件，
- 方法2：启用SSL加密和mod_rewrite，并且采用动态编译模式，需要启用mod_so。可以动态的添加模块，而不用重新编译。

### 配置httpd.conf

[httpd](https://www.cnblogs.com/hgzero/p/14136149.html#1.%20httpd)

![](C:/notebook/pictures/Snipaste_2022-12-10_00-57-06.png =800x)

```shell
vim /usr/local/apache2/conf/httpd.conf
# 打开以下开关
LoadModule authn_anon_module modules/mod_authn_anon.so
EnableMMAP on
EnableSendfile on
# 添加如下
<IfModule mpm_prefork_module>
        StartServers    5
        MinSpareServers 5
        MaxSpareServers 10
        ServerLimit     4000
        MaxClients      4000
        MaxRequestsPerChild     0
</IfModule>

<IfModule mpm_worker_module>
        StartServers    5
        ServerLimit     20
        ThreadLimit     200
        MaxClients      4000
        MinSpareThreads 25
        MaxSpareThreads 250
        ThreadsPerChild 200
        MaxRequestsPerChild     0
</IfModule>
```

![](c:/notebook/pictures/Snipaste_2022-12-10_10-05-01.png =700x)
![](c:/notebook/pictures/Snipaste_2022-12-10_10-05-49.png =700x)

### 虚拟主机配置

**基于IP、基于端口、基于域名3种方式**

- 基于IP的需要拥有多个IP
- 基于端口的只需要1个IP，或需要不同端口访问不同虚拟主机时使用
- 基于域名的可以在同一个IP上配置多个域名，并通过80端口访问

**切换虚拟主机配置方式**

- 将/usr/local/apache2/conf/httpd.conf中其他配置的Listen删除。

#### 基于IP

**有多个IP**

1. 绑定IP

```shell
ifconfig ens33:1 192.168.186.101
ifconfig ens33:2 192.168.186.102
ifconfig ens33:3 192.168.186.103
```

2. 修改主机/etc/hosts文件

```shell
vim /etc/hosts
echo -e "192.168.186.101 www.zjk01.com\n192.168.186.102 www.zjk02.com\n192.168.186.103 www.zjk03.com" >> /etc/hosts
```

3. 建立虚拟主机存放网页的根目录，并创建首页文件

```shell
mkdir /data/www
cd /data/www
mkdir 101
mkdir 102
mkdir 103
echo "192.168.186.101" > 101/index.html
echo "192.168.186.102" > 102/index.html
echo "192.168.186.103" > 103/index.html
```

4. 修改httpd.conf，添加

```shell
echo -e "Listen 192.168.186.101:80\nListen 192.168.186.102:80\nListen 192.168.186.103:80\nInclude conf/vhost/*.conf" >> /usr/local/apache2/conf/httpd.conf
```

5. 编辑每个IP的配置文件

```shell
mkdir /usr/local/apache2/conf/vhost
vim /usr/local/apache2/conf/vhost/www.zjk01.conf
# 编辑101
<VirtualHost 192.168.186.101>
    ServerName    www.zjk01.com
    DocumentRoot    /data/www/101
    <Directory "/data/www/101">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>

vim /usr/local/apache2/conf/vhost/www.zjk02.conf
# 编辑102
<VirtualHost 192.168.186.102>
    ServerName    www.zjk02.com
    DocumentRoot    /data/www/102
    <Directory "/data/www/102">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>

# 编辑103
vim /usr/local/apache2/conf/vhost/www.zjk03.conf
# 编辑103
<VirtualHost 192.168.186.103>
    ServerName    www.zjk03.com
    DocumentRoot    /data/www/103
    <Directory "/data/www/103">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

#### 基于端口 

**只有1个IP或需要不同端口访问不同的虚拟主机**

1. 绑定IP

```shell
ifconfig ens33:4 192.168.186.157
```

2. 修改主机/etc/hosts文件

```shell
vim /etc/hosts
echo -e "192.168.186.157 www.test.com" >> /etc/hosts
```

3. 建立虚拟主机存放网页的根目录，并创建首页文件

```shell
mkdir /data/port
cd /data/port
mkdir 7081
mkdir 8081
mkdir 9081
echo "port 7081" > 7081/index.html
echo "port 8081" > 8081/index.html
echo "port 9081" > 9081/index.html
```

4. 修改httpd.conf，添加

```shell
echo -e "Listen 192.168.186.157:7081\nListen 192.168.186.157:8081\nListen 192.168.186.157:9081" >> /usr/local/apache2/conf/httpd.conf
```

5. 编辑每个IP的配置文件

```shell
mkdir /usr/local/apache2/conf/vhost
vim /usr/local/apache2/conf/vhost/www.test.7081.conf
# 编辑7081
<VirtualHost 192.168.186.157:7081>
    ServerName    www.test.com
    DocumentRoot    /data/port/7081
    <Directory "/data/port/7081/">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>

vim /usr/local/apache2/conf/vhost/www.zjk04.8081.conf
# 编辑8081
<VirtualHost 192.168.186.157:8081>
    ServerName    www.test.com
    DocumentRoot    /data/port/8081
    <Directory "/data/port/8081/">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>

# 编辑9081
vim /usr/local/apache2/conf/vhost/www.zjk04.9081.conf
# 编辑9081
<VirtualHost 192.168.186.157:9081>
    ServerName    www.test.com
    DocumentRoot    /data/port/9081
    <Directory "/data/port/9081/">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

#### 基于域名

1. 绑定IP

```shell
ifconfig ens33:5 192.168.186.105
```

2. 修改主机/etc/hosts文件

```shell
vim /etc/hosts
echo -e "192.168.186.105 www.oa.com\n192.168.186.105 www.bbs.com\n192.168.186.105 www.test.com" >> /etc/hosts
```

3. 建立虚拟主机存放网页的根目录，并创建首页文件

```shell
mkdir /data/www
cd /data/www
mkdir www.oa.com
mkdir www.bbs.com
mkdir www.test.com
echo "www.oa.com" > www.oa.com/index.html
echo "www.bbs.com" > www.bbs.com/index.html
echo "www.test.com" > www.test.com/index.html
```

4. 修改httpd.conf，添加

```shell
echo -e "Listen 192.168.186.105:80" >> /usr/local/apache2/conf/httpd.conf
```

5. 编辑每个IP的配置文件

```shell
mkdir /usr/local/apache2/conf/vhost
vim /usr/local/apache2/conf/vhost/www.oa.com.conf
# 编辑www.oa.com
<VirtualHost 192.168.186.105:80>
    ServerName    www.oa.com
    DocumentRoot    /data/www/www.oa.com
    <Directory "/data/www/www.oa.com">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>

# 编辑www.bbs.com
vim /usr/local/apache2/conf/vhost/www.bbs.com.conf
# 编辑www.bbs.com
<VirtualHost 192.168.186.105:80>
    ServerName    www.bbs.com
    DocumentRoot    /data/www/www.bbs.com
    <Directory "/data/www/www.bbs.com">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>

# 编辑test.com
vim /usr/local/apache2/conf/vhost/www.test.com.conf
# 编辑www.test.com
<VirtualHost 192.168.186.105:80>
    ServerName    www.bbs.com
    DocumentRoot    /data/www/www.test.com
    <Directory "/data/www/www.test.com">
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

#### 在现有的Web服务器上增加虚拟主机

**为现存主机建造一个`<VirtualHost>`定义块**

### 启动Apache服务

```shell
/usr/local/apache2/bin/apachectl start
# 测试
curl http://www.zjk01.com
curl http://www.zjk02.com
curl http://www.zjk03.com
```

```shell
[root@localhost port]# curl http://www.test.com
<html><body><h1>It works!</h1></body></html>
```

## Apache安全控制与认证

### `<Diretory>`定义块 虚拟目录

```shell
<Diretory "目录路径">
    目录相关配置和命令
</Diretory>
```

- （虚拟主机配置文件）每个`<Diretory>`段作用于`<Directory>`中指定的目录及其里面所有文件和子目录。在段中可以设置与该指定目录相关的参数和指令，包括访问控制和认证。
- 控制命令：Require。
- 控制方法：基于IP地址、域名、http方法、用户等。

| 指令                            | 说明            |
| :----------------------------- | :-------------- |
| Require all granted            | 允许所有主机访问 |
| Require all denied             | 拒绝所有主机访问 |
| Require ip 192.168.186.156     | 允许指定主机访问 |
| Require ip 192.168.186.0/24    | 允许指定网络访问 |
| Require not ip 192.168.186.156 | 拒绝指定主机访问 |
| Require host www.test.com      | 允许指定域名访问 |
| Require not host www.test.com  | 拒绝指定域名访问 |

- 访问没有权限的的地址时：Forbidden ....

#### Apache认证

**基本认证和摘要认证**


##### 基本认证 (用户认证) 

###### /usr/local/apache2/bin/htpasswd 

**创建用户**

```shell
cd /usr/local/apache2/bin
# 创建口令文件，同时设置admin密码
./htpasswd -c /usr/local/apache2/conf/users.list admin
# 添加用户，同时设置密码 显示的是加密的密码
./htpasswd /usr/local/apache2/conf/users.list user1
```

**删除用户**

- 直接进入/usr/local/apache2/conf/users.list文件内删除相应内容

###### 修改虚拟主机配置文件

**指令**

```shell 
# AuthName指定使用认证的域名，该域会出现在用户的密码提问对话中。
AuthName 域名
# AuthType用于选择一个目录的用户认证类型：Basic基本认证/Digest摘要认证
AuthType Basic/Digest
# AuthUserFile用于设定一个纯文本，包含用于认证的用户名和密码的列表
AuthUserFile 文件名
# Require用于设置哪些认证用户允许访问指定的资源
Require user 用户名 [用户名2...] # 授权给指定用户
Require valid-user # 授权给所有用户
```

```shell
vim /usr/local/apache2/conf/vhost/www.zjk01.conf
# 修改：
<VirtualHost 192.168.186.101>
    ServerName    www.zjk01.com
    DocumentRoot    /data/www/101
    <Directory "/data/www/101">
        Options Indexes FollowSymLinks
        AllowOverride None
        AuthType Basic
        AuthName "auth"
        AuthFile /usr/local/apache2/conf/users.list
        Require user admin
    </Directory>
</VirtualHost>
```

###### .htaccess 分布式配置文件

**需要在虚拟主机配置文件中开启选项**

- AllowOverride All

1. 删除原有关于访问控制和用户认证的参数和指令，否则这些指令会被写入.htacess文件
2. 添加AllowOverride All选项，允许.htaccess文件覆盖httpd.conf文件中关于bm目录的配置，否则.htacess文件配置不能生效。

```shell
vim /usr/local/apache2/conf/vhost/www.zjk01.conf
# 修改：
<VirtualHost 192.168.186.101>
    ServerName    www.zjk01.com
    DocumentRoot    /data/www/101
    <Directory "/data/www/101">
        Options Indexes FollowSymLinks
        AllowOverride All
    </Directory>
</VirtualHost>
```

**重启Apache服务,在/data/www/www.zjk01.com**

```shell
AuthType Basic
    AuthName "auth"
    AuthFile /usr/local/apache2/conf/users.list
    Require user admin
```

# LVS 集群负载均衡

![](C:/notebook/pictures/Snipaste_2022-12-12_15-30-22.png =300x)

# 集群技术与双机热备


# KVM虚拟化

# OpenStack

# Hadoop

# Spark