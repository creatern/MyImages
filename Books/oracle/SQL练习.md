# 1. 行程和用户问题

## 题目

**表**

```sql
CREATE TABLE users 
(
 users_id INT PRIMARY KEY
,banned VARCHAR(10)
,role VARCHAR(20) CHECK(role IN ('client', 'driver', 'partner'))
);

CREATE TABLE trips
(
 id int PRIMARY KEY
,client_id INT
,driver_id INT
,city_id int
,status VARCHAR2(20) CHECK(status IN ('completed', 'cancelled_by_driver', 'cancelled_by_client'))
,request_at DATE
);

insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('1', '1', '10', '1', 'completed', TO_DATE('2013-10-01','YYYY-MM-DD'));
insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('2', '2', '11', '1', 'cancelled_by_driver', TO_DATE('2013-10-01','YYYY-MM-DD'));
insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('3', '3', '12', '6', 'completed', TO_DATE('2013-10-01','YYYY-MM-DD'));
insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('4', '4', '13', '6', 'cancelled_by_client', TO_DATE('2013-10-01','YYYY-MM-DD'));
insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('5', '1', '10', '1', 'completed', TO_DATE('2013-10-02','YYYY-MM-DD'));
insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('6', '2', '11', '6', 'completed', TO_DATE('2013-10-02','YYYY-MM-DD'));
insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('7', '3', '12', '6', 'completed', TO_DATE('2013-10-02','YYYY-MM-DD'));
insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('8', '2', '12', '12', 'completed', TO_DATE('2013-10-03','YYYY-MM-DD'));
insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('9', '3', '10', '12', 'completed', TO_DATE('2013-10-03','YYYY-MM-DD'));
insert into Trips (id, client_id, driver_id, city_id, status, request_at) 
values ('10', '4', '13', '12', 'cancelled_by_driver', TO_DATE('2013-10-03','YYYY-MM-DD'));

insert into Users (users_id, banned, role) 
values ('1', 'No', 'client');
insert into Users (users_id, banned, role) 
values ('2', 'Yes', 'client');
insert into Users (users_id, banned, role) 
values ('3', 'No', 'client');
insert into Users (users_id, banned, role) 
values ('4', 'No', 'client');
insert into Users (users_id, banned, role) 
values ('10', 'No', 'driver');
insert into Users (users_id, banned, role) 
values ('11', 'No', 'driver');
insert into Users (users_id, banned, role) 
values ('12', 'No', 'driver');
insert into Users (users_id, banned, role) 
values ('13', 'No', 'driver');
```

**需求**

取消率 的计算方式如下：(被司机或乘客取消的非禁止用户生成的订单数量) / (非禁止用户生成的订单总数)。

写一段 SQL 语句查出 "2013-10-01" 至 "2013-10-03" 期间非禁止用户（乘客和司机都必须未被禁止）的取消率。非禁止用户即 banned 为 No 的用户，禁止用户即 banned 为 Yes 的用户。

返回结果表中的数据可以按任意顺序组织。其中取消率 Cancellation Rate 需要四舍五入保留 两位小数 。


## 解题

### SQL

#### 子查询

```sql
SELECT DISTINCT ROUND((c.yes_num / t.total_num),2) AS "取消率"
FROM(
     SELECT COUNT(id) yes_num
     FROM trips
     WHERE status != 'completed'
      AND (SELECT banned
           FROM users
           WHERE users_id = client_id
           ) = 'Yes'
     ) c
     ,(
      SELECT COUNT(id) total_num
      FROM trips
      WHERE(SELECT banned
            FROM users
            WHERE users_id = client_id
            ) = 'Yes'
     ) t
      ,trips
WHERE request_at BETWEEN TO_DATE('2013-10-01','YYYY-MM-DD') AND TO_DATE('2013-10-03','YYYY-MM-DD');
```

#### 连接

