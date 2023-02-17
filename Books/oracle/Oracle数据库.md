# 安装

## Linux

[参考安装](https://www.cnblogs.com/mmzs/p/9033112.html)

**RHEL74**

```shell
# 先准备oracle11g 压缩包
mkdir /root/install
cd /root/install

# 解压缩
unzip linux.x64_11gR2_database_1of2.zip
unzip linux.x64_11gR2_database_2of2.zip

# 将解压后的文件夹直接移动到/software目录下：
mv database /software/database
cd /software

# 关闭selinux
vim /etc/selinux/config
# 设置SELINUX=disabled
setenforce 0

# 关闭防火墙
systemctl restart firewalld.service
systemctl list-unit-files|grep firewalld.service
systemctl disable firewalld.service

# 安装所需的安装包
#gcc 
#make 
#binutils 
#gcc-c++ 
#compat-libstdc++-33 
#libstdc++-devel
#libstdc++ 
#elfutils-libelf-devel 
#elfutils-libelf-devel-static 
#elfutils-libelf
#libgcc
#zlib-devel
#compat-libcap1
#libXext
#libXtst
#libX11
#libXau
#libxcb
#libXi
#ksh 
#libaio 
#libaio-devel 
#numactl-devel 
#sysstat 
#unixODBC 
#unixODBC-devel 
#pcre-devel

# rpm安装依赖
# 已经有的 make libaio libaio-devel libXext libXtst libX11 libXau libxcb libXi
# 挂载
mkdir /mnt/cdrom
mount -t iso9660 /dev/cdrom /mnt/cdrom
cd /mnt/cdrom/Packages
# gcc libmpc cpp kernel glibc-header gblic-devel
rpm -ivh libmpc-1.0.1-3.el7.x86_64.rpm
rpm -ivh cpp-4.8.5-16.el7.x86_64.rpm
rpm -ivh kernel-headers-3.10.0-693.el7.x86_64.rpm
rpm -ivh glibc-headers-2.17-196.el7.x86_64.rpm
rpm -ivh glibc-devel-2.17-196.el7.x86_64.rpm
rpm -ivh gcc-4.8.5-16.el7.x86_64.rpm
# binutils binutils-devel zlib
rpm -ivh zlib-devel-1.2.7-17.el7.x86_64.rpm
rpm -ivh binutils-devel-2.25.1-31.base.el7.x86_64.rpm
rpm -ivh binutils-2.25.1-31.base.el7.x86_64.rpm
# gcc-c++ libstdc++-devel libstdc++
rpm -ivh libstdc++-devel-4.8.5-16.el7.x86_64.rpm
rpm -ivh gcc-c++-4.8.5-16.el7.x86_64.rpm
rpm -ivh libstdc++-4.8.5-16.el7.x86_64.rpm
# compat-libstdc++-33 下载安装 
mkdir /root/mytempfile
cd /root/mytempfile
rpm -ivh compat-libstdc++-33-3.2.3-72.el7.x86_64.rpm
# elfutils-libelf-devel elfutils-libelf-devel-static 下载以下0.168 
rpm -ivh elfutils-libelf-devel-0.168-8.el7.x86_64.rpm
rpm -ivh elfutils-libelf-devel-static-0.168-8.el7.x86_64.rpm
# ksh 下载http://mirror.centos.org/centos/7/updates/x86_64/Packages/ksh-20120801-144.el7_9.x86_64.rpm
rpm -ivh ksh-20120801-144.el7_9.x86_64.rpm
cd /mnt/cdrom/Packages
rpm -ivh libaio-devel-0.3.109-13.el7.x86_64.rpm
# numactl-devel
rpm -ivh numactl-devel-2.0.9-6.el7_2.x86_64.rpm
# sysstat
# unixODBC
rpm -ivh unixODBC-2.3.1-11.el7.x86_64.rpm
# unixODBC-devel
rpm -ivh unixODBC-devel-2.3.1-11.el7.x86_64.rpm
# pcre-devel
rpm -ivh pcre-devel-8.32-17.el7.x86_64.rpm

cd /software 

# 添加安装用户和用户组
groupadd oinstall
groupadd dba
useradd -g oinstall -G dba oracle
passwd oracle
#[root@bogon software]# id oracle
#uid=1001(oracle) gid=1001(oinstall) groups=1001(oinstall),1002(dba)

# 修改内核参数配置文件
vim /etc/sysctl.conf
#在末尾添加以下内容：
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax =1073741824 #2147483648 本机物理内存的一半 Byte
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
#检查
sysctl -p 

# 修改用户的限制文件
vim /etc/security/limits.conf
#在末尾添加以下内容：
oracle           soft    nproc           2047
oracle           hard    nproc           16384
oracle           soft    nofile          1024
oracle           hard    nofile         65536
oracle           soft    stack           10240 

vim /etc/pam.d/login
#在末尾添加以下内容：
session required  /lib64/security/pam_limits.so
session required   pam_limits.so 

vim /etc/profile

#在末尾添加以下内容：
#oracle配置
if [ $USER = "oracle" ]; then
  if [ $SHELL = "/bin/ksh" ]; then
      ulimit -p 16384
      ulimit -n 65536
  else
      ulimit -u 16384 -n 65536
  fi
fi 

# 创建安装目录和设置文件权限
mkdir -p /data/oracle/product/11.2.0
mkdir /data/oracle/oradata
mkdir /data/oracle/inventory
mkdir /data/oracle/fast_recovery_area
chown -R oracle:oinstall /data/oracle
chmod -R 775 /data/oracle

## 以下是oracle用户操作
# 设置oracle用户环境变量 
su -l oracle
vim .bash_profile
#在末尾添加如下内容：
ORACLE_BASE=/data/oracle
ORACLE_HOME=$ORACLE_BASE/product/11.2.0
ORACLE_SID=orcl #此处必须与创建的数据库实例名称一致，否则数据库启动后无法访问。
PATH=$PATH:$ORACLE_HOME/bin
export ORACLE_BASE ORACLE_HOME ORACLE_SID PATH

# 使可用
source .bash_profile

# 编辑静默安装响应文件
#复制安装文件夹response到当前oracle用户的家目录下：
# 先去检查解压的文件是否在/software/database里，而不是/software，（root新建/software/database，并移到到目录）
cp -R /software/database/response/ .
cd response/
vim db_install.rsp
#需要设置的选项如下：
oracle.install.option=INSTALL_DB_SWONLY
ORACLE_HOSTNAME=CentOS # 有问题
UNIX_GROUP_NAME=oinstall
INVENTORY_LOCATION=/data/oracle/inventory
SELECTED_LANGUAGES=en,zh_CN
ORACLE_HOME=/data/oracle/product/11.2.0
ORACLE_BASE=/data/oracle
oracle.install.db.InstallEdition=EE
oracle.install.db.DBA_GROUP=dba
oracle.install.db.OPER_GROUP=dba
DECLINE_SECURITY_UPDATES=true

# 根据响应文件静默安装Oracle11g
cd /software/database/
./runInstaller -silent -responseFile /home/oracle/response/db_install.rsp -ignorePrereq
#出现以下
#[WARNING] [INS-32055]
#问题不大继续回车。。。
#出现以下则为成功
To execute the configuration scripts:
         1. Open a terminal window 
         2. Log in as "root" 
         3. Run the scripts 
         4. Return to this window and hit "Enter" key to continue 

Successfully Setup Software.
# 打开终端，退出到root身份登录，执行脚本：
#oracle退出 exit
exit
# 换回root操作
sh /data/oracle/inventory/orainstRoot.sh
#以下成功
Changing permissions of /data/oracle/inventory.
Adding read,write permissions for group.
Removing read,write,execute permissions for world.

Changing groupname of /data/oracle/inventory to oinstall.
The execution of the script is complete.
# 继续
sh /data/oracle/product/11.2.0/root.sh
#以下成功
Check /data/oracle/product/11.2.0/install/root_iZ2f570bi1k56uZ_2018-05-13_14-25-04.log for the output of root script

# 以静默方式配置监听
# 换回oracle操作
netca /silent /responseFile /home/oracle/response/netca.rsp 
#必须使用/silent /responseFile格式，而不是-silent -responseFile，因为是静默安装
#等待。。
[oracle@bogon ~]$ netca /silent /responseFile /home/oracle/response/netca.rsp

Parsing command line arguments:
    Parameter "silent" = true
    Parameter "responsefile" = /home/oracle/response/netca.rsp
Done parsing command line arguments.
Oracle Net Services Configuration:
Profile configuration complete.
Oracle Net Listener Startup:
    Running Listener Control: 
      /data/oracle/product/11.2.0/bin/lsnrctl start LISTENER
    Listener Control complete.
    Listener started successfully.
Listener configuration complete.
Oracle Net Services configuration successful. The exit code is 0
#成功运行后，在/data/oracle/product/11.2.0/network/admin中生成listener.ora和sqlnet.ora
#通过netstat命令可以查看1521端口正在监听。
[oracle@bogon ~]$ netstat -tnulp | grep 1521
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp6       0      0 :::1521                 :::*                    LISTEN      17510/tnslsnr 

#以静默方式建立新库，同时也建立一个对应的实例
vim /home/oracle/response/dbca.rsp
```
```shell
#修改文件中以下参数：

[GENERAL]

# oracle版本，不能更改
RESPONSEFILE_VERSION = "11.2.0"

# Description   : Type of operation
OPERATION_TYPE = "createDatabase"

[CREATEDATABASE]

# Description   : Global database name of the database
# 全局数据库的名字=SID+主机域名
# 第三方工具链接数据库的时候使用的service名称
GDBNAME = "orcl.test" #GDBNAME = "orcl.bogon" 

# Description   : System identifier (SID) of the database
# 对应的实例名字
SID = "orcl"

# Description   : Name of the template
# 建库用的模板文件
TEMPLATENAME = "General_Purpose.dbc"

# Description   : Password for SYS user
# SYS管理员密码
SYSPASSWORD = "123456"

# Description   : Password for SYSTEM user
# SYSTEM管理员密码
SYSTEMPASSWORD = "123456"

# Description   : Password for SYSMAN user
# SYSMAN管理员密码
SYSMANPASSWORD = "123456"

# Description   : Password for DBSNMP user
# DBSNMP管理员密码
DBSNMPPASSWORD = "123456"

# Description   : Location of the data file's
# 数据文件存放目录
DATAFILEDESTINATION =/data/oracle/oradata

# Description   : Location of the data file's
# 恢复数据存放目录
RECOVERYAREADESTINATION=/data/oracle/fast_recovery_area

# Description   : Character set of the database
# 字符集，重要!!! 建库后一般不能更改，所以建库前要确定清楚。
# (CHARACTERSET = "AL32UTF8" NATIONALCHARACTERSET= "UTF8")
CHARACTERSET = "ZHS16GBK"

# Description   : total memory in MB to allocate to Oracle
# oracle内存1638MB,物理内存2G*80%
TOTALMEMORY = "1638" 
```

```shell
# 进行静默配置
dbca -silent -responseFile /home/oracle/response/dbca.rsp
#等待较长时间
100% complete
Look at the log file "/data/oracle/cfgtoollogs/dbca/orcl/orcl.log" for further details.

# 建库后进行实例进程检查：
ps -ef | grep ora_ | grep -v grep
#如下
[oracle@bogon ~]$ ps -ef | grep ora_ | grep -v grep
oracle    18520      1  0 15:18 ?        00:00:00 ora_pmon_orcl
oracle    18522      1  0 15:18 ?        00:00:01 ora_vktm_orcl
oracle    18526      1  0 15:18 ?        00:00:00 ora_gen0_orcl
oracle    18528      1  0 15:18 ?        00:00:00 ora_diag_orcl
oracle    18530      1  0 15:18 ?        00:00:00 ora_dbrm_orcl
oracle    18532      1  0 15:18 ?        00:00:00 ora_psp0_orcl
oracle    18534      1  0 15:18 ?        00:00:00 ora_dia0_orcl
oracle    18536      1  0 15:18 ?        00:00:00 ora_mman_orcl
oracle    18538      1  0 15:18 ?        00:00:00 ora_dbw0_orcl
oracle    18540      1  0 15:18 ?        00:00:00 ora_dbw1_orcl
oracle    18542      1  0 15:18 ?        00:00:00 ora_lgwr_orcl
oracle    18544      1  0 15:18 ?        00:00:00 ora_ckpt_orcl
oracle    18546      1  0 15:18 ?        00:00:00 ora_smon_orcl
oracle    18548      1  0 15:18 ?        00:00:00 ora_reco_orcl
oracle    18550      1  0 15:18 ?        00:00:00 ora_mmon_orcl
oracle    18552      1  0 15:18 ?        00:00:00 ora_mmnl_orcl
oracle    18554      1  0 15:18 ?        00:00:00 ora_d000_orcl
oracle    18556      1  0 15:18 ?        00:00:00 ora_s000_orcl
oracle    18582      1  0 15:18 ?        00:00:00 ora_qmnc_orcl
oracle    18585      1  0 15:18 ?        00:00:00 ora_q000_orcl
oracle    18587      1  0 15:18 ?        00:00:00 ora_q001_orcl
oracle    18604      1  0 15:19 ?        00:00:00 ora_cjq0_orcl

# 查看监听状态：
lsnrctl status
#如下
[oracle@bogon ~]$ lsnrctl status

LSNRCTL for Linux: Version 11.2.0.1.0 - Production on 02-DEC-2022 15:27:29

Copyright (c) 1991, 2009, Oracle.  All rights reserved.

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 11.2.0.1.0 - Production
Start Date                02-DEC-2022 14:43:04
Uptime                    0 days 0 hr. 44 min. 50 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /data/oracle/product/11.2.0/network/admin/listener.ora
Listener Log File         /data/oracle/diag/tnslsnr/bogon/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=bogon)(PORT=1521)))
Services Summary...
Service "orcl.bogon" has 1 instance(s).
  Instance "orcl", status READY, has 1 handler(s) for this service...
Service "orclXDB.bogon" has 1 instance(s).
  Instance "orcl", status READY, has 1 handler(s) for this service...
The command completed successfully

#数据库创建完成

# 登录查看实例状态：
sqlplus / as sysdba
```

- 打开防火墙

```shell
#打开SELINX
vim /etc/selinux/config
#设置 SELINUX=enforcing

#开启防火墙
systemctl enable firewalld.service
systemctl restart firewalld.service
systemctl list-unit-files | grep firewalld.service
```

### 启动

**1.启动lsnrctl**

```shell
#查看lsnrctl状态
lsnrctl status
#启动lsnrctl
lsnrctl start
```

**2.启动SID**

```shell
#启动
sqlplus / as sysdba
startup
```

# 权限管理

## 系统权限SYSDBA,SYSOPER

- SYSDBA和SYSOPER是数据库中的两个超级权限，同属于系统权限(System Privileges)，Oracle允许在数据库没有打开的情况下，拥有这两个权限的用户访问实例。在执行重大的数据库操作（尤其是数据库的启动和关闭）时，需要这两个权限。
- SYSOPER的权限比SYSDBA的小。

**SYSDBA可以执行下面的任务：**

- 启动、关闭数据库(STARTUP、SHUTDOWN)
- 修改数据库(ALTER DATABASE),包括打开、装载、备份、改变字符集等
- 创建数据库(CREATE DATABASE)
- 删除数据库(DROP DATABASE)
- 创建服务器参数文件(CREATE SPFILE)
- 改变归档模式(ALTER DATABASE ARCHIVELOG)
- 执行数据库恢复（ALTER DATABASE RECOVER)

## 数据库管理

### 以SYSDBA登录数据库

```
SQLPLUS sys/change_on_install[@orcl] AS SYSDBA
```

### 免密 windows server

- 运行：`lusrmgr.msc`

![](c:/notebook/pictures/Snipaste_2022-11-27_18-50-41.png =400x)

![](c:/notebook/pictures/Snipaste_2022-11-27_18-51-57.png =400x)

### 切换用户

```
CONNECT scott/tiger@orcl
```

## 数据库安全性

- 用户
- 角色
- 概要文件 密码出错几次后锁定，时间、与CPU访问交互信息

## 新建用户 用户 “创建”

- 一般信息 表空间 分配用户创建表空间等数据库对象存储再硬盘中分配空间
- 系统 系统权限
- 角色 系统/对象权限赋予角色，角色赋予用户
- 限额 表空间

## 权限

**数据库安全性、系统安全性、数据安全性**

- 系统权限 对数据库的权限
- 对象权限 操作数据库对象的权限
        - 系统权限 超过100多种
        - DBA数据管理员 高权限

### 创建用户 DBA使用

```
CREATE USER user_name
INDENTIFIED BY password;
```

### 赋予权限/角色

```
GRANT 权限/角色
TO user_name;
```

### 创建用户表空间

```
ALTER USER user_name QUOTA UNLIMITED  --QUOTA 大小 --UNLIMITED 不限制
ON users(表)
```

### 改密码(可用户自己修改)

```
ALTER USER user_name
INDETIFIED BY password;
```

### 给普通用户解锁

```
ALTER user SCOTT  
IDENTIFIED BY tiger 
ACCOUNT UNLOCK;
```

## 对象权限

1. 不同对象具不同的对象权限
2. 对象的拥有者拥有所有的权限  或 SYSDBA/SYSTEM用户
3. 对象的拥有者可以向外分配权限

### GRANT 权限

```
ON table  或 ON user_name.table
TO user_name;
```

### 使用户同样具有分配权限的权利 WITH GRANT OPTION

```
WITH GRANT OPTION
TO PUBLIC;  --向数据库所有用户分配权限
```

### 收回对象权限 REVOKE

```
REVOKE 权限
ON table
FROM user_name
```

### 查询权限分配情况

| 数据库字典           | 描述                     |
| :------------------ | :----------------------- |
| ROLE_SYS_PRIVS      | 角色拥有的系统权限         |
| ROLE_TAB_PRIVS      | 角色拥有的对象权限         |
| USER_ROLE_PRIVS     | 用户拥有的角色            |
| USER_TAB_PRIVS_MADE | 用户分配的关于表对象权限   |
| USER_TAB_PRIVS_RECD | 用户拥有的关于表对象权限   |
| USER_COL_PRIVS_MADE | 用户分配的关于列的对象权限 |
| USER_COL_PRIVS_RECD | 用户拥有的关于列的对象权限 |
| USER_SYS_PRIVS      | 用户拥有的系统权限         |

## 查看用户信息

### oracle 查看当前用户名

```
SHOW USER
```

### oracle 查看所有用户名 ALL_USERS

```
SELECT *
FROM ALL_USERS
```

# 数据泵

官方文档

<https://docs.oracle.com/cd/E11882_01/server.112/e22490/dp_import.htm#SUTIL300>

<https://www.cnblogs.com/Jingkunliu/p/13705626.html>
<https://www.cnblogs.com/ggykx/p/11856404.html>
<https://blog.csdn.net/zhyp29/article/details/84854882>

<https://www.cnblogs.com/tinazzz/p/9708062.html>

## 导出 注 即复制

```
sql>
CREATE USER tuser;
GRANT exp_full_database TO tuser;

CREATE DIRECTORY dmp_dir AS 'f:\dump';
GRANT READ, WRITE ON DIRECTORY TO tuser;

os>
在操作系统相同位置创建f:\dump文件夹

cmd> 注：没有分号 ;
```

```
1.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=fulldb.dmp FULL=Y
2.
expdp tuser DIRECTORY=dmp01_dir DUMPFILE=userdum.dmp SCHEMAS=scott,system
3.
expdp tuser DIRECTORY='dmp02_dir' DUMPFILE=DUMPtable.dmp TABLES=emp,tuser.info,kk.sales:sales_P2
4.
expdp tuser DIRECTORY=dmp01_dir DUMPFILE=userdum01.dmp SCHEMAS=tuser.info
5.
expdp tuser DIRECTORY='dmp_dir' DUMPFILE='dumptablespace.dmp' TABLESPACEs=tbs1,tbs2;
6.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=tuser_comp.dmp COMPRESSION=METADATA_ONLY
7.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=tuser_dump01.dmp CONTENT=ALL
8.  注：若要导出多个.dmp文件到文件夹，需要创建DIRECTORY对象如：dump_dir001
    %U 多个.dmp文件
sql>
CREATE DIRECTORY dmp_dir001 AS 'F:\dump\dumpdir'
os>
在操作系统相同位置创建f:\dump\dumpdir文件夹
cmd>
expdp tuser SCHEMAS=tuser DIRECTORY=dmp_dir DUMPFILE=dum_dir001:exp1.dump, exp2%U.dmp
9.
expdp tuser SCHEMAS=tuser DIRECTORY=dmp_dir DUMPFILE=tuser1_enc.dmp JOB_NAME=enc1 ENCRYPTION=DATA_ONLY ENCRYPTION_PASSWORD=cooboy
10. LRM-00116: ')'后跟 'DD-MM-YYYY HH24:' 时出现语法错误
expdp tuser ESTIMATE_ONLY=Y SCHEMAS=tuser
注 接下来的两种若之前以及使用，需要到路径删除相关的文件才能再次使用
expdp tuser ESTIMATE=BLOCKS SCHEMAS=tuser
expdp tuser ESTIMATE=STATISTICS SCHEMAS=tuser
11.
expdp tuser DIRECTORY=dmp_dir DUMPFILE='dump_ex.dmp' EXCLUDE=VIEW, FUNCTION
12.  https://www.cnblogs.com/daduxiong/archive/2010/08/19/1803764.html
expdp tuser DIRECTORY=dmp_dir DUMPFILE='dump_scn.dmp' FLASHBACK_SCN=25443
13.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=tuser_scn.dmp FLASHBACK_TIME="TO_TIMESTAMP('07-06-2022 20:00:00', 'DD-MM-YYYY HH24:MI:SS')"
注 LRM-00116 : syntax error at 'DD-MM-YYYY HH24:' following ')'
原因 Escape the double quotation marks by \, or put the flashback_time to parfile.
     The " characters are needed by the expdp, and without escaping them, they are eaten by the shell/command.com.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=tuser_scn.dmp FLASHBACK_TIME=\"TO_TIMESTAMP('07-06-2022 20:00:00', 'DD-MM-YYYY HH24:MI:SS')\"
14.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=include_tuser.dmp INCLUDE=TABLE:\"IN ('EPM','DEPARTMENTS')\"
15.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=job_name_tuser.dmp JOB_NAME=exp_job
16.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=tuser.dmp LOG_FILE=tuser_export.log
17.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=network_dump.dmp NETWORK_LINK=指定数据库链接 LOGFILE=tuser_log.log
18.
os>
新建文件 C:\Users\10548\Desktop\pkk.txt
DIRECTORY=dmp_dir
DUMPFILE=tuser_test01.dmp
TABLES=info,tvb
cmd>
expdp PARFILE='C:\Users\10548\Desktop\pkk.txt'
19.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=dump_query.dmp QUERY=employees:"WHERE employee_id > 10 AND salary > 1000"
20.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=dump_query.dmp QUERY=employees:"WHERE employee_id > 10 AND salary > 1000" REUSE_DUMPFILES=Y
21.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=tuser.dmp SAMPLE=20 REUSE_DUMPFILES=Y
22.
expdp tuser DIRECTORY=dmp_dir SCHEMAS=tuser STATUS=400
23.
expdp tuser DIRECTORY=dmp_dir DUMPFILE=tuser_tt.dmp TABLES=tuser.info TRANSPORTABLE=always
注 ORA-29335: 表空间 'USERS' 不为只读
   https://www.cnblogs.com/java-home/archive/2012/04/26/2641193.html
impdp SYSTEM DIRECTOR=dir_dmp DUMPFILE=tuser_tt.dmp TRANSPORT_DATAFILES='D:\U01.dmnp'
```

## 交互式接口 Ctrl+c

### Export

```
1.
Export> START_JOB
ORA-39004: 状态无效
ORA-39016: 当作业处于 EXECUTING 状态时不支持该操作。

Export> exit_client

UDE-31623: 操作产生了 ORACLE 错误 31623
ORA-31623: 作业没有通过指定的句柄连接到此会话
ORA-06512: 在 "SYS.DBMS_SYS_ERROR", line 79
ORA-06512: 在 "SYS.DBMS_DATAPUMP", line 1137
ORA-06512: 在 "SYS.DBMS_DATAPUMP", line 4583
ORA-06512: 在 line 1

应该是
CONTINUE_CLIENT
```

### Import

```
impdp
1.
impdp system DIRECTORY=imp_dir DUMPFILE=FULLDB.dmp --之前创建的导出文件
2.
impdp system DIRECTORY=dmp_dir DUMPFILE=ORCL_FULL_.dmp REUSE_DATAFILES=N TABLE_EXISTS_ACTION=SKIP
UDI-31626: 操作产生了 ORACLE 错误 31626
ORA-31626: 作业不存在
ORA-39086: 无法检索作业信息
ORA-06512: 在 "SYS.DBMS_DATAPUMP", line 3263
ORA-06512: 在 "SYS.DBMS_DATAPUMP", line 4488
ORA-06512: 在 line 1
https://blog.csdn.net/csdnhsh/article/details/95800277
```


# 异质数据源ODBC

**安装ODBC**

1. 先下载相应的ODBC
2. 打开控制面板，“管理工具→ODBC数据源（32位）”打开ODBC数据源配置对话框
3. 在数据源配置对话框中单击“系统DSN”选项卡下的“添加”按钮，创建数据源

---数据源名称
---描述
---用户名

# 加密(钱包)

## 1.修改参数文件sqlnet.ora

- 位于admin文件夹下的，添加如下：

```
ENCRYPTION_WALLET_LOCATION=
(SOURCE=(METHOD=FILE)
       (METHOD_DATA=
        (DIRECTORY=F:\ORA_WALLET))) --路径文件夹(目录DIRECTORY)
```

## 2.设置钱包密码

```sql
ALTER SYSTEM 
SET ENCRYPTION KEY AUTHENTICATED BY "tiger";
```

## 3.使用

```sql
CREATE TABLESPACE secertspace
DATAFILE 'F:\NEWDB\ZDB\userdb01' 
SIZE 200M
ENCRYPTION USING 'AES128'
DEFAULT STORAGE(ENCRYPT);
```

## 4.配置自动打开钱包（可选）

# 初始化参数(Initialization Parameter)

**参数文件**

Oracle把上面的初始化参数及其值存放在一个文件中，我们把这个文件叫参数文件。

启动数据库时（严格地说，应该叫启动实例），Oracle会读取参数文件中的值，然后根据这些值对实例和数据库进行设置。

PFILE和SPFILE是两种类型的参数文件，它们都用于启动数据库。启动数据库时，Oracle会读取这两个文件中的信息(Oracle一次只使用一个)，并进行相关的设置（如设置高速缓冲区的大小、指定控制文件的位置等)。PFILE、SPFILE内容相同，只是文件的格式不同而已。

- PFILE(初始化参数文件)是文本文件，可以使用编辑器修改该文件的内容。
- SPFILE(服务器参数文件)是二进制文件，不能使用编辑器直接修改它的内容，SPFILE的内容要通过Oracle的命令进行读写。
    - 在实例运行时，DBA可以动态修改SPFILE中的参数，因此，Oracle推荐使用SPFILE。

**初始化参数**

数据库中有许多初始化参数(Initialization Parameter)，这些参数用于对整个数据库进行限制、对用户和进程进行限制、对数据库资源进行限制、对数据库的性能进行调整。

部分初始化参数如下所示：

- DB_BLOCK_SIZE
- DB_NAME
- SESSIONS
- CLUSTER_DATABASE
- COMPATIBLE
- ...

## 参数文件

### 初始化参数文件 PFILE

PFLE是传统的参数文件。它是一个文本文件，用于存放初始化参数。一个初始化参数文件的样例如下所示：

```
db name='ZDB'
java pool size=4194304
job queue_processes=10
undo management='AUTO'
undo tablespace='UNDOTBS1'
user_dump_dest='D:\ORACLE\PRODUCT\10.2.0\ADMIN\ZDB\UDUMP
log buffer=11154944
lock sga=TRUE
...
```

