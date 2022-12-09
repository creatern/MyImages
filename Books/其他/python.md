# Python

## pip python包的管理工具

 -cmd pip-v 查看pip版本

 环境变量 安装Python中勾选Add Python 3.x to PATH

 手动配置 此电脑-->环境变量-->Path-->新建-->添加pip的路径

```
pip install 安装指定的包
pip uninstall 删除指定的包
pip list 显示已经安装的包
pip freeze 显示已经安装的包，并以指定的格式显示
```

### 修改pip下载源

 `pip install 包名 -i 源地址`

默认下载源：htpps://files.pythonhosted.org/     国外(可能下载失败)(如果网络好)

国内常用源列表

- 阿里云 <https://mirrors.aliyun.com/pypi/simple/>
- 中国科技大学 <https://pypi.mirrors.ustc.edu.cn/simple/>
- 豆瓣 <https://pypi.douban.com/simple/>
- 清华大学 <https://pypi.tuna.tsinghua.edu.cn/simple/>

## 终端运行

1)python解释器 Python 3.x (64-bit)

`C:\Users\zjk10>python`

  退出python环境

```
 1.exit()
 2.ctrl + z ==> enter
```

2)ipython解释器

`C:\Users\zjk10>ipython`

## python基础

### 注释

```
单行注释 #
多行注释 '''  '''
```

### 变量

可以变化的量，可以随时进行修改

程序用来处理数据，变量用来存储数据

```
a = 1
print(a)
```

#### 变量类型

- number 数值  
- int
- long
- float
- complex
- boolean 布尔 流程控制语句
- True
- False
- string 字符串 单引号/双引号
- list [] 列表 多数据存储列表访问 第一个下标为0，[0],而不是1
- tuple () 元组
- dictionary dict {} 字典 scrapy框架使用

```
list 列表
a_list = [1,2,'s']
tuple 元组
b_tuple = (1,2)
dict 字典
变量 = {key1:value1,key2,value2,……}
person = {'name':'张三','age':18} 
```

#### 查看数据类型

变量没有类型，数据才有类型

```
type(变量)
print(type(变量))
string -> str
boolean -> bool
```

#### 变量命名规范

标识符和关键字

1. 标识符由字母、下划线_、数字组成。且数字不能开头
1. 严格区分大小写  "SQL不区分大小写"
1. 不能使用关键字

```
1)驼峰命名法
  第一个单词以小写字母开头，第二个单词的首字母大写
  my_Name
  第一个单词的首字母都大写 
  MyName
2)下划线_连接
  my_name
```

关键字

一些具特殊功能的标识符

已被官方使用，不允许开发者自己定义和关键字相同的标识符
|          |        |       |        |        |         |        |
|----------|--------|-------|--------|--------|---------|--------|
| False    | None   | True  | and    | assert | break   | class  |
| continue | def    | elif  | else   | except | finally | for    |
| from     | global | if    | import | in     | is      | lambda |
| nonlocal | not    | or    | pass   | raise  | return  | try    |
| while    | with   | yield |        |        |         |        |

#### 类型转换

强制转换为谁就写什么方法

1)转换为整型

```
int(变量)
1. float --> int
   返回小数点前数据
2. boolean --> int
   True --> 1,False --> 0
3.string --> int 不能直接转换
  1) print(int('123'))
  2)字符串包含非法字符，不能转换
  '123.456' string -> float -> int
  '12ab' 报错 
```

2)转换为浮点数

```
float(变量)
1. string -->float
2. int --> float
   a =1
   print(float(a))
```

3)转换为字符串

`str(变量)   --不是string`

4)转换为布尔类型

  非空即True,空即False

```
bool(变量)  --不是boolean
1. int --> boolean
   0 --> False,1 --> True
2.float --> boolean
   同int
3.string --> boolean
   字符串有内容(空格也算) --> True
   字符串无内容'' --> False 
4.list --> boolean
5.tuple -->boolean
6.dictionary -->boolean
```

### 运算符

#### 算术运算符

```
+ - * /
// 取整除
% 取余
** 指数
```

不同类型的数字混合运算，整数会转换成浮点数计算

