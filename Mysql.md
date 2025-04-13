### 函数

#### 聚合函数

| 函数  |   功能   |
| :---: | :------: |
| count | 统计数量 |
|  max  |  最大值  |
|  min  |  最小值  |
|  avg  |  平均值  |
|  sum  |   求和   |

> null值不参与聚合函数运算



- DQL-书写顺序

```mysql
select 
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段列表
having	
	分组后条件列表
order by
	排序字段列表
limit
	分页参数
```



where跟having的区别：

​	执行时机不同：where是分组之前进行过滤，不满足where条件的不参与分组；而having是分组之后对结果进行过滤。

​	判断条件不同：where不能对聚合函数进行判断，而having可以。

注：

​	执行顺序:where -> 聚合函数 -> having

​	分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义



- DQL-执行顺序

​	from - where - group by - having - select - order by - limit



---

#### 字符串函数

|           函数           |                           功能                            |
| :----------------------: | :-------------------------------------------------------: |
|   concat(s1,s2,...sn)    |    字符串拼接，将s1，s2，...sn个字符串拼接成一个字符串    |
|        lower(str)        |                 将字符串str全部转化为小写                 |
|        upper(str)        |                 将字符串str全部转化为大写                 |
|     lpad(str,n,pad)      | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度 |
|     rpad(str,n,pad)      | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度 |
|        trim(str)         |                去掉字符串头部和尾部的空格                 |
| substring(str,start,len) |      返回从字符串str从start位置起的len个长度的字符串      |



---

#### 数值函数

|    函数    |               功能               |
| :--------: | :------------------------------: |
|  ceil(x)   |             向上取整             |
|  floor(x)  |             向下取整             |
|  mod(x,y)  |           返回x/y的模            |
|   rand()   |        返回0~1内的随机数         |
| round(x,y) | 求参数x四舍五入的值，保留y位小数 |



---

#### 日期函数

|               函数                |                       功能                        |
| :-------------------------------: | :-----------------------------------------------: |
|             curdate()             |                   返回当前日期                    |
|             curtime()             |                   返回当前时间                    |
|               now()               |                返回当前日期和时间                 |
|            year(date)             |                获取指定date的年份                 |
|            month(date)            |                获取指定date的月份                 |
|             day(date)             |                获取指定date的日期                 |
| date_add(date,INTERVAL expr typr) | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
|       datediff(date1,date2)       |    返回起始时间date1和结束时间date2之间的天数     |

interval  /ˈɪntərvl/ 间隔

```mysql
select date_add('2022-01-01', INTERVAL 10 day )
执行结果：2022-01-11
返回日期2022-01-01间隔10天后的日期

select date_add('2022-01-01', interval 10 month )
执行结果：2022-11-01
返回日期2022-01-01间隔10个月后的日期

select date_add('2022-01-01', interval 10 year )
执行结果：2032-01-01
返回日期2022-01-01间隔10年后的日期

select datediff('2022-01-01','2022-11-01')
执行结果：-304
返回值是：date1 - date2 间隔的天数

select datediff('2022-11-01','2022-10-01')
执行结果：31
```



---

#### 流程函数

|                            函数                            |                           功能                           |
| :--------------------------------------------------------: | :------------------------------------------------------: |
|                       if(value,t,f)                        |           如果value为true，则返回t，否则返回f            |
|                   ifnull(value1,value2)                    |       如果value1不为空，返回value1，否则返回value2       |
|    case when [val1] then [res1] ... else [default] end     |    如果val1为true，返回res1，...否则返回default默认值    |
| case [expr] when [val1] then [res1] ... else [default] end | 如果expr的值等于val1，返回res1，...否则返回default默认值 |



### 约束

#### 概述

概念：约束时作用于表中字段上的规则，用于限制存储在表中的数据

目的：保证数据库中的数据的正确、有效性和完整性

分类：

|            约束            |                           描述                           |   关键字    |
| :------------------------: | :------------------------------------------------------: | :---------: |
|          非空约束          |                限制该字段的数据不能为null                |  NOT NULL   |
|          唯一约束          |          保证该字段的所有数据都是唯一、不重复的          |   UNIQUE    |
|          主键约束          |         主键是一行数据的唯一标识、要求非空且唯一         | PRIMARY KEY |
|          默认约束          |      保存数据时，如果未指定该字段的值，则采用默认值      |   DEFAULT   |
| 检查约束（8.0.16版本之后） |                  保证字段值满足某一条件                  |    CHECK    |
|          外键约束          | 用来让两张表的数据之间建立连接，保证数据的一致性和完整性 | FOREIGN KEY |



### 子查询

#### 标量子查询

子查询结果为单个值



#### 列子查询

子查询结果为一列（可以是多行）

常用的操作符：in、not in、any、some、all

| 操作符 |                  描述                  |
| :----: | :------------------------------------: |
|   in   |      在指定的集合范围之内，多选一      |
| not in |         不在指定的集合范围之内         |
|  any   |  子查询返回列表中，有任意一个满足即可  |
|  some  | 与any等同，使用some的地方都可以使用any |
|  all   |    子查询返回列表的所有值都必须满足    |



#### 行子查询

子查询结果为一行（可以是多列）



#### 表子查询

子查询结果为多行多列



### 事务

事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作==要么同时成功，要么同时失败==。

默认MySQL的事务是自动提交的，也就是说，当执行一条DML语句，MySQL会立即隐式的提交事务。

#### 事务的四大特性

- 原子性（Atomicity /ˌætəˈmɪsəti/）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败
- 一致性（Consistency /kənˈsɪstənsi/）：事务完成时，必须使所有的数据都保持一致状态
- 隔离性（Isolation /ˌaɪsəˈleɪʃn/）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
- 持久性（Durability /dərəˈbɪlɪti/）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

#### 并发事务问题

