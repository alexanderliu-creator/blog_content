---
title: mySQL数据库
date: 2021-02-20 16:05:21
tags: 
	Database


---



# mySQL数据库初步探讨

---



<!--more-->





# mySQL自学

## 数据库模型：

> 1. 层次模型
> 2. 网状模型
> 3. 关系模型（最终获胜）



## 使用MySQL时，不同的表可以选择不同的数据库引擎，若不知道选择哪种引擎选择InnoDB!!!



# 正文：

> ## 关系模型是建立在关系模型上的
>
> 
>
> ## 每一行被称为一个record,每一列被称为一个column,通常情况下，避免字段为NULL,不允许为NULL可以简化查询条件，加快查询速度
>
> 
>
> ## 与excel不同，关系数据库的表与表之间的逻辑关系更加复杂，数据关系库中关系通过主键和外键来维护！！！
>
> 
>
> > 主键:  
> >
> > 1. 一条记录就是由多个字段组成，如果能通过某个字段区分出不同的记录，则这个字段被称为主键。
> > 2. 插入相同主键两条记录是不被允许的，例如以名字为例，无法插入两个相同的名字
> > 3. 记录一旦插入到表中最好不要修改主键，主键是用来唯一定位记录的，一旦修改了主键会造成一系列的影响。
> > 4. 选取主键的基本原则：不适用任何业务相关的字段作为主键。例如你可以自己内置一些id去区分各个个体，
> > 5. 常见的id类型：1. 自增整数类型   2.全局唯一GUID类型(对于大部分类型来说自增类型完全够用)
> > 6. 联合主键：可以由多个主键，允许有几个主键有重复，只要不是所有主键重复即可。（联合主键并不常用。）
> >
>



> > 外键：
> >
> > 1. 在一个表中，可以把数据和另外一个表关联起来，这种列被称为外键。
> > 2. 外键不是通过列名来实现的，是通过定义外键约束实现的
> >
> > ```mysql
> > ALTER TABLE students
> > ADD CONSTRAINT fk_class_id
> > FOREIGN KEY (class_id)
> > REFERENCES classes (id);
> > ```
> >
> > fk_class_id可以任意，foreign key指定了谁作为外键，references则制定了将这个外键关联到class表的id列。
> >
> > 3. 删除外键要用ALTER TABLE来实现:
> >
> > ```mysql
> > ALTER TABLE students
> > DROP FOREIGN KEY fk_class_id;
> > ```
> >
> > 删除外键约束并没有删除外键这一列，删除一列是通过Drop Column来实现。
> >
> > 4. 关系的对应十分随机，可以一对一的对应，可以多对一，一对多，多对多（甚至通过中间表来进行对应）
>



> > 索引:
> >
> > 1. 比如常常对一个表中的score列进行查询，就可以对其创建索引。
> >
> > ```mysql
> > ALTER TABLE students
> > ADD INDEX idx_score (score);
> > ```
> >
> > 创建了一个名称为idx_score的对于score的索引，索引如果有多列，可以以此在下面的括号中写上。
> >
> > 2. 索引的效率还取决于索引值是否是散列的，如果值越不相同，则查找的效率越高，如果相同的很多，则创建索引的意义就不是特别大。
> > 3. 同时可以对于一个表创建多个索引，索引提高了查询的效率，但同时删改增的过程中则需要同时修改索引，素的就会满嗷！
> > 4. 系统会自动为主键创建索引，索引效率是最高的，因为主键一定是唯一的，这就是效率高的理由。
> > 5. 唯一索引：再次强调，具有业务含义的东东不适合作为主键，但同时又要唯一，这个时候可以这样进行初始化：
> >
> > ```mysql
> > ALTER TABLE students
> > ADD UNIQUE INDEX uni_name (name);
> > ```
> >
> > 通过unique关键字，我们成功添加了一个唯一索引。
> >
> > 6. 同时，unique不知能进行唯一索引的创建，它也能完成唯一约束的创建（在不创建索引的情况下）
> >
> > ```mysql
> > ALTER TABLE students
> > ADD CONSTRAINT uni_name UNIQUE (name);
> > ```
> >
> > 
>