```
字符串的算术运算
1.字符串的加法 --拼接
a = '123'
b = 'abc'
print(a + b)  --'123abc'
2.字符串的乘法 --重复
a = 'abc'
print(a * 2)  --'abcabc'
```

#### 赋值运算符

= 把等号右边的结果赋给左边的变量

1.单个变量赋值

`a = 1`

2.同时为多个变量赋值

  1)等号连接

`a = b =1`

  2)逗号分隔

```
a,b = 1,2
交换
a,b = b,a
```

#### 复合赋值运算符

- +=
- -=
- *=
- /=
- //=
- %=
- **=

`a += c -> a = a + c`

#### 比较赋值运算符

返回的都是boolean

- ==
- !=      --python2使用 <>
- \>
- \>=
- <
- <=

所有比较运算符中，返回1表示真，返回0表示假

#### 逻辑运算符

##### **and**  与

1. 只要有一个运算数是False结果就是False，只有所有运算数都为True，结果才是True
1. 做取值运算时，取第一个为False的值，若所有值都为True，取最后一个值

##### **or**  或

1. 只要有一个运算数是True结果就是True，只有所有运算数都为False，结果才是False
1. 做取值运算时，取第一个为True的值，若所有值都为False，取最后一个值

##### **not**  非

布尔"非"，not x，如果x为True则返回False，如果x为False则返回True

##### 逻辑运算符性能提升

and的性能优化 短路与

当and前面的结果是False时，那么后面的代码就不执行了。

```
a = 36
a > 10 and print('hello world')
a < 10 and print('hello world')
```

or的性能优化 短路或

当or前面的结果是True时，那么后面的代码就不执行了。

```
a = 36
a > 10 or print('hello world')
a < 10 or print('hello world')
```

### 输入输出

#### 输出

1.普通输出

print()

2.格式化输出

%s 字符串

%d 数值

```
age = 18
name = '小明'
print('名字是%s,年龄是%d' % (name,age))
```

#### 输入

input()

```
password = input('输入密码')
print(password)
```

### 流程控制语句

满足一个条件时，才会执行代码，否则不执行

if下面的代码必须是有一个缩进

代码的缩进是一个Tab或4个空格

#### if判断语句

```
if 要判断的条件:
    条件成立时，要做的事情(代码)
```

```
age = 30
if age >= 18:
    print('成年')
if age < 18:
    print('未成年')
```

else 不满足条件时操作 if else语法

```
age = input('your age')
if int(age) >= 18:
    print('you can')
else:
    print(you can't)

input返回的时string,与int不可比较，需要强制转换为int()
```

elif
age = int(input('your age:'))
if age >= 90:
    print('old')
elif age >= 50:
    print('half')
elif age >= 18:
    print('desult')
else:
    print('young')

#### for循环

```
for 临时变量 in 列表或字符串等可遍历的对象:
    循环满足条件时执行的代码(方法体)
```

循环(遍历) 一个一个的输出

```
a = 'china'
for i in a:
    print(i)
```

i 是字符串中一个又一个的字符的变量
a 代表的是要遍历的数据

#### range()方法

range()方法的结果 一个可以遍历的对象
range() 左闭右开
range(起始值,结束值,步长)

```
1.
for i in range(5):
    print i
--0,1,2,3,4
2.
range(1,6)
--1,2,3,4,5
3.
range(1,10,3)
--1,4,7
```

循环一个列表
 应用 会爬取一个列表返回给我们 爬虫

1.遍历列表中的元素

2.遍历列表中的下标

    判断列表中元素的个数  len(列表)

```
a_list = ['周','林','李']
for i in a_list:
    print(i)
for i in range(len(a_list)):
    print(i)
```

### 数据类型高级

#### 字符串高级

```
len 获取长度 len函数可以获取字符串的长度
find 查找内容 查找指定内容在字符串内是否存在，
     如果存在就返回该内容在字符串中第一次出现的开始位置索引值，
     如果不存在 则返回-1
startwith 判断 判断字符串是否以……开头 
endswith 判断 判断字符串是否以……结尾
count 计算出现次数 返回str在start和end之间在字符串里出现的次数
replace 替换内容 替换字符串指定的内容，如果指定次数count，则替换不会超过count次
split 切割字符串 通过参数的内容切割字符串
upper 修改大写 将字符串转换为大写
lower 修改小写 将字符串转换为小写
strip 空格处理 去空格
join 字符串拼接 字符串拼接
```

