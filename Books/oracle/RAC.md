## 解释

### 集群概念介绍

### RAC 工作原理和相关组件

OracleRAC 是多个单实例在配置意义上的扩展，实现由两个或者多个节点（实例）使用一个共同的共享数据库（例如，一个数据库同时安装多个实例并打开）。在这种情况下，每一个单独的实例有它自己的 cpu 和物理内存，也有自己的 SGA 和后台进程。和传统的 oracle 实例相比，在系统全局区（SYSTEM CLOBAL AREA,SGA）与后台进程有着显著的不同。最大的不同之处在于多了一个GRD,GRD内存块主要是记录此rac有多少个集群数据库与系统资源，同时也会记录数据块的相关信息，因为在 rac 架构中，每个数据块在每一个 SGA 中都有一份副本，而 rac 必须知道这些数据块的位置，版本，分布以及目前的状态，这些信息就存放在 GRD 中，但 GRD 只负责存放不负责管理，管理的责任则交给后台进程 GCS 和 GES 来进行。Oracle 的多个实例访问一个共同的共享数据库。每个实例都有自己的 SGA、PGA 和后台进程，这些后台进程应该是熟悉的，因为在 RAC 配置中，每个实例将需要这些后台进程运行支撑的。可以从以下几个方面了解 RAC工作原理和运行机制。

#### SCN

SCN 是 Oracle 用来跟踪数据库内部变化发生先后顺序的机制，可以把它想象成一个高精度的时钟，每个 Redo日志条目，Undo Data Block，Data Block 都会有 SCN 号。 Oracle 的Consistent-Read， Current-Read，Multiversion-Block 都是依赖 SCN 实现。在 RAC 中，有 GCS 负责全局维护 SCN 的产生，缺省用的是 Lamport SCN 生成算法，该算法大致原理是： 在所有节点间的通信内容中都携带 SCN， 每个节点把接收到的 SCN 和本机的 SCN 对比，如果本机的 SCN 小，则调整本机的 SCN 和接收的一致，如果节点间通信不多，还会主动地定期相互通报。 故即使节点处于 Idle 状态，还是会有一些 Redo log 产生。 还有一个广播算法（Broadcast），这个算法是在每个 Commit 操作之后，节点要想其他节点广播 SCN，虽然这种方式会对系统造成一定的负载，但是确保每个节点在 Commit 之后都能立即查看到 SCN.这两种算法各有优缺点，Lamport 虽然负载小，但是节点间会有延迟，广播虽然有负载，但是没有延迟。Oracle 10g RAC 缺省选用的是 BroadCast 算法，可以从 alert.log 日志中看到相关信息：Picked broadcast on commit scheme to generate SCNS

#### RAC 的 GES/GCS 原理

全局队列服务（GES）主要负责维护字典缓存和库缓存的一致性。字典缓存是实例的 SGA 内所存储的对数据字典信息的缓存，用于高速访问。由于该字典信息存储在内存中，因而在某个节点上对字典进行的修改（如DDL)必须立即被传播至所有节点上的字典缓存。GES 负责处理上述情况，并消除实例间出现的差异。处于同样的原因，为了分析影响这些对象的 SQL 语句，数据库内对象上的库缓存锁会被去掉。这些锁必须在实例间进行维护，而全局队列服务必须确保请求访问相同对象的多个实例间不会出现死锁。LMON、LCK 和 LMD 进程联合工作来实现全局队列服务的功能。GES 是除了数据块本身的维护和管理（由 GCS 完成）之外，在 RAC 环境中调节节点间其他资源的重要服务。为了保证集群中的实例的同步，两个虚拟服务将被实现：全局排队服务（GES），它负责控制对锁的访问。