参数文件的缺点是，在数据库运行时，DBA不能永久修改初始化参数的值。要想永久修改初始化参数的值，DBA需要先关闭数据库，对参数文件(PFLE)进行修改，然后用修改后的PFILE重新启动数据库。

### 服务器参数文件 SPFILE

SPFILE是一个位于数据库服务器上的二进制文件。和初始化参数文件(PFILE)一样，服务器参数文件也用于存放初始化参数，不同的是，在数据库运行的时候，DBA可以永久修改初始化参数的值。

- 服务器参数文件的默认名字及默认位置

|  操作系统   |        默认名字        |        默认路径        |
| :--------: | :-------------------: | :-------------------: |
| UNIX/Linux | spfileSORACLE_SID.ora |   $ORACLE_HOME/dbs    |
|  Windows   | spfileSORACLE_SID.ora | %ORACLE_HOME\database |

### PFLE、SPFILE的相互转换

#### 根据SPFILE创建PFILE

为了备份初始化参数的设置，可以根据SPFILE生成PFILE(一个文本文件)。PFILE也可以用来启动数据库。

- 在Windows系统中，PFILE和SPFILE的默认路径通常是：
   - `C:\app\10548\product\11.2.0\dbhome_1\database\SPFILEORCL.ORA`

**根据SPFILE创建PFILE的语法如下所示：**

```
CREATE PFILE='PFILE的完整路径'
FROM SPFILE='SPFILE的完整路径';
```

- 要想执行这个命令，用户必须拥有SYSDBA或者SYSOPER权限。
- PFILE可以是任意名字

**创建PFILE过程说明如下。**

1. 以SYSDBA的身份登录数据库。

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 执行语句根据SPFILE创建PFILE的语法

```
CREATE PFILE='E:\MyPFILEORCL.ORA'
FROM SPFILE='C:\app\10548\product\11.2.0\dbhome_1\database\SPFILEORCL.ORA';
```

3. 检查新创建的PFILE的存在

#### 根据PFILE创建SPFILE

**语法**

```
CREATE PFILE='PFILE的完整路径'
FROM SPFILE='SPFILE的完整路径';
```

- SPFILE的名字通常满足格式：`SPFILE$ORACLE_SID.ORA`

**创建SPFILE过程说明如下。**

1. 以SYSDBA的身份登录数据库

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 关闭数据库

```
SHUTDOWN IMMEDIATE
```

3. 执行语句根据PFILE创建SPFILE

```
CREATE SPFILE='E:\MySPFILEORCL.ORA'
FROM PFILE='E:\MyPFILEORCL.ORA';
```

4. 检查新创建的SPFILE的存在 

5. 重新启动数据库

```
STARTUP
```

### 使用参数文件启动数据库

#### 使用SPFILE启动数据库

1. 以SYSDBA的身份登录数据库

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 使用SPFILE启动数据库

```
STARTUP
```

#### 使用PFILE启动数据库

1. 以SYSDBA的身份登录数据库

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 使用PFILE启动数据库

```
STARTUP PFILE = 'E:\MyPFILEORCL.ORA';
```

## 初始化参数

### 显示初始化参数的值

- 要想查看初始化参数的设置，有两种方法：**用SHOW命令和查询视图V$PARAMETER**。

**显示所有初始化参数的值。**

```
方法1：
SELECT name,value
FROM V$PARAMETER
其中，name是参数的名字；value是参数的值。

方法2：
SHOW PARAMETERS
其中，PARAMETERS是关键字，表示显示所有参数的值。
```

**显示指定初始化参数的值。**

```
方法1：
SHOW PARAMETER 参数名
>方法2：
SELECT NAME,VALUE
FROM V$PARAMETER
WHERE name = '参数名';
```

### 初始化参数的修改

**永久修改初始化参数**

为了对数据库进行限制或者调整数据库的性能，DBA需要永久修改初始化参数，则：

- 如果数据库使用的是初始化参数文件(PFILE),要想永久修改初始化参数文件的参数，
   - DBA需要经历关闭数据库→修改初始化参数文件(PFILE)中的参数→新启动数据库的过程。
- 如果数据库使用的是服务器参数文件(SPFILE),
   - DBA只需使用`ALTER SYSTEM SET...SCOPE=SPFILE命令`修改初始化参数，这个命令会更新初始化参数文(SPFILE)。

**临时更改初始化参数的值**

如果DBA只是想临时更改初始化参数的值，

- 可以使用`ALTER SYSTEM SET...SCOPE=MEMORY`命令，这种更改只对当前实例有效，也就是说，如果DBA重新启动实例，刚才的修改将会丢失。
- 用户也可以使用`ALTER SESSION命令`改变某些初始化参数值，但是这种改变仅限于本次会话。


# 数据库的启动/关闭

## 数据库启动

**Oracle数据库的启动经历三个阶段：**

1. 启动实例 (Start An Instance)
- 当启动一个实例时，Oracle会读取一个参数文件，这个文件可以是初始化参数文件（PFILE），也可以是服务器端参数文件(SPFILE)，通常，我们把两者都称为参数文件(Parameter File),
- Oracle根据参数文件(PFILE、SPFILE)中的参数，分配相应大小的一片内存区域叫系统全局区(SGA),然后启动一系列的后台进程（DBWR：数据库写入进程、LGWR：日志写入程序、CKPT：检查点进程、SMON：系统监控进程、PMON：进程监控进程、ARCH：归档进程等)。这些内存和进程合起来组成实例(Oracle Instance)。Oracle还依据参数文件(PFILE、SPFILE)进行其他一些设置（初始化参数）
   - **Oracle的内存和进程构成了实例**。一台机器上可以同时运行多个实例。每个实例都有自己的名字(SD)。实例是用来驱动数据库的，在RAC(Real Application Cluster,集群)环境中，多个实例可以同时驱动一个数据库。实例启动完成以后，数据库就处于**NOMOUNT状态**。在实例启动完成以后，数据库还没有和实例关联。这时候，数据库不可访问。这个阶段主要用于数据库的维护（如重建控制文件等）。
   - 参数文件(PFILE或者SPFILE)指定了控制文件(Control File)的位置。

2. 装载数据库 (Mount The Database)
- 在这个阶段中，Oracle根据参数文件(PFILE或者SPFILE)中的参数(CONTROL_FILES)找到控制文件(Control File)，然后打开控制文件。从控制文件中获得数据文件(Datafile)和重做日志文件(Redo Log File)的名字及位置。
- 这个时候，Oracle已经把实例(Instance)和数据库关联起来。对于普通用户来说，数据库还是不可访问。
- 处于MOUNT阶段的数据库，主要用于数据库的维护（如恢复数据库等）。
3. 打开数据库 (Open The Database)
- 当打开数据库的时候，Oracle打开数据文件(Datafiles)和重做日志文件(Redo Log File)。
- 这时候，数据库可以使用。普通用户可以登录数据库，并对数据库进行操作。

### NOMOUNT 非装载状态（只启动实例）

1. 以SYSDBA的身份登录数据库

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 启动数据库到NOMOUNT状态

```
STARTUP NOMOUNT
```

```
SQL> STARTUP NOMOUNT
ORACLE 例程已经启动。

Total System Global Area 1720328192 bytes
Fixed Size                  2176448 bytes
Variable Size            1174407744 bytes
Database Buffers          536870912 bytes
Redo Buffers                6873088 bytes
```

### MOUNT 装载状态（打开控制文件，读取数据文件和重做日志文件的名称和位置）

1. 以SYSDBA的身份登录数据库

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 启动数据库到MOUNT状态

```
STARTUP MOUNT
```

```
SQL> STARTUP MOUNT
ORACLE 例程已经启动。

Total System Global Area 1720328192 bytes
Fixed Size                  2176448 bytes
Variable Size            1174407744 bytes
Database Buffers          536870912 bytes
Redo Buffers                6873088 bytes
数据库装载完毕。
```

- 如果数据库处于NOMOUNT状态

```
ALTER DATABASE MOUNT;
```

### OPEN 打开数据库

1. 以SYSDBA的身份登录数据库

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 启动数据库

```
STARTUP
```

```
SQL> STARTUP
ORACLE 例程已经启动。

Total System Global Area 1720328192 bytes
Fixed Size                  2176448 bytes
Variable Size            1174407744 bytes
Database Buffers          536870912 bytes
Redo Buffers                6873088 bytes
数据库装载完毕。
数据库已经打开。
```

- 如果数据库处于MOUNT状态

```
ALTER DATABASE OPEN;
```

- 如果数据库处于NOMOUNT状态

```
ALTER DATABASE MOUNT; 
ALTER DATABASE OPEN;
```

### 重启数据库

1. 以SYSDBA的身份登录数据库

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 重启数据库

```
STARTUP FORCE;
```

- 会执行实例恢复的过程系统，正常运行的情况下，不推荐

### 将数据库启动到只读模式

1. 以SYSDBA的身份登录数据库

```sql
CONNECT sys/change_on_install AS SYSDBA;
```

2. 以只读方式启动数据库

```sql
STARTUP OPEN READ ONLY;
```

3. V$DATABASE 检查数据库是否以只读方式打开

```sql
SELECT open_mode
FROM V$DATABASE;
```

### Windows 自动启动/关闭

**1. 将Oracle的服务设置为自动启动**

![](C:/NoteBook/pictures/318933016239486.png =253x)

**2. 编辑注册表**

1. 运行-->regedit命令
2. 进入HKEY_LOCAL_MACHINE\ORACLE\ORACLE_HOME_NAME
3. 将数据库设置成自动启动。 
    - 将参数ORA_SID_AUTOSTART设置为TRUE。ORA_SID_AUTOSTART=TRUE表示操作系统启动时，将自动启动Oracle数据库。
![](C:/NoteBook/pictures/167793416227353.png =567x)
![](C:/NoteBook/pictures/513285816247519.png =568x)
4. 把数据库设置成自动关闭。
    - 将参数ORA_SID_SHUTDOWN设置为TRUE。ORA_SID_SHUTDOWN=TRUE表示操作系统关闭时，将自动关闭Oracle数据库。
![](C:/NoteBook/pictures/123680017240188.png =564x)
    - 将ORA_SID_SHUTDOWNTYPE设置成IMMEDIATE。ORA_SID_SHUTDOWNTYPE于设置数据库的关闭方式。
![](C:/NoteBook/pictures/479550017236743.png =568x)

**3. 创建SPFILE(如果SPFILE不存在)**

**4. 重启Windows**

**5. 测试数据库是否能自动启动**

## 数据库关闭

### 数据库关闭原理

**数据库启动要经历三个阶段，数据库关闭也要经历三个阶段。**

- 阶段一：关闭数据库(Close The Database);
   - 关闭数据库的时候，Oracle首先把SGA中的数据写到数据文件和重做日志文件中。
   - 然后，Oracle关闭所有的数据文件和重做日志文件。这时候，数据库已经不可访问。
   - 这个阶段完成以后，控制文件仍然处在打开状态。
- 阶段二：卸载数据库(Unmount The Database);
   - 数据库关闭完成以后，Oracle将分离数据库和实例之间的联系，这个阶段叫“卸载数据库”或者叫“UNMOUNT数据库”。
   - 这个阶段仅仅是卸载数据库，实例仍然存活在内存中。
- 阶段三：关闭实例(Shut Down The Instance)。
   - Oracle将从内存中移出SGA并终止正在运行的后台进程。
   - 至此，数据库关闭已经完成。

**数据库关闭有四种方式**：

- NORMAL
- IMMEDIATE
- TRANSACTIONAL
- ABORT

### IMMEDIATE方式是数据库关闭使用频率最高的方式。

**以IMMEDIATE方式关闭数库时，会发生下面的事情：**

- 新的用户不能注册（登录）数据库。
- 未提交的事务将被回滚(Rolled Back)。
- Oracle不会等待所有的用户（连接）退出数据库。

**以IMMEDIATE方式关闭数据库具有以下特点：**

- 以IMMEDIATE方式关闭数据库不需要实例恢复(Instance Recovery)。
- `SHUTDOWN IMMEDIATE`是最安全的数据库关闭方式。
- 使用`SHUTDOWN IMMEDIATE`关闭数据库的过程比较慢。

**使用IMMEDIATE方式关闭数据库的过程说明如下。**

1. 以SYSDBA的身份登录数据库

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 以IMMEDIATE方式关闭数据库

```
SHUTDOWN IMMEDIATE;
```


### ABORT方式是关闭数据库最快的方式。

**以ABORT方式关闭数据库时，会发生下面的事情：**

- 不允许启动新的连接(New Connections)和新的事务(New Transactions)。
- 客户端的SQL语句(Client SQL Statements)立刻中止。
- 未提交的事务不被回滚(RolIBack)。
- Oracle立刻中止所有连接（会话)。

**以ABORT方式关闭数据库具有以下特点：**

- 只有数据库出现问题的时候，才使用这种方式关闭数据库。
- 这是一种最不安全的关闭方式，数据库重启时需要实例恢复(Oracle后台进行)。
   - 不推荐使用这种方式关闭数据库。 

**以ABORT方式关闭数据库的过程说明如下。**

1. 以SYSDBA的身份登录数据库

```sql
CONNECT sys/change_on_install AS SYSDBA;
```

2. 以ABORT方式关闭数据库

```sql
SHUTDOWN ABORT;
```

### NORMAL方式(默认)是关闭数据库最慢的方式

- 以NORMAL关闭数据库时，不需要实例恢复(Instance Recovery)。

**使用NORMAL方式关闭数据库时，有以下特点：**

- 允许新的用户注册（登录）数据库。
- 要等所有的用户自动退出Oracle以后，Oracle才关闭数据库。所以，如果数据库中存在一个用户，那么，Oracle就一直等待，直到该用户退出，Oracle才关闭数据库。
   - 如果使用NORMAL方式关闭数据库，也许有的数据库永远也不能关闭。 
   - `SHUTDOWN NORMAL`是最慢的一种数据库关闭方式。

**使用NORMAL方式关闭数据库的过程说明如下。**

1. 以SYSDBA的身份登录数据库

```sql
CONNECT sys/change_on_install AS SYSDBA;
```

2. 以NORMAL方式关闭数据库

```sql
SHUTDOWN NORMAL;
```

### TRANSACTIONAL方式关闭数据库

**以TRANSACTIONAL方式关闭数据库时，会发生下面的事情：**

- 不允许新的用户注册（登录）数据库：
- 不允许建立新的事务(New Transactions):
- 所有的事务(Transactions)完成以后才关闭数据库；
- 一个用户（会话）执行完手里的事务(Transactions)后将被强行断开与数据库的联机。

**以这种方式关闭数据库有以下特点：**

- 这种关闭方式不会使客户端的数据丢失；
- 这种关闭方式不需要实例恢复(Instance Recovery);
- 使用SHUTDOWN TRANSACTIONAL命令关闭数据库的过程比较慢。

**使用TRANSACTIONAL方式关闭数据库的过程说明如下。**

1. 以SYSDBA的身份登录数据库

```
CONNECT sys/change_on_install AS SYSDBA;
```

2. 以TRANSACTIONAL方式关闭数据库

```
SHUTDOWN TRANSACTIONAL;
```


## 故障排除

**数据启动故障诊断由两个阶段组成：**

- 检查数据库
   - 警报日志文件(Alert Log)
   - 跟踪日志文件(Trace Log)）
- 检查操作系统
   - 操作系统的日志文件
   
### 警报日志文件(Alert Log)

- 警报日志文件，用于记录数据库的重大活动和发生的错误
   - 特别注意的是，警报日志文件除了记录数据库中发生的错误外，还记录数据库中发生的重大事件。
- 警报日志文件按照时间的先后记录发生的事件。
- 警报日志文件的名字的格式是alertSID.log,其中，SID表示实例名(Instance)。
- 警报文件的位置由初始化参数BACKGROUND DUMP DEST指定。

**警报日志文件记录的内容如下：**

- 每次数据库启动(STARTUP)和关闭(SHUTDOWN)的详细信息。
- DBA执行的某些管理操作，如ALTER SYSTEM，ALTER DATABASE。
- 某些数据库错误，如Oracle的内部错误(ORA-600)，空间错误等。
- 共享服务器相关的信息和错误。
- 值是非默认值的初始化参数（Initialization Parameters)信息。
- 物化视图(Materialized View)自动刷新产生的错误。

### 跟踪日志文件(Trace Log)

- 每个服务器进程和后台进程都写跟踪日志文件。
- 当一个后台进程检测到错误的时候，Oracle会把错误信息写到跟踪日志文件中。
- 跟踪信息被写到两个目录中，
    - 和后台进程(Background Processes)相关的信息被写到初始化参数DIAGNOSTIC_DEST指定目录 
    - 和服务器进程(Server Procee)相关的信息被写到初始化参数USER_DUMP_DEST指定的目录。
    
- 跟踪日志文件的名字在每种操作系统上会有所不同，但是，每个跟踪日志文件的名宇都包含进程的名字。
- 如果数据库长时间运行，跟踪日志文件会变得越来越大。手工清除跟踪日志文件，或限制跟踪日志文件的大小，
    - Oracle的初始化参数MAX_DUMP_FILE_SIZE用于限制跟踪日志文件的大小。
    - MAX_DUMP_FILE_SIZE的格式如下：`MAX_DUMP_FILE_SIZE = { 大小 [K | M] | UNLIMITED}`

### 查看操作系统的日志文件

# 数据库存储

## 获取数据库系统的设计

### 查看数据块大小

```
SHOW PARAMETER DB_BLOCK_SIZE;
```

### 获取用户中表的信息（建立E-R模型）

**表名：数据字典CAT**

```
SELECT *
FROM CAT;
```

**表结构：DESC 表名**

**约束：数据字典USER_CONSTRAINTS和USER_CONS_COLUMNS**

```
SELECT owner
      ,constraint_name
      ,constraint_type
      ,table_name
      ,r_constraint_name
FROM USER_CONSTRAINTS;

SELECT owner
      ,constraint_name
      ,table_name
      ,column_name
FROM USER_CONS_COLUMNS;
```

**索引：数据字典USER_INDEXES和USER_IND_COLUMNS**

## 数据库的体系结构

### 数据库内存结构

### 数据库的逻辑结构

Oracle存储的逻辑结构由数据块（Data Block），区（Extent），段（Segment）和表空间（Tablespace）以及模式对象(Schema Object)组成

![](C:/NoteBook/pictures/111792515221155.png =584x)

Oracle数据库在逻辑上是由多个表空间组成的，表空间在物理上包含一个或多个数据文件。而数据文件大小是块大小的整数倍；表空间中存储的对象称为段，比如数据段、索引段和回滚段。段由区组成，区是磁盘分配的最小单位。段的增大是通过增加区的个数来实现的。每个区的大小是数据块大小的整数倍，区的大小可以不相同，数据块是数据库中最小的/0单位，同时也是内存数据缓冲区的单位，以及数据文件存储空间单位。块的大小由参数DB_BLOCK_SIZE设置，其值应设置为操作系统块大小的整数倍。

#### 数据块(Data Block)

- 数据块是最基本的存储单元，它是Oracle最小的存储单位（数据文件磁盘存储空间单元），也是数据库I/O的最小单元。
  - 一个数据库至少要有两个数据块 
- 数据块组成区，区组成段，段组成表空间，表空间组成数据库

**数据块的大小由DB_BLOCK_SIZE参数决定。**

- 一般数据块的尺寸为操作系统块的尺寸的整数倍（1、2、4等）。 
- 在创建数据库时可以指定，一般是4KB或8KB，一旦指定后，不可更改
- 只要在内存结构中存在8KB的缓存，在块大小为4KB的数据库中也可以指定创建块大小为8K的表空间。   

**块的结构**

- 块的头部信息区
  - 块头 
     - 包含块的一般信息，如：块的物理地址、块所属的段类型。
  - 表目录
     - 如果块中存储的数据是表数据，则在表目录中保存块中所包含的表的相关信息。 如：表的结构定义等。
  - 行目录 
     - 行记录的相关信息，如ROWID。 
- 块的存储区
  - 主要包含空闲空间和已经使用的空间，通过以下参数管理： 
  - PCTFREE参数
     - 指定块中必须保留的最小空闲空间比例。
     - 当块中的空闲存储空间减少到PCTFREE所设置的比例时，Oracle将块标记为不可用状态，新的数据行将不能被加入到这个块。 
  - PCTUSED参数
     - 指定一个比例，当块中已经使用的存储空间减少到这个比例以下时，这个块将会被重新标记为可用状态。 
  - 可以在表空间或段的级别指定这两个参数，段级别的设置优先级更高。 

#### 区(Extent)

- 区是磁盘空间分配和回收的最小单位（分配单元），是两个或多个相邻的Oracle数据库组成。磁盘按区划分，每次至少分配一个区。
- 区(Extent)是数据库的一个逻辑存储单元，存储在段中，它由连续的数据块组成。数据块(Block)组成区，区组成段(Segment)。
   - 特别强调的是，区是一段连续的存储空间。
   - 当段中的空间耗尽时，Oracle会分配一个新的区给这个段。
- 在创建具有独立段结构的数据库对象时，首先为这些对象创建一个“段”，并为该“段”分配一个“初始区”

**回收对象中未使用的区**

```sql
ALTER TABLE 表
DEALLOCATE UNUESD;
```

```sql
ALTER TABLE scott.employees
DEALLOCATE UNUSED;
```

#### 段(Segment)

- 段是分配给某个逻辑结构的一组区，是存放数据的逻辑存储单元。

**表和段的关系**

- 表由段组成，一个表由一个或者多个段组成。普通表由一个段组成，分区表由多个段组成。
- 创建表的时候，其实质就是创建一个或者多个段。

![](C:/NoteBook/pictures/205771216247520.png =296x)

**段的类型**

- 数据段(Data Segment)、
- 索引段(Index Segment)、
- 临时段(Temporary Segment).、
- 回滚段(Rollback Segment)


1. 数据段
- 用于存放数据。
- 一个单独的段可以存放下面的数据：
   - 一个普通表的数据
   - 分区表(Partition Table)中的一个分区
   - 聚簇表(Cluster Table)
- 创建表的时候，其实质就是创建一个或者多个段。创建数据段的时候，可以指定数据段的存储参数。
2. 索引段
- 索引段用于存放索引数据，每个非分区索引(Nonpartitioned Index)有一个单独的索引段。
- 可以使用`CREATE INDEX`命令创建索引，实质上是创建一个或者多个索引段。创建索引段的时候，可以指定索引段的存储参数。
3. 临时段
- 临时段用于存放临时数据。当进行查询的时候，Oracle会用到临时段。
- 临时段是Oracle自动创建和维护的。
- 临时段用于数据的排序(Sort)。只有内存不足的时候，Oracle才会用到临时段。
   - Oracle用到临时段，是数据库性能降低的一个征兆。所以，DBA应该想方设法尽量让排序在内存中进行。
- 在使用`CREATE USER`命令创建用户或者使用`ALTER USER`命令修改用户的属性时，可以利用子句`TEMPORARY TABLESPACE`为用户指定临时表空间。
   - 如果没有为用户指定临时表空间，Oracle会默认使用SYSTEM表空间（系统表空间）作为用户的临时表空间。
   - SYSTEM表空间主要用于存放数据字典，推荐每个用户使用非系统表空间作为临时表空间。
   - 所有分配的临时段属于用户SYS,不属于执行操作的用户。
4. 回滚段
- 回滚段用于保存回滚条目，Oracle将数据被修改以前的版本保存在回滚条目中，利用这些条目，用户可以撤销以前对数据的修改。
- 回滚条目还可以实现读一致性。

#### 模式对象（Schema Object）

- 模式对象是一种应用，包括表、约束条件、聚簇、视图、索引、序列、同义词、哈希、存储过程、存储函数、触发器、包及数据库链等。
- 对数据库的操作基本可以归结为对数据对象的操作，对象也是一个逻辑结构，是建立于段之上的

#### 表空间(Tablespace)

### Oracle数据库物理结构

- Oracle数据库的物理结构由数据库的操作系统文件所决定，包含数据文件、重做日志文件、控制文件、参数文件、密码文件、归档日志文件、备份文件、告警日志文件、跟踪文件等；
   - 其中数据文件、重做日志文件、控制文件和参数文件是必需的，其他文件可选。

#### 数据文件(Datafile)

- 数据文件是数据库的物理存储单位。
- 数据文件用来存储数据库中的全部数据，
   - 例如，数据库表中的数据和索引数据。通常为`*.DBF`格式

**数据文件的定义**

- 一个数据库有多个数据文件。每个数据文件都是一个操作系统文件。数据文件真正存放数据库的数据。数据库中的表(Tables)/索引(Indexes)的数据，物理上存放在数据文件中。
   - 一个数据文件由多个操作系统块(OSBlock)组成
   - 注：操作系统块和Oracle的数据块是两个概念。
   
**数据文件有下面的特征：**

- 一个数据文件只能属于一个数据库。
- 数据文件可以被设置成自动扩展。当该文件上的空间使用完以后，它可以根据需要自动扩展。
- 一个或者多个数据文件形成一个表空间（表空间是Oracle的逻辑结构）。
- 数据库的数据存储在表空间中，表空间可能某一个或者多个数据文件中。
    - 一个表空间可以由一个或多个数据文件组成，
    - 一个数据文件只能属于一个表空间。
    - 一旦数据文件被加入某个表空间后，就不能删除这个文件如果要删除某个数据文件，只能删除其所属的表空间才行。

用户查询一个表，如果这个表的数据不在内存中，Oracle会读取该表所在的数据文件，并把数据放到内存中。

#### 重做日志文件(Redo Log File)

- 重做日志文件用于记录数据库所做的全部变更（如增加、删除、修改），以便在系统发生故障时，用它对数据库进行恢复。
    - 名字通常为`Log*.DBF`格式，如Log1GCC.dbf,
- 至少两组重做日志文件，Oracle以循环的方式来使用。

#### 控制文件(Control File)

- 每个Oracle数据库都有相应的控制文件，它们是较小的二进制文件，
- 用于记录数据库的物理结构，如数据库名、数据库的数据文件和重做日志文件的名字和位置等信息，
- 用于打开、存取数据库。
- 名字通常为`Ctrl*.CTL`格式，

#### 配置文件(Configuration File)

- 配置文件记录Oracle数据库运行时的一些重要参数，如数据块的大小，内存结构的配置等。
   - 名字通常为`init*.ORA`格式，
   
#### 密码文件(Password File)

- 密码相关的数据存储。

#### 参数文件(Parameter File)

- 参数相关的数据存储。

### Oracle数据库逻辑结构与物理结构的耦合关系

- Oracle数据库的逻辑结构与物理结构的相互耦合连接可以达到数据存储持久化的目的。
   - 所谓持久化就是将数据物理地存储在磁盘上，不受关机或断电的影响。
   
![](C:/NoteBook/pictures/234015519240194.png =477x)

- 一个用户可以使用一个或多个表空间，一个表空间也可以供多个用户使用。
- 用户和表空间没有隶属关系，表空间是一个用来管理数据存储的逻辑概念，表空间只是和数据文件发生关系，数据文件是物理的，
- 一个表空间可以包含多个数据文件，而一个数据文件只能隶属一个表空间。
- SID是Oracle实例的唯一名称标识，用户去访问数据库，实际上是向某一个Oracle实例发送请求，Oracle实例负责向数据库获取数据。
    - Oracle实例=内存结构(SGA)+后台进程(Background Process),
    - 所以Oracle实例是临时性的。
    - `STARTUP NOMOUNT`只启动实例
- 一个实例只能对应一个数据库，一个数据库可以使用多个实例、

![](C:/NoteBook/pictures/167780520236749.png =458x)

**获取这些信息的SQL语句如下。**

- 查看Oracle数据库的库名SQL语句：

数据字典V$DATABASE

```
select name from v$database;
```

- 查看Oracle数据库的实例名SQL语句；

数据字典V$INSTANCE

```
select instance_name from v$instance;
```

- 查看Oracle数据库的域名SQL语句：

```
show parameter domain
```

- 查看Oracle数据库的全局名SQL语句

数据字典GLOBAL_NAME

```
select from global_name;
```

## 表空间

### 概要