```sql
SELECT t.request_at Day,
       ROUND(SUM(DECODE(t.status,'completed',0,1))/COUNT(1),2) "Cancellation Rate" 
FROM trips t
 LEFT JOIN users u1 ON (t.client_id = u1.users_id AND u1.role = 'client')
 LEFT JOIN users u2 ON (t.driver_id = u2.users_id and u2.role = 'driver')
WHERE u1.banned <> 'Yes' 
 AND c.banned <> 'Yes' 
 AND u2.request_at between to_date('2013-10-01','yyyy-mm-dd') and to_date('2013-10-03','yyyy-mm-dd')
GROUP BY t.request_at
```

### PL/SQL

```plsql
DECLARE 
  v_canceltion NUMBER;
  v_total NUMBER;
  v_canceltion_rate NUMBER(4,2);
BEGIN
  SELECT COUNT(id)
  INTO v_total
  FROM trips
  WHERE (SELECT banned
         FROM users
         WHERE trips.client_id = users.users_id
         ) != 'No'
    AND request_at BETWEEN TO_DATE('2013-10-01','yyyy-mm-dd') AND TO_DATE('2013-10-03','yyyy-mm-dd');
         
  SELECT COUNT(id)
  INTO v_canceltion
  FROM trips
  WHERE (SELECT banned
         FROM users
         WHERE trips.client_id = users.users_id
         ) != 'No'
    AND request_at BETWEEN TO_DATE('2013-10-01','yyyy-mm-dd') AND TO_DATE('2013-10-03','yyyy-mm-dd')
    AND status != 'completed';
  
  v_canceltion_rate := ROUND((v_canceltion / v_total),2);
    
  DBMS_OUTPUT.PUT_LINE('取消率: '||v_canceltion_rate);
END; 
```

# 2. 部门工资前三高的所有员工

## 题目

**表**

```sql
create table Employee (id int, name varchar(255), salary int, departmentId int);
create table Department (id int, name varchar(255));
Truncate table Employee;
insert into Employee (id, name, salary, departmentId) values ('1', 'Joe', '85000', '1');
insert into Employee (id, name, salary, departmentId) values ('2', 'Henry', '80000', '2');
insert into Employee (id, name, salary, departmentId) values ('3', 'Sam', '60000', '2');
insert into Employee (id, name, salary, departmentId) values ('4', 'Max', '90000', '1');
insert into Employee (id, name, salary, departmentId) values ('5', 'Janet', '69000', '1');
insert into Employee (id, name, salary, departmentId) values ('6', 'Randy', '85000', '1');
insert into Employee (id, name, salary, departmentId) values ('7', 'Will', '70000', '1');
Truncate table Department;
insert into Department (id, name) values ('1', 'IT');
insert into Department (id, name) values ('2', 'Sales');
```

**需求**

公司的主管们感兴趣的是公司每个部门中谁赚的钱最多。

一个部门的高收入者是指一个员工的工资在该部门的不同工资中排名前三 (相同工资放在同一排位，不额外占用名额)。

编写一个SQL查询，找出每个部门中 收入高的员工 。

## 解题

### SQL

#### 1. 子查询 DENSE_RANK() OVER()函数

```sql
SELECT *
FROM (
  SELECT d.name dept_name,e.name emp_name,salary,
         DENSE_RANK() OVER(PARTITION BY departmentid ORDER BY salary) rn
  FROM employee e, department d
  WHERE e.departmentid = d.id
) t
WHERE t.rn <= 3;
```

# 3. 第N高的薪水

## 题目

**表**

```sql
Create table Employee (Id int, Salary int);
Truncate table Employee;
insert into Employee (id, salary) values ('1', '100');
insert into Employee (id, salary) values ('2', '200');
insert into Employee (id, salary) values ('3', '300');
```

**需求**

编写一个SQL查询来报告 Employee 表中第 n 高的工资。如果没有第 n 个最高工资，查询应该报告为 null 。

## 解决

### PL/SQL函数

```plsql
CREATE OR REPLACE FUNCTION getNthHighestSalary
(
    N IN NUMBER
)
RETURN NUMBER
IS
  result NUMBER;
BEGIN
    SELECT NVL(salary,NULL)
    INTO result
    FROM(
     SELECT rownum rn,
            salary
     FROM (
           SELECT salary
           FROM employee
           ORDER BY salary DESC
           )
     )
    WHERE rn = N;

    RETURN result;
END;
```

# 4. 体育馆的人流量