|    问题    |                             描述                             |
| :--------: | :----------------------------------------------------------: |
|    脏读    |           一个事务读到另外一个事务还没有提交的数据           |
| 不可重复读 | 一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读 |
|    幻读    | 一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在 |

#### 事务隔离级别

|       隔离级别        | 脏读 | 不可重复读 | 幻读 |
| :-------------------: | :--: | :--------: | :--: |
|   Read uncommitted    |  √   |     √      |  √   |
|    Read committed     |  ×   |     √      |  √   |
| Repeatable Read(默认) |  ×   |     ×      |  √   |
|     Serializable      |  ×   |     ×      |  ×   |

从上往下事务的隔离级别逐渐变高

事务的隔离级别越高，数据越安全，但是性能越低



### 存储引擎

存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式。存储引擎是基于表而不是基于库的，所以存储引擎也可以被称为表引擎。
默认存储引擎是InnoDB。

#### InnoDB

InnoDB是一种兼顾高可靠性和高性能的通用存储引擎，在MySQL5.5之后，InnoDB是默认的MySQL存储引擎

特点：

- DML操作遵循ACID模型，支持==事务==
- ==行级锁==，提高并发访问性能
- 支持==外键==FOREIGN KEY约束，保证数据的完整性和正确性



文件：

- xxx.ibd：xxx代表的是表名，innoDB引擎的每张表都会对应这样一个表空间文件，存储该表的表结构（frm、sdi）、数据和索引

- 参数：innodb_file_per_table（决定多张表共享一个表空间还是每张表对应一个表空间，默认开启：即一张表一个表空间）



InnoDB存储结构

![image-20250405172639894](D:\学习笔记\assets\image-20250405172639894.png)



#### MyISAM

MyISAM是MySQL早期的默认存储引擎

特点：

- 不支持事务，不支持外键
- 支持表锁，不支持行锁
- 访问速度快

文件：

- xxx.sdi：存储表结构信息
- xxx.MYD：存储数据
- xxx.MYI：存储索引



#### Memory

Memory引擎的表数据是存储在内存中的，受硬件问题、断电问题的影响，只能将这些表作为临时表或缓存使用

特点：

- 存放在内存中，速度快
- hash索引（默认）

文件：

* xxx.sdi：存储表结构信息



#### 存储引擎特点

|     特点     |       InnnoDB       | MyISAM | Memory |
| :----------: | :-----------------: | :----: | :----: |
|   存储限制   |        64TB         |   有   |   有   |
|   事务安全   |      ==支持==       |   -    |   -    |
|    锁机制    |      ==行锁==       |  表锁  |  表锁  |
|  B+tree索引  |        支持         |  支持  |  支持  |
|   Hash索引   |          -          |   -    |  支持  |
|   全文索引   | 支持（5.6版本之后） |  支持  |   -    |
|   空间使用   |         高          |   低   |  N/A   |
|   内存使用   |         高          |   低   |  中等  |
| 批量插入速度 |         低          |   高   |   高   |
|   支持外键   |      ==支持==       |   -    |   -    |



#### 存储引擎的选择

在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合。

- InnoDB: 如果应用对事物的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包含很多的更新、删除操作，则 InnoDB 是比较合适的选择
- MyISAM: 如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不高，那这个存储引擎是非常合适的。
- Memory: 将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。Memory 的缺陷是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性



### 索引

#### 索引概述

索引是帮助 MySQL **高效获取数据**的**数据结构（有序）**。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查询算法，这种数据结构就是索引。

优点：

- 提高数据检索效率，降低数据库的IO成本
- 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗

缺点：

- 索引列也是要占用空间的
- 索引大大提高了查询效率，但降低了更新的速度，比如 INSERT、UPDATE、DELETE



#### 索引结构

|       索引结构        |                             描述                             |
| :-------------------: | :----------------------------------------------------------: |
|        B+Tree         |          最常见的索引类型，大部分引擎都支持B+树索引          |
|         Hash          | 底层数据结构是用哈希表实现，只有精确匹配索引列的查询才有效，不支持范围查询 |
|  R-Tree（空间索引）   | 空间索引是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少 |
| Full-Text（全文索引） | 是一种通过建立倒排索引，快速匹配文档的方式，类似于Lucene，Solr，ES |

|   索引    |    InnoDB     | MyISAM | Memory |
| :-------: | :-----------: | :----: | :----: |
|  B+Tree   |     支持      |  支持  |  支持  |
|   Hash    |    不支持     | 不支持 |  支持  |
|  R-Tree   |    不支持     |  支持  | 不支持 |
| Full-Text | 5.6版本后支持 |  支持  | 不支持 |



##### 二叉树

![image-20250406212010546](D:\学习笔记\assets\image-20250406212010546.png)

二叉查找树（Binary Search Tree，BST）缺点：顺序插入时，会形成一个链表，查询性能大大降低。大数据量情况下，层级较深，检索速度慢

二叉树的缺点可以用红黑树来解决：

<img src="D:\学习笔记\assets\image-20250406212306918.png" alt="image-20250406212306918" style="zoom:50%;" />

红黑树也存在大数据量情况下，层级较深，检索速度慢的问题



##### B-Tree

为了解决上述问题，可以使用B-Tree结构

B-Tree（多路平衡查找树）以一颗最大度数（max-degree，指一个节点的子节点个数）为5（5阶）的B-Tree为例（每个节点最多存储4个key，5个指针）

![image-20250406213105237](D:\学习笔记\assets\image-20250406213105237.png)

> B-Tree 的数据插入过程动画参照：https://www.bilibili.com/video/BV1Kr4y1i7ru?p=68
> 演示地址：https://www.cs.usfca.edu/~galles/visualization/BTree.html



##### B+Tree

![image-20250406213421058](D:\学习笔记\assets\image-20250406213421058.png)

> 演示地址：https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html

与 B-Tree 的区别：

- 所有的数据都会出现在叶子节点
- 叶子节点形成一个单向链表

