# R入门
## 包 package
### 安装包 

`install.packages("package")`
### 加载包

`library(package)`

单独加载包内函数

`  包名::函数()`
### 更新包


```
update.packages(package) --指定package更新
                         --未填入package则逐个更新
```

### 移除包

`remove.packages()`
## 工作目录
### 获取帮助
1)某个函数的帮助

```
?help
help("函数")
```

2)某个关键词的帮助

```
??help
help.search("关键词")
```

3)某个包的帮助
`help(package = "包名")`
### 获取当前工作目录
`getwd()`
### 设置当前工作目录
`setwd("C:/路径")`
### 获取文件路径


```
file.choose()
read.文件格式(file.choose()) --或者路径
```

### 转义字符 \

### 保存R文件
.R

.RData

.Rhistory

.RData

鼠标点击


```
save()  
save.image("文件路径")
```


### 加载R文件
`load(file.choose())`
### 加载某个包内置的数据集
`data()`
### 列示当前环境中的对象
`ls() `
### 移除某个对象
`rm("对象")`
### 移除所有对象

`rm(list = ls())`
### 项目工程
## R常用数据类型
数值型

字符型

逻辑型
### 判断数据类型 

```
函数名(参数名 = 参数值)
is.numeric()
is.character()
is.logical()   --TRUE T FALSE F ;大小写敏感
```

### 转换数据类型


numeric <-> character <- logical

character --> logical (输出NA)

as.logical("TRUE") 输出TRUE /FALSE同


```
as.numeric()
as.character()
as.logical()
```

### 特殊值

```
NA    --缺失值
is.na(NA)
NULL  --空值
is.null(NULL)
NaN   --非数  (0/0)
is.nan(NaN)
Inf   --正无穷大
-Inf  --负无穷大
is.infinite(-Inf)
```
## R常用数据结构
### 赋值


1.对象名 -> 对象值  (快捷键 <Alt> + <->)

2. = 函数中对参数取值

```
v1 <- 1:5       --符号 : 1~5
v2 <- c(v1,2,3,4)
v3 <- rep(v2,each  = 2,times = 2)
v4 <- seq(from = 2,to = 2,by = 3,length.out = 3)
```


```
c()    --连接函数
       --包含强制转换 
              ->character
              ->numeric
rep()  --重复  --先each再times
   --each 每个的个数倍数 (1,2 -> 1,1,2,2)
   --times 整个重复倍数  (1,2 -> 1,2,1,2)
seq()  --序列
   --from 起点
   --to   终点
   --by   步长
   --length.out 向量长度(元素个数)
```

### 向量
一连串的数值或字符所构成的数据结构
只接受一种数据类型 (强制转换)
#### 向量元素名称


```
names(向量1) <- 向量2  --向量2给向量1(命名)
> v11 <- c(1,2,3,6)
> v22 <- c(3,4,5)
> names(v11) <- v22
> v11
3 4 5 <NA>
1 2 3  6
```
#### 向量长度


```
length()
length(向量) <- 长度
```

#### 向量索引 
--从向量提取单独的元素
--从1开始，不是0

```
向量[]
v1 <- c(1,2,3,4,5,7)
v1[1]         --提取下标为1的
v1[c(1,3,6)]  --提取下标为1,3,6的
v1[-c(1,2)]   --除下标为1,2的都提取
v1[向量元素名称]
v1[c(向量元素名称1,向量元素名称2)]
v1[条件]  --v1[v1 %% 2 == 1] --v1余数为1的
```

### 常量

```
pi           --3.141593
letters      --26个小写英文字母
LETTERS      --26个大写英文字母
month.name   --12个月份全称
month.abb    --12个月份缩写
```

### 矩阵
只接受一种数据类型，同向量 (强制转换)
```
matrix(
1:6,
nrow = 2,
ncol = 3,  --可只给出一个nrow/ncol，默认排列
byrow = F, --F 按列填充，T 按行填充
dimnames = list(c("r1","r2"),
                c("c1","c2","c3"))   --dimnames 矩阵行列名称
)
可用作填充容器
matrix(
NA,
nrow = 3,
ncol = 3,
byrow = T,
dimnames = list(c("r1","r2","r3"),
                c("c1","c2","c3"))
)
```

#### 行列名称