- 表空间是Oracle存储的逻辑结构，而数据文件是Oracle存储的物理结构。

#### 表空间

- 表空间是数据库中最大的逻辑单位，每一个表空间由一个或多个数据文件组成，一个数据文件只能与一个表空间相联系。
- 表空间用于存放数据库对象（如表、索引等）。一个数据库逻辑上由一个或多个表空间（tablespace）构成。
- 表空间具有在线(Online)和离线(Offline)属性，可以将除System以外的其他任何表空间设置为离线。
- 每个数据库都拥有一个系统表空间(SYSTEM Tablespace)和一个辅助表空间(SYSAUX Tablespace)。
    - 创建数据库的时候，Oracle会自动创建这两个表空间。

**数据库创建完后会有如下表空间：**

- System表空间；
   - 用于存储系统的数据字典表、程序系统单元、过程函数、包和触发器等，也可用于存储用户数据表、索引对象。
- Sysaux表空间；
- 撤销UNDO表空间：
- 临时Temporary表空间。

对于索引、表等数据库对象，虽然也存在于表空间所属的数据文件中，但这些对象本身不会指定要存放在哪个数据文件上，数据文件只与表空间关联。

**数据库中有以下类型的表空间**

- 永久表空间(Permanent Tablespaces，又叫常规表空间)
- 临时表空间(Temporary Tablespace)
- 回滚表空间(Undo Tablespace）。

**大文件表空间和小文件表空间**

- 在默认情况下，Oracle创建小文件表空间(Smallfile Tablespace)，小文件表空间是传统的表空间。系统表空间和辅助表空间是小文件表空间。
- 同时，Oracle还允许创建大文件表空间(Bigfile Tablespace)。大文件表空间是由单个很大的文件组成的表空间。

**表空间与数据文件的关系**

- 表空间用于存放数据库对象，而这些对象实际上存放在数据文件中，数据文件是数据的物理载体。
- 表空间的尺寸是组成这个表空间的所有数据文件的尺寸之和。



#### 表空间中区(Extent)的管理方式

**对于表空间中区的空间管理，Oracle有两种管理方式：**

- 字典管理(Dictionary Managed Tablespace)，通过数据字典对区进行管理。
- 本地管理(Locally Managed Tablespace)，通过位图对区进行管理。
    - 本地管理是Oracle推荐使用的区的管理方式。
    
##### 字典管理（Dictionary Managed Tablespaces)

- 如果使用字典管理，Oracle将使用数据字典来跟踪表空间中区的使用。
- 当分配一个新的区、使用一个区或者释放一个区的时候，Oracle都会更新数据字典`FET$与UET$`。通过数据字典来反映区的状态。
- 当更新数据字典时，会产生回滚信息，Oracle会存储这些回滚信息。这正是字典管理的表空间逐渐被淘汰的根源。

##### 本地管理(Locally Managed Tablespaces)

**EXTENT MANAGEMENT LOCAL**

- 本地管理表空间是Oracle推荐使用的方式，它通过本地表空间来管理区的使用。
- 表空间中的每个教据文件都有一个位图(Bitma即)，位图存放在数据文件头，位图中的每个位(bit)与一个或者一组数据块(B1ok)相对应。当分配一个区或者释放一个区的时候，Oracle将更新位图以反映这种改变。
- 特别注意的是，对于本地管理的表空间，Oracle不会更新数据字典，也不会产生回滚信息

#### 索引段(Segment)的管理方式

当用户进行数据的插入、更新、删除的时候，段中数据块的空间会发生改变，块的状态也会发生改变。

**Oracle使用两种方式来管理这些块：**

- 段的手动管理(Manual Segment Space Management):
- 段的自动管理(Automatic Segment Space Management,ASSM)。

##### 段的手动管理

**SEGMENT SPACE MANAGEMENT MANUAL**

Oracle使用自由列表(Free List)来管理段中的块。自由列表列出了允许进行插入操作的数据块。

**Oracle通过PCT_FREE（PCTFREE)和PCT_USED(PCTUSED)两个参数来控制一个数据块是否允许插入数据。**

- 当对一个块进行INSERT或者UPDATE操作以后，Oracle会把这个块中剩余的自由空间和PCT_FREE进行对比，如果这个数据块中的自由空间小于PCT_FREE的设置，则Oracle将把这个块从自由列表中拿出来，表示这个块不再允许进行插入，但是，这个块中剩余空间允许更新操作；
- 当对一个块进行DELETE或者UPDATE操作以后，Oracle会把这个块中已使用的空间和PCT_USED进行对比，如果这个数据块中已经使用的空间小于PCT_USED的设置，则Oracle把这个块放回自由列表中，表示这个块允许数据的插入。因此，PCT_FREE涉及把块从自由列表中移出，PCT_USED涉及把块移到自由列表中。

##### 段的自动管理

**SEGMENT SPACE MANAGEMENT AUTO**

“段的自动管理”使用位图来管理段中的数据块。当块中的空间发生变化的时候，Oracle会更新位图，以反映这块是否允许数据的插入。位图使Oracle能够自动管理自由空间。


### 常规表空间管理

#### 创建表空间

```
CREATE TABLESPACE 表空间名
DATAFILE '数据文件完整路径.DBF'
SIZE 大小 K|M 
AUTOEXTEND ON NEXT 大小 K|M  --自动扩展
MAXSIZE UNLIMITED 
LOGGING EXTENT MANAGEMENT LOCAL  --表示本地管理的表空间
SEGMENT SPACE MANAGEMENT {AUTO | DEFAULT | MANUAL}; --使用段的自动管理
```

```
CREATE TABLESPACE mytest_tablespace
DATAFILE 'G:\mytest_tablespace\datafile_01.DBF'
SIZE 100M
AUTOEXTEND ON NEXT 50M
MAXSIZE UNLIMITED
LOGGING EXTENT MANAGEMENT LOCAL
SEGMENT SPACE MANAGEMENT AUTO
```

```
CREATE TABLESPACE mytest_tablespace_02
DATAFILE 'G:\mytest_tablespace_02\datafile_02_01.DBF'
SIZE 200M
AUTOEXTEND ON NEXT 50M
MAXSIZE UNLIMITED
LOGGING EXTENT MANAGEMENT LOCAL
SEGMENT SPACE MANAGEMENT MANUAL
```

#### 扩展表空间(扩展组成表空间的数据文件)

##### 手动增加(重新设置大小)表空间中的数据文件大小

```
ALTER DATABASE DATAFILE '数据文件路径' 
RESIZE 大小 K|M;
```

```
ALTER DATABASE DATAFILE 'G:\mytest_tablespace\datafile_01.DBF' 
RESIZE 300M
```

##### 把表空间中的数据文件设置为自动扩展

```
ALTER DATABASE DATAFILE '数据文件路径' 
AUTOEXTEND ON NEXT 大小 
MAXSIZE UMLIMITED
```

```
ALTER DATABASE DATAFILE 'G:\mytest_tablespace\datafile_01.DBF'
AUTOEXTEND ON NEXT 30M
MAXSIZE UNLIMITED
```

##### 往表空间中增加数据文件

```
ALTER TABLESPACE 表空间名 
ADD DATAFILE '数据文件路径' SIZE 大小 K|M
```

```
ALTER TABLESPACE mytest_tablespace
ADD DATAFILE 'G:\mytest_tablespace\datafile_02.DBF' SIZE 100M
```

#### 表空间离线

**表空间在线**

- 使表空间中的数据可以访问，
- 在数据库打开的状态下，SYSTEM表空间永远在线
   - Oracle需要访问存放在SYSTEM表空间内的数据字典 
   - SYSTEM表空间不能被离线。
 
**表空间离线**

- 使表空间中的数据不可访问
- 通常用于维护时，离线。

##### 使表空间离线

具有一定的风险。

```
ALTER TABLESPACE 表空间名 OFFLINE [NORMAL|TEMPORARY|IMMEDIATE];
```

**离线的三种方式**

1. NORMAL 常规的离线方式。会执行校验点，比较安全。
2. TEMPORARY 当数据文件有误时，使用
3. IMMEDIATE 离线数据文件，但是不会产生校验点，表空间重新在线时，需要执行恢复。

```
ALTER TABLESPACE mytest_tablespace OFFLINE;
```

##### 使表空间在线

```
ALTER TABLESPACE 表空间名 ONLINE;
```

```
ALTER TABLESPACE mytest_tablespace ONLINE;
```

#### 重命名表空间

- 不能重命名表空间SYSTEM和SYSAUX
- 如果表空间离线或者表空间中有数据文件离线，表空间不能被重命名。

```
ALTER TABLSPACE 表空间名
RENAME TO 新表空间名;
```

```
ALTER TABLESPACE mytest_tablespace_02
RENAME TO mytest_tablespace02;
```

#### 表空间的读写

**只读表空间(Read Only Tablespace)**

- 只读表空间，顾名思义，这个表空间只允许读，不允许修改。鉴于只读表空间的这种特性，只读表空间可以放在只读的设备上，如CD-ROM、WORM。
- 如果要想修改一个只读表空间，只能先把这个表空间的状态设成可读/写(Read/Write),接着对该表空间上的数据进行修改，修改完成后，再把该表空间改成只读(Read Only)。
- 由于只读表空间很少被修改，所以只读表空间不需要频繁备份。

##### 使表空间只读

```
ALTER TABLESPACE 表空间名 
READ ONLY;
```

```
ALTER TABLESPACE mytest_tablespace
READ ONLY;
```

##### 使表空间可读写

```
ALTER TABLESPACE 表空间名
READ WRITE;
```

```
ALTER TABLESPACE mytest_tablespace
READ WRITE;
```

#### 删除表空间

```
DROP TABLESPACE 表空间名
```

##### 表空间内有内容时

```
DROP TABLESPACE 表空间名
INCLUDING CONTENTS;
```

##### 删除表空间及其数据文件

```
DROP TABLESPACE 表空间名
INCLUDING CONTENTS AND DATAFILES;
```

```
DROP TABLESPACE mytest_tablespace02
INCLUDING CONTENTS AND DATAFILES;
```

#### 大文件表空间

##### 定义

**大文件表空间(Bigfile Tablespace)**

- 大文件表空间由一个单独很大的数据文件组成。大文件表空间使Oracle能够使用和管理超级大的文件。
- 在默认情况下，系统默认创建小文件表空间(Smallfile Tablespace)，小文件表空间(**SMALLFILE**)是传统的表空间类型。Oracle创建的SYSTEM表空间和SYSAUX表空间都是小文件表空间。

**使用大文件表空间有下面的优点。**

   - 使表空间的容量更大。
      - 使用大文件表空间（bigfile tablespace）可以显著地增强Oracle数据库的存储能力。一个小文件表空间（smallfile tablespace）最多可以包含1024个数据文件（datafile），而 一个大文件表空间中只包含一个文件，这个数据文件的最大容量是小数据文件的1024倍。这样看来，大文件表空间和小文件表空间的最大容量是相同的。但是由 于每个数据库最多使用64K个数据文件，因此使用大文件表空间时数据库中表空间的极限个数是使用小文件表空间时的1024倍，使用大文件表空间时的总数据 库容量比使用小文件表空间时高出三个数量级。换言之，当一个Oracle数据库使用大文件表空间，且使用最大的数据块容量时（32K），其总容量可以达到 8EB。（1EB =1024PB，1PB = 1024TB，1TB=1024GB）
   - 在超大型数据库中，简化数据文件的管理。使用大文件表空间，可以减小SGA的需求。同时，还可以减少控制文件的尺寸。
   -简化数据库的管理。
       - 当数据库文件由Oracle管理（Oracle-managed files），且使用大文件表空间（bigfile tablespace）时，数据文件对用户完全透明。换句话说，用户只须针对表空间（tablespace）执行管理操作，而无须关心处于底层的数据文件 （datafile）。使用大文件表空间，使表空间成为磁盘空间管理，备份，和恢复等操作的主要对象。使用大文件表空间，并与由Oracle管理数据库文件（Oracle-managed files）技术以及自动存储管理（Automatic Storage Management）技术相结合，就不再需要管理员手工创建新的数据文件（datafile）并维护众多数据库文件，因此简化了数据库文件管理工作。

- 只有本地管理的（locally managed），且段空间自动管理（automatic segmentspace  management）的表空间（tablespace）才能使用大文件表空间（bigfile tablespace）。 
    - 但是有两个例外：本地管理的撤销表空间（undo tablespace）和临时表空间（temporary tablespace），即使其段（segment）为手工管理（manually managed）
    
**使用大文件表空间时需要考虑的因素**

- 大文件表空间（bigfile tablespace）应该和自动存储管理（Automatic  Storage Management）或其他逻辑卷管理工具（logical volume manager）配合使用，这些工具应该能够支持动态扩展逻辑卷，也能支持striping（数据跨磁盘分布）或RAID。
- 应该避免在不支持striping的系统上使用大文件表空间，因为这将不利于并行执行（parallel execution）及 RMAN 的并行备份（backup parallelization）。
- 当表空间正在使用的磁盘组（disk group）可能没有足够的空间，且扩展表空间的唯一办法是向另一个磁盘组加入数据文件时，应避免使用大文件表空间。
- 不建议在不支持大文件的平台上使用大文件表空间，这会限制表空间（tablespace）的容量。参考相关的操作系统文档了解其支持的最大文件容量。
- 如果使用大文件表空间替代传统的表空间，数据库开启（open），checkpoints，以及 DBWR 进程的性能会得到提高。但是增大数据文件（datafile）容量可能会增加备份与恢复的时间

**特有的属性**

1. 每个大文件表空间只能包含一个数据文件。如果试图添加新的文件，则会报告 ORA-32771 错误
2. 只有自动段空间管理的 LMT (locally managed tablespaces ) 支持大文件表空间
3. 相对文件号(RELATIVE_FNO)为1024 ( 4096 on OS/390)
   - 因为大文件表空间只有一个数据文件，所以其相对文件号也是固定的:1024

##### 创建大文件表空间

```
CREATE BIGFILE TABLESPACE 表空间名  --BIGFILE指定创建的表空间是大文件表空间。 
DATAFILE '大数据文件路径' SIZE 大小;
```

```
CREATE BIGFILE TABLESPACE mytest_big_tablespace
DATAFILE 'G:\big_datafile_01.DBF' SIZE 100M;
```

##### 修改数据库默认的表空间类型：

``` 
ALTER DATABASE 
SET DEFAULT BIGFILE|SMALLFILE TABLESPACE;  --默认SMALLFILE表空间
```

#### 表空间的加密

```
ALTER TABLESPACE 表空间名
DATAFILE '数据文件路径' SIZE 大小
ENCRYPTION USING '加密算法'
DAFAULT STORAGE(ENCRYPT);
``` 

**加密算法**

- 3DES168
- AES128
- AES192
- AES256

#### 表和表空间的关系（查询表空间信息）

##### 列出数据库中的表空间

**数据字典DBA_TABLESPACES**

```
SELECT tablespace_name
FROM DBA_TABLESPACES;
```

##### 查询组成表空间的数据文件

**数据字典DBA_DATA_FILES**

```
SELECT file_name
FROM DBA_DATA_FILES
WHERE tablespace_name = '表空间名';
```

```
SELECT file_name
FROM DBA_DATA_FILES
WHERE tablespace_name = 'MYTEST_TABLESPACE';
```

##### 查询表空间的剩余空间

```
--统计所有表空间的空间使用情况
SELECT tbs 表空间名
      ,SUM(totalM) 总共大小M
      ,SUM(usedM) 已使用空间M
      ,SUM(remainedM) 剩余空间M
      ,TRUNC(SUM(usedM)/SUM(totalM)*100,1) 已使用百分比
      ,TRUNC(SUM(remainedM)/SUM(totalM)*100,1) 剩余百分比
FROM (
--查询每个表空间的空间使用情况
 SELECT b.file_id ID
       ,b.tablespace_name tbs
       ,b.file_name name
       ,b.bytes/1024/1024 totalM
       ,(b.bytes - SUM(NVL(a.bytes,0)))/1024/1024 usedM
       ,SUM(NVL(a.bytes,0))/1024/1024 remainedM
       ,SUM(NVL(a.bytes,0))/(b.bytes)*100 剩余百分比
       ,(100 - (SUM(NVL(a.bytes,0))/(b.bytes)*100)) 已使用百分比
 FROM DBA_FREE_SPACE a
     ,DBA_DATA_FILES b
 WHERE a.file_id = b.file_id
 GROUP BY b.tablespace_name
         ,b.file_name
         ,b.file_id
         ,b.bytes
 ORDER BY b.tablespace_name
)
GROUP BY tbs;
```

##### 查询创建表空间的代码

- 先设置显示

```
SET LONG 10000;
```

**软件包DBMS_METADATA**

```
SELECT DBMS_METADATA.GET_DDL('TABLESPACE','MYTEST_TABLESPACE')
FROM dual;
```


### 临时表空间管理

#### 概要

**临时表空间，存放临时数据的表空间：（包括以下临时数据）**

- 索引数据
- 临时表数据，临时索引数据
- 临时大对象（LOB）的信息

##### 排序段(Sort Segment)

- 临时表空间中含有排序段（ort Segment)，临时数据放在排序段中。
- 临时数据是进行排序时用到的一些数据，这些数据暂时存放在临时段中。
- 一个SQL语句可能会使用一个或者多个临时表空间。
- 临时表空间中不能创建永久性的数据库对象（如表、索引）。

- 一个排序段可以被多个排序操作所共享
   - 当执行第一次排序时，Oracle在临时表空间中创建排序段，该排序段不断扩展，直到能够容纳所有的排序数据。但数据库关闭时，临时段被释救。

**下面的SQL语句将会用到排序段（临时表空间）：**

- CREATE INDEX
- SELECT...ORDER BY
- SELECT DISTINCT...
- SELECT.GROUP BY
- SELECT...UNION
- SELECT.INTERSECT
- SELECT...MINUS

##### 临时数据文件(Temporary Datafiles)

**临时数据文件属于临时表空间。临时数据文件和常规的数据文件差不多，只是有下面的一些差异：**

- 临时数据文件总是被设置成NOLOGGING。
- 不能使临时数据文件只读。
- 不能使用ALTER DATABASE创建临时数据文件。
- 介质恢复(Media Recovery)不能恢复临时数据文件。
- 创建临时数据文件时，Oracle并不总是给它分配空间，直到需要使用这些空间。
- 临时数据文件的信息并不是存在于数据字典DBA_DATA_FILES或V$DATAFILE中。
   - 临时数据文件的信息存放在单独的数据字典DBA_TEMP_FILES和V$TEMPFILE中。

#### 创建临时表空间

```
CREATE TEMPORARY TABLESPACE 临时表空间名
TEMP FILE '路径'
SIZE 大小；
```

```
CREATE TEMPORARY TABLESPACE mytest_temp_tablespace_01
TEMPFILE 'F:\mytest_tablespace\mytest_temp_tablespace_01\mytest_tempfile_01_01'
SIZE 100M;
```

#### 查询信息

**视图DBA_TEMP FILES用于描述临时文件的信息**

```
SELECT tablespace_name
      ,file_name
      ,status
FROM DBA_TEMP_FILES
ORDER BY tablespace_name;
```

#### 默认临时表空间(Default Temporary Tablespace)

