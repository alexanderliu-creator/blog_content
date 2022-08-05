---
title: MySQL数据库高级
date: 2021-08-21 19:16:05
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821192227.png
---



# Mysql优化先康康嗷，我裂开来，Docker看的脑阔疼，学完这个再去学K8S嗷！！！



<!--more-->

## Mysql的Linux版的安装：

[Mysql安装具体视频嗷！！！](https://www.bilibili.com/video/BV1KW411u7vy?p=3&spm_id_from=pageDriver)

[知乎安装教程以及资源](https://zhuanlan.zhihu.com/p/165312150)

[Ubuntu20.04中Mysql修改用户密码，yyds！！！](https://blog.csdn.net/qq_26164609/article/details/106881079)

- 还有什么开机自动启动啊，字符集啊，安装位置验证啊等一系列的验证和配置工作嗷！！！
- Mysql相应目录详解：

![image-20210822090959714](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822091000.png)

```shell
root@alexander-virtual-machine:/usr/bin# ls | grep mysql
mysql
mysqladmin
mysqlanalyze
mysqlbinlog
mysqlcheck
mysql_config_editor
mysqld_multi
mysqld_safe
mysqldump
mysqldumpslow
mysqlimport
mysql_migrate_keyring
mysqloptimize
mysqlpump
mysqlrepair
mysqlreport
mysql_secure_installation
mysqlshow
mysqlslap
mysql_ssl_rsa_setup
mysql_tzinfo_to_sql
mysql_upgrade
```



- 修改字符集：
  - 大问题啊兄弟！！！
  - 先复制一份出来，再去修改，不然你昨晚的时间就白浪费了orz。

```shell
root@alexander-virtual-machine:/usr/bin# cd /usr/share/mysql
root@alexander-virtual-machine:/usr/share/mysql# ls
bulgarian                    japanese
charsets                     korean
czech                        mysqld_multi.server
danish                       mysql-log-rotate
debian_create_root_user.sql  mysql-systemd-start
dictionary.txt               norwegian
docs                         norwegian-ny
dutch                        polish
echo_stderr                  portuguese
english                      romanian
estonian                     russian
french                       serbian
german                       slovak
greek                        spanish
hungarian                    swedish
innodb_memcached_config.sql  ukrainian
install_rewriter.sql         uninstall_rewriter.sql
italian

```

- 配置文件处于：

```shell
root@alexander-virtual-machine:/etc# cd mysql/
root@alexander-virtual-machine:/etc/mysql# ls
conf.d  debian.cnf  debian-start  my.cnf  my.cnf.fallback  mysql.cnf  mysql.conf.d
```

中的my.cnf嗷！！！

修改Mysql的默认编码哈！！！

查看编码：

```shell
mysql> show variables like '%char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8mb4                    |
| character_set_connection | utf8mb4                    |
| character_set_database   | utf8mb4                    |
| character_set_filesystem | binary                     |
| character_set_results    | utf8mb4                    |
| character_set_server     | utf8mb4                    |
| character_set_system     | utf8mb3                    |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.02 sec)
```

可以看出，上面的默认都是utf-8嗷！！！

- 在修改字符集编码的时间点之前创建的库，字符集可能就固定了嗷。在修改字符集编码的时间点之后创建的库，才能够按照全新配置文件来创建呀！！！注意一下，更改完配置文件之后，要将mysql重启，才能够起到更新的目的嗷！！！



- 主要配置文件：

  - ![image-20210822100359027](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822100359.png)

  上面这个就是一系列的主要配置文件嗷！！！

  - ![image-20210822100253836](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822100254.png)
  - frm是图书馆，myd就是书，myi就是书的编号嗷！！！





## 初识Mysql架构：

- Mysql架构可以在不同的场景中应用并发挥良好的功能，主要体现在存储引擎的架构上。**插件式的存储引擎将查询处理和其他的系统任务以及数据的存储提取相分离**。
- ![image-20210822102226253](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822102227.png)
- 出问题的时候，根据不同的层去做不同的优化行为和优化措施嗷！！！

- 具体分层：

  1. 连接层：

  ![image-20210822102411285](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822102411.png)

  2. 服务层：

  ![image-20210822102439995](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822102440.png)

  3. 引擎层：

  ![image-20210822102516665](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822102516.png)

  最常用就是：InnoDB和MyISAM

  4. 存储层：

  ![image-20210822102533218](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822102533.png)

- 查看搜索引擎：

```shell
mysql> show engines;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)
```

![image-20210822103824461](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822103824.png)



```shell
mysql> show variables like '%storage_engine%';
+---------------------------------+-----------+
| Variable_name                   | Value     |
+---------------------------------+-----------+
| default_storage_engine          | InnoDB    |
| default_tmp_storage_engine      | InnoDB    |
| disabled_storage_engines        |           |
| internal_tmp_mem_storage_engine | TempTable |
+---------------------------------+-----------+
4 rows in set (0.00 sec)
```

上面可以查看默认搜索引擎嗷！！！



### MyISAM和InnoDB的区别：

![image-20210822104036712](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822104037.png)

- 阿里巴巴大部分Mysql数据库其实使用的percona的原型加以修改。（把InnoDB给改了orz）
- AliSql和AliRedis很猛嗷！！！



# 索引优化：

## 优化分析：

### 性能下降SQL慢：

- select语句写的烂
- 索引失效（建了索引没用上）：
  - 单值索引：
    - 只用了该表的某一个字段建立索引
  - 复合索引：
    - 使用多个字段建立索引
- 关联查询的join太多了（设计缺陷或者不得已的需求）
- 服务器调优以及各个参数设置（缓冲，线程数等）



## 常见通用的Join查询：

- 手写和机读是不一样的嗷！！！
- Mysql是先从FROM开始执行SQL的嗷！！！

![image-20210822105653647](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822105653.png)

![image-20210822105756292](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822105756.png)



### 所有的七种Join关系：

1. ![image-20210822105949195](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822105949.png)

`select * from table A inner join table B on A.Key = B.Key`

2. ![image-20210822110104066](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822110104.png)

`select * from table A left join table B on A.Key = B.Key`

3. ![image-20210822110411946](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822110412.png)

`select * from table A right join table B on A.Key = B.Key`

4. ![image-20210822110754207](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822110754.png)

`select * from table A left join table B on A.Key = B.Key where B.Key is NULL`

5. ![image-20210822111229659](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822111229.png)

`select * from table A right join table B on  A.Key = B.Key where A.Key is NULL`



6. ![image-20210822111318303](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822111318.png)

`select * from table A full outer join table B on A.Key = B.Key`



7. ![image-20210822111444731](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210822111445.png)

`select * from table full outer join table B on A.Key = B.Key where A.Key is NULL or B.Key is NULL`



### 七种Join关系的编写：

1. Inner join：

```mysql
mysql> select * from tbl_emp a inner join tbl_dept b on a.deptId = b.id;
+----+------+--------+----+----------+--------+
| id | name | deptId | id | deptName | locAdd |
+----+------+--------+----+----------+--------+
|  1 | z3   |      1 |  1 | RD       | 1      |
|  2 | z4   |      1 |  1 | RD       | 1      |
|  3 | z5   |      1 |  1 | RD       | 1      |
|  4 | w5   |      2 |  2 | HR       | 12     |
|  5 | w6   |      2 |  2 | HR       | 12     |
|  6 | s7   |      3 |  3 | MK       | 13     |
|  7 | s8   |      4 |  4 | MIS      | 14     |
+----+------+--------+----+----------+--------+
7 rows in set (0.00 sec)
```

- 中间只有id相等的才同时出现嗷！！！我们获得是两者相等的公有部分嗷！！！

2. left join：

```mysql
mysql> select * from tbl_emp a left join tbl_dept b on a.deptId = b.id;
+----+------+--------+------+----------+--------+
| id | name | deptId | id   | deptName | locAdd |
+----+------+--------+------+----------+--------+
|  1 | z3   |      1 |    1 | RD       | 1      |
|  2 | z4   |      1 |    1 | RD       | 1      |
|  3 | z5   |      1 |    1 | RD       | 1      |
|  4 | w5   |      2 |    2 | HR       | 12     |
|  5 | w6   |      2 |    2 | HR       | 12     |
|  6 | s7   |      3 |    3 | MK       | 13     |
|  7 | s8   |      4 |    4 | MIS      | 14     |
|  8 | s9   |     51 | NULL | NULL     | NULL   |
+----+------+--------+------+----------+--------+
8 rows in set (0.00 sec)
```

- 左表全都有，右表没有的补上NULL

3. right join：

```mysql
mysql> select * from tbl_emp a right join tbl_dept b on a.deptId = b.id;
+------+------+--------+----+----------+--------+
| id   | name | deptId | id | deptName | locAdd |
+------+------+--------+----+----------+--------+
|    1 | z3   |      1 |  1 | RD       | 1      |
|    2 | z4   |      1 |  1 | RD       | 1      |
|    3 | z5   |      1 |  1 | RD       | 1      |
|    4 | w5   |      2 |  2 | HR       | 12     |
|    5 | w6   |      2 |  2 | HR       | 12     |
|    6 | s7   |      3 |  3 | MK       | 13     |
|    7 | s8   |      4 |  4 | MIS      | 14     |
| NULL | NULL |   NULL |  5 | FD       | 15     |
+------+------+--------+----+----------+--------+
8 rows in set (0.00 sec)
```

- 右表全都有，左表没有的补上NULL。左右表如果有多个匹配上的要全排列嗷！！！
- 最好理解为加法，inner join加上左边或者右边没有匹配上的嗷！！！

4. left join + null：

```mysql
mysql> select * from tbl_emp a left join tbl_dept b on a.deptId = b.id where b.id is null;
+----+------+--------+------+----------+--------+
| id | name | deptId | id   | deptName | locAdd |
+----+------+--------+------+----------+--------+
|  8 | s9   |     51 | NULL | NULL     | NULL   |
+----+------+--------+------+----------+--------+
1 row in set (0.00 sec)
```

5. right join + null：

```mysql
mysql> select * from tbl_emp a right join tbl_dept b on a.deptId = b.id where a.deptId is null;
+------+------+--------+----+----------+--------+
| id   | name | deptId | id | deptName | locAdd |
+------+------+--------+----+----------+--------+
| NULL | NULL |   NULL |  5 | FD       | 15     |
+------+------+--------+----+----------+--------+
1 row in set (0.00 sec)
```



6. FULL OUTER JOIN：

- 吐了兄弟，上面这种语法Oracle支持，Mysql不支持嗷！！！
- 拼一拼呗~

```mysql
mysql> select * from tbl_emp a left outer join tbl_dept b on a.deptId = b.id
    -> union
    -> select * from tbl_emp a right outer join tbl_dept b on a.deptId = b.id;
+------+------+--------+------+----------+--------+
| id   | name | deptId | id   | deptName | locAdd |
+------+------+--------+------+----------+--------+
|    1 | z3   |      1 |    1 | RD       | 1      |
|    2 | z4   |      1 |    1 | RD       | 1      |
|    3 | z5   |      1 |    1 | RD       | 1      |
|    4 | w5   |      2 |    2 | HR       | 12     |
|    5 | w6   |      2 |    2 | HR       | 12     |
|    6 | s7   |      3 |    3 | MK       | 13     |
|    7 | s8   |      4 |    4 | MIS      | 14     |
|    8 | s9   |     51 | NULL | NULL     | NULL   |
| NULL | NULL |   NULL |    5 | FD       | 15     |
+------+------+--------+------+----------+--------+
9 rows in set (0.00 sec)
```

- 相当于左边一整个圆加上右边一整个圆，然后把中间公共部分给去重而已嗷！！！Union就有这个功效嗷，天生自带拼接去重嗷！！！！！



7. A独有的 + B独有的：

- 这个和上面这个十分类似嗷！！！用Union进行拼接而已嘛！！！

```mysql
mysql> select * from tbl_emp a left outer join tbl_dept b on a.deptId = b.id where b.id is null
    -> union
    -> select * from tbl_emp a right outer join tbl_dept b on a.deptId = b.id where a.deptId is null;
+------+------+--------+------+----------+--------+
| id   | name | deptId | id   | deptName | locAdd |
+------+------+--------+------+----------+--------+
|    8 | s9   |     51 | NULL | NULL     | NULL   |
| NULL | NULL |   NULL |    5 | FD       | 15     |
+------+------+--------+------+----------+--------+
2 rows in set (0.00 sec)
```

- 和拼积木一样嗷，第一条就是左半边，第三条就是右半边，第二条和胶水一样拼接起来嗷！！！



## 索引简介：

- 什么是索引？

  - 官方定义：Index是帮助Mysql**高效获取数据的数据结构**。
    - 例如查找mysql , 先找m，再找到y，接着找sql
  - **可以理解为“排好序的快速查找数据结构”**
  - 索引两大功能：
    - 查找
    - 排序
  - 二叉查找树：

  ![image-20210823110116008](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823110116.png)

  - B树：

  ![image-20210823110814670](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823110815.png)

- 优势：

  - 提高数据检索的效率，降低数据库的I/O成本。
  - 对于数据进行了排序，降低数据排序的成本，降低了CPU的消耗。

- 劣势：

  - 索引本质上也是一张表，要占空间的嗷！！！
  - 降低了更新表的速度，因为要保存数据的同时，也要更新索引的字段。
  - 花时间研究建立最优秀的索引嗷！！！



- 索引分类：
  - 单值索引：
    - 一个索引包含单个列，一个表可以有多个单列索引。（一张表建议不超过5个索引嗷！！！）
  - 唯一索引：
    - 索引列的值必须唯一，允许有控制。（unique）
  - 复合索引：
    - 一个索引包含多个列



- 基本语法：

![image-20210823111811202](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823111811.png)



### Mysql索引结构：

- 结构分类：
  - **BTree索引**
  - Hash索引
  - full-text全文索引
  - R-Tree索引
- 磁盘索引原理：

![image-20210823115911931](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823115912.png)

![image-20210823115946944](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823115947.png)

就是操作系统里面介绍的，非叶子结点相当于存了参考值和指针。叶子结点存的才是数据嗷！！！



## 建索引：

1. 主键自动建立唯一索引
2. 频繁作为查询条件的字段
3. 查询中和其他表关联的字段，外键关系建立索引
4. 频繁更新的字段不适合建索引
5. Where里面用不到的字段不创建索引嗷！
6. 高并发下倾向于创建组合索引嗷！！！
7. 排序字段如果通过索引取访问将大大提高排序速度嗷！！！
8. 查询中统计或者分组字段（内部也要用到排序嗷！！！）



## 不建索引：

1. 表记录太少（300万条数据出现性能下降）
2. 表经常需要增删改查！
3. 数据重复并且平均的表字段，注意，如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果嗷！！！（数据的差异率不高就没有必要建立嗷！！！）

![image-20210823125742855](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823125743.png)

# 性能分析：

- MySQL Query Optimizer
- MySQL常见瓶颈：
  - CPU负担重
  - IO负担重
  - 服务器硬件的性能瓶颈



## Explain：

- 用法：`explain + SQL语句`
- 能干吗？

![image-20210823161003394](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823161003.png)







### 字段解释：

```mysql
mysql> explain select * from tbl_dept;
+----+-------------+----------+------------+------+---------------+-----
| id | select_type | table    | partitions | type | possible_keys | key 
+----+-------------+----------+------------+------+---------------+-----
|  1 | SIMPLE      | tbl_dept | NULL       | ALL  | NULL          | NULL
+----+-------------+----------+------------+------+---------------+-----
1 row in set, 1 warning (0.00 sec)
```



- Id：

  - select查询的序列号，包含一组数字，表示查询中执行select子句或操作表的顺序

  - 三种情况：

    1. ID相同，执行顺序是从上到下嗷！！！

    ![image-20210823161520148](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823161520.png)

    2. ID越大，越先被执行嗷！

    ![image-20210823161903021](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823161903.png)

    3. ID相同不同，同时存在：

    ![image-20210823162308690](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823162308.png)

    遵循上面的规则，数字大的先走，数字相同的从上往下执行。

- select_type：

  - simple：简单查询，不包含子查询或者union
  - primary：包含复杂字部分，最外层就是primary
  - subquery：被包含的子查询（Select或Where中）
  - derived：FROM列表中包含的子查询被标记为DERIVED（衍生表），MYSQL会递归执行这些子查询，把结果放在临时表中。
  - union：第二个SELECT出现在UNION之后，就会被标记为UNION。若UNION包含在FROM子句的子查询中，外层SELECT会被标记为：DERIVED
  - union result：从UNION表获取结果的SELECT



- table：这一行的数据是关于哪张表的嗷！！！



- type：

  - 访问类型排列（显示查询使用了何种类型）

  - 最好到最差的排序：

    system > const > eq_ref > ref > range > index > ALL

  ![image-20210823164022064](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823164022.png)

  上面的比较常用嗷，下面这个看看就得了嗷！！！

  - 一般来说至少达到range级别，最好达到ref

  - 具体解释：

    - system：表只有一行记录（相当于系统表），这是const类型的特例，平时不会出现，这个也可以忽略不记。
    - const：通过一次索引就找到了，const用于比较primary key或者unique索引。因为只匹配一行数据，所以很快将主键置于where列表中，MySQL就能将查询转换为一个常量。
    - eq_ref：唯一性索引扫描，每个索引键，表中只有一条记录与之匹配。常见于主键或唯一索引扫描！！！
    - ref：非唯一性索引扫描，本质上是一种索引匹配。它返回所有匹配某个单独值的行，然而，它可以找到多个符合条件的行，所以他应该属于查找和扫描的混合体。
    - range：只检索给定范围的行，key列显示使用了哪个索引，一般就是你的where语句中出现了between , < , > , in等的查询。这种范围扫描这种范围扫描索引会比全表扫描要好，因为它只需要开始于索引的某一点，而结束语另外一点，不用扫描全部索引嗷！！！
    - index：Index与ALL区别为index只遍历索引树，通常比ALL快。ALL和Index都是读整张表，Index是从索引中读取的，而All是从硬盘中读取的嗷！！！

    ![image-20210823194936577](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823194936.png)

    这里要求select的东西是建立了索引的，不然就会默认从磁盘里去读取嗷！！！

    - full：Full Table Scan，将遍历全表以找到匹配的行。
    - 备注：一般来说，得保证查询至少要达到range级别，最好能达到ref。



- possible_keys：显示可能应用在这张表中的索引，有一个或者多个。查询涉及到的字段上若存在的索引，则该索引被列出，但是不一定被查询实际使用嗷！！！



- key：表示实际使用的索引，如果为NULL，则没有使用索引嗷！！！如果查询中使用了**覆盖索引**，则该索引仅仅出现在key列表中！！！

  - 覆盖索引是啥？
  - select查询的字段和我们建立的复合索引的个数和字段一一致。刚好Match上了嗷！！！刚好吻合的时候，就可以从索引上取嗷！！！
  - 说白了possible_keys没有单独的col1和col2，复合主键col1和col2都不是单独的posstible_keys，所以possible_keys为NULL。但是！！！你符合我们复合索引的要求，因此通过复合索引来取得对应字段的值嗷！！！

  ![image-20210823201308178](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823201308.png)







- key_len：
  - 表示索引中使用的字节数，可以通过该列计算查询中使用的索引的长度。准确度一定的情况下，长度越短越好。
  - key_len显示的值为索引字段的最大可能长度，并非实际使用长度。即key_len是根据表定义计算而得到，不是通过表内检查得出的嗷！！！



- ref：

  - 显示哪一列被使用了。如果可能的话，是一个常数。哪些列或者常量被用于索引列上的值。

  ![image-20210823202817524](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823202818.png)



- rows：
  - 根据表统计信息以及索引选用的情况，大致估算出找出所需的记录所需要读取的行数嗷！！！
  - 用的越少越好嗷！！！



- extra：

  - 包含十分重要的额外信息。

  - 分类：

    - Using filesort：用不上索引，只好自己内部排序。很危险，九死一生。

    ![image-20210823203849507](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823203849.png)

    - Using temporary：十死无生。使用临时表处理数据，常见于group by 和 order by

      - 一般来说哈，group by非常常用也非常容易拖慢性能，尽量按照我们索引建立的顺序来使用group by 嗷！！！

      ![image-20210823204252382](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823204252.png)

    - Using where：使用了**覆盖索引**，避免访问了表的数据行，效率不错。如果没有同时出现using where，表明索引用来读取数据而非执行查找动作嗷！！！

    ![image-20210823204807306](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823204807.png)

    - Using join buffer：使用了连续缓存
    - impossible where：出问题咯

    ![image-20210823205434789](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823205435.png)

    - Select tables optimized away：

    ![image-20210823205510739](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210823205510.png)

    - distinct：

    优化distinct操作，在找到第一匹配的元组后即停止找同样的值的动作嗷！！！

    - 前三个是最重要的嗷！！！

## 索引优化：

### 索引分析：

#### 单表：

![image-20210824091622697](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824091623.png)

```mysql
mysql> explain select id,author_id from article where category_id=1 and comments > 1 order by views desc limit 1;
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                       |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------------------+
|  1 | SIMPLE      | article | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |    33.33 | Using where; Using filesort |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------------------+
1 row in set, 1 warning (0.00 sec)

```

![image-20210824094605814](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824094615.png)

明显看出，全表扫描和order by导致的using filesort都出现了，功能实现了但是性能上有欠缺嗷！！！

- 索引：

![image-20210824094753302](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824094753.png)

只有primary key自带的索引

- 建立索引：

  - where后面使用的条件要建立索引嗷！！！ 

  - ```mysql
    mysql> create index idx_article_ccv on article(category_id,comments,viewws);
    Query OK, 0 rows affected (0.03 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    ```

  ![image-20210824095219934](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824095220.png)



- 建立后的结果:

![image-20210824095640597](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824095641.png)

索引建立完成之后，范围可能会导致索引失效嗷！！！

但是上面是我们的业务，虽然解决了全表扫描问题，但是仍然性能不佳，不符合我们的选择嗷，我们删掉它。

![image-20210824100110352](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824100110.png)

- 大胆假设：

  - 既然中间的那个范围会让索引失效，那我们干脆绕开它康康？？？

  - ```mysql
    mysql> create index idx_article_cv on article(category_id,views);
    Query OK, 0 rows affected (0.02 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> explain select id,author_id from article where category_id=1 and comments > 1 order by views desc limit 1;
    +----+-------------+---------+------------+------+----------------+----------------+---------+-------+------+----------+----------------------------------+
    | id | select_type | table   | partitions | type | possible_keys  | key            | key_len | ref   | rows | filtered | Extra                            |
    +----+-------------+---------+------------+------+----------------+----------------+---------+-------+------+----------+----------------------------------+
    |  1 | SIMPLE      | article | NULL       | ref  | idx_article_cv | idx_article_cv | 4       | const |    1 |    33.33 | Using where; Backward index scan |
    +----+-------------+---------+------------+------+----------------+----------------+---------+-------+------+----------+----------------------------------+
    1 row in set, 1 warning (0.00 sec)
    ```

  - ![image-20210824100341492](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824100341.png)

  - 芜湖，没有using filesort了嗷！！！



#### 双表：

- 左外连接和右外连接，

```mysql
mysql> explain select * from class left join book on class.card = book.card;
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+--------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                                      |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+--------------------------------------------+
|  1 | SIMPLE      | class | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   20 |   100.00 | NULL                                       |
|  1 | SIMPLE      | book  | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   20 |   100.00 | Using where; Using join buffer (hash join) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+--------------------------------------------+
2 rows in set, 1 warning (0.00 sec)
```

![image-20210824101141155](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824101141.png)

重点在于全表扫描嗷！！！那么索引建在哪个表上呢？？？

- 建在右表的card上：

![image-20210824101502885](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824101503.png)

- 建在左表的card上：

![image-20210824101546342](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824101546.png)

- 结论：

左右连接相反加，因为左右连接的特性。比如左连接，左边一定都有的，重要的是从右边索引的特性嗷，所以要在右边建立索引。

- 左连接加右表上，右连接加左表上嗷！！！



#### 三表：

- 三表SQL建在哪个字段上？

![image-20210824103329241](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824103329.png)

- 和双表一样的嗷！！！

![image-20210824103455795](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824103456.png)

- 结论：

![image-20210824103524830](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824103525.png)

类似于用书籍类别来驱动书籍。

![image-20210824104231155](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824104231.png)



### 索引优化重点：

#### 避免索引失效：

![image-20210824111621720](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824111622.png)



##### 1. 全值匹配我最爱：

![image-20210824111840042](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824111840.png)

![image-20210824112035171](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824112035.png)

- 复合索引是阶梯式的，Name -> Age -> Pos
- 只有Name都可以使用到一部分的复合索引，但是如果没有Name，直接上Pos或者Age。索引根本就用不上，索引失效了嗷！！！
- 由此引申出了下面的内容。



##### 2. 最佳左前缀法则：

![image-20210824113019273](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824113019.png)

上面这个只用到了Name的所有，Pos用不到索引嗷！！！

- 索引多列的时候，最佳做前缀法则，查询从索引的最左前列开始，并且不跳过中间的索引。
- 第一句口诀：**带头大哥不能死**嗷！！！
- 第二句口诀：**中间兄弟不能断**嗷！！！



- 说白了，你想要借用复合索引，你就要根据我从前到后索引生效的顺序来。中间如果断了，我只保证前面的生效，后面的就不一定了嗷！！！

##### 3. 不在索引列做任何操作：

- 包括：
  - 计算
  - 函数
  - 自动或手动的类型转换
- 上面这些情况会导致索引失效而转向全表扫描。

![image-20210824114538896](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824114539.png)

![image-20210824114855211](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824114855.png)



##### 4. 存储引擎不能使用索引中范围条件右边的列：

![image-20210824115055764](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824115056.png)

- 下面那个使用顺序很关键，**Name用于检索查询**，**age被用于排序**，之后的pos根本没被用上嗷！！！
- 范围之后，全失效！！！



##### 5. 尽量覆盖索引，减少select *：

![image-20210824120039724](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824120040.png)

你查的东西最好和索引设置的一样，尽可能多去覆盖嗷！！！

![image-20210824125553371](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824125554.png)

![image-20210824125735158](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824125735.png)



##### 6. 使用不等于的时候无法使用索引：

- 会导致全表扫描嗷！！！

![image-20210824125922827](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824125923.png)



##### 7. IS [NOT] NULL也无法使用索引：

![image-20210824130205952](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824130206.png)

- 其实给了我们业务一个提示，就算某个值为空，也最好给一个默认的default值，为NULL的话会影响到查询和检索的效率嗷！！！

##### 8. 模糊查询：

![image-20210824163053620](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824163054.png)

- 百分like放右边！！！
- 如何解决`%字符串%`时索引不被使用的方法？？？
  - 使用覆盖索引的方法，人为去建立复合索引
  - 然后查询的时候，使用覆盖索引来查询嗷！！！
- [聚簇索引与回表查询](https://zhuanlan.zhihu.com/p/142139541)
- [网课第11分钟开始的思考，为啥用Name去select id也是使用了复合索引？？？](https://www.bilibili.com/video/BV1KW411u7vy?p=40&spm_id_from=pageDriver)

- 只要我们select查询的所有字段都在我们建立的索引的覆盖之下，或者实际上利用了我们的索引（例如利用索引去获取ID），都是覆盖所有嗷！！！



##### 9. 字符串不加单引号导致索引失效：

- 重罪，Varchar类型不能时区单引号嗷！！

![image-20210824165844856](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824165845.png)

本质上，mysql底层自动帮助我们做了转型，这违反了上面的第3条。要避免出现内部对于数据类型发生的隐式的操作嗷！！！开发的时候，这是重罪！！！



##### 10. 少用or来连接，会导致索引失效：

![image-20210824170400707](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824170401.png)



##### 11. 小总结：

1. 带头大哥不能死
2. 中间兄弟不能断
3. 索引列上不计算
4. Like百分加右边
5. 范围之后全失效
6. 字符串里有引号

- 小练习：

![image-20210824170747161](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824170747.png)

![image-20210824171037196](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824171037.png)





- 题目练习：

[上述规则题目应用联系视频嗷！！！](https://www.bilibili.com/video/BV1KW411u7vy?p=44)

- ![image-20210824171620943](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824171621.png)

最好是第一种嗷！！！上面这些都可以使得索引生效时因为，Mysql的优化器在底层做了调整嗷！！！我们可以按照规矩来写，尽量减少调整所花费的时间嗷！！！

优化器yyds，优化器会在底层自动帮助我们进行调优，调整顺序，尽可能去符合我们的索引嗷！！！

- ![image-20210824174311579](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824174311.png)

![image-20210824174902089](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824174902.png)

- ![image-20210824175215926](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824175216.png)
- ![image-20210824175557783](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824175558.png)

![image-20210824175722593](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824175723.png)

梯子没接上？就是这种感觉嗷！！！



- ![image-20210824175905293](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824175905.png)

上面这个东西哈，group by的法则和order by 非常类似，分组排序嘛！！！

group by基本上都需要进行排序，会有临时表的产生嗷！！！



- 范围还是有不一样的嗷！！！

![image-20210824190347285](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824190347.png)

![image-20210824190529625](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824190530.png)



- 口诀：

![image-20210824190721793](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824190722.png)



#### 一般性建议：

![image-20210824180247654](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824180248.png)







# 查询截取分析：

![image-20210824190948164](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824190948.png)



## 查询优化：

### 小表驱动大表：

![image-20210824191515050](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824191515.png)

- in 和 exist 的使用都是根据情况来小表驱动大表。 查询结果都是一样样的嗷！！！

![image-20210824191717133](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824191717.png)



### Order By排序优化：

- Order by尽量使用Index方式排序，避免使用FileSort方式排序。
- 现在的中心放在，在Order by后面使用什么，会不会出现FileSort嗷！！！

![image-20210824192439557](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824192440.png)

![image-20210824192535940](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824192536.png)

![image-20210824192931595](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824192931.png)

![image-20210824193348988](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824193349.png)

- 高高低低，梯子不平，走不顺，FileSort嗷！！！

- FileSort和Index两种：

![image-20210824194533894](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824194534.png)



- 提高Order By的速度：

![image-20210824195353318](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824195353.png)



- Mysql的两种排序方式：
  - 文件排序
  - 扫描有序索引排序
- Mysql能够为排序和查询使用相同的索引嗷！！！



- 能够使用索引的情况：

![image-20210824195818310](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824195818.png)

- 不能使用索引的情况：

![image-20210824200117885](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824200118.png)

- summary：

不用记这么多的，其实就是可以康康，是不是“连起来”，比如下面的最后一个。a in (...)是一个range，无效嗷！！！a相当于裂开了，b,c就不得行嗷！！！

再比如上面的最后一个，a没问题，b>const裂开了，但是后面的是b,c。刚好和a构成了a,b,c，于是没有问题嗷！！！



### Group By排序优化：

- 和上面的几乎没啥区别，就多了一个东西嗷！！！

![image-20210824201512295](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824201512.png)



### 慢查询日志：

- 记录运行时间超过long_query_time的sql，被记录到慢查询日志中去。
- 默认意思是运行查过10s以上的语句嗷！！！
- 可以查看哪些SQL超过了我们的最大忍耐时间，再结合explain，对于特定的SQL进行全面分析即可嗷！！！
- 默认慢查询日志没有开启，如果没有调优的需要的话，一般不建议启动该参数。开启慢日志或多或少会带来一定的性能影响。

![image-20210824202056984](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824202057.png)

![image-20210824202306149](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824202306.png)

![image-20210824202446848](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824202447.png)

- 哪些SQL会被记录呢？

![image-20210824202629840](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824202630.png)

这里记录的值，是判断大于long_query_time，而不是大于等于嗷！！！

设置以后查看发现还是10秒？？？

![image-20210824202827142](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824202827.png)

- 如何演示控制睡眠？

`select sleep(n seconds)`

![image-20210824203118948](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824203119.png)

![image-20210824203159934](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824203200.png)

- 那么如何使用配置文件来实现上述这些配置呢？

![image-20210824203315582](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824203315.png)

- 日志分析工具mysqldumpslow：

![image-20210824203510679](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824203511.png)

![image-20210824203522454](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824203522.png)



## 批量数据脚本：

[SQL批量数据脚本教学嗷！！！](https://www.bilibili.com/video/BV1KW411u7vy?p=50&spm_id_from=pageDriver)

![image-20210824203908606](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824203909.png)

- 定义函数插入数据：

![image-20210824204024692](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824204024.png)



## Show Profile：

- mysql提供的可以用来分析当前会话中语句执行的资源消耗情况，可以用于SQL的调优测量。
- 默认处于关闭状态，保存最佳15次的运行结果。
- 查看mysql版本是否支持：

![image-20210824212802439](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824212802.png)

- 开启功能：

![image-20210824212918144](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824212918.png)

- 运行SQL并且验证：

![image-20210824213152999](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824213153.png)

![image-20210824213304267](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824213304.png)

这里就指的是上面的3号语句

![image-20210824213346423](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824213347.png)

- show profile所能用到的参数：

![image-20210824213605714](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824213605.png)

- 常见的大问题！！！

![image-20210824213752317](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824213752.png)

化验单例子：

![image-20210824213917058](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824213917.png)

- Summary：

![image-20210824214005001](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210824214005.png)



## 全局查询日志；

- 只能够在测试环境用，不能在生产环境使用嗷！！！永远不要在生产开启这个功能嗷！！！

- 打开全局查询日志：

![image-20210825085024063](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825085040.png)

![image-20210825085146180](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825085146.png)





# 数据库锁理论：

- 锁可以对于有限的资源进行保护，解决隔离和并发的矛盾。
- 锁的分类：
  - 数据操作：
    - 读锁：共享锁
    - 写锁：排它锁
  - 数据操作：
    - 表锁：偏向于MyISAM存储引擎，粒度大
    - 行锁：
    - 页锁



## MyISAM表锁：

- 创建表：

![image-20210825091932879](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825091933.png)

- 手动增加表锁：

`lock table 表名字 read(write), 表名字2 read(write)`

- 查看表上加过的锁：

`show open tables`

- 手动释放锁：

`unlock tables`

上面这个可以释放所有表中的锁嗷！！！



### 案例1：

- Session1加读锁，Session2不管，尝试实验。

- 和上面说的一样，共享锁可以共享读但是不能够写嗷！！！锁住了，别的人和自己都不能写嗷！！！
- 一个表加读锁，别的表的数据也读取不到嗷！！！你只有把当前的锁放了，你才能去读别的嗷！！！你把帐给我了结了，你再去干别的事儿嗷！！！
- 一个会话加了表级别的读锁，另外的会话如果想去向这个表该数据，**阻塞了！！！**，这就会导致出大问题嗷！！！
- 读，阻塞，写的例子嗷！！！

- summary：

![image-20210825101546589](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825101546.png)

![image-20210825101511460](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825101511.png)

![image-20210825101527916](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825101528.png)



### 案例2：

- Session1加写锁，Session2不管，尝试实验。

- 自己加锁，自己能读并且自己能改嗷！！！
- 自己加锁之后，读不了其他表嗷！！！
- 读取加了写锁的的表会阻塞嗷！！！



- summary：

![image-20210825113650194](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825113650.png)

![image-20210825113811966](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825113812.png)





### 小总结：

![image-20210825113841799](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825113842.png)

![image-20210825115035980](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825115036.png)

- 分析锁定的表：

![image-20210825115756496](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825115757.png)

![image-20210825115817258](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825115817.png)

![image-20210825115917153](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825115917.png)

- MyISAM就是因为表锁的锁定，而适用于读取，而不是读取和修改嗷！！！



## InnoDB行锁：

- 粒度小，并发高嗷！！！
- 与MyISAM最大的不同点：
  1. 支持事务的概念
  2. 采用了行级锁
- ACID：
  - 原子性
  - 一致性
  - 隔离性
  - 持久性
- 并发事务带来的问题：
  - 脏读：
    - 读取到了已经修改但是尚未提交的数据（中间数据）
  - 更新丢失
  - 不可重复读：
    - 两次读取结果不一致
  - 幻读：
    - 两次读取，读取发现数据增多了
- 默认隔离级别是可重复读嗷！！！
- 四个级别：
  - 读未提交
  - 读已提交
  - 可重复读
  - 序列化

![image-20210825120724813](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825120725.png)



![image-20210825120952563](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825120952.png)

- 默认即使RR -> 可重复读嗷！！！

### 案例：

![image-20210825125607574](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825125607.png)

- 多个Session去操作同一行，第一个操作的那个会获得锁，后面的其他Session都会阻塞嗷！！！
- 但是如果多个Session操作的是不同行，获得的是不同行的锁，就没问题嗷！！！
- [AutoCommit的关闭而导致的，不commit的话，就没有办法及时获得别的Session更新后的结果的情况嗷！！！](https://www.bilibili.com/video/BV1KW411u7vy?p=56&spm_id_from=pageDriver)

WHY？？？？？   ----->     可重复读哇，在我的一个事务中，我读取的某个变量的值应该是一定的嗷！！！所有只有commit之后，开启了一条新的事务，才能够重新获得新的值嗷！！！



### 索引失效：

- 导致行锁升级为表锁嗷！！！

![image-20210825152425346](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825152426.png)

- 所以说，开发过程中，这一类的Bug是最难找的嗷！！！varchar类型赋值的时候写成数字，就会gg！！！是重罪！！！
- 只有左边提交了，右边的阻塞才能够结束嗷！！



### 间隙锁危害：

![image-20210825152923810](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825152924.png)

![image-20210825153001107](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825153001.png)





### 如何锁定一行：

![image-20210825153228092](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825153228.png)

- 这里经过测试之后证明，左边锁定了这一行之后，右边仍然可以读取这一行的数据，但是右边是不可以对于这一行做修改的嗷！！！
- 实现了行级锁定，虽然锁定机制的性能损耗可能高一些。但是在并发处理能力远远优于MyISAM的表级锁定嗷！！！但是如果使用不当，性能甚至可能比MyISAM更差哦！
- 一个命令：

`show status like 'innodb_row_lock%';`

![image-20210825161243241](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825161243.png)

比较重要的是：

![image-20210825161338997](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825161339.png)



- 优化建议：

1. 建议所有数据检索通过索引来完成，避免无索引行锁升级为表锁。
2. 合理设计索引，尽量缩小锁的范围。
3. 避免间隙锁，减少检索条件。
4. 尽量控制事务大小，减少锁定资源量和时间长度。
5. 尽可能低级别的事务隔离。





# 主从复制：

- Window上面一台，Linux上面有一台

## 复制基本原理：

- slave会从master读取binlog来进行数据同步。

![image-20210825161855636](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825161856.png)

- 基本原则：
  1. 一个slave只有一个master
  2. 一个master可以有多个slave
  3. 每个slave只能有一个唯一的服务器ID
- 复制的问题：可能会有时延嗷！



## 一主一从：

- 要求：
  - mysql版本一致且在后台以服务运行
  - 统一网段，而且ping的通嗷！！！
- Windows做主机，Linux做从机（要求两边能够相互Ping通嗷！！！）



### 主机：

- 修改my.ini文件，[我的主机中，配置文件所在的位置](C:\ProgramData\MySQL\MySQL Server 8.0)
- 配置如下：

![image-20210825164218482](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825164218.png)

![image-20210825164304266](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825164304.png)

- 可选的可以都不用咋调整嗷！！！
- 配置完成之后重启嗷！！！



### 从机：

- 修改my.cnf配置文件嗷！！！

![image-20210825164520575](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825164520.png)

- 配置完成之后重启嗷！！！



### 主机建立账户并授权slave：

![image-20210825164805080](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825164805.png)

- 允许服务器以zhangsan用户连接并完成主从复制嗷！！！
- show master status：

![image-20210825164924338](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825164924.png)

第一个代表文件名称，第二个代表开始复制的位置嗷！！！Do没有，代表都要复制。Ignore代表忽略，不需要复制的数据库。



### 从机完成相应的配置：

![image-20210825165209721](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825165210.png)

- 第一行就是上面这一行嗷！！！主要是完成主机连接的一些设置而已嗷！！！

![image-20210825165322744](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825165323.png)





### 停止从机：

![image-20210825165610396](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825165610.png)



### 错误处理：

- 随着写入数据的变化，master status也会有变化，会出一些问题。
- 所以每次重新开始同步的时候，都需要先查看一下master status，再进行同步嗷！！！

![image-20210825170027954](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210825170028.png)





