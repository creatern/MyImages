## T-SQL简介

## SSMG 默认输入语言

从开发者的角度来看这个问题，这其实是跟main窗体有关系的，你可以试试用鼠标点击“对象资源管理器”（也可能是菜单栏）将焦点切换到这个窗体，然后切换输入法到英文，这个时候，你再切换标签页或者来回切换界面就不会再默认为中文了

## 数据库

### CREATE DATABASE 数据库创建

```sql
CREATE DATABASE TESTDB001
ON
(
 NAME = DATABASETEST001_1_DAT
,FILENAME = 'G:\DataBaseSQLserver\TEST00101.MDF'
,SIZE = 10 MB
,MAXSIZE = 50 MB
,FILEGROWTH = 5 %
),
(
 NAME = DATABASETEST001_2_DAT
,FILENAME = 'G:\DataBaseSQLserver\TEST00102.NDF'
,SIZE = 10 MB
,MAXSIZE = 50 MB
,FILEGROWTH = 5 %
),
FILEGROUP TEST001_GRP2
(
 NAME = TEST001_GRP2_F1_DAT
,FILENAME = 'G:\DataBaseTest001\TEST001_GRP2_F1_DAT'
,SIZE = 10 MB
,MAXSIZE = 50 MB
,FILEGROWTH = 10 MB
)
LOG ON
(
 NAME = TEST001_LOG
,FILENAME = 'G:\DataBaseSQLserver\TEST001LOG.LDF'
,SIZE = 5 MB
,MAXSIZE = 25 MB
,FILEGROWTH = 5 MB
);
```

```sql
CREATE DATABASE MydbTest01
ON PRIMARY
(
 NAME = 'mydb01_data'
,FILENAME = 'D:\mydb01\mydb_data.mdf'
,SIZE = 50 MB
,MAXSIZE = 100 MB
,FILEGROWTH = 10 %
),
FILEGROUP mydb01_data_g1 
DEFAULT
(
 NAME = 'mydb01_data_g1_f1'
,FILENAME = 'C:\mydb01\mydb01_data_g1_f1.ndf'
,SIZE = 20 MB
,MAXSIZE = 50 MB
,FILEGROWTH = 10 %
),
(
 NAME = 'mydb01_data_g1_f2'
,FILENAME = 'C:\mydb01\mydb01_data_g1_f2.ndf'
,SIZE = 10 MB
,MAXSIZE = 30 MB
,FILEGROWTH = 10 MB
)
/* FILESTREAN 需要特殊权限
,
FILEGROUP mydb01_data_g2_blob
CONTAINS FILESTREAM 
(
 NAME = 'mydb01_data_g2_f1_blob'
,FILENAME = 'D:\mydb01_data_g2_f1_blob.ndf'
/*FILESTREAM 的BLOB文件不能有以下属性的设置
,SIZE = 20 MB
,MAXSIZE = 50 MB
,FILEGROWTH = 20 %
*/
)
*/
LOG ON
(
 NAME = 'mydb01_log'
,FILENAME = 'D:\mydb01\mydb01_log.ldf'
,SIZE = 10 MB
,MAXSIZE = 30 MB
,FILEGROWTH = 10 %
)
,
/*事物日志文件不能归于文件组
FILEGROUP mydb01_log_g1
(
 NAME = 'mydb01_log_g1_f1'
,FILENAME = 'D:\mydb01\mydb01_log_g1_f1'
,SIZE = 5 MB
,MAXSIZE = 25 MB
,FILEGROWTH = 5 %
)

*/
--COLLATE 
--FOR ATTACH_REBUILD_LOG
```

#### PRIMARY

#### FILEGROUP

#### CONTAINS FILESTREAM

#### COLLATE

- 定义应用于数据库或表列的排序规则或字符串表达式时的排序规则强制转换操作。
- 排序规则名称可以是 Windows 排序规则名称或 SQL 排序规则名称。
  - 如果在创建数据库时未指定，则会为数据库分配 SQL Server 实例的默认排序规则。
  - 如果在创建表列时未指定该列，则该列将分配给数据库的默认排序规则。

```sql
COLLATE { <collation_name> | database_default }
<collation_name> :: =
    { Windows_collation_name } | { SQL_collation_name }
```

**参数**

- 要应用于collation_name表达式、列定义或数据库定义的排序规则的名称。
  - collation_name只能是指定的 Windows_collation_name 或 SQL_collation_name。
  - collation_name 必须是文本值。
  - collation_name 不能由变量或表达式表示。
- Windows_collation_name 是 Windows 排序规则名称的排序规则名称。
- SQL_collation_name 是 SQL Server 排序规则名称的排序规则名称。
- database_default COLLATE 子句继承当前数据库的排序规则。

**解释**

可以在不同级别指定“复制”子句。 其中包括：

##### 1.创建或修改数据库

- CREATE DATABASE 或者 ALTER DATABASE，可以使用语句的 COLLATE 子句指定数据库的默认排序规则。
  - 使用 SQL 服务器管理工作室创建数据库时，还可以指定排序规则。
  - 如果未指定排序规则，则会在数据库中指定 SQL Server 实例的默认排序规则。

- 注意
  - 仅当 COLLATE 子句中将 nchar、nvarchar 和 ntext 数据类型应用于列级和表达式级数据时，专用于 Unicode 的 Windows 排序规则才可用。
  - 不能在 COLLATE 子句中定义或更改数据库或服务器实例的排序规则。

##### 2.创建或修改表列

- CREATE TABLE 或者 ALTER TABLE，可以使用 语句的 COLLATE 子句为字符类型的每一列指定排序规则。
  - 使用 SQL 服务器管理工作室创建表时，还可以指定排序规则。
  - 如果未指定排序规则，则列指定数据库的默认排序规则。
- 还可以使用 COLLATE 子句中的选项指定临时表中的列使用连接当前用户数据库的默认排序规则，而不是 tempdb。database_default

##### 3.强制转换表达式排序规则

- 可以使用 COLLATE 子句将字符表达式应用于特定的排序规则。
  - 字符文本和变量指定当前数据库的默认排序规则。
  - 列引用指定列的默认排序规则。