- 创建用户时，应该给用户指定一个临时表空间，用户的排序段存放在该表空间。
- 如果没有给用户指定临时表空间，Oracle会自动给这个用户指定一个临时表空间，这个临时表空间叫默认临时表空间(Default Temporary Tablespace）。
   - 如果系统表空间(SYSTEM Tablespace)是本地管理的(Locally Managed)，则必须定义一个默认临时表空间，**SYSTEM表空间不能作为默认临时表空间**；
   - 如果系统表空间是字典管型的(Dictionary Managed,则不必定义一个默认临时表空间，SYSTEM表空间可以作为默认临时表空间，但是，一旦用户使用SYSTEM表空间进行排序，Oae将会发出警告。
   - 从性能的角度考虑，不应该把SYSTEM表空间作为临时表空间。

##### 默认临时表空间的创建

创建数据库的时候，可以指定默认临时表空间。

##### 默认临时表空间的修改

数据库创建完成以后，我们也可以修改默认的临时表空间。

```
ALTER DATABASE DEFAULT TEMPORARY TABLESPACE 表空间名;
```

##### 查找默认临时表空间

```
SELECT property_name
      ,property_value
FROM DATABASE_PROPERTIES
WHERE property_name = 'DEFAULT_TEMP_TABLESPACE';
```

#### 表空间组

在Oracle11g以前，一个用户只能使用一个临时表空间，如果临时表空间不足，将导致排序错误。同时，单个表空间的机制影响数据库的性能。Oracle11g引入了表空间组，一个用户可以同时使用多个表空间，这样的益处不言而喻，尤其值得注意的是，它使在**多个临时表空间上的并行执行**成为现实。
述表空间组的使用。

##### 把临时表空间加入表空间组

- 如果该表空间组不存在，该语句隐含创建该表空间组。

```
ALTER TABLESPACE 临时表空间名 
TABLESPACE GROUP 表空间组名;
```

##### 查询表空间组的信息。

**数据字典DBA_TABLESPACE_GROUPS**

```
SELECT *
FROM DBA_TABLESPACE_GROUPS;
```

##### 把默认临时表空间改成表空间组。

```
ALTER DATABASE DEFAULT TEMPORARY TABLESPACE 表空间组名;
```

##### 把临时表空间从表空间组中删除。

```
ALTER TABLESPACE 临时表空间名 TABLESPACE GROUP '';
```

### 回滚表空间

- 回滚表空间是表空间的一种类型，用于存放回滚数据。

#### 概要

##### 回滚数据（Undo Data)

- 当用户修改数据的时候，Oracle会把数据修改以前的值保存起来，这样的数据叫回滚数据(Undo Data)。

**回滚数据有下面的用途：**

- 事务回滚(Transaction Undo),回滚未提交的事务
- 提供读一致性(Read Consistency)
- 数据库恢复(Database Recovery)
- 闪回查询(Flashback Query)

**回滚数据一直被保存在回滚段中，直到下面的情况出现：**

- 用户输入了ROLLBACK命令回滚了这个事务(Transaction)
- 用户输入了COMMIT命令提交了这个事务(Transaction)
- 用户（会话）被异常中止（事务被回滚）
- 用户正常退出Oracle登录（事务被提交）

**读一致性（Read Consistency)**

回滚数据为Oracle提供读一致性。当一个用户启动一个事务，对数据库进行修改，但数据未提交(Commit),而正在此时，另外一个用户查询刚才那个用户修改的数据，则Oracle会从回滚段中获得查询需要的数据。

![](C:/NoteBook/pictures/18422011227356.png =668x)

**数据库恢复(Database Recovery)**

回滚数据是实例恢复(Instance Recovery)的重要组成部分。实例恢复是Oracle自动执行的。进行实例恢复时，Oracle应用重做日志文件中提交(Committed)的和未提交(Uncommitted)的事务，把数据库恢复到实例失败时的状态，最后，Oracle回滚未提交的事务，Oracle回滚的时候就会用到回滚段中的数据。

**闪回查询(Flashback Query)**

闪回查询是Oracle的新特性，它允许用户查询过去某个点（如时间点）表的状态。这项功能允许DBA把表恢复到过去的某个时间点。

**事务和回滚数据(Transactions and Undo Data)**

启动一个事务的时候，Oracle会把一个回滚段赋予这个事务。当数据被改变的时候，数据被改变以前的值将被复制到回滚段中，

**数据字典视图V$TRANSACTION**

- 查询回滚段被赋予的事务

##### 回滚段(Undo Segment)

- 回滚段是特殊的段，需要的时候，Oracle会自动创建回滚段。
- 与其他类型的段一样，回滚段由区组成，区由块组成。
- 回滚段会自动增长(Grow)和收缩(Shrink)。
- 回滚段与数据库中的数据段和索引段一样，都用于存放数据，只是回滚段存放的是回滚数据。
- 回滚段存放在一种特殊的表空间中，这种表空间叫“回滚表空间。
- 一个数据库可以有多个回滚表空间，但是只能有一个活动(Active)的回滚表空间。
- 回滚段存放一个或者多个表的回滚数据。
- 回滚段中的区是循环使用的，当回滚段中的一个区使用完成的时候，Oracle会使用已经存在的区或者分配一个新的区。当所有的区都使用完成的时候，如果第一个区没被其他活动事务所使用，Oracle会重新使用第一个区。

##### 回滚段的原理

事务T1开始写区E1，当E1写满的时候，Oracle检查下一个区，发现区E2上有活动的事务（即有未提交的数据）；Oracle继续检查下一个区，发现区E3是不活动的(数据已经提交)，Oracle把回滚数据写到E3中，当E3写满的时候，Orac1e对后续的区进行检查，发现E4、E5、E6中有活动事务，这几个区不能写，Omce又继续检查，发理一个空白的区E7,Oace开始往这个区中写回滚数据，当E7写满后，Oce继续检发现最后个区3是活动的，这时候，所有的区已经用完，OCe循环到区E引，如果区E1可用，Oracle会重用区E1,如果区E1不可用，Oracle只能给这个回滚段分配新的区（即再加一个E9）。

![](C:/NoteBook/pictures/374622213221063.png =218x)

当用户输入DML (INSERT、UPDATE、DELETE)后，就开始了一个事务，Oracle把该事务和一个回滚段进行关联。Oracle将使用这个回滚段存放这个事务的回滚数据
对表中的任何数据的改变将被记录到回滚段中。

**当前活动的回滚段的信息，可以遍过查视图V$ROLLNAME获得。**

```
SELECT *
FROM V$ROLLNAME;
```

- 无论哪个用户使用回滚段，所有回滚段的拥有者都是用户SYS。
- 一个回滚段至少有两个区。
    - 因为至少要两个区才能实现回滚段的循环利用。
- 一个回滚段可以有许多区，对于数据块大小是8KB的表空间，一个回滚段默认情况下最多可有32765个区。

##### 回滚段的管理

回滚段有两种管理方式：手动回滚段管理(Manual Undo Management)和自动回滚段管理(Automatic Undo Management)。

**手动回滚段方式**

手动回滚段方式比较烦琐，许多工作都需要DBA手动完成，如手动创建回滚段，手动使回滚段在线(Online)。这些手动
的操作增加DBA的管理负担，使回滚段的管理变得更加复杂。手动回滚段管理正逐渐被Oracle所淘汰。

**自动回滚段管理**

自动回滚段管理(Automatic Undo Management)使DBA能够从繁重的工作中解脱出来。自动回滚段管理只需一个回滚表空间。回滚段的创建和释放等工作都由Oracle自动完成。

- 设置初始化参数UNDO_MANAGEMENT=AUTO,
- 并且通过初始化参数UNDO_TABLESPACE指定数据库使用哪个回滚表空间。

目前，自动回滚段管理方式已经得到广泛应用。

#### 回滚表空间(Undo Tablespace)

- 回滚表空间是一个特殊的表空间，回滚表空间只能用于存放回滚段。不能在回滚表空间创建其他数据库对象（如表、索引）。
- 每个数据库可有零个或者多个回滚表空间。但是，只能有一个活动的回滚表空间。
- SYSTEM回滚段是特殊的回滚段，它存在于SYSTEM表空间，SYSTEM回滚段用于回滚系统事务。
    - 非系统事务只能交由非系统回滚段来处理。
    - 因此，除了系统回滚表空间外，每个数据库应该另外创建一个回滚表空间。
    
#### 创建回滚表空间

```
CREATE UNDO TABLESPACE 回滚表空间名
DATAFILE '路径' 
SIZE 大小
REUSE --表示重用已经存在的数据文件
AUTOEXTEND ON; --打开自动扩展
```

```
CREATE UNDO TABLESPACE mytest_undo_tbs_01
DATAFILE 'F:\orcl\mytest_undo_tbs_01\undo_dat_01_01.DBF' 
SIZE 8M
REUSE 
AUTOEXTEND ON;
```

#### 启用回滚表空间自动管理

**1.创建回滚表空间**

```
CREATE UNDO TABLESPACE mytest_undo_tbs_02
DATAFILE 'F:\orcl\mytest_undo_tbs_02\undo_dat_02.DBF'
SIZE 10M
REUSE
AUTOEXTEND ON;
```

**2.关闭数据库**

```
SHUTDOWN IMMEDIATE
```


**3.创建PFILE（也可以使用SPFILE来操作）**

```
CREATE PFILE = 'F:\orcl\mytestpfile.ORA'
FROM SPFILE = 'C:\app\zjk10\product\11.2.0\dbhome_1\database\SPFILEORCL.ORA';
```

**4.修改新创建PFILE，增加两个参数**

- UNDO_MANAGEMENT= AUTO
- UNDO_TABLESPACE= 回滚表空间名

```
orcl.__db_cache_size=3087007744
orcl.__java_pool_size=16777216
orcl.__large_pool_size=16777216
orcl.__oracle_base='C:\app\zjk10'#ORACLE_BASE set from environment
orcl.__pga_aggregate_target=2650800128
orcl.__sga_target=3976200192
orcl.__shared_io_pool_size=0
orcl.__shared_pool_size=805306368
orcl.__streams_pool_size=0
*.audit_file_dest='C:\app\zjk10\admin\orcl\adump'
*.audit_trail='db'
*.compatible='11.2.0.0.0'
*.control_files='C:\app\zjk10\oradata\orcl\control01.ctl','C:\app\zjk10\flash_recovery_area\orcl\control02.ctl'
*.db_block_size=8192
*.db_domain=''
*.db_keep_cache_size=0
*.db_name='orcl'
*.db_recovery_file_dest='C:\app\zjk10\flash_recovery_area'
*.db_recovery_file_dest_size=4102029312
*.diagnostic_dest='C:\app\zjk10'
*.dispatchers='(PROTOCOL=TCP) (SERVICE=orclXDB)'
*.memory_target=6616514560
*.open_cursors=300
*.processes=150
*.remote_login_passwordfile='EXCLUSIVE'
*.undo_tablespace='UNDOTBS1'
```

- 修改后

```
orcl.__db_cache_size=3087007744
orcl.__java_pool_size=16777216
orcl.__large_pool_size=16777216
orcl.__oracle_base='C:\app\zjk10'#ORACLE_BASE set from environment
orcl.__pga_aggregate_target=2650800128
orcl.__sga_target=3976200192
orcl.__shared_io_pool_size=0
orcl.__shared_pool_size=805306368
orcl.__streams_pool_size=0
*.audit_file_dest='C:\app\zjk10\admin\orcl\adump'
*.audit_trail='db'
*.compatible='11.2.0.0.0'
*.control_files='C:\app\zjk10\oradata\orcl\control01.ctl','C:\app\zjk10\flash_recovery_area\orcl\control02.ctl'
*.db_block_size=8192
*.db_domain=''
*.db_keep_cache_size=0
*.db_name='orcl'
*.db_recovery_file_dest='C:\app\zjk10\flash_recovery_area'
*.db_recovery_file_dest_size=4102029312
*.diagnostic_dest='C:\app\zjk10'
*.dispatchers='(PROTOCOL=TCP) (SERVICE=orclXDB)'
*.memory_target=6616514560
*.open_cursors=300
*.processes=150
*.remote_login_passwordfile='EXCLUSIVE'
*.undo_tablespace='MYTEST_UNDO_TBS_02'
*.undo_management=AUTO
```

**5.使用创建的PFILE启动数据库**

```
STARTUP PFILE = 'F:\orcl\mytestpfile.ORA';
```

#### 删除回滚表空间

- 不能删除数据库正在使用的回滚表空间。只能将数据库切换到另外一个回滚表空间才能删除。

#### 往回滚表空间添加数据文件

```
ALTER TABLESPACE 回滚表空间名
ADD DATAFILE '数据文件路径'
SIZE 大小
AUTOEXTEND ON NEXT 大小
MAXSIZE UNLIMITED；
```

```
ALTER TABLESPACE mytest_undo_tbs_02
ADD DATAFILE 'F:\orcl\mytest_undo_tbs_02\mytest_undo_02_01.DBF'
SIZE 10M
AUTOEXTEND ON NEXT 1M
MAXSIZE UNLIMITED;
```

#### 切换数据库当前的回滚表空间

**查看正在使用的回滚表空间**

```
SHOW PARAMETER UNDO_TABLESPACE;
```

**切换当前正在使用的回滚表空间**

- 需要在使用SPFILE启动数据库的情况下使用
   - 否则：ORA-32001: 已请求写入 SPFILE, 但是没有正在使用的 SPFILE

```
ALTER SYSTEM
SET UNDO_TABLESPACE = 要切换的新回滚表空间
SCOPE = BOTH;  --表示此修改是永久有效的，且立即生效
```

#### 查询数据库中的回滚表空间

**数据字典DBA_TABLESPACES**

```
SELECT tablespace_name
FROM DBA_TABLESPACES
WHERE contents = 'UNDO';
```

## 数据文件

### 数据文件管理

#### 使数据文件离线

数据文件离线(Datafile Offline)的原理和表空间离线的原理是相同的。

- 表空间离线时，表空间中所有的数据文件都离线。我们也可以使表空间中的单个数据文件离线。
- 离线的数据文件不可访问。
- 在非归档（模式下，不要轻易离线数据文件，在这种模式下离线数据文件是很危险的，这样离线的数据文件可能永远也不能重新在线(Online).


##### 在归档模式下（ARCHIVELOG）使数据文件离线

```
ALTER DATABASE DATAFILE '数据文件路径' OFFLINE
```

**注: 在非归档模式下使用该语句时：**

- ORA-01145: 除非启用了介质恢复, 否则不允许立即脱机


##### 在非归档模式下（NOARCHIVELOG）使数据文件离线

- 使用FOR DROP选项，离线的数据库文件将永远不能再次在线，

```
ALTER DATABASE DATAFILE '数据文件路径' OFFLINE FOR DROP;
```

#### 使数据文件在线

- 在线的数据文件使该数据文件中的数据可以访问。

```
ALTER DATABASE DATAFILE '数据文件路径' ONLINE;
```

#### 移动数据文件

**查询当前数据文件路径**

```
SELECT file_name
      ,tablespace_name
FROM DBA_DATA_FILES;
```

**使该数据文件离线**

```
ALTER DATABASE DATAFILE '数据文件路径' OFFLINE;
```

**修改该数据文件在控制文件中的信息**

```
ALTER TABLESPACE 表空间名
RENAME DATAFILE '旧的数据文件路径名'
TO '新的数据文件路径名';
```

**使该数据文件重新在线**

```
ALTER DATABASE DATAFILE '数据文件路径' ONLINE; 
```

#### 删除数据文件

```
ALTER TABLESPACE 表空间名
DROP DATAFILE '数据文件路径';

--表空间只有一个数据文件时，该数据文件不能删除
ORA-03261: 表空间 MYTEST_TABLESPACE 只有一个文件
```

##### 删除临时文件

```
ALTER TABLESPACE 临时表空间名
DROP TEMPFILE '临时文件名';

ALTER DATABASE TEMPFILE '临时文件名' DROP
INCLUDING DATAFILES;
```

#### 查询数据库中的数据文件

```
SELECT file#
      ,name
FROM V$DATAFILE;
```

# ASM 自动存储管理

- 自动管理磁盘组，提供数据冗余。

## 概要

### ASM实例(ASM Instances)

- ASM实例有自己的内存（SGA）和后台进程（Background Processes）。
    - 数据库实例和ASM实例是两种不同的实例
- ASM实例用于加载磁盘组(DiskGroups)，使ASM文件有效，以便数据库实例能够访问磁盘组中的数据。
- 在启动ASM实例时，可以不启动数据库实例。但启动数据库实例时，ASM实例必须已经启动（如果存在ASM）
   - ASM实例必须先于数据库实例的启动，和数据库实例同步运行，迟于数据库实例的关闭。
- ASM实例与数据库实例可以是1: n的关系，（如果是n>1,则需要ASM_HOME）。

### ASM磁盘组(ASM Disk Groups)

- ASM磁盘组其实是一个虚拟的概念，
- 一个ASM磁盘组由多个磁盘组成。
- 一个ASM文件只能存放在一个磁盘组中，一个文件所获得的空间要从磁盘组中进行划分。
- 每个磁盘组都包含ASM元数据(ASM Metadata)。

![](C:/NoteBook/pictures/376745114239489.png =300x)

#### ASM元数据(ASM Metadata)

ASM元数据驻留在磁盘组(Disk Groups)中，Orace使用这些信息管理磁盘组。

**ASM元数据包括：**

- 一个磁盘属于哪个磁盘组
- 磁盘组上的有效空间
- 一个磁盘组中有哪些文件
- 区(Extent)的位置

#### ASM磁盘(ASM Disk)

ASM磁盘是真正存放数据的地方，

**一个ASM磁盘可以是：**

- 磁盘阵列上的一个磁盘或者分区
- 一整块磁盘或者来自一个磁盘上的分区
- 逻辑卷(Logical Volumes)
- 网络文件系统(Network-attached Files,NFS)

#### 分配单元(Allocation Unit,AU)

- 磁盘组中的每个磁盘会被分成多个单元(Allocation Units,AU)，AU是磁盘组中空间分配的单位。
- 在创建磁盘组时，可以指定AU的大小。可以是1MB、2MB、4MB、8MB、16MB、32MB或者64MB。
- AU是ASM存储中结构最小的元素。

### ASM文件(ASM Files)

- 存放在ASM磁盘组中的文件(ASM Disk Group)叫ASM磁盘文件，
- 一个ASM文件由多个区(Extent)组成。
- 一个ASM磁盘文件只能存放在一个磁盘组中，
- ASM自动产生ASM文件的文件名，我们也可以给ASM文件指定一个容易记忆的别名。

![](C:/NoteBook/pictures/275055414247522.png =228x)

**可以存放在磁盘组中的文件有：**

- 控制文件(Control files)
- 数据文件(Datafiles)、临时文件(Temporary Datafiles)和数据文件副本(Datafil Copies)
- 服务器参数文件(SPFILE)
- 重做日志文件(Online Redo Logs)、归档日志文件(Archive Logs)和闪回日志文件(Flashback Logs)
- RMAN生成的备份(RMAN Backups)
- 灾难恢复配置(Disaster Recovery Configurations)
- Change Tracking Bitmaps(改变跟踪位图)

### 区

- ASM磁盘组上创建文件时，最小的可寻址单元是区(ASM区和数据库的区概念不相同)。
- 每个区驻留在磁盘组中一个单独的磁盘，每个区由多个AU组成。
- 区的大小可变，区大小从1MB开始，文件达到某个阈值时，区大小增加至4MB，然后是16MB,最后当达到某个阈值后，区大小为64MB。
   - ASM实例会自动分配合适的区大小。
   - 较少数量的区即可容纳大量数据，减少共享池中的区总数，从而将性能提高。

![](C:/NoteBook/pictures/562945614240191.png =186x)

### ASM冗余 镜像和失败组(Mirroring and Failure Groups)

- ASM通过<mark>镜像算法</mark>来实现对数据的保护，Oracle提供三种冗余机制(Redundancy Level Controls)来保护数据
- 在一个磁盘组内，ASM会把每个区尽量分布在不同的ASM磁盘上，当要读取数据时，Oracle会<mark>并行读取</mark>多块磁盘中的数据。

![](C:/NoteBook/pictures/281610815247984.png =231x) 

**常规冗余(Normal Redundancy)**

![](C:/NoteBook/pictures/424270215236746.png =504x)

- 常规冗余(Normal Redundancy），只有两份相同的区(Extent)，一份存放在一个磁盘组中，另外一份存放在另一个磁盘组中。
- 常规冗余只能容忍一个磁盘组的损坏。

**高度冗余(High Redundancy)**

![](C:/NoteBook/pictures/188950515250380.png =573x)

- 高度冗余(High Redundancy)，只有3份相同的区(Extent),分别放在3个磁盘组中。
- 其中两个磁盘组损坏，也不会导致数据的丢失

**外部冗余(Extemal Redundancy)**

- 由外部存储系统实现

## ASM使用

### 配置ASM实例

#### Windows Server

- windows server
- 11g
- 11g_grid

##### ASM磁盘准备 

**1.准备磁盘分区**

- 不分配路径盘符
- 不格式化

![](C:/NoteBook/pictures/167412115226727.png =462x)

**2.使用asmtoolg标记磁盘分区**

- 运行-->asmtoolg
  - 路径：C:\app\zjk10\product\11.2.0\dbhome_1\BIN\asmtoolg.exe
- 需要以管理员权限运行
   - 否则 
![](C:/NoteBook/pictures/419713515249167.png =268x)

![](C:/NoteBook/pictures/587283915244303.png =402x)
![](C:/NoteBook/pictures/141724015237849.png =402x)

##### 安装grid

![](C:/NoteBook/pictures/427465918240191.png =579x)
![](C:/NoteBook/pictures/244490019236746.png =576x)
![](C:/NoteBook/pictures/309030119232500.png =583x)
![](C:/NoteBook/pictures/52480319250380.png =580x)
![](c:/notebook/pictures/Snipaste_2022-11-27_13-44-47.png =580x)
![](c:/notebook/pictures/Snipaste_2022-11-27_13-48-35.png =580x)

##### 安装11gDatabase

1. 安装服务器类
2. 使用自动内存管理
![](c:/notebook/pictures/Snipaste_2022-12-04_00-10-57.png =580x)
- 不启用自动备份
- 口令：tiger

##### OHAS的配置

**1. 查看ora.orcl.db配置文件**

- 路径  C:\app\Administrator\product\11.2.0\grid\BIN

- 命令：

```
C:\app\Administrator\product\11.2.0\grid\BIN\crsctl.exe modify resource ora.orcl.db -attr "AUTO_START=1"

C:\app\Administrator\product\11.2.0\grid\BIN\crs_stat.exe -p ora.orcl.db
```

```相当于
C:\app\Administrator\product\11.2.0\grid\BIN>crsctl enable
```

![](c:/notebook/pictures/Snipaste_2022-11-27_14-10-14.png =600x)

**2.查看各个资源状态**

```
C:\app\Administrator\product\11.2.0\grid\BIN\crs_stat.exe -t -v
```

![](c:/notebook/pictures/Snipaste_2022-11-27_14-12-09.png =600x)

**3.关闭ohas服务**

```
C:\app\Administrator\product\11.2.0\grid\BIN\crsctl.exe stop has
```

![](c:/notebook/pictures/Snipaste_2022-12-04_01-36-49.png =600x)

- 有出错。。。
![](c:/notebook/pictures/Snipaste_2022-11-27_14-15-41.png =600x)

**4.启动ohas服务**

```
C:\app\Administrator\product\11.2.0\grid\BIN\crsctl.exe start has
CRS-4123: Oracle High Availability Services 已启动。
```

##### 其他查看

- 以sysdba身份

**查看磁盘组**

```sql
SELECT name,state,total_mb
FROM v$asm_diskgroup;
```

![](c:/notebook/pictures/Snipaste_2022-11-27_14-28-58.png =400x)

**查看实例**

```sql
SELECT instance_name,status,database_status
FROM v$instance;
```

![](c:/notebook/pictures/Snipaste_2022-11-27_14-31-06.png =400x)

### ASM实例管理

**初始化参数**

| 参数             | 说明                                                         |
| :-------------- | :----------------------------------------------------------- |
| INSTANCE_TYPE   | 指定实例的类型，设置为ASM                                      |
| DB_UNIQUE_NAME  | ASM实例的名称，默认ASM+                                       |
| ASM_POWER_LIMIT | ASM实例的磁盘重新平衡的能力，默认为1                            |
| ASM_DISKTRING   | 指定一个字符串，ASM实例在创建磁盘组时按照这个字符串搜索可用的磁盘 |
| ASM_DISKGROUPS  | 指定磁盘组的名称，ASM实例启动时将自动加载这些磁盘组              |

#### 登入

1. SET ORACLE_SID=+ASM
2. sqlplus / as sysasm

```
C:\Users\Administrator>SET ORACLE_SID=+ASM
C:\Users\Administrator>sqlplus / as sysasm

SQL*Plus: Release 11.2.0.1.0 Production on 星期日 12月 4 01:47:21 2022

Copyright (c) 1982, 2010, Oracle.  All rights reserved.

连接到:
Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Automatic Storage Management option
```

#### 启动/关闭ASM实例

##### sqlplus

```sql
startup;
shutdown;
```

##### srvctl工具

```sql
srvctl status ASM; --ASM实例的状态
srvctl stop ASM;  --关闭ASM实例
srvctl start ASM; --启动ASM实例 
```

![](c:/notebook/pictures/Snipaste_2022-11-27_17-29-46.png =400x)

#### 开启CSS服务

- 用于同步ASM实例和数据库实例

**启动CSS服务**

```cmd
localconfig.bat -add
```

![](C:/notebook/pictures/Snipaste_2022-11-27_17-31-26.png =600x)

**检查是否运行正常**

```cmd
crsctl.exe check css
```

![](c:/notebook/pictures/Snipaste_2022-11-27_17-34-24.png =400x)

### 磁盘组管理

- 磁盘组，Oracle通过ASM技术将多个磁盘组织在一起，作为一个整体向数据库提供存储空间。在数据库运行时，数据将平均分布在磁盘组的各个磁盘。数据库管理员可以在不关闭数据库的情况下向磁盘组中添加/删除磁盘。在删除/添加磁盘时，自动存储管理将重新平衡磁盘，将数据重新平均发布在各个磁盘。
- 为获得最佳性能，在创建磁盘组时，应该将大小、性能相同或相近的磁盘放在一个磁盘组中，相反则放在其他磁盘组中。

#### 磁盘组信息

V$ASM_DISK

V$ASM_DISKGROUP

#### 创建磁盘组

##### asmtool 标记磁盘

- 即利用asmtool工具进行标记磁盘
- 或使用asmtoolg工具标记

###### asmtool -list 查看可用磁盘分区

![](c:/notebook/pictures/Snipaste_2022-11-27_17-27-23.png =600x)

- 标有ORCLDISK___0~n 的表示已被使用的磁盘组
- 若为空置的，则为可用磁盘分区（空余磁盘空间）

###### asmtool -add 标记ASM磁盘

```cmd
asmtool -add 指向 命名（即：ORCLDISK的后缀）
```

![](C:/notebook/pictures/Snipaste_2022-12-04_14-16-44.png =600x)


##### 查看ASM磁盘状态

```sql
SELECT path,mount_status
FROM V$ASM_DISK;
```

![](c:/notebook/pictures/Snipaste_2022-12-04_14-26-28.png =500x)

##### 创建磁盘组的同时加入新的ASM磁盘

```sql
CREATE DISKGROUP 磁盘组名 HIGH|NORMAL|EXTERNAL REDUNANCY 
--高/普通/外部冗余
[DISK 'ASM磁盘的path'; --EXTERNAL 只有一块磁盘时的外部冗余]
[FAILGROUP 失败磁盘组名 DISK 'ASM磁盘的path'
 FAILGROUP 失败磁盘组名 DISK 'ASM磁盘的path'
] --NORMAL|HIGH 普通/高冗余，需要失败磁盘组
```

![](c:/notebook/pictures/Snipaste_2022-12-04_14-36-28.png =500x)

###### 磁盘阵列的RAID盘

- 则不需要为磁盘组指定镜像。

###### 有失败组的普通冗余

- 如果是普通冗余，至少要两块ASM磁盘作为失败组，可以多。
- 如果是高冗余，则至少需要3块ASM磁盘作为失败组，可以多。

![](c:/notebook/pictures/Snipaste_2022-12-04_15-17-22.png =600x)

###### 一块磁盘的外部冗余

![](C:/notebook/pictures/Snipaste_2022-12-04_14-34-19.png =600x)

- 此时，其对应的mount_status变为CACHED
![](c:/notebook/pictures/Snipaste_2022-12-04_14-48-38.png =600x)

##### 将磁盘组加入到SPFILE

```sql
SHOW PARAMETER ASM_DISK;
```

![](c:/notebook/pictures/Snipaste_2022-12-04_14-50-20.png =550x)

- TYPE 标识为string的即为加入SPFIEL的。

**若TYPE没有标识，即为新加入的磁盘组，未加入SPFIEL**

1. 导出PFILE
![](c:/notebook/pictures/Snipaste_2022-12-04_14-56-09.png =300x)
2. 修改PFILE，在INIT_ASM.ora加入 `.asm_diskgroups='磁盘组名1',磁盘组名2`
![](C:/notebook/pictures/Snipaste_2022-12-04_14-57-57.png =450x)
3. 创建新的SPFIEL，并启动（需要先将连接的Oracle实例关闭，才能关闭ASM实例）
![](C:/notebook/pictures/Snipaste_2022-12-04_15-02-55.png =400x)
4. 检查：`SHOW PARAMETER ASM_DISK`

#### 删除磁盘组

- 删除磁盘组时，磁盘组上的所有文件将一起被删除。在删除时，ASM实例必须被启动，磁盘组必须被加载，并且磁盘组上的所有文件必须被关闭。
- 在删除之后，需要查看SPFILE是否改变。

**磁盘组中没有文件时**

```sql
DROP DISKGROUP 磁盘组名;
```

**磁盘组中有文件时**

```sql
DROP DISKGROUP 磁盘组名 INCLUDING CONTENTS;
```

![](C:/notebook/pictures/Snipaste_2022-12-04_15-34-02.png =800x)
![](C:/notebook/pictures/Snipaste_2022-12-04_15-36-31.png =500x)

#### 磁盘组添加/删除磁盘

##### 添加

**添加一个磁盘**

```sql
ALTER DISKGROUP 磁盘组名 ADD DISK 'ASM磁盘的path' NAME '对该磁盘命名';
```

**添加多个磁盘**

```sql
ALTER DISKGROUP 磁盘组名 ADD DISK 'ASM磁盘的path','ASM磁盘的path'...;
-- 或
ALTER DISKGROUP 磁盘组名 ADD FILEGROUP 失败组名 DISK 'ASM磁盘的path','ASM磁盘的path'...;
```

- 如果没有指定ASM磁盘所属的失败组，Oracle指定确定磁盘属于哪个磁盘组。
- FAILGROUP关键字指定要加入/创建的失败组

![](C:/notebook/pictures/Snipaste_2022-12-04_15-50-27.png =500x)

##### 删除

**删除一个磁盘**

```sql
ALTER DISKGROUP 磁盘组名 DROP DISK 磁盘名;
```

**删除失败组内所有磁盘**

```sql
ALETR DISKGROUP 磁盘组名 DROP DISKS IN FAILGROUP 失败组名;
```

###### UNDROP 取消删除

- 在删除完成前可以使用UNDROP进行取消

```sql
ALTER DISKGROUP 磁盘组名 UNDROP DISKS;
```

###### FORCE 强制删除，无法恢复

#### 磁盘组的重新平衡

- 当磁盘组中的磁盘数量改变时，ASM实例将对其自动进行一次重新平衡，将磁盘组中的内容重新平均分布到现有的各个磁盘上。
- 磁盘组的平衡能力分为12级从0~11，0表示停止平衡，11表示速度最快的平衡；指定的级别越高，需要消耗的系统资源就越多。**初始化参数ASM_POWER_LIMIT**限制可以使用的最高平衡级别，使高于这个值的级别的平衡操作无效。

```sql
ALTER DISKGROUP 磁盘组名 REBALANCE POWER 级别;
```

#### 磁盘组的加载/卸载

- 为了能访问磁盘组中的文件，必须在ASM实例启动时加载磁盘组。**初始化参数ASM_DISKGROUP**指定自动加载的磁盘组。当关闭ASM实例时，磁盘组被自动卸载。对于新创建的磁盘组，用户需要手动设置。

**手动加载**

```sql
ALTER DISKGROUP 磁盘组名 MOUNT;
--加载所有
ALTER DISKGROUP ALL MOUNT;
```

**手动卸载**

- 当磁盘组上有文件处于打开状态时，卸载将失败，此时使用FORCE关键字强制卸载。

```sql
ALTER DISKGROUP 磁盘组名 DISMOUNT;
--强制卸载
ALTER DISKGROUP 磁盘组名 DISMOUNT FORCE;
```

#### 目录管理

- 在磁盘组中包含一套完整的目录层次，在创建磁盘组时自动产生，数据库文件存储在这些目录中。每个文件名称由Oracle自动产生，称为系统别名。也可以指定用户别名，存储在用户创建的目录。

**用户别名**

- 每个目录以"+"号和磁盘组的名称为开始，包括完整路径。
- 在创建一个目录时，其上层目录必须存在。

##### 创建目录 

```sql
ALTER DISKGROUP 磁盘组名 ADD DIRECTORY '+磁盘组名\目录';
```

![](c:/notebook/pictures/Snipaste_2022-12-05_13-41-16.png =500x)

##### 重命名目录

```sql
ALTER DISKGROUP 磁盘组名 RENAME DIRECTORY '+磁盘组名\要重命名的目录' TO '+磁盘组名\新的目录名';
```

![](c:/notebook/pictures/Snipaste_2022-12-05_13-43-53.png =500x)

##### 删除目录

```sql
ALTER DISKGROUP mydg1 DROP DIRECTORY '+磁盘组名\要删除的目录' [FORCE];
```

- 如果目录中包含文件，那么需要FORCE关键字强制删除。
- 对于磁盘组中本就存在的文件（系统别名），无法删除

![](C:/notebook/pictures/Snipaste_2022-12-05_13-46-16.png =500x)

##### 查看目录内容

##### 别名

- 只能给文件起别名，目录不能起别名。

###### 指定别名

- 在添加用户别名时，需要提供该别名的完整路径。

```sql
ALTER DISKGROUP 磁盘组名 ADD ALIAS '+磁盘组\别名' FOR '+磁盘组\文件路径';
```

![](C:/notebook/pictures/)

###### 修改别名

```sql
ALTER DISKGROUP 磁盘组名 RENAME ALIAS '+磁盘组\旧别名' TO '+磁盘组\新别名';
```

![](C:/notebook/pictures/)

###### 删除别名

```sql
ALTER DISKGROUP 磁盘组 DELETE ALIAS '+磁盘组\要删除的别名';
```

![](C:/notebook/pictures/)

#### 文件操作

- 利用ASM管理数据库文件时，文件存储在磁盘组的磁盘中，磁盘组中的文件对操作系统是不可见的。

**Oracle实例的SPFILE**

![](C:/notebook/pictures/Snipaste_2022-12-05_14-06-17.png =650x)

**在创建表空间、数据文件等时，可以使用ASM磁盘作为存放的位置，与之前在Windows磁盘的使用相同**
**可以通过对SPFILE等启动文件的初始化参数的修改实现对存放位置等的修改或创建**

# Oracle 内存

![](C:/NoteBook/pictures/217564418221066.png =726x)

![](C:/NoteBook/pictures/407375618239492.png =562x)

## 数据库的内存结构

Oracle数据库的进程结构主要包括用户进程、服务器进程、程序全局区、数据库实例

### User Process(用户进程)

- 管理Oracle客户端的用户登录。
- 当用户运行一个应用程序时，系统就为它建立一个用户进程。

### Server Process(服务器进程)

- 帮助Oracle客户端连接到服务端。
   - 服务器进程处理与之相连的用户进程的请求，它与用户进程相通信，为相连的用户进程的Oracle请求服务。

### PGA (Program Global Area,程序全局区)

**管理每次会话的SQL执行。** PGA是一块独占内存区域，Oracle进程以专有的方式用他来存放数据和控制信息，当Oracle进程启动时，PGA就有Oracle数据库创建。当用户进程连接到数据库并创建一个对应的会话时，Oracle服务进程会为这个用户专门设置一个PGA区，用来存储这个用户会话的相关内容。当这个用户会话终止时，系统终端释放这个PGA区所占用的内存。

**PGA由以下几个部分组成。**

- Private SQL Area:私有SQL区。
- Session Memory:会话记忆区。
- SQL Work Areas:SQL工作区。

| 部分   | 说明                                        |
| :----- | :----------------------------------------- |
| 排序区 | 主要关注，在必要时手动调整                    |
| 会话区 | -                                          |
| 堆栈区 | -                                          |
| 游标区 | 动态的区域，在游标打开时创建，游标关闭时释放。 |

#### 结构

##### 四个分区

###### 排序区

**存放排序操作产生的临时数据** 当用户对数据进行排序时，数据库将需要排序的数据保存到PGA程序缓冲区的一个排序区中，在这个排序区中对这些数据进行排序。如果排序的数据大小为M，则这个排序区必须至少有M的空间容纳这些数据，且在排序过程中还需要M的空间来保存排序后的数据。如果排序区空间不足，则会将部分数据使用硬盘来读写，排序的效率降低。

- 排序区的大小占据PGA的大部分空间，通过<mark>初始化参数SORT_AREA_SIZE</mark>调整大小。

###### 会话区

**保存会话具备的权限、角色、性能统计等。由数据库自我维护，会话结束时自动释放。** 当用户进程和数据库建立会话时，系统将这个用户的相关权限查询出来，然后保存在这个会话区。用户进程访问数据时，系统会核对会话区内的用户权限信息。

###### 堆栈区

**保存绑定变量、会话变量、SQL语句运行时的内存结构等信息。由数据库自我维护，会话结束时自动释放。**

###### 游标区

**动态的区域，在游标打开时创建，游标关闭时释放。** 当运行时使用CURSOR时，数据库在PGA中为其分配一块区域，关闭CURSOR时，释放该区域。在创建和释放时，会占用一定的系统资源，因此使用CURSOR执行效率较低，应尽量减少使用

- 可以通过限制游标的数量来提高数据库性能，初始化参数OPEN_CURSORS，控制用户可以同时打开的CURSOR数量。但是如果硬件能够支持，应该将这个参数设置宽松，避免用户频繁地打开关闭游标，对游标区频繁的创建释放，影响数据库性能。


##### Private SQL Area

- Private SQL Area 保存每个Session私有的信息，而Shared Pool中有一个Public SQL Areaa，保存SQL执行计划等共享信息。
- Sersion Process每执行一个SQL都需要申请一个Private SQL Area(Cursor)。Server Process在执行SQL语句之前，必须在Shared Pool中定位语句的Share SQL Area。在PGA中，也是如此。如果定位失败，服务器必须为其分配一个Private SQL Area，并初始化，这个过程消耗大量CPU资源。
- PGA中可以有多个Private SQL Area，Server Process也会使用重用算法，增大Private SQL Area的重用，一个大的PGA可以减少对Private SQL Area的置换，减少CPU开销。
  
**Private SQL Area分为Persistent Area和Run-Time Area.**

**Persistent Area**

- 存放绑定变量，数据类型转换等Cursor结构信息，Cursor关闭时，该区域释放。
- 当一个SQL语句执行后，Run-Time Area就被释放，而Private SQL Area可以被其他SQL语句重用，重用时必须初始化。

**Run-Time Area**

- 在SQL运行时使用，大小依赖于SQL语句操作方式，处理数据行数和每行记录大小。如果是DML语句，执行完就释放。如果是SELECT语句，在记录全部传给客户端或取消查询后才释放。

###### Private SQL Area寻找Cursor的过程

1. 是否存在某个OPEN CURSOR，是：执行，否：继续下一步。
2. 是否存在SESSION CACHED CURSOR，是：执行，否：继续下一步。
3. 是否存在HOLD CURSOR，是：执行，否：继续下一步。
4. OPEN CURSOR，继续下一步
5. 检索SQL AREA，继续下一步
6. 是否可重用判断，是：软解析，否：硬解析。

##### Work Area

- 对于复杂的查询，PGA的很大一部分被内存需要很大的操作分配给Work Area。
   - 基于排序的操作：ORDER BY、GROUP BY、ROLLUP、窗口函数，参数SORT_AREA_SIZE
   - Hash join，参数HASH_AREA_SIZE
   - Bitmap merge，参数BITMAP_MERGE_AREA_SIZE
   - Bitmap create，参数CREATE_BITMAP_AREA_SIZE
- 如果操作所处理的数据量大于Work Area的大小，则将输入的数据分为一些更小的数据片，使一些数据能够在内存中处理，而其他的则在临时表空间的磁盘上稍后处理。虽然不会将数据放在硬盘上处理，但是操作的复杂度和Work Area的大小成反比。大的Work Area，使一些特定的操作性能更佳，但同时也消耗更大的内存。

##### Session Memory

- Session Memory保存会话中的变量和其他与会话相关的信息。在共享服务器模式下，该Session Memory是共享的。

###### 会话信息查看

```sql
SELECT server "服务器模式"
      ,s.username "用户"
      ,osuser "操作系统用户"
      ,name "内存名称"
      ,ROUND(value/1024/1024,4) "占用内存MB"
      ,s.sid "会话ID"
      ,s.serial# "SESSION序列号"
      ,spid "操作系统进程ID"
FROM V$SESSION s
    ,V$SESSTAT st
    ,v$STATNAME sn
    ,v$PROCESS p
WHERE st.sid = s.sid
 AND st.statistic# = sn.statistic#
 AND sn.name LIKE 'session pga memory'
 AND p.addr = s.paddr
ORDER BY value DESC;
```

#### 设置

##### 自动PGA管理 PGA_AGGREGATE_TARGET

- 设值PGA_AGGREGATE_TARGET为非0值（为PGA开辟的总内存），则启用PGA自动管理，并忽略所有`*_AREA_SIZE`的设置。
- 默认为启用PGA的自动管理，Oracle根据SGA的20%来动态调整PGA中专用与Work Area部分的内存大小。

###### PGA_AGGREGATE_TARGET的设置

- PGA_AGGREGATE_TARGET设置为SGA的20%

```sql
ALTER SYSTEM SET PGA_AGGREGATE_TARGET=1200M SCOPE=BOTH;
```

**查看Oracle建议的PGA_AGGREGATE_TARGET的大小**

1. 查看所有会话的PGA占用情况

```sql
SELECT p.spid
      ,b.name
      ,s.program
      ,s.sid
      ,s.serial#
      ,s.osuser
      ,s.machine
FROM V$PROCESS p
    ,V$SESSION s
    ,V$BGPROCESS b
WHERE p.addr = s.paddr
 AND p.addr = b.paddr
UNION ALL
SELECT p.spid
      ,s.username
      ,s.program
      ,s.sid
      ,s.serial#
      ,s.osuser
      ,s.machine
FROM V$PROCESS p
    ,V$SESSION s
WHERE p.addr = s.paddr
 AND s.username IS NOT NULL;
```
  
2. 查看当前会话

```sql
SELECT s.username,s.sid,s.serial#
FROM V$LOCKED_OBJECT lo,DBA_OBJECTS ao,V$SESSION s
WHERE ao.object_id = lo.object_id
 AND lo.session_id = s.sid;
```

3. 查看某会话PGA占用情况

```sql
SELECT a.name, b.value 
FROM v$statname a, v$sesstat b 
WHERE a.statistic# = b.statistic# 
 AND b.sid = &sid 
 AND a.name like '%ga %' 
ORDER BY a.name;
```

**建议PGA大小**

```sql
select round(pga_target_for_estimate /(1024*1024)) "预测PGA_AGGREGATE_TARGET值M"
      ,estd_pga_cache_hit_percentage "Est.Cache Hit %"
      ,round(estd_extra_bytes_rw/(1024*1024)) "Est.ReadWrite(M)"
      ,estd_overalloc_count "Est.Over-Alloc" 
from v$pga_target_advice ;
```

- 或

```sql
SELECT 'PGA AGGREGATE Target' "条目"
      ,ROUND (pga_target_for_estimate / 1048576) "目标值(M)"
      ,estd_pga_cache_hit_percentage "相关缓存命中率"
      ,ROUND ( ( ( estd_extra_bytes_rw / DECODE ((b.BLOCKSIZE * i.avg_blocks_per_io),0,1, (b.BLOCKSIZE * i.avg_blocks_per_io)))* i.iotime)/100 ) "响应时间(秒)" 
FROM v$pga_target_advice
    ,(SELECT /*+AVG TIME TO DO AN IO TO TEMP TABLESPACE*/  
             AVG ( (readtim + writetim) / DECODE ((phyrds + phywrts), 0, 1, (phyrds + phywrts)) ) iotime
            ,AVG ( (phyblkrd + phyblkwrt)/ DECODE ((phyrds + phywrts), 0, 1, (phyrds + phywrts))) avg_blocks_per_io     
      FROM v$tempstat) i
    ,(SELECT /* temp ts block size */ VALUE BLOCKSIZE 
      FROM v$parameter 
      WHERE NAME = 'db_block_size') b;
```

##### 禁用自动PGA管理

**调整以下参数**

- BITMAP_MERGE_AREA_SIZE
- CREATE_BITMAP_AREA_SIZE
- HASH_AREA_SIZE
- SORT_AREA_SIZE

### UGA 用户全局区

- 在专用服务器模式下，进程和会话一对一，UGA被包含在PGA内。
- 在联机服务器模式下，进程和会话一对多，UGA不再属于PGA而是在Large Pool 中分配，当如果从Large Pool中分配失败，则使用Shared Pool分配。

### SID 数据库实例(Instance)

![](C:/notebook/pictures/Snipaste_2022-12-09_15-30-45.png =600x)

**数据库实例分为两个部分：SGA和Background Process(后台进程)。**

- 当在计算机服务器上启动Oracle数据库后，称服务器上启动了一个Oracle实例：
- Oracle实例是存取和控制数据库的软件机制，它包含系统全局区(SGA)和Oracle进程两部分：
    - SGA是系统为实例分配的一组共享内存缓冲区，用于存放数据库实例和控制信息，以实现对数据库中数据的治理和操作。

**进程**

- 一个进程执行一组操作，完成一个特定的任务
- 对Oracle数据库治理系统来说，<mark>进程由用户进程、服务器进程和后台进程</mark>所组成：
    - 当用户运行一个应用程序时，系统就为它建立一个用户进程。
    - 服务器进程处理与之相连的用户进程的请求，它与用户进程相通信，为相连的用户进程的Oace请求服务。
    - 为了提高系统性能，更好地实现多用户功能，Oracle还在系统后台启动一些后台进程用于数据库数据操作。

#### SGA(Systerm Global Area,系统全局区)

- SGA是Oracle为一个实例分配的一组共享内存缓冲区，它包含该实例的数据和控制信息：SGA在实例成功时被自动分配，当实例关闭时被收回。数据库的所有数据操作都要通过SGA来进行。

##### SGA操作

###### 查询SGA

```sql
SHOW SGA;
```

![](C:/notebook/pictures/Snipaste_2022-12-03_19-36-53.png =680x)

###### 调整SGA大小

```sql
ALTER SYSTEM
SET DB_CACHE_SIZE = 1500M SCOPE=BOTH;
```

- SCOPE参数：

| SCOPE参数     | 说明                        |
| :----------- | :------------------------- |
| SCOPE=BOTH   | 默认，内存和SPFILE都更改     |
| SCOPE=MEMORY | 仅仅更改内存，下次启动失效    |
| SCOPE=SPFILE | 仅仅更改SPFILE，下次启动生效 |

##### SGA构成

###### Shared Pool(共享池)

- 存放SQL语句、PL/SQL代码、数据字典、资源锁和其他控制信息。
- 由初始化参数SHARED_POOL_SIZE控制大小
- 它包含<mark>库缓冲区(Library Cache)、数据字典高速缓存区(Data Dictionary Cache)、结果高速缓存区(Result Cache)。</mark>

**Library Cache**

- 保存最近解析过的SQL语句、PL/SQL过程和包。Oracle在执行语句之前，先在Library Cache中搜索，如果存在已经解析的，则直接使用Library Cache中保存的解析结果和执行计划，而不必再次解析。
- 将每一条SQL语句分为可共享和不可共享。
  - 共享SQL区：存储最近执行的SQL语句、解析后的语法树、优化后的执行计划。Oracle在执行一条新语句时，会为其在共享SQL区中分配空间，分配的大小取决于SQL语句的复杂度。如果共享SQL区中没有空闲空间，则使用LRU算法释放被占用的空间。
  - 私有SQL区（共享模式时）：存储执行SQL语句时与每个会话或用户相关的私有信息。其他会话执行相同的SQL语句时，不会用到这些私有信息（如：绑定变量、环境、会话参数）。

**Data Dictionary Cache**

- 存储经常使用的数据字典信息，它为所有用户进程所共享。Oracle运行过程中进程访问该缓存以便解析SQL语句，确定操作对象的存在、权限等，如果不在Data Dictionary Cache中，则从保存数据字典信息的数据文件中将其读入Data Dictionary Cache。
- 保存的是一条一条的记录，而不是数据块信息。

**Result Cache**

- 存储SQL查询和PL/SQL函数的结果，包含SQL查询结果高速缓存和PL/SQL函数结果高速缓存。

**锁与其他控制结构** 存储Oracle例程内部操作所需信息，如锁、闩、寄存器值等。

**参数指定**

```sql
ALTER SYSTEM
SET SHARED_POOL_SIZE=1200M SCOPE=BOTH;
```

###### Database Buffer Source (数据库缓冲资源/块缓冲区)

- 存储从数据文件中读取的数据块，由<mark>初始化参数DB_CACHE_SIZE</mark>决定。
- 工作原理：<mark>LRU</mark>。查询时，Oracle先把从磁盘读取的数据放入内存供所有用户共享。插入和更新时，Oracle会先在该区中缓存数据，之后再批量写入磁盘。

**缓存块**

- 在Database Buffer Source中存放数据库中数据库块的复制，它由一组缓冲块所组成，这些缓冲块为所有与该实例相连接的用户进程所共享。
- 缓冲块的数目由初始化参数DB_BLOCK_BUFFERS确定，
- 缓冲块的大小由初始化参数DB_BLOCK_SIZE确定。
- 大的数据块可提高查询速度。它由DBWR操作

**分类**

1. <mark>**Dirty Buffers 脏缓存块**</mark>
   - 保存修改过的缓存块，即当一条SQL语句对某个缓存块中的数据进行修改之后，该缓存块就被标记为脏缓存块，被**DBWn进程**写入硬盘的数据文件。
2. **Pinned Buffers 命中缓存块**
   - 保存最近正在被访问的缓存块，始终保留在Database Buffers中。   
3. **Free Buffers 空闲缓存块**
   - 没有数据，等待数据写入。Oracle从数据文件读取数据之后，寻找Free Buffers以写入。

**管理**

1. **DIRTY列表** 保存已经被修改但还没有被写入数据文件的Dirty Buffers
2. **LRU列表** 保存所有的Buffers（包括还未移到DIRTY列表的Dirty Buffers、Pinned Buffers、Free Buffers）。当某个Buffers被访问之后，该Buffers就移到LRU列表的头部，其他Buffers则向尾部移动，放在最尾部的最先被移出。

###### Java Pool(Java池)

-  为JVM和基于Java的应用而开辟的内存空间。保存了JVM中特定会话的Java Code和数据。
- 由JAVA_POOL_SIZE控制大小。
- 在编译数据库中的Java代码和使用数据库中的Java资源对象时，会用到Java Pool，Java的类加载程序对每个加载的类会使用约8K的空间。系统跟踪运行过程中，动态加载的Java类也会使用到Java Pool。

###### Large Pool(大池)

- 可选。<mark>Large Pool起隔离作用</mark>，如果没有Large Pool，则（需要大量内存空间的）操作将占用Shared Pool，对Shared Pool造成一定的性能影响。

**需要大量内存的操作**

- 数据库备份和恢复，如RMAN
- 具有大量排序操作的SQL语句
- 并行化的数据库操作（并行查询），存放进程间的消息缓冲区
- 共享服务器下UGA在Large Pool分配（如果有Large Pool）

**LAGER_POOL_SIZE参数 设置Large Pool大小**

```sql
ALTER SYSTEM
SET LAGER_POOL_SIZE=20M SCOPE=BOTH;
```

###### Redo Log Buffer(日志缓冲区)  

- 服务进程从用户空间复制每条DML/DDL语句的REDO条目到REDO LOG BUFFER中，用于存放日志条目（记录对数据的改变）。当这块区域用完，**后台进程LGWR**把日志条目写到磁盘的联机日志文件中。
- Redo Log Buffer的大小由<mark>初始化参数LOG_BUFFER</mark>决定。对于长时间运行的事务，应该将Redo Log Buffer的大小尽量设大。
- 可以循环使用，服务进程复制新的REDO覆盖Redo Log Buffer中已经通过LGWR写入磁盘（ONLINE REDO LOG）的条目。

**LGWR执行写Redo Log Buffer到Online Redo Log的条件：**

1. 每3秒
2. 缓存达到1MB或1/3
3. COMMIT
4. DBWn将修改的缓存区写入磁盘时（如果相应的Redo Log Buffer未尚未写入磁盘）。

###### Stream Pool(流池缓存)

- 流池会用于缓存流进程在数据库间移动/复制数据时使用的队列消息。（如果没有流池，则是Shared Pool中至多10%的空间）
- 由Oracle Streams使用，加强对流的支持。一般由STREAM_POOL_SIZE控制。


###### Keep Buffer Cache(保持缓存)

- 保存Buffer Cache中存储的数据，使其尽可能时间长

###### Recycle Buffer Cache(回收缓存)

- 保存Buffer Cache中即将过期的数据

###### nK Block Size Buffer(非标准块缓存)

- 为与数据库默认数据块大小不同的数据块提供缓存，用来支持表空间的传输

#### 后台进程

##### SMON(系统监控进程)

- 负责完成自动实例恢复和回收分类(Sot)表空间

##### PMON(进程监控进程)

- 实现用户进程故障恢复、清理内存区和释放该进程所需资源等

##### DBWn(数据库写进程)

- 数据库缓冲区的治理进程。在它的治理下，数据库缓冲区中总保持有一定数量的自由缓冲块，以确保用户进程总能找到供其使用的自由缓冲块（数据库实例与物理文件的连接)
- 对于大多数情况下，一个DBW0就足够，可以通过DB_WRITER_PROCESS初始化参数修改，最多20个，DBW0~DBW9和DBWa~DBWj。主要取决于CPU来设置数量，对于单CPU而言，配置额外的DBWR进程对性能提高无用。
- 将Buffer Cache中的Dirty Buffer写入磁盘中。根据LRU从Buffer Cache的冷端批量写入Dirty Buffer，该操作是异步进行的。

**满足以下条件时，DBWn将Dirty Buffer写入磁盘**

1. 当Server Process扫描了一定数量的Buffer之后，没有找到Free Buffer。
2. DBWn周期性的写Buffer，使得checkpoint前移，即实例恢复是重做日志的起始位置前移，该位置由最早的Dirty Buffers决定。

##### LGWR(日志文件写进进程)

- 每个实例只有一个LGWR进程
- LGWR是日志缓冲区的治理进程，负责把日志缓冲区中的日志项写入磁盘中的日志文件上。当LGWR将日志缓冲区的重做条目写入日志文件，Server Process同时将新的条目复制到日志缓冲区，覆盖已经被LGWR写入日志文件的重做条目。
- LGWR同步地向一个日志组的多个镜像成员写入。如果其中一个成员文件损坏，则LGWR继续向其他成员文件写入，并将错误记录到LGWR进程的trace文件和 alert log文件。如果一个日志组的所有成员文件都损坏或日志组未归档而暂时不可用，则LGWR无法继续工作。
- **快速提交机制**当用户执行了COMMIT，LGWR将提交记录放进日志缓冲区，并将之与事务的重做条目一起立即写入磁盘，而相关的被修改的数据块（Dirty Buffer）要等待更高效的时机才写入磁盘。一个事务的提交记录及相关的重做条目将通过一个原子性的写操作记录到磁盘，这个单一事件决定了事务是否被成功提交。此时被修改的数据缓冲区还未写入磁盘，但Oracle已经返回事务提交成功。如果重做日志缓存区的空间不足，那么LGWR在事务提交（COMMIT）之前就会将重做日志条目写入磁盘，这样的重做日志条目只有在相关事务提交之后才能永久存储。
- 当一个用户提交一个事务时，这个事务就被赋予一个**系统改变号SCN**，记录在redo log中，故恢复操作可以在RAC、分布式数据上同步进行。
- **批量提交**：当一个事务的重做条目被写入磁盘时，在此期间处于等待提交的事务的重做条目可以被一起写入磁盘。

**执行LGWR写入的条件**

1. 每3秒写入一次
2. 当日志缓冲区使用了1/3
3. 当DBWR将Dirty Buffer写入磁盘，但需要的日志还未写入

###### 先写日志原则 write-ahead protocol

- 执行LGWR写入的条件3：当DBWR将Dirty Buffer写入磁盘，但需要的日志还未写入
- 在DBWn向磁盘写入Dirty Buffer时，所有与修改数据相关的重做记录都必须被写入日志文件，即：write-ahead protocol。如果DBWn发现存在没有写入的重做条目，则通知LGWR写入，并等待LGWR完成写入，才继续执行DBWn的写入。

##### CKPT(检查点进程)



##### ARCn(归档进程)
 
- 把已经填满的在线日志文件复制到一个指定的存储设备上。
- 仅当日志文件组开关(Switch)出现时，才进行ARCH操作。
- ARCH不是必需的，而只有当自动归档可使用或者当手工归档请求时才发出

##### RECO(恢复进程)
     
- 是在具有分布式选项时使用的一个进程，主要用于解决引用分布式事务时所出现的故障。
- 它只能在答应分布式事务的系统中出现

##### LCKn(封锁进程)

- 用于并行服务器系统，主要完成实例之间的封锁

## 内存工作机制

**1. 检查共享SQL区有无该会话发出的SQL语句**

- 如果有：<mark>Library Cache Hit</mark> 在该共享SQL区中执行该会话的SQL语句
- 如果无：<mark>Library Cache Miss</mark> 为该SQL语句分配共享SQL区，同时将该语句指定给这些SESSION的Private Area

**2. 检查Share Pool（共享服务器模式）或PGA（专用服务器模式）中的Dictionary Cache中有无要访问的表/视图信息**

- 若无：<mark>Rowcache Miss</mark> 将其读入Dictionary Cache

**3. 检查Database Buffer Cache 当前有无Dictionary Cache被 Library Cache Hit**

- 有：<mark>Data Buffer Hit</mark> 使用当前的缓存
- 无：<mark>Date Buffer Miss</mark> 为该数据请求新的缓存，并进行以下：
   -  <mark>**逻辑读**</mark>：搜索LRU List；发现Dirty Buffer 则写入Dirty List，并继续搜索；发现Free Buffer 则 将其分配给Session，同时移至MRU List；若未能搜索到Free Buffer，则触发DBWR进程，将一些Dirty Buffer写入磁盘，并将这部分释放为Free Buffer。

**4. 如果是DML操作，系统为其分配Redo Log Buffer，记录数据变更情况**

- 当Redo Log Buffer中无free buffer时，触发LGWR进程，将Redo Log Buffer中的部分数据写入Log File


# 性能优化

## 执行计划

### 概念

SQL执行计划的概念主要包括ROWID(伪列)、Recursive SQL(递归SOL)、RomSource And Predicate(行源和谓词)、Driving Table(驱动表)、Probed Table(被查表)、Concatenated Index(组合索引)、Selectivity(可选择性)、物理读(Physical Reads)、逻辑读(Logical Reads)、一致性读(Consistant Get)、读一致性、当前读(Db Block Gets)等

**ROWID(伪列)**

- ROWID是一个伪列，既然是伪列，那么这个列就不是用户定义，而是系统给加上的。

- 对每个表都有一个ROWID的伪列，但是表中并不物理存储ROWID列的值。
- 不过可以像使用其他列那样使用它，但是不能删除此列，也不能对此列的值进行修改、插入。
   - 一旦一行数据插入数据库，则ROWID在该行的生命周期内是唯一的，即使该行产生行迁移，但其行的ROWID值不会改变。
   

**Recursive SQL(递归SQL)**

- 有时为了执行用户发出的一个SQL语句，Oracle必须执行一些额外的操作或者语句，将它们称为“Reeursive Calls”或“Recursive SQL Statements”,
  - 当一个DDL语句发出后，Oracle总是隐含地发出一些Recursive SQL语句，来修改数据字典信息，以便用户可以成功地执行该DDL语句。
  - 当需要的数据字典信息没有在共享内存中时，经常会发生Recursive Calls,这些Recursive Calls会将数据字典信息从硬盘读入内存中。
  - 用户不必关心这些Recursive SQL语句的执行情况，在需要时，Oracle会自动在内部执行这些语句。
- DML语句引起RecursiveSQL的可能性最大。
  - 简单地说，可将触发器视为Recursive SQL.

**Row Source And Predicate(行源和谓词)**

- Row Source(行源)：
   - 用于查询中，由上一操作返回的符合条件的行的集合，
      - 既可以是表的全部行，也可以是表的部分行；
      - 还可以是对以上2个Row Source(行源)进行连接操作（如join连接）后得到的行。
- Predicate(谓词)：一个查询中的WHERE限制条件。

**Driving Table(驱动表)**

- 驱动表又称外层表(Outer Table)。
- 此表用来嵌套于Hash连接中。
- 如果此Row Source返回较多的行数据，则对所有的后续操作有负面影响。
- 注意，与其说驱动表，不如说驱动行源(Driving Row Source)更为确切。
   - 一般来说，在应用查询的限制条件后，返回较少行源的表作为驱动表，
   - 如果一个大表的WHERE条件有限制条件（如等值限制），则该大表作为驱动表也是合适的，
      - 所以并不是只有较小的表可以作为驱动表。
   - 在执行计划中，应为最右最上的那个Row Source,后面会给出具体说明。

**Probed Table(被探查表)**

- Probed Table(被探查表)：该表又称内层表(Inner Table)。
- 在从驱动表中得到具体行的数据后，再在该表中寻找符合连接条件的行。
- 所以该表应当为大表（实际上应为返回较大Row Source的表)且相应的列上应该有索引。