```
rownames(向量)
colnames(向量)
dimnames(向量)
赋值 dimnames(向量) = (c("r1","r2"),
                      c("c1","c2","c3"))
```

#### 维度信息

```
dim(向量)  --返回行和列的信息 (几行几列)
nrow(向量) --返回列的信息 (几列)
ncol(向量) --返回行的信息 (几行)
```

#### 矩阵索引
矩阵[参数1,参数2]

```
m1 <- matrix(
             1:6,
             nrow = 2,
             byrow = T,
             dimnames = list(c("r1","r2"),
                             c("c1","c2","c3"))
             )
m1[1,2]   --向量
m1[1,]    --向量
m1[,1]    --向量
m1[1:2,1:2]     --矩阵
m1["r1","c1"]   --向量
m1["r1",]    --向量
m1[,"c1"]    --向量
m1[c("r1","r2"),c("c1","c2")]  --矩阵
```

#### 转换成向量
`as.vector(矩阵)`
### 数组
`array()`
### 列表
数据类型可不同

```
list(成分名称 = 成分的值)
l1 <- list(com1 = v1,
           com2 = m1)
```

#### 长度信息
`length(列表)  --查询成分个数`
#### 名称
`names(列表)  --查询成分名称`
#### 列表索引

```
l1$com1        --返回向量
l1[["com1"]]   --返回矩阵 (只有com1成分)
l1[[2]]        --返回矩阵 (只有下标为2的成分，即com2)
l1["com1"]     --返回列表
l1[2]          --返回列表
```
#### 新建成分

```
列表$成分名称 <- 值
l1$com3 <- 1:6
```

#### 释放列表

```
unlist(列表)  --转为向量,含向量名称(列表的每个成分都有名称)
unlist(l1)
```
### 数据框
二维数据结构
特殊的列表

类似矩阵，但可以有不同的数据类型

读取外源数据时，R自动处理为数据框

```
data.frame()
df1 <- data.frame(c1 = 2:5,
                  c2 = letters[2:5])
```

#### 维度信息

```
dim(数据框)
nrow(数据框)
ncol(数据框)
```


#### 行列名称

```
dimnames(数据框)
names(数据框)   --返回列名称 等同colnames()
rownames(数据框)
colnames(数据框)
```

#### 数据框索引

```
df1[]
df1[1:2,2]    --返回向量
df1[,2]       --返回向量
df1[1,]       --返回数据框
df1["1","c1"]  --返回向量
df1["1",]    --返回数据框
df1[,"c1"]   --返回向量
df1[[2]]   --返回向量
df1$"c1"   --返回向量
df1[2]     --返回数据框
df1["c1"]  --返回数据框

```
#### 新建列或对已有的列进行修改
`df1$c3 <- 1:4`
#### 生成用于网格搜索的数据框
生成机械学习建模过程中网格搜索  数搜索空间的数据框

如随机森林，用索引去寻优的  数 

在若干个  数中逐个去尝试，找到最优的一个  数的组合

mtry 决定随机森林每一颗随机树在分裂时，首先从全部的自变量挑选出若干个变量，再在这若干个变量中去选择最优的分裂变量，即先选mtry个自变量，在从中选最优的一个分裂变量

ntree 决定由多少个决策树构成随机森林
```
expand.grid(mtry = 2:5,
            ntree = c(200,500))
```

## 常见运算与函数
### 基本运算


```
+  加
-  减
*  乘
/  除

循环扩展  --长度不为1的向量运算
   对应位置元素之间的运算 
   向量长度相等    c(1:3)/c(2:4) 相同位置的进行计算
   向量长度不相等  c(1:2)/c(2:5) 循环短的向量 
底数 ^ 指数 幂运算
exp(指数) 自然常数e为底的幂运算
log(x = 25,base = 5)  5为底25的对数
sqrt(数) 开平方
abs(数) 绝对值
sign(数) 符号函数(负数返回-1，正数返回1，0返回0)
round(数，指定位) 保留指定位小数，四舍五入
signif(数，指定位) 保留指定位有效数字
ceiling(数) 天花板(大于这个数的最小整数)
floor(数) 地板(小于这个数的最大整数)
逻辑运算
 ==
 !=
 >
 >=
 <
 <=
 2 %in% 2:5 2是否在2:5之中
 & 与
 | 或
 ! 非

```
### 向量相关函数