##### 标识符的排序规则取决于定义标识符的级别

- 实例级对象的标识符（如登录名和数据库名称）指定实例的默认排序规则。
- 数据库中对象的标识符（如表、视图和列名称）指定数据库的默认排序规则。
  - 例如，如果创建两个表，它们的名称仅在大小写上不同，
  - 则可以在具有区分大小写的排序规则的数据库中创建它们，
  - 但不能在具有不区分大小写的排序规则的数据库中创建它们。
  - 有关详细信息，请参阅数据库标识符。

- 当连接上下文与一个数据库关联时，可以创建变量、GOTO 标签、临时存储过程和临时表，并在将上下文切换到另一个数据库时引用它们。
  - 变量、GOTO 标签、临时存储过程和临时表标识符遵循服务器实例的默认排序规则。

##### COLLATE 子句只能应用于字符、矢量、文本、数字、数字和 ntext 数据类型

- COLLATE 使用 collate_name 引用应用于表达式、列定义或数据库定义的 SQL Server 或 Windows 排序规则的名称。
  - collation_name 只能指定指定的 Windows_collation_name 或 SQL_collation_name，
  - 并且参数必须包含文本值。
  - collation_name 不能由变量或表达式表示。

- 排序规则通常由排序规则名称标识。
  - 但是，在设置过程中，这是一个例外。
  - 在安装过程中，为 Windows 排序规则指定根排序规则说明符（排序规则区域设置），然后指定区分大小写和重音的排序选项。

##### 获取 Windows 和 SQL Server 排序规则的所有有效排序规则名称的列表

通过执行系统函数 fn_helpcollations，可以获取 Windows 和 SQL Server 排序规则的所有有效排序规则名称的列表。

```sql
SELECT name
     , description
FROM fn_helpcollations();
```

##### SQL Server 只能支持正在运行的操作系统支持的代码页

- 执行依赖于排序规则的操作时，引用对象使用的 SQL Server 排序规则必须使用计算机上运行的操作系统支持的代码页。
- 此类操作包括：
  - 创建或修改数据库时，请指定数据库的默认排序规则。
  - 创建或修改表时指定列的排序规则。
  - 还原或连接数据库时，操作系统必须支持数据库的默认排序规则以及数据库中任何类型的字符、varchar 和文本类型的列或参数的排序规则。
- 注意

- 字符和 varchar 数据类型支持代码页转换，
  - 但文本数据类型不支持代码页转换。
  - 在代码页转换期间，不会报告数据丢失。
- 如果指定的排序规则或引用的对象使用的排序规则使用 Windows 不支持的代码页，
  - SQL Server 将显示错误。

- 示例
A. 在选择时指定排序规则
下面的示例创建一个简单的表并插入四行。 下面介绍如何在从表中选择数据时应用两个排序规则，以便 以不同的方式存储 。

```sql
CREATE TABLE Locations
(Place varchar(15) NOT NULL);
GO
INSERT Locations(Place) 
VALUES ('Chiapas'),('Colima'), ('Cinco Rios'), ('California');
GO
--Apply an typical collation
SELECT Place FROM Locations
ORDER BY Place
COLLATE Latin1_General_CS_AS_KS_WS ASC;
GO
-- Apply a Spanish collation
SELECT Place 
FROM Locations
ORDER BY Place
COLLATE Traditional_Spanish_ci_ai ASC;
GO
```

下面是第一个查询的结果：

```
Place
-------------
California
Chiapas
Cinco Rios
Colima
```

下面是第二个查询的结果：

```
Place
-------------
California
Cinco Rios
Colima
Chiapas
```

#### FOR ATTACH

### ALTER 修改数据库

#### 改数据库名

```sql
ALTER DATABASE dbname
MODIFY NAME = newname;
```

```sql
EXEC sp_renamedb 'oldname','newname';
```

#### 添加文件

##### 1.添加次要数据文件

```sql
ALTER DATABASE NEWDBSALES
ADD FILE
(
 NAME = Sales_dat2
,FILENAME = 'G:\DataBaseSQLserver\Sales_dat2.ndf'
,SIZE = 10 MB
,MAXSIZE = 50 MB
,FILEGROWTH = 5 %
)
```

##### 2.添加日志文件

```sql
ALTER DATABASE Mydb
ADD LOG FILE
(
 NAME = mydb_log_add
,FILENAME = 'G:\databasesqlserver\mydb_log_add.ldf'
,SIZE = 5 MB
,MAXSIZE = 25 MB
,FILEGROWTH = 5 MB
)
```

##### 3.添加文件组  

```sql
ALTER DATABASE Mydb
ADD FILEGROUP mydb_add_group
GO
ALTER DATABASE Mydb
ADD FILE 
(
 NAME = mydb_add_group_f1_dat
,FILENAME = 'G:\databasesqlserver\mydb_add_group_f1_dat.ndf'
,SIZE = 25 MB
,MAXSIZE = 50 MB
,FILEGROWTH = 5 %
),
(
 NAME = mydb_add_group_f2_dat
,FILENAME = 'g:\databasesqlserver\mydb_add_group_f2_dat.ndf'
,SIZE = 20 MB
,MAXSIZE = 50 MB
,FILEGROWTH = 5 MB
)
TO FILEGROUP mydb_add_group
GO
ALTER DATABASE Mydb
MODIFY FILEGROUP mydb_add_group DEFAULT
GO
```

#### 修改文件

#### 删除文件

### 删除数据库

```sql
DROP DATABASE dbname1[,dbname2]
```

## GO 类似于Oracle的分号

- 当多个语句在单个批处理中发送时，GO 关键字分隔语句。 如果批处理中只有一个语句，则 GO 是可选的。

## 数据类型

| 数据类型     | 具体                           |
| :---------- | :----------------------------- |
| 字符         | (n)CHAR、(n)VARCHAR、(n)TEXT   |
| 二级制       | BINARY、VARBINARY、IMAGE       |
| 日期时间     | DATETIME、SMALLDATETIME        |
| 逻辑         | BIT                            |
| 数字数据     | INT、SMALLINT、TINYINT、BIGINT |
| 其他         |                                |
| 用户数据类型 |                                |

