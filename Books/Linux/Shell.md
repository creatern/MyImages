# Shell 基础

## 规范

```shell
#!/bin/bash
# 解释器

# 注释 <<关键字
<<END
 注释
END

# 执行的命令
echo "Hello World"
```

## 执行

```shell
# 1.直接执行 需要可执行权限
# 在当前shell中打开一个子shell来执行脚本内容，当脚本内容结束，则子shell关闭，回到父shell中
chmod +x test.sh
./test.sh
# 2.使用解释器 不需要执行权限 
# 在当前shell中打开一个子shell来执行脚本内容，当脚本内容结束，则子shell关闭，回到父shell中
bash test.sh
sh test.sh
# 3.使用 . 或 source的方式 
# 使脚本内容在当前shell里执行，而无需打开子shell
. test.sh
source test.sh
# 子shell中设置的当前变量，父 shell 是不可见的
```

## 引号

### 单引号和双引号

#### 对特殊符号的处理

**单引号**：将特殊符号变成普通符号
**双引号**：保持特殊符号的特殊使用

```shell
[root@www Test]# echo "current position is `pwd`"
current position is /root/Test
[root@www Test]# echo 'current position is `pwd`'
current position is `pwd`
```

### 命令替换 ``反引号

- 使用命令的输出结果来替换命令

```shell
tar -czvf /root/Test/log-`date +%Y%m%d`.tar.gz /var/log

[root@www Test]# echo "current position is `pwd`"
current position is  /root/Test
```

# 变量

## 系统变量

| 系统变量     | 说明                                                                          |
| :---------- | :--------------------------------------------------------------------------- |
| $PATH       | 执行文件查找的路径，文件查找的顺序与PATH的变量的顺序有关，目录与目录中间以:冒号隔开 |
| $HOME       | 当前用户主目录                                                                 |
| $HISTSIZE　 | 代表可以容纳多少条历史命令                                                      |
| $LOGNAME    | 当前用户的登录名                                                               |
| $HOSTNAME　 | 指主机的名称                                                                  |
| $SHELL      | 当前用户Shell类型                                                             |
| $LANG 　    | 当前的语系类型                                                                 |
| $MAIL　     | 当前用户的邮件存放目录                                                         |
| $PS1        | 基本提示符                                                                    |
| $RANDOM     | 随机数的变量                                                                  |

## 自定义变量

### 定义变量 

**变量定义规则**

1. 变量名称可以由字母、数字和下划线组成，但是不能以数字开头，环境变量名建议大写。
2. 等号两侧不能有空格
3. 在 bash 中，变量默认类型都是字符串类型，无法直接进行数值运算。
4. 变量的值如果有空格，需要使用双引号或单引号括起来。

```shell
变量名=变量值
# 或
declare 变量名=变量值
```

#### declare

- declare命令 用于声明和显示已存在的shell变量。当不提供变量名参数时显示所有shell变量。declare命令若不带任何参数选项，则会显示所有shell变量及其值。declare的功能与typeset命令的功能是相同的。

![](C:/notebook/pictures/Snipaste_2022-12-17_17-10-06.png =400x)

#### set 

- set命令 作用主要是显示系统中已经存在的shell变量，以及设置shell变量的新变量值。
- 使用set更改shell特性时，符号“+”和“-”的作用分别是打开和关闭指定的模式。
- set命令**不能够定义新的shell变量**。如果要定义新的变量，可以使用declare命令以变量名=值的格式进行定义即可。

![](C:/notebook/pictures/Snipaste_2022-12-17_17-14-12.png =500x)

### unset 撤销变量

- unset命令 用于删除已定义的shell变量（包括环境变量）和shell函数。
- unset命令不能够删除具有只读属性的shell变量和环境变量

![](C:/notebook/pictures/Snipaste_2022-12-17_17-03-41.png =300x)

```shell
unset 变量
```

### readonly 声明静态变量 

- readonly命令 用于定义只读shell变量和shell函数。readonly命令的选项-p可以输出显示系统中所有定义的只读变量。