```
s = 'China'
1.len
print(len(s))
--5
2.find
print(s.find('a'))
--4
3.startswith
print(s.startswith('C'))
--True
4.endswith
print(s.endswith('n'))
--False
5.count
print(s.count('i'))
--1
6.replace
print(s.replace('a','ese'))
--'Chinese'
7.split
print(s.split('Chin'))
--'a'
8.upper
print(s.upper())
--CHINA
9.lower
print(s.lower())
--china
10.strip
s1 = '  ch  i   na  '
print(len(s1.strip()))
--5
11.join
s2 = 'hello'
s3 = 'world'
print(s3.join(s2))
--'hworldeworldlworldlworldoworld'
```

#### 列表高级 增删改查

##### 添加元素

- append 在末尾添加元素 追加

列表.append(新添元素)

- insert 在指定位置插入元素

列表.insert(index下标,元素)

- extend 合并两个列表，将另一个列表的元素逐一添加

列表.extend(列表)

```
1.append 
name = ['小明','小红']
name.append('小军')
print(name)
--'小明','小红','小军'
2.insert
char = ['a','c']
char.insert(1,'b')
print(char)
--'a','b','c'
3.extend
num_list1 = [1,2,3]
num_list2 = [4,5,6]
num_list1.extend(num_list2)
print(num_list1)
--1,2,3,4,5,6

```

##### 修改

将列表中元素修改，通过下标修改

列表[下标] = 元素

```
city_list = ['北京','上海','深圳']
city_list[2] = '大连'
print(city_list)
--'北京','大连','深圳'
```

##### 查找

查看指定的元素是否存在

in 存在 如果存在，结果为True，否则为False

not in 不存在 如果不存在，结果为True，否则为False

```
food_list = ['菜','肉','饭']
food = iniput('请点餐')
if food in food_list:
    print('有')
else:
    print('无')
```

##### 删除元素

- del 根据下标进行删除 爬取的数据，有个别不需要时

del 列表[下标]

- pop 删除最后一个元素

列表.pop()

- remove 根据元素的值进行删除

列表.remove(元素)

```
a_list = ['a','b','c','d','e']
1.del
del a_list[1]
print(a_list)
--'b','c','d','e'
2.pop
a_list.pop()
print(a_list)
--'a','b','c','d'
3.remove
a_list.remove('b')
print(a_list)
--'a','c','d','e'
```

#### 元组高级

元组的元素不能修改，也不能删除

定义只有一个数据的元组，需要在唯一的元素后写一个逗号

但元组只有一个元素，那么它是整型数据

```
a_tuple = (1,2,3,4)
print(a_tuple[0])
--1
b_tuple = (1)  int
b_tuple = (1,) tuple

```

#### 切片

对操作的对象截取其中一部分的操作

字符串，列表，元组都支持切片操作

语法: [起始:结束:步长]  [起始:结束]

选取的区间从起始位开始，到结束的前一位结束(不包括结束位)

步长表示选取间隔

- 索引 通过下标取某一个元素
- 切片 通过下标取某一段元素

```
s = 'hello world'
print(s[4]) 字符串的第五位元素  不是第四位,下标从0开始
--'o' 
print(s[3:7]) 包含下标3，不包含下标7
--'lo w' 
print(s[1:]) 从下标1开始，取后面所有
-- 'ello world'
print(s[:4]) 从起始开始，取到下标4的元素，不包括结束位
--'hel'
print(s[1:5:2]) 从下标1开始，取到下标5的前一个元素，步长为2
--'el'

```

#### 字典高级

##### 查看元素

- key

字典['key']

- .get()

字典.get('key'[,默认值])

```
info = {'name':'部长','age':18}
1.key 获取不存在的key会发生异常
print(info['age']) 
--18
2..get()
1)print(info.get('sex')) 读取不存在的key为NULL，不会出现异常
--null 
2)print(info.get('sex','男')) 获取不存在的key，可以提供一个默认值
--男
```

##### 修改元素

字典的每个元素中的数据可以修改的，只要通过key找到即可

