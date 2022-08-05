---
title: MySQL学习
date: 2021-03-09 19:40:26
tags: 后端
categories: 动力结点后端课程
---



# 这是对于老杜动力节点相关MySQL课程的学习，个人觉得非常棒，讲的很透彻，担忧一部分的内容由于业务接触少，没有详细介绍，在这个学期的数据库课程里应该会提到，那个时候再学嗷！！！

<!--more-->





# 数据库概念

## 数据库是啥

- 序列化和反序列化可以的，也可以存成文件。但是复杂，也不够便利
- 底层还是流来处理，只是现在更加高级，不用写流了，直接用sql语句就能完成高层数据库的调用。



## 数据库的安装与登陆

- 有一个比较好的安装教程

[Mysql安装教程](https://zhuanlan.zhihu.com/p/37152572)

- 关于新版的installer的一些使用：

由于我下载的时候选择出了一些问题，下载了很多我不需要的东西，后面才发现这个东西很好用。即使用下载好的installer打开后可以进行进一步的配置。

![image-20210303112714373](https://gitee.com/alexs-rabbit//blog/raw/master/20210303112714.png)

左边是已经安装好的mysql相关的产品，右边可以选择添加，调整或者删除对应的产品，也可以进行升级，Quick Action里面的Reconfigure可以帮助你快速的调整Mysql的配置。

- 当配置完成环境变量之后：

![image-20210308174226353](https://gitee.com/alexs-rabbit//blog/raw/master/20210308174233.png)

用cmd登陆mysql , -u表示登陆的账号， -p表示password ，输入完成就可以登陆mysql。

![image-20210303113119884](https://gitee.com/alexs-rabbit//blog/raw/master/20210303113119.png)

下面这种也可以的，这种方式可以保护你的密码。

- 服务端口为3306 ， 以及可以在本电脑->管理中来调整一些类似于开机自动启动的设置。



### 卸载MySQL（血的教训）

- **卸载mysql有点复杂**，除了在安装包中（就是下载的时候下载的那个msi）中可以remove安装的mysql产品之外，`C:\Program Files\MySQL和C:\ProgramData\MySQL和C:\Program Files (x86)\MySQL`都要干掉，下次下载才能顺利。



## SQL与DB与DBMS的关系

- SQL也是高级语言，由程序员来写。执行之前也需要进行编译。
- SQL由DBMS来执行
- DBMS通过执行SQL语句来操作DB中的数据。
- 说白了MySQL是用来管理数据库的。





# MySQL语句学习（CRUD）

### 语句类型分类

	- DQL ：查询语句 (所有的select有关的)
	- DML：数据操作语言（对于数据进行增删改：insert , delete , update）
	- DDL：数据定义语言  （对于表结构进行增删改：drop , create , outer）
	- TCL：事务控制语言（这里的T为Transaction：roll back之类的）
	- DCL：数据控制语言（授权以及撤销权限。）

- 表的结构是很少动的，因为动表的结构的成本太高了。
- 导入数据

### 数据实操

- 用cmd或者mysql自带的cmd登录

- 查看数据库: show databases; （这个不是sql语言，是mysql命令）

- 创建数据库：create database liututu;（命令）

- 使用数据库：use liututu; （命令）

- 查看表格：show tables; （命令）

- 初始化数据：这里很难实践，如果有.sql为后缀的资源（这种文件被称为sql脚本，其中编写了大量的sql语句，其实本质就是文本文件哈哈哈哈哈哈）的话，可以用：source+资源路径; 来初始化数据表。sql脚本中的数据量太大的时候无法打开，使用source命令完成初始化。

- 删除数据库： drop databases test;  （sql语言）

- 查表的结构：desc+表名; 

  ![image-20210303172510103](https://gitee.com/alexs-rabbit//blog/raw/master/20210303172517.png)

- 查看某个表中的数据：select * from table-name







### MySQL常用命令

- create database ...;
- 查询当前使用的数据库：
  - select database();
  - select version();(查看当前数据库的版本)
  - 实在不知道你可以直接先show databases; 然后再use嘛！！！
- 结束一条写错的语句，直接加\c可以实现
- 推出mysql可以用exit
- show tables from 表名
- desc + 表名
- 查看创建表的语句： show create table 表名;







### SQL语句学习

#### DQL：

##### 单表查询

- **最基本用法**

  select 字段名1，字段名2，... from 表名; 不记得表里有哪些东西就先用desc + 表名。大小写都可以，换行也可以，打上分号才才行。

  e.g.

  1. select ename , sal * 12 from tmp;   (表中字段可以参与数学运算)
  2. select ename , sal * 12 as yearsal from tmp;   （表中字段可以重命名，如果要用中文，要用引号，例如 as '年薪'）
  3. 这里单双引号都可以，建议单引号。双引号在别的SQL语言里面不一定可以识别，但在Mysql里面可以，为了避免问题，还是用单引号。标准sql语句中，字符串要用单引号括起来。
  4. as关键字可以省略。
  5. 查询全部字段:  select * from temp; 私下可以，但是java程序中，实际开发中不要写。这个方式的效率太低了，会影响产品的性能。

- **条件查询**：

  1. select 字段名1，字段名2，... from 表名 where 条件; 执行顺序是先from , 然后where , 最后select。 e.g. select sal from emp where ename='SMITH'。另外提醒一下，在mysql中，string就是varchar , 字符串使用单引号括起来。

  2. 在sql中，<>就是!=，此外sql中!=也是可以使用的，<>的意思是大于或者小于的，类似于>=表示大于后者等于的

  3. select ename, sal from emp where sal >= 1100 and sal<=3000; ， 等价于 select ename, sal from emp where sal between 1100 and 3000 ; between必须左小右大，表示的是闭区间。

  4. between字符串也是可以的，和计组中学的一样的，可以理解为左闭右开。

  5. is NULL和0是不一样的，当一个位置为空时，要使用is null或者is not null去选出对应的选项。用 =null 是错的，没有这种用法哦。

     **这里非常重要，NULL表示任何值，所以首先不能用=，其次，不能用in的语法去进行匹配，任何值都可以匹配上NULL，所以在后面的练习题中，要匹配都要搭配is not null来一起使用。**

  6. and 和 or 洒洒水啦，很简单的 ， 复杂的话就加括号，永远的神！！！不然的话，and的优先级会大于or ，就可能会gg。

  7. in等同于or , usage: `select ename,job from emp where job='salesman' or job='manager'`和`select ename,job from emp where job in ('salesman','manager')`，有点类似于python的感觉。`select ename,job from emp where sal in (1000,5000)`，in里面对应的不是区间，是确定的多个值！！！！！！not in就是去反面，不在这几个值之中。

- **模糊查询**：

  关键字为like，类似的意思，就是模糊查询

  模糊查询必须掌握的特殊符号：%和_

  %代表任意多个字符，_代表任意一个字符

  1. select ename from emp where ename like '%o%'查询名字中有o的
  2. 找出名字中第三个为A的，select ename from emp where ename like '__A%'
  3. 找出名字中有下划线的, select ename from emp where ename like `'%\_%'`

- **排序**

  关键词为order by

1. select ename , sal from emp order by sal; 默认为升序排序，怎么指定升序或者降序？asc表示升序，desc表示降序select ename , sal from emp order by sal desc;

2. 搞心态！！！

   ![image-20210303204033854](https://gitee.com/alexs-rabbit//blog/raw/master/20210303204034.png)

​			按照从左到右顺序来排序，后面的可能都用不上哦。

    3. 还可以用select * from emp order by 1 ， 表示按照第1列排序，但是这种方式不够健壮，所以不推荐用这个。
    
    4. 综合使用：select ename , job , sal from emp where job = 'SALESMAN' order by sal desc; 
    
    5. 顺序： from -> where -> select -> order by

- **分组函数**：

  count , sum , avg , max , min五个分组函数，count为计数，找出个数

  1. select sum(sal) from emp
  2. select max(sal) from emp
  3. select avg(sal) from emp
  4. select count(ename) from emp / select count(*) from emp;

  分组函数一共五个，都是对于一组数据进行处理的，又叫做多行处理函数。

  - 分组函数会自动忽略NULL哦，如果你在对于含有null的列操作的话，那些对应的null的行会被忽略掉
  - 分组函数都会自动忽略null

- **单行处理函数**：

  1. 有null参与的运算最终的结果也是null，所有数据库都是这样规定的。但是发现sum出问题了！！！！sum函数把null忽略了。但是但是但是！！！(800+null)/2的值也是Null哦！！！
  2. ifnull()为空值处理函数，可能为null的数据被当作什么处理
  3. select ename,ifnull(comm,0) as comm from emp
  4. select enames,(sal+ifnull(comm,0)) as comm from emp
  5. 这么叫这个函数，是因为，进去多少行出来多少行，是对于每一行处理的。而多行处理函数则是多对一这种形式哦。

- **特殊的一些规则和用法**：

  1. 找出工资高于平均工资的员工：select ename from emp where sal > avg(sal)  ， 会报错，显示无效使用了分组函数。**SQL语句中有一个规则，分组语句不可以直接出现在where后面！！！**

  2. 这儿就要改！！！

     select avg(sal) from emp;

     select ename , sal from emp where sal > 2073.214286

     =========================>

     select ename , sal from emp where sal > (select avg(sal) from emp) ;   这个是子查询！！！select语句的嵌套

  3. select sum(comm) from emp where comm is not null 没有必要这么写，分组函数自动忽略null

  4. select count(*) from emp; 统计总记录条数，select count(sal) from emp; 记录的是salary的不为null的数据的条数。 两者的区别使用的时候要注意。

  5. 分组函数可以组合使用。select count(*) , avg(sal) from emp;

  6. mysql中，指令是不区分大小写的，但是数据是有大小写的哦，数据的大小写是有区分的。

  7. as是用来重命名的，如果后面为英文不用单引号，如果是中文就要用字符串的形式括起来啦！

- **group by 和 having**:

  group by：按照某个字段或者某些字段进行分组

  having：对于分组之后的数据进行再次过滤

  1. 找出每个岗位的最高薪资： select max(sal) from emp group by job , mention: 分组函数一般都会和group by 联合使用。

  2. **正是因为上面那五个分组函数，很多情况都是与group by一起使用，并且都是在group by以后执行，因此叫分组函数。当一条sql语句没有group by 的话，整张表会自成一组，也是默认要group by的！！！**（先分组，后执行）

  3. 分组函数不能用于where后面，**因为先执行where，然后才执行group by , 最后才执行分组函数**，因此在后面执行不了啊兄dei。（where的时候还没分组，你上哪儿用分组函数啊？？？）

  4. ```sql
     select	5
     	...
     from	1
     	...
     where	2
     	...
     group by	3 //之后才能用分组函数
     	...
     having	4
     	...
     order by	6
     	...
     ```

     指令执行顺序

  5. 记住：where后面绝对不能用分组函数！！！！！嵌套的话可以哦！！！！！！

  6. select ename , max(sal) , job from emp group by job;   这个不行的哈，明显有问题，因为这个ename没有参与分组啊！！！Mysql不报错但是结果没有意义，oracle会报错，这个有隐患的，不要用嗷！！！

  7. 得出结论，group by 出现的话，前面只能跟分组函数和参与分组的字段，别的都不能跟着。

  8. 找出每个部门，不同工作岗位的最高薪资：select deptno , job , max(sal) from emp group by deptno,job ;

  9. 找出每个部门的最高薪资，要求显示薪资大于2900的数据： 

     select max(sal) , deptno from emp group by deptno having max(sal) > 20 -- having是用于group by之后进行信息过滤的。这里用where好一点，不然的话分组了，找了最大值，后面又扔了效率就低了。where在having之前和分组之前执行的，先把没必要的扔了，效率就高了。

     

     select max(sal) , deptno from emp where sal>2900 group by deptno; //这个和上面相比效率高一些

     这里是   对于所有数据不经过分组处理就使用where，对于分组处理前的数据进行筛选是可以用where的！！！

  10. 找出部门的平均薪资，要求显示薪资大于2000的数据

      select deptno , avg(sal) from emp group by deptno having avg(sal) > 2000; 这样就只能用having , 因为where后面不能接avg！！！这里是对于所有数据分组处理后，对于处理后的数据做筛选，这个时候where就失效了，就只能用having了。

  11. having是group by的搭档，是对于group by后的数据进行处理的。没有group by是不能用having的。

- **sql的处理**：

  1. select	5
     	...
     from	1
     	...
     where	2
     	...
     group by	3 //之后才能用分组函数
     	...
     having	4
     	...
     order by	6
     	...
  2. 上面这个顺序编程的时候是不能变的，执行就是先从表中取出，然后用where筛选第一次，分组后，再筛选一次，取出来，排序，执行时这个循序。

- **去除重复记录**：

  1. select **distinct** job from emp;
  2. select ename , **distinct** job from emp;这个是错的，distinct只能出现在所有字段的最前方。表示联合去除重复的记录，将多个字段联合起来去重。
  3. 统计岗位的数量，select count(distinct(job)) from emp;





##### 两表连接

- **连接查询**：

  1. 多张表联合查询出最终的结果，一个业务会对应多张表，数据会有冗余，数据库中尽量不出现重复的数据。

2. 分类：

   - 根据语法出现的年代划分：

     - SQL92
       - SQL99 （这个才是比较新的语法）

   - 根据表的链接方式来划分：

     - 内连接：

       - 等值连接
         - 非等值连接

       - 自连接

     - 外连接：

       - 左外连接（左连接）
         - 右外连接（右连接）

     - 全连接（这个不讲，很少用）
  3. 笛卡尔积现象（笛卡尔乘积现象） e.g.  select ename , dname from emp , dept;  结果是两个表数量的乘积，这个就仅仅是简单的排列组合。定义：当两张表进行连接查询的时候，没有限制，则结果是两张表记录条数的乘积。

  4. 关于表的别名：`select e.ename,d.dname from emp e, dept d;`，这样就可以取别名，方便一些。这儿的意思是e.ename和d.dname要粘贴在一起，进行匹配，这就是原理。
  5. 如何避免？条件过滤！！！这种方式不会减少匹配的次数，只会减少显示的有效记录而已，底层是一样实现的，还是要一一匹配然后去判断。e.g. select e.ename , d.dname from emp e, dept d where e.deptno = d.deptno。匹配次数还是54次，这个是sql 92语法，已经out了，以后不用了。



###### 内连接：

1. 等值连接：
   - 特点：条件是等量关系。
   - SQL92: `e.g.select e.ename , d.dname from emp e, dept d where e.deptno = d.deptno。`
   - SQL99:`e.g.select e.ename , d.dname from emp e join dept d on e.deptno = d.deptno`
   - 99语法：`select ... from ... join ... on ... where ...`，表的连接和连接后的数据过滤分开来了，结构更加清晰一些，表的连接和后面的where分离了。
   - 由于on的条件是等号，因此叫做等值连接，这里的join前面其实少了一个inner , 表示的应该是inner join。带上Inner可读性好一些
2. 非等值连接：
   - 特点：找出每个员工的工资等级：
   - 找出所有员工的工资等级： select e.ename , e.sal , s.grade from emp e join salgrade s on e.sal between s.losal and s.hisal;
   - 逸豪的想法：本质上可以先表拼接，然后再去根据行之间数据的关系去构造非等值连接。
3. 自连接：
   - 特点：一张表看作两张表。自己连接自己
   - 找出员工和他的领导！！！select e.ename as ename , e.mgr  as managerId, s.ename as boss from emp e join emp s on e.mgr = s.empno; 本质就是分别用两个别名表示一样的表即可看作两张表来使用嗷QAQ！！！
   - 记得起别名！！！
   - summary: 这儿要记一下，其实可以看做从A表拿一条data , 从B表拿一条data，根据每一行中某些数据的大小关系，判定要不要取出来。

###### 外连接：

​	什么是内连接？

把A和B表能够匹配上的记录查询出来，这就是内连接。

​	什么是外连接？

AB两张表中一张是主表，一张是附表。当附表中的数据没和主表匹配上，附表会自动生成NULL与之匹配。A是主角，B是附表。

1. 左连接

   - 左边的表是主表

   - 找出每个员工的上级领导！！！`e.g.select e.ename '员工' , s.ename '领导' from emp e left join emp s on e.mgr = s.empno; `

2. 右连接

   - 右边的表是主表
   - 找出每个员工的上级领导！！！`e.g.select e.ename '员工' , s.ename '领导' from emp e right join emp s on e.mgr = s.empno; `
   - summary: 左连接就是左边是主表，右连接右边是主表，同理，outer时可以省略的。能省略是因为重点不在这两个单词，有的话可读性会更好嗷！！！
   - 例题，找出哪个部门没有员工：显然部门是主表嗷！！！`e.g. select e.* , d.* from emp e right join dept d on e.deptno  = d.deptno on e.empno is NULL;`

###### 全连接：

- 情况比较特殊，应用场景较少，业务场景也较少，有兴趣的话自己自学。



##### 多表查询

- 找出每一个员工的部门名称以及工资等级
- 规则： `from A join B on ... join C on ...`（表示A和B先进行连接，连接后的结果再和C连接 ）`select e.ename , d.dname , s.grade from emp e join dept d on e.deptno = d.deptno join salgrade s on e.sal between s.losal and s.hisal;`
- 又有一个例子：找出每一个员工的部门名称，工资等级以及上级领导。`select e.ename , d.dname , s.grade , el.ename '领导' from emp e join dept d on e.deptno = d.deptno join salgrade s on e.sal between s.losal and s.hisal left join emp el on e.mgr= el.empno;`表可以认为从左往右不断迭代的，因此要注意不同的表之间的inner和outer的关系嗷呜！！！



##### 子查询

select里面嵌套select语句，子查询可以出现在：`select...(select) from ...(select) where ...(select)`

- 找出高于平均薪资的员工信息：(where后面加select)

  - 第一步： select  avg(sal) from emp;
  - 第二步：select * from emp where sal > 2073.214286
  - 合并：select * from emp where sal > (select avg(sal) from emp);

- 找出每个部门平均薪水的等级：(from后面加入select)

  - 第一步：select deptno , avg(sal) as avgsal group by deptno;

  - 第二步：

    ```sql
    select
    	t.* , s.grade
    from 
    	(select deptno , avg(sal) as avgsal group by deptno) t
    join salgrade
    	s
    on
    	t.avgsal between s.losal and s.hisal;
    ```

- 找出每个部门平均的薪水等级：(from后面加入select)

  - 第一步：`select e.ename ,  e.sal , s.grade from emp e join salgrade s on s.losal and s.hisal;`

  - 第二步：

    ```sql
    select 
    e.ename ,  e.sal , s.grade , avg(s.grade)
    from 
    emp e 
    join 
    salgrade s 
    on
    e.sal between s.losal and s.hisal
    group by
    e.deptno;
    ```

- 找出每个员工所在的部门名称，要求显示员工名和部门名：(select后面接上select):

  - `select e.ename,e.deptno,(select d.dname from dept d where e.deptno = d.deptno) as dname from emp e;`

##### Union

- 可以将查询结果相加

  ```sql
  select ename , job from emp where job = 'MANAGER'
  union
  select ename , job from emp where job = 'SALESMAN'
  ```

- 可以将毫不相关的结果从下往下拼接起来。



##### **limit(mysql中特有的)**

- 分页查询用得到
- limit取结果集中的部分数据，
- 语法: limit startIndex , length 分别表示起始位置，表示取几个。`e.g. select ename , sal from emp order by sal desc limit 0,5;`先排序，再取5个，也可以直接写limit 5; limit是最后才执行的



##### 通用标准分页sql

- 例如 ，每页显示三条记录：`一页： 0,3  二页：3,3 三页：6,3 四页：9,3`，所以可以看出来，对于第n页有 (pageNo - 1) *pagesize , pageSize

- java代码：

  ```java
  int pageNo = 2
  int pageSize = 10
  
  limit 10 , 10
  ```





#### DDL：

- creat

  - 创建表格的语句格式：

  ```sql
  create table Tabel_Name(
  	Field1 Type1,
  	Field2 Type2,
  	...
  )
  ```

  - 数据类型：

    - int（对应java中的int）

    - bigint   长整型（对应java中的long）

    - float    浮点型（对应java中的float double）

    - char     定长字符串（对应java中的String）

    - varchar     可变长字符串（最多存255个字符，对应java中的StringBuffer/StringBuilder）

    - date     日期类型（存储日期，对应java.sql.Date类型）

    - blob     二进制大对象（存储图片，视频等流媒体信息，Binary Large Object，对应java中的Object）

    - clob     字符大对象（存储大文本，例如 可以储存4G的字符串，Character Large Object对应java中的object）

      ......

  - char和varchar如何选择？明显看长度的关系啦！！！例如Name字段，如果少于Char，没问题，多余Char，爆了。Varchar比char智能一点，Varchar只要不超过限定的范围，底层是可以自动根据你输入的数据来分配空间的。Char的话，分配的空间就定死了。比如一个数据类型的长度始终是固定的，例如生日啊，男女性别啊，就用char , 其他不定的情况就用varchar, 例如简介啊，姓名啊之类的。char的效率比varchar要高嗷！！！

  - clob和blob怎么使用，大文本用CLOB，图片啊之类的数据必须使用BLOB，Java中必须使用IO流才能写入BLOB。图片啊之类的如果大的话就别放在db里面了嗷！

  - 表名再数据库中一般建议以t_或者tbl_开始。

  - 举个例子：创建学生表   学号：bigint , 姓名：varchar , 班级编号：int

  性别：char ,  生日：char

```sql
create table t_student(
	no bigint,
	names varchar(255),
	sex char(1),
	classno varchar(255),
	birth char(10),
);
```

- create另一种用法：（表的复制）
  - `create table emp as select emp.ename from emp;`
  - create table 表名 as select语句，将查询结果当作表创建出来

- 删除表格：
  - drop table 表名;
  - drop table if exits 表名; 如果表存在的话就把表删掉。
- alter table 表名 rename to 新表名 ， 表的重命名
- 对于表的结构的修改这里暂时不讲了，因为实际开发过程中用的比较少，即使修改也可以使用工具，出现在java代码中的数据只有增删改查操作(CRUD) ----> Create Retrieve Update Delete



#### DML：

- insert

  写法一：

  - `insert into t_student values(1, 'Jack', 'o','高三二班,'1996-10-12')`

  写法二：

  - `insert into t_student(name) values('ZhangSan')`，这个东西实际上，会自动把他补上
  - 列的数量必须和给予数据的数量相同，字段省略不写的话，后面的顺序和数量就要和默认表的一样的。

  写法三：

  - `insert into t_student(no,name) values(2,'兔子'),(3,'乐乐') `

- insert

  - `insert into tbl1 select * from dept;`这个不一定可以哦，列数相同才对的上哦。将查询结果当作数据插入到表里。

- update

  - 用于修改条件的时候使用的
  - update 表名 set 字段1=值1 , ... where 条件; 如果不写条件的话就是整整一张表都要更新哦！这里多个条件的设置是用“，”哦，不是用and呢！！！
  - 更新所有记录：`update dept1 set loc = 'x' , dname = 'y'`

- delete

  - 用于删除数据
  - 没有条件会全部删除
  - `delete from dept1 where deptno=10;`
  - 全部删除：delete from dept1;
  - 执行语句比较慢，而且不释放空间，可以回滚，这个是可以追溯的。如果要删的快而且删的彻底的话。
  - truncate table 表名;  表被截断，不可回滚，彻底丢失。这个是把表截断，只留下表头，其他彻底删除。





# 创建表加入约束

- 什么是约束：例如数据不能重复，数据不能为空之类的，约束的目的在于保证表中的数据的合法性，有效性，完整性
- 种类：
  - 非空约束(not null)：约束的字段不能为Null
  - 唯一约束(unique)：约束的字段不能重复
  - 主键约束(primary key)：约束的字段不能为NULL，也不能重复
  - 外键约束(foreign key)：
  - 检查约束(check)：Oracle中有check约束 , mysql目前不支持该约束


## 非空约束

- 一个例子：
  - `create table t_user(id int, username varchar(255) not null, password varchar(255));`
  - 这个只能加在字段后面，是不可以作为表级约束的。

## 唯一性约束

- unique , 修饰的字段具有唯一性，不能重复，但可以为NULL，NULL和NULL是不一样的，可以重复。

- ```sql
  create table t_user(
  	id int,
  	username varchar(255) unique
  )
  ```

- 注意一下，唯一性是可以为NULL的，给多个列添加unique

- 此外，在insert中，可以选择插入的字段，其余的字段会用系统初始的默认值，如果在create表的时候，在字段后面给予了自定义的默认值，则默认的值为你定义的默认值。

- 给多个列加unique:

  - ```sql
    create table t_user(
    	id int,
    	username varchar(255),
    	usercode varchar(255),
    	unique(usercode,username)
    );//这个是两个数据联合起来添加一个约束，就是说这两个不能同时相同
    ```

- ```sql
  create table t_user(	
  	id int,
  	username varchar(255) unique,
  	usercode varchar(255) unique
  );//这个是对于两个字段分别添加约束，每一个都不能重
  ```

- 这里就有两个概念，上面一个是列级约束，上面第二个是表级约束。

- 非空约束只能在字段后面加，往后面加是没有表级约束的。



## 主键约束

- ```sql
  create table t_user(	
  	id int primary key, // 列级约束
  	username varchar(255) unique,
  	usercode varchar(255) unique
  );//主键字段不能为空，也不能重复
  ```

- 主键相关的术语：

  - 主键字段
  - 主键值
  - 主键约束

- 作用：

  - sql设计三范式中有要求，要求任何一张表都有主键

  - 主键的作用是啥？

    - 主键是一行的唯一标识，就像身份证号码一样，只要主键值不一样，就可以认为两条记录完全不一样。

  - 主键的分类：

    - 按照主键字段和数据量：
      - 单一主键（常用到的）
      - 复合主键（不常用，违背三范式，多个字段联合起来添加一个主键约束）
    - 根据主键性质来划分：
      - 自然主键（推荐，这种方式和业务无任何挂钩，推荐）
      - 业务主键：最好主键和其他值不挂钩，比如说id和身份证分开，不用身份证做主键。不然业务一旦发生改变的时候，主键可能也跟着发生变化，但是有时候可能没有办法变化，这就很尴尬。

  - 一个表中主键约束只能有一个

  - 使用标记约束定义主键：

    - ```sql
      create table t_user(
      	id int,
      	username varchar(255),
      	primary key(id)
      );
      ```

    - ```sql
      create table t_user(
      	id int,
      	username varchar(255),
      	primary key(id,username)
      );
      ```

- mysql提供主键值自增：

  - ```mysql
    create table t_user(
    	id int primary key auto_increment,   //id字段自动维护一个自增的字段，从1开始，以1自增
    	username varchar(255)
    );
    ```

  - Oracle中也提供了一个自增机制，叫做sequence（序列）

## 外键约束

- 相关术语：

  - 外键约束：foreign key
  - 外键字段：添加有外键约束的字段
  - 外键值：外键字段中的每一个值

- 业务背景：

  - 设计数据表，用来维护学生和搬家的信息

  - 做法和想法：

    - 放在一张表中（缺点是冗余）
    - 放在两张表中（班级表和学生表）

  - 比如上面两张表要关联起来，我们就需要外键，不加外键约束的话可以随便写，而如果添加了外键的话，写的值就有一定的范围的。

  - t_student中的classno字段引用t_class表中的cno地段，此时t_student表叫做组表，t_class表叫做父表。删除数据的时候先删除子表再删除父表，创建或者添加数据的时候，先添加父表再添加子表。两张表之间有明显的依赖关系。

  - ```sql
    create table t_class(
    	cno int,
    	cname varchar(255),
    	primary key(cno)
    );
    
    create table t_student(
    	sno int primary key auto_increment,
    	sname varchar(255),
        classno int,
        primary key(sno),
    	foreign key(classno) references t_class(cno)
    );
    
    //先插入父表数据，再插入子表数据
    insert into t_class values(101,'xxxxxxxxx');
    insert into t_class values(102,'yyyyyyyyy');
    insert into t_student values('zs1',101);
    ```

- 外键值可以为NULL？

  - `insert into t_student(sno,sname) values(6,'zs6')`
  - 外键可以为NULL

- 外键字段引用别的表的字段的时候，被引用的字段必须是主键否？不一定，但是对应的字段必须要是唯一的。不然对应的外键说不清对应的是哪一个，一般情况下都是要对应主键的，不对应的话至少要有unique约束



# 存储引擎

- 描述的表的存储方式，例如InnoDB , `ENGINE=InnoDB`等

- 完整的建表语句还要加上engine的，例如

  ```sql
  CREATE TABLE `t_x`(
  	`id` int(11) DEFAULT NULL
  )ENGINR=InnoDB DEFAULT CHARSET=utf-8
  ```

  mention: 在mysql中，凡是自己定义的标识符都是可以使用飘号括起来的，最好别用，不是通用的。

  建表的时候使用的引擎是InnoDB，默认的字体为utf-8

- what is engine? 这个名字只有在mysql中存在，oracle中则有对应的机制，但是不叫存储引擎。每一个存储引擎都有自己的优缺点。

- 查看当前mysql支持的存储引擎，`show engines \G;`

- 经典的存储引擎有：

  - InnoDB：
    - 是数据库的默认引擎
    - 优缺点：
      - 优点：支持事务，行级锁，外键。数据存储比较安全。表的结构存储在xxx.frm文件中。在mysql数据库崩溃后自动恢复。支持级联删除，级联更新（父级会影响所有的孩子，开发的时候比较少用）。事务，安全，重量级。
      - 缺点：处理速度不是最快的
  - MyISAM：
    - 之前最常用的存储引擎
    - 采用三个文件表示每个表：
      - 格式文件
      - 数据文件
      - 索引文件
    - 优缺点：
      - 优点：可被压缩，节省存储空间。可压缩，可转换形式，节省空间。
      - 缺点：不支持事务
  - Memory:
    - 优缺点：
      - 缺点：
        - 不支持事务
        - 数据容易丢失，数据和索引都是在内存当中
        - 不能包含TEXT和BLOB
        - 断电后数据就丢失了。
      - 有点：
        - 搜索速度最快







# 事务（Transaction）概述：

- 一个事务是一个完整的业务逻辑单元，不可再分。（相当于原子操作！！！和操作系统就连上了，本质上实现了线程同步！！！）
- 例如：从银行转账，从A账户转B账户10000元，那有转入和转出两条语句。想要保证上面两条操作同时成功或者同时失败，就要使用数据库的“事务机制”。
- 只有DML语句才有事务，其他的语句和事务都没关系，因为只有这三条语句才和数据操作有关。事务时为了保证数据的**完整性，安全性**。
- 假设所有的业务都能使用一条DML解决，那就不需要事务了。只有需要多个DML操作组合成一个事物的时候，才需要事务操作。通常一个事务多条DML才需要操作。
- 本质相当于将多条DML语句打包成原子操作！！！
- 原理，how to control ？
  - 开启事务机制：
    - 执行insert，先执行保存到操作历史中，不会操作硬盘上的数据
    - 执行update，先执行保存到操作历史中，不会操作硬盘上的数据
    - 执行delete，先执行保存到操作历史中，不会操作硬盘上的数据
    - 最后提交或者回滚事务：（本质就是操作绑定嘛！！！）
      - 提交：将历史操作更改同步到硬盘文件当中，并将历史操作清空。（成功）
      - 回滚：直接清空历史记录，不同步到硬盘当中。（失败）
    - 提交事务：commit , 回滚事务：rollback

### TCL:

- commit-提交
- rollback-回滚
- savepoint: 保存点，可以滚到某一个点，相当于保存游戏进度。

### 事务的特性：

- 原子性：事务是最小的工作单元，不可再分。(A)
- 一致性：事务必须保证多条DML语句同时成功或者同时失败。(C)
- 隔离性：事务A与事务B之间具有隔离。（隔离级别的概念，级别不一样，影响效果不同。）(I)
- 持久性：持久性说的是数据最终必须持久化到硬盘文件中，事务才算成功的结束。(D)

### 事务的隔离性：

- 隔离级别：

  - 理论上包括四个：
    - 第一级别：读未提交(read uncommitted)：别人未提交的事务的我们当前事务能够读到。存在脏读（Dirty Read）现象，读到了脏的数据。
    - 第二级别：读已提交(read committed)：对方提交的事务我们才能读到。解决了脏读的问题，但是存在不可重复读的现象，别人改了我的数据的话，每次读到的都是最新的数据，出来的结果是不一样的。
    - 第三级别：可重复读(repeatable read)：解决了不可重复读的问题。但是存在的问题是，读取到的数据是幻象。每次读的数据都是备份数据，所以都是相同的，存在读的是假数据的问题。
    - 第四级别：序列化读/串行化读(serializable)，解决了所有问题。但是效率低，需要事务排队。
  - mysql默认是第三级别，oracle默认是第二级别。

- mysql事务默认是自动提交的，就是执行一条DML，自动提交一次。

- how to close自动提交：

  - start transaction ;  //这个会开启事务，关闭自动提交机制。

  - 演示：

    ```sql
    drop table if exists t_user;
    create table t_user(
    	id int primary key auto_increment,
        username varchar(255)
    );
    
    rollback; //这个是回不去的，自动提交了。。。
    ```

  - 感觉有点像git的使用！！！

  - rollback等命令执行完之后，事务就结束了，如果还要写事务，还记得要加start transaction，这个就类似于开启git功能？？？

- savepoint name; 设置保存点，再输入rollback name; ， 这就可以滚回提交点了。

- 更改隔离级别：`set global transaction isolation level read uncommitted;`其他的用法都是一样的，只是后面改个名字而已。

- 查看事务的全局隔离级别：` select @@global.transaction_isolation;`

- 可以同时打开两个终端显示操作来演示嗷！！！每种都有自己各自符合的方式嗷！！！

















# 索引

- 什么是索引，有什么用：
  - 相当于一本书的目录，通过目录可以快速找到对应的资源。数据库方面，一张表的查询有两种方式：
    - 全表扫描
    - 根据索引检索（效率很高）
  - 第二种由于缩小了扫描的范围，因此可以提高检索的效率。索引虽然效率高，但是不能随意添加索引，因为索引是数据库中的数据，也是有维护成本的。
  - 索引是给某一个字段，或者说某些字段添加索引。

- 什么时候考虑添加索引呢？
  - 数据量庞大。（根据客户的要求，根据线上的环境）
  - 该字段很少的DML操作。（因为字段进行修改操作，索引也需要维护）
  - 该字段经常出现在where子句中。（经常根据哪个字段查询）
- 注意：主键和具有unique约束的字段会自动添加索引。
- 普通的select语句，where后面的操作都是全表扫描
- `explain select ...`这个后面跟着会显示类似于type之类的信息。可以查看mysql中语句的执行计划

## 添加索引：

- `craete index emp_sal_index on emp(sal);`
- 底层用的是B-Tree数据结构，查询效率很高。
- 再去查看查询的条数之类的，发现明显变少
- `create index index_name on table_name(字段);`



## 删除索引：

- `drop index index_name on table_name;`









## 底层数据结构：

- B+Tree
- 底层原理：
  - 当字段上没有索引的时候，全表扫描，效率较低
  - 处理：
    - 首先，索引会对于记录进行排序
    - 接下来，会在硬盘文件或内存（不同的引擎不同）生成索引对象
    - 然后，索引会分区，例如人名的话，就会用名字字母分区，储存为B+树。
  - select的时候，首先查看ename字段有没有对应的索引，如果有的话，根据索引对于数据进行定位，缩短了扫描的数量，很快定位到了。oracle中物理地址就是rowid。这个树中，还携带了物理地址，根据索引对应的物理地址，拿到物理地址对应的对象，不走表了，直接根据地址拿到数据。（二分查找）
- 索引不是万能的
- 总结：B Tree缩小了扫描范围，底层索引进行了排序，分区，索引携带“物理地址”。通过索引检索到数据，获得关联的物理地址，通过物理地址，定位表中的数据，效率是最高的。

## 分类：

- 单一索引：单个字段添加索引
- 复合索引：多个字段联合添加索引
- 主键索引：主键自动添加索引
- 唯一索引：有unique约束的字段自动添加索引



## 索引失效：

- 模糊查询的时候，例如通配符，"%A%"没办法用B树去索引，第一个尽量不要使百分号。这样会造成索引失效。







# 视图(view)

- what is view?
  - 站在不同的角度去看到数据
- how to create / delete view?
  - create view view_name as select empno,ename from emp; 
  - 对于视图的增删改查，会影响到原表的数据，但是是通过视图来实现的。（不是直接操作原表）
  - 只有DQL语句才能创建视图，但是对于视图可以CRUD操作。
- 为啥要引入视图呢？
  - 保密，只给你部门的数据，甚至重命名，对应的数据也可以不给你的，但是对于视图的操作，可以直接作用到原表上。
  - 视图可以隐藏表的实现细节，适用于保密级别较高的系统。



# 数据库的数据的导入导出

- DBA命令：导入，导出（被封）

## 导出数据：

Windows中的dos命令窗口：

- `mysqldump sql-name>某个盘\test.sql -uroot -ppassword`

## 导入数据：

- create database ...;
- use databasename;
- source sql文件

## 导出指定表：

- `mysqldump databaseName tableName>.sql文件 -uroot -ppassword`



# 数据库设计三范式

- what?
  - 设计表的依据，这么设计不会出现冗余
- 是哪些？
  1. 任何一张表都应该有主键，并且每一个字段的原子性不可再分。
  2. 在一范式基础上，所有非主键字段必须完全依赖主键，**不能产生部分依赖**。（有可能出现数据冗余，不推荐复合主键）。
  3. 在二范式基础上，所有非主键关系直接依赖主键字段，**不能产生传递依赖**。（例如班级和班级序号，这就产生了传递依赖，传递依赖本质就是出现了数据冗余。）
- 两条口诀：
  - 多对多，三张表，关系表，两外键。
  - 一对多，两张表，多的表，加外键。
- 实际开发中，以满足客户需求为主，有的时候会拿冗余换执行速度。

- 一对一如何设计？？？
  - 有可能有两张表，例如用户信息，可以把详细信息拆分为用户名，密码，与详细信息表。
  - 两种设计方案：
    - 主键共享：
      - pk+fk：保证了一对一
    - 外键唯一：
      - fk+unique约束：保证了一对一



































# 练习题：

1. 取出每个部门最高薪水的人员名称

   - 第一，取得每个部门的最高薪水

     - `select deptno , max(sal) as maxsal from emp group by deptno;`

   - 第二，通过以上的表取出最高薪水的人员名称：

     - ```sql
       select e.ename , t.*
       from
       emp e
       join
       (select deptno , max(sal) as maxsal from emp group by deptno) t
       on
       e.sal = t.maxsal and e.deptno = t.deptno;
       ```

2. 哪些人的薪水在部门平均薪水之上

   - 第一，求出每个部门的平均薪水：

     - `select deptno , avg(sal) from emp group by deptno;`

   - 通过以上的零食表构建关系，找出高于平均工资的人:

     - ```sql
       select e.deptno , e.ename , e.sal , t.avgsal from emp e
       join 
       (select deptno , avg(sal) avgsal from emp group by deptno) t
       on
       e.deptno = t.deptno and e.sal > t.avgsal;
       ```

3. **取得部门中所有人的平均的薪水等级**

   - 第一，求出所有人的薪水等级

     - ```sql
       select 
       e.deptno , avg(t.grade)
       from 
       emp e
       join 
       salgrade t
       on
       e.sal > t.losal and e.sal < t.hisal
       group by
       e.deptno;
       ```
       
     
   - 第二，这儿连接成一张表之后就很方便可以继续group by，没有必要再当成一张临时表去操作啦！

   - ![image-20210310172059646](https://i.loli.net/2021/03/10/MIJV8LYKbk1sTwn.png)

4. 不用组函数(Max)，取得最高薪水（给出两种方式）

   - 第一种：`select ename,sal from emp order by sal desc limit 1;`(降序排序+limit取出第一个)
   - 第二种：用max
   - 第三种：表的自连接：`select distinct a.sal from emp a join emp b on a.sal < b.sal;`(只要b表中存在比a表中大的工资，都会被找出来！！！然后distinct，换句话说，没被找出来的，就是最大工资！！！)`select sal from emp where sal not in (select distinct a.sal as hehe from emp a join emp b on a.sal < b.sal);` 这儿也说明，和一列比较是和每一个值分别比较，尤其是in和not in这种，和列名无关。
   
5. 取得平均薪水最高的部门的部门编号

   - 取得部门的平均薪水,  然后降序选第一个 ， 用limit求出最大值：

     - `select deptno , avg(sal) as avgsal from emp group by deptno order by avgsal desc limit 1;`
     - 作为临时表查找最高的avg(sal)

   - ```sql
     (select deptno , avg(sal) as avgsal from emp group by deptno) t //找出每个部门平均薪资
     ```

     - `select max(avgsal) from (select deptno , avg(sal) as avgsal from emp group by deptno) t;`//找出最大值
     - `select deptno,avg(sal) as avgsal from emp group by deptno having avgsal = (select max(avgsal) from (select deptno , avg(sal) as avgsal from emp group by deptno) t);`//使用分组后过滤。

   - 查出平均工资表后用自连接就直接找的呀，一个的avgsal = 第二个的max(avgsal)

6. 取出平均薪水最高的部门名称

   - ```sql
     select 
     e.deptno , avg(e.sal) as avgsal ,d.dname
     from 
     emp e 
     join 
     dept d
     on
     e.deptno = d.deptno
     group by 
     deptno 
     order by 
     avgsal 
     desc 
     limit 1;
     ```

7. 求平均薪水等级最低的部门的部门名称

   - 求出每个部门的平均薪水等级：

     - ```sql
       select
       e.deptno , avg(s.grade) as avggrade
       from
       emp e
       join
       salgrade s
       on
       e.sal > s.losal and e.sal < s.hisal
       group by
       e.deptno
       order by
       avggrade asc
       limit
       1;
       ```

   - 上面就是答案，表连接实际上只是把表补全了而已，from...和join...on实际上就是一个新的表了，根据新的表，我们对于deptno进行分组，求出平均薪资。**对于avg(sal)进行排序，然后取出最小值。**

   - 如果多个值的话，可能就要用having avg(sal) = ***，这种形式来过滤出最小值。

8. 取得比普通员工的最高薪水还要高的领导人的姓名

   - 找出普通员工（员工编号没有在mgr字段里面）
     - `select * from emp where empno not in (select distinct mgr from emp where mgr is not null);`
     - not in使用过程中，记得排除掉里面的NULL
     - distinct在select后面才能使用
     - not in后面和一列比较，这一列要先用select提取出来，有点像固定搭配的感觉。
   - 找出普通员工的最高薪水
     - `select max(sal) from emp where empno not in (select distinct mgr from emp where mgr is not null);`
   - 找出薪资高于1600的
     - `select * from emp where sal > (select max(sal) from emp where empno not in (select distinct mgr from emp where mgr is not null));`
   - ！！！比普通员工最高薪水还要高的领导，比普通员工最高薪水还要高的一定不是普通员工。

9. 取得薪水最高的前五名普通员工

   - 先取出员工：

     ```sql
     select * from
     emp
     where
     empno 
     not in
     (select mgr from emp where mgr is not null);
     ```

   - 取出薪资最高的前五名：

     ```sql
     select sal , ename from
     emp
     where
     empno 
     not in
     (select mgr from emp where mgr is not null)
     order by
     sal
     desc
     limit 5;
     ```

10. 取出薪水最高的第六到第十名员工

    - ```sql
      select 
      sal , ename
      from
      emp
      order by
      sal
      desc
      limit 5,5;
      ```

    - 这儿注意下，limit后面是没有括号的，而且下表是从0开始的，5在这里表示第六个数。