## 表

### CREATE TABLE 创建表

#### 创建简单表

```sql
USE Mydb  --选择使用的数据库
CREATE TABLE employee
(
 employee_id CHAR(4) NOT NULL
,employee_name VARCHAR(20) NOT NULL
,sex CHAR(2) NOT NULL
,birth_date DATE NOT NULL
,address VARCHAR(50)
,telephone VARCHAR(12)
,wages MONEY
,department_id CHAR(4)
,resume TEXT
)
```

#### 为表指定文件组

```sql
CREATE TABLE Mydb.dbo.supplier   --数据库Mydb 所有者dbo 表 supplier
(
 supplier_id CHAR(6) NOT NULL
,supplier_name VARCHAR(50) NOT NULL
,linkman_name VARCHAR(20)
,address VARCHAR(50)
,telephone VARCHAR(12) NOT NULL
)
ON [PRIMARY]   --PRIMARY文件组
```

#### 对计算列使用表达式

```sql
CREATE TABLE salary
(
 姓名 VARCHAR(10)
,基本工资 MONEY
,奖金 MONEY
,总计 AS 基本工资 + 奖金   -- 计算列
)
```

#### 定义表自动获取信息

函数：

- GETDATE() 从 SQL Server 返回当前的时间和日期。
- USER_NAME()

```sql
CREATE TABLE autoser
(
 编号 INT IDENTITY(1,1)NOT NULL
,用户代码 VARCHAR(18)
,登入时间 AS GETDATE()
,用户名 AS USER_NAME()
)
```

#### 创建临时表

```sql
CREATE TABLE #students
(
 学号 VARCHAR(8)
,姓名 VARCHAR(10)
,性别 VARCHAR(2)
,班级 VARCHAR(10)
)
```

### ALTER TABLE 修改表

1. ADD COLUMN

```sql
ALTER TABLE employees
ADD COLUMN employee_id INT NOT NULL;
```

2. ALTER COLUMN

3. DROP COLUMN

### DROP TABLE 删除表

```sql
USE Mydb
DROP TABLE employees
```

```sql
DROP TABLE Mydb.dbo.employees  --数据库.架构名.表名
```

### 对表的操作 CRUD

#### INSERT 插入

```sql
USE Mydb
GO
INSERT INTO supplier
VALUES('R001','华科电子有限公司','施宾彬','朝阳路56号','2636565')

(1 行受影响)
//即插入成功
```

##### 隐式

如上

##### 显式

```sql
INSERT INTO Mydb.dbo.supplier(supplier_id,supplier_name,linkman_name,address,telephone)
VALUES('R00015','华科电子有限公司','施宾彬','朝阳路56号','2636565')
```

- 可以插入值少于列的个数(没有非空约束的)

```sql
INSERT INTO Mydb.dbo.supplier(supplier_id,supplier_name,telephone)
VALUES('R00016','晨光电子实业公司','4561681')
```

- 可以显示的打乱输入顺序

```sql
INSERT INTO Mydb.dbo.supplier (supplier_name,telephone,supplier_id)
VALUES('晨光电子科技实业公司','4561681','R00016')
```

##### 将数据装载到带有标识符的表

```sql
CREATE TABLE customer2
(
 customer_id BIGINT NOT NULL IDENTITY(0,1)
,customer_name VARCHAR(50) NOT NULL
,linkman_name VARCHAR(20)
,address VARCHAR(50)
,telephone CHAR(12) NOT NULL
)
GO
INSERT INTO customer2
VALUES('东方体育用品公司','刘平','东方市中山路25号','75368025')

INSERT INTO customer2
(customer_name,linkman_name,address,telephone)
VALUES('北京泛亚实业公司','张卫民','长岭市五一路号','68510231')

SET IDENTITY_INSERT Mydb.dbo.customer2 ON
INSERT INTO customer2
(customer_id,customer_name,linkman_name,address,telephone)
VALUES ('2','洞庭强华电器公司','马东','滨海市洞庭大道号','76053331')
```

###### IDENTITY

- 要创建一个可在多个表中使用的自动递增数字或者可以从应用程序中调用而不引用任何表的自动递增数字

```sql
IDENTITY (data_type [ , seed , increment ] ) AS column_name  

*参数
data_type
标识列的数据类型。 
标识列的有效数据类型可以是任何整数数据类型类别的数据类型（bit 数据类型除外），也可以是 decimal 数据类型。

seed
要分配给表中第一行的整数值。 
为每一个后续行分配下一个标识值，该值等于上一个 IDENTITY 值加上 increment 值。 
如果既没有指定 seed，也没有指定 increment，那么它们都默认为 1。

increment
要加到表中后续行的 seed 值上的整数值。

column_name
将插入到新表中的列的名称。

*返回类型
返回与 data_type 相同的数据类型。
```

- 只用于在带有 INTO 子句的 SELECT 语句中将标识列插入到新表中。
- 尽管类似，但是 IDENTITY 函数不是与 CREATE TABLE 和 ALTER TABLE 一起使用的 IDENTITY 属性。

```sql
以下示例将来自 AdventureWorks2019 数据库的 Contact 表的所有行都插入到名为 NewContact 的新表中。
使用 IDENTITY 函数在 NewContact 表中从 100 而不是 1 开始编标识号

USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.NewContact', N'U') IS NOT NULL  
    DROP TABLE Person.NewContact;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
SELECT  IDENTITY(smallint, 100, 1) AS ContactNum,  
        FirstName AS First,  
        LastName AS Last  
INTO Person.NewContact  
FROM Person.Person;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
SELECT ContactNum, First, Last FROM Person.NewContact;  
GO  
```

###### SET IDENTITY_INSERT