- 字典['key'] = 元素

```
person = {'name':'小明','age':18}
person['name'] = '法外狂徒'
print(person)
--{'name':'法外狂徒','age':18}
```

##### 添加元素

-字典['key'] = 元素

若这个key键在字典不存在，则新增元素，否则即修改

```
person = {'name' = '张三'}
person['age'] = 18
```

##### 删除元素

###### 删除字典中指定的某一个元素

- del 字典['key']

`del person['age']`

###### 删除整个字典

- del 字典

`del person`

###### 清空字典，但是保留字典对象(删除数据，保留结构{})

- 字典.clear()

`person.clear()`

###### 遍历字典

`person = {'name':'张三','age':18,'sex':'男'}`

- 遍历字典的key

字典.keys()
 方法 获取字典中的所有key值

```
for i in person.keys()
    print(i)
```

- 遍历字典的value

字典.values() 方法 获取字典中所有的value值 value是一个变量，我们可以随便起名

```
for i in person.values()
    print(i)
```

- 遍历字典的项/元素

字典.items()

```
for i in person.items()  
      print(i)

```

### 函数

很多重复的业务逻辑，重复出现的时候

定义函数的格式

```
def 函数名():
    代码
```

调用函数

`函数名()`

```
def f1():
    print('你好')
f1()
```

1. 函数定义好之后，函数里的代码不会立即执行，如果想要执行函数体里的内容，需要手动的调用函数
1. 每次调用函数，函数都会从头开始执行，当这个函数的代码执行结束后，意外着调用结束了

- 定义函数时，()里写等待赋值的参数
- 调用参数时，()里写真正要运行的数据

#### 函数参数

- 让一个函数更通用，在定义一个函数的时候可以让函数接收数据
- 调用一个带参数的函数时，要在括号中传递数据

```
def sum(a,b):
    c = a + b
    print(c)
位置传参 --按位置一一对应传递参数
sum(1,2)
--3
关键字传参 --
sum(a = 1,b = 2)
--3

```

形参 形式参数 定义时小括号中的参数，用来接收数据用的

实参 实际参数 调用时小括号中的参数，用来传递参数用的

#### 函数返回值

程序中函数完成一件事情后，最后调用者的结果

想要在函数中吧结果返回后调用者，需要在函数中使用return

- return 存在函数中 使用一个变量来接受函数的返回值

```
def sum(a,b):
    c = a + b
    return(c)
print(sum(1,2))
```

#### 局部变量

在函数内部定义的变量

特点 其作用范围是函数内部，而函数的外部不可使用

```
def f1():
    a = 1
    print(a)
print(a) 
--报错
def f1()
--1
```

#### 全局变量

定义在函数外部的变量

特点 可以在函数的外部使用，也可以在函数内部使用

```
a = 1
def f1():
    print(a)
f1()
print(a)
```

在满足条件的情况下，要使用作用域最小的那个变量范围

### 文件

#### 文件的打开与关闭

open函数可以打开一个已经存在的文件，或创建一个文件

- open(文件路径,访问模式)

`f = open('test.txt','w')`

文件夹是不可以创建的，暂时需要手动创建

##### 文件路径

绝对路径 指的是绝对位置，完整地描述了目标的所在地，所有的目录屋级关系一目了然

相对路径 从当前文件的所在文件夹开始的路径

```
test.txt 在当前文件夹查找test.txt文件
./test.txt 在当前文件夹查找test.txt文件 
   ./ 表示当前文件夹
../test.text 从当前文件夹的上一级文件夹查找test.txt
   ../ 表示的是上一级的文件夹
demo/test.txt 在当前文件夹里查找demo这个文件夹，并在这个文件夹(demo)里查找test.txt文件
```

#### 访问模式

- r

1. 以只读方式打开
1. 文件的指针将会放在文件的开头
1. 若文件不存在，则报错
1. 默认模式

- w

1. 打开一个文件只用于写入
1. 如果该文件已存在，则将其覆盖
1. 如果该文件不存在，则创建新文件

- a

1. 打开一个文件用于追加
1. 如果该文件已存在，则文件指针将放在文件的结尾(即新的内容将会被写入到已有的内容之后)
1. 如果该文件不存在，创建新文件进行写入

