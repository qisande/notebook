# 基础操作与库命令

## MySQL 基础操作命令

- `安装目录/mysql start`：`Linux` 系统启动 `MySQL` 服务。

- `mysql -h地址 -p端口 -u账号 -p`：客户端连接`MySQL`服务（需要二次输入密码）。
- `show status;`：查看 `MySQL` 运行状态。
- `quit`：退出当前数据库连接。

## MySQL 库相关的命令

- `show databases;`：查看目前 `MySQL` 中拥有的所有库。
- `show tables;`：查看一个库中的所有表。
- `show create database 库名;`：查看创建某个库的 `SQL` 详细信息。
- `show create table 表名;`：查看创建某张表的 `SQL` 详细信息。
- `use 库名;`：使用/进入指定的某个数据库。
- `create database 库名;`：新建一个数据库，后面还可以指定编码格式和排序规则。
- `drop database 库名;`：删除一个数据库。
- `ALTER DATABASE 库名 DEFAULT CHARACTER SET 编码格式 DEFAULT COLLATE 排序规则`：修改数据库的编码格式、排序规则。

## MySQL 表相关的命令

创建表的 `SQL` 命令

```sql
CREATE TABLE `库名`.`表名`  (
    字段名称1 数据类型(精度限制) [字段选项],
    字段名称2 数据类型(精度限制) [字段选项]
) [表选项];
```

**字段选项**（可以不写，不选使用默认值）：

- `NULL`：表示该字段可以为空。
- `NOT NULL`：表示改字段不允许为空。
- `DEFAULT 默认值`：插入数据时若未对该字段赋值，则使用这个默认值。
- `AUTO_INCREMENT`：是否将该字段声明为一个自增列。
- `PRIMARY KEY`：将当前字段声明为表的主键。
- `UNIQUE KEY`：为当前字段设置唯一约束，表示不允许重复。
- `CHARACTER SET 编码格式`：指定该字段的编码格式，如`utf8`。
- `COLLATE 排序规则`：指定该字段的排序规则（非数值类型生效）。
- `COMMENT 字段描述`：为当前字段添加备注信息，类似于代码中的注释。

**表选项**（可以不写，不选使用默认值）：

- `ENGINE = 存储引擎名称`：指定表的存储引擎，如`InnoDB、MyISAM`等。
- `CHARACTER SET = 编码格式`：指定表的编码格式，未指定使用库的编码格式。
- `COLLATE = 排序规则`：指定表的排序规则，未指定则使用库的排序规则。
- `ROW_FORMAT = 格式`：指定存储行数据的格式，如`Compact、Redundant、Dynamic....`。
- `AUTO_INCREMENT = n`：设置自增列的步长，默认为`1`。
- `DATA DIRECTORY = 目录`：指定表文件的存储路径。
- `INDEX DIRECTORY = 目录`：指定索引文件的存储路径。
- `PARTITION BY ...`：表分区选项，后续讲《MySQL表分区》再细聊。
- `COMMENT 表描述`：表的注释信息，可以在这里添加一张表的备注。

**创建表的具体例子：**

```sql
-- 在 db_zhuzi 库下创建一张名为 zz_user 的用户表
CREATE TABLE `db_zhuzi`.`zz_user`  (
    -- 用户ID字段：int类型、不允许为空、设为自增列、声明为主键
    `user_id` int(8) NOT NULL AUTO_INCREMENT PRIMARY "i_p_id" COMMENT '用户ID',
    -- 用户名称字段：字符串类型、运行为空、默认值为“新用户”
    `user_name` varchar(255)  NULL DEFAULT "新用户" COMMENT '用户名'
)
-- 存储引擎为InnoDB、编码格式为utf-8、字符排序规则为utf8_general_ci、行格式为Compact
ENGINE = InnoDB 
CHARACTER SET = utf8 
COLLATE = utf8_general_ci 
ROW_FORMAT = Compact;
```

## 表的分析、检查、修复与优化操作

### 分析表

语法如下：