```
max()     最大值
cummax()  累积最大值
min()     最小值
cummin()  累积最小值
sum()     最大值
comsum()  累积最大值
prod()    乘积
cumprod() 累积乘积
mean()    均值
median()  中位数
sd()      标准差
var()     方差
rev()     向量逆转(逆序)
sort()    向量重排(排序)
rep()     向量重复 v1 <- rep(v2,times = 2,each = 3)
table()   向量元素频数统计
unique()  向量的取值水平(第一次出现的该值，唯一)
    unique(2,1:6)   --2,1,3,4,5,6
```

### 索引函数
which()  返回下标

```
v1 <- 1:9
which(v1 == 7)  --满足的下标都返回
which.max(v1)   --只返回第一个出现的最大最小值的下标
which.min(v1)
```
### 布尔运算
只允许两个向量

返回元素唯一值

```
intersect(向量1,向量2) 交集
setdiff(向量1，向量2)  差集
union(向量1，向量2)    并集
```
### 数据框和矩阵相关函数

```
dfs <- data.frame(
  a = 1:5,
  b = 3:7,
  d = letters[1:5]
)

```

#### 行列合并

```
df1 <- dfs[1:3,]
df2 <- dfs[3:5,]
```

#### 行合并
行数相同
`rbind(df1,df2)`
#### 列合并
列名称、列数相同
`cbind(df1,df2)`
#### 行列运算

```
colMeans(dfs[,1:2])
colSums(dfs[,1:2])
rowMeans(dfs[,1:2,])
rowSums(dfs[,1:2])
```

#### apply

```
apply(x,margin,function)
--margin 1 行, 2 列
--fuction 执行运算函数
apply(dfs[,1:2],2,sd)
apply(
     dfs[,1:2],
     2,
     function(dfs[,1:2]){sum(is.na(dfs[,1:2]))}
)
```
#### 对象结构信息

```
str(dfs)  --返回dfs是数据框，有5个观察样本obs，有3个变量variables，每个变量的名称，数据类型，
          --展示前几个值
 'data.frame':	5 obs. of  3 variables:
 $ a: int  1 2 3 4 5
 $ b: int  3 4 5 6 7
 $ d: chr  "a" "b" "c" "d" ...
summary(dfs)  --
         a           b          d            
 Min.   :1   Min.   :3   Length:5          
 1st Qu.:2   1st Qu.:4   Class :character  
 Median :3   Median :5   Mode  :character  
 Mean   :3   Mean   :5                     
 3rd Qu.:4   3rd Qu.:6                     
 Max.   :5   Max.   :7 
View(dfs)    --展示数据框
head(dfs,n = 2)  --返回数据框的前n行，默认6
tail(dfs,n = 2)  --返回数据框的末尾n行，默认6
```

#### 矩阵运算

```
m3 <- matrix(
             c(5,7,3,4),
             ncol = 2,
             byrow = T
)
m4 <- matrix(
             c(5,7,3,4,8,9)
             ncol = 3,
             byrow = T
)
t(m3)  --转至，行变成列，列变成行
m3 %*% m4  --矩阵的乘法 二元操作符
           --前面对象的列数和行数和后面对象相同
solve(矩阵) 求矩阵的例
solve(m3)      --m3 %*% x = E
solve(m3,m4)   --m3 %*% x =m4 信息方程组
det(m3)  --行列式
```
### 字符函数与分布相关函数
#### 连接成字符向量
```
paste(向量，collapse = "字符")
  --collapse 向量内每个元素之间的连接符
paste(向量1，向量2，seq = "字符")
  --sep 向量对应位置的连接,短的向量循环连接
paste0(向量1，向量2)   
  --无字符连接
paste(1:5,collapse = "+")
paste(letters[1:5],collapse = "-") 
paste(1:5,letters[1:8],sep = "~")   
paste0(1:5,letters[1:8])
```
#### 字符长度

```
nchar(字符/向量)
nchar(month.name)
```
#### 全部转大写
`toupper(字符/向量)`
#### 全部转小写
`tolower(字符/向量)`
#### 含有某个字符的元素的索引(下标)

```
grep("Ju",month.name)
--返回向量中含有''的元素的下标
```

#### 替换字符

```
gsub("e","000",month.name)
--将month.name中含e都把e都改成000
```

#### 随机分布函数