## 题目

**表**

```sql
Create table Stadium (id int, visit_date DATE NULL, people int);
Truncate table Stadium;
insert into Stadium (id, visit_date, people) values ('1', TO_DATE('2017-01-01','yyyy-mm-dd'), '10');
insert into Stadium (id, visit_date, people) values ('2', TO_DATE('2017-01-02','yyyy-mm-dd'), '109');
insert into Stadium (id, visit_date, people) values ('3', TO_DATE('2017-01-03','yyyy-mm-dd'), '150');
insert into Stadium (id, visit_date, people) values ('4', TO_DATE('2017-01-04','yyyy-mm-dd'), '99');
insert into Stadium (id, visit_date, people) values ('5', TO_DATE('2017-01-05','yyyy-mm-dd'), '145');
insert into Stadium (id, visit_date, people) values ('6', TO_DATE('2017-01-06','yyyy-mm-dd'), '1455');
insert into Stadium (id, visit_date, people) values ('7', TO_DATE('2017-01-07','yyyy-mm-dd'), '199');
insert into Stadium (id, visit_date, people) values ('8', TO_DATE('2017-01-08','yyyy-mm-dd'), '188');
```

**需求**

编写一个 SQL 查询以找出每行的人数大于或等于 100 且 id 连续的三行或更多行记录。

返回按 visit_date 升序排列 的结果表。

## 解决

### SQL

#### 1. 自连接

```sql
SELECT DISTINCT a.*
FROM stadium a,stadium b,stadium c
WHERE a.people >= 100
  AND b.people >= 100
  AND c.people >= 100
  AND ((a.id = b.id - 1 AND a.id = c.id - 2) OR
       (a.id = b.id + 1 AND a.id = c.id + 2) OR
       (a.id = b.id - 1 AND a.id = c.id + 1))
ORDER BY a.id ASC;
``` 

#### 2. 排序

```sql
WITH temp 
AS(
SELECT id, visit_date, people,
       id - ROW_NUMBER() OVER(ORDER BY id) as rnk1
       --id可能是不连续的，而ROW_NUMBER() OVER(ORDER BY id)是连续排列的
       --所以id - rnk1 的值：如果是连续的id，则rnk1值相同，否则在不连续的地方rnk1值改变
FROM stadium 
WHERE people >= 100
)

SELECT id, visit_date, people
FROM temp
WHERE rnk1 IN (
    SELECT rnk1 
    FROM temp
    GROUP BY rnk1
    HAVING COUNT(rnk1) > 2
    --寻找相同的rnk1的数量大于2（即3以上的）的rnk1值
)
ORDER BY id;
```

#### 3. 子查询

```sql
SELECT *
FROM stadium
WHERE people >= 100
 AND (
      (id IN (SELECT id FROM stadium WHERE people >= 100) AND
       id - 1 IN (SELECT id FROM stadium WHERE people >= 100) AND
       id - 2 IN (SELECT id FROM stadium WHERE people >= 100)
       ) 
    OR (
       id + 2 IN (SELECT id FROM stadium WHERE people >= 100) AND
       id + 1 IN (SELECT id FROM stadium WHERE people >= 100) AND
       id IN (SELECT id FROM stadium WHERE people >= 100) 
       ) 
    OR(
       id + 1 IN (SELECT id FROM stadium WHERE people >= 100) AND
       id IN (SELECT id FROM stadium WHERE people >= 100) AND
       id - 1 IN (SELECT id FROM stadium WHERE people >= 100)
       )
      );
``` 

### PL/SQL

```plsql

```

# 5. 重复邮箱查找

## 题目

**表**

```sql
Create table Person (id int, email varchar(255));
Truncate table Person;
insert into Person (id, email) values ('1', 'a@b.com');
insert into Person (id, email) values ('2', 'c@d.com');
insert into Person (id, email) values ('3', 'a@b.com');
```

**需求**

编写一个 SQL 查询，查找 Person 表中所有重复的电子邮箱。

## 解决

### SQL

#### 1. 自连接

```sql
SELECT DISTINCT p2.email
FROM person p1
 JOIN person p2 ON p1.id <> p2.id  AND p1.email = p2.email
```