全局内存服务（GCS)，控制对数据块的访问。GES 是 分布式锁管理器(DLM)的扩展，它是这样一个机制，可以用来管理 oracle 并行服务器的锁和数据块。在一个群集环境中，你需要限制对数据库资源的访问，这些资源在单 instance 数据库中被 latches 或者 locks 来保护。比如说，在数据库字典内存中的对象都被隐性锁所保护，而在库高速缓存中的对象在被引用的时候，必须被 pin 所保护。在 RAC 群集中，这些对象代表了被全局锁所保护的资源。GES 是一个完整的 RAC 组件，它负责和群集中的实例全局锁进行沟通，每个资源有一个主节点实例，这个实例记录了它当前的状态。而且，资源的当前的状态也记录在所有对这个资源有兴趣的实例上。GCS,是另一个 RAC 组件，负责协调不同实例间对数据块的访问。对这些数据块的访问以及跟新都记录在全局目录中（GRD）,这个全局目录是一个虚拟的内存结构，在所有的实例中使用扩张。每个块都有一个master实例，这个实例负责对GSD的访问进行管理，GSD里记录了这个块的当前状态信息。GCS 是 oracle 用来实施 Cache fusion 的机制。被 GCS 和 GES 管理的块和锁叫做资源。对这些资源的访问必须在群集的多个实例中进行协调。这个协调在实例层面和数据库层面都有发生。实例层次的资源协调叫做本地资源协调；数据库层次的协调叫做全局资源协调。

本地资源协调的机制和单实例 oracle 的资源协调机制类似，包含有块级别的访问，空间管理，dictionary cache、library cache 管理，行级锁，SCN 发生。全局资源协调是针对 RAC 的，使用了 SGA 中额外的内存组件、算法和后台进程。GCS 和 GES 从设计上就是在对应用透明的情况下设计的。换一句话来说，你不需要因为数据库是在 RAC上运行而修改应用,在单实例的数据库上的并行机制在 RAC 上也是可靠地。

支持 GCS 和 GES 的后台进程使用私网心跳来做实例之间的通讯。这个网络也被 Oracle 的 群集组件使用，也有可能被 群集文件系统（比如 OCFS)所使用。GCS 和 GES 独立于 Oracle 群集组件而运行。但是，GCS 和GES 依靠 这些群集组件获得群集中每个实例的状态。如果这些信息不能从某个实例获得，这个实例将被关闭。这个关闭操作的目的是保护数据库的完整性，因为每个实例需要知道其他实例的情况，这样可以更好的确定对数据库的协调访问。GES 控制数据库中所有的 library cache 锁和 dictionary cache 锁。这些资源在单实例数据库中是本地性的，但是到了 RAC 群集中变成了全局资源。全局锁也被用来保护数据的结构，进行事务的管理。一般说来，事务和表锁 在 RAC 环境或是 单实例环境中是一致的。

Oracle 的各个层次使用相同的 GES 功能来获得，转化以及释放资源。在数据库启动的时候，全局队列的个数将被自动计算。GES 使用后台进程 LMD0 和 LCK0 来执行它的绝大多数活动。一般来说，各种进程和本地的 LMD0 后台进程沟通来管理全局资源。本地的 LMD0 后台进程与 别的实例上的 LMD0 进程进行沟通。