> >查询数据:
> >
> >> 基本查询: SELECT * FROM <表名>（查询所有的行）
> >>
> >> > 1. SELECT是关键字，*表示所有的列，FROM表示从哪个表中查询
> >> >
> >> > 2. SELECT甚至可以用于计算: SELECT 100+200(结果会输出300)
> >> >
> >> > 
> >>
> >> 条件查询：SELECT * FROM <表名> WHERE <条件表达式>
> >>
> >> > 1. 条件表达式可以使用<条件1> AND <条件2> 这种写法
> >> >
> >> > 2. 同时也可以使用 <条件1> OR <条件2>这种写法
> >> >
> >> > 3. NOT <条件>表示不符合该条件的记录
> >> >
> >> > 多个条件混合查询的例子：
> >> >
> >> > ```mysql
> >> > SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';
> >> > ```
> >> >
> >> > 4. 判断相等用=   ; 判断不等用<>   ;判断相似用 name LIKE "ab%"
> >> >
> >> > 
> >>
> >> 投影查询：SELECT (特定的列) FROM <表名>
> >>
> >> > 1. 将某一列读取出来并且重命名可以用SELECT score points FROM<表名>
> >> >
> >> > 将列名score重命名为points
> >> >
> >> > 
> >>
> >> 排序：排序通常是按照id来进行，也就是根据主键来进行排序，如果要根据别的条件来进行排序，一般我们加上ORDER BY子句。
> >>
> >> > 1. 例子来一个:
> >> >
> >> > ```mysql
> >> > SELECT id, name, gender, score FROM students ORDER BY score;
> >> > 
> >> > ```
> >> >
> >> > 2. 默认为从低到高，如果要从高到底，则可以加上DESC表示逆序:
> >> >
> >> > ```mysql
> >> > SELECT id, name, gender, score FROM students ORDER BY score DESC;
> >> > ```
> >> >
> >> > 3. 如果同一列内有相同的数据要进行进一步的排序，则可以继续添加类名，表示先按第一条件排序，若相同则按第二条件排序，以此类推。
> >> >
> >> > ```mysql
> >> > SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
> >> > 
> >> > ```
> >> >
> >> > 4. 默认排序规则是ASC:升序，可以忽略，ORDER BY score ASC和ORDER BY score的效果是一样滴！
> >> > 5. 带有WHERE条件的ORDER BY:
> >> >
> >> > 举个栗子:
> >> >
> >> > ```mysql
> >> > SELECT id, name, gender, score
> >> > FROM students
> >> > WHERE class_id = 1
> >> > ORDER BY score DESC;
> >> > 
> >> > ```
> >> >
> >> > 
> >>
> >> 分页查询：LIMIT <M> OFFSET <N>(分页本质上是记录的截取)
> >>
> >> > ```mysql
> >> > SELECT id, name, gender, score
> >> > FROM students
> >> > ORDER BY score DESC
> >> > LIMIT 3 OFFSET 0;
> >> > 
> >> > ```
> >> >
> >> > 1. LIMIT限制了一页记录的条数，OFFSET设置了从第几条开始（默认下标也是从0开始）
> >> >
> >> > - `LIMIT`总是设定为`pageSize`；
> >> > - `OFFSET`计算公式为`pageSize * (pageIndex - 1)`
> >> >
> >> > 2. `OFFSET`超过了查询的最大数量并不会报错，而是得到一个空的结果集。
> >> >
> >> > 3. `OFFSET`是可选的，如果只写`LIMIT 15`，那么相当于`LIMIT 15 OFFSET 0`。
> >> >
> >> >    在MySQL中，`LIMIT 15 OFFSET 30`还可以简写成`LIMIT 30, 15`。
> >> >
> >> >    使用`LIMIT <M> OFFSET <N>`分页时，随着`N`越来越大，查询效率也会越来越低。
> >> >
> >> >    
> >>
> >> 聚合查询：这里用到的是一些内置的函数
> >>
> >> > 1. COUNT()函数  SELECT COUNT(*) FROM students
> >> >
> >> > 查询所有列的函数并输出
> >> >
> >> > ```mysql
> >> > SELECT COUNT(*) num FROM students;
> >> > ```
> >> >
> >> > 查询所有列的函数并输出(并重命名为num)
> >> >
> >> > 2. COUNT(*)和COUNT(id)效果是一样的，因为表的行数是一定的，同时也可以使用WHERE关键字来进行筛选
> >> >
> >> > ```mysql
> >> > SELECT COUNT(*) boys FROM students WHERE gender = 'M';
> >> > 
> >> > ```
> >> >
> >> > mention: 如果WHERE没有匹配到任何一行，则count会返回0哦
> >> >
> >> > - 每页3条记录，如何通过聚合查询获得总页数？
> >> > - SELECT CEILING(COUNT(*) / 3) FROM students;
> >> >
> >> > 3. 分组聚合，永远的神
> >> >
> >> > ```mysql
> >> > SELECT class_id,COUNT(*) num FROM students GROUP BY class_id;
> >> > ```
> >> >
> >> > 此处GROPE BY以class_id为对象分别去分组，使得分别COUNT得以实现，最终会把每一行的结果按照顺序返回。同时可以在表中列出class_id。
> >> >
> >> > 4. 同时可以多个列来进行分组:
> >> >
> >> > ```mysql
> >> > SELECT class_id, gender, COUNT(*) num FROM students GROUP BY class_id, gender;
> >> > ```
> >> >
> >> > 5. ```mysql
> >> >    SUM()`、`AVG()`、`MAX()`和`MIN()
> >> >    ```
> >> >
> >> > 一样的，后面加上列号
> >> >
> >> > ```mysql
> >> > SELECT AVG(score) average FROM students WHERE gender = 'M';
> >> > ```
> >> >
> >> > 如果没有匹配到的话会自动返回NULL
> >> >
> >> > 6. 给我的感觉就是，SELECT的列和GROUP BY的列应该是一样的嗷！
> >> >
> >> > 两个练习:
> >> >
> >> > - 请使用一条SELECT查询查出每个班级的平均分。
> >> >
> >> > - 请使用一条SELECT查询查出每个班级男生和女生的平均分。
> >> >
> >> > 7. 如果要查询三条记录，如何通过聚合查询获得总页数：
> >> >
> >> > SELECT CEILING(COUNT(*) / 3) FROM students
> >> >
> >> > 
> >>
> >> 多表查询：SELECT * FROM <表一> <表二>
> >>
> >> > 1. 这种一次查询两个表的数据，查询的结果也是一个二维表，它是`students`表和`classes`表的“乘积”，即`students`表的每一行与`classes`表的每一行都两两拼在一起返回。结果集的列数是`students`表和`classes`表的列数之和，行数是`students`表和`classes`表的行数之积。
> >> >
> >> > 2. 特别注意呀，这种多表查询又被称为笛卡尔查询，由于目标结果集合是行数乘积，两个100行的表就会返回10000条记录，因此一定要注意。
> >> >
> >> > 3. 仍然可以利用投影查询的“设置列的别名”来给两个表各自的`id`和`name`列起别名
> >> >
> >> >    ```mysql
> >> >    SELECT
> >> >        students.id sid,
> >> >        students.name,
> >> >        students.gender,
> >> >        students.score,
> >> >        classes.id cid,
> >> >        classes.name cname
> >> >    FROM students, classes;
> >> >    ```
> >> >
> >> >    注意，多表查询时，要使用表名.列名这种方式来应用和设置别名，这样才能避免结果集的列名重复问题。
> >> >
> >> > 4. 同理，用FROM也能给予一些特定的元素别名   用法: FROM <表名1> <别名1>, <表名2> <别名2>       例如：`FORM student s, classes c;`
> >> >
> >> > 5. **尽量不要使用多表查询，很容易爆炸。**
> >> >
> >> > 
> >>
> >> 连接查询：连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。
> >>
> >> > ```mysql
> >> > SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
> >> > FROM students s
> >> > INNER JOIN classes c
> >> > ON s.class_id = c.id;
> >> > ```
> >> >
> >> > 1. INNER JOIN查询的写法是：
> >> >
> >> > - 先确定主表 仍然使用`FROM <表1>`这种写法
> >> >
> >> > - 再写确定需要连接的表，使用`INNER JOIN <表2>`
> >> >
> >> > - 确定连接的条件，使用`ON <条件>`,这里的条件是`s.class_id = c.id`，表示`students`表的`class_id`列与`classes`表的`id`列相同的行需要连接；
> >> >
> >> > - 可以加上WHERE和ORDER BY等子句。
> >> >
> >> >   自我总结：例如上面这个例子，将students和classes中id相同的连接起来了，然后把对应的项也整合到了表中
> >> >
> >> > 2. OUTER JOIN查询的写法是：
> >> >
> >> > - 与1. 完全类似的，只是INNER JOIN改成了OUTER JOIN而已。
> >> > - 同时：
> >> >
> >> > 有RIGHT OUTER JOIN，就有LEFT OUTER JOIN，以及FULL OUTER JOIN。它们的区别是：
> >> >
> >> > INNER JOIN只返回同时存在于两张表的行数据，由于`students`表的`class_id`包含1，2，3，`classes`表的`id`包含1，2，3，4，所以，INNER JOIN根据条件`s.class_id = c.id`返回的结果集仅包含1，2，3。
> >> >
> >> > RIGHT OUTER JOIN返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以`NULL`填充剩下的字段。
> >> >
> >> > LEFT OUTER JOIN则返回左表都存在的行。如果我们给students表增加一行，并添加class_id=5，由于classes表并不存在id=5的行，所以，LEFT OUTER JOIN的结果会增加一行，对应的`class_name`是`NULL`：
> >> >
> >> > FULL OUTER JOIN，它会把两张表的所有记录全部选择出来，并且，自动把对方不存在的列填充为NULL
> >> >
> >> > 