MySQL 索引数据结构对经典的 B+Tree 进行了优化。在原 B+Tree 的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的 B+Tree，提高区间访问的性能

![image-20250406213529323](D:\学习笔记\assets\image-20250406213529323.png)



##### Hash

哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中
如果两个（或多个）键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决

特点：

- Hash索引只能用于对等比较（=、in），不支持范围查询（betwwn、>、<、…）
- 无法利用索引完成排序操作
- 查询效率高，通常只需要一次检索就可以了，效率通常要高于 B+Tree 索引

存储引擎支持：

- Memory
- InnoDB: 具有自适应hash功能，hash索引是存储引擎根据 B+Tree 索引在指定条件下自动构建的



##### 为什么 InnoDB 存储引擎选择使用 B+Tree 索引结构？

- 相对于二叉树，层级更少，搜索效率高
- 对于 B-Tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，指针也跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低
- 相对于 Hash 索引，B+Tree 支持范围匹配及排序操作



#### 索引分类

|   分类   |                         含义                         |           特点           |  关键字  |
| :------: | :--------------------------------------------------: | :----------------------: | :------: |
| 主键索引 |               针对于表中主键创建的索引               | 默认自动创建，只能有一个 | primary  |
| 唯一索引 |           避免同一个表中某数据列中的值重复           |        可以有多个        |  unique  |
| 常规索引 |                   快速定位特定数据                   |        可以有多个        |          |
| 全文索引 | 全文索引查找的是文本中的关键词，而不是比较索引中的值 |        可以有多个        | fulltext |

在 InnoDB 存储引擎中，根据索引的存储形式，又可以分为以下两种：

|            分类             |                            含义                            |         特点         |
| :-------------------------: | :--------------------------------------------------------: | :------------------: |
| 聚集索引（Clustered Index） | 将数据存储与索引放到了一块，索引结构的叶子节点保存了行数据 | 必须有，而且只有一个 |
| 二级索引（Secondary Index） | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键 |     可以存在多个     |

聚集索引选取规则：

- 如果存在主键，主键索引就是聚集索引
- 如果不存在主键，将使用第一个唯一(UNIQUE)索引作为聚集索引
- 如果表没有主键或没有合适的唯一索引，则 InnoDB 会自动生成一个 rowid 作为隐藏的聚集索引

![image-20250406215219308](D:\学习笔记\assets\image-20250406215219308.png)

![image-20250406220123559](D:\学习笔记\assets\image-20250406220123559.png)



#### 索引语法

创建索引：
```mysql
CREATE [ UNIQUE | FULLTEXT ] INDEX index_name ON table_name (index_col_name, ...);
```

如果不加 CREATE 后面不加索引类型参数，则创建的是常规索引

查看索引：
```mysql
SHOW INDEX FROM table_name;
```

删除索引：
```mysql
DROP INDEX index_name ON table_name;
```



#### SQL性能分析

##### SQL执行频率

MySQL客户端连接成功后，通过show [ session | global ] status命令可以提供服务器状态信息。通过如下命令，可以查看当前数据库的insert、update、delete、select的访问频次：

```mysql
show global status like 'Com_______'; (7个下划线)
```



##### 慢查询日志

慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句的日志，可以通过慢查询日志定位执行时间较长的SQL。通过如下命令可以查看慢查询日志是否开启：

```mysql
show variables like 'slow_query_log';
```



##### profile详情

show profiles能够在做SQL优化时帮助我们了解时间都耗费到哪里去了。通过have_profiling参数，能够看到当前MySQL是否支持profile操作：

```mysql
select @@have_profiling;

# 默认profiling是关闭的，可以通过set语句在session/global级别开启profiling：
set profiling = 1;

# 查看每一条SQL的耗时基本情况：
show profiles;

#查看指定query_id的SQL语句各个阶段的耗时情况：
show profile for query_id;

#查看指定query_id的SQL语句CPU的使用情况：
show profile cpu for query_id;
```



##### explain执行计划

explain或者desc命令获取MySQL如何执行select语句的信息，包括在select语句执行过程中如何连接和连接的顺序。

语法：直接在select语句之前加上关键字explain/desc

```mysql
explain select 字段列表 from 表名 where 条件;
```

explain执行计划各字段含义：

- id

  select查询的序列号，表示查询中执行select子句或者是操作表的顺序（id相同，执行顺序从上到下；id不同，值越大，越先执行）

- select_type

​	表示select的类型，常见的取值有simple（简单表，即不使用表连接或者子查询）、primary（主查询，即外层的查询）、union（union中的第二个或者后面的查询语句）、subquery（select/where之后包含了子查询）等

- **type**

​	表示连接类型，性能由好到差的连接类型为NULL、system、const、eq_ref、ref、range、index、all

- possible_key

​	显示可能应用在这张表上的索引，一个或多个

- key

  实际使用的索引，如果为NULL，则没有使用索引

- key_len

​	表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好

- rows

  MySQL认为必须要执行查询的行数，在innodb引擎的表中，是一个估计值，可能并不总是准确的

- filtered

​	表示返回结果的行数占需读取行数的百分比，filtered的值越大越好

- extra

​	不适合在其他字段中显示，但是十分重要的额外信息



#### 索引使用

- 最左前缀法则

​	如果索引了多列（联合索引），要遵守最左前缀法则。最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列

​	如果跳过了某一列，**索引将部分失效（后面的字段索引失效）**



- 范围查询

​	联合索引中，出现范围查询（>,<），**范围查询右侧的列索引失效**



- 索引列运算

​	不要在索引列上进行运算操作，**否则索引将失效**



- 字符串不加引号

​	字符串类型字段使用时，不加引号，**索引将失效**



- 模糊查询

​	如果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，索引失效



- or连接的条件

​	用or分割开的条件，如果or前的条件中的列有索引，而后面的列中没有索引，那么涉及的索引都不会被用到（需都有索引才走索引）



- 数据分布影响