语法

![](c:/notebook./pictures/Snipaste_2022-12-17_17-05-03.png =300x)

```shell
readonly 变量名=变量值
```

### export 提升为全局变量

- export命令 用于将shell变量输出为环境变量，或者将shell函数输出为环境变量。
- 一个变量创建时，它不会自动地为在它之后创建的shell进程所知。而命令export可以向后面的shell传递变量的值。当一个shell脚本调用并执 行时，它不会自动得到原为脚本（调用者）里定义的变量的访问权，除非这些变量已经被显式地设置为可用。export命令可以用于传递一个或多个变量的值到任何后继脚本。

![](c:/notebook/pictures/Snipaste_2022-12-17_17-07-19.png =450x)

```shell
export 变量名
```

```shell
# 给变量A、B赋值
[zjk@localhost root]$ A=1
[zjk@localhost root]$ B=2
[zjk@localhost root]$ echo $A+$B
1+2  # bash变量默认是字符串
# 撤销变量
[zjk@localhost root]$ unset A
# 声明静态变量
[zjk@localhost root]$ readonly C="Hello World"
[zjk@localhost root]$ echo $C
Hello World
# 静态变量不能被unset
[zjk@localhost root]$ unset C
bash: unset: C: cannot unset: readonly variable
# 提升为全局变量
[zjk@localhost root]$ export B
```

## 特殊变量

| 特殊变量      | 说明                                                                                                                                                  |
| :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$n`         | n 为数字，`$0` 代表该脚本名称，`$1-$9` 代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如`${10}`                                            |
| `$#`         | 获取所有输入参数个数，常用于循环,判断参数的个数是否正确以及加强脚本的健壮性                                                                                  |
| `$*` 和 `$@` | 代表命令行中所有的参数；`$*`把所有的参数看成一个整体；`$@`把每个参数区分对待                                                                                |
| `$?`         | 最后一次执行的命令的返回状态。如果这个变量的值为 0，证明上一个命令正确执行；如果这个变量的值为非0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。 |

```shell
[root@localhost shellTest]# vim test01.sh

#!/bin/bash
echo "文件名称：" $0
echo "参数1：" $1
echo "参数2：" $2
echo "当前输入参数的个数：" $#
echo "命令行中的所有参数：" 
echo -e "\t\$\*" $*
echo -e "\t\$\@" $@
echo "命令是否正确执行：" $?

[root@localhost shellTest]# bash test01.sh 参数1 参数2
文件名称： test01.sh
参数1： 参数1
参数2： 参数2
当前输入参数的个数： 2
命令行中的所有参数：
        $\* 参数1 参数2
        $\@ 参数1 参数2
命令是否正确执行： 0
```

- 当它们被双引号“”包含时，`$*`会将所有的参数作为一个整体，以`“$1 $2 …$n”`的形式输出所有参数；`$@`会将各个参数分开，以`“$1” “$2”…“$n”`的形式输出所有参数

```shell
#!/bin/bash

for i in $*
do
    echo "\$*  $i"
done

for i in $@
do
    echo "\$@  $i"
done    

for i in "$*"
do
    echo "\$*  $i"
done

for i in "$@"
do
    echo "\$@  $i"
done 
```

```shell
[zjk@www ShellTest]$ . forTest02.sh 1 2 3
$*  1
$*  2
$*  3
$@  1
$@  2
$@  3
$*  1 2 3
$@  1
$@  2
$@  3
```

## 相关命令

### echo

### printf

### read 从键盘读取变量值

- read命令 从键盘读取变量的值，通常用在shell脚本中与用户进行交互的场合。该命令可以一次读取多个变量的值，变量和输入的值都需要使用空格隔开。在read命令后面，如果没有指定变量名，读取的数据将被自动赋值给特定的变量REPLY

![](c:/notebook/pictures/Snipaste_2022-12-17_16-10-32.png =250x)

```shell
#!/bin/bash

read -t 10 -p "请在10秒内输入: " value1
echo "输出：$value1"
```