**Concatenated Index(组合索引)**

- 由多个列构成的索引，
   - 如`CREATE index sy_idx_emp on emp(coll,col2,col3,)`,则称idx_emp索引为组合索引。
- 在组合索引中有一个重要的概念：引导列(leading column
    - 在上面的例子中，col1列为引导列。
    - 当进行查询时可以使用“where col1=?”,也可以使用“where col1=? and col2=?”,这样的限制条件会使用索引，
    - 但是“where col2=?”查询不会使用该索引。
    - 所以，限制条件中包含先导列时，该限制条件才会使用该组合索引。

**Selectivity(可选择性)**

- 比较列中唯一键值的数量和表中的行数，就可以判断该列的可选择性。
   - 如果该列的“唯一健的数量表中的行数”的比值越接近1，则该列的可选择性越高，
   - 即重复度越小，该列就越适合创建索引，其索引的可选择性也越高。
- 在可选择性高的列上进行查询时，返回的数据就较少，比较适合使用索引查询。

**物理读(Physical Reads)**

- 从做盘读取数据块到内存的操作称为物理读，当SGA的高速缓存(Cache Buffer)中不存在这些数据块时，就会产生物理读。
- 另外，像全表扫指、磁盘样序等操作也可能产生物理读，
   - 原因是Orle数据库需要访问的数据块较多，而有些数据块不在内存当中，需要从磁盘读取。

**逻辑读(Logical Reads)**

- 概念1:
   - 逻辑读是指Oracle从内存读到的数据块数量。
   - 一般来说，Logical Reads=Db Block Gets(当前读)+Consistent Gets(一致性读)。
- 概念2：
   - 逻辑读是指从Buffer Cache中读取数据块。
   - 按照访问数据块的模式不同，可分为当前模式读(Current Read)和一致性读(Consistent Read)。
      - 这两个概念本质相同，只是措辞不同。
      
**一致性读(Consistent Gets)**

- 一致性读是面向多个会话的，即所有当前会话都要进行一致性读。
- Oracle是一个多用户系统，当一个会话开始读取数据还未结束读取之前，可能会有其他会话修改了它将要读取的数据。如果会话读取到修改后的数据，就会造成数据的不一致。一致性读就是为了保证数据的一致性。
   - 在Buffer Cache中的数据块上都会有最后一次修改数据块时的SCN(System Change Number:系统改变号)。
   - 如果一个事务需要修改数据块中的数据，会先在回滚段中保存一份修改前的数据和SCN编码的数据块，然后更新Buffer Cache中数据块的数据及其SCN，并标识其为“脏”数据。当其他进程读取数据块时，会先比较数据块上的SCN和进程自己的SCN。
        - 如果数据块上的SCN小于等于进程本身的SCN，则直接读取数据块上的数据；
        - 如果数据块上的SCN大于进程本身的SCN,则会从回滚段中找出修改前的数据块读取数据。
- 通常，普通查询都是一致性读。

**读一致性(Read Consistency)**

- 读一致性是针对众多会话中的一个会话而言的，是一个会话的查询所获得的数据必须来自同一时间点。
- Oracle针对这个会话，在必要时会使用Undo数据来构造CR(Consistant Read)块，从而提供非阻塞的查询。

**当前读(Db Block Gets)**

- 通常情况下，当前读(Db Block Gets)可以理解为DML操作产生的.
- 当前读(Db Block Gets)即读取数据块是当前的最新数据。任何时候在Buffer Cache中只有一份当前数据块。
- 当前读通常发生在对数据进行修改、删除操作时。这时，进程会给数据加上行级锁，并且标识数据为“脏”数据。
- Current Mode产生Db Block Gets，一般在DML操作时产生，Query Mode产生Consisten Gts(一致性读)，一般在查询时产生。它们两个总和一般称为逻辑读(Logical Read).

### `SQL*PLUS` 执行计划的设置

#### TIMING参数（SQL运行时间）

- SET TIMING ON
  - 显示运行时间
- SET TIMING OFF
  - 屏蔽运行时间  默认   

#### AUTOTRACE参数（自动跟踪参数）

- `SET AUTOTRACE OFF`
   -  默认，关闭自动跟踪
- `SET AUTOTRACE ON`
   - 包含执行计划和统计信息 
- `SET AUTOTRACE ON EXPLAIN`  
   - 只显示优化器执行路径报告（执行计划）
- `SET AUTOTRACE ON STATISTICS` 
   - 只显示执行统计信息
- `SET AUTOTRACE TRACEONLY` 
   - 同set autotrace on，但是不显示查询输出
- `SET AUTOTRACE TRACEONLY EXPLAIN`
- `SET AUTOTRACE TRACEONLY STATISTICS`

**注：**

1. 需要权限`GRANT select any dictionary TO user_name`; 
2. 只能在sql plus下执行 

#### 在`SQL*PLUS`中显示SQL执行计划

**查看预估的执行计划（可能与实际不符）**

```sql
EXPLAIN PLAN FRO SQL语句;

SELECT plan_table_output
FROM TABLE(DBMS_XPLAN.DISPLAY('PLAN_TABLE'));
```

**查看实际的执行计划**

```sql
SET AUTOTRACE ON;
SET AUTOTRACE TRACEONLY;
```

#### 执行计划信息

##### 执行计划

```sql
执行计划
----------------------------------------------------------
Plan hash value: 1139150879

---------------------------------------------------------------------------------------------
| Id  | Operation                     | Name        | Rows  | Bytes | Cost (%CPU)| Time     |
---------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT              |             |   106 |  3286 |     7  (29)| 00:00:01 |
|   1 |  HASH GROUP BY                |             |   106 |  3286 |     7  (29)| 00:00:01 |
|   2 |   MERGE JOIN                  |             |   106 |  3286 |     6  (17)| 00:00:01 |
|   3 |    TABLE ACCESS BY INDEX ROWID| DEPARTMENTS |    27 |   432 |     2   (0)| 00:00:01 |
|   4 |     INDEX FULL SCAN           | DEPT_ID_PK  |    27 |       |     1   (0)| 00:00:01 |
|*  5 |    SORT JOIN                  |             |   107 |  1605 |     4  (25)| 00:00:01 |
|   6 |     TABLE ACCESS FULL         | EMPLOYEES   |   107 |  1605 |     3   (0)| 00:00:01 |
---------------------------------------------------------------------------------------------
```

**Id** 

- 序号，不是执行的先后顺序
  - 执行的先后顺序根据缩进来判断。
- Id列中`*`的含义
   - `*`对应步骤有驱动或过滤条件。

**Operation**

- 当前操作的内容
    - 该列反映SQL语句在每个步骤上都具体做了什么操作，如全表扫描、索引扫描、分区扫描、哈希连接、合并连接、嵌套循环等，是重点关注的信息。
    - 一般来讲，如果表上存在索引而走了表扫描，说明该SQL语句存在问题或者执行计划采集到的统计信息过旧导致，要具体问题具体分析。
- 执行顺序判断 
    - 按“Operation(操作)”列的缩进长度来判断，缩进最大的最先执行，
    - 如果有n行缩进一样，那么就先执行上面的，即最右最上原则。
    
**Name**

**Rows** 

- 当前操作的rows，oracle估计当前操作的返回结果集
- Rows值表示CBO预期从一个行源(Row Source)返回的记录数，这个行源可能是一个表，一个索引，也可能是一个子查询。
- Rows值对于CBO做出正确的执行计划来说至关重要。
    - 如果CBO(Cost-Basd Optimization,基于成本的优化)获得的Rows值不够准确，通常是没有做分析或者分析数据过旧造成，在执行计划成本计算上就会出现偏差，从而导致CBO错误地制订出执行计划，
    - 在多表关联查询或者SQL中有子查询时，每个关联表或子查询的Rows值对主查询的影响非常大，甚至可以说，CBO就是依赖于各个关联表或者子查询Rows值计算出最后的执行计划。
        - 对于多表查询，CBO使用每个关联表返回的行数(Rows)决定用什么样的访问方式来做表关联，如NESTED(嵌套)LOOPS Join或HASH(哈希)Join或MERGE(合并)Join
        - HASH(哈希)Join
        - MERGE(合并)Join
        - NESTED(嵌套)LOOPS Join
   - 对于子查询，它的Rows将决定子查询是使用索引还是使用全表扫描的方式访问数据。
        
**Cost(%CPU)**

- Oracle计算出来一个数值（代价），用于说明SQL执行的代价

**Time** 

- Oracle估计当前操作的时间

##### 谓词说明

```
Predicate Information (identified by operation id):
---------------------------------------------------

   5 - access("E"."DEPARTMENT_ID"="D"."DEPARTMENT_ID")
       filter("E"."DEPARTMENT_ID"="D"."DEPARTMENT_ID")
```

**access**
 
- 表示谓词条件的值将会影响数据的访问路径（表还是索引）。
- 在谓词中主要注意access,要考虑谓词的条件，使用的访问路径是否正确。

**Filter**

- 表示谓词条件的值不会影响数据的访问路径，只起过滤的作用。

##### 统计信息

```
统计信息
----------------------------------------------------------
          0  recursive calls
          0  db block gets
          9  consistent gets
          0  physical reads
          0  redo size
       3721  bytes sent via SQL*Net to client
        589  bytes received via SQL*Net from client
          8  SQL*Net roundtrips to/from client
          1  sorts (memory)
          0  sorts (disk)
        104  rows processed
```

**recursive calls**

- 在用户和系统级别生成的递归调用数。
- Oracle数据库维护用于内部处理的表，当需要更改这些表时，Oracle数据库生成一个内部SQL语句，该语句又生成一个递归调用。简言之，递归调用是SQL中的SQL。因此，如果必须解析查询，则可能需要运行其他查询才能获得数据字典信息，这些导致递归调用。空间管理、安全检查、从SQL调用SQL等都会产生递归SQL调用。
- 此项指标，需重点关注。

**db block gets**

- 请求的数据块在Buffer能满足的个数即“当前读”。
- 从BUFFER CACHE的block数量中，当前请求的块数目，当前请求的块数目就是在操作中直接提取的块数目，而不是在一致性读的情况下产生的。
   - 正常情况下，一个查询提取的块是在查询开始的那个时间点上存在的数据块，当前块是在此刻这个时间点上存在的数据块，而不是这个时间点之前或者之后的的数据块数目。
- 当前模式块是在它们当前存在时检索的，而不是以一致性读取的方式检索的。
    - 通常，查询检索的块在查询开始时被检索为存在。当前模式块是在它们当前存在时检索的，而不是从以前的时间点检索的。在选择期间，可能会看到由于读取数据字典而导致当前模式检索，以便查找表进行完整扫描的范围信息（因为需要“立即”信息，而不是一致读取)。在修改期间(DML操作)，将访问当前模式中的块以便写入它们。