> 修改数据:
>
> > INSERT: INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);
> >
> > > ```mysql
> > > INSERT INTO students (class_id, name, gender, score) VALUES (2, '大牛', 'M', 80);
> > > ```
> > >
> > > 1. id是自增主键，可以被数据库自己推导出来，不用其他多余的插入。如果字段本身有默认值的话其实也可以不用在insert语句中出现。
> > > 2. 一次性也可以同时插入多条新的记录，VALUES后面可以接上多个括号:
> > >
> > > ```mysql
> > > INSERT INTO students (class_id, name, gender, score) VALUES
> > >   (1, '大宝', 'M', 87),
> > >   (2, '二宝', 'M', 81);
> > > 
> > > ```
> > >
> >
> > UPDATE: UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
> >
> > > ```mysql
> > > UPDATE students SET name='大牛', score=66 WHERE id=1;
> > > ```
> > >
> > > 1. 注意到UPDATE语句中，WHERE的用法其实和之前都类似的，所以我们完全可以通过这种方式来一次更新多条记录
> > >
> > > ```mysql
> > > UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;
> > > ```
> > >
> > > 2. 更新字段的时候甚至可以使用表达式：
> > >
> > > ```mysql
> > > UPDATE students SET score=score+10 WHERE score<80;
> > > ```
> > >
> > > 3. 如果没有匹配到任何的记录，UPDATE语句也不会报错，当然也就不会更新任何记录
> > >
> > > 4. **如果UPDATE条件没有任何的WHERE条件的限制，整个表的记录都会被更新，所以通常最好的做法是先用SELECT语句来测试是狗WHERE挑选除了期望的记录集，然后再用UPDATE更新。**
> > >
> > > 5. 在使用`MySQL`这类真正的关系数据库时，`UPDATE`语句会返回更新的行数以及`WHERE`条件匹配的行数。
> > >
> > >    例如，更新`id=1`的记录时：
> > >
> > >    ```
> > >    mysql> UPDATE students SET name='大宝' WHERE id=1;
> > >    Query OK, 1 row affected (0.00 sec)
> > >    Rows matched: 1  Changed: 1  Warnings: 0
> > >    ```
> > >
> > >    MySQL会返回`1`，可以从打印的结果`Rows matched: 1 Changed: 1`看到。
> > >
> > >    当更新`id=999`的记录时：
> > >
> > >    ```
> > >    mysql> UPDATE students SET name='大宝' WHERE id=999;
> > >    Query OK, 0 rows affected (0.00 sec)
> > >    Rows matched: 0  Changed: 0  Warnings: 0
> > >    ```
> > >
> > >    `MySQL`会返回`0`，可以从打印的结果`Rows matched: 0 Changed: 0`看到。
> >
> > DELETE: DELETE FROM <表名> WHERE ...;
> >
> > > 1. DELETE同时也可以删除多条记录哦，与UPDATE类似，可以通过WHERE来选定多条记录并执行删除操作。
> > > 2. 如果`WHERE`条件没有匹配到任何记录，`DELETE`语句不会报错，也不会有任何记录被删除。
> > > 3. 最后，要特别小心的是，和`UPDATE`类似，不带`WHERE`条件的`DELETE`语句会删除整个表的数据
> > >
> > > ```mysql
> > > DELETE FROM students;
> > > ```
> > >
> > > ### MySQL
> > >
> > > 在使用MySQL这类真正的关系数据库时，`DELETE`语句也会返回删除的行数以及`WHERE`条件匹配的行数。
> > >
> > > 例如，分别执行删除`id=1`和`id=999`的记录：
> > >
> > > ```
> > > mysql> DELETE FROM students WHERE id=1;
> > > Query OK, 1 row affected (0.01 sec)
> > > 
> > > mysql> DELETE FROM students WHERE id=999;
> > > Query OK, 0 rows affected (0.01 sec)
> > > ```
> > >
> > > 