```sql
SET IDENTITY_INSERT [ [ database_name . ] schema_name . ] table_name { ON | OFF }  

*参数
database_name
是指定表所在的数据库的名称。

schema_name
是表所属的模式的名称。

table_name
是具有标识列的表的名称。

*
在任何时候，会话中只有一个表可以将 IDENTITY_INSERT 属性设置为 ON。
如果一个表已经将此属性设置为 ON，并且为另一个表发出了 SET IDENTITY_INSERT ON 语句，
则 SQL Server 将返回一条错误消息，
指出 SET IDENTITY_INSERT 已为 ON，并报告它为其设置为 ON 的表。

如果插入的值大于表的当前标识值，SQL Server 会自动使用新插入的值作为当前标识值。

SET IDENTITY_INSERT 的设置是在执行或运行时设置的，而不是在解析时设置的。

*权限
用户必须拥有该表或对该表具有 ALTER 权限。
```

```sql
以下示例创建了一个带有标识列的表，
并显示了如何使用该SET IDENTITY_INSERT设置来填补由DELETE语句引起的标识值的空白。

USE AdventureWorks2012;  
GO  
-- Create tool table.  
CREATE TABLE dbo.Tool(  
   ID INT IDENTITY NOT NULL PRIMARY KEY,   
   Name VARCHAR(40) NOT NULL  
);  
GO  
-- Inserting values into products table.  
INSERT INTO dbo.Tool(Name)   
VALUES ('Screwdriver')  
        , ('Hammer')  
        , ('Saw')  
        , ('Shovel');  
GO  
  
-- Create a gap in the identity values.  
DELETE dbo.Tool  
WHERE Name = 'Saw';  
GO  
  
SELECT *   
FROM dbo.Tool;  
GO  
  
-- Try to insert an explicit ID value of 3;  
-- should return an error:
-- An explicit value for the identity column in table 'AdventureWorks2012.dbo.Tool' can only be specified when a column list is used and IDENTITY_INSERT is ON.
INSERT INTO dbo.Tool (ID, Name) VALUES (3, 'Garden shovel');  
GO  
-- SET IDENTITY_INSERT to ON.  
SET IDENTITY_INSERT dbo.Tool ON;  
GO  
  
-- Try to insert an explicit ID value of 3.  
INSERT INTO dbo.Tool (ID, Name) VALUES (3, 'Garden shovel');  
GO  
  
SELECT *   
FROM dbo.Tool;  
GO  
-- Drop products table.  
DROP TABLE dbo.Tool;  
GO
```

##### 使用SELECT和EXECUTE选项装载数据

```sql
CREATE TABLE Mydb.dbo.newcustomer
(
 customerName VARCHAR(50) NOT NULL
,linkmanName VARCHAR(20)
)
```

```sql
INSERT INTO newcustomer
SELECT customer_name
      ,linkman_name
FROM customer2
```

```sql
CREATE PROCEDURE MySp_Customer
AS
SELECT customer_name
      ,linkman_name
FROM customer2
```

```sql
INSERT INTO newcustomer
EXECUTE MySp_Customer

INSERT INTO newcustomer
EXECUTE ('SELECT customer_name,linkman_name FROM customer2')
```
#### SQL Server中的标识列

```sql
一、标识列的定义以及特点
SQL Server中的标识列又称标识符列,习惯上又叫自增列。
该种列具有以下三种特点：
1、列的数据类型为不带小数的数值类型
2、在进行插入(Insert)操作时，该列的值是由系统按一定规律生成,不允许空值
3、列值不重复，具有标识表中每一行的作用,每个表只能有一个标识列。
由于以上特点，使得标识列在数据库的设计中得到广泛的使用。
二、标识列的组成
创建一个标识列，通常要指定三个内容:
1、类型（type）
在SQL Server 2000中，标识列类型必须是数值类型，如下：
decimal、int、numeric、smallint、bigint 、tinyint 
其中要注意的是，当选择decimal和numeric时，小数位数必须为零
另外还要注意每种数据类型所有表示的数值范围
2、种子(seed)
是指派给表中第一行的值,默认为1
3、递增量(increment)
相邻两个标识值之间的增量，默认为1。
三、标识列的创建与修改
标识列的创建与修改，通常在企业管理器和用Transact-SQL语句都可实现，使用企业管理管理器比较简单，请参考SQL Server的联机帮助，这
里只讨论使用Transact-SQL的方法
1、创建表时指定标识列
标识列可用 IDENTITY 属性建立，因此在SQL Server中，又称标识列为具有IDENTITY属性的列或IDENTITY列。
下面的例子创建一个包含名为ID,类型为int,种子为1，递增量为1的标识列
CREATE TABLE T_test
(ID int IDENTITY(1,1),
Name varchar(50)
)
2、在现有表中添加标识列
下面的例子向表T_test中添加一个名为ID,类型为int,种子为1，递增量为1的标识列
--创建表
CREATE TABLE T_test
(Name varchar(50)
)
--插入数据
INSERT T_test(Name) VALUES('张三')
--增加标识列
ALTER TABLE T_test
ADD ID int IDENTITY(1,1)
3、判段一个表是否具有标识列
可以使用 OBJECTPROPERTY 函数确定一个表是否具有 IDENTITY（标识）列,用法:
Select OBJECTPROPERTY(OBJECT_ID('表名'),'TableHasIdentity')
如果有，则返回1,否则返回0
4、判断某列是否是标识列
可使用 COLUMNPROPERTY 函数确定 某列是否具有IDENTITY 属性,用法
SELECT COLUMNPROPERTY( OBJECT_ID('表名'),'列名','IsIdentity')
如果该列为标识列，则返回1,否则返回0
4、查询某表标识列的列名
SQL Server中没有现成的函数实现此功能，实现的SQL语句如下
SELECT COLUMN_NAME FROM INFORMATION_SCHEMA.columns
WHERE TABLE_NAME='表名' AND COLUMNPROPERTY( 
OBJECT_ID('表名'),COLUMN_NAME,'IsIdentity')=1
5、标识列的引用
如果在SQL语句中引用标识列，可用关键字IDENTITYCOL代替
例如，若要查询上例中ID等于1的行，
以下两条查询语句是等价的
SELECT * FROM T_test WHERE IDENTITYCOL=1
SELECT * FROM T_test WHERE ID=1
6、获取标识列的种子值
可使用函数IDENT_SEED,用法：
SELECT IDENT_SEED ('表名')
7、获取标识列的递增量
可使用函数IDENT_INCR ,用法：
SELECT IDENT_INCR('表名')
8、获取指定表中最后生成的标识值
可使用函数IDENT_CURRENT，用法:
SELECT IDENT_CURRENT('表名') 
注意事项：当包含标识列的表刚刚创建,为经过任何插入操作时，使用IDENT_CURRENT函数得到的值为标识列的种子值，这一点在开发数据库应用程序的时候尤其应该注意。


利用 SQL 语句修改出一个标识列使用sql语句创建修改SQL <wbr>Server标识列(即自动增长列)
--将数据复制到临时表
select * into #aclist from aclist

--删除数据表
drop table aclist

--创建数据表（并设置标识列）
create table aclist(id int identity(1,1),[date] datetime,version nvarchar(6),[class] nvarchar(10),actitle nvarchar(50),acdetail nvarchar(max),author nvarchar(50))

--设置标识列允许插入
set identity_insert aclist on

--将数据从临时表转移过来
insert into aclist(id,[date],version,[class],actitle,acdetail,author)
select id,[date],version,[class],actitle,acdetail,author from #aclist

--关闭标识列插入
set identity_insert aclist off

--强制设置标识列的起始值:
DBCC CHECKIDENT (表名, RESEED, 1) --强制使标识值从1开始.

 

----------------

修改原有字段中，不删除表，直接修改表中字段，删除数据后，处理。


---创建没有自动增长的数据表
CREATE TABLE [tbMessage](
[id] [decimal](18, 0),
[msg_content] [varchar](max) NULL
) ON [PRIMARY]

GO
----插入测试数据
insert into [tbMessage]([id],[msg_content])
values(20,'你知道吗？')

insert into [tbMessage]([id],[msg_content])
values(21,'你知道吗？201')
go
--查看数据
---select * from tbMessage

--插入临时表
select * into #tbMessage from [tbMessage]
go
--删除表数据
delete [tbMessage]
go

--删除字段ID
alter table [tbMessage] drop column [ID]
---增加ID自动增长字段
alter table [tbMessage] add [id] int identity(1,1)

set identity_insert [tbMessage] on

--将数据从临时表转移过来

insert into [tbMessage]([msg_content]
,[id])
select [msg_content]
,[id] from #tbMessage

--关闭标识列插入
set identity_insert [tbMessage] off

---删除临时表
drop table #tbMessage
--------------------------------------------------
/*
drop table tbMessage
---------------检测自动增长字段是否正常----------
----获取种子数据
SELECT IDENT_SEED ('[tbMessage]')

---drop table tbMessage
---插入二条数据

insert into [tbMessage]([msg_content])
values('你知道吗20111')

insert into [tbMessage]([msg_content])
values('你知道吗20112')


---查看这个ID是否,正常增长
select * from tbMessage
```