### env 显示系统中已存在的环境变量

- 如果使用env命令在新环境中执行指令时，会因为没有定义环境变量“PATH”而提示错误信息“such file or directory”。此时，用户可以重新定义一个新的“PATH”或者使用绝对路径。

![](c:/notebook/pictures/Snipaste_2022-12-17_17-13-00.png =500x)

### locale 和 localectl

- localectl列出来的是当前软件所使用的语系，而localectl显示的当前系统的语系

**locale**

- locale -a：查看当前Linux系统支持的语系
- locale：查看当前软件的语系

**localectl**

```shell
# 查看当前系统语系
localectl   
#设置当前系统语系 同时修改配置文件
localectl set-locale LANG=en_US.UTF-8 
```

#### 语系变量的设置

**locale 不修改配置文件**

- 前提：我们设置语系变量时，一般建议更改**LANG与LC_ALL**这两个变量，因为设置这两个变量，其他语系变量都会替换成LANG与LC_ALL这两个变量的值。
- 注意事项：设置语系环境变量，只在此次命令行执行中有效，退出后失效，又恢复成原样
- 方法：LANG直接用=号赋值即可。LC_ALL需要使用export设置。我们设置LANG=zh_CN.UTF-8，发现其他的变量都变成zh_CN.UTF-8

```shell
LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```

**localectl 修改配置文件**

# 运算符

## 算术运算式

```shell
$((运算式)) 
# 或 
$[运算式]
```

```shell
[zjk@localhost ~]$ s=(1+2)
[zjk@localhost ~]$ s1=$((1+2))
[zjk@localhost ~]$ s2=$[1+2]
[zjk@localhost ~]$ echo -e "$s\n$s1\n$s2" 
1+2
3
3
```

## let

![](c:/notebook/pictures/Snipaste_2022-12-31_22-38-04.png =600x)

```shell
[root@www Test]# let A=1+2
[root@www Test]# echo $A
3
```

## bc

- bc命令 是一种支持任意精度的交互执行的计算器语言。
- bash内置了对整数四则运算的支持，但是并不支持浮点运算。

![](c:/notebook/pictures/Snipaste_2022-12-31_22-42-49.png =300x)

```shell
[root@www Test]# echo $[ 9.9 * 9 ]
-bash: 9.9 * 9 : syntax error: invalid arithmetic operator (error token is ".9 * 9 ")

# 算术操作高级运算bc命令它可以执行浮点运算和一些高级函数：
echo "1.212*3" | bc 
3.636
# 设定小数精度（数值范围）
echo "scale=2;3/8" | bc # 参数scale=2是将bc输出结果的小数位设置为2位。
0.37

# 进制转换
abc=192
echo "obase=2;$abc" | bc
#执行结果为：11000000，这是用bc将十进制转换成二进制。
abc=11000000
echo "obase=10;ibase=2;$abc" | bc
#执行结果为：192，这是用bc将二进制转换为十进制。

# 计算平方和平方根：
echo "10^10" | bc
echo "sqrt(100)" | bc
```

## 条件判断

### [] 和 test

![](c:/notebook/pictures/Snipaste_2022-12-19_16-17-55.png =600x)

- -z 或 -n 判断字符串是否为空，最好使用"$变量"的方式判断，因为空格也被看作不为空。
- -x 判断文件存在且当前用户拥有执行权限
- -s 判断文件存在且非空
- ! 取反
- -a 即&&
- -o 即||

**多条件判断**

- && 表示前一条命令执行成功时，才执行后一条命令，
- || 表示上一条命令执行失败后，才执行下一条命令

```shell
[zjk@www ~]$ [ 1 -eq 2 ]
[zjk@www ~]$ echo $?
1
[root@www ~]# [ -e /usr/local/apache2/conf/vhost/www.zjk.com.conf ]
[root@www ~]# echo $?
0
[root@www ~]# [ 1 -eq 2 ] || echo "1不等于2"
1不等于2
[root@www ~]# [ 1 -ne 2 ] && echo "1不等于2"
1不等于2
```