> 关于mysql的相关的操作和其他原理:
>
> > - 直接用MySQL Client的可执行程序mysql就可以与数据库相连接，mysql实际上是MySQL客户端，真正的MySQL服务器程序是mysqld,在后台运行。
> >
> > > 关于MySQL相关操作：
> > >
> > > 1. 查找列出所有的数据库: SHOW DATABASES;(可以列出所有的数据库，其中infotmation_schema, mysql, performance_schema, sys是系统库，不要尝试去改动他们。其余的就是用户创建的数据库。)
> > > 2. 创建新的数据库: CREAT DATABASE <name>
> > > 3. 删除一个数据库: DROP DATABASE <name>(删除一个数据库将导致此数据库中所有的表全部被删除)
> > > 4. 切换数据库的方法: USE <name>
> > > 5. 列出当前数据库的所有的表: SHOW TABLES
> > > 6. 查看表的结构: DESC <name>
> > > 7. 查看创建表的SQL语句：SHOW CREAT TABLE <name>
> > > 8. 创建新的表: CEATE TABLE <name>
> > > 9. 删除表: DROP TABLE <name>
> > > 10. 修改表: ALTER TABLE <name> ADD COLUMN <列名> 
> > > 11. 例如要更改列名该怎么做呢？:
> > >
> > > ALTER TABLE <name> CHANGE COLUMN <columnname1> <columnname2> VARCHAR(20) NOT NULL;
> > >
> > > 12. 删除列： ALTER TABLE <name> DROP COLUMN <columnname>
> > > 13. 退出mysql: EXIT
> > >
> > > EXIT只是断开了客户端与服务器的连接，MySQL服务器仍然继续运行。
> > >
> > > 





