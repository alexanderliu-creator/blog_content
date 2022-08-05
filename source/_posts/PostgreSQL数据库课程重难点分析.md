---
title: PostgreSQL数据库课程重难点分析
date: 2021-03-21 10:52:19
tags:
	Database
---





# 由于已经有mysql的基础，PostgreSQL的中心会放在于Mysql不同的DBMS语法上，重点记录图形化界面PgAdmin的使用和一下不同的语法特点。



<!--more-->





# PgAdmin图形界面



## 整体结构

- 数据库的创建：

![image-20210320204610659](https://i.loli.net/2021/03/20/3BxypZlVuFjQefi.png)

- 表，约束的相关查看方式：

![image-20210320204737520](https://i.loli.net/2021/03/20/BEZSLq7gPleh4Hi.png)

表的索引，约束等相关信息在这里都可以看的。

- 创建表和修改表的时候，在图形化界面中大多都是采用属性相关的内容添加：

![image-20210320204930655](https://i.loli.net/2021/03/20/yM1Fo3QjbWXxght.png)



- SQL语句的执行：

![image-20210320205252944](https://i.loli.net/2021/03/20/ztZaG6cBhLElWXv.png)



## 表的创建

- 注意数据类型

![image-20210320205957125](https://i.loli.net/2021/03/20/AZJE2Y1keOiVByF.png)



MySQL中的`varchar`和`char`对应的就是这些



- 约束

![image-20210320210144204](https://i.loli.net/2021/03/20/GLYpsnCEgufH4ti.png)



所有的约束在这个里面定义嗷，可以加名字呀，编辑呀，删除呀。特点是，可以给SQL中的约束取名字，在创建和删除约束的时候会比较方便嗷！

- 约束后期添加也是可以的嗷

![image-20210320210550818](https://i.loli.net/2021/03/20/OmYG18DrHiU7owc.png)



这个里面实际上就直接选择关联列就可以了

行动处：

![image-20210320210657209](https://i.loli.net/2021/03/20/96f12xPz5YlyHXK.png)

可以定理级联更新和级联删除

![image-20210320210731645](https://i.loli.net/2021/03/20/z6Tkp7wG1ifgUKC.png)

定义完成后点击加号才有作用，完成后点击保存更改成功。





- 操作完成之后，刷新一下才有效果嗷！

![image-20210320211329675](https://i.loli.net/2021/03/20/6j4r5eJoNXiHqQb.png)





# PostgreSQL语法

- 在MySQL中应该也是一样的，我还没有测试，`create table STUDENT`和`create table "STUDENT"`，前者忽略大小写，后者则不忽略，我猜想的结论是：只要没有括号，语句也好，名字也好，全部都是按小写处理执行的，但是打了双引号的我们则遵循字符串的本身的样式。

## 相同的语法：

- `create database name;`

- ```sql
  create table tableName(
  	columnName	dataType	primary key,
  );
  ```

- `select * from tableName;`

- `drop table tableName`

- `insert into Student(name) values('刘兔兔')`

- `delete from tableName where xxx`，如果不加where整张表删除

- `update student set email='xxx' where studentid =yyy;`



## 不同的语法：

### Alter语法：

- 这种语法在PostgreSQL中常用，对于表和表级约束操作往往是alter table , 对于列级约束操作alter table alter column...

- 对于数据库的操作，例如`alter database originalName rename to newName; `发现又一个问题改不了？？？

  ![image-20210320212939965](https://i.loli.net/2021/03/20/ImkrsCt9Z7HJ6Pp.png)

- `select pid from pg_stat_activity where DATNAME = 'test2';`命令行中用这个可以查询到链接test2的进程，有这个进程链接肯定是改不了的，先断开才能改嗷！

- 放弃了，那个是后台线程关不掉，算了不改了orz。

### 添加约束：

- check约束，这个是mysql中没有的：

```sql
CourseType varchar(10) null check(CourseType in('基础课',"专业课","选修")),
```

- 添加**表级约束**：

```sql
create table Register
(
	courseregid serial not null,
    courserplanid int not null,
    studentid char(13),
    note varchar(30),
    constraint courseregid_pk primary key(courseregid),
    constraint courseplanid_fk foreign key(courseplanid)
    references plan(courseplanid)
    on delete cascade,
    constraint studentid_fk foreign key(studentid)
    references student(studentid)
    on delete cascade
);
```

以constraint为开头的关键字嗷！！！！后面接着是表级约束的名字！！！再后面根据约束的不同来指定不同的行（例如主键，外键等），若是外键，再向外指定连接的表和设定级联关系即可！！！

**看的很清楚主键是表级约束，就算primary key没有写成表级约束而是跟在列的后面，它依然是表级约束嗷！！！**

### **数据库表修改SQL语句**：

- 要注意哈，在pgadmin右键点开database输入语句时，实际上已经选中了数据库了，后面才是对于表的操作。不管你在哪个表打开query的，本质上你还是在这个数据库这一级。

#### 删除列：

![image-20210320215357963](https://i.loli.net/2021/03/20/LoIUCjw4QZVFMhf.png)

#### 添加列：

![image-20210320215544537](https://i.loli.net/2021/03/20/phnt27TbmXwWNYx.png)

注意哈，这个添加列，没有`column`！！！后面加上约束也不用`constraint`，直接`列名   类型   约束`按照顺序写就行了QAQ

#### 删除表级约束：

![image-20210320215809283](https://i.loli.net/2021/03/20/jiaf15qzV6nL3SQ.png)

#### 添加表级约束：

![image-20210320221834803](https://i.loli.net/2021/03/20/T2j1GHR53zY4raX.png)



![image-20210320224433350](https://i.loli.net/2021/03/20/E2aWrhoRnPCtHeA.png)

#### 更改表名：

![image-20210320215922748](https://i.loli.net/2021/03/20/J3qNFfl6omvSicB.png)

#### 更改列名：

![image-20210320220117719](https://i.loli.net/2021/03/20/RaiSLdDQt5WcoIk.png)

注意一下，rename原列名to新列名，没有column！！！好像只有在丢掉列的时候才有列名？？？

- 和上面对比，一个是`rename to...`，另外一个是`rename 列名 to ...`

#### 创建索引：

`create index indexName on tableName columnName;`

#### 删除索引：

`drop index indexName;`

#### 修改索引名字：

`alter index indexName rename to newName`

#### 查询语句新结构：

`select xxx,yyy,zzz into <newTableName> where ggg`;

可以把查询结果保存到一张新的表内！



#### 删除列级约束：

![image-20210320221753310](https://i.loli.net/2021/03/20/sFEQMaJxZkPK8XC.png)

#### 添加列级约束：

![image-20210320224052877](https://i.loli.net/2021/03/20/QuYRzKAdCSn8wLx.png)