```sql
analyze [local | no_write_to_binlog] table 表名1;
```

执行结果如下：

![image-20241130145146026](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20241130145146026.png)

如果 `Msg_text` 显示的是 `OK`，则代表这张表的键不存在问题。

### 检查表

语法如下：

```sql
check table 表名1,表名2... [检查选项];
```

执行结果如下：

![image-20241130145233821](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20241130145233821.png)

这回的结果出现了些许不同，`Msg_text` 中出现了一个 `Error` 信息，提示咱们检查的 `zz_u` 表不存在，而对于一张存在的 `zz_users` 表，则返回 `OK` ，表示没有任何问题。

### 修复表

语法如下：

```sql
repair [local | no_write_to_binlog] table 表名 [quick] [extended] [use_frm];
```

执行结果如下：

![image-20241130145353865](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20241130145353865.png)

上述 `Msg_text` 信息翻译过来的意思是：选择的表其引擎并不支持修复命令。

上述这个修复机制默认是不开启的，因为 `InnoDB` 不需要这个恢复机制，`InnoDB` 有完善的事务和持久化机制，客户端提交的事务都会持久化到磁盘，除非你人为损坏 `InnoDB` 的数据文件，否则基本上不会出现 `InnoDB` 数据损坏的情况。

### 优化表

语法如下：

```sql
optimize [local | no_write_to_binlog] table 表名;
```

执行结果如下：

![image-20241130145547209](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20241130145547209.png)

这里值得一提的是：此优化非彼优化，并不意味着你的表存在性能问题，执行后它会自动调优，而是指清除老数据。

其实删除一条数据本质上并不会立马从磁盘移除，而是会先改掉隐藏的删除标识位，执行这条优化命令后，`MySQL` 会将一些已经 `delete` 过的数据彻底从磁盘删除，从而释放这些“废弃数据”占用的空间。

# 增删改查语句

## 基本的增删改查语句
### 插入数据

如果要插入一条完整的数据，字段名可以用`*`代替所有字段。

```sql
-- 向指定的表中插入一条数据。
insert into 表名(字段名...) values(字段值...);

-- 向表中插入多条数据。
insert into 表名(字段名...) values(字段值...),(...)...;

-- 插入一条数据，但只插入某个字段的值。
insert into 表名 set 字段名=字段值,...;

-- 批量插入
-- 使用insert语句批量插入另一张表中查询的数据
insert into 表名(字段名...) select 字段名... from 表名...;

-- 使用replace语句来实现批量插入
replace into 表名(字段名1,字段名2...) values(字段值....),(字段值...),...;
```

上述批量插入数据的方式中，还可以通过 `replace` 关键字来实现插入，它与 `insert `有啥区别呢？答案在于它可以实现批量更新，使用 `replace` 关键字来插入数据的表必须要有主键，`MySQL` 会根据主键值来决定新增或修改数据，当批量插入的数据中，主键字段值在表中不存在时，则会向表中插入一条相应的数据，而当插入数据中的主键值存在时，则会使用新数据覆盖原有的老数据。

### 删除数据

```sql
-- 删除一张表的所有数据。
delete from 表名;

-- 根据条件删除一条或多条数据。
delete from 表名 where 条件;

-- 清空一张表的所有数据，但保留表结构。
truncate table 表名;
```

### 修改数据

```sql
-- 修改表中所有记录的数据。
update 表名 set 字段名=字段值,...;

-- 根据条件修改一条或多条记录的数据。
update 表名 set 字段名=字段值,... where 条件;

-- 批量修改对应主键记录的数据。
replace 表名(字段名1,...) values(字段值...),...;
```

### 查询数据