​	如果MySQL评估使用索引比全表更慢，则不使用索引



- SQL提示

​	是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的

​	use index：建议使用哪个索引，实际使用哪个索引MySQL会自己权衡运行速度去决定使用哪个索引

​	ignore index：不使用哪个索引

​	force index：强制使用哪个索引

```mysql
select * from 表名 [use/ignore/force index(索引名)] where 条件;
```



- 覆盖索引

​	尽量使用覆盖索引（查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到），减少select *的使用

​	explain中extra字段出现了：

​	`using index condition`：查找使用了索引，但是需要回表查询数据

​	`using where;using index`：查找使用了索引，需要的数据都在索引列中能找到，不需要回表查询数据，性能更好



- 前缀索引

  当字段类型为字符串（varchar, text等）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO，影响查询效率，此时可以只将字符串的一部分前缀，建立索引，这样可以大大节约索引空间，从而提高索引效率

​	语法：`create index idx_xxxx on table_name(columnn(n));` 其中n表示截取前几个字符

​	前缀长度：可以根据索引的选择性来决定，而选择性是指不重复的索引值（基数）和数据表的记录总数的比值，索引选择性越高则查	询效率越高，唯一索引的选择性是1，这是最好的索引选择性，性能也是最好的。

​	求选择性公式：

```mysql
select count(distinct column_name)/count(*) from table_name;
# 截取字段column_name的前五个字符计算选择性
select count(distinct substring(column_name,1,5))/count(*) from table_name;
```



- 单列索引与联合索引

​	单列索引：即一个索引只包含单个列

​	联合索引：即一个索引包含了多个列

​	在业务场景中，如果存在多个查询条件，考虑针对于查询字段建立索引时，建议使用联合索引，而非单列索引



==多条件联合查询时，MySQL优化器会评估哪个字段的索引效率更高，会选择该索引完成本次查询==



#### 索引设计原则

1. 针对于数据量较大，且查询比较频繁的表建立索引
2. 针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高
4. 如果是字符串类型的字段，字段长度较长，可以针对于字段的特点，建立前缀索引
5. 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率
6. 要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价就越大，会影响增删改的效率
7. 如果索引列不能存储NULL值，请在创建表时使用NOT NULL约束它。当优化器知道每列是否包含NULL值时，它可以更好地确定哪个索引最有效地用于查询



### SQL优化

#### 插入数据

- insert优化
  - 批量插入（一次插入的数据不建议超过1000条）
  - 手动提交事务
  - 主键顺序插入
- 大批量数据插入

​	如果一次性需要插入大批量数据，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令插入

```mysql
# 客户端连接服务器时，加上参数 --local-infile
mysql --local-infile -u root -p
# 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
select @@local_infile;
set global local_infile=1;
# 执行load指令将准备好的数据加载到表结构中,其中','与'\n'分别是文件中的内容以逗号分隔，以\n结束一行数据
load data local infile '文件路径' into table '表名' fields terminated by ',' lines terminated by '\n';
```



#### 主键优化

- 数据组织方式

​	在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为**索引组织表**（Index organized table, IOT）

- 页分裂

  页可以为空，也可以填充一半，也可以填充100%，每个页包含了2到N行数据（如果一行数据过大，会行溢出），根据主键排列

- 页合并

  当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged）为删除并且它的空间变得允许被其他记录声明使用。当页中删除的记录到达 MERGE_THRESHOLD（默认为页的50%），InnoDB会开始寻找最靠近的页（前后）看看是否可以将这两个页合并以优化空间使用

MERGE_THRESHOLD：合并页的阈值，可以自己设置，在创建表或创建索引时指定

- 主键设计原则
  - 满足业务需求的情况下，尽量降低主键的长度
  - 插入数据时，尽量选择顺序插入，选择使用auto_increment自增主键
  - 尽量不要使用UUID做主键或者是其他的自然主键，如身份证
  - 业务操作时，避免对主键的修改

主键有序插入性能比主键无序高，无序可能会产生页分裂，影响性能



#### order by优化

Using filesort：通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区 sort buffer 中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序

Using index：通过有序索引顺序扫描直接返回有序数据，这种情况即为 using index，不需要额外排序，操作效率高

- 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则
- 尽量使用覆盖索引
- 多字段排序，一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）
- 如果不可避免出现filesort，大数据量排序时，可以适当增大排序缓冲区大小 sort_buffer_size（默认256k）



#### group by优化

- 在分组操作时，可以通过索引来提高效率
- 分组操作时，索引的使用也是满足最左前缀法则的



#### limit优化

常见的问题如`limit 2000000, 10`，此时需要 MySQL 排序前2000000条记录，但仅仅返回2000000 - 2000010的记录，其他记录丢弃，查询排序的代价非常大

优化方案：一般分页查询时，通过创建覆盖索引能够比较好地提高性能，可以通过覆盖索引加子查询形式进行优化



#### count优化

MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 count(\*) 的时候会直接返回这个数，效率很高（前提是不用where）

InnoDB 在执行 count(*) 时，需要把数据一行一行地从引擎里面读出来，然后累计计数

优化方案：自己计数，如创建key-value表存储在内存或硬盘，或者是用redis

count的几种用法：

- 如果count函数的参数（count里面写的那个字段）不是NULL（字段值不为NULL），累计值就加一，最后返回累计值
- 用法：count(*)、count(主键)、count(字段)、count(1)
- count(主键)跟count(\*)一样，因为主键不能为空；count(字段)只计算字段值不为NULL的行；count(1)引擎会为每行添加一个1，然后就count这个1，返回结果也跟count(*)一样；count(null)返回0

各种用法的性能：

- count(主键)

  InnoDB引擎会遍历整张表，把每行的主键id值都取出来，返回给服务层，服务层拿到主键后，直接按行进行累加（主键不可能为空）

- count(字段)

  没有not null约束的话，InnoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，服务层判断是否为null，不为null，计数累加；

  有not null约束的话，InnoDB引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加