#### 3. 组函数 HAVING

```sql
SELECT email
FROM person
GROUP BY email
HAVING COUNT(email) >= 2;
```

#### 3. 子查询 组函数

```sql
SELECT t.email
FROM (SELECT email,COUNT(*) num
      FROM person
      GROUP BY email
      ) t
WHERE t.num >= 2;
```

#### 4. WITH

```sql
WITH temp
AS
(
 SELECT email,COUNT(id) num
 FROM person p2
 GROUP BY email
)

SELECT DISTINCT p.email
FROM person p,temp t
WHERE p.email = t.email 
  AND t.num >= 2;
```

# 6. 树节点

## 题目

**表**

```sql
Create table Tree (id int, p_id int);
Truncate table Tree;
insert into Tree (id, p_id) values ('1', null);
insert into Tree (id, p_id) values ('2', '1');
insert into Tree (id, p_id) values ('3', '1');
insert into Tree (id, p_id) values ('4', '2');
insert into Tree (id, p_id) values ('5', '2');
```

**需求**

树中每个节点属于以下三种类型之一：

- 叶子：如果这个节点没有任何子节点。
- 根：如果这个节点是整棵树的根，即没有父节点。
- 内部节点：如果这个节点既不是叶子节点也不是根节点。
 
写一个查询语句，输出所有节点的编号和节点的类型，并将结果按照节点编号排序。上面样例的结果为：

![](vx_images/152765608239495.png =82x)

## 解决

### SQL

#### 1. 分层查询 DECODE函数

```sql
SELECT id,DECODE(LEVEL,1,'ROOT',2,'Inner',3,'Leaf') "类型"
FROM tree
START WITH p_id IS NULL
CONNECT BY PRIOR id = p_id
ORDER BY id;
```

#### 2. CASE ... WHEN 自连接

```sql
SELECT DISTINCT self.id,
       CASE
           WHEN self.p_id IS NULL THEN 'Root'
           WHEN child.id IS NULL THEN 'Leaf'
           WHEN child.id IS NOT NULL THEN 'Inner'
       END "类型"
FROM tree self
  LEFT JOIN tree child ON self.id = child.p_id;
```

# 7. 市场分析 I

## 题目

**表**

```sql
Create table Users (user_id int, join_date date, favorite_brand varchar(10));
Create table Orders (order_id int, order_date date, item_id int, buyer_id int, seller_id int);
Create table Items (item_id int, item_brand varchar(10));
Truncate table Users;
insert into Users (user_id, join_date, favorite_brand) values ('1', TO_DATE('2018-01-01','YYYY-MM-DD'), 'Lenovo');
insert into Users (user_id, join_date, favorite_brand) values ('2', TO_DATE('2018-02-09','YYYY-MM-DD'), 'Samsung');
insert into Users (user_id, join_date, favorite_brand) values ('3', TO_DATE('2018-01-19','YYYY-MM-DD'), 'LG');
insert into Users (user_id, join_date, favorite_brand) values ('4', TO_DATE('2018-05-21','YYYY-MM-DD'), 'HP');
Truncate table Orders;
insert into Orders (order_id, order_date, item_id, buyer_id, seller_id) values ('1', TO_DATE('2019-08-01','YYYY-MM-DD'), '4', '1', '2');
insert into Orders (order_id, order_date, item_id, buyer_id, seller_id) values ('2', TO_DATE('2019-08-02','YYYY-MM-DD'), '2', '1', '3');
insert into Orders (order_id, order_date, item_id, buyer_id, seller_id) values ('3', TO_DATE('2019-08-03','YYYY-MM-DD'), '3', '2', '3');
insert into Orders (order_id, order_date, item_id, buyer_id, seller_id) values ('4', TO_DATE('2019-08-04','YYYY-MM-DD'), '1', '4', '2');
insert into Orders (order_id, order_date, item_id, buyer_id, seller_id) values ('5', TO_DATE('2019-08-05','YYYY-MM-DD'), '1', '3', '4');
insert into Orders (order_id, order_date, item_id, buyer_id, seller_id) values ('6', TO_DATE('2019-08-06','YYYY-MM-DD'), '2', '2', '4');
Truncate table Items;
insert into Items (item_id, item_brand) values ('1', 'Samsung');
insert into Items (item_id, item_brand) values ('2', 'Lenovo');
insert into Items (item_id, item_brand) values ('3', 'LG');
insert into Items (item_id, item_brand) values ('4', 'HP');
```