```sql
-- ---------------基本查询---------------
-- 查询一张表的所有数据。
select * from 表名;

-- 根据条件查询表中相应的数据。
select * from 表名 where 条件;

-- 根据条件查询表中相应数据的指定字段。
select 字段1,字段2... from 表名 where 条件;

-- 对查询后的结果集，进行某个函数的特殊处理。
select 函数(字段) from 表名;

-- ---------------高级查询---------------
-- 为查询出来的字段取别名
select 字段1 as 别名,... from 表名 where 条件;
select 字段1 别名,... from 表名;

-- 为查询出的表取别名
select * from 表名 as 别名;

-- 以多条件查询数据
-- 所有条件都符合时才匹配
select * from 表名 where 字段1=值1 and 字段2=值2 and ...;
-- 符合任意条件的数据都会返回
select * from 表名 where 字段1=值1 or 字段2=值2 or ...;
-- =符号，可以根据情况换为>、<、>=、<=、!=、between and、is null、not is null这些

-- 对查询后的结果集使用函数处理
select 函数(字段) from 表名 where 条件;

-- 对查询条件使用函数处理
select * from 表名 where 函数(条件);

-- 模糊查询
-- 查询字段值以指定字符结尾的所有记录
select * from 表名 where 字段 like "%字符";
-- 查询字段值以指定字符开头的所有记录
select * from 表名 where 字段 like "字符%";
-- 查询字段值包含指定字符的所有记录
select * from 表名 where 字段 like "%字符%";

-- 按照多值查询对应行记录
select * from 表名 where 字段 in (值1，值2,...);
-- 按照多值查询相反的行记录
select * from 表名 where 字段 not in (值1，值2,...);
-- 基于多个字段做多值查询
select * from 表名 where (字段1,字段2...) in ((值1，值2,...),(...),...);

-- 只需要查询结果中的前N条数据
select * from 表名 limit N;
-- 返回查询结果中 N~M 区间的数据
select * from 表名 limit N,M;

-- 联合多条SQL语句查询（union all表示不去重，union表示对查询结果去重）
select * from 表名 where 条件
union all 
select * from 表名 where 条件;
```

#### 分组过滤、排序

写`SQL`语句时，有些需求往往无法通过最基本的查询语句来实现，因此就需要用到一些高级的查询语法，例如分组、过滤、排序等操作。

```sql
-- 基于一个字段进行排序查询
-- 按字段值正序返回结果集
select * from 表名 order by 字段名 asc;
-- 按字段值倒序返回结果集
select * from 表名 order by 字段名 desc;
-- 按照多字段进行排序查询
select * from 表名 order by 字段1 asc,字段2 desc; 

-- 基于字段进行分组
select * from 表名 group by 字段1,字段2....;

-- 基于分组查询后的结果做条件过滤
select * from 表名 group by 字段1 having 条件;
```

实际上 `group by、having` 这些语句，更多的要配合一些聚合函数使用，如 `min()、max()、count()、sum()、avg()....`，这样才能更符合业务需求。

`where、having` 的区别：

> 这两个关键字都是用来做条件过滤的，但 `where` 优先级会比 `group by` 高，因此当分组后需要再做条件过滤时，就无法使用 `where` 来做筛选，而 `having` 就是用来对分组后的结果做条件过滤的。查询语句中的各类关键字执行优先级为：`from → where → select → group by → having → order by`。

#### 子查询

子查询也可以理解成是查询嵌套，是指一种由多条`SQL`语句组成的查询语句，语法如下：

```sql
-- 基于一条SQL语句的查询结果进一步做查询
select * from (select * from 表名 where 条件) as 别名 where 条件;

-- 将一条SQL语句的查询结果作为条件继续查询（只适用于子查询返回单值的情况）
select * from 表名 where 字段名 = (select 字段名 from 表名 where 条件);

-- 将一条SQL语句的查询结果作为条件继续查询（适用于子查询返回多值的情况）
select * from 表名 where 字段名 exists (select 字段名 from 表名 where 条件);
-- 上述的exists可以换为not exists，表示查询不包含相应条件的数据

-- 将一条SQL语句的多个查询结果，作为条件与多个字段进行范围查询
select * from 表名 where (字段1,字段2...) in (select 字段1,字段2... from 表名);
```