**consistent gets (逻辑读)**    

- 从Buffer Cache中读取的Undo数据的Block的数量。
- 数据请求总数就是在回滚段(Undo)Buffer中的数据一致性读所需的数据块，意思是在处理这个操作时需要在一致性读状态上处理多个块。
   - 这些块产生的主要原因是因为在查询过程中，由于其他会话对数据块进行操作（主要是DML操作），而对所要查询的块有了修改。但是，由于查询是在这些修改之前调用的，为了保证数据的一致性，需要对回滚段中数据块的前映像进行查询，这样就产生了一致性读。
- 需重点关注，<mark>一般来讲，逻辑读越多越好。</mark>

**physical reads (物理读)**

- 物理读就是从磁盘上读取数据块的数量。
- 其产生的主要原因是如下：
  1. 在数据库高速缓存中不存在这些块；
  2. 全表扫描
  3. 磁盘排序
- 关系: 逻辑读是指Oracle从内存读到的数据块数量。一般来说是Consistent Gets+Db BlockGets。当在内存中找不到所需的数据块就需要从磁盘中获取，于是就产生了Physical Reads.
- Physical Reads通常是最关心的，如果这个值很高，说明要从磁盘请求大量的数据到Buffer Cache中，通常意味着系统中存在大量全表扫描的SQL语句，这会影响数据库的性能，因此尽量避免语句做全表扫描，对于全表扫描的SQL语句，建议增加相关的索引，优化SQL语句来解决。
- 此指标需重点关注，<mark>一般来讲，物理读越少越好。</mark>
**关于Physical Reads、Db Block Gets和Consistent Gets这3个参数之间有一个换算公式：**

`数据缓冲区的使用命中率 = 1 - (Physical Reads / (Db Block Gets + Consistent Gets)`

- `查询出来的结果Buffer Cache的命中率应在90%以上，否则需要增加数据缓冲区的大小`。
- 用以下语句可以查看数据缓冲区的命中率：

```sql
SELECT name,value 
FROM V$SYSSTAT 
WHERE name IN ('db block gets','consistent gets','physical reads');
```


**redo size , sorts(memeory) , sorts(disk)**

- redo size 
   - DML生成的REDO(日志记录)的大小。
- sorts(memory):
   - 在内存执行的排序量。
- sorts(disk):
   - 在磁盘执行的排序量。
- 需要至少一个磁盘写入的排序操作数。需要磁盘I/O的排序是资源密集型的。
  - 试着增加初始化参数`SORT_AREA_SIZE`大小（通过`SELECT * FROM V$SYSSTAT WHERE name LIKE '%sot%'`)来查看当前的排序内存使用情况并确认是否需要调整，如果调整的话，
      - 在PGA自动管理的情况下，可以调大`PGA_AGGREGATE_TARGET`参数值；
      - 如果PGA为非自动，可以直接调整`SORT_AREA_SIZE`

**其余**

- `bytes sent via SQL*Net to client` 
   - 从`SQL*Net`向客户端发送了..字节的数据
- `bytes received via SQL*Net from client`
   - 客户端向`SQL*Net`发送了..字节的数据 
- `SQL*Net roundtrips to/from client`
   - 从客户端发送和接收的Oracle网络信息的总数  

```sql
SET AUTOTRACE ON;
SET AUTOTRACE TRACEONLY;
SET PAGESIZE 0;
SET LONG 200000;
SET FEEDBACK OFF;
SET ECHO OFF;
SET LINESIZE 160;

SELECT AVG(salary),last_name,department_name
FROM scott.employees e
 JOIN scott.departments d
 ON e.department_id = d.department_id
GROUP BY department_name,last_name;

执行计划
----------------------------------------------------------
Plan hash value: 1139150879

---------------------------------------------------------------------------------------------
| Id  | Operation                     | Name        | Rows  | Bytes | Cost (%CPU)| Time     |
---------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT              |             |   106 |  3286 |     7  (29)| 00:00:01 |
|   1 |  HASH GROUP BY                |             |   106 |  3286 |     7  (29)| 00:00:01 |
|   2 |   MERGE JOIN                  |             |   106 |  3286 |     6  (17)| 00:00:01 |
|   3 |    TABLE ACCESS BY INDEX ROWID| DEPARTMENTS |    27 |   432 |     2   (0)| 00:00:01 |
|   4 |     INDEX FULL SCAN           | DEPT_ID_PK  |    27 |       |     1   (0)| 00:00:01 |
|*  5 |    SORT JOIN                  |             |   107 |  1605 |     4  (25)| 00:00:01 |
|   6 |     TABLE ACCESS FULL         | EMPLOYEES   |   107 |  1605 |     3   (0)| 00:00:01 |
---------------------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   5 - access("E"."DEPARTMENT_ID"="D"."DEPARTMENT_ID")
       filter("E"."DEPARTMENT_ID"="D"."DEPARTMENT_ID")


统计信息
----------------------------------------------------------
          0  recursive calls
          0  db block gets
          9  consistent gets
          0  physical reads
          0  redo size
       3721  bytes sent via SQL*Net to client
        589  bytes received via SQL*Net from client
          8  SQL*Net roundtrips to/from client
          1  sorts (memory)
          0  sorts (disk)
        104  rows processed
```

### SQL解析

#### 硬解析和软解析

**硬解析的概念**

- 硬解析，也称库缓存未命中。如果数据库不能重用现有SQL代码，则它必须生成一个新的可执行版本，本次操作称为一个硬解析。
   - 数据库对DDL语句始终执行硬解析。
- 在硬解析期间，数据库多次访问库缓存和数据字典缓存以检查数据字典。
    - 当数据库访问这些区域时，它在所需对象上使用一个叫作闩锁的串行化设备，以便控制这些对象不能被更改。
    - 闩锁的争用会增加语句的执行时间，并降低并发性。

**软解析的概念**

- 软解析，也称库缓存命中。任何跳过硬解析的解析都是软解析。
- 如果接收到的SQL语句与在共享池中某个可重用SQL语句相同，则数据库将重用该现有代码。
   - 重用代码也称库缓存命中。
- 如果数据库中软解析频率比硬解析频率高则是最好的
   - 因为数据库可以跳过优化和行源生成步骤，而直接进入执行阶段。

### SQL硬解析与软解析的发生过程。

当数据库接收到一条SQL语句，在执行和返回结果前Oracle对此SQL将进行几个步骤的处理过程：

**1. 语法检查(syntax check)**

- 检查此SQL的拼写是否正确。

**2. 语义检查(semantic check)**

- 例如，检查SQL语句中的访问对象是否存在及该用户是否具备相应的权限。

**3. 对SQL语句进行解析（prase)**

- 当数据库接收到一个SQL语句时，会发出一个解析调用，以准备执行该语句，解析调用会打开或创建一个游标，它是一个对特定于会话的私有SQL区的句柄，其中包含已分析的SQL语句和其他处理信息。
   - 游标和私有SQL区位于PGA中。
- 然后，利用内部算法对SQL进行解析，生成解析树(Parse Tree)及执行计划(Execution Plan)，这些信息被存放于私有SQL区。
   - 具体的处理过程——数据库执行共享池检查，以确定是否跳过耗费大量资源的语句处理过程
   - 为此，数据库使用一种哈希算法为每个接收到的SQL语句生成一个哈希值。
      - 语句的哈希值即是在V$SQL.SQL_ID中显示的(4个于SQL相关的字段)
      - hash_value
      - sql_hash_value
      - plan_hash_value
      - sql_id
- 当数据库再次接收到一个SQL语句时，搜索共享SQL区，以查看是否存在一个与现成的已分析过的语句具有相同的哈希值。
  - SQL语句的哈希值有别于该语句的内存地址值(`V$sql的address字段值`)和该语句执行计划的哈希值(`V$SQL_PLAN视图的plan_hash_value字段值`)。
  - 假设存在，则将此SQL与cache中的进行比较，假设“相同”，将利用已有的解析树与执行计划，而省略了优化器的相关工作。这个过程称为软解析。
  - 当然，如果前述的两个假设中任意有一个不成立，那么优化器都将进行创建解析树、生成执行计划的动作。这个过程称为硬解析。
      - 硬解析对于SQL的执行来说是消耗大量资源的动作，所以，应当极力避免硬解析，而使用软解析。

**4. 执行SQL,返回结果(execute and return)。**


#### 动态分析采样

如果在执行计划中有如下提示：

```
Note
-----------------------------------------------
-dynamic sampling used for the statement
```

- 这提示CBO(基于成本的优化方式)当前使用的技术，需要用户在分析计划时考虑到这些因素。
- 当出现这个提示时，说明当前表使用了动态采样，从而推断这个表可能没有做过分析。这里会出现以下两种情况：
   - 如果表没有做过分析，那么CBO可以通过动态采样的方式来获取分析数据，正确地执行计划。
   - 如果表做过分析，但是分析信息过旧，这时CBO就不会再使用动态采样，而是使用这些旧的分析数据，从而可能导致错误的执行计划。

### RBO与CBO

- RBO(Rule-Based Optimization)，是指基于规则的优化方式
- CBO(Cost-Based Optimization)，是指基于成本的优化方式。
- 它们是优化器在优化SQL语句时所遵循的原则，

#### 基于规则的优化方式(RBO)

- 优化器在分析SOL语句时，所遵循的是Oracle内部预定的一些规则，对数据不敏感。它只借助少量的信息来决定一个SQL语句的执行计划，包括：
   - SQL语句本身；
    - SQL中涉及的Table、View、Index等的基本信息；
    - 本地数据库中数据字典中的信息（远程数据库数据字典信息对RBO无效）。
    
#### 基于成本的优化方式(CBO)

CBO是根据语句的代价(Cost)，通过代价引擎来估计每个执行计划所需的代价，该代价将每个执行计划所耗费的资源进行量化，CBO根据这个代价选择出最优的执行计划。

**一个查询所耗费的资源可分为3部分：**

- I/O代价
   - I/O代价是指把数据从磁盘读入内存时所需的代价（该代价是查询所需最主要的，所以在优化时一个基本原则就是降低I/O总次数)。
- CPU代价
   - CPU代价是指处理内存中数据所需的代价，数据一旦读入内存，当识别出所要的数据后，会在这些数据上执行排序(Sort)或连接(Join)操作，这需要消耗CPU资源。
- Network代价。
   - 对于访问远程节点来说，Network代价的花费也是很大的。
   - 优化器在判断是否用哪种方式时，主要参照的是表及索引的统计信息。统计信息给出表的大小、有多少行、每行的长度等信息。
   - 这些统计信息起初在库内是没有的，是做Analyze（分析)后才出现的，很多时候过期统计信息会令优化器做出一个错误的执行计划，因此应及时更新这些信息 
      - `DBMS_STAT.ANALYZE`

#### 优化模式

| 优化模式---- | 描述                                                                                                                                                  |
| :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Rule         | 基于规则的方式                                                                                                                                         |
| Choose       | 默认情况下Oracle用的是这种方式。是指的当一个表或索引有统计信息，则走CBO的方式，如果表或索引没统计信息，表又不是特别小，且相应的列有索引时，那么就走索引，走RBO方式 |
| First Rows   | 它与Choose方式类似，所不同的是当一个表有统计信息时，它将是以最快的方式返回查询最先的几行，从总体上减少了响应时间。                                              |
| All Rows     | 也就是所说的Cost方式，当一个表有统计信息时，它将以最快的方式返回表的所有行，从总体上提高查询的吞吐量。没有统计信息则走RBO方式                                    |


#### 设定选用哪种优化模式

**1. 在initSID.ora中设定**  控制文件

`OPTIMIZER_MODE=RULE|CHOOSE|FIRST_ROWS|ALL_ROWS`

- 默认是CHOOSE
- 选择方式：在`OPTIMIZER_MODE=CHOOSE`时，如果表有统计信息（分区表外），优化器将选择CBO，否则选RBO。

**2. Sessions级别修改**

在用户与Oracle数据库交互环境`(SQL*Plus`、SQL Developer、TOAD)中发出如下语句：

```sql
ALTER SESSION SET OPTIMIZER_MODE=RULE|CHOOSE|FIRST_ROWS|ALL_ROWS;
```

此修改只对当前会话有效。

**3. 语句级别用`Hint(/*+.*/)`来设定**

### 关于执行计划中的索引访问方法

**执行计划的索引方法**

- 索引唯一扫描 INDEX UNIQUE SCAN
- 索引范围扫描 INDEX RANGE SCAN
- 索引跳跃扫描 INDEX SKIP SCAN
- 索引快速全扫描 INDEX FAST FULL SCAN

### DBMS_XPLAN包查看以往的SQL执行计划

- `DBMS_XPLAN`包用来查看Explain Plan生成的执行计划。

**函数**

- `DISPLAY`:
   - 格式化和显示计划的内容；
- `DISPLAY_AWR`:
   - 在AWR中格式化和显示存储的SQL语句的执行计划的内容；
- `DISPLAY_CURSOR`:
   - 格式化和显示任何加载游标的执行计划的内容；
- `DISPLAY_SQL_PLAN_BASELINE`:
   - 为SQL句柄标识的SQL语句显示一个或多个执行计划
- `DISPLAY_SQLSET`:
   - 格式化和显示存储在SQL调谐集中语句的执行计划的内容。

#### DISPLAY_CURSOR函数

- 显示存储在库缓存(Library Cache)中的实际执行计划
  - 如果查询某个SQL语句的实际执行计划，前提是这个SQL的执行计划还在库缓存中，
  - 如果已经被刷出库缓存，就无法获取其实际执行计划。

**参数**

- SQL_ID
   - 指定位于库缓存执行计划中SQL语句的父游标。默认值为NULL。
   - 当使用默认值时，SQL_ID当前会话的最后一条SQL语句的执行计划将被返回。
   - 可通过查询`V$SQL`或`V$SQLAREA`的SQL_ID列来获得SQL语句的SQL_ID
- CURSOR_CHILD_NO
   - 指定父游标下子游标的序号。即指定被返回执行计划的SQL语句的子游标。默认值为0。
   - 如果为NULL，则SQL_ID所指父游标下所有子游标的执行计划都将被返回
- FORMAT
   - 控制SQL语句执行计划的输出部分，即哪些可以显示哪些不显示。
   - 与Display函数的Format参数及修饰符在这里同样适用

**此外**

- 当在开启`STATISTICS_LEVEL=ALL`时，或使用`GATHER_PLAN_STATISTICS`显示可以获得执行计划中实时的统计信息。

**具体使用**

1. 查找SQL语句的SQL_ID

查询V$SQL视图，查找SQL语句的SQL_ID，有可能SQL语句不在Share Pool中，表明SQL语句已经被踢出Share Pool。

```sql
SELECT SQL_ID,CHILD_NUMBER,SQL_TEXT 
FROM V$SQL 
WHERE SQL_TEXT LIKE '%UPDATE CHEPB a SET a.CHEPH=%';

SQL_ID        CHILD_NUMBER SQL_TEXT
------------- ------------ --------------------------------------------------------------------------------
avuy3ax7k40fh            0 SELECT SQL_ID,CHILD_NUMBER,SQL_TEXT FROM V$SQL WHERE SQL_TEXT LIKE '%UPDATE CHEP
5csj10mfqb1wz            0 SELECT SQL_ID,CHILD_NUMBER,SQL_TEXT FROM V$SQL WHERE SQL_TEXT LIKE '%UPDATE CHEP
```

2. 查看实际执行计划

```sql
SELECT *
FROM TABLE (DBMS_XPLAN.DISPLAY_CURSOR('avuy3ax7k40fh',0));

PLAN_TABLE_OUTPUT
--------------------------------------------------------------------------------
SQL_ID  avuy3ax7k40fh, child number 0
-------------------------------------
SELECT SQL_ID,CHILD_NUMBER,SQL_TEXT FROM V$SQL WHERE SQL_TEXT LIKE
'%UPDATE CHEPB a SET a.CHEPH=%'
Plan hash value: 903671040
---------------------------------------------------------------------------
| Id  | Operation        | Name              | Rows  | Bytes | Cost (%CPU)|
---------------------------------------------------------------------------
|   0 | SELECT STATEMENT |                   |       |       |     1 (100)|
|*  1 |  FIXED TABLE FULL| X$KGLCURSOR_CHILD |     1 |   536 |     0   (0)|
---------------------------------------------------------------------------
Predicate Information (identified by operation id):
---------------------------------------------------
   1 - filter(("KGLNAOBJ" IS NOT NULL AND "KGLNAOBJ" LIKE '%UPDATE
              CHEPB a SET a.CHEPH=%' AND "INST_ID"=USERENV('INSTANCE')))
20 rows selected
```

### 常用Hint （提示）

#### 与优化器相关的Hint

在查看分析SQL语句的执行计划前先要对操作表进行必要的分析，以确保SOL执行计划能够采集到最新信息，从而保证SQL执行计划最优。

对操作表进行分析的SQL语句

```sql
ANALYZE TABLE 表 COMPUTE STATISTICS FOR TABLE|{ALL INDEXES}|{ALL INDEXED COLUMNS};
SET AUTOTRACE TRACEONLY;
```

或

```plsql
EXEC DBMS.STATS.GATHER_SCHEMA_STATS (OWNNAME=>USER,OPTIONS=>'GATHER STALE');
```

```sql
ANALYZE TABLE scott.employees COMPUTE STATISTICS FOR TABLE;
ANALYZE TABLE scott.employees COMPUTE STATISTICS FOR ALL INDEXES;
ANALYZE TABLE scott.employees COMPUTE STATISTICS FOR ALL INDEXED COLUMNS;
SET AUTOTRACE TRACEONLY;
```

```plsql
EXEC DBMS_STATS.GATHER_SCHEMA_STATS(OWNNAME=>'SCOTT',OPTIONS=>'GATHER STALE');
```

上面SQL语句执行后能确保优化器采集到最新信息或者参考，从而解析出或者制订出效率最优的SQL语句执行计划或者执行方案。

##### ALL_ROWS

- ALL_ROWS是针对整个目标SQL的Hint，它的含义是让优化器启用CBO，而且在得到目标SQL的执行计划时会选择那些吞吐量最佳的执行路径。
     - “吞吐量最佳”是指资源消耗量（对IVO、CPU等硬件资源的消耗量）最小，
     - 在ALL_ROWS Hint生效的情况下，优化器会启用CBO，而且会依据各个执行路径的资源消耗量来计算它们各自的成本。
- 如果目标SOL中除了ALL_ROWS之外，还使用了其他与执行路径、表连接相关的Hint,优化器会优先考虑ALL_ROWS。
- 默认的优化器模式

**查看优化器模式**

```sql
SHOW PARAMETER OPTIMIZER_MODE;
```

**格式：**

```sql
/*+ ALL_ROWS */
```