**需求**

请写出一条SQL语句以查询每个用户的注册日期和在 2019 年作为买家的订单总数。

以 任意顺序 返回结果表。

## 解决

### SQL

#### 1. COUNT

```sql
SELECT DISTINCT o.buyer_id,u.join_date,s.order_num_2019
FROM orders o,users u,
   (SELECT COUNT(order_id) order_num_2019,buyer_id
    FROM orders
    WHERE TO_CHAR(order_date,'YYYY') = '2019'
    GROUP BY buyer_id
    ) s
WHERE s.buyer_id = o.buyer_id
  AND u.user_id = o.buyer_id
ORDER BY o.buyer_id ASC;
```

#### 2. SUM

```sql
select a.buyer_id,
       to_char(b.join_date,'yyyy-mm-dd') join_date,
       SUM((CASE
             WHEN to_char(a.order_date, 'yyyy') = '2019' THEN
              1
             else
              0
           END)) orders_in_2019
  from Orders a, Users b
 where a.buyer_id = b.user_id
 group by a.buyer_id, b.join_date
 order by a.buyer_id;
```

#### 3. LEFT JOIN

```sql
SELECT Users.user_id buyer_id,
       TO_CHAR(Users.join_date,'yyyy-MM-dd') join_date,
       COUNT(buyer_id) orders_in_2019
FROM Users 
  LEFT OUTER JOIN (SELECT * FROM Orders WHERE TO_CHAR(order_date,'yyyy') = '2019') 
  ON Users.user_id= buyer_id
GROUP BY Users.user_id,Users.join_date
ORDER BY buyer_id;
```

# 8.

## 题目

**表**

```sql
Create table Seat (id int, student varchar(255));
Truncate table Seat;
insert into Seat (id, student) values ('1', 'Abbot');
insert into Seat (id, student) values ('2', 'Doris');
insert into Seat (id, student) values ('3', 'Emerson');
insert into Seat (id, student) values ('4', 'Green');
insert into Seat (id, student) values ('5', 'Jeames');
```

**需求**

编写SQL查询来交换每两个连续的学生的座位号。如果学生的数量是奇数，则最后一个学生的id不交换。

按 id 升序 返回结果表。

## 解决

### SQL

#### 1.

解题思路：
首先使用decode判断当前行id是奇数还是偶数，如果是奇数用lead函数往下一行偏移查数据，如果是偶数用lag函数往上一行偏移查数据。
lead(field, num, defaultvalue) field需要查找的字段，num往后查找的num行的数据，defaultvalue没有符合条件的默认值。
lag(field, num, defaultvalue)与lead(field, num, defaultvalue)刚好相反，是往前查找num行的数据。

代码

select id,
decode(mod(id,2),0,lag(student,1,student) over(order by id),1,lead(student,1,student) over(order by id)) student
from seat

```sql

```

1.使用子查询直接改变student的值


SELECT a.id,
       --为空就肯定是最后一行是奇数了,直接给原本的值就OK了
       NVL((CASE
             WHEN mod(a.id, 2) <> 0 THEN
              (SELECT s1.student FROM seat s1 where s1.id = a.id + 1)
             else
              (SELECT s1.student FROM seat s1 where s1.id = a.id - 1)
           END),
           a.student) student
  FROM seat a;
2.使用子查询改变ID的值在排序


SELECT *
  FROM (SELECT NVL((CASE
                     WHEN mod(a.id, 2) <> 0 AND
                          a.id = (SELECT COUNT(1) FROM seat) THEN
                      id
                     WHEN mod(a.id, 2) <> 0 THEN
                      id + 1
                     WHEN mod(a.id, 2) = 0 THEN
                      id - 1
                   END),
                   a.id) id,
               a.student
          FROM seat a) A
 order by A.id


# 9.

## 题目

**表**

```sql

```

**需求**


## 解决

### SQL

#### 1.

```sql

```