在上述子查询语法中，`exists` 的作用和`in`大致相同，只不过 `not in` 时会触发全表扫描，而 `not exists` 依旧可以走索引查询，因此通常情况下尽量使用 `not exists` 代替 `not in` 来查询数据。

#### 关联查询

关联查询也被称之为连表查询，也就是指利用主外键连接多张表去查询数据，这几乎也是日常开发中写的最多的一类查询语句，`MySQL`中支持多种关联类型，如：

- 交叉连接
- 内连接
- 外连接：
  - 左连接
  - 右连接
  - 全连接

语法如下：

```sql
-- 交叉连接：默认把前一张表的每一行数据与后一张表的所有数据做关联查询
-- 这种方式默认采用交叉连接的方式
select * from 表1,表2...;
-- 显式声明采用交叉连接的方式
select * from 表1 cross join 表2;

-- 内连接：只返回两张表条件都匹配的数据
-- 隐式的内连接写法
select * from 表1,表2... where 表1.字段 = 表2.字段 ...; 
-- 等值内连接
select * from 表1 别名1 inner join 表2 别名2 on 别名1.字段 = 别名2.字段;
-- 不等式内连接
select * from 表1 别名1 inner join 表2 别名2 on 别名1.字段 < 别名2.字段;

-- 左外连接：左表为主，右表为次，无论左表在右表是否匹配，都返回左表数据，缺失的右表数据显示 NULL
select * from 表1 left join 表2 on 表1.字段 = 表2.字段;

-- 右外连接：和左连接相反，右表为主，左表为次，永远返回右表的所有数据
select * from 表1 right join 表2 on 表1.字段 = 表2.字段;

-- 全外连接：两张表没有主次之分，每次查询都会返回两张表的所有数据，不匹配的显示 NULL
-- MySQL 中不支持全连接语法，只能通过 union all 语句，将左、右连接查询拼接起来实现
select * from 表1 left join 表2 on 表1.字段 = 表2.字段
union all
select * from 表1 right join 表2 on 表1.字段 = 表2.字段;

-- 继续拼接查询两张以上的表
select * from 表1 left join 表2 on 表1.字段 = 表2.字段 left join 表3 on 表2.字段 = 表3.字段;
-- 通过隐式连接的方式，查询两张以上的表
select * from 表1,表2,表3... where 表1.字段 = 表2.字段 and 表1.字段 = 表3.字段...;
-- 通过子查询的方式，查询两张以上的表
select * from 
(表1 as 别名1 left join 表2 as 别名2 on 别名1.字段 = 别名2.字段) 
left join 
表3 as 别名3 on 别名1.字段 = 别名3.字段;
```

笛卡尔积问题就是指两张表的所有数据都做关联查询，一般连表查询都需要指定连接的条件，但如果不指定时，`MySQL` 默认会将左表每一条数据挨个和右表所有数据关联一次，然后查询一次数据。比如左表有 `3` 条数据，右表有 `4` 条数据，笛卡尔积情况出现时，一共就会查询出 `3 x 4 = 12` 条数据。

> 笛卡尔积现象出现时，会随着表数据增长越来越大，因此在连表查询时一定要消除笛卡尔积问题，咋消除呢？其实就是指定加上关联条件即可。

# MySQL数据库函数

## 数学函数

- `abs(X)`：返回`X`的绝对值，如传进`-1`，则返回`1`。
- `mod(X,Y)`：返回`X`除以`Y`的余数。
- `ceil(X) | ceiling(X)`：返回不小于`X`的最小整数，如传入`1.23`，则返回`2`。
- `round(X)`：返回`X`四舍五入的整数。
- `floor(X)`：返回`X`向下取整后的值，如传入`2.34`，会返回`2`。
- `rand(N)`：返回一个`0~N``0~1`之间的随机小数（不传参默认返回`0~1`之间的随机小数）。
- `format(x,y)`：将`x`格式化位以逗号隔开的数字列表，`y`是结果的小数位数。

## 字符串函数