#### UPDATE 修改数据

##### 简单的UPDATE语句

```
UPDATE customer2
SET linkman_name = '佚名'
   ,address = NULL
   ,telephone = ''
   
--更新列的所有数据
```

##### 在UPDATE语句中使用WHERE子句

```
UPDATE customer2
SET telephone = '0731-' + telephone
WHERE LEN(telephone) = 8
--只更新满足指定的记录
```

##### 在UPDATE语句中使用来自另一个表的数据

创建sell_order表

```
USE Mydb
GO
CREATE TABLE sell_order
(
 order_id1 CHAR(6)
,goods_id CHAR(6)
,employee_id CHAR(4)
,customer_id CHAR(4)
,transporter_id CHAR(4)
,order_num FLOAT
,discount FLOAT
,order_date DATETIME
,send_date DATETIME
,arrival_date DATETIME
,cost MONEY
)
```

创建goods表

```
USE Mydb
GO
CREATE TABLE goods
(
 goods_id CHAR(6)
,goods_name VARCHAR(50)
,classification_id CHAR(6)
,unit_price MONEY
,stock_quantity FLOAT
,order_quantity FLOAT
)
```

UPDATE

```
UPDATE sell_order
SET cost = s.order_num * g.unit_price * (1 - s.discount)
FROM sell_order s,goods g
WHERE s.goods_id = g.goods_id
```

##### 在UPDATE语句中使用SELECT ... TOP语句

```
对销售最差的10件商品降价

UPDATE goods
SET unit_price = unit_price * 0.9
FROM goods
    , 
    (
     SELECT TOP 10 goods_id
           ,SUM(order_num) AS total_num
     FROM sell_order
  GROUP BY goods_id
  ORDER BY total_num ASC
 ) total_sum
WHERE goods.goods_id = total_sum.goods_id
```

#### DELETE TRUNCATE 删除数据

##### DELETE

###### 不带WHERE删除所有行

```
USE Mydb
GO
DELETE customer2

--清空行记录，但不删除表，不删除表结构
```

###### 带WHERE有条件的删除

```
DELETE FROM sell_order
WHERE customer_id = 'C00006'
```

###### 在DELETE中使用连接或者子查询

```
将所有地址address以东方市开头的客户customer_id的销售订单从sell_order表中删除
```

1.连接

```
DELETE sell_order
FROM sell_order s
INNER JOIN customer2 c
ON s.customer_id = c.customer_id
WHERE c.address LIKE '东方市%'
```

2.AND

```
DELETE sell_order
FROM sell_order s,customer2 c
WHERE s.customer_id = c.customer_id
AND c.customer_name LIKE '东方市%'
```

3.子查询

```
DELETE FROM sell_order
WHERE customer_id IN ( 
                       SELECT customer_id
        FROM customer2
        WHERE customer_name LIKE '东方市%'
        )
```

##### TRUNCATE TABLE

```
TRUNCATE TABLE customer2
```

##### DELETE 和 TRUNCATE 对比


## 索引 INDEX

### 创建索引

- 建立主键约束时，自动创建主键索引。

```sql
CREATE [UNIQUE] [CLUSTERED|NONCLUSTERED] INDEX 索引名 ON 表/视图(列1 [DESC|ASC] [,列2,..])；
```