- r+

1. 打开一个文件用于读写
1. 文件的指针将会放在文件的开头

- w+

1. 打开一个文件用于读写
1. 如果该文件已存在，则将其覆盖
1. 如果该文件不存在，则创建新文件

- a+

1. 打开一个文件用于读写
1. 如果该文件已存在，则文件指针将放在文件的结尾
1. 文件打开时会是追加模式
1. 如果文件不存在，创建新文件用于读写

- rb

1. 以二进制格式打开一个文件用于只读
1. 文件的指针将会放在文件的开头

- wb

1. 以二进制格式打开一个文件只用于写入
1. 如果该文件已存在，则将其覆盖
1. 如果该文件不存在，则创建新文件

- ab

1. 以二进制格式打开一个文件用于追加
1. 如果该文件已存在，则文件指针将放在文件的结尾
1. 如果该文件不存在，创建新文件进行写入

- rb+

1. 以二进制格式打开一个文件用于读写
1. 文件的指针将会放在文件的开头

- wb+

1. 以二进制格式打开一个文件用于读写
1. 如果该文件已存在，则将其覆盖
1. 如果该文件不存在，则创建新文件

- ab+

1. 以二进制格式打开一个文件用于读写
1. 如果该文件已存在，则文件指针将放在文件的结尾
1. 如果该文件不存在，创建新文件进行读写

#### 文件的关闭

- fp.close()

减少内存占用

```
fp = open('a.txt','w')
fp.write('hello')
fp.close()
```

#### 文件的读写

##### write方法

```
fp = open('test.txt','w')
fp.write('hello world\n' * 5)
fp.close()
```

如果文件存在，则先清空原先的数据，然后再写

##### append方法

```
fp.open('test.txt','a')
fp.write('hello')
fp.close()
```

##### read方法

默认 read时一个一个字节的读取，效率较低

```
1.一字节一字节读取 默认
fp = open('test.txt','r')
content = fp.read()
print(content)
2.一行一行读取，但只读一行
fp = open('test.txt','r')
content = fp.readline()
print(content)
3.按照行来读取，但会将所有数据都读取到，并且会以一个列表的形式返回，而列表的元素是一行一行的数据
fp = open('test.txt','r')
content = fp.readlines()
print(content)
```

#### 文件的序列化和反序列化

如果是一个对象(列表、字典、元组等)，就无法直接写入到一个文件里，

需要对这个对象进行序列化，然后才能写入到文件里

- 序列化 设计一套协议，按照某种规则，把内存中的数据转换韦字节序列，保存到文件

对象 -- 字节序列

- 反序列化 反之，从文件的字节序列恢复到内存中

字节序列 -- 对象

使用JSON模块实现序列化和反序列化

##### JSON模块

- 是一种轻量化的数据交换标准
- JSON的本质是字符串
- JSON提供了dump和dump方法，将一个对象进行序列化

###### dumps方法

作用是把对象转换成为字符串，，它本身不具备将数据写入到文件的功能

```
1.
import json
file = opne('names.txt','w')
names = ['张三','李四','王五']
result = json.dumps(names)
file.name(result)
f = open('test.txt','w')
2.
name_list = ['A','B']
import json
names = json.dumps(name_list)
f.write(names)
f.close
```

使用scrapy框架是，框架会返回一个对象，我们要将对象写入到文件中，就需要使用json.dumps()

###### dump

将对象转换为字符串的同时，指定一个文件的对象，把转换后的字符串写入到文件里

```
fp = open('test.txt','w')
name_list = ['A','B']
import json
json.dump(name_list,fp)
fp.close()
```

反序列化 将json的字符串变成一个python对象

```
1.loads加载数据
fp = open('test.txt','r')
content = fp.read()
import json
result = json.loads(content)
print(result)
2.loads加载文件
fp = open('text.txt,'r')
import json
result  = josn.loads(fp)
print(result)
fp.close()
```

### 异常

格式

```
try:
    可能出现异常的代码
except 异常的类型
    提示
```

```
try:
    fp.open('test.txt','r')
    fp.read()
except FileNotFoundError:
    print('请稍后再试')
```

# 爬虫

## 页面结构的介绍