```sql
SQL> SET AUTOTRACE ON;
SQL> SET AUTOTRACE TRACEONLY;
SQL> SET PAGESIZE 0;
SQL> SET LONG 200000;
SQL> SET FEEDBACK OFF;
SQL> SET ECHO OFF;
SQL> SET LINESIZE 160;
SQL>
SQL> SELECT AVG(salary),last_name,department_name
  2  FROM scott.employees e
  3   JOIN scott.departments d
  4   ON e.department_id = d.department_id
  5  GROUP BY department_name,last_name;

执行计划
----------------------------------------------------------
Plan hash value: 1139150879

---------------------------------------------------------------------------------------------
| Id  | Operation                     | Name        | Rows  | Bytes | Cost (%CPU)| Time     |
---------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT              |             |   106 |  3286 |     7  (29)| 00:00:01 |
|   1 |  HASH GROUP BY                |             |   106 |  3286 |     7  (29)| 00:00:01 |
|   2 |   MERGE JOIN                  |             |   106 |  3286 |     6  (17)| 00:00:01 |
|   3 |    TABLE ACCESS BY INDEX ROWID| DEPARTMENTS |    27 |   432 |     2   (0)| 00:00:01 |
|   4 |     INDEX FULL SCAN           | DEPT_ID_PK  |    27 |       |     1   (0)| 00:00:01 |
|*  5 |    SORT JOIN                  |             |   107 |  1605 |     4  (25)| 00:00:01 |
|   6 |     TABLE ACCESS FULL         | EMPLOYEES   |   107 |  1605 |     3   (0)| 00:00:01 |
---------------------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   5 - access("E"."DEPARTMENT_ID"="D"."DEPARTMENT_ID")
       filter("E"."DEPARTMENT_ID"="D"."DEPARTMENT_ID")


统计信息
----------------------------------------------------------
        889  recursive calls
          0  db block gets
        235  consistent gets
         16  physical reads
          0  redo size
       3721  bytes sent via SQL*Net to client
        589  bytes received via SQL*Net from client
          8  SQL*Net roundtrips to/from client
         17  sorts (memory)
          0  sorts (disk)
        104  rows processed

SQL> ED
已写入 file afiedt.buf

  1  SELECT /*+ ALL_ROWS*/ AVG(salary),last_name,department_name
  2  FROM scott.employees e
  3   JOIN scott.departments d
  4   ON e.department_id = d.department_id
  5* GROUP BY department_name,last_name
SQL> /

执行计划
----------------------------------------------------------
Plan hash value: 1139150879

---------------------------------------------------------------------------------------------
| Id  | Operation                     | Name        | Rows  | Bytes | Cost (%CPU)| Time     |
---------------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT              |             |   106 |  3286 |     7  (29)| 00:00:01 |
|   1 |  HASH GROUP BY                |             |   106 |  3286 |     7  (29)| 00:00:01 |
|   2 |   MERGE JOIN                  |             |   106 |  3286 |     6  (17)| 00:00:01 |
|   3 |    TABLE ACCESS BY INDEX ROWID| DEPARTMENTS |    27 |   432 |     2   (0)| 00:00:01 |
|   4 |     INDEX FULL SCAN           | DEPT_ID_PK  |    27 |       |     1   (0)| 00:00:01 |
|*  5 |    SORT JOIN                  |             |   107 |  1605 |     4  (25)| 00:00:01 |
|   6 |     TABLE ACCESS FULL         | EMPLOYEES   |   107 |  1605 |     3   (0)| 00:00:01 |
---------------------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   5 - access("E"."DEPARTMENT_ID"="D"."DEPARTMENT_ID")
       filter("E"."DEPARTMENT_ID"="D"."DEPARTMENT_ID")


统计信息
----------------------------------------------------------
         15  recursive calls
          0  db block gets
         13  consistent gets
          0  physical reads
          0  redo size
       3721  bytes sent via SQL*Net to client
        589  bytes received via SQL*Net from client
          8  SQL*Net roundtrips to/from client
          1  sorts (memory)
          0  sorts (disk)
        104  rows processed
```

##### FIRST_ROWS(n)

- FIRST_ROWS(n)是针对整个目标SQL的Hint，
- 让优化器启用CBO模式，而且在得到目标SQL的执行计划时，会选择那些能以最快的响应时间返回头n条记录的执行路径。
- 在FIRST_ROWS(n) Hint生效的情况下，优化器会自用CBO，且会依据返回头n条记录的响应时间来决定目标SQL的执行计划。

**格式：**

```sql
/*+ FIRST_ROWS(n) */
```

**注意**

- Hint的`FIRST_ROWS(n)`的Hint和优化器模式`FIRST_ROWS_n`不是一一对应的。
   - 优化器模式FIRST_ROWS_n中的n只能是1、10、100、1000。
   - Hint的 FIRST_ROWS(n)中的n还可以是其他值。
- 如果在UPDATE、DELETE或者含如下内容的查询语句中使用了FIRST_ROWS(n)Hint，则该Hint会被忽略。
    - 优化器会忽略FIRST_ROWS(n) Hint,是因为对于这些类型的SQL，Oracle必须访问所有的行记录后才能返回满足条件的头n行记录，即在上述情况下，使用该Hint是没有意义的。 
    - 集合运算（如UNION,INTERSACT,MINUS,UNION ALL等）
    - GROUP BY
    - FOR UPDATE
    - 聚合函数（如SUM等）
    - DISTINCT
    - ORDER BY (对应的排序列上没有索引)

##### RULE

- RULE是针对整个目标SQL的Hint
- 表示对目标SQL启用RBO(基于规则的优化器，非Oracle默认)。

**格式：**

```sql
/*+ RULE */
```

**注**

- RULE不能与除`DRIVING_SITE`以外的Hint联用，当RULE与除`DRIVING_SITE`以外的Hint联用时，其他Hint可能会失效
   - 当RULE与DRIVING_SITE联用时，它自身可能会失效，所以RULE Hint最好单独使用。
- 一般情况下，并不推荐使用RULE Hint。
   - 一是因为Oracle早就不支持RBO了，
   - 二是启用RBO后优化器在执行目标SQL时可选择的执行路径将大大减少。
      - 很多执行路径RBO根本就不支持（如哈希连接），也就意味着启用RBO后目标SQL跑出正确执行计划的概率将大大降低。
      - 因为很多执行路径RBO根本就不支持，所以即使在目标SQL中使用了RULE Hint，如果出现了如下这些情况（包括但不限于），RULE Hint依然会被Oracle忽略。
          - 目标SQL除RULE之外还联合使用其他Hint(如DRIVING_SITE)。
          - 目标SQL使用并行执行
          - 目标SQL所涉及的对象有IOT
          - 目标SQL所涉及的对象有分区表
          
#### 与表访问相关的Hint

##### FULL

- 是针对单个目标表的Hint
- 让优化器对目标表执行全表扫描，而不考虑走目标表上的任何索引。

**格式**

```sql
/*+ FULL(目标表) */
```

#### ROWID

- 是针对单个目标表的Hint
- 让优化器对目标表执行ROWID扫描
- 只有目标SQL中使用含ROWID的WHERE条件时，该Hint才有意义

**格式**

```sql
/*+ ROWID(目标表) */
```

#### 与索引访问有关的Hint

##### INDEX

- 是针对单个目标表的Hint
- 让优化器对目标表的目标索引执行索引扫描操作
- 目标索引可以是Oracle数据库中的所有类型索引。

**格式1**

```sql
/*+ 
INDEX(目标表1 目标索引1)
INDEX(目标表2 目标索引2)
  ...
INDEX(目标表n 目标索引n)
*/
```

- 仅指定目标表上的一个目标索引
- 此时优化器只会考虑对这个目标索引执行索引扫描操作，而不会考虑全表扫描或者对该目标表上的其他索引执行索引扫描操作。

**格式2**

```sql
/*+ 
INDEX(目标表1 目标索引1 目标索引2 ... 目标索引n)
INDEX(目标表2 目标索引1 目标索引2 ... 目标索引n)
  ...
INDEX(目标表n 目标索引1 目标索引2 ... 目标索引n)
*/
```

- 指定目标表上的n个目标索引
- 此时优化器只会考虑对这n个目标索引执行索引扫描操作。
   - 可能是分别计算出单独扫描各个目标索引的成本后，选择成本值最低的索引。
   - 也可能是先分别扫描目标索引中的两个或多个索引，然后对扫描结果执行合并操作。
      - 这个可能的前提：优化器计算出来这样做的成本值最低 。即成本值最低原则。

**格式3**

```sql
/*+ 
INDEX(目标表1 (目标索引1的索引列名) (目标索引2的索引列名1 目标索引2的索引列名2...) ... (目标索引n的索引列名))
INDEX(目标表2 (目标索引1的索引列名) (目标索引2的索引列名1 目标索引2的索引列名2...) ... (目标索引n的索引列名))
  ...
INDEX(目标表n (目标索引1的索引列名) (目标索引2的索引列名1 目标索引2的索引列名2...) ... (目标索引n的索引列名))
*/

--如果目标索引2是复合索引，使用如上指定该目标索引的多个索引列，各个索引列之间用空格分隔。
```

- 同格式2

**格式4**

```sql
/*+ 
INDEX(目标表1)
INDEX(目标表2)
  ...
INDEX(目标表n)
*/
```

- 指定目标表上所有已存在的索引
- 此时优化器只会考虑对该目标表上所有已存在的索引执行索引扫描操作，而不会考虑全表扫描操作。
   - 成本最低原则

##### NO_INDEX

- 针对单个目标表的Hint
- 是INDEX的反义Hint，让优化器不对目标表上的目标索引执行扫描操作。
- 目标索引可以是Oracle数据库中的所有类型索引。

**格式1**

```sql
/*+ NO_INDEX(目标表 目标索引) */
```

- 仅指定目标表上的一个目标索引
- 此时优化器只是不会考虑对该目标表上的这个目标索引执行索引扫描操作，但还是会考虑全表扫描操作或对该目标表上的其他索引执行索引扫描操作。

**格式2**

```sql
/*+ NO_INDEX(目标表 目标索引1 目标索引2 ... 目标索引n) */
```

- 指定目标表上的n个目标索引
- 此时优化器只是不会考虑对该目标表上的这n个目标索引执行索引扫描操作。

**格式3**

```sql
/*+  NO_INDEX(目标表 (目标索引1的索引列名) (目标索引2的索引列名1 目标索引2的索引列名2...) ... (目标索引n的索引列名)) */

--如果目标索引2是复合索引，使用如上指定该目标索引的多个索引列，各个索引列之间用空格分隔。
```

- 同格式2

**格式4**

```sql
/*+ NO_INDEX(目标表) */
```

- 指定目标表上所有已存在的索引
- 相当于对目标表指定全表扫描操作，此时优化器不会考虑对该目标表上所有已存在的索引执行索引扫描操作。

##### INDEX_DESC

- 针对单个目标表的Hint
- 让优化器对目标表上的目标索引执行索引降序扫描操作。
    - 如果目标索引是升序的，则INDEX_DESC Hint使Oracle以降序的方式扫描该索引
    - 如果目标索引是降序的，则INDEX_DESC Hint使Oracle以升序的方式扫描该索引

**格式1**

```sql
/*+ INDEX_DESC(目标表 目标索引) */
```

**格式2**

```sql
/*+ INDEX_DESC(目标表 目标索引1 目标索引2 ... 目标索引n) */
```

**格式3**

```sql
/*+ INDEX_DESC(目标表 (目标索引1的索引列名) (目标索引2的索引列名1 目标索引2的索引列名2...) ... (目标索引n的索引列名)) */

--如果目标索引2是复合索引，使用如上指定该目标索引的多个索引列，各个索引列之间用空格分隔。
```

**格式4**

```sql
/*+ INDEX_DESC(目标表) */
```

##### INDEX_COMBINE

- 针对单个目标表的Hint
- 让优化器对目标表上的多个目标索引执行位图布尔运算。
- 作用对象可以是：位图索引 / B*tree索引 

**格式1**

```sql
/*+ INDEX_COMBINE(目标表 目标索引1 目标索引2 ... 目标索引n) */
```

- 指定目标表上的n个目标索引
- 此时优化器会考虑对这n个目标索引中的两个或多个执行位图布尔运算

**格式2**

```sql
/*+ INDEX_COMBINE(目标表) */
```

- 指定目标表上所有已存在的索引
- 此时优化器会考虑对改表上已存在的所有索引中的两个或多个执行位图布尔运算。

**BITMAP相关关键字的出现 P45**

##### INDEX_FFS

- 针对单个目标表的Hint
- 让优化器对目标表上的目标索引执行索引快速全扫描操作
- 索引快速全扫描操作成立前提：
  - SELECT语句中所有的查询列都存在于目标索引中

**格式1**

```sql
/*+ INDEX_FFS(目标表 目标索引) */
```

**格式2**

```sql
/*+ INDEX_FFS(目标表 目标索引1 目标索引2 ... 目标索引n) */
```

**格式3**

```sql
/*+ INDEX_FFS(目标表 (目标索引1的索引列名) (目标索引2的索引列名1 目标索引2的索引列名2...) ... (目标索引n的索引列名)) */

--如果目标索引2是复合索引，使用如上指定该目标索引的多个索引列，各个索引列之间用空格分隔。
```

**格式4**

```sql
/*+ INDEX_FFS(目标表) */
```

##### INDEX_JOIN 

- 针对单个目标表的Hint
- 让优化器对目标表上的多个目标索引执行INDEX JOIN操作
- INDEX JOIN成立前提：
   - SELECT语句中的所有查询列都存在于目标表的多个目标索引中

**格式1**

```sql
/*+ INDEX_JOIN(目标表 目标索引1 目标索引2 ... 目标索引n) */ 
```

**格式2**

```sql
/*+ INDEX_JOIN(目标表) */
```

##### AND_EQUAL 

- 针对单个目标表的Hint
- 让优化器对目标表上的多个目标索引执行INDEX MERGE操作
- INDEX MERGE成立前提：
   - 目标SQL的WHERE条件中出现了多个针对不同单列的等值条件，并且这些列上都有单键值的索引。
   - 在Oracle数据库中，能够做INDEX MERGE的索引数量最大值为5

**格式**

```sql
/*+ AND_EQUAL(目标表 目标索引1 目标索引2 ... 目标索引5)
```

#### 与表连接顺序有关的Hint

##### ORDERED

- 针对多个目标表的Hint
- 让优化器对多个目标表执行表连接操作时，依照他们在目标SQL的WHERE条件中出现的顺序从左到右依次进行连接。

**格式**

```sql
/*+ ORDERED(目标表1 目标表2 ... 目标表n) */
```

##### LEADING

- 针对多个目标表的Hint
- 让优化器将指定的多个表的连接结果作为目标表连接过程中的驱动结果集，并且将LEADING Hint中从左到右出现的第一个目标表作为整个表连接过程中的首个驱动表。
   - LEADING只是指定了首个驱动表。而ORDERED是要优化器依照给出的顺序来连接 
- 当LEADING Hint中指定的表并不能作为目标SQL的连接过程中的驱动表或者驱动结果集时，Oracle忽略该Hint。

**格式**

```sql
/*+ LEADING(目标表1 目标表2 ... 目标表n) */
```

## Oracle优化器

## 内外存优化

|     项目     | 内存 | 外存 |
| :---------: | :--: | :--: |
| 数据访问速度 | 很快 | 很慢 |
|  存储的数据  | 临时 | 永久 |

- 将尽可能多的磁盘（外存）操作变成内存操作，即尽可能减少IO。在一个数据库系统中，IO操作越少，系统的效率一定会越高。

### DBMS_SHARED_POOL软件包 将程序常驻内存 

#### V$DB_OBJECT_CACHE数据字典，查看内存使用情况

- 共享池中对象级的统计信息

```sql
SELECT name
      ,namespace
      ,sharable_mem
      ,executions
      ,kept
FROM V_$DB_OBJECT_CACHE
WHERE LOWER(OWNER) = 'scott';
```

#### 安装DBMS_SHARED_POOL软件包

- Oracle系统自带一个`dbmspool.sql`的脚本文件，保存在其他维护脚本文件相同的目录下。
   - 目录：`C:\app\zjk10\product\11.2.0\dbhome_1\RDBMS\ADMIN\dbmspool.sql` 
- 使用SYSDBA的权限安装
   - 11.2或12.1中默认SYSTEM用户权限也可以安装

```
@C:\app\zjk10\product\11.2.0\dbhome_1\RDBMS\ADMIN\dbmspool.sql
```

#### 使用DBMS_SHARED_POOL.KEEP()

- 将程序在 V_$DB_OBJECT_CACHE数据字典中标识为KEPT

```sql
EXECUTE DBMS_SHARED_POOL.KEEP('scott.show_name');
--可以是存储过程，存储函数，软件包，触发器，序列号等。
```

```sql
SQL> EXECUTE DBMS_SHARED_POOL.KEEP('SCOTT.SHOW_NAME');
PL/SQL procedure successfully completed
```
#### 清空共享池内存

```sql
ALTER SYSTEM FLUSH SHARED_POOL;
```

- V_$DB_OBJECT_CACHE数据字典中标识为KEPT的对象是不能被以上语句清除的

#### DBMS_SHARED_POOL.UPKEEP()

```sql
EXECUTE DBMS_SHARED_POOL.UNKEEP('用户.程序名');
--可以是存储过程，存储函数，软件包，触发器，序列号等。
```

```sql
SQL> EXECUTE DBMS_SHARED_POOL.UNKEEP('scott.show_name');
PL/SQL procedure successfully completed
```

### 缓存表

#### 获取所有表的相关信息

```sql
SELECT table_name
      ,tablespace_name
      ,cache  --是否缓存在内存中
FROM USER_TABLES;
```

```sql
TABLE_NAME                     TABLESPACE_NAME                CACHE
------------------------------ ------------------------------ --------------------
DEPT                           USERS                              N
EMP                            USERS                              N
BONUS                          USERS                              N
SALGRADE                       USERS                              N
LOG_TABLE                      USERS                              N
TEST                           USERS                              N
```

- 以上的CACHE属性都是N，表示：这些表没有缓存在内存中，
    - 即这些表在进行全表扫描操作时，其数据都放在数据库缓冲区的LRU队列的队尾。
- 而缓存表的数据是放在LRU队列的队头，但随着时间的推移有可能被淘汰出数据库缓冲区

#### 修改为缓存表

- 缓存的表应该要是经常使用的表，且这个表不能太大。
   - 防止浪费或占满数据库缓冲区。 

```sql
ALTER TABLE test CACHE;
```

```sql
TABLE_NAME                     TABLESPACE_NAME                CACHE
------------------------------ ------------------------------ --------------------
TEST                           USERS                              Y
```

#### 从缓存表改为非缓存表

```sql
ALTER TABLE test NOCACHE;
```

### 数据常驻内存

#### 查看相关信息

```sql
--为DBA数据字典收集信息
EXECUTE dbms_stats.gather_index_stats('SCOTT','EMP_EMP_ID_PK')
EXECUTE dbms_stats.gather_table_stats('SCOTT','EMPLOYEES')

SHOW PARAMETER BLOCK_SIZE;
--DBA 显示当前数据库的数据块大小
SELECT owner
      ,segment_type
      ,blocks
FROM DBA_SEGMENTS
WHERE OWNER = 'SCOTT%'
 AND segment_name IN ('EMPLOYEES',' EMP_EMP_ID_PK');
--将得到的 (blocks值相加)*8/1024;

--DBA 查看数据库内存缓冲区
方式1.
SHOW PARAMETER DB_KEEP_CACHE_SIZE;
NAME                                 TYPE        VALUE
------------------------------------ ----------- ------------------------------
db_keep_cache_size                   big integer 0
--未设置数据库缓冲区
方式2.
SELECT id
      ,name
      ,block_size
      ,buffers
FROM V$BUFFER_POOL;
        ID NAME                 BLOCK_SIZE    BUFFERS
---------- -------------------- ---------- ----------
         3 DEFAULT                    8192     354600
--未设置数据库缓冲区

--查看表使用的数据库缓冲区信息
SELECT table_name
      ,tablespace_name
      ,buffer_pool
FROM USER_TABLES
WHERE table_name = 'EMPLOYEES';
--查看索引使用的数据库缓冲区信息
SELECT index_name
      ,table_name
      ,tablespace_name
      ,buffer_pool
FROM USER_INDEXES
WHERE TABLE_NAME = 'EMPLOYEES';
```

#### 设置数据库缓冲区keep

```sql
ALTER SYSTEM 
SET DB_KEEP_CACHE_SIZE = 64 M;
```

#### 修改表/索引使用的缓冲区 ,即常驻内存

```sql
ALTER TABLE employees
STORAGE (BUFFER_POOL keep);

ALTER INDEX emp_emp_id_pk
STORAGE (BUFFER_POOL keep);
```

- 当用户使用这个表/索引的时候，Oracle服务器就将这个表/索引放入KEEP BUFFER POOL中并一直保留在其中，即常驻内存。

#### 将表/索引重新放回默认数据库缓冲区，即不再常驻内存

```sql
ALTER TABLE employees
STORAGE (BUFFER_POOL default);

ALTER INDEX emp_emp_id_pk
STORAGE (BUFFER_POOL default);
```

#### 释放KEEP BUFFER POOL所占的内存空间 即不再需要keep缓冲区

```sql
ALTER SYSTEM SET DB_KEEP_CACHE_SIZE = 0;
```

### 将查询的结果缓存在内存 

**共享查询的结果**

- Oracle11g和Oracle12c是使用共享池中的一个专用的内存缓冲区来存储这些缓存的SQL查询结果的。
- 只要这些缓存的查询结果是有效的，其他的语句和会话就可以共享它们。
    - 如果所查询的对象被修改了，则这些缓存的查询结果就变成无效的了。

- 尽管任何查询的结果都可以缓存在内存中，不过那些要访问大量的数据行而只返回少量数据的查询语句才是最好的候选者。

**Oracle系统如何处理缓存查询结果**

当第一个会话执行一个查询时，该查询语句将从数据库中获取数据，然后将这一查询的结果存储在SQL查询结果的内存缓冲区中。如果第二个会话（也包括之后的所有会话）执行相同的查询语句，其查询语句将从查询结果的内存缓冲区中直接提取结果而不需要访问硬盘了。

#### result_cache_mode参数

- 控制Oracle的优化器是否自动地将查询的结果存入查询结果缓冲区。

- result_cache_mode参数在系统/会话级设置，其可取的值为AUTO、MANUAL或FORCE

| 值      | 说明                                                         |
| :----- | :----------------------------------------------------------- |
| AUTO   | 优化器依据执行重复的次数决定哪些查询结果存入结果缓冲区。          |
| MANUAL | 默认，要使用`/*+ RESULT_CACHE */`说明将查询结果存入结果缓冲区中 |
| FORCE  | 所有的查询的结果都存入结果缓冲区中                              |

     
##### 查看result_cache_mode参数的设置

```sql
SHOW PARAMETER result_cache_mode;
```

#### 使用`SQL*Plus`的EXPLAIN PLAN FOR命令

- Oracle首先查看结果内存缓冲区以检查查询的结果是否已经在这个缓冲区中。
- 如果存在则直接从这个内存缓冲区中取出结果，
- 如果不存在就执行这一语句，并将返回的查询结果存入到结果内存缓冲区。

```sql
EXPLAIN PLAN FOR
SELECT /*+ RESULT_CACHE */ department_id
      ,AVG(salary)
      ,COUNT(salary)
      ,MIN(salary)
      ,MAX(salary)
FROM employees
GROUP BY department_id;

SQL> /
Explained
```

**获取所解释SQL语句的执行计划**

```
SELECT id
      ,operation
      ,options
      ,object_name
FROM plan_table;

  ID OPERATION        OPTIONS          OBJECT_NAME
--- ---------------- ---------------- ------------------------------
 0 SELECT STATEMENT                       
 1 RESULT CACHE                         bba140yxjhwj22rgcwwfgkdur3
 2 HASH                GROUP BY                                                                         
 3 TABLE ACCESS        FULL             EMPLOYEES
```

#### DBMS_RESULT_CACHE 软件包

**查看查询结果缓冲区的状态**

```sql
SELECT DBMS_RESULT_CACHE.STATUS 
FROM dual;
```

**将所有依赖于该表的缓存结果设置为无效**

```sql
EXEC DBMS_RESULT_CACHE.INVALIDATE('SCOTT','EMPLOYEES');
```

**将结果内存缓冲区清空**

```sql
EXEC DBMS_RESULT_CACHE.FLUSH;
```

**获取结果缓冲区使用的统计信息**

```sql
SET SERVEROUTPUT ON;

EXEC DBMS_RESULT_CACHE.MEMORY_REPORT;

--------------------------------------------------------
R e s u l t   C a c h e   M e m o r y   R e p o r t
[Parameters]
Block Size          = 1K bytes
Maximum Cache Size  = 16192K bytes (16192 blocks)
Maximum Result Size = 809K bytes (809 blocks)
[Memory]
Total Memory = 10704 bytes [0.001% of the Shared Pool]
... Fixed Memory = 10704 bytes [0.001% of the Shared Pool]
... Dynamic Memory = 0 bytes [0.000% of the Shared Pool]

PL/SQL 过程已成功完成。
```

##### 相关的数据字典

- V$RESULT_CACHE_STATISTICS:
   - 列出各种缓存的设置和内存使用的统计信息。
- V$RESULT_CACHE_MEMORY:
   - 列出所有内存块和对应的统计信息。
- V$RESULT_CACHE_OBJECTS:
   - 列出所有对象（缓存结果和依赖的）连同它们的属性。
- V$RESULT_CACHE_DEPENDENCY:
   - 列出所有缓存结果和依赖性之间的依赖性细节。

### 跨会话的PL/SQL函数结果缓存

函数结果缓存机制，这种缓存机制将PL/SQL函数的结果存储在共享池中并提供了语言的支持和系统管理方法，而运行的应用程序的每一个会话都可以访问这一缓存后的PL/SQL函数结果。这种缓存机制对于应用而言是既有效又简单，而且它也减轻了设计和开发自己的缓存和缓存管理应用的负担。

当每次使用不同的参数值调用一个结果缓存的PL/SQL函数时，这些参数和该函数的返回值都会被存储在内存缓冲区中。随后，当相同的函数以相同的参数值被调用时，Oracle服务器将从结果缓冲区中提取该函数的结果。如果被用来计算缓存结果的一个数据库对象被修改了，那么缓存的结果就变成了无效并且必须重新计算。

通常使用这一结果缓存特性的适用范围是：那些经常被调用并且所依赖的信息（对象）从来不变或很少变化的函数。

- 要开启一个PL/SQL函数的结果缓存功能，只需**在函数中加入RESULT_CACHE子句**。
- 当一个结果缓存函数被调用时，Oracle系统首先检查函数的结果缓存。
    - 如果缓冲区中包含了之前以相同参数值调用这一函数的结果，那么系统就直接将缓存的结果返回给调用者而并不执行函数体（即函数的PL/SQL程序代码)。
    - 如果缓冲区中没有包含该函数的结果，那么系统就执行这一函数体并在控制返回调用者之前将其结果（连同使用的参数值）添加到结果缓冲区中。
- 在这个结果缓冲区中可以存放许多结果一对于调用该函数的每一个唯一的参数值的组合都有一个结果。
- 如果系统需要更多的内存，那么一个或多个结果会被从结果缓冲区中清除。
- 如果函数在执行期间产生了一个无法处理的异常，这个异常的结果不会被存储在结果缓冲区中。

```sql
CREATE OR REPLACE FUNCTION select_hire_date
(
 v_id NUMBER
)
RETURN VARCHAR2
RESULT_CACHE RELIES_ON(copy_emp)
IS
 v_hire_date DATE;
BEGIN
 SELECT hire_date
 INTO v_hire_date
 FROM copy_emp
 WHERE employee_id = v_id;

 RETURN TO_CHAR(v_hire_date,'YYYY"年"MM"月"DD"日"');
END; 

EXEC DBMS_OUTPUT.PUT_LINE(select_hire_date(177));
```

**查看是否放入结果内存缓冲区**

- 使用sys用户

```sql
SELECT name
      ,status
FROM V$RESULT_CACHE_OBJECTS;

 NAME                                                                  STATUS
-------------------------------------------------------------------- -------------------
"SCOTT"."SELECT_HIRE_DATE"::8."SELECT_HIRE_DATE"#762ba075453b8b0d #1  Invalid
```

### DBMS_STATS 为数据字典收集相关的统计信息

- 以DBA_XXXX的数据字典都是静态数据字典，为了数据库的效率，Oracle并不实时更新。

```sql
EXECUTE dbms_stats.gather_index_stats('SCOTT','EMPLOYEES_INDEX')
```

```sql
EXECUTE dbms_stats.gather_table_stats('SCOTT','EMPLOYEES')
```


# RMAN

## RMAN

#### RMAN环境简介

1. RMAN是对数据库进行备份和恢复操作并自动管理相关备份策略的客户端工具。
2. RMAN环境至少包括两部分：
      - 目标数据库（target database）：即需要进行备份和恢复的数据库，在RMAN命令行下通过target命令指定；
      - RMAN客户端：默认存放于$ORACLE_HOME/bin目录下的可执行程序，用于执行RMAN命令进行数据库备份和恢复操作。