```
set.seed(整数)
  --随机数种子
sample(1:2,12,,replace = T)
  --随机抽样
  --从1:2中抽样，replace = T有返回
rnorm(10,mean = 1,sd = 2)
pnorm(1,mean = 1,sd = 2)
qnorm(0.5,mean = 1,sd = 2)
dnorm(1,mean = 1,sd = 2)
  r 生成随机数 
  p 累积概率，正态分布     --pnorm与qnorm互为反函数
  q 累积概率对应的分布点/数
  d 概率密度
rnorm(随机变量的取值/对应的概率密度,mean = 平均值,sd = 标准差,log = False)
plot(x = seq(-5,7,length = 1000),    --x轴长度范围
     y = dnorm(seq(-5,7,length = 1000),  --y轴
               mean = 1, 
               sd = 2),  
     type = "1",   --
     ylim = c(0,0.25))   --
abline(h = 0,   --h = 0， y等于0的一条水平线
       v = 1)   --v = 1， x等于1的一条竖直线
```

## 语法
### 循环结构
#### for循环

for(循环变量 in 循环空间-向量){循环语句}
```
for (x in c(-2,3,0,4)){
    print(x)
    y = abs(x)
    z = y^3
    print(z)
    print("--")
}
```

#### while循环

while(条件){循环语句}

在while前设置初始变量

注意能够使循环条件不被满足的时候,跳出死循环
```
v1 <- 1:5
i <- 1
while (i <= length(v1)){
    print(i)
    print(sunm(v1[1:i]))
    i = i + 1
    print(i)
    print("##")
}
```
 示例
```

df <- data.frame(c1 = 2:5,
                 c2 = 4:7,
                 c3 = -19:-16)

for (i in 1:nrow(df)) {
  print(sum(df[i, ]))
}


j = 1
while(j <= nrow(df)) {
  print(sum(df[j, ]))
  j = j + 1
}
```
#### next 跳出这次循环，进行下一次循环

#### break 结束循环


### 条件结构
#### if

```
if(条件){执行语句}
else if(条件){执行语句}
else(条件){执行语句}
a <- 7
if(a > 6) {
  print("a>6")
}
--------
a <- 5
if(a > 6) {
  print("a>6")
} 
else {
  print('a<=6')
}
--------
a <- 2
if(a > 6) {
  print("a>6")
} 
else if (a>3){
  print('a>3')
} 
else {
  print('a<=3')
}
--------
s = 40
if(s %% 2 == 0) {
  print("s是偶数。")
} 
else {
  print("s是奇数。")
}
ifelse(55 %% 2 == 0, "偶数", "奇数")
```

### 函数构建