### [[]]

- 支持使用&&和||符号，而在[]中不支持
- 使用<或>时，系统进行的是排序操作，使用的是本地的locale语言顺序，可以使用`LANG=C`设置使用ASCII码排序
- 在[[]]中，==是模式匹配，可以使用通配符`*、?、[...]`等，而在[]中==是匹配字符串是否相同
- [[]]支持使用=~进行正则匹配，而[]不支持正则
- [[]]允许使用()进行运算，而[]仅支持部分

```shell
[root@bogon testDir01]# [[ b > a ]] && echo Y || echo N
Y
[root@bogon testDir01]# [[ a > A ]] && echo Y || echo N
N
[root@bogon testDir01]# [[ b > a  && a > c ]] && echo Y || echo N
N
[root@bogon testDir01]# testName=jack
[root@bogon testDir01]# [[ $testName == j* ]] && echo Y || echo N
Y
[root@bogon testDir01]# [[ $testName =~ c ]] && echo Y || echo N
Y
[root@bogon testDir01]# [[ $testName =~ [a-z] ]] && echo Y || echo N
Y
```

# 流程控制

## 判断

### if

![](c:/notebook/pictures/Snipaste_2022-12-19_16-44-15.png =650x)

```shell
#!/bin/bash

if [ $1 -eq $2 ]
then
        echo "$1等于$2"
elif [ $1 -lt $2 ]
then
        echo "$1小于$2"
else
        echo "$1大于$2"
fi
```

### case

![](c:/notebook/pictures/Snipaste_2022-12-19_16-54-28.png =650x)

```shell
#!/bin/bash

case $1 in
"1")
    echo "选项1"
;;
"2")
    echo "选项2"
;;
*)
    echo "其他选项"
;;
esac
```

## 循环

### for

![](c:/notebook/pictures/Snipaste_2022-12-19_16-58-12.png =700x)
![](C:/notebook/pictures/Snipaste_2022-12-19_17-00-18.png =700x)

```shell
#!/bin/bash

for ((i=0;i<10;i++))
do
    echo $i
done

for i in 第一名 第二名 第三名
do 
   echo $i 
done 
```

### while

![](c:/notebook/pictures/Snipaste_2022-12-19_17-12-31.png =700x)

```shell
#!/bin/bash

i=0

while [ $i -lt 3 ]
do
    i=$[ $i+1 ]
    echo $i
done
```

# 函数

## 系统函数

- 即shell命令

### basename

```shell
basename [string / pathname] [suffix]
```

- basename 命令会删掉所有的前缀包括最后一个（‘/’）字符，然后将字符串显示出来。
- 选项：suffix 为后缀，如果 suffix 被指定了，basename 会将 pathname 或 string 中的 suffix 去掉。

```shell
[zjk@www ShellTest]$ basename /root/test/hello.txt
hello.txt
[zjk@www ShellTest]$ basename /root/test/hello.txt .txt
hello
```

### dirname

```shell
dirname 文件绝对路径
```

- 从给定的包含绝对路径的文件名中去除文件名（非目录的部分），然后返回剩下的路径（目录的部分）

```shell
[zjk@www ShellTest]$ dirname /root/test/hellio.txt
/root/test
```

## 自定义函数

```shell
[ function ] 函数名[()]
{
    执行语句;
    [return int;]
}
```

1. 必须在调用函数地方之前，先声明函数，shell 脚本是逐行运行。
2. 函数返回值，只能通过`$?`系统变量获得，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。return 后跟数值 n(0-255)

```shell
#!/bin/bash

function sum()
{
    sumValue=$[ $1 + $2 ]
    echo "sum=$sumValue"
}

avg()
{
    echo "avg=$[ ($1 + $2) / 2 ]"
}

function minus
{
    echo "minus=$[ $1 - $2 ]"
}

read -t 10 -p "请输入值1: " num1;
read -t 10 -p "请输入值2：" num2;

sum $num1 $num2;
avg $num1 $num2;
minus $num1 $num2;
```