HTML 5 file

```
    <html lang = "en">
        <head>
            <meta charset = 'utf-8'>
            <title> Title </title>
        <head>
        <body>
            <table>
                <tr> 
                    <td>
                    </td>
            </table>
        </body>
    </html>
```

- table 表格
- tr 行
- td 列
- ul li 无序列表 爬虫使用场景非常多
- ol li 有序列表

```
<!DOCTYPEhtml>
<heml lang = "en">
<head>
    <meta charset = 'utf-8'>
    <title>Title</title>
</head>
<body>
<table width = "200px" height = "200px" border = "1px">
<tr>
<td>
    姓名
</td>
<td>
    年龄
</td>
<td>
    性别
</td>
</tr>
<tr>
<td>
    张三
</td>
<td>
    18
</td>
<td>
    男
</td>
</tr>
</table>
<ul>
<li> A等 </li>
<li> B等 </li>
</ul>
<ol>
<li> 睁眼 </li>
<li> 穿衣 </li>
</ol>
<a href = "http://atguigu"> 尚硅谷 </a>
</body>
</html>
```

## 爬虫相关概念

### 解释

1. 通过一个程序，根据url(<http://www.taobao.com>)进行爬取网页，获取有用信息
1. 使用程序模拟浏览器，请向服务器发送请求，获取相应的信息

#### 爬虫核心

1. 爬取网页 包含了网页的所有内容
1. 解析数据 将网页中得到的数据进行解析
1. 难点 爬虫与反爬虫之间的博弈

#### 爬虫的用途

- 数据分析/人工数据集
- 社交软件冷启动
- 舆情监控
- 竞争对手监控

#### 爬虫分类

##### 通用爬虫

- 实例

百度、360、google、sougou等搜索引擎 --伯乐在线

- 功能

访问网页->爬取数据->数据存储->数据处理->提供检索引擎

- 网站排名(SEO)

1. 根据PageRank算法进行排名(参考各个网站流量、点击率等指标)
1. 百度竞价排名

- 缺点

1. 抓取的数据大多数是无用的
1. 不能根据用户的需求来精确获取数据

- robots协议

一个约定俗成的协议，添加robots.txt文件，来说明本网站哪些内容不可以被爬取，起不到限制作用

自己写的代码无需遵守

##### 聚焦爬虫

- 功能

根据需求，实现爬虫程序，抓取需要的数据

- 设计思路

1. 确定要爬取的url --如何获取url
1. 模拟浏览器通过http协议访问url，获取服务器返回的html代码 --如何访问
1. 解析html字符串，根据一定规则提取需要的数据 --如何解析

#### 反爬手段

##### user-agent

user-agent文名为用户代理，UA，

是一个特殊字符串头，使服务器能够识别客户使用的操作系统及版本、CPU类型、浏览器版本、浏览器渲染引擎、浏览器语言、浏览器插件等

##### 代理IP

- 西次代理
- 快代理

什么是高匿名、匿名、透明代理?

1. 使用透明代理 对方服务器可以知道你使用了代理，知道你的真实IP
1. 使用匿名代理 对方服务器可以知道你使用了代理，但不知道你的真实IP
1. 使用高匿名代理 对方服务器不可以知道你使用了代理，不知道你的真实IP

##### 验证码访问

- 打码平台
- 云打码平台
- 超级老鹰

##### 动态加载网页

网络返回的是js数据，并不是网页的真实数据

selenium驱动真实的浏览器发生请求

##### 数据加密

分析js代码

## urllib库的使用

- urllib.request.urlopen()  模拟浏览器向服务器发送请求
- response 服务器返回的数据是HttpResponse
- decode 字节 --> 字符串 解码
- encode 字符串 -->字节 编码
- read() 字节形式读取二进制  --read(5) 返回前几个字节
- redline() 读取一行
- readlines() 读取多行
- getcode() 获取动态码
- geturl() 获取url
- getheaders() 获取headers
- url.request.urlretrieve() 请求网页，图片，视频

### 使用urllib来获取百度的源码

```
import urllib.request
url = 'http://www.baidu.com'  
#定义一个url，即要访问的地址
response = urllib.request.urlopen(url)
#模拟浏览器向服务器发送请求 response
content = response.read().decode('utf-8')
#获取相应的网页的源代码 content
#read方法 返回的是字节形式的二进制数据，要将二进制数据转换为字符串
#decode('(该页面)编码的格式') 二进制 -> 字符串
print(content)
#打印数据
```

### 模拟浏览器向服务器发送请求 response

response 是HTTPResponse的类型

1. 读取字节 content = response.read()
1. 读取多个字节 content = response.read(5)
1. 读取一行 content = response.readline()
1. 读取多行 content = response.readlines()

- 返回状态码 print(response.getcode()) --如果是200了，那么证明我们的逻辑没有错
- 返回url地址 print(response.geturl())
- 获取状态信息 print(response.getheaders())

### 下载

#### 下载网页

```
url_page = 'http://www.baidu.com'
urllib.request.urlretrieve(url_page,'baidu.html')
#url代表的是下载的路径 filename 文件的名字
```

#### 下载图片

```
url_image = '图片的链接地址/图片地址'
urllib.request.urlretrieve(url = url_page,filename = '1.jpg')
```

#### 下载视频

右键检查 -> 左上角 -> 点击视频 定位

```
url_video = '视频地址'
urllib.request.urlretrieve(url_video,'2.mp4')
```

### 请求对象的定制

UA user-agent

- 语法

`request = urllib.request.Request()`

- url组成

`url = 'https://www.baidu.com/?wd='`

1. 协议 http/https
1. 主机 www.baidu.com
1. 端口号 80/443
1. 路径 s
1. 参数 wd=
1. 锚点 #

- 端口号  

|         |       |
|---------|-------|
| https   | 80    |
| https   | 443   |
| mysql   | 3306  |
| oracle  | 1521  |
| redis   | 6379  |
| mongodb | 27017 |

```
import urllib.request
url = 'https://www.baidu.com'
response = urllib.request.urlopen(url)
content = response.read().decode('utf-8')
print(content)
#被反爬 User-Agent https协议
```

### 找自己的UA UA反爬

右键检索-->Network-->刷新页面-->Name-->域名-->heads-->下面滑-->User-Agent

```
import urllib.request
url = 'https://www.baidu.com'
headers = {'user-agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36'}
request = urllib.request.Request(url = url,heads = heads)
#因为urlopen方法中不能存储字典，所以headers不能传递进去
#请求对象的定制 解决反爬
#注 因为参数顺序的问题，不能直接写url和headers，中间还有一个data参数，需要关键字传参
response = urllib.request.urlopen(request)
content = response.read().decode('utf-8')
print(content)
```

### 编解码

Unicode 编码

#### get请求的quote方法

- Headers - Gemeral - Request Method: GET
- Preview - User-Agent

```
import urllib.request
import urllib.parse
url = 'https://www.baidu.com/s?wd='
headers = {'user-agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36'}
name = urllib.parse.quote('周杰伦')
url = url + name
request = urllib.request.Request(url = url,headers = headers)
response = urllib.request.urlopen(request)
content = response.read().decode('utf-8')
print(content)
```

#### get请求的urlencode方法

应用场景 多个参数时

```
import urllib.request
import urllib.parse
base_url = 'https://ww.baidu.com/s?'
data = {'wd':'周杰伦','sex':'男','loaction':'中国台湾省'}
new_data = urllib.parse.urlencode(data)
url = base_url + new_data
headers = {'user-agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36'}
request = urllib.request.Request(url = url,headers = headers)
response = urllib.request.urlopen(request)
content = response.read().decode('utf-8')
print(content)
```

#### post请求百度翻译

post请求的参数必须进行编码，编码之后必须调用encode方法，

且不会拼接在url的后面，而是放在请求对象定制的参数中

```
import urllib.request
import urllib.parse
import json
url = 'https://fanyi.baidu.com/sug'
headers = {'user-agent':Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36}
keyworld = input('请输入要翻译的单词')
data = {'kw':''}
data = urllib.parse.urlencode(data).encode('utf-8')
request = urllib.request.Request(url =url,headers = headers,data = data)
response = urllib.request,urlopen(request)
print(response.read().decode('utf-8'))
obj = json.loads(content)
print(obj)
```
