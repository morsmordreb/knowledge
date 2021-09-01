View Default 

- SQL 代码规范 (OLTP)
  - [1.前言](https://git.woa.com/standards/sql/blob/master/README.md#1前言)
  - [1.1 为什么要有 OLTP SQL 规范](https://git.woa.com/standards/sql/blob/master/README.md#11-为什么要有-oltp-sql-规范)
  - 2.建表规约
    - 命名规范
      - [【必须】](https://git.woa.com/standards/sql/blob/master/README.md#必须)
      - [【推荐】](https://git.woa.com/standards/sql/blob/master/README.md#推荐)
      - [【可选】](https://git.woa.com/standards/sql/blob/master/README.md#可选)
    - 数据类型
      - [【必须】](https://git.woa.com/standards/sql/blob/master/README.md#必须)
      - [【推荐】](https://git.woa.com/standards/sql/blob/master/README.md#推荐)
      - [【可选】](https://git.woa.com/standards/sql/blob/master/README.md#可选)
  - 3.索引规约
    - [【必须】](https://git.woa.com/standards/sql/blob/master/README.md#必须)
    - [【推荐】](https://git.woa.com/standards/sql/blob/master/README.md#推荐)
    - [【可选】](https://git.woa.com/standards/sql/blob/master/README.md#可选)
  - 4.SQL 语句
    - [【必须】](https://git.woa.com/standards/sql/blob/master/README.md#必须)
    - [【推荐】](https://git.woa.com/standards/sql/blob/master/README.md#推荐)
    - [【可选】](https://git.woa.com/standards/sql/blob/master/README.md#可选)
- SQL代码规范 (OLAP)
  - 1.前言
    - [1.1 为什么要有OLAP SQL规范](https://git.woa.com/standards/sql/blob/master/README.md#11-为什么要有olap-sql规范)
    - [1.2 采用等级约定](https://git.woa.com/standards/sql/blob/master/README.md#12-采用等级约定)
    - [1.3 应用范围](https://git.woa.com/standards/sql/blob/master/README.md#13-应用范围)
    - [1.4 对规范存在异议时候，如何提交建议及变更流程](https://git.woa.com/standards/sql/blob/master/README.md#14-对规范存在异议时候如何提交建议及变更流程)
  - 2.命名规范
    - [2.1 通用规则](https://git.woa.com/standards/sql/blob/master/README.md#21-通用规则)
    - [2.2 库命名](https://git.woa.com/standards/sql/blob/master/README.md#22-库命名)
    - [2.3 表命名](https://git.woa.com/standards/sql/blob/master/README.md#23-表命名)
    - [2.4 字段命名](https://git.woa.com/standards/sql/blob/master/README.md#24-字段命名)
  - [3.编码格式规范](https://git.woa.com/standards/sql/blob/master/README.md#3编码格式规范)
  - [4.查询语句规范](https://git.woa.com/standards/sql/blob/master/README.md#4查询语句规范)
  - [5.DDL&DML规范](https://git.woa.com/standards/sql/blob/master/README.md#5ddldml规范)
  - [6.Code Review 规则](https://git.woa.com/standards/sql/blob/master/README.md#6code-review-规则)
  - 7.不同数据引擎下的使用建议
    - [7.1 spark/hive](https://git.woa.com/standards/sql/blob/master/README.md#71-sparkhive)
    - [7.2 impala](https://git.woa.com/standards/sql/blob/master/README.md#72-impala)
    - [7.3 clickhouse](https://git.woa.com/standards/sql/blob/master/README.md#73-clickhouse)

# SQL 代码规范 (OLTP)

## 1.前言

本规范在 [Alibaba-Java-Coding-Guidelines-mysql-rules](https://alibaba.github.io/Alibaba-Java-Coding-Guidelines/#3-mysql-rules)的基础上，根据腾讯实际情况进行了调整和补充。同时还参考了公司内部 KM 规范，SQL Programming Style 书籍等。

- [SQL Style Guide](https://www.sqlstyle.guide/)
- [财付通数据库 SQL 编写规范](http://km.oa.com/group/22807/articles/show/297312)

## 1.1 为什么要有 OLTP SQL 规范

不正确、不规范的 SQL 会有以下问题

- 查询出错误数据
- 查询性能差
- 执行时间长，期间会锁住设计表 DDL 操作等
- 可能会占用大量临时表空间影响其他正常查询
- RDS 通过 IDB 执行的 sql 没有超时退出，sql 可能会在后台一直执行

故为形成公司统一的 SQL 编码风格，以保障公司项目代码的易维护性和编码安全性，特制定本规范。

每项规范内容，给出了要求等级，其定义为：

- **必须（Mandatory）**：用户必须采用；
- **推荐（Preferable）** ：用户理应采用，但如有特殊情况，可以不采用；
- **可选（Optional）** ：用户可参考，自行决定是否采用

## 2.建表规约

### 命名规范

#### 【必须】

1. 表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字，不能以下划线结尾。

   > 数据库字段名的修改代价很大，因为无法进行预发布，所以字段名称需要慎重考虑。

   - 说明：MySQL 在 Windows 下不区分大小写，但在 Linux 下默认是区分大小写。因此，数据库名、表名、字段名，都不允许出现任何大写字母，避免节外生枝。

   | 正例                          | 反例                         |
   | :---------------------------- | :--------------------------- |
   | choerodon_admin， level3_name | choerodonAdmin，level_3_name |

2. 表名使用集合术语、类，尽量少使用复数名称。

   - 说明： 因为表示一个集合，不是标量值，所以集合或类名称比单数名称好。如果表中有且只有一行，则使用单数形式的表名，如常量表。

   | 正例           | 反例                 |
   | :------------- | :------------------- |
   | staff， people | employee，individual |

3. 库名与应用名称尽量一致，在命名表的列时，不要重复表的名称。

   - 说明：在名为`staff`的表中避免使用名为`staff_lastname`的字段

4. 禁用保留字，如 `desc`、 `range`、 `match`、 `delayed` 等， 请参考 MySQL 官方保留字。

5. 主键索引名为

    

   ```
   pk_字段名
   ```

   ； 唯一索引名为

    

   ```
   uk_字段名
   ```

   ； 普通索引名则为

    

   ```
   idx_字段名
   ```

   。

   - 说明： pk*即 primary key； uk* 即 unique key； idx_ 即 index 的简称。

6. 表达是与否概念的字段，必须使用

    

   ```
   is_xxx
   ```

    

   的方式命名，数据类型是

    

   ```
   unsigned tinyint
   ```

    

   （

   ```
   1
   ```

    

   表示是，

    

   ```
   0
   ```

    

   表示否）。

   - 说明：任何字段如果为非负数，必须是 `unsigned`。

   | 正例                                                         |
   | :----------------------------------------------------------- |
   | 表达逻辑删除的字段名 `is_deleted`， 1 表示删除， 0 表示未删除 |

7. 避免使用驼峰命名法，会使阅读速度减慢。请使用

   snake-cased

   - 说明：如果单词本来就符合驼峰命名法，如 MacDonald 可以这样使用。

#### 【推荐】

1. 为了方便维护，建表时需要对相应字段填写对应的描述信息。

2. 如果修改字段含义或对字段表示的状态追加时，需要及时更新字段注释。

3. 字段允许适当冗余，以提高查询性能，但必须考虑数据一致。冗余字段应遵循：

   - 不是频繁修改的字段。
   - 不是 `varchar` 超长字段，更不能是 `text` 字段。

   | 正例                                                         |
   | :----------------------------------------------------------- |
   | 商品类目名称使用频率高， 字段长度短，名称基本一成不变， 可在相关联的表中冗余存储类目名称，避免关联查询。 |

4. 表推荐必备字段：

    

   ```
   id
   ```

   ,

    

   ```
   create_time
   ```

   ,

    

   ```
   update_time
   ```

   - 说明： 其中 id 必为主键，类型为 unsigned bigint、未来若分库分表须以雪花算法或者其他生算法生成，并保持有序性。
   - `create_date`, `update_time` 的类型均为 datetime 类型。

5. 时间戳（timestamp 保存精度到毫秒）应该以

   ```
   _at
   ```

   后缀为结尾，日期（date 保存精度到天）应该以

   ```
   _date
   ```

   后缀为结尾，月份应该以

   ```
   _month
   ```

   后缀为结尾。

   | 正例                       |
   | :------------------------- |
   | deal_closed_at，birth_date |

6. 库名、表名、列名长度不要超过 32 个字符

7. 表的命名最好是加上“业务名称_表的作用”，同时表名称不应该取得太长（一般不超过三个英文单词）

   | 正例                                                |
   | :-------------------------------------------------- |
   | `kanban_task` / `devops_project` / `website_config` |

#### 【可选】

1. 下列后缀有统一的意义，能保证 SQL 代码更容易理解。
   - _id —— 独一无二的标识符，如主键。
   - _status —— 标志值或任何表示状态的值，比如 publication_status。
   - _total —— 总和或某些值的和。
   - _num —— 表示该字段包含数值。
   - _name —— 表示名字，例如 first_name。
   - _seq —— 包含一系列值。
   - _date —— 表示该列包含日期。
   - _tally —— 计数值。
   - _size —— 大小，如文件大小或服装大小。
   - _addr —— 地址，有形的或无形的，如 ip_addr
   - _code —— 编码，是由一个可靠来源维护的一个标准，如邮政编码
   - _class —— 内部编码，没有外部源来反映实体的子分类
   - _img —— 图像数据类型，例如.jpg 和.gif
2. 相关模块的表名与表名之间尽量体现 join 的关系，如 user 表和 user_login 表。
3. 单表不要有太多字段，建议在 20 个以内
4. 库的名称格式：`业务系统名称\_子系统名`，同一模块使用的表名尽量使用统一前缀
5. 核心表（如用户表，金钱相关的表）必须有行数据的创建时间字段和最后更新时间字段，便于查问题。

### 数据类型

#### 【必须】

1. 小数类型为

    

   ```
   decimal
   ```

   ，禁止使用 float 和 double。

   - 说明： float 和 double 在存储的时候，存在精度损失的问题，很可能在值的比较时，得到不正确的结果。如果存储的数据范围超过 decimal 的范围，建议将数据拆成整数和小数分开存储。

2. 如果存储的字符串长度几乎相等，使用 char 定义长字符串类型。

3. varchar 是可变长字符串，不预先分配存储空间，长度不要超过 5000，如果存储长度大于此值，定义字段类型为 text，独立出来一张表，用主键来对应，避免影响其它字段索引效率。

   - 说明： 该表的命名以 `原表名_字段缩写` 的格式命名。

4. 大部分场景下应该还是 datetime，跨时区场景用 timestamp，对排序性能要求高并且未来年限比较远的用 bigint 存储时间戳+datetime。如果需要区分不同记录的时区，就需要新增一个时区字段。

   - 时间存 datetime 类型时，时区存 char
   - 时间存时间戳格式时(bigint 或者 timestamp 类型)，时区存 int

5. 新增加的表，所有字段禁止 NULL

   - 说明：允许 NULL 值，会增加应用程序的复杂性，必须得增加特定的逻辑代码，以防止出现各种意外的 bug。旧表新加字段，需要允许为 NULL（避免全表数据更新 ，长期持锁导致阻塞）

6. 系统中所有逻辑型中数值 0 表示为“假”，数值 1 表示为“真”

7. 字符型的默认值为一个空字符值串’’，数值型的默认值为数值 0，逻辑型的默认值为数值 0

#### 【推荐】

1. 单表行数超过 500 万行或者单表容量超过 2GB，才推荐进行分库分表。
   - 说明： 如果预计三年后的数据量根本达不到这个级别，请不要在创建表时就分库分表。
2. 尽量避免使用 blob，text 等类型。
   - 说明：它们都比较浪费硬盘和内存空间。在加载表数据时，会读取大字段到内存里从而浪费内存空间，影响系统性能。建议非结构化数据考虑对象存储。
3. 采用分库策略的，库的数量不能超过 1024。采用分表策略的，表的数量不能超过 4096。

#### 【可选】

1. 合适的字符存储长度，不但节约数据库表空间、节约索引存储，更重要的是提升检索速度。

   - 正例： 如下表，其中无符号值可以避免误存负数，且扩大了表示范围。

   | 对象     | 年龄区间   | 类型                | 字节 | 表示范围                        |
   | :------- | :--------- | :------------------ | :--- | :------------------------------ |
   | 人       | 150 岁之内 | `unsigned tinyint`  | 1    | 无符号值： 0 到 255             |
   | 龟       | 数百岁     | `unsigned smallint` | 2    | 无符号值： 0 到 65535           |
   | 恐龙化石 | 数千万年   | `unsigned int`      | 4    | 无符号值： 0 到约 42.9 亿       |
   | 太阳     | 约 50 亿年 | `unsigned bigint`   | 8    | 无符号值： 0 到约 10 的 19 次方 |

2. 创建数据库时必须显式指定字符集，并且字符集只能是 utf8 或者 utf8mb4。

   - 说明：存储表情需要 utf8mb4 来进行存储，注意它与 utf-8 编码的区别

3. 对表里的 blob、text 等大字段，垂直拆分到其他表里，仅在需要读这些对象的时候才去 select。

4. 文本数据尽量用 varchar 存储。

   - 说明： 因为 varchar 是变长存储，比 char 更省空间。MySQL server 层规定一行所有文本最多存 65535 字节，因此在 utf8 字符集下最多存 21844 个字符，超过会自动转换为 mediumtext 字段。而 text 在 utf8 字符集下最多存 21844 个字符，mediumtext 最多存 224/3 个字符，longtext 最多存 232 个字符。一般建议用 varchar 类型，字符数不要超过 2700。

5. 字段长度选择够用就好，越小越好。用尽量少的存储空间来存储一个字段的数据

   - 如果不需要用到变长类型的话，那么就统一采用 char 型。例如邮编(postcode)
   - 使用 int 就不要使用 varchar、char
   - 用 varchar(16)就不要使 varchar(256)
   - 能使用 tinyint 就不要使用 smallint，int
   - IP 地址使用 int 类型

## 3.索引规约

#### 【必须】

1. 业务上具有唯一特性的字段，即使是多个字段的组合，也必须建成唯一索引。
   - 说明：不要以为唯一索引影响了 insert 速度，这个速度损耗可以忽略，但提高查找速度是明显的；另外，即使在应用层做了非常完善的校验控制，只要没有唯一索引，根据墨菲定律，必然有脏数据产生。
2. 超过三个表禁止 join。需要 join 的字段，数据类型必须绝对一致；多表关联查询时，保证被关联的字段需要有索引。
   - 说明：即使双表 join 也要注意表索引、SQL 性能。
3. 在 varchar 字段上建立索引时，必须指定索引长度，没必要对全字段建立索引，根据实际文本区分度决定索引长度即可。
   - 说明：索引的长度与区分度是一对矛盾体，一般对字符串类型数据，长度为 20 的索引，区分度会高达 90%以上，可以使用 count(distinct left(列名，索引长度))/count(*)的区分度来确定。
4. 页面搜索严禁左模糊或者全模糊，如果需要请走搜索引擎来解决。
   - 说明：索引文件具有 B-Tree 的最左前缀匹配特性，如果左边的值未确定，那么无法使用此索引。
5. 不应该对小型的表（仅使用几个页的表）创建索引。
   - 说明：这是因为完全表扫描操作可能比使用索引执行的查询快。

#### 【推荐】

1. 如果有

   ```
   order by
   ```

   的场景，请注意利用索引的有序性。

   ```
   order by
   ```

    

   最后的字段是组合索引的一部分，并且放在索引组合顺序的最后，避免出现 file_sort 的情况，影响查询性能。

   | 正例                                       | 反例                                                         |
   | :----------------------------------------- | :----------------------------------------------------------- |
   | where a=? and b=? order by c; 索引： a_b_c | 索引中有范围查找，那么索引有序性无法利用，如： WHERE a>10 ORDER BY b; 索引 a_b 无法排序。 |

2. 利用覆盖索引来进行查询操作， 避免回表。

   - 说明：如果一本书需要知道第 11 章是什么标题，会翻开第 11 章对应的那一页吗？目录浏览一下就好，这个目录就是起到覆盖索引的作用。

   | 正例                                                         |
   | :----------------------------------------------------------- |
   | 能够建立索引的种类分为主键索引、唯一索引、普通索引三种，而覆盖索引只是一种查询的一种效果，用 explain 的结果，extra 列会出现： using index。 |

3. 利用延迟关联或者子查询优化超多分页场景。

   - 说明：MySQL 并不是跳过 offset 行，而是取 offset+N 行，然后返回放弃前 offset 行，返回 N 行，那当 offset 特别大的时候，效率就非常的低下，要么控制返回的总页数，要么对超过特定阈值的页数进行 SQL 改写。

   - 正例：先快速定位需要获取的 id 段，然后再关联：

     ```
     SELECT a.* FROM 表 1 a, (select id from 表 1 where 条件 LIMIT 100000,20 ) b where a.id=b.id
     ```

4. SQL 性能优化的目标：至少要达到 range 级别，要求是 ref 级别，如果可以是 consts 最好。

   - 说明：
     1. consts 单表中最多只有一个匹配行（主键或者唯一索引），在优化阶段即可读取到数据。
     2. ref 指的是使用普通的索引（`normal index`） 。
     3. range 对索引进行范围检索。 | 反例 | | :---------------------------------------------------------------------------------------------------------------------- | | explain 表的结果， type=index，索引物理文件全扫描，速度非常慢，这个 index 级别比较 range 还低，与全表扫描是小巫见大巫。 |

5. 建组合索引的时候，区分度最高的在最左边。

   - 说明： 存在非等号和等号混合判断条件时，在建索引时，请把等号条件的列前置。如： where a>? and b=? 那么即使 a 的区分度更高，也必须把 b 放在索引的最前列。

   | 正例                                                         |
   | :----------------------------------------------------------- |
   | 如果 where a=? and b=? ， a 列的几乎接近于唯一值，那么只需要单建 idx_a 索引即可。 |

6. 防止因字段类型不同造成的隐式转换，导致索引失效。

   - 说明：数值类型有一种隐式转换，如果以数字开关的，后面的字符将被截断，只取前面的数字值，如果不以数字开关的将被置为 0。

7. 单表索引数不超过 6 个。

   - 说明：索引加快了查询速度，但是却会影响写入性能。一个表的索引应该结合这个表相关的所有 SQL 综合创建，尽量合并。

8. 不要给选择性低的字段建单列索引。

   - 说明：SQL SERVER 对索引字段的选择性有要求，如果选择性太低 SQL SERVER 会放弃使用。不适合创建索引的字段：性别、0/1、TRUE/FALSE

#### 【可选】

1. 创建索引时避免有如下极端误解：
   - 认为一个查询就需要建一个索引。
   - 认为索引会消耗空间、严重拖慢更新和新增速度。
   - 抵制惟一索引。认为业务的惟一性一律需要在应用层通过“先查后插”方式解决。
2. 限制 JOIN 个数
   - 单个 SQL 语句的表 JOIN 个数不能超过 5 个
   - 过多的 JOIN 个数会带来极低的查询效率
   - 过多 JOIN 在编译执行计划时消耗很大

## 4.SQL 语句

#### 【必须】

1. 所有关键字必须大写，如 SELECT 和 WHERE。

2. 在定义变量时用到的数据类型必须小写。

3. 别名必须要显性加上 AS 关键字。

   - 说明：因为这样的明确声明易于阅读。

4. 一般来说，别名应该由对象名中每一个单词的首字母组成。如果已经有相同的别名了，那么在别名后加一个数字。

   ```
   SELECT first_name AS fn
   FROM staff AS s1
   JOIN students AS s2
      ON s2.mentor_id = s1.staff_num;
   ```

5. 不要使用 count(列名)或 count(常量)来替代 count(*)， count(*)是 SQL92 定义的标准统计行数的语法，跟数据库无关，跟 NULL 和非 NULL 无关。

   - 说明： count(*)会统计值为 NULL 的行，而 count(列名)不会统计此列为 NULL 值的行。

6. 不要使用count(distinct col1, col2) 方式进行统计

   ```
   count(distinct col)
   ```

   计算该列除 NULL 之外的不重复行数。

   > count(distinct col1, col2) 如果其中一列全为 NULL，那么即使另一列有不同的值，也返回为 0。

7. 当某一列的值全是 NULL 时， count(col)的返回结果为 0，但 sum(col)的返回结果为 NULL，因此使用 sum()时需注意 NPE 问题。

   - 正例： 可以使用如下方式来避免 sum 的 NPE 问题：

     ```
     SELECT IF(ISNULL(SUM(g)),0,SUM(g))
     FROM table;
     ```

8. 使用 ISNULL()来判断是否为 NULL 值。

   - 说明： NULL 与任何值的直接比较都为 NULL。
     1. NULL<>NULL 的返回结果是 NULL， 而不是 false。
     2. NULL=NULL 的返回结果是 NULL， 而不是 true。
     3. NULL<>1 的返回结果是 NULL，而不是 true。

9. 在代码中写分页查询逻辑时，若 count 为 0 应直接返回，避免执行后面的分页语句。

10. 禁止使用存储过程，存储过程难以调试和扩展，更没有移植性。

11. 数据订正（特别是删除、 修改记录操作） 时，要先 select，避免出现误删除，确认无误才能执行更新语句。

12. 为了提高可移植性，仅使用标准 SQL 函数而不是特定供应商的函数。

    - 说明：如 MSSQL 的 getdate() 函数，MYSQL 不是这个

13. 必要时在 SQL 代码中加入注释。优先使用 C 语言式的以

     

    ```
    /*
    ```

     

    开始以

     

    ```
    */
    ```

     

    结束的块注释。

    - 说明：标准 SQL 中，注释以两个短划线（--）开头，以新行结束，但这是因为第一个 SQL 引擎是在 IBM 大型机上的，使用的是穿孔卡。在可以存储自由格式文本的现代计算机中，这不是一种好的选择。程序文本中单词换行会将注释分割开并导致错误。后来的标准增加了 C 风格，得到了很多数据库厂商的支持。

```
/* Updating the file record after writing to the file */
UPDATE file_system
   SET file_modified_date = '1980-02-22 13:19:01.00000',
      file_size = 209732
WHERE file_name = '.vimrc';
```

1. 注释应以英文为主。尽可能详细、全面创建每一数据对象前，应具体描述该对象的功能和用途，传入参数的含义应该有所说明，如果取值范围确定，也应该一并说明，取值有特定含义的变量（如 boolean 类型变量），应给出每个值的含义

- 说明：实际应用中，发现以中文注释的 SQL 语句版本在英文环境中不可用，为避免后续版本执行过程中发生某些异常错误，建议使用英文注释

1. 只在真的需要浮点数运算的时候才使用 REAL 和 FLOAT 类型，否则使用 NUMERIC 和 DECIMAL 类型。因为浮点数会有误差。
2. 避免创建临时表。
   - 说明：在某些数据库厂商语言中，程序员可以直接创建临时表，而在标准 SQL 中，临时表只能由具有管理权限的人创建。使用子查询表达式、派生表和视图代替临时表。
3. 避免使用游标。
   - 说明：游标很难移植，一般来说比非过程化的纯 SQL 语句的运行速度慢得多。这里所说的慢，指的是速度不在一个数量级上，因为游标是把结果集放在服务器内存，并通过循环一条一条处理记录，对数据库资源（特别是内存和锁资源）的消耗是非常大的。
4. 避免使用动态 SQL。
   - 说明：动态 SQL 既慢又危险，同时也是对自己的应用程序缺乏正确的设计。
5. 在 Update 语句，禁止将“,”写成“and”

```
# 正确示例：
update Table set uid=uid+1000,gid=gid+1000 where id <=2;

# 错误示例
update Table set uid=uid+1000 and gid=gid+1000 where id <=2;

# 此时“uid=uid+1000 and gid=gid+1000”将作为值赋给uid，并且也无Warning。
```

1. 在`WHERE`和`HAVING`同样够用的情况下，优先选择`WHERE`。

#### 【推荐】

1. 除了在 CTE（Common Table Expressions）中为 4 个空格，其他情况缩进应为 2 个空格。请勿使用制表符，只能使用空格，或者您的编辑器应设置为将制表符转换为空格。

   ```
   /* 2个空格 */
   SELECT
     is_deleted AS is_del,
     is_changed     AS is_cha
   FROM table
   
   /* 4个空格 */
   WITH temp AS(
       SELECT t1.ParentId,t1.Name FROM T_STAFF t1 WHERE t1.Code = '330106'
       UNION ALL
       SELECT t2.ParentId,t2.Name FROM T_STAFF t2,temp t1 WHERE t2.Id = t1.ParentId
   )
   ```

2. in 操作能避免则避免，若实在避免不了，需要仔细评估 in 后边的集合元素数量，控制在 1000 个之内。

3. 最好使用关键字的全称而不是简写，用 ABSOLUTE 而不用 ABS。

4. 尽量使用 BETWEEN 而不是多个 AND 语句，使用 IN() 而不是多个 OR 语句。

5. 在 CREATE TABLE 语句后先定义主键。

   - 说明：在表的声明中首先阅读键，这样能够获得关于表的本质的重要信息。

6. 禁止在数据库做复杂运算，如 XML 解析，字符串搜索。

   - 说明：复杂运算应在程序端完成。

7. 尽量避免大事务操作

   - 说明：只在数据需要更新时候开始事务，减少资源锁持有时间。

8. 禁止跨 db 的 join 语句

   - 说明：因为这样可以减少模块间耦合，为数据库拆分奠定坚实基础。

9. 对于超过 100W 行的大表进行 alter table，必须在业务低峰期执行。

   - 说明：因为 alter table 会产生表锁，期间阻塞对于该表的所有写入，对于业务可能会产生极大影响。

#### 【可选】

1. 不建议在开发代码中使用此语句 TRUNCATE TABLE
   - TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少，但 TRUNCATE 无事务且不触发 trigger，有可能造成事故，故不建议在开发代码中使用此语句。
   - 说明： TRUNCATE TABLE 在功能上与不带 WHERE 子句的 DELETE 语句相同。
2. 避免使用多余的括号
   - 说明：很多人会觉得产生 SQL 代码中会有多余级别的括号可以安全的执行，但是当有 5 个以上的为此和无用的嵌套括号时，代码就很难阅读了，而且很难定位到丢失的括号。
3. 避免使用 SELECT *
   - 说明：因为 select *会将不该读的数据也从 MySQL 里读出来，造成网卡压力。且表字段一旦更新，但 model 层没有来得及更新的话，系统会报错。
4. 使用 UNION ALL 替换 UNION
   - 说明：UNION 会对 SQL 结果集去重排序，增加 CPU、内存等消耗。union all 不需要去重，节省数据库资源，提高性能。
5. 尽量少使用触发器，扩展性差
   - 说明：触发器对应用不够透明。
6. 不推荐使用 enum，set
   - 说明：因为它们浪费空间，且枚举值写死了，变更不方便。推荐使用 tinyint 或 smallint。
7. 减少使用 order by
   - 说明：排序可以放到程序端去做。order by、group by、distinct 这些语句较为耗费 CPU，数据库的 CPU 资源是极其宝贵的
8. order by、group by、distinct 这些 SQL 尽量利用索引直接检索出排序好的数据
9. 包含了 order by、group by、distinct 这些查询的语句，where 条件过滤出来的结果集请保持在 1000 行以内，否则 SQL 会很慢。
10. 批量操作数据时，需要控制事务处理间隔时间，进行必要的 sleep，一般建议值 5-10 秒。
11. 事务里包含 SQL 不超过 5 个（支付业务除外）
    - 说明：过长的事务会导致锁数据较久，MySQL 内部缓存、连接消耗过多等雪崩问题
12. 事务里更新语句尽量基于主键或 unique key。
    - 说明：如果不这样会产生间隙锁，内部扩大锁定范围，导致系统性能下降，产生死锁
13. 对于 MySQL 主从延迟严格敏感的 select 语句，请开启事务强制访问主库。
14. 不得使用外键与级联，一切外键概念必须在应用层解决。

- 说明：以学生和成绩的关系为例，学生表中的 student_id 是主键，那么成绩表中的 student_id 则为外键。如果更新学生表中的 student_id，同时触发成绩表中的 student_id 更新， 即为级联更新。外键与级联更新适用于单机低并发，不适合分布式、高并发集群；级联更新是强阻塞，存在数据库更新风暴的风险:外键影响数据库的插入速度。

# SQL代码规范 (OLAP)

## 1.前言

### 1.1 为什么要有OLAP SQL规范

OLTP与OLAP是两种不同的数据应用场景，现有公司规范偏向OLTP，主要应用于关系型数据库场景。而在如今的大数据时代，OLAP被大量应用在数据仓库、经营分析、BI、数据工程场景。在ad-hoc分析被广泛应用的今天，不正确、不规范的 SQL 会带来以下问题

- 查询出错误数据，影响分析结果
- 万亿级别的大表被全局扫描，同时打爆集群IO和网络带宽，影响别的查询
- 浪费大量计算资源的同时，产出效率低下
- 代码风格各异，维护成本高
- Code Review 及异常排查困难

故为形成统一的OLAP SQL 编码风格，以保障公司项目代码的易维护性和编码安全性，特制定本规范。

### 1.2 采用等级约定

- **必须（Mandatory）**：用户必须采用；
- **推荐（Preferable）** ：用户理应采用，但如有特殊情况，可以不采用；
- **可选（Optional）** ：用户可参考，自行决定是否采用

### 1.3 应用范围

- 数据仓库建设（Venus、US调度、OLA Data OPS）
- IDEX、灯塔自助分析、OLA自助分析
- 其他支持sql的查询入口

### 1.4 对规范存在异议时候，如何提交建议及变更流程

1. 提交新增或争议条款的变更说明（包括条款内容，变更理由，样例的Bad & Good 代码）
2. 代码委员会 & 通道 讨论&评审
3. 投票通过后合并入规范
4. 更新代码规范检测工具条款

## 2.命名规范

### 2.1 通用规则

- 必须（Mandatory）

  1. 不允许使用保留关键字,使用一致的、描述意义的命名
  2. 只在命名中使用字母、数字和下划线
  3. 命名以字母开头，不能以数字、下划线开头，不能以下划线结尾
  4. 不允许在名字中出现连续下划线
  5. 不允许使用驼峰命名,建议使用 *下划线* 分割（无法迁移大小写不敏感的引擎）

- 推荐（Preferable）

  1. 尽量不要用拼音，用贴切的英文单词
  2. 尽量避免使用缩写词，使用时一定确定这个缩写简明易懂

- 可选（Optional）

  1. 使用统一的后缀，例如：
     - cnt --> 表示数值统计。如: user_cnt
     - uv --> 表示用户去重计数。如：play_uv
     - amt --> 表示计量金额。如：charge_amt
     - ts --> 表示时间长度。如：play_ts
     - area --> 表示面积。如：exp_area
     - hight --> 表示高度。如：exp_hight
     - rate --> 表示比例。如：exp_click_rate
     - id --> 独一无二的标识符。如：user_id
     - name --> 表示名字，如: task_name
     - status --> 标志值或任何表示状态的值，如 query_status。
     - date --> 表示该列包含日期。 如：stat_date
     - hour --> 表示该列包含小时。 如：stat_hour

- 样例

  ```
    -- Good
  
       userid ,media_id, user_cnt, stat_date, query_option, imp_date
  
    -- Bad
  
       _uid,mid_,mediaId,users,date,option,order
  ```

### 2.2 库命名

- 必须（Mandatory）

  1. 全部小写命名，禁止出现大写
  2. 数据库名称不要超过 30 个字符

- 推荐（Preferable）

  1. 按照业务模块、产品线和访问权限隔离需求来创建数据库（数据仓库规范有约定，参考约定）

- 样例

  ```
    -- Good
  
       pcg_video_ads,pcg_news_dw,pcg_sport_ods
  
    -- Bad
  
       video,sportDW,newsApp
  ```

### 2.3 表命名

- 必须（Mandatory）

  1. 全部小写命名，禁止出现大写
  2. 表名不超过50个字符，符合分层规范，且简洁、清晰、易懂

- 推荐（Preferable）

  1. 表名符合统一的数仓规范，包括分层、主题、存储策略、存储周期等信息
  2. 用单数形式表示名称，例如，使用 employee，而不是 employees
  3. 表名不能和列名同名

- 样例

  ```
    -- Good
  
       dim_newsapp_channel_basic_info_d_f  -- 维度表
  
       ods_app_startup_noadd_di  -- ods层数据
  
       dws_all_video_account_scorebalance_ds -- dws层数据
  
    -- Bad
  
      t_newsapp_bi_install_channel_basic_info_accum  -- 维度表不清晰说明
  
      t_newsapp_bi_install_channel_newuser -- 无法识别是什么层的数据（貌似是个统计结果）
  ```

### 2.4 字段命名

- 必须（Mandatory）

  1. 全部小写命名，禁止出现大写
  2. 布尔值列命名为 [is_描述]。如 member 表上表示为 enabled 的会员的列命名为 is_enabled；
  3. 字段名不超过 32 个字符；

- 推荐（Preferable）

  1. 建议各表之间相同意义的字段应同名，并且一定使用相同的字段类型；
  2. 尽量给每个字段添加注释，枚举型需指明主要值的含义，如”0 - 离线，1 - 在线”；
  3. 尽量不要出现uid、pid、cid这样的缩写，字段名尽量做到可以正确理解，避免后期关联困难

- 样例

  ```
    -- Good
  
      imp_date bigint comment '时间分区',
      platform string comment '平台',
      is_vip bigint comment '0 - 非会员 1 - 会员',
      user_cnt  bigint comment '用户数'
  
    -- Bad
  
      date bigint ,
      dim1 string ,
      dim2 bigint,
      valOne bigint,
      val2 bigint
  ```

## 3.编码格式规范

- 必须（Mandatory）

  1. 关键字必须大写 或者 全部小写，禁止混用
  2. 每行不超过80个字符，禁止不经过任何format，所有sql代码全部堆在一两行
  3. 使用空格作为缩进符，禁止使用TAB（请在编辑器设置TAB转空格），4个空格为1个缩进量，所有的缩进均为1个缩进量的整数倍，同一层嵌套sql代码保证缩进一致。
  4. 同一语句占用多于一行时，每行的关键字应左对齐。
  5. SELECT子句排列要求SELECT语句中所用到的FROM、WHERE、GROUP BY、HAVING、ORDER BY、JOIN和UNION等子句，需要遵循如下要求换行编写
     - 与相应的SELECT语句左对齐编排
     - 子句首个单词后添加1个缩进量，再编写后续的代码
  6. 注意下列情况必须加入空格
     - 在等号（=）前后
     - 在逗号（,）后
     - 成对的单引号（'）前后，除非在括号中或后面是逗号 / 分号
  7. CASE WHEN语句的编写：WHEN子语在CASE语句后换行编写，并缩进1个缩进量后开始编写
     - 每个WHEN子句尽量在1行内编写，如果语句较长可以换行
     - CASE语句必须包含ELSE子语，ELSE子句与WHEN子句对齐，END与CASE对齐
  8. 子查询及With子句，左右括号必须列对齐

- 推荐（Preferable）

  1. 建议每个语句尽量不要超过100行（过长不易维护，且容易出错。若必须可以考虑with写法）
  2. 建议每个嵌套子查询在SELECT前添加注释说明该部分代码主要作用、唯一主键或组合主键（若存在的话）
  3. 任何使用常量或者硬编码的地方均要注释
  4. 若在'${date}' '${hour}'上进行加减操作，需注释说明特殊逻辑
  5. 下列情况建议换行
     - 在 AND 或 OR 前
     - 在分号后（分隔语句以提高可读性）
     - 在每个关键字定义之后
     - 将多个列组成一个逻辑组时的逗号后
     - 将代码分隔成相关联的多个部分，帮助提高大段代码的可读性

- 可选（Optional）

  1. 把逗号写在字段前面（避免丢逗号或这from前面多个逗号，方便批量编辑）

- 样例

  ```
  -- Good
  
        SELECT 
            imp_date
            , platform
            , is_vip
            , COUNT(DISTINCT qimei) AS user_cnt
        FROM 
            pcg_xxxx_dw.dws_app_startup_di 
        WHERE 
            imp_date >= 20210506
            AND imp_date <= 20210511
        GROUP BY 
            imp_date
            , platform
            , is_vip
  
          -- Bad
  
        seleCT imp_date,platform,is_vip,COUNT(distinct qimei) AS user_cnt from pcg_xxxx_dw.dws_app_startup_di where
        imp_date >= 20210506 and imp_date <= 20210511 group BY imp_date ,platform,is_vip
  
          -- Good
  
        select 
            imp_date
            , platform
            , is_vip
            , count(distinct qimei) as user_cnt
        from 
              pcg_xxxx_dw.dws_app_startup_di 
        where 
            imp_date >= 20210506
            and imp_date <= 20210511
        group by 
            imp_date
            , platform
            , is_vip    
  
            -- Bad
  
  
        Select imp_date,platform
        ,is_vip,count(distinct qimei) as usercnt 
        From pcg_xxxx_dw.dws_app_startup_di 
        Where imp_date >= 20210506 
        and imp_date <= 20210511 
        Group By imp_date ,platform,is_vip
  
            -- Good CASE WHEN 
        select 
            imp_date
            , case 
                  when channel = '1' then 'App Store'
                  when channel = '2' then 'Google Play'
                  when channel = '3' then 'xxxx'
                  else channel 
            end as channel_name
            , count(distinct user_id) as user_cnt
        from 
            pcg_xxx_dw.dws_xxxxxx_startup_di
        where 
            imp_date >= 20210506
            and imp_date <= 20210511
        group by
            imp_date
            , channel_name    
  ```

## 4.查询语句规范

- 必须（Mandatory）

  1. 查询必须加上分区字段（包括不限于时间分区），避免大量扫描
  2. 查看明细表必须加上LIMIT
  3. 不要直接select *在子查询中,去掉不必要的列(select* 影响对代码逻辑的理解，且影响引擎性能和查询效率)

- 推荐（Preferable）

  1. 查询尽量加上LIMIT（如果可预知结果大小也建议直接加上限制）

     ```
        --Good
        select 
            imp_date
            , start_type
            , user_id
            , platform
            , brand
            , model
        from 
            pcg_xxx_dw.dws_xxxxxx_startup_di
        where 
          imp_date = 20210506 --分区字段
        limit 20
     
        --Bad
        select 
            *
        from 
            pcg_xxx_dw.dws_xxxxxx_startup_di
        where 
            exp_date = 20210506 --非分区字段
     ```

  2. 对不熟悉的表发起查询前，请用EXPLAIN 查看执行计划，评估资源消耗

     ```
     --Good
     explain
     select 
         imp_date
         , start_type
         , platform
         , brand
         , model
         , count(user_id) as dau
     from 
         pcg_xxx_dw.dws_xxxxxx_startup_di
     where 
         imp_date = 20210506 --分区字段
     group by
         imp_date
         , start_type
         , platform
         , brand
         , model 
     ```

  3. 尽可能查仓库上层聚合表或者提前加上过滤条件，减少IO（减少数据量是最好的优化）

  4. WHERE条件中列尽量不要使用函数转换，强制类型转换等

  5. 尽量避免使用like

     ```
       --Good
       select 
           imp_date
           , start_type
           , count(user_id) as dau
       from 
           pcg_xxx_dw.dws_xxxxxx_startup_di
       where 
           imp_date = 20210506 
           and platform in ('android', 'android-lite')
       group by
           imp_date
           , start_type
     
       --Bad
       select 
           imp_date
           , start_type
           , count(user_id) as dau
       from 
           pcg_xxx_dw.dws_xxxxxx_startup_di
       where 
           imp_date = 20210506 
           and platform like 'android%'
       group by
           imp_date
           , start_type
     ```

     1. 尽量使用IN，不要用多个OR语句

     2. 去掉不必要的is not null 条件，换成 >= 或者 <=

     3. 如果需要多重判断的时候，尽量使用CASE表达式

     4. 尽量避免使用UNION（明显不会有重复值时候用union all替代）

     5. 不要大量使用多层JOIN（数据仓库表可以考虑冗余部分重要信息，方便后续单表查询）

     6. 尽量不要用笛卡尔积

     7. 大表关联小表时候请用 map join （尽量使用sql hint指定）

        ```
           select /*mapjoin(t2)*/
               imp_date
               , channel_type_1
               , channel_type_2
               sum(new_user) as new_user
           from 
           (
               select 
                   imp_date
                   , channel_id
                   , count(user_id) as new_user
               from 
                   pcg_xxx_dw.dws_xxxxxx_startup_app_di
               where 
                   imp_date = 20210506
                   and is_new_user = 1 
               group by
                   imp_date
                   , channel_id
           ) t1
           left join
           (
               select 
                   channel_id
                   , channel_type_1
                   , chanenl_type_2
               from 
                   pcg_xxx_dw.dim_xxxxxx_channel_info
               where 
                   imp_date = 20210506
           ) t2
           on
             t1.channel_id = t2.channel_id
           group by 
               imp_date
               , channel_type_1
               , channel_type_2 
        ```

  6. Where条件中仅存在 OR 条件可以转换成 UNION ALL 改写（部分引擎会通过优化器自行优化，不用改写）

## 5.DDL&DML规范

- 必须（Mandatory）
  1. 大表需要按规范设置合理的生命周期
  2. 数据类型不允许完全使用STRING类型，以免数据加工环节的数据质量问题无法及时暴露
     - 带小数点数据使用DOUBLE类型(金额也可以使用厘，存BIGINT)，需要注释或字段说明单位
     - 字符类数据使用STRING类型
     - MPP引擎需要考虑准确的数据类型，减少内存消耗
- 推荐（Preferable）
  1. 非临时表包含表的数仓分层、主题域、存储策略、存储周期等信息
  2. 建表语句尽量给每个语句写Comment
  3. 尽量使用parquet、orcfile，同时加上压缩关键字
  4. 尽量避免使用drop语句，需要double check（点错了，数据可能就没了，特别是大表）
  5. impala 引擎insert 后尽量添加统计信息

## 6.Code Review 规则

- 必须进行Code Review的场景
  1. ETL JOB上线需要进行Code Review
  2. DDL&DML 语句必须进行Code Review
  3. 收到系统推送的慢查询和优化提示，请及时关注并按提示优化语句
- Code Review 关注内容
  - 代码逻辑准确性
  - 代码风格
  - 代码质量，效率是否存在问题
  - 注释是否充分
  - 是否符合数据仓库规范

## 7.不同数据引擎下的使用建议

### 7.1 spark/hive

1. 注意数据倾斜
2. 注意数据膨胀
3. 避免不必要的函数调用
4. 使用Cube时候需要配合grouping函数确认正确性
5. 关注执行计划
6. [tdw优化建议](http://km.oa.com/group/2430/articles/show/127863?kmref=search&from_page=1&no=2)

### 7.2 impala

1. 避免扫描超大规模数据
2. 关注Join优化（遵循右表为小表） [KM的翻译版本](http://km.oa.com/group/35526/articles/show/446099)
3. 对表和字段做统计信息
4. 用parquet文件格式
5. 关注执行计划和运行时统计信息，并且关注优化提示标签
6. IO密集型表利用Alluxio优化
7. [官方优化说明](https://docs.cloudera.com/documentation/enterprise/6/6.3/topics/impala_performance.html)

### 7.3 clickhouse

1. 注意引擎的特性，选用合适的引擎(AggregatingMergeTree\SummingMergeTree等)，减少扫描数据量是最好的优化
2. 尽量不用使用select *，仅加载需要的列
3. 避免Join操作，低于100w的维度可以使用Dict引擎（建议使用外部字典表配置同步）
4. 数据写Local表，尽量不要直接写Distributed表
   - 如果有ETL诉求（调整字段类型，过滤转化结构等），建议走 null 表 -> Materialized View pipe -> local表 形式写入
   - 选用合理的表Order by ，充分考虑粒度和使用频率
   - 控制写入数据量和并发，禁止小数据batch提交。
   - 建表语句尽量遍历节点执行（zk稳定性需要关注）
5. 关注ZK的 配置及状态
6. 关注磁盘的IO负载
7. 关注[官方函数文档](https://clickhouse.tech/docs/en/sql-reference/functions/)，利用特性函数和特性类型，优化调整查询