```sql
CREATE UNIQUE NONCLUSTERED INDEX emp_id_name_index ON employee(employee_id DESC,employee_name DESC);
```

### 删除索引

- 对于创建表时自动创建的索引（定义PRIMARY KEY和UNIQUE约束创建的索引），应该使用`ALTER TABLE ... DROP CONSTRAINT`语句。
- 在删除表/视图时，自动删除表/视图创建的索引。
- 在删除索引视图的聚集索引时，自动删除同一视图的所有非聚集索引和自动创建的统计信息。

```sql
DROP INDEX 索引名 ON 表/视图;
```

### 查询索引

```sql
EXEC sp_helpindex 索引所在的表名;
```

### 修改索引

**修改索引名**

```sql
EXEC sp_rename '表.旧索引名','新索引名','对象类型(INDEX)';
```

## 视图

- 虚拟表，视图并不在数据库中以存储的数据形式存在。

**分类**

- 标准视图
- 索引视图
- 分区视图
- 系统视图

**注**

- 可以嵌套视图，最多嵌套32层。

**视图与基表的比较**

- 同:
   1. 都是以二维表的形式存在
   2. 都可以通过表和视图的数据源进行查询、插入、修改、删除操作 
- 异： 
   1. 视图是虚拟表，数据存储在基表中。基表的数据存储在数据库中。
   2. 通过视图进行查询、插入、修改和删除操作时的语法格式、注意事项等不一样。
       - 其中语法格式主要是插入操作不同
       - 注意事项在通过视图进行各操作中都存在相应的注意事项。

### 基本操作

#### 创建视图

```sql
CREATE VIEW 视图名[(列1[,列2...])]
[WITH {[ENCRPTION] [SCHEMABINDING] [VIEW_METADATA]}] 
AS 
 SELECT语句; 
[WITH CHECK OPTION]
```

- ENCRYPTION 加密，使得界面化无法修改视图。
- SCHEMABINDING 与用户绑定。
- VIEW_METADATA
- WITH CHECK OPTION 在对视图操作时，需要满足创建视图时的SELECT语句中的条件。

```sql
--如果要使用ORDER BY子句：需要TOP n 
CREATE VIEW emp_id_name_order_index
AS
SELECT TOP 100 PERCENT employee_id,employee_name
FROM employee
ORDER BY employee_id DESC;
```

#### 修改视图

```sql
ALTER VIEW 视图名[(列1[,列2...])]
[WITH {[ENCRYPTION] [SCHEMABINDING] [VIEW_METADATA]}] 
AS 
 SELECT语句
[WITH CHECK OPTION];
```

#### 删除视图

```sql
DROP VIEW 视图名;
```

#### 查看视图

```sql
EXEC sp_hekptext 视图名;
```

#### 重命名视图

```sql
EXEC sp_rename '旧视图名','新视图名';
```

### 视图的应用

#### 通过视图检索表数据

- 在建立视图时，并不检查视图所参照的数据库对象是否存在，当查询时，才检查。
- 在建立视图之后，新增加的列不会出现在已定义的视图中。

```sql
SELECT 列
FROM 视图;
```

#### 通过视图添加表数据

- 插入视图的列值个数和数据类型必须和视图一一对应。
- 在通过视图向基表插入数据时，需要满足基表中的约束(如：非空约束)，包括视图中没有定义的（但基表中存在的约束），否则插入失败
- 使用`WITH CHECK OPTION`子句创建的视图，需要满足创建视图时的SELECT子句的条件。
- 还需要包括**通过视图修改表数据**的注意

```sql
INSERT INTO 视图
VALUES(...);
```

#### 通过视图修改表数据

- 若视图中含有多个基表
   - 要更改的列属于同一个基表，可以更新
   - 要更改的列分别属于不同的基表，不可以更新
   - 要更改的列为多个基表的公共列，不可以更新
- 使用`WITH CHECK OPTION`子句创建的视图，需要满足创建视图时的SELECT子句的条件。

```sql
UPDATE 视图
SET ...;
```

#### 通过视图删除表数据

- 要删除的数据行必须包含在视图的定义中。即在创建视图时，SELECT语句的范围内的数据。
- 删除的WHERE子句中的条件中只能包含视图中有的列。

```sql
DELETE FROM 视图
WHERE ...;
```

## 完整性

**完整性分类**



### RULE 规则（数据完整性）

#### 查看规则

```sql
EXEC sp_helptext 规则名;
```

#### 创建规则

```sql
CREATE RULE 规则名
AS 逻辑表达式; --@变量
```

#### 绑定和解绑规则

```sql
EXEC sp_bindrule 规则;
```

```sql
EXEC sp_unbindrule 规则;
```

1. 表的一列或一个用户定义数据类型只能与一个规则绑定，而一个规则可以绑定多个对象。
2. 

#### 删除规则

- 需要先解绑规则

```sql
DROP RULE 规则;
```

### DEFAULT 默认值

```sql
CREATE DEAULT 默认值名 AS 默认值(不能是列或者其他数据库对象);
```

#### 查看默认值 sp_helptext

```sql
EXEC sp_helptext 默认值名;
```

#### 默认值的绑定 sp_binderfaults

- 如果列同时绑定了一个规则和一个默认值，那么默认值一个符合规则的规定。
- 不能绑定默认值到一个已经用DEFAULT指定了默认值的列上

**语法:**

```sql
sp_binderfault [@defname = ] 默认值,[@objname = ] '对象名(表.列)';

sp_binderfault [@defname = ] 默认值,[@objname = ] '对象名(表.列)'[,[@futureonly = ] 'futureonly']
--futureonly仅在绑定默认值到用户定义数据类型上时可以使用。仅之后使用此用户数据类型的列会应用新的默认值，而之前的则不变。
```

```
CREATE DEFAULT today_def AS getdate();

EXEC sp_binderfault today_def,'employee.hire_date';
```

#### 默认值的松绑

**语法**

```
sp_underfault '要松绑的表.列名';
```

#### 删除默认值