- count(1)

  InnoDB 引擎遍历整张表，但不取值。服务层对于返回的每一层，放一个数字 1 进去，直接按行进行累加

- count(*)

  InnoDB 引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加

按效率排序：count(字段) < count(主键 id) < count(1) ≈ count(\*)，所以尽量使用 count(*)



#### update优化（避免行锁升级为表锁）

==InnoDB 的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁，从而导致并发性能降低==



### 视图

视图(View)是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的

通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上

创建视图：

```mysql
CREATE [OR REPLACE] VIEW 视图名称(列名列表)] AS SELECT语句 [WITH[CASCADED|LOCAL] CHECK OPTION]
```

查看创建视图语句：

```mysql
SHOW CRETE VIEW 视图名称;
```

查看视图数据：

```mysql
SELECT * FROM 视图名称…;
```

修改视图：

```mysql
CREATE [OR REPLACE]VIEW 视图名称(列名列表)AS SELECT语句[WITH[CASCADEDLLOCAL] CHECK OPTION
ALTER VIEW 视图名称(列名列表)AS SELECT语句[WITH[CASCADED|LOCAL]CHECK OPTION]
```

删除视图：

```mysql
DROP VIEW [IF EXISTS]视图名称[,视图名称]
```



#### 视图的检查选项

当使用`WITH CHECK OPTION`子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如 插入，更新，删除，以使其符合视图的定义。MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性

为了确定检查的范围，MySQL提供了两个选项:CASCADED 和 LOCAL，默认值为CASCADED

**CASCADED：**在对创建时含有该字段的视图，插入数据时，该视图依赖的视图都会加上检查，需要**所有条件**都满足才能够插入成功

**LOCAL：**在对创建时含有该字段的视图，插入数据时，对于该视图依赖的视图中**含有检查语句的条件**进行检查判断



#### 视图的更新

要使视图可更新，视图中的行与基础表中的行之间**必须存在一对一的关系**。

如果视图包含以下任何一项，则该**视图不可更新**：

1. 聚合函数或窗口函数（SUM()、MIN()、MAX()、COUNT()等）
2. DISTINCT
3. GROUP BY
4. HAVING
5. UNION 或者 UNION ALL



#### 视图的作用

- **简单**
  视图不仅可以简化用户对数据的理解，也可以简化他们的操作。那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件
- **安全**
  数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图用户只能查询和修改他们所能见到的数据
- **数据独立**
  视图可帮助用户屏蔽真实表结构变化带来的影响



### 存储过程

存储过程是事先经过编译并存储在数据库中的一段 SQL语句的集合，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。

存储过程思想上很简单，就是数据库 SQL语言层面的代码封装与重用。

特点：

- 封装，复用
- 可以接收参数，也可以返回数据
- 减少网络交互，效率提升

基本语法：

```mysql
# 创建
create procedure 存储过程名([参数列表])
begin
  # sql语句
end;

# 调用
call 存储过程名([参数列表]);

# 查询指定数据库的存储过程及状态信息
select * from information_schema.ROUTINES where ROUTINE_SCHEMA = 'xxx';
# 查询某个存储过程的定义
show create procedure 存储过程名;

# 删除
drop procedure [if exists] 存储过程名;
```

> 注：在命令行中，执行创建存储过程的SQL时，需要通过关键字delimiter指定SQL语句的结束符

 

#### 变量

##### 系统变量

是MySQL服务器提供，不是用户定义的，属于服务器层面。分为全局变量(GLOBAL)、会话变量(SESSION)

```mysql
# 查看所有系统变量
show [session|global] variables;

# 可以通过like模糊匹配方式查找变量
show [session|global] variables like '';

# 查看指定变量的值
select @@[session|global]系统变量名;

# 设置系统变量
set [session|global] 系统变量名 = 值;
set @@[session|global] 系统变量名 = 值;
```

> 如果没有指定 session / global，默认 session，会话变量
>
> mysql服务器重新启动后，所设置的全局参数会失效（初始化为默认值），要想不失效，可以在配置文件中修改



##### 用户自定义变量

是用户根据需要自己定义的变量，用户变量不用提前声明，在用的时候直接用”@变量名“使用就可以。其作用域为当前连接

```mysql
# 赋值
set @var_name = expr[,@var_name = expr]...;
set @var_name := expr[,@var_name := expr]...;

select @var_name := expr[,@var_name := expr]...;
# 查询出来的结果通过into关键字赋值给@var_name
select 字段名 into @var_name from 表名;

# 使用
select @var_name;
```

> 用户定义的变量无需对其进行声明或初始化，只不过获取到的值为NULL
>
> 因为MySQL没有 == ，判断两个值是否相等也是使用 = ，为了区分，推荐使用 := 进行赋值



##### 局部变量

是根据需要定义的在局部生效的变量，访问之前，需要declare声明。可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的begin...end块

```mysql
# 声明,变量类型就是数据库字段类型：int、bigint、char、varchar、date、time等
declare 变量名 变量类型[default ...];

# 赋值
set 变量名 = 值;
set 变量名 := 值;
select 字段名 into 变量名 from 表名;
```



#### if判断

```mysql
if 条件1 then
	...
elseif 条件2 then    -- 可选
	...
else                -- 可选
	...
end if;
```



#### 参数（in，out，inout）

| 类型  |                     含义                     | 备注 |
| :---: | :------------------------------------------: | :--: |
|  in   |   该类参数作为输入，也就是需要调用时传入值   | 默认 |
|  out  | 该类参数作为输出，也就是该参数可以作为返回值 |      |
| inout |    既可以作为输入参数，也可以作为输出参数    |      |

```mysql
create procedure 存储过程名([in/out/inout 参数名 参数类型])
begin
  # sql语句
end;
```



#### case