```
funcation(参数名称、默认值){
    函数体
}
```
```
f1 <- function(aug1){
  res1 <- 1:aug1
  res2 <- prod(res1)
  return(res2)   --返回结果
}
f1(aug1 = 10)
f1(10)

--设定默认值,该参数未传参时,取值时自动使用
f2 <- function(aug1, aug2=4){
  res <- aug1 + aug2
  return(res)
}
f2(34)
f2(34, 5)
```
'''


## 数据操作—tidyverse包函数
- library(tidyverse)

组成包介绍 https://www.tidyverse.org/
### csv数据导入

- read.csv(file.choose())
- data.table::fread(file.choose())

```
rawdata <- read.table(file.choose(), header = T, sep = ",")
--默认 header = T,sep = ","
head(rawdata, n=4) --默认n = 6
tail(rawdata, n=10) --默认n = 6
rawdata[95:105,]
str(rawdata)
```
### csv数据导出
- write.csv()
- data.table::fwrite()

```
write.table(rawdata,
            "test.csv",
            sep = ",",
            row.names = F)
```
### 读取excel表
- excel_sheets(file.choose())
```
library(readxl)
data1 <- read_excel(file.choose())
```
### 批量读取数据

```
files <- list.files(".\\房地产PB\\")
files
paths <- paste(".\\房地产PB\\", files, sep = "")
paths
df <- list()
for (i in 1:length(paths)) {
  datai <- read_excel(paths[i])
  datai$object <- str_sub(files[i], start = 1, end = -6)
  df[[i]] <- datai
  print(i)
}
df_all <- bind_rows(df)
```

### dplyr

```
library(dplyr)
head(ToothGrowth)
str(ToothGrowth)
```


### 新增变量和变量重新赋值
toothgrowth2 <- mutate(ToothGrowth,
                       len = len^2,
                       nv = 1:nrow(ToothGrowth),
                       nv2 = ifelse(nv > median(nv), "H", "L"))
head(toothgrowth2)
# 筛选行（样本）
toothgrowth3 <- filter(toothgrowth2,
                       nv %in% 1:50,
                       nv2 == "H")
toothgrowth3

# 筛选列（变量）
toothgrowth4 <- select(toothgrowth3,
                       c(2,4))
head(toothgrowth4)

# 分组计算
summarise(ToothGrowth, len_max = max(len), len_min = min(len))
summarise(group_by(ToothGrowth, supp), len_max = max(len))
summarise(group_by(ToothGrowth, dose), len_max = max(len))
summarise(group_by(ToothGrowth, dose, supp), len_max = max(len))


# 管道操作符
library(magrittr)
ToothGrowth %>%
  mutate(nv = 1:nrow(ToothGrowth)) %>%
  filter(nv %in% 1:50) %>%
  select(1:2) %>%
  group_by(supp) %>%
  summarise(len_max = max(len)) %>%
  as.data.frame()


# 连接（合并）数据框
library(dplyr)
df1 <- data.frame(c1 = 2:5,
                  c2 = LETTERS[2:5])
df1
df2 <- data.frame(c3 = LETTERS[c(2:3,20:23)],
                  c4 = sample(1:100, size = 6))
df2

# 左连接
left_join(df1, df2, by = c('c2' = 'c3'))
df1 %>% left_join(df2, by = c('c2' = 'c3'))

# 右连接
df1 %>% right_join(df2, by = c('c2' = 'c3'))

# 全连接
df1 %>% full_join(df2, by = c('c2' = 'c3'))

# 内连接
df1 %>% inner_join(df2, by = c('c2' = 'c3'))



######################################################

# 列的分裂与合并
library(tidyr)
# 分裂
df3 <- data.frame(c5 = paste(letters[1:3], 1:3, sep = "-"),
                  c6 = paste(letters[1:3], 1:3, sep = "."),
                  c4 = c("B", "B", "B"),
                  c3 = c("H", "M", "L"))
df3
df4 <- df3 %>%
  separate(col = c5, sep = "-", into = c("c7", "c8"), remove = F) %>%
  separate(col = c6, sep = "\\.", into = c("c9", "c10"), remove = T)
df4


# 合并
df4 %>%
  unite(col = "c11", c("c7", "c8"), sep = "_", remove = F) %>%
  unite(col = "c12", c("c9", "c10"), sep = ".", remove = T) %>%
  unite(col = "c13", c("c4", "c3"), sep = "", remove = F)

#########

# 长宽数据转换
library(tidyr)
# 宽数据转长数据
set.seed(42)
df5 <- data.frame(time = rep(2011:2013, each=3),
                  area = rep(letters[1:3], times=3),
                  pop = sample(100:1000, 9),
                  den = round(rnorm(9, mean = 3, sd = 0.1), 2),
                  mj = sample(8:12, 9, replace = T))
df5

df6 <- df5 %>%
  pivot_longer(cols = -c(1:2),
               names_to = "varb",
               values_to = "value")
df6

# 长数据转宽数据
df6 %>%
  pivot_wider(names_from = c(area, varb),
              values_from = value)

######################################################

######################################################

# R语言绘图

# par函数
# 保存初始设定
inipar <- par(no.readonly = T)
# 恢复初始设定
par(inipar)

par(mfrow = c(2,3)) # mfcol
plot(1:30)
plot(1:30)
plot(1:30)

# 保存图片
png("pic.png")
# 绘图过程
plot(1:30)
# 关闭当前绘图设备
dev.off()


#########

# plot函数
plot(x = -1:6,
     y = 2*(-1:6),
     type = "o",
     family = "serif",
     xlim = c(-5,7),
     ylim = c(-5,14),
     ylab = "y----",
     xlab = "----x",
     main = "plot示例")

# lines函数
lines(x = 1:6, y = 2:7, col = "blue")
# abline函数
abline(a = 3, b = 2, col = "green")
abline(v = 0, h = 3)
# text函数
text(x = 3, y = 2.5, labels = "y=3")

#########
set.seed(432)
d0 <- data.frame(rs1 = sample(letters[1:4], 100, replace = T),
                 rs2 = sample(LETTERS[21:22], 100, replace = T))

# barplot函数
barplot(1:5, names.arg = letters[1:5])
barplot(table(d0$rs1), main = "barplot")

# boxplot函数
boxplot(ToothGrowth$len)
boxplot(len ~ supp, data = ToothGrowth)


# hist函数
hist(rnorm(1000), breaks = 15)

# 直方图叠加密度曲线
set.seed(10)
d1 <- rnorm(1000)
hist(d1, breaks = 100, freq = F, main = "Histogram")
lines(density(d1), col = "blue", lwd=2)
d2 <- seq(min(d1), max(d1), length=10000)
lines(d2, dnorm(d2), col = "red", lwd=2)

# 马赛克图
table(d0$rs1)
table(d0$rs2)
table(d0$rs1, d0$rs2)
mosaicplot(table(d0$rs1, d0$rs2))

######################################################
######################################################


# ggplot2包
library(tidyverse)
set.seed(432)
d3 <- data.frame(
  ind = 1:100,
  rn = rnorm(100),
  rt = rt(100, df=5),
  rs1 = sample(letters[1:3], 100, replace = T),
  rs2 = sample(LETTERS[21:22], 100, replace = T)
)

# 点图
ggplot() +
  geom_point(data = d3,
             mapping = aes(x=rn, y=rt, fill=rs2),
             shape = 21,
             size = 5)

# 线图
ggplot(d3, aes(x=ind, y=rn)) +
  geom_line(size=1.2)

# 条形图
d3 %>%
  ggplot(aes(x=rs1)) +
  geom_bar(fill = "white", color = "black")

d3 %>%
  group_by(rs1) %>%
  summarise(mean_rn = mean(rn)) %>%
  ggplot(aes(x=rs1, y=mean_rn)) +
  geom_col(fill="grey", colour="black", width = 0.5)

d3 %>%
  group_by(rs1, rs2) %>%
  summarise(m = median(rn)) %>%
  ggplot(aes(x = rs1, y = m, fill = rs2)) +
  geom_col(position = "dodge")

# 箱线图
ggplot(ToothGrowth, aes(y=len)) +
  geom_boxplot()

ggplot(ToothGrowth, aes(x=supp, y=len)) +
  geom_boxplot()

ggplot(d3, aes(x=rs1, y=rn, fill=rs2)) +
  geom_boxplot()

# 直方图
ggplot(d3, aes(x=rn, fill=rs2)) +
  geom_histogram(bins = 20, alpha=0.1, colour="black")

# 密度曲线
ggplot(d3, aes(x=rn, fill=rs2)) +
  geom_density(alpha=0.1) +
  labs(x = "aa", y = "bb", title = "density") +
  theme(plot.title = element_text(hjust = 0.5))

######################################################
######################################################

# 因子
set.seed(42)
l3 <-sample(letters[1:3], 10, replace = T)
l3
as.factor(l3)
factor(l3)
# factor()


# 描述性统计
set.seed(432)
d3 <- data.frame(
  ind = 1:1000,
  rn = rnorm(1000),
  rn2 = rnorm(1000, mean = 2, sd = 3),
  rt = rt(1000, df=5),
  rs1 = as.factor(sample(letters[1:3], 1000, replace = T)),
  rs2 = as.factor(sample(LETTERS[21:22], 1000, replace = T))
)

# 描述性统计结果
summary(d3)
library(skimr)
skim(d3)

# 偏度
e1071::skewness(d3$rn)
# 峰度
e1071::kurtosis(d3$rn2)

# 相关系数
cor(d3$rn, d3$rt)
cor(d3[,2:4])
# 相关性检验
cor.test(d3$rn, d3$rt)
library(psych)
corr.test(d3[,1:3])

# 列联表
table(d3$rs1)
prop.table(table(d3$rs1))


######################################################
# 假设检验

# 正态分布检验
# shapiro.test()
library(rstatix)
head(ToothGrowth)
# 单一变量检验
ToothGrowth %>%
  shapiro_test(len)
# 分组检验
ToothGrowth %>%
  group_by(dose) %>%
  shapiro_test(len)

###########################

# 方差齐性检验
# 两组检验
var.test(len ~ supp, data = ToothGrowth)
# 两组及以上的检验
bartlett.test(len ~ dose, data = ToothGrowth)

##########################

# 均值检验
# t检验
t.test(ToothGrowth$len,
       mu = 18)

t.test(len ~ supp,
       data = ToothGrowth,
       var.equal = T)

# 方差分析
ToothGrowth$dose <- as.factor(ToothGrowth$dose)
aovfit <- aov(len ~ dose, data = ToothGrowth)
aovfit
summary(aovfit)

##########################

# 非参数检验
# 差异检验：Wilcoxon秩和检验(Mann-Whitney U检验)，适用于两组数据
wilcox.test(len ~ supp, data = ToothGrowth, exact = F)
# 差异检验：Kruskal-Wallis检验，适用于两组及以上的数据
kruskal.test(len ~ dose, data = ToothGrowth)

# 方差齐性非参数检验
fligner.test(len ~ dose, data = ToothGrowth)

######################################################
######################################################


# 普通线性回归
head(mtcars, n = 10)
colnames(mtcars)
rownames(mtcars)
str(mtcars)

# 了解变量，并且把变量处理好
mtcars$vs <- factor(mtcars$vs)
mtcars$am <- factor(mtcars$am)

# 建模
lmfit <- lm(mpg ~ hp + wt + vs + am, data = mtcars)
lmfit
# 模型结果汇总
lmsum <- summary(lmfit)
lmsum
# 模型系数
coef(lmfit)
coef(lmsum)
# 模型残差
resid(lmfit)
# 提取结果成分
str(lmsum)


# 格式化输出
library(stargazer)
# 在console显示表格，输出到本地
stargazer(lmfit, type="text", out="lm.htm")

# 模型诊断
par(mfrow=c(2,2))
plot(lmfit)
library(lmtest)
# 异方差检验
bptest(lmfit)
# 序列相关检验，主要针对时序回归
dwtest(lmfit)
# 修正
library(sandwich)
coeftest(lmfit, vcovHC(lmfit))
coeftest(lmfit, vcovHAC(lmfit))
# 多重共线性检验
library(car)
vif(lmfit)

# 预测
pred <- predict(lmfit, newdata = mtcars)
# 图示
plot(mtcars$mpg, pred)
abline(a=0, b=1, col="red")

# 性能评估
library(caret)
defaultSummary(
  data.frame(obs = mtcars$mpg,
             pred = pred)
)

######################################################
######################################################

# logistic回归
# 读取数据
bcdata <- read.csv(file.choose())

# 查看数据概况
library(skimr)
skim(bcdata)

# 删除含有缺失值的样本
bcdata <- na.omit(bcdata)

# 变量类型修正
bcdata$class <- factor(bcdata$class)

# 查看分类型变量编码
contrasts(bcdata$class)
# 查看分类型变量频数分布
table(bcdata$class)

# logistic回归建模
glmfit <- glm(class ~ .-ID, data = bcdata, family = binomial())
glmfit
summary(glmfit)

# 格式化输出
library(stargazer)
# 在console显示表格，输出到本地
stargazer(glmfit, type="text", out="logit.htm")

# 预测概率
predprob <- predict(glmfit, newdata = bcdata, type = "response")
# 有些模型的predict输出的概率是矩阵，注意识别。

# ROC曲线
library(pROC)
rocs <- roc(response = bcdata$class, # 实际类别
            predictor = predprob) # 预测概率
# 注意Setting direction

# ROC曲线
plot(
  rocs, # roc对象
  print.auc = TRUE, # 打印AUC值
  auc.polygon = TRUE, # 显示AUC区域
  grid = T, # 网格线
  max.auc.polygon = T, # 显示AUC=1的区域
  auc.polygon.col = "skyblue", # AUC区域填充色
  print.thres = T, # 打印最佳临界点
  legacy.axes = T # 横轴显示为1-specificity
)

# 约登法则
bestp <- rocs$thresholds[
  which.max(rocs$sensitivities + rocs$specificities - 1)
]
bestp

# 预测分类
predlab <- as.factor(
  ifelse(predprob > bestp, "malignant", "benign")
)

# 混淆矩阵
library(caret)
confusionMatrix(data = predlab, # 预测类别
                reference = bcdata$class, # 实际类别
                positive = "malignant",
                mode = "everything")



'''