- 在删除一个默认值前，必须先将其于其绑定的所有对象松绑，否则删除失败。

**语法：**

```
DROP DEFAULT 默认值名1[,默认值2...]
```

### 约束

#### PRIMARY KEY 主键约束（实体完整性）

- 使用表的一列或几列的组合在表中唯一的指定一行记录。
- 非空且唯一
- 主键约束的列级约束可以是一列或几列的组合作为主键，而以多列的组合作为主键只能是主键约束的表级约束。
- 一个表最多只能有一个主键，可以没有主键

**语法**

```
列级：
[CONSTRAINT 主键约束名] PRIMARY KEY [CLUSTED|NONCLUSTED]

表级：
[CONSTRAINT 主键约束名] PRIMARY KEY [CLUSTED|NONCLUSTED] (列1[,列2...])
```

**列级主键约束**

```
CREATE TABLE test_table1
(
 id INT CONSTRAINT id_pk PRIMARY KEY
,name VARCHAR(20)
,score INT
)
```

**表级约束**

```
CREATE TABLE test_table2
(
 id INT
,name VARCHAR(20)
,score INT
,CONSTRAINT id_name_pk PRIMARY KEY(id,name)
)
```

#### FOREIGN KEY 外键约束（参照完整性）

- 表与表之间的关系
- 当一个表中的一列或多列的组合和其他表中的主键定义相同时，可以将这些列设定为该表的外键，并设定与该外键相关联的表或列。
- 当具有外键的表插入数据时，需要和与外键相关联的（表中的）列值相同。

**语法：**

```
列级：
[CONSTRAINT 约束名] FOREIGN KEY (列1[,列2...])
REFERENCES 相关联的表[(列1[,列2...])]
[ON DELETE {CASCADE|NO ACTION}]  --级联删除 默认NO ACTION
[ON UODATE {CASCADE|NO ACTION}]  --级联修改
[NOT FOR REPLICATION] --把从其他复制的数据插入时，该外键不作用
 
表级：
[CONSTRAINT 约束名] [FOREIGN KEY]
REFERENCES 相关联的表
[NOT FOR REPLICATION]
```

**CASCADE 级联修改  级联删除**

- 主键表（被关联的表）中的值修改或删除时，外键表中的对应的值也修改或删除。

**列级**

```
CREATE TABLE pk_table
(
 id INT CONSTRAINT id_ok PRIMARY KEY
,name VARCHAR(20)
,score INT
)

 CREATE TABLE fk_table1
(
 id INT CONSTRAINT id1_fk FOREIGN KEY REFERENCES pk_table(id) 
,name VARCHAR(20)
,score INT
,class VARCHAR(20) 
)
```

**表级**

```
 CREATE TABLE fk_table2
(
 id INT 
,name VARCHAR(20)
,score INT
,class VARCHAR(20) 
,CONSTRAINT id2_fk FOREIGN KEY(id)
   REFERENCES pk_table(id)
   ON DELETE CASCADE
   ON UPDATE CASCADE
)
```

#### UNIQUE 唯一性约束（实体完整性）

**语法：**
```
列级：
[CONSTRAINT 约束名] UNIQUE [CLUSTERED|NONCLUSTERED]  

表级：
[CONSTRAINT 约束名] UNIQUE [CLUSTERED|NONCLUSTERED] (列1[,列2...])
```

**列级**

```
CREATE TABLE unique_test1
(
 id INT CONSTRAINT ut1_id_uk UNIQUE
,name VARCHAR(20) CONSTRAINT ut1_name_uk UNIQUE
,score INT 
)
```

**表级**

```
CREATE TABLE unique_test2
(
 id INT
,name VARCHAR(20)
,score INT
,CONSTRAINT ut2_uk UNIQUE (id,name)
)
```

#### CHECK 检查约束（数据完整性）

- 可以为每列指定多个CHECK约束

**语法：**

```
[CONSTRAINT 约束名] CHECK [NOT FOR APLLICATION] (逻辑表达式（可以使用LIKE))
```

```
CREATE TABLE check_test
(
 id INT CONSTRAINT ct_id_ck CHECK (id BETWEEN 1 AND 99)
,name VARCHAR(20) CHECK (name LIKE '郑%')  --列级
,id2 INT
,CHECK (id != id2) --表级
)
```

##### DEFAULT 默认约束

**语法**

```
[CONSTRAINT 约束名] DEFAULT 值 [FOR 列名]
```

```
CREATE TABLE default_test1
(
 id INT CONSTRAINT dt1_id_def DEFAULT 0 
,name VARCHAR(20)
)
```

###### 添加默认约束

```
ALTER TABLE default_test1
ADD CONSTRAINT dt1_name_def DEFAULT '学生' FOR name;
```

## 查询

### SELECT

- 查看当前版本

```sql
SELECT @@version "版本"
```

#### TOP n

**语法**

```sql
SELECT TOP 数字 [PERCENT] [WITH TIES] 列1,列2
FROM 表
```

#### DISTINCT

### FROM 子句

- ORACLE 必须有FROM子句
- SQLserver 不必须

### WHERE 子句

#### 条件表达式

- =
- `>`
- `<`
- `>=`
- <=
- !=  或 <>

```sql
列出employee表中月工资在4000元以上的员工记录
SELECT *
FROM employees
WHERE wages > 4000
```

```sql
对employee表，求出男员工的平均工资
SELECT AVG(wages) "男员工的平均工资"
FROM employees
WHERE sex = '男'
```

#### 特殊运算符

- ALL
- ANY
- BETWEEN
- EXSIST 为空则false
- IN
- LIKE
   -  _ 表示1个字符
   -  % 表示0到多个字符 
   - [] 表示范围内的字符 [0-9] 表示0-9的数字
   - [^] 表示不在范围内的字符
- SOME

### ORDER BY

- ASC 升序 默认
- DESC 降序

###  INTO 

```sql
SELECT employee.*
INTO test_table
FROM emoployee
WHERE department_id = 'D002';
```

### 集合函数

- AVG()
- SUM()
- COUNT()
- MIN()
- MAX()