```mysql
CASE case_value
	WHEN when_value1 THEN statement_list1
	[WHEN when_value2 THEN statement_list2]...
	[ELSE statement_list]
END CASE;

CASE
	WHEN search_condition1 THEN statement_list1
  	WHEN search_condition2 THEN statement_list2]...
  	[ELSE statement_list]
END CASE;
```



#### 循环

##### while

while 循环是有条件的循环控制语句。满足条件后，再执行循环体中的SQL语句

```mysql
# 先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
WHILE 条件 DO
  SQL逻辑...
END WHILE;
```



##### repeat

repeat是有条件的循环控制语句,当满足条件的时候退出循环

```mysql
#先执行一次逻辑，然后判定逻辑是否满足，如果满足，则退出。如果不满足，则继续下一次循环
REPEAT
  SQL逻辑...
  UNTIL 条件
END REPEAT;
```



##### loop

loop实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。loop可以配合一下两个语句使用

- LEAVE:配合循环使用，退出循环。
- ITERATE:必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环。

```mysql
[begin label:] LOOP
  SQL逻辑..
END LOOP [end label];

LEAVE label;  -- 退出指定标记的循环体
ITERATE label;-- 直接进入下一次循环

```



#### 游标-cursor

游标(CURSOR)是用来存储查询结果集的数据类型,在存储过程和函数中可以使用游标对结果集进行循环的处理。游标的使用包括游标的声明、OPEN、FETCH和 CLOSE

```mysql
# 声明游标
declare 游标名称 cursor for 查询语句;

# 打开游标
open 游标名称;

# 获取游标记录
fetch 游标名称 into 变量[,变量];

# 关闭游标
close 游标名称;
```

> 声明变量需要定义在声明游标之前



#### 条件处理程序-handler

条件处理程序(Handler)可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤

```mysql
DECLARE handler_action HANDLER FOR condition_value [, condition_value].... statement;

handler_action
  CONTINUE: 继续执行当前程序
  EXIT: 终止执行当前程序
  
condition_value
  SQLSTATE sqlstate_value:状态码，如 02000
  SQLWARNING:所有以01开头的SQLSTATE代码的简写
  NOT FOUND:所有以02开头的SQLSTATE代码的简写
  SQLEXCEPTION:所有没有被SQLWARNING 或 NOT FOUND捕获的SQLSTATE代码的简写
```



#### 存储函数

存储函数是有返回值的存储过程，存储函数的参数只能是IN类型的

存储函数用的较少，能够使用存储函数的地方都可以用存储过程替换

```mysql
CREATE FUNCTION 存储函数名称([ 参数列表 ])
RETURNS type [characteristic ...]
BEGIN
  -- SQL语句
  RETURN ...;
END ;

characteristic说明:
· DETERMINISTIC:相同的输入参数总是产生相同的结果
· NO SQL:不包含SQL语句
· READS SQL DATA:包含读取数据的语句，但不包含写入数据的语句
```



### 触发器

触发器是与表有关的数据库对象，指在 insert/update/delete 之前或之后，触发并执行触发器中定义的SQL语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性，日志记录，数据校验等操作

使用别名 OLD 和 NEW 来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发

| 触发器类型 |                        new和old                        |
| :--------: | :----------------------------------------------------: |
|   insert   |             new表示将要或者已经新增的数据              |
|   update   | old表示修改之前的数据，new表示将要或者已经修改后的数据 |
|   delete   |             old表示将要或者已经删除的数据              |

```mysql
# 创建
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE
ON table_name FOR EACH ROW -- 行级触发器
BEGIN
  trigger_stmt;
END;

# 查看
show trigger;

# 删除
drop trigger [schema_name.]trigger_name; -- 如果没有指定schema_name，默认为当前数据库
```



### 锁

锁是计算机协调多个进程或线程并发访问某一资源的机制。在数据库中，除传统的计算资源(CPU、RAM、I/0)的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂

MySQL中的锁，按照锁的粒度分，分为以下三类：

1. 全局锁：锁定数据库中的所有表
2. 表级锁：每次操作锁住整张表
3. 行级锁：每次操作锁住对应的行数据



#### 全局锁

全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML语句，DDL语句，已经更新操作的事务提交语句都将被阻塞

其典型的使用场景是做全库的逻辑备份，对所有的表进行锁定，从而获取一致性视图，保证数据的完整性

![image-20250409163852826](D:\学习笔记\assets\image-20250409163852826.png)

![image-20250409164618553](D:\学习笔记\assets\image-20250409164618553.png)

使用全局锁：`flush tables with read lock;`
释放全局锁：`unlock tables;`

数据库中加全局锁，是一个比较重的操作，存在以下问题:

1. 如果在主库上备份，那么在备份期间都不能执行更新，业务基本上就得停摆
2. 如果在从库上备份，那么在备份期间从库不能执行主库同步过来的二进制日志(binlog)，会导致主从延迟

在InnoDB引擎中，我们可以在备份时加上参数 --single-transaction 参数来完成不加锁的一致性数据备份



#### 表级锁

每次操作锁住整张表。锁定粒度大，发生锁的冲突的概率最高，并发度最低。应用在MyISAM、InnoDB、BDB等存储引擎中

对于表级锁，主要分为以下三类：

1. 表锁
2. 元数据锁（meta data lock，MDL）
3. 意向锁



##### 表锁

对于表锁，分为两类：

1. 表共享读锁（read lock）
2. 表独占写锁（write lock）

语法：

1. 加锁：lock tables 表名... read/write
2. 释放锁：unlock tables / 客户端连接断开

**读锁不会阻塞其他客户端的读，但是会阻塞写。写锁既会阻塞其他客户端的读，又会阻塞其他客户端的写**

<img src="D:\学习笔记\assets\image-20250409170523558.png" alt="image-20250409170523558" style="zoom: 67%;" />



##### 元数据锁（meta data lock，MDL）