实用sql语句:

> - ### 插入或替换
>
>   如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就先删除原记录，再插入新记录。此时，可以使用`REPLACE`语句，这样就不必先查询，再决定是否先删除再插入：
>
>   ```mysql
>   REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
>   ```
>
>   若`id=1`的记录不存在，`REPLACE`语句将插入新记录，否则，当前`id=1`的记录将被删除，然后再插入新记录。
>
> - ### 插入或更新
>
>   如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就更新该记录，此时，可以使用`INSERT INTO ... ON DUPLICATE KEY UPDATE ...`语句：
>
>   ```mysql
>   INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;
>   ```
>
>   若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，当前`id=1`的记录将被更新，更新的字段由`UPDATE`指定。
>
> - ### 插入或忽略
>
>   如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就啥事也不干直接忽略，此时，可以使用`INSERT IGNORE INTO ...`语句：
>
>   ```mysql
>   INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
>   ```
>
>   若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，不执行任何操作。
>
> - ### 快照
>
>   如果想要对一个表进行快照，即复制一份当前表的数据到一个新表，可以结合`CREATE TABLE`和`SELECT`：
>
>   ```mysql
>   -- 对class_id=1的记录进行快照，并存储为新表students_of_class1:
>   CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
>   ```
>
>   新创建的表结构和`SELECT`使用的表结构完全一致。
>
> - ### 写入查询结果集
>
>   如果查询结果集需要写入到表中，可以结合`INSERT`和`SELECT`，将`SELECT`语句的结果集直接插入到指定表中。
>
>   例如，创建一个统计成绩的表`statistics`，记录各班的平均成绩：
>
>   ```mysql
>   CREATE TABLE statistics (
>       id BIGINT NOT NULL AUTO_INCREMENT,
>       class_id BIGINT NOT NULL,
>       average DOUBLE NOT NULL,
>       PRIMARY KEY (id)
>   );
>   ```
>
>   然后，我们就可以用一条语句写入各班的平均成绩：
>
>   ```mysql
>   INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;
>   ```
>
>   确保`INSERT`语句的列和`SELECT`语句的列能一一对应，就可以在`statistics`表中直接保存查询的结果：
>
>   ```mysql
>   > SELECT * FROM statistics;
>   +----+----------+--------------+
>   | id | class_id | average      |
>   +----+----------+--------------+
>   |  1 |        1 |         86.5 |
>   |  2 |        2 | 73.666666666 |
>   |  3 |        3 | 88.333333333 |
>   +----+----------+--------------+
>   3 rows in set (0.00 sec)
>   ```
>   
>   
>   
> - ### 强制使用指定索引
>
>   在查询的时候，数据库系统会自动分析查询语句，并选择一个最合适的索引。但是很多时候，数据库系统的查询优化器并不一定总是能使用最优索引。如果我们知道如何选择索引，可以使用`FORCE INDEX`强制查询使用指定的索引。例如：
>
>   ```mysql
>   > SELECT * FROM students FORCE INDEX (idx_class_id) WHERE class_id = 1 ORDER BY id DESC;
>   ```
>
>   指定索引的前提是索引`idx_class_id`必须存在。
>
>   