- `length(S)`：返回字符串的字节数，传入“竹子爱熊猫”，返回`15`，一个汉字占位`3`字节。
- `char_length(str)`：返回字符串的字符数。
- `concat_wa(sep,S1,S2...)`：合并传入的多个字符串，每个字符串之间用`sep`间隔。
- `replace(S,old,new)`：使用`new`新字符替换掉`S`字符串中的`old`字符。
- `substring(S,index,N)`：截取`S`字符串，从`index`位置开始，返回长度为`N`的字符串。

## 日期和时间函数

- `now() | sysdate()`：返回当前系统的日期时间，如`2022-10-21 17:30:59`。
- `unix_timestamp()`：获取一个数值类型的`unix`时间戳，如`1666348711`。
- `datediff(date1,date2)`：计算两个日期之间的间隔天数。
- `date_format(date,format)`：将一个日期格式化成指定格式。
- `time_format(time,format)`：将一个时间格式化成指定格式。

## 聚合函数

聚合函数一般是会结合 `select、group by having` 筛选数据使用。

- `max(字段名)`：查询指定字段值中的最大值。

- `min(字段名)`：查询指定字段值中的最小值。

- `count(字段名)`：统计查询结果中的行数。

- `sum(字段名)`：求和指定字段的所有值。

- `avg(字段名)`：对指定字段的所有值，求出平均值。

- `group_concat(字段名)`：返回指定字段所有值组合成的结果，如下：

- `distinct(字段名)`：对于查询结果中的指定的字段去重。

- `convert(expr USING transcoding_name)`：expr: 要转换的值，transcoding_name: 要转换成的字符集。

  ```sql
  -- utf8mb4
  SELECT CHARSET('ABC');
  -- gbk
  SELECT CHARSET(CONVERT('ABC' USING gbk));
  ```

- `convert(expr,type)`：expr: 要转换的值，type: 要转换为的数据类型。

  ```sql
  -- 将值转换为DATE数据类型
  -- 2022-05-25
  SELECT CONVERT('2022-05-25', DATE);
  -- 2022-05-25 17:58:48
  SELECT NOW();
  -- 2022-05-25
  SELECT CONVERT(NOW(), DATE);
  
  -- 将值转换为DATETIME数据类型
  -- 2022-05-25 00:00:00
  SELECT CONVERT('2022-05-25', DATETIME);
  
  -- 将值转换为TIME数据类型
  -- 14:06:10
  SELECT CONVERT('14:06:10', TIME);
  -- 2022-05-25 17:25:12
  SELECT NOW();
  -- 17:25:12
  SELECT CONVERT(NOW(), TIME);
  
  -- 将值转换为CHAR数据类型
  -- '150'
  SELECT CONVERT(150, CHAR);
  -- 'Hello World437'
  SELECT CONCAT('Hello World',CONVERT(437, CHAR));
  
  -- 将值转换为SIGNED数据类型
  -- 5
  SELECT CONVERT('5.0', SIGNED);
  -- 2
  SELECT (1 + CONVERT('3', SIGNED))/2;
  -- -5
  SELECT CONVERT(5-10, SIGNED);
  -- 6
  SELECT CONVERT(6.4, SIGNED);
  -- -6
  SELECT CONVERT(-6.4, SIGNED);
  -- 7
  SELECT CONVERT(6.5, SIGNED);
  -- -7
  SELECT CONVERT(-6.5, SIGNED);
  
  -- 将值转换为UNSIGNED数据类型
  -- 5
  SELECT CONVERT('5.0', UNSIGNED);
  -- 6
  SELECT CONVERT(6.4, UNSIGNED);
  -- 0
  SELECT CONVERT(-6.4, UNSIGNED);
  -- 7
  SELECT CONVERT(6.5, UNSIGNED);
  -- 0
  SELECT CONVERT(-6.5, UNSIGNED);
  
  -- 将值转换为DECIMAL数据类型 
  -- 9
  SELECT CONVERT('9.0', DECIMAL);
  -- DECIMAL(数值精度，小数点保留长度)
  -- DECIMAL(10,2)可以存储最多具有8位整数和2位小数的数字
  -- 精度与小数位数分别为10与2
  -- 精度是总的数字位数，包括小数点左边和右边位数的总和
  -- 小数位数是小数点右边的位数
  -- 9.50
  SELECT CONVERT('9.5', DECIMAL(10,2));
  -- 99999999.99
  SELECT CONVERT('1234567890.123', DECIMAL(10,2));
  -- 220.232
  SELECT CONVERT('220.23211231', DECIMAL(10,3));
  -- 220.232
  SELECT CONVERT(220.23211231, DECIMAL(10,3));
  ```

  