MDL加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上。MDL锁主要作用是维护表元数据的数据一致性，在表
上有活动事务的时候，不可以对元数据进行写入操作。==为了避免DML与DDL冲突，保证读写的正确性==

在MySQL5.5中引入了MDL，当对一张表进行增删改查时，加MDL读锁（共享）；当对表结构进行变更操作时，加MDL写锁（排他）

|                    对应SQL                    |                锁类型                 |                       说明                       |
| :-------------------------------------------: | :-----------------------------------: | :----------------------------------------------: |
|          lock tables xxx read/write           | shared_read_only/shared_no_read_write |                                                  |
|     select、select ... lock in share mode     |              shared_read              | 与shared_read、shared_write兼容，与exclusive互斥 |
| insert、update、delete、select ... for update |             shared_write              | 与shared_read、shared_write兼容，与exclusive互斥 |
|                alter table ...                |               exclusive               |                与其他的MDL都互斥                 |

查看元数据锁：`select *from performance_schema.metadata_locks;`



##### 意向锁

为了避免DML在执行时，加的行锁与表锁的冲突，在InnoDB中引入了意向锁，使得表锁不用检查每行数据是否加锁，使用意向锁来减
少表锁的检查

- 意向共享锁（IS）：

  由语句select ... lock in share mode添加

  与表锁共享锁（read）兼容，与表锁排它锁（write）互斥

- 意向排他锁（IX）：

  由insert、update、delete、select ... for update添加

  与表锁共享锁（read）及排它锁（wtite）都互斥。意向锁之间不会互斥

查看意向锁：`select *from performance_schema.data_locks;`



#### 行级锁

行级锁，每次操作锁住对应的行数据。锁定粒度最小，发生锁冲突的概率最低，并发度最高。应用在InnoDB存储引擎中

InnoDB的数据是基于索引组织的，行锁是通过对索引上的索引项加锁来实现的，而不是对记录加的锁。对于行级锁，主要分为以下三类：

1. 行锁(Record Lock)：锁定单个行记录的锁，防止其他事务对此行进行update和delete。在RC、RR隔离级别下都支持
2. 间隙锁(GapLock)：锁定索引记录间隙(不含该记录)，确保索引记录间隙不变，防止其他事务在这个间隙进行insert，产生幻读。在RR隔离级别下支持
3. 临键锁(Next-Key Lock)：行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙Gap。在RR隔离级别下支持



##### 行锁

InnoDB实现了以下两种类型的行锁：

1. 共享锁(S)：允许一个事务去读一行，阻止其他事务获得相同数据集的排它锁
2. 排它锁(X)：允许获取排它锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排它锁

|             | S（共享锁） | X（排它锁） |
| :---------: | :---------: | :---------: |
| S（共享锁） |    兼容     |    冲突     |
| X（排它锁） |    冲突     |    冲突     |



|              SQL              |  行锁类型  |                  说明                  |
| :---------------------------: | :--------: | :------------------------------------: |
|    insert、update、delete     |   排它锁   |                自动加锁                |
|            select             | 不加任何锁 |                                        |
| select ... lock in share mode |   共享锁   | 需手动在select之后加lock in share mode |
|     select ... for update     |   排它锁   |     需手动在select之后加for update     |



默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key锁进行搜索和索引扫描，以防止幻读

1. 针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将会自动优化为行锁
2. InnoDB的行锁是针对于索引加的锁，不通过索引条件检索数据，那么InnoDB将对表中的所有记录加锁，此时 **就会升级为表锁**



##### 间隙锁/临键锁

默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key锁进行搜索和索引扫描，以防止幻读

1. 索引上的等值查询（唯一索引），给不存在的记录加锁时，优化为间隙锁
2. 索引上的等值查询（普通索引），向右遍历时最后一个值不满足查询需求时，next-key lock退化为间隙锁
3. 索引上的范围查询（唯一索引），会访问到不满足条件的第一个值为止

> 间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁



### InnoDB引擎

#### 逻辑存储结构

![image-20250411180032581](D:\学习笔记\assets\image-20250411180032581.png)



#### 架构

MySQL5.5版本开始，默认使用InnoDB存储引擎，它擅长事务处理，具有崩溃恢复特性，在日常开发中使用非常广泛。下面是InnoDB架构图，左侧为内存结构，右侧为磁盘结构：

![image-20250411180622228](D:\学习笔记\assets\image-20250411180622228.png)



##### 内存架构

![image-20250411180838956](D:\学习笔记\assets\image-20250411180838956.png)



![image-20250411181351205](D:\学习笔记\assets\image-20250411181351205.png)



![image-20250411181528650](D:\学习笔记\assets\image-20250411181528650.png)



![image-20250411181832357](D:\学习笔记\assets\image-20250411181832357.png)

##### 磁盘结构

![image-20250411182435750](D:\学习笔记\assets\image-20250411182435750.png)



![image-20250411182508921](D:\学习笔记\assets\image-20250411182508921.png)



![image-20250411182532489](D:\学习笔记\assets\image-20250411182532489.png)



##### 后台线程

![image-20250411182834054](D:\学习笔记\assets\image-20250411182834054.png)



#### 事务原理

![image-20250411183813533](D:\学习笔记\assets\image-20250411183813533.png)



##### redo log

重做日志，记录的是事务提交时数据页的物理修改，是用来实现事务的**持久性**

该日志文件由两部分组成：重做日志缓冲(redo log buffer)以及重做日志文件(redo log file)，前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都存到该日志文件中，用于在刷新脏页到磁盘，发生错误时，进行数据恢复使用

![image-20250411192442376](D:\学习笔记\assets\image-20250411192442376.png)



##### undo log

回滚日志，用于记录数据被修改前的信息，作用包含两个：提供回滚和MVCC（多版本并发控制）

undo log 和 redo log 记录物理日志不一样，它是逻辑日志。可以认为当 delete 一条记录时，undo log中会记录一条对应的insert记录，反之亦然，当 update 一条记录时，它记录一条对应相反的 update 记录。当执行 rollback 时，就可以从 undo log 中的逻辑记录读取到相应的内容并进行回滚