- 在某些情况下，RMAN环境还可能包括：
   - 快速恢复区（fast recovery area）：用于存放和管理备份恢复相关文件的地方，可通过DB_RECOVERY_FILE_DEST和DB_RECOVERY_FILE_DEST_SIZE设置。
   - 介质管理器：RMAN与介质设备打交道所需要的应用程序；
   - 恢复目录（recovery catalog）：单独的数据库方案，用于存储其管理的多个目标数据库的元数据。

#### 启动RMAN并连接到目标数据库

- 通过cmd命令行而不是SQL环境启动RMAN；

##### 1.选择要连接的数据库实例

- 如果只有一个实例，则无需选择，Oracle自动默认连接

```
SET ORACLE_SID=数据库实例;
```

```
SET ORACLE_SID=ORCL;
```

##### 2.连接RMAN

1. 本地连接

```
RMAN TARGET 用户/密码;
RMAN TARGET /; --无密码连接
```

```
RMAN TARGET sys/change_on_install;
```

2. 连接到远程数据库

```
RMAN TARGET 用户/密码@数据库实例
```

```
RMAN TARGET sys/change_on_install@orcl;
```

**记录RMAN日志**

- 记录本次RMAN的操作,连接失败也记录
- 该文件实时更新，记录RMAN语句的输出值，查询结果等
   - 此时，在命令行界面没有显示输出值
   - 而在设置的RMAN日志文件中实时更新应该显示的输出值。 
   - 直到HOST；

```
RMAN TARGET sys/change_on_install LOG '记录日志存放路径.txt'
```

```
RMAN TARGET sys/chenge_on_install@orcl LOG 'E:\rman_test.txt'

RMAN SHOW DEVICE TYPE;
--文件内容
恢复管理器: Release 11.2.0.1.0 - Production on 星期五 10月 21 21:21:12 2022

Copyright (c) 1982, 2009, Oracle and/or its affiliates.  All rights reserved.

连接到目标数据库: ORCL (DBID=1634085857)

RMAN> 
使用目标数据库控制文件替代恢复目录
db_unique_name 为 ORCL 的数据库的 RMAN 配置参数为:
CONFIGURE DEVICE TYPE DISK PARALLELISM 1 BACKUP TYPE TO BACKUPSET; # default
```

###### 无密码连接时 可能出现的错误(远程数据库连接的方式正常)

```
C:\Users\zjk10>RMAN TARGET /

恢复管理器: Release 11.2.0.1.0 - Production on 星期五 10月 21 20:30:52 2022

Copyright (c) 1982, 2009, Oracle and/or its affiliates.  All rights reserved.

RMAN-00571: ===========================================================
RMAN-00569: =============== ERROR MESSAGE STACK FOLLOWS ===============
RMAN-00571: ===========================================================
RMAN-00554: 内部恢复管理器程序包初始化失败
RMAN-04005: 目标数据库中存在错误:
ORA-12560: TNS: 协议适配器错误
```

**解决**

1. 法1

- 到Oracle安装目录找到 network\admin\sqlnet.ora文件`C:\app\10548\product\11.2.0\dbhome_1\NETWORK\ADMIN`
- 将**SQLNET.AUTHENTICATION_SERVICES**= (NONE)修改为 SQLNET.AUTHENTICATION_SERVICES= (NTS)，将操作系统用户添加到ORA_DBA组

2. 法2

- 增加recovery_file_dest_size参数值即可(SQL环境)

```
ALTER SYSTEM
SET DB_RECOVERY_FILE_DEST_SIZE=大小 SCOPE=SPFILE;
```

##### 3.退出RMAN

```
HOST
```

#### RMAN基本使用

- 在RMAN中可以使用数据库的启动关闭等语句。
- 使用RMAN连接到数据库时，默认使用SYSDBA角色登陆。
- 可以使用除了SELECT外的SQL语句
   - 格式
   
```
SQL 'sql语句';
```

#### 查看当前RMAN配置

```
SHOW ALL;
```

```
SHOW CHANNEL;
```

```
SHOW DEVICE TYPE;
```

```
SHOW DEFAULT DEVICE TYPE;
```

#### BACKUP 备份数据库

1. 通过BACKUP命令备份数据库，默认情况下将备份文件放到快速恢复区中，若要备份到其他路径，需要设置FORMAT参数。

```
BACKUP;
```

2. RMAN默认备份类型为备份集而非镜像。可通过输入BACKUP AS COPY来备份镜像文件。

```
BACKUP AS COPY;
```

##### 在归档模式备份数据库

- 若数据库开启归档，则数据库支持OPEN状态进行RMAN备份，
   - 但此时的备份为非一致性备份，即在恢复时，需要使用到归档重做日志文件，才能使数据库恢复到一致性状态。

```
BACKUP DATABASE PLUS ARCHIVELOG;
```

##### 在非归档模式下进行RMAN备份

- 在非归档模式下的数据库，只能进行一致性备份，
    - 备份前需要将数据库一致性关闭后，打开到MOUNT状态，再进行备份。


```
SHUTDOWN IMMEDIATE;

BACKUP DATABASE;
```

##### 常用典型备份选项

**FORMAT**：指定备份片段的存放路径和命名规则。

```
BACKUP FORMAT '/u01/app/oracle/oradata/enmo1/AL_%d/%t/%s/%p'   
ARCHIVELOG LIKE '%arc_dest%';

其中：
%U获得系统分配的一个唯一名；
%d为DB_NAME;
%t备份设置的时间戳；
%s为备份设置的编号；
%p为备份片段编号。
```

**TAG**：给备份片段指定一个标签，若不指定，则系统自动生成一个随机唯一标签。

- 标签在备份原数据中总是以大写方式存储。

```
BACKUP TAG 'weekly_full_db_bkup' DATABASE MAXSETSIZE 10M;
```

##### 增量备份

###### 创建增量备份

- 增量备份分为累计增量备份（cumulative incremental backup）和差异增量备份（differential incremental backup）。
- oracle默认类型为差异增量备份。
- 增量备份需要先进行一次0级备份，作为备份的起点。
    - oracle建议备份的级别只限于0级和1级，方便备份管理

**差异增量备份和累积增量备份的差别：**

- 差异增量备份：每次备份至上一次备份级别小于等于当前级别的备份。
- 累积增量备份：每次备份至上一次小于当前级别的备份。

**增量备份例子：**

```
backup incremental level 0 database;----0级增量备份，作为增量备份策略的基础
backup incremental level 1 cumulative database;----1级累积增量备份
backup incremental level 1 database;----1级差异增量备份
```

###### 创建增量更新备份

**RMAN支持增量更新备份，但是该特性需要以下前提条件：**

1. 需要0级数据文件镜像作为基础，并且这些镜像需要一个系统定义或者个人定义的标签。
2. 定期备份期间，1级差异增量备份的标签需要和0级数据文件镜像的标签一致。
    - 通过`BACKUP FOR RECOVER OF COPY`命令可以指定一个只备份从上一个相同标签的备份以来发生数据块改变的增量备份。
3. 定期备份期间，增量备份被应用到0级镜像上，由于数据文件镜像已经被增量备份更新过，这样在介质恢复时，所需要的工作量将大大减少。

**命令例子：**

```
backup incremental level 0 for recover of copy tag 'test' database; ----使用tag标记数据文件镜像作为备份策略基础
recover copy of database with tag 'test';----增量更新备份
```

#### 检查数据库文件和备份的有效性

1. 有效性包括：数据文件是否存在于正确的路径下，并且是否存在物理块损坏。
    - 通过`CHECK LOGICAL`命令，还可以检查是否存在逻辑块损坏。
2. 在备份时，对数据文件和归档日志文件进行检查：`BACKUP VALIDATE CHECK LOGICAL DATABASE ARCHIVELOG ALL;`
3. 支持对数据文件中的数据库进行检查：`VALIDATE DATAFILE 4 BLOCK 10 TO 30;`
4. 支持对数据库备份集进行验证：`VALIDATE BACKUPSET 3;`

#### 使用脚本文件来执行RMAN任务

- RMAN支持所有格式的文件

**脚本文件的引用**

1.

```
RMAN @/my_dir/test.txt
```

2.登陆RMAN后

```
@/my_dir/test.txt
```

#### 列出RMAN备份信息

##### 使用LIST列出RMAN备份信息

**列出数据库中所有文件的备份信息：**

```
LIST BACKUP OF DATABASE;
```

**列出指定表空间的备份信息：**

```
LIST COPY OF TABLESPACE '表空间名';
```

**列出指定数据文件的备份信息：**

```
LIST BACKUP OF DATAFILE '数据文件路径';
```

```
list backup of database by backup;
list backup by file;
list backup summary;
list expired backupset/copy;
list backup recoverable;
```

##### 使用REPORT列出RMAN备份信息

```
REPORT NEED BACKUP DATABASE;--列出当前需要备份的数据文件
REPORT OBSOLETE;
REPORT SCHEMA;
REPORT UNRECOVERABLE;
```

#### 维护RMAN备份

**CROSSCHECK**：

- 该命令将同步数据库备份和镜像在实际存储上的位置和资源库中的逻辑记录，如果备份文件在磁盘存在，CROSSCHECK会检查文件头部是否有效。
- 一般在删除备份之前，需要进行CROSSCHECK检查。

```
CROSSCHECK BACKUP ;     CROSSCHECK  COPY;
```

#### 删除备份

**DELETE命令**

- 将会删除磁盘或者磁带上的备份文件，并且更新控制文件中的状态为“已删除”或者删除备份目录（backup catalog）中的记录。


**删除陈旧备份**

- 当使用RMAN执行备份操作时，RMAN会根据备份冗余策略确定陈旧备份。     


```
DELETE OBSOLETE;
```

**删除EXPIRED备份**

- 执行crosscheck命令核对备份集，那么会将该备份集标记为EXPIRED状态。为了删除相应的备份记录，可以执行delete expired backup命令。     

```
DELETE EXPIRED BACKUP;
```

**删除EXPIRED副本**

```
DELETE EXPIRED COPY;
```

**删除特定备份集**

```
DELETE BACKUPSET 22;
```

**删除特定备份片**

```
DELETE BACKUPPIECE '路径'
```

RMAN> delete backuppiece 'd:\backup\rusky.bak';

**删除所有备份集**

```
DELETE BACKUP
```

**删除特定映像副本**

```
DELETE DATAFILECOPY '路径';
```

RMAN> delete datafilecopy 'd:\backup\rusky.bak';

**删除所有映像副本**

```
DELETE COPY;
```

**在备份后删除输入对象**

```
DELETE ARCHIVELOG ALL DELETE INPUT;
```

```
DELETE BACKUPSET 22 FORMAT = "路径" DELETE INPUT;
```

RMAN> delete backupset 22 format = ''d:\backup\%u.bak'' delete input;  

#### 使用数据恢复建议器（data recovery advisor）进行失败诊断和修复

1. 列出当前失败并确定修复选项：
    - oracle中失败是指被Health Monitor监测到的数据损坏，例如逻辑或物理的数据块损坏、数据文件丢失等。
    - 监测到的失败有不同的优先级（CRITICAL、HIGHT和LOW），还有状态（OPEN和CLOSED）。
2. 通过`LIST FAILURE`可以查看当前监测到的失败，
    - 若在同一会话中执行`ADVISE FAILURE`命令，数据库会列出手工和自动的修复选项以供选择。
3. 一般首先通过手工修复方式来进行修复，若手工修复不成功，再进行自动修复。


#### 使用闪回数据库技术
1. 想使用闪回数据库技术，需要先开启闪回日志功能。
   - 闪回日志只能存放在快速恢复区中，并且不会进行归档
2. 闪回数据库不能用于介质恢复和修复数据文件的丢失。
3. 闪回数据库需要在MOUNT状态下进行。

**命令：**

```
shutdown immediate ;----调整至mount状态 
startup mount;
flashback database to scn 1526845;----闪回到过去的某时刻
flashback database to restore point before_points;
flashback database to timestamp to_date('20140510','yyyymmdd');
alter database read only;----将数据库置为只读状态，进行验证
shutdown immediate;----若闪回后满足要求，启动数据库
startup mount;
alter database open resetlogs;
```

### 还原和恢复数据库文件

1. 还原是指从所有的备份或镜像文件中找到一个用于恢复操作的数据文件。
2. 恢复是指在还原的数据文件上应用redo日志或增量备份中记录的变化，使得数据文件向前滚到一个SCN值或者一个时间点。
3. 在对数据库进行还原恢复操作时，可以先进行预览：restore database preview summary;
4. 恢复整个数据库：
    
```
     startup force mount;----将数据库至于mount状态
     restore database;----还原数据库
     recover database;----恢复数据库
     alter database open;
```

5. 恢复表空间：当数据库处于打开状态，可以通过restore tablespace和recover tablespace来进行表空间级的还原和恢复。在对表空间进行还原和恢复时，需要将表空间涉及到的数据文件置为离线，恢复完成后，再设为在线。
     alter tablespace users offline;
     ![](C:/NoteBook/pictures/563094213221062.png)
     alter tablespace users online;
6. 对数据块进行恢复
    RMAN工具在进行备份时，会将备份过程中检测到的坏块记录到V$DATABASE_BLOCK_CORRUPTION视图中；
    在备份过程中，坏块还会记录在告警日志和TRACE文件中，可以通过V$DIAG_INFO查看这些文件的位置，找到相关文件进行坏块查看。
    使用RMAN进行坏块恢复
     recover corruption list;----修复所有的坏块
     recover datafile 1 block 33, 44 datafile 2 blocke 1 to 200;


### 介质恢复

介质恢复是基于物理备份恢复数据，它包括块恢复、数据文件恢复、表空间恢复和整个数据库的恢复。

#### 介质恢复的过程

**步骤**

介质恢复过程包括还原（RESTORE）和恢复（RECOVER）两个步骤

- RESTORE：将某个时间点的数据文件的拷贝再拷贝回去，还原到一个不一致的状态或不是最新的状态，还需要recover操作
- RECOVER：使用日志（归档或是联机在线日志）将不一致的数据库应用到一致性状态

**恢复过程分类**

数据库的恢复过程又分为完全恢复和不完全恢复。

- 完全恢复
   - 完全恢复是一种没有数据丢失的恢复方式，它能够恢复到最新联机在线日志中已提交的数据。
- 不完全恢复
   - 不完全恢复是一种丢失数据的恢复方式，称为基于时间点的恢复。
   - 它只能针对整个数据库的恢复。
   - 不完全恢复根据备份情况恢复到与指定时间、日志序列号和SCN具有一致性的数据，之后的数据都将丢失。
   - 使用`RESTLOGS`选项打开数据库

**场景**

- 丢失归档日志，联机日志的情形
- 在某一刻误操作数据，想要找回数据


##### 数据库完全恢复

Oracle数据库中有各种各样的块，如空块、表头块、数据块、索引块、数据文件头块、UNDO段块等。如果损坏的是普通表的数据块，那即不会影响启动数据库，也不会影响其他表的正常访问，只是在访问该表的时候会出现问题。如果损坏的是数据文件的块头，那么将导致数据库无法正常启动和停止，进行块恢复不能修复这个问题，只有数据文件的完全恢复才能恢复数据文件块头的损坏或尝试使用BBED修改数据文件块头。

###### 数据文件的完全恢复

**命令还原、恢复6号数据文件**

```
RMAN TARGET sys/change_on_install;
RESTORE DATAFILE '数据文件路径'|文件号;
RECOVER DATAFILE '数据文件路径'|文件号;
```

```
RMAN TARGET sys/change_on_install;
RESTORE DATAFILE 'E:\test_dat_01.DBF';
RECOVER DATAFIILE 'E:\test_dat_01.DBF';
```

**打开数据库**

```
ALTER DATABASE OPEN;
```

##### 数据库不完全恢复

###### 不完全恢复的选项


|      不完全恢复方式      |   	RMAN选项    |
| :--------------------: | :------------: |
|    恢复到某个时间点	    |   until time   |
| 恢复到某个日志序列号	 | until suquence |
|     恢复到某个SCN号     |  	until SCN   |


###### 基于时间点的不完全恢复

```
rman target sys/change_on_install
run {
shutdown immediate;
startup mount;

SQL "alter session set nls_date_format=''YYYY-MM-DD HH24:MI:SS''";

set until time '2022-10-21 18:00:00';

restore database;

recover database;

alter database open resetlogs;
}
```

```
RMAN> run {
2> shutdown immediate;
3> startup mount;
4>
5> SQL "alter session set nls_date_format=''YYYY-MM-DD HH24:MI:SS''";
6>
7> set until time '2022-10-21 18:00:00';
8>
9> restore database;
10>
11> recover database;
12>
13> alter database open resetlogs;
14> }

数据库已卸装
Oracle 实例已关闭

已连接到目标数据库 (未启动)
Oracle 实例已启动
数据库已装载

系统全局区域总计    1720328192 字节

Fixed Size                     2176448 字节
Variable Size               1174407744 字节
Database Buffers             536870912 字节
Redo Buffers                   6873088 字节

sql 语句: alter session set nls_date_format=''YYYY-MM-DD HH24:MI:SS''

正在执行命令: SET until clause

启动 restore 于 21-10月-22
分配的通道: ORA_DISK_1
通道 ORA_DISK_1: SID=130 设备类型=DISK

RMAN-00571: ===========================================================
RMAN-00569: =============== ERROR MESSAGE STACK FOLLOWS ===============
RMAN-00571: ===========================================================
RMAN-03002: restore 命令 (在 10/21/2022 21:39:04 上) 失败
RMAN-06026: 有些目标没有找到 - 终止还原
RMAN-06023: 没有找到数据文件5的副本来还原
RMAN-06023: 没有找到数据文件4的副本来还原
RMAN-06023: 没有找到数据文件3的副本来还原
RMAN-06023: 没有找到数据文件2的副本来还原
RMAN-06023: 没有找到数据文件1的副本来还原
```

###### 基于SCN的不完全恢复

```
run {
shutdown immediate;

startup mount;

set until scn 5166390;

restore database;

recover database;

alter database open resetlogs;
}
```

###### 基于序列的不完全恢复

```
run {
shutdown immediate;

startup mount;

set until scn 5166390;

restore database;

recover database;

alter database open resetlogs;
}
```


### 归档模式  (Data guard???)

Oracle数据库有联机重做日志，这个日志是记录对数据库所做的修改，比如插入，删除，更新数据等，对这些操作都会记录在联机重做日志里。

一般数据库至少要有2个联机重做日志组。当一个联机重做日志组被写满的时候，就会发生日志切换，这时联机重做日志组2成为当前使用的日志，当联机重做日志组2写满的时候，又会发生日志切换，去写联机重做日志组1，就这样反复进行。

- 如果数据库处于非归档模式,联机日志在切换时就会丢弃. 
- 而在归档模式下，当发生日志切换的时候，被切换的日志会进行归档。
    - 比如，当前在使用联机重做日志1，当1写满的时候，发生日志切换，开始写联机重做日志2，这时联机重做日志1的内容会被拷贝到另外一个指定的目录下。这个目录叫做归档目录，拷贝的文件叫归档重做日志。
- 数据库使用归档方式运行时才可以进行灾难性恢复。

**归档日志模式和非归档日志模式的区别**

- 非归档模式只能做冷备份,并且恢复时只能做完全备份.最近一次完全备份到系统出错期间的数据不能恢复.
- 归档模式可以做热备份,并且可以做增量备份,可以做部分恢复.
- 用ARCHIVE LOG LIST 可以查看模式状态是归档模式还是非归档模式.

#### 归档日志文件

[2768249.html](https://www.cnblogs.com/mq0036/archive/2012/11/13/2768249.html)

#### 归档参数

* log_archive_dest_n：归档文件的存放目录。可以是本地目录，网络目录和网络连接存储（NAS）。
* log_archive_dest_state_n：为归档目标值定义两个状态：如果为 enable，则 arch 进程可以将归档日志归档至指定的值；如果为 defer，则 arch 进程不会在相对应的位置存放归档。
* log_archive_format：指定归档日志的命名格式。
* log_archive_max_processes：定义数据库启动 arch 进程的数量。
* log_archive_min_succeed_dest：指定归档最小成功复制的位置个数。
* log_archive_start：默认为 false。Oracle 10g 及其以后版本 arch 进程自动启动，故不在使用。

#### 配置数据库的归档模式

- 需在 MOUNT 状态下修改归档模式

##### 改变非归档模式/归档模式

**需在 MOUNT 状态下修改归档模式**

1. 关闭数据库

```
SHUTDOWN NORMAL/IMMEDIATE;
```

2. 将数据库启动到MOUNT状态

```
START MOUNT;
```

3. 修改数据库为归档模式/非归档模式

```
ALTER DATABASE ARCHIVELOG|NOARCHIVELOG;
```

4. 将数据库完全启动

```
ALTER DATABASE OPEN;
```

##### 启用自动归档: LOG_ARCHIVE_START=TRUE

- 归档模式下,日志文件组不允许被覆盖(重写)，
   - 当日志文件写满之后，如果没有进行手动归档,
   - 那么系统将挂起,直到归档完成为止.
   - 这时只能读而不能写.

**运行过程中关闭归档日志进程**

```
ARCHIVE LOG STOP
```

**运行过程中关闭和重启归档日志进程**

```
ARCHIVE LOG START
```

##### 手动归档: LOG_ARCHIVE_START=FALSE

###### 归档当前/所有日志文件

```
ALTER SYSTEM 
ARCHIVE LOG CURRENT|ALL;
```

###### 归档指定序号的日志文件

1. 查看日志文件的序号

```
SQL> ARCHIVE LOG LIST
数据库日志模式         存档模式
自动存档              启用
存档终点              USE_DB_RECOVERY_FILE_DEST
最早的联机日志序列     58
下一个存档日志序列     60
当前日志序列           60
```

2. 执行归档指定序列的日志文件

```
ALTER SYSTEM 
ARCHIVE LOG SEQUENCE 序号;

--不能用于当前的日志文件归档
ORA-00259: 日志 10 (打开实例 orcl 的日志, 线程 1) 为当前日志, 无法归档
--不能用于已经归档的日志文件
ORA-16013: 日志 1 sequence# 58 不需要归档
```

###### 改变归档日志目标

```
ALTER SYSTEM 
ARCHIVE LOG CURRENT TO '归档日志文件存放路径';
```

##### 配置多个归档进程 LOG_ARCHIVE_MAX_PROCESSES

- 如果归档过程会消耗大量的时间，那么可以启动多个归档进程
- 这是个动态参数,可以用`ALTER SYSTEM`动态修改.

```
ALTER SYSTEM 
SET LOG_ARCHIVE_MAX_PROCESSES = 数量;
```

Oracle9i中最多可以指定10个归档进程

**与归档进程有关的动态性能视图**

- V$BGPROCESS

```
SQL> DESC V$ARCHIVE_PROCESSES
 名称                                      是否为空? 类型
 ----------------------------------------- -------- ----------------------------
 PROCESS                                            NUMBER
 STATUS                                             VARCHAR2(10)
 LOG_SEQUENCE                                       NUMBER
 STATE                                              VARCHAR2(4)
 
SELECT *
FROM V$ARCHIVE_PROCESSES;
```

- V$ARCHIVE_PROCESSES

```
DESC V$BGPROCESS
 名称                                      是否为空? 类型
 ----------------------------------------- -------- ----------------------------
 PADDR                                              RAW(8)
 PSERIAL#                                           NUMBER
 NAME                                               VARCHAR2(5)
 DESCRIPTION                                        VARCHAR2(64)
 ERROR                                              NUMBER
```

##### 配置归档目标(归档路径)   LOG_ARCHIVE_DEST_n


| Oracle基本参数(LOG_ARCHIVE_DEST_n)            |
| :------------------------------------------- |
| 此参数指定归档日志文件的位置                    |
| 参数类型:字符串                                |
| 语法:LOG_ARCHIVE_DEST_1=’path_to_dir’         |
| 默认值:无                                     |
| 大部分可通过ALTER SYSTEM 或 ALTER SESSION 修改 |
| 此为基本参数                                  |

**其他信息**

- n的取值范围为1到10,可设置多个归档日志路径
- 该参数有很多属性，其中location或service是必须的(二选一)
- location指定本地路径，service为dataguard备库的service name
- location或service必须是第一个设置的,其他属性可选

**语法**

```
ALTER SYSTEM|SESSION 
SET LOG_ARCHIVE_DEST_n = '
 { null_string |
 { LOCATION=path_name | SERVICE=service_name }
 [ MANDATORY ]
 [ REOPEN[=seconds] ]
 [ DELAY[=minutes] ]
 [ ENCRYPTION=ENABLED|DISABLED ]
 [ GROUP=group ]
 [ NOREGISTER ]
 [ PRIORITY=priority ]
 [ TEMPLATE=template ]
 [ ALTERNATE=destination ]
 [ MAX_FAILURE=count ]
 [ SYNC | ASYNC ]
 [ AFFIRM | NOAFFIRM ]
 [ NET_TIMEOUT=seconds ]
 [ VALID_FOR=(redo_log_type,database_role) ]
 [ DB_UNIQUE_NAME ]
 [ COMPRESSION={ENABLE|DISABLE|ZLIB|LZO} ]
 }'
```

**注：**

- 可通过查询V$ARCHIVE_DEST 视图来查看具体的属性设置
- 该参数必须和LOG_ARCHIVE_DEST_STATE_n一一对应

**查看信息**

```
SHOW PARAMETER LOG_ARCHIVE_DEST_n;
```

**存档终点**

oracle10默认是把存档终点设置为USE_DB_RECOVERY_FILE_DEST，所以归档日志默认是保存在oracle系统的闪回恢复区（Flash recovery area）的。默认的db_recovery_file_dest是2G，所以一般建议将存档终点修改为操作系统的适当目录。

```
SHOW USE_DB_RECOVERY_FILE_DEST;
```

###### 归档目标(归档路径) 

**本地归档目标:**

```
ALTER SYSTEM 
SET LOG_ARCHIVE_DEST_n = "LOCATION=路径";
```

```
ALTER SYSTEM
SET LOG_ARCHIVE_DEST_1 = 'LOCATION=E:\log_archive_test\';
```

**远程归档目标:**

```
ALTER SYSTEM 
SET LOG_ARCHIVE_DEST_n = "SERVICE=STANDBY_DB1";
```

**强制的归档目标,如果出错,一定时间后重试:**

```
ALTER SYSTEM 
SET LOG_ARCHIVE_DEST_n = "LOCATION=路径 MANDATORY REOPEN=时间(秒)";
```

```
ALTER SYSTEM
SET LOG_ARCHIVE_DEST_4 = 'LOCATION=E:\log_archive_test\ MANDATORY ROPEN=30';
```

**可选的归档目标,如果出错,放弃归档:**

```
ALTER SYSTEM 
SET LOG_ARCHIVE_DEST_n = "LOCATION=路径 OPTIONAL";
```

##### 归档目标状态 LOG_ARCHIVE_DEST_STATE_n

**关闭归档目标1**

```
ALTER SYSTEM 
SET LOG_ARCHIVE_DEST_STATE_n = DEFER 
```

**打开归档目标2**

```
ALTER SYSTEM 
SET LOG_ARCHIVE_DEST_STATE_n = ENABLE 
```

##### 归档日志格式

```
LOG_ARCHIVE_FORMAT
```

##### 获取归档日志信息

**数据字典**

- V$ARCHVIED_LOG
- V$ARCHVIE_DEST
- V$LOG_HISTORY
- V$DATABASE
- V$ARCHIVE_PROCESSES

**语句**

```
ARCHIVE LOG LIST;
```

```
SQL> ARCHIVE LOG LIST
数据库日志模式         存档模式
自动存档              启用
存档终点              USE_DB_RECOVERY_FILE_DEST
最早的联机日志序列     58
下一个存档日志序列     60
当前日志序列           60
```

#### 查看oracle数据库是否为归档模式：

**视图V$DATABASE**

```
SELECT name
      ,log_mode
FROM V$DATABASE;
```