这里稍微介绍一个日常业务中碰到次数较多的需求：

```sql
select * from zz_users;
+---------+-----------+----------+----------+---------------------+
| user_id | user_name | user_sex | password | register_time       |
+---------+-----------+----------+----------+---------------------+
|       1 | 熊猫      | 女       | 6666     | 2022-08-14 15:22:01 |
|       2 | 竹子      | 男       | 1234     | 2022-09-14 16:17:44 |
|       3 | 子竹      | 男       | 4321     | 2022-09-16 07:42:21 |
|       4 | 黑熊      | 男       | 8888     | 2022-09-17 23:48:29 |
|       8 | 猫熊      | 女       | 8888     | 2022-09-27 17:22:29 |
|       9 | 棕熊      | 男       | 0369     | 2022-10-17 23:48:29 |
+---------+-----------+----------+----------+---------------------+

-- 基于性别字段分组，然后显示各组中的所有 ID
select 
    convert(
        group_concat(user_id order by user_id asc separator ",") 
    using utf8) as "分组统计" 
from `zz_users` group by user_sex;
+-------------+
| 分组统计    |
+-------------+
| 1,8         |
| 2,3,4,9     |
+-------------+
```

上述利用了 `group_concat()、group by` 实现了按照一个字段分组后，显示对应分组的所有 `ID`。

## 控制流程函数

- `if(expr,r1,r2)`：`expr`是表达式，如果成立返回`r1`，否则返回`r2`。
- `ifnull(v,r)`：如果`v`不为`null`则返回`v`，否则返回`r`。
- `nullif(v1,v2)`：如果`v1 == v2`，则返回`null`，如果不相等则返回`V1`。

## 加密函数

`password(str)`：将`str`字符串以数据库密码的形式加密，一般用在设置`DB`用户密码上。

`md5(str)`：对`str`字符串以`MD5`不可逆算法模式加密。

`encode(str,key)`：通过`key`密钥对`str`字符串进行加密（对称加密算法）。

`decode(str,key)`：通过`key`密钥对`str`字符串进行解密。

`aes_encrypt(str,key)`：通过`key`密钥对`str`字符串，以`AES`算法进行加密。

`aes_decrypt(str,key)`：通过`key`密钥对`str`字符串，以`AES`算法进行解密。

`sha(str)`：计算`str`字符串的散列算法校验值。

`encrypt(str,salt)`：使用`salt`盐值对`str`字符串进行加密。

`decrypt(str,salt)`：使用`salt`盐值对`str`字符串进行解密。

## 系统函数

- `database() | schema()`：返回当前连接位于哪个数据库，即`use`进入的库。

- `charset(str)`：返回当前数据库的编码格式。
- `collation(str)`：返回当前数据库的字符排序规则。

## 其他

你需要的某个功能在 `MySQL` 中没有提供函数支持，你也可以通过 `create function` 的方式自定义存储函数。

# 索引相关的命令

创建各类索引的多种方式：