Undo log 销毁：undo log在事务执行时产生，事务提交时，并不会立即删除undo log，因为这些日志可能还用于MVCC

Undo log 存储：undo log采用段的方式进行管理和记录，存放在rollback segment回滚段中，内部包含1024个undo log segment



#### MVCC

- 当前读

  读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。对于我们日常的操作，如:select…lock in share mode(共享锁)，select… for update、update、insert、delete(排他锁)都是一种当前读

- 快照读

  简单的select(不加锁)就是快照读，快照读，读取的是记录数据的可见版本，有可能是历史数据，不加锁，是非阻塞读

  - Read committed：每次select，都生成一个快照读
  - Repeatable Read：开启事务后第一个select语句才是快照读的地方
  - Serializable：快照读会退化为当前读

- MVCC

  全称 Multi-Version Concurrency Control，多版本并发控制。指维护一个数据的多个版本，使得读写操作没有冲突，快照读为MySQL实现MVCC提供了一个非阻塞读功能。MVCC的具体实现，还需要依赖于数据库记录中的三个隐式字段、undo log日志、read View

  - 隐式字段

    ![image-20250411194028597](D:\学习笔记\assets\image-20250411194028597.png)

  - undo log

    - 回滚日志，在insert、update、delete的时候产生的便于数据回滚的日志

    - 当insert的时候，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除

    - 而update、delete的时候，产生的undo log日志不仅在回滚时需要，在快照读时也需要，不会立即被删除

    - undo log版本链

      ![image-20250411194856975](D:\学习笔记\assets\image-20250411194856975.png)

  - ReadView

    ReadView(读视图)是**快照读**SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃的事务(未提交的)id

    ReadView包含四个核心字段：

    - m_ids：当前活跃的事务ID集合
    - min_trx_id：最小活跃事务ID
    - max_trx_id：预分配事务ID，当前最大事务ID+1（因为事务ID是自增的）
    - creator_trx_id：ReadView创建者的事务ID

    ![image-20250411195639257](D:\学习笔记\assets\image-20250411195639257.png)

    ![image-20250411200238953](D:\学习笔记\assets\image-20250411200238953.png)

    ![image-20250411200502932](D:\学习笔记\assets\image-20250411200502932.png)



### MySQL管理

#### 系统数据库

MySQL数据库安装完成后，自带了以下四个数据库，具体作用如下：

|       数据库       |                             含义                             |
| :----------------: | :----------------------------------------------------------: |
|       mysql        | 存储MySQL服务器正常运行所需要的各种信息（时区、主从、用户、权限等） |
| information_schema | 提供了访问数据库元数据的各种表和视图，包含数据库、表、字段类型及访问权限等 |
| performance_schema | 为MySQL服务器运行时状态提供了一个底层监控功能，主要用于收集数据库服务器性能参数 |
|        sys         | 包含了一系列方便DBA和开发人员利用performance_schema性能数据库进行性能调优和诊断的视图 |



#### 常用工具

- mysql

  该mysql不是指mysql服务，而是指mysql的客户端工具

  ```mysql
  # 语法
  mysql [options] [database]
  # 选项
  -u # 指定用户名
  -p # 指定密码
  -h # 指定服务器ip或域名
  -P # 指定端口号
  -e # 执行sql语句并推出
  # -e选项可以在mysql客户端执行sql语句，而不用连接到mysql数据库再执行，对于一些批处理脚本，这种方式尤其方便
  mysql -uroot -proot db_name -e "select * from table_name";
  ```

- mysqladmin

  是一个执行管理操作的客户端程序。可以用来检查服务器的配置和当前状态、创建并删除数据库等

- mysqlbinlog

  由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到mysqlbinlog日志管理工具

  ```mysql
  # 语法
  mysqlbinlog [options] log-files1 logfiles2 ...
  # 选项
  -d # 指定数据库名称，只列出指定的数据库相关操作
  -o # 忽略掉日志中的前n行命令
  -r # 将输出的文本格式日志输出到指定文件
  -s # 显示简单格式，省略掉一些信息
  --start-datatime=date1 --stop-datetime=date2 # 指定日期间隔内的所有日志
  --start-position=pos1 --stop-position=pos2 # 指定位置间隔内的所有日志
  ```

- mysqlshow

  客户端对象查找工具，用来很快地查找存在哪些数据库、数据库中的表、表中的列或者索引

  ```mysql
  mysqlshow [options] [db_name[table_name[col_name]]]
  --count # 显示数据库及表的统计信息（数据库、表均可以不指定）
  -i # 显示指定数据库或者指定表的状态信息
  ```

- mysqldump

  客户端工具用来备份数据库或在不同数据库之间进行数据迁移。备份内容包含创建表及插入表的sql语句

  ```mysql
  # 语法
  mysqldump [options] db_name [tables]
  mysqldump [options] --database/-B db1[db2 db3...]
  mysqldump [options] --all-databases/-A
  # 连接选项
  -u
  -p
  -h
  -P
  # 输出选项
  --add-drop-database # 在每个数据库创建语句前加上drop database语句
  --add-drop-table # 在每个表创建语句前加上drop table语句，默认开启；不开启（--skip-add-drop-table）
  -n # 不包含数据库的创建语句
  -t # 不包含数据表的创建语句
  -d # 不包含数据
  -T # 自动生成两个文件，一个sql文件，创建表结构的语句，一个txt文件，数据文件
  ```

- mysqlimport/source

  mysqlimport是客户端数据导入工具，用来导入mysqldump加-T参数后导出的文本文件

  ```mysql
  mysqlimport [options] db_name textflie1 [textfile2...]
  # 如果需要导入sql文件，可以使用mysql中的source指令（这是在mysql客户端中使用的）
  source /xxx.sql # source 后面加文件地址
  ```





​	