LCK0 后台进程用来获得整个实例需要的锁。比如，LCK0 进程负责维护 dictionary cache 锁。影子进程(服务进程） 与这些后台进程通过 AST(异步陷阱）消息来通信。异步消息被用来避免后台进程的阻塞，这些后台进程在等待远端实例的的回复的时候将阻塞。后台进程也能 发送 BAST(异步锁陷阱）来锁定进程，这样可以要求这些进程把当前的持有锁置为较低级限制的模式。资源是内存结构，这些结构代表了数据库中的组件，对这些组件的访问必须为限制模式或者串行化模式。换一句话说，这个资源只能被一个进程或者一直实例并行访问。如果这个资源当前是处于使用状态，其他想访问这个资源的进程必须在队列中等待，直到资源变得可用。队列是内存结构，它负责并行化对特殊资源的访问。如果这些资源只被本地实例需求，那么这个队列可以本地来获得，而且不需要协同。但是如果这个资源被远程实例所请求，那么本地队列必须变成全球化。

### ClusterWare 架构

在单机环境下，Oracle 是运行在 OS Kernel 之上的。 OS Kernel 负责管理硬件设备，并提供硬件访问接口。Oracle 不会直接操作硬件，而是有 OS Kernel 代替它来完成对硬件的调用请求。在集群环境下， 存储设备是共享的。OS Kernel 的设计都是针对单机的，只能控制单机上多个进程间的访问。 如果还依赖 OS Kernel 的服务，就无法保证多个主机间的协调工作。 这时就需要引入额外的控制机制，在RAC 中，这个机制就是位于 Oracle 和 OS Kernel 之间的 Clusterware，它会在 OS Kernel 之前截获请求，然后和其他结点上的 Clusterware 协商，最终完成上层的请求。在 Oracle 10G 之前，RAC 所需要的集群件依赖与硬件厂商，比如 SUN,HP,Veritas. 从 Oracle 10.1版本中，Oracle推出了自己的集群产品. Cluster Ready Service(CRS),从此 RAC 不在依赖与任何厂商的集群软件。 在 Oracle 10.2版本中，这个产品改名为：Oracle Clusterware。所以我们可以看出， 在整个 RAC 集群中，实际上有 2 个集群环境的存在，一个是由 Clusterware 软件组成的集群，另一个是由 Database 组成的集群。

(一) Clusterware 的主要进程

1. CRSD——负责集群的高可用操作，管理的 crs 资源包括数据库、实例、监听、虚拟 IP，ons，gds 或者其他，操作包括启动、关闭、监控及故障切换。改进程由 root 用户管理和启动。crsd 如果有故障会导致系统重启。

2. cssd，管理各节点的关系，用于节点间通信，节点在加入或离开集群时通知集群。该进程由 oracle 用户运行管理。发生故障时 cssd 也会自动重启系统。

3. oprocd – 集群进程管理 —Process monitor for the cluster. 用于保护共享数据 IO fencing。

4. 仅在没有使用 vendor 的集群软件状态下运行

5. evmd ：事件检测进程，由 oracle 用户运行管理

 Cluster Ready Service（CRS，集群准备服务）是管理集群内高可用操作的基本程序。Crs 管理的任何事物被称之为资源，它们可以是一个数据库、一个实例、一个监听、一个虚拟 IP（VIP）地址、一个应用进程等等。CRS是根据存储于 OCR 中的资源配置信息来管理这些资源的。这包括启动、关闭、监控及故障切换（start、stop、monitor 及 failover）操作。当一资源的状态改变时，CRS 进程生成一个事件。当你安装 RAC 时，CRS 进程监控Oracle 的实例、监听等等，并在故障发生时自动启动这些组件。默认情况下，CRS 进程会进行 5 次重启操作，如果资源仍然无法启动则不再尝试。Event Management（EVM）：发布 CRS 创建事件的后台进程。Oracle Notification Service（ONS）：通信的快速应用通知（FAN：Fast Application Notification）事件的发布及订阅服务。RACG：为 clusterware 进行功能扩展以支持 Oracle 的特定需求及复杂资源。它在 FAN 事件发生时执行服务器端的调用脚本（server callout script）Process Monitor Daemon（OPROCD）：此进程被锁定在内存中，用于监控集群（cluster）及提供 I/O 防护（I/Ofencing）。OPROCD 执行它的检查，停止运行，且如果唤醒超过它所希望的间隔时，OPROCD 重置处理器及重启节点。一个 OPROCD 故障将导致 Clusterware 重启节点。

Cluster Synchronization Service（CSS）：CSS 集群同步服务，管理集群配置，谁是成员、谁来、谁走，通知成员，是集群环境中进程间通信的基础。同样，CSS 也可以用于在单实例环境中处理 ASM 实例与常规 RDBMS 实例之间的交互作用。在集群环境中，CSS 还提供了组服务，即关于在任意给定时间内哪些节点和实例构成集群的动态信息，以及诸如节点的名称和节点静态信息（这些信息在节点被加入或者移除时被修改）。CSS 维护集群内的基本锁功能（尽管大多数锁有 RDBMS 内部的集成分布式锁管理来维护）。除了执行其他作业外，CSS 还负责在集群内节点间维持一个心跳的程序，并监控投票磁盘的 split-brain 故障。在安装 clusterware 的最后阶段，会要求在每个节点执行 root.sh 脚本，这个脚本会在/etc/inittab 文件的最后把这 3 个进程加入启动项，这样以后每次系统启动时，clusterware 也会自动启动，其中 EVMD 和 CRSD 两个进程如果出现异常，则系统会自动重启这两个进程，如果是 CSSD 进程异常，系统会立即重启。

注意：

1. Voting Disk 和 OCR 必须保存在存储设备上供各个节点访问。

2. Voting Disk、OCR 和网络是安装的过程中或者安装前必须要指定或者配置的。安装完成后可以通过一些工具进行配置和修改。

## RAC配置 Linux

### 网卡

```
vim /etc/sysconfig/network-scripts/ifcfg-eth0
vim /etc/sysconfig/network-scripts/ifcfg-eth1

service network restart
```

#### RAC1

##### NAT网卡 eth0

```
DEVICE=eth0
HWADDR=00:50:56:2D:B0:BE
TYPE=Ethernet
UUID=d75da050-2a7c-4c1a-877d-103596eff910
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
GATEWAY=192.168.186.2
IPADDR=192.168.186.129
NETMASK=255.255.255.0
```

##### 主机 eth1

```
DEVICE=eth1
HWADDR=00:50:56:20:DF:5E
TYPE=Ethernet
UUID=d75da050-2a7c-4c1a-877d-103596eff910
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
#GATEWAY=192.168.186.2
IPADDR=192.168.26.129
NETMASK=255.255.255.0


vim /etc/sysconfig/network-scripts/ifcfg-eth0
vim /etc/sysconfig/network-scripts/ifcfg-eth1

service network restart
```

#### RAC2

##### NAT网卡 eth0

```
DEVICE=eth0
HWADDR=00:50:56:26:EE:7E
TYPE=Ethernet
UUID=d75da050-2a7c-4c1a-877d-103596eff910
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
GATEWAY=192.168.186.2
IPADDR=192.168.186.130
NETMASK=255.255.255.0
```

##### 主机 eth1

```
DEVICE=eth1
HWADDR=00:50:56:37:8E:9E
TYPE=Ethernet
UUID=d75da050-2a7c-4c1a-877d-103596eff910
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
#GATEWAY=192.168.186.2
IPADDR=192.168.26.130
NETMASK=255.255.255.0
```

#### 测试

```
ping 192.168.186.129
ping 192.168.186.130
```

### 关闭防火墙

#### 重启后生效 一直关闭

##### 开启

chkconfig iptables on

##### 关闭

chkconfig iptables off

#### 即时生效 下次重启重新打开

##### 开启

service iptables start

##### 关闭

service iptables stop

### 关闭selinux

```
vim /etc/selinux/config   
修改SELINUX=disabled   

检查    
 /usr/sbin/sestatus
```

### HOST文件配置

#### vim /etc/sysconfig/network 修改

##### rac1

```
NETWORKING=yes
HOSTNAME=rac1
#GATEWAY=192.168.186.2
```

##### rac2

```
NETWORKING=yes
HOSTNAME=rac2
#GATEWAY=192.168.186.2
```

#### vim /etc/hosts

节点1和节点2相同

```
#public
192.168.186.129  rac1
192.168.186.130  rac2

#virtual
192.168.186.131  rac1-vip
192.168.186.132  rac2-vip

#private
192.168.26.129  rac1-priv
192.168.26.130  rac2-priv

#scan
192.168.186.150 rac-scan

private ip怎么能和public ip同一个网段？
vip怎么能和public ip不同网段？
```

##### rac1 rac2 的 /ect/hosts文件修改

```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
#::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

#public
192.168.186.129  rac1
192.168.186.130  rac2
#virtual
192.168.186.131  rac1-vip
192.168.186.132  rac2-vip
#private
192.168.26.129  rac1-priv  
192.168.26.130  rac2-priv 
#scan
192.168.186.150  rac-scan
```

#### 检查

```
reboot 重启

hostname 查看主机名
是否为rac1 rac2

more /etc/hosts
```

### 生成共享磁盘

```
E:\vmWare
注 VWware安装路径 

注 -c -s 2g   2g--容量大小
   -t 2       2 --冗余

先新建文件夹 D:\rac\rac-sharedisk
在该路径下执行以下命令
vmware-vdiskmanager.exe -c -s 2g -a lsilogic -t 2 D:\rac\rac-sharedisk\orc_voting_disk.vmdk
注 存放集群资源
vmware-vdiskmanager.exe -c -s 5g -a lsilogic -t 2 D:\rac\rac-sharedisk\fra_arc_disk.vmdk
注 归档
vmware-vdiskmanager.exe -c -s 5g -a lsilogic -t 2 D:\rac\rac-sharedisk\data_disk.vmdk
注 数据库
```

### 编辑虚拟机配置文件
  
#### 关闭虚拟机

#### 共享文件夹 D:/rac/rac-sharedisk

#### 记事本打开 `虚拟机名字.wmx` ,追加以下内容

```
disk.locking = "FALSE"
diskLib.dataCacheMaxSize = "0"
diskLib.dataCacheMaxReadAheadSize = "0"
diskLib.dataCacheMinReadAheadSize = "0"
diskLib.maxUnsyncedWrites = "0"

scsi1.present = "TRUE"
scsi1.virtualDev = "lsilogic"
scsil.sharedBus = "VIRTUAL"

scsi1:0.present = "TRUE"
scsi1:0.mode = "independent-persistent"
scsi1:0.fileName = "D:\rac\rac-sharedisk\ocr_voting_disk.vmdk"
scsi1:0.deviceType = "disk"
scsi1:0.redo = ""

scsi1:1.present = "TRUE"
scsi1:1.mode = "independent-persistent"
scsi1:1.fileName = "D:\rac\rac-sharedisk\data_disk.vmdk"
scsi1:1.deviceType = "disk"
scsi1:1.redo = ""

scsi1:2.present = "TRUE"
scsi1:2.mode = "independent-persistent"
scsi1:2.fileName = "D:\rac\rac-sharedisk\fra_arc_disk.vmdk"
scsi1:2.deviceType = "disk"
scsi1:2.redo = ""
```

#### 再次追加

在以下后面追加

```
scsi1.pciSlotNumber = "38"
usb:0.present = "TRUE"
usb:0.deviceType = "hid"
usb:0.port = "0"
usb:0.parent = "-1"
isolation.tools.hgfs.disable = "FALSE"
sharedFolder0.present = "TRUE"
sharedFolder0.enabled = "TRUE"
sharedFolder0.readAccess = "TRUE"
sharedFolder0.writeAccess = "TRUE"
sharedFolder0.hostPath = "D:\rac\rac-sharedisk"
sharedFolder0.guestName = "rac-sharedisk"
sharedFolder0.expiration = "never"
sharedFolder.maxNum = "1"
```

追加内容同之前第一次追加

### 配置用户/组/目录/权限

```
groupadd oinstall
groupadd dba
groupadd oper
groupadd asmadmin
groupadd asmdba
groupadd asmoper
useradd -g oinstall -G dba,asmdba,asmadmin,asmoper grid
useradd -g oinstall -G dba,oper,asmdba oracle
echo -n oracle|passwd --stdin grid
echo -n oracle|passwd --stdin oracle
mkdir -p /u01/app/11.2.0/grid
mkdir -p /u01/app/grid
mkdir -p /u01/app/oracle
chown grid:oinstall /u01/app/11.2.0/grid
chown grid:oinstall /u01/app/grid
chown -R oracle:oinstall /u01/app/oracle
chmod -R 775 /u01/
chown -R grid:oinstall /u01
```

### 修改系统内核参数

```
sed -i 's/kernel.shmmax/#kernel.shmmax/g' /etc/sysctl.conf
sed -i 's/kernel.shmall/#kernel.shmall/g' /etc/sysctl.conf

cat >> /etc/sysctl.conf << EOF
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 1073741824
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048586
EOF

#提交内核参数
sysctl -p
```

### 两节点资源限制

```
cat >> /etc/security/limits.conf << EOF
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
oracle soft stack 10240
grid soft nproc 2047
grid hard nproc 16384
grid soft nofile 1024
grid hard nofile 65536
grid soft stack 10240
EOF
```

### /etc/profile配置

```
cat >> /etc/profile << EOF
if [ \$USER = "oracle" ] || [ \$USER = "grid" ];then
if [ \$SHELL = "/bin/ksh" ];then
ulimit -p 16384
ulimit -n 65536
else
ulimit -u 16384 -n 65536
fi
umask 022
fi
################################
export PATH=\$PATH:/u01/app/11.2.0/grid/bin
#color of grep
alias grep='grep --color=auto'
EOF
```

### 配置NTP服务 (可选)

或者VMware的 将客服机时间与主机同步

#### 查看安装

```
rpm -qa ntp
```

#### 启动服务(双节点执行)

```
service ntpd restart
```

#### 系统自加载(双节点执行)

```
chkconfig ntpd on
```

#### 查看运行状态

```
ntpq -p
```

### 下载安装ASMlib

#### 查看内核

```
uname -r

2.6.32-358.el6.x86_64
```

#### 安装

```
mkdir oracleasm

rpm -ivh kmod-oracleasm-2.0.6.rh1-2.el6.x86_64.rpm
rpm -ivh oracleasm-support-2.1.8-1.el6.x86_64.rpm    ---waring 
rpm -ivh oracleasmlib-2.0.4-1.el6.x86_64.rpm

检查安装
rpm -qa | grep oracleasm
有以下三个:
kmod-oracleasm-2.0.6.rh1-2.el6.x86_64
oracleasmlib-2.0.4-1.el6.x86_64
oracleasm-support-2.1.8-1.el6.x86_64

cd /lib/modules/;ll
有：
total 4
drwxr-xr-x. 8 root root 4096 Jun 23 23:23 2.6.32-358.el6.x86_64


```

#### 配置ASM磁盘

##### 初始化(两个节点执行) rac1 rac2

```
1.
oracleasm configure -i
···
Configuring the Oracle ASM library driver.

This will configure the on-boot properties of the Oracle ASM library
driver.  The following questions will determine whether the driver is
loaded on boot and what permissions it will have.  The current values
will be shown in brackets ('[]').  Hitting <ENTER> without typing an
answer will keep that current value.  Ctrl-C will abort.

Default user to own the driver interface []: grid
Default group to own the driver interface []: oinstall
Start Oracle ASM library driver on boot (y/n) [n]: y
Scan for Oracle ASM disks on boot (y/n) [y]: y
Writing Oracle ASM library driver configuration: done
···
2.
oracleasm init


检查：
cd /dev/oracleasm/; ll
如下：
total 0
drwxr-xr-x 1 root root     0 Jun 23 23:38 disks
drwxrwx--- 1 grid oinstall 0 Jun 23 23:38 iid

```

##### 磁盘分区(节点1执行即可) rac1

```
查看:
fdisk -l

ll /dev/sd*
```

```
fdisk /dev/sdb
如下：
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x8c01abc9.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
         switch off the mode (command 'c') and change display units to
         sectors (command 'u').

Command (m for help): n
Command action
   e   extended
   p   primary partition (1-4)
p
Partition number (1-4): 1
First cylinder (1-261, default 1): 
Using default value 1
Last cylinder, +cylinders or +size{K,M,G} (1-261, default 261): 
Using default value 261

Command (m for help): 
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

```
fdisk /dev/sdc
fdisk /dev/sdd
同上
```

```
检查：
ll /dev/sd*

brw-rw---- 1 root disk 8,  0 Jun 23 22:13 /dev/sda
brw-rw---- 1 root disk 8,  1 Jun 23 22:13 /dev/sda1
brw-rw---- 1 root disk 8,  2 Jun 23 22:13 /dev/sda2
brw-rw---- 1 root disk 8, 16 Jun 23 23:45 /dev/sdb
brw-rw---- 1 root disk 8, 17 Jun 23 23:45 /dev/sdb1
brw-rw---- 1 root disk 8, 32 Jun 23 23:47 /dev/sdc
brw-rw---- 1 root disk 8, 33 Jun 23 23:47 /dev/sdc1
brw-rw---- 1 root disk 8, 48 Jun 23 23:48 /dev/sdd
brw-rw---- 1 root disk 8, 49 Jun 23 23:48 /dev/sdd1
```

#### 创建磁盘(节点1执行即可) rac1

```
oracleasm createdisk OCR_VOTE /dev/sdb1
oracleasm createdisk DATA /dev/sdc1
oracleasm createdisk ARC /dev/sdd1
```

#### 扫描ASM磁盘

```
节点1：rac1
oracleasm scandisks
oracleasm listdisks
结果:
[root@rac1 ~]# oracleasm scandisks
Reloading disk partitions: done
Cleaning any stale ASM disks...
Scanning system for ASM disks...
[root@rac1 ~]# oracleasm listdisks
ARC
DATA
OCR_VOTE

节点2：rac2
oracleasm scandisks
oracleasm listdisks
```

### 配置用户环境变量(对应用户下执行)

#### grid用户

```
su - grid
```

##### 节点1 rac1

```
cat >> /home/grid/.bash_profile <<EOF
export TMP=/tmp;
export TMPDIR=\$TMP;
export ORACLE_HOSTNAME=rac1;
export ORACLE_SID=+ASM1;
export ORACLE_BASE=/u01/app/grid;
export ORACLE_HOME=/u01/app/11.2.0/grid;
export NLS_DATE_FORMAT="yy-mm-dd HH24:MI:SS";
export PATH=\$ORACLE_HOME/bin:\$PATH;
export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK;
EOF
```

```
source .bash_profile

检查：
env | grep ORA
结果：
ORACLE_SID=+ASM1
ORACLE_BASE=/u01/app/grid
ORACLE_HOSTNAME=rac1
ORACLE_HOME=/u01/app/11.2.0/grid

```

##### 节点2 rac2

```
cat >> /home/grid/.bash_profile <<EOF
export TMP=/tmp;
export TMPDIR=\$TMP;
export ORACLE_HOSTNAME=rac2;
export ORACLE_SID=+ASM2;
export ORACLE_BASE=/u01/app/grid;
export ORACLE_HOME=/u01/app/11.2.0/grid;
export NLS_DATE_FORMAT="yy-mm-dd HH24:MI:SS";
export PATH=\$ORACLE_HOME/bin:\$PATH;
export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK;
EOF
```

```
source .bash_profile

检查：
env | grep ORA
结果：

ORACLE_SID=+ASM2
ORACLE_BASE=/u01/app/grid
ORACLE_HOSTNAME=rac2
ORACLE_HOME=/u01/app/11.2.0/grid

```

#### oracle用户

```
su -oracle
oracle
```

##### 节点1 rac1

```
cat >> /home/oracle/.bash_profile <<EOF
export TMP=/tmp;
export TMPDIR=\$TMP;
export ORACLE_HOSTNAME=rac1;
export ORACLE_BASE=/u01/app/oracle;
export ORACLE_HOME=\$ORACLE_BASE/product/11.2.0/db_1;
export ORACLE_UNQNAME=prod;
export ORACLE_SID=prod1;
export ORACLE_TERM=xterm;
export PATH=/usr/sbin:\$PATH;
export PATH=\$ORACLE_HOME/bin:\$PATH;
export LD_LIBRARY_PATH=\$ORACLE_HOME/lib:/lib:/usr/lib;
export CLASSPATH=\$ORACLE_HOME/JRE:\$ORACLE_HOME/jlib:\$ORACLE_HOME/rdbms/jlib;
export NLS_DATE_FORMAT="yyyy-mm-dd HH24:MI:SS";
export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK;
EOF
```

```
source .bash_profile
```

##### 节点2 rac2

```
cat >> /home/oracle/.bash_profile <<EOF
export TMP=/tmp;
export TMPDIR=\$TMP;
export ORACLE_HOSTNAME=rac2;
export ORACLE_BASE=/u01/app/oracle;
export ORACLE_HOME=\$ORACLE_BASE/product/11.2.0/db_1;
export ORACLE_UNQNAME=prod;
export ORACLE_SID=prod2;
export ORACLE_TERM=xterm;
export PATH=/usr/sbin:\$PATH;
export PATH=\$ORACLE_HOME/bin:\$PATH;
export LD_LIBRARY_PATH=\$ORACLE_HOME/lib:/lib:/usr/lib;
export CLASSPATH=\$ORACLE_HOME/JRE:\$ORACLE_HOME/jlib:\$ORACLE_HOME/rdbms/jlib;
export NLS_DATE_FORMAT="yyyy-mm-dd HH24:MI:SS";
export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK;
EOF
```

```
source .bash_profile
```

##### 检查

```
[oracle@rac2 ~]$ env | grep ORA
ORACLE_UNQNAME=prod
ORACLE_SID=prod2
ORACLE_BASE=/u01/app/oracle
ORACLE_HOSTNAME=rac2
ORACLE_TERM=xterm
ORACLE_HOME=/u01/app/oracle/product/11.2.0/db_1
```

### 等效性 互性

出错就查看

- /etc/hosts是否有误
- 网卡配置

#### grid用户等效性

```
su - grid
oracle
```

```
rac1
ssh-keygen -t rsa

ssh-keygen -t dsa

rac2
ssh-keygen -t rsa

ssh-keygen -t dsa

以上默认 一路回车
```

```
rac1
cat ~/.ssh/*.pub >> ~/.ssh/authorized_keys
ssh grid@rac2 cat ~/.ssh/*.pub >> ~/.ssh/authorized_keys

rac2
cat ~/.ssh/*.pub >> ~/.ssh/authorized_keys
ssh grid@rac1 cat ~/.ssh/*.pub >> ~/.ssh/authorized_keys
```

```
建立等效性
rac1 rac2 双节点执行
ssh rac1 date

ssh rac1-priv date

ssh rac2 date

ssh rac2-priv date
```

#### oracle用户等效性

```
rac1
ssh-keygen -t rsa
ssh-keygen -t dsa

rac2
ssh-keygen -t rsa
ssh-keygen -t dsa
 
以上默认 一路回车
```

```
建立等效性
rac1 rac2 双节点执行
ssh rac1 date
ssh rac1-priv date
ssh rac2 date
ssh rac2-priv date
```