```sql
-- 创建一个普通索引（方式①）
create index 索引名 ON 表名 (列名(索引键长度) [ASC|DESC]);
-- 创建一个普通索引（方式②）
alter table 表名 add index 索引名(列名(索引键长度) [ASC|DESC]);
-- 创建一个普通索引（方式③）
CREATE TABLE tableName(  
  columnName1 INT(8) NOT NULL,   
  columnName2 ....,
  .....,
  index [索引名称] (列名(长度))  
);
-- 后续其他类型的索引都可以通过这三种方式创建


-- 创建一个唯一索引
create unique 索引名 ON 表名 (列名(索引键长度) [ASC|DESC]);

-- 创建一个主键索引
alter table 表名 add primary key 索引名(列名);

-- 创建一个全文索引
create fulltext index 索引名 ON 表名(列名);

-- 创建一个前缀索引
create index 索引名 ON 表名 (列名(索引键长度));

-- 创建一个空间索引
alter table 表名 add spatial key 索引名(列名);

-- 创建一个联合索引
create index 索引名 ON 表名 (列名1(索引键长度),列名2,...列名n);
```

索引查看、使用与管理：

```sql
-- 查看一张表上的所有索引
show index from 表名;

-- 删除一张表上的某个索引
drop index 索引名 on 表名;

-- 强制指定一条SQL走某个索引查找数据
select * from 表名 force index(索引名) where .....;

-- 使用全文索引（自然搜索模式）
select * from 表名 where match(索引列) against('关键字');
-- 使用全文索引（布尔搜索模式）
select * from 表名 where match(索引列) against('布尔表达式' in boolean mode);
-- 使用全文索引（拓展搜索模式）
select * from 表名 where match(索引列) against('关键字' with query expansion);

-- 分析一条SQL是否命中了索引
explain select * from 表名 where 条件....;
```

# 事务与锁相关的命令

- `start transaction; | begin; | begin work;`：开启一个事务。

- `commit;`：提交一个事务。

- `rollback;`：回滚一个事务。

- `savepoint 事务点名称;`：添加一个事务点。

- `rollback to 事务点名称;`：回滚到指定名称的事务点。

- `release savepoint 事务点名称;`：删除一个事务点。

- `select @@tx_isolation;`：查询事务隔离级别（方式一）。

- `show variables like '%tx_isolation%';`：查询事务隔离级别（方式二）。

`- set transaction isolation level 级别`：设置当前连接的事务隔离级别。

- `set @@tx_isolation = "隔离级别";`：设置当前会话的事务隔离级别。

- `set global transaction isolation level 级别;`：设置全局的事务隔离级别，选项如下：
  - `read uncommitted`：读未提交级别。
  - `read committed`：读已提交级别。
  - `repeatable-read`：可重复读级别。
  - `serializable`：序列化级别。

- `show variables like 'autocommit';`：查看自动提交事务机制是否开启。

- `set @@autocommit = 0|1|ON|OFF;`：开启或关闭事务的自动提交。

- `select ... lock in share mode;`：手动获取共享锁执行 `SQL` 语句。

- `select ... for share;`：`MySQL8.0` 之后优化版的共享锁写法。

- `select ... for update;`：手动获取排他锁执行。

- `lock tables 表名 read;`：获取表级别的共享锁。

- `lock tables 表名 write;`：获取表级别的排他锁。

- `show open tables where in_use > 0;`：查看目前数据库中正在使用的表锁。

- `flush tables with read lock;`：获取全局锁。

- `unlock tables;`：释放已获取的表锁/全局锁。

- `update 表名 set version=version+1 ... where... and version=version;`：乐观锁模式执行。

# MySQL 常见的错误码

`MySQL` 的错误信息由 `ErrorCode、SQLState、ErrorInfo` 三部分组成，即错误码、`SQL` 状态、错误信息三部分组成，如下：

> ERROR 1045 (28000): Access denied for user 'zhuzi'@'localhost' (using password: YES)

其中 `1045` 属于错误状态码，`28000` 属于 `SQL` 状态，后面跟着的则是具体的错误信息，不过 `MySQL` 内部大致定义了两三千个错误码，其错误码的定义位于`include/mysqld_error.h、include/mysqld_ername.h` 文件中，而 `SQLState` 的定义则位于 `include/sql_state.h` 文件中，所有的错误信息列表则位于 `share/errmsg.txt` 文件中。

[mysql错误代码对照表](https://www.cnblogs.com/cuianbing/p/16260796.html)