```sql
查询employee表中，分别查询公司的员工总人数和公司员工的人均薪酬
SELECT COUNT(*) "总人数"
FROM employees

SELECT AVG(wages) "人均薪资"
FROM employees
```

#### GROUP BY

#### HAVING

### UNION 不包括重复

- 将多个查询结果合并

#### UNION ALL 包括重复

### 连接

## 表达式

### 数据

#### 用户自定义数据类型

**创建用户自定义数据类型**

**sp_addtype**

```sql
EXEC sp_addtype [@typename=] 自定义的数据类型名,
           [@phystype=] '系统数据类型',
         [ [@nulltype=] 'NULL | {NOT NULL} | NONULL',]  --是否允许空值 NONULL即NOT NULL
         [ [@owner=] '所有者' ];    
```

- `@nulltype` 如果是NOT NULL / NONULL，则在表中等于有非空约束。 

- 用户自定义类型可以绑定默认值和规则，和普通的绑定一样。

#### 常量

#### 变量

**标识符规则**

1. 以字母，下划线_，@，# 开头
2. 可以是字母，下划线_，@，$ 内容
3. 不允许空格和其他非法符号

##### 局部变量

- 标识 @
- 作用范围只在程序内部

**定义局部变量**

```sql
DECLARE @局部变量名 数据类型 [,@局部变量2 数据类型 ...]
```

- 数据类型可以是自定义的数据类型

**赋值**

- 需要先定义局部变量
- 默认NULL

1. SELECT语句

```sql
SELECT @局部变量 = 值 [,@局部变量2 = 值 ...]
[FROM ...];
```

2. UPDATE...SET语句
3. SET语句

```sql
SET @变量 = 值
```

**打印**

```sql
SELECT @局部变量
```

##### 全局变量
  
- 标识 @@
- 作用范围在任何程序范围
- 用于被服务器用来跟踪服务器范围和特定会话期间的信息
- 不能被显示地被赋值和声明
- 不能被用户定义，不能被应用程序用来在处理器之间交叉传递信息

### 运算符

#### 连接运算符 + 

- Oracle中的 || 或 CONCAT() 

#### 算术运算符

#### 位运算符

#### 比较运算符

#### 逻辑运算符

#### LIKE通配符

- `_`
- `%`
- `[]`
- `[^]`

## 函数

### 常用函数

#### 字符串函数

#### 数学函数

#### 日期函数

**日期**

| 日期         | 日期       | 表示 | 取值         |
| :---------- | :-------- | :--- | :---------- |
| Year        | 年         | yy   | 1753~9999   |
| Quarter     | 季度       | qq   | 1~4         |
| Month       | 月         | mm   | 1~12        |
| Dayofyear   | 日（年）   | dy   | 1~366       |
| Day         | 日（月份） | dd   | 1~31        |
| Week        | 周         | wk   | 1~54        |
| Weekday     | 星期       | dw   | 1~7 Mon~Sun |
| Hour        | 时         | hh   | 0~23        |
| Minute      | 分钟       | mi   | 0~59        |
| Second      | 秒         | ss   | 0~59        |
| Miliseconde | 毫秒       | ms   | 0~999       |

**函数**

![](C:/notebook/pictures/Snipaste_2022-12-01_10-15-21.png =700x)

### 用户定义函数

#### 标量函数

**格式**

```sql
CREATE [所有者.]FUNCTION 函数名
(
  @变量 数据类型
  [,...]
)
RETURNS 数据类型[数据类型长度]  --RETURNS 数据类型可以有长度的...和Oracle不同
AS --没有IS
BEGIN
   函数体
   RETURN 表达式/值  --没有分号
END;
```

```sql
CREATE FUNCTION date_to_quarter
(
  @v_date DATETIME
)
RETURNS CHAR(6)
AS
BEGIN
   RETURN (DATENAME(Q,@v_date) + 'Q' + DATENAME(YYYY,@v_date))
END;
```

#### 内嵌表值函数

**格式**

```sql
CREATE [所有者.]FUNCTION 函数名
(
  @变量 数据类型
  [,...]
)
RETURNS TABLE
AS
RETURN (SELECT语句) --无分号
```

```sql
CREATE FUNCTION 
(
   @v_emp_id VARCHAR(30)
)
RETURNS TABLE
AS
RETURN(
   SELECT employee_id,employee_name 
   FROM employee
   WHERE employee_id = @employee_id
   );
```

#### 多语句表值函数

```sql
CREATE FUNCTION [所有者.]函数名
(
  
)
RETURNS  
AS
BEGIN
  函数体
  RETURN
END
```

#### 调用

##### 标量函数

- 如果在多个数据库中存在同名的用户定义函数，则需要指定所有者

```sql
SELECT [所有者.]函数名(参数)
```

##### 内嵌表函数

- 返回的是一个表

```sql
SELECT *
FROM [所有者.]函数名参数)
```

## 存储过程

**类型**

| 存储过程类型       | 存储                                            |
| :---------------- | :--------------------------------------------- |
| 系统存储过程       | 存储在master数据库，以sp_前缀                    |
| 用户自定义存储过程 | 存储在用户数据库                                 |
| 临时存储过程       | 关闭SQL server时删除，以#（本地）、##（全局）前缀 |
| 扩展存储过程       | DLL动态链接库，以xp_前缀                         |

### 创建/修改

```sql
CREATE|ALTER PROCEDURE 存储过程名
[@参数 数据类型,@参数 数据类型 = 默认值,@参数 数据类型 OUTPUT] --OUTPUT返回值给该环境的传入参数
[WITH ENCRYPTION|RECOMPILE|ENCRYPTION,RECOMPRILE]
[FOR REPLICATION] 
AS
执行语句
[RETURN 状态值] --0成功，-99~-1错误，其他可自定义（通过触发器）
```

- REPLICATION 是否将执行结果放入缓存

### 调用

```sql
EXECUTE 存储过程 值1,@参数2=值2,有值的局部变量3 OUTPUT，值4 [WITH REPLICATION]
```

### 删除存储过程

```sql
DROP PROCEDURE 存储过程名
```