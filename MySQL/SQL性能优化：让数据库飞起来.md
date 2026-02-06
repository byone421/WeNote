SQL优化这个问题，已经是老生常谈了。它既是面试中的常客又是工作中我们实际会面临到的问题。这里我分享一下我对SQL优化的理解，希望对大家有点帮助。本文将以MySQL为例来说明解释SQL优化。

SQL语句无非增删改查，下面我们会分情况来进行讨论。

# 1. 表结构设计的建议
要让一条SQL语句达到最佳性能与其表结构设计也是分不开的，以下是一些建议：

## 1.1 更小的类型通常更好
更小的类型通常意味着：

+ 占用更少的磁盘空间 ;
+ 占用更少的内存（Buffer Pool / Sort Buffer / Join Buffer）
+ CPU 处理更快（拷贝、比较、排序）

## 1.2 数字通常比字符串更优
因为：

+ 数字只需一次比较， 字符串需要逐字符比较，还涉及字符集 / 排序规则（collation）
+ 数字在 排序、分组时更快  

##  1.3 尽量使用 NOT NULL  
因为：

+ NULL 需要额外的空间来标记该字段是否为NULL， NOT NULL 直接存值  
+  NULL 值在索引里要特殊处理 ，IS NULL/IS NOT NULL 优化空间有限 
+  NULL会导致三值逻辑（TRUE / FALSE / UNKNOWN）
    - 任何值和 NULL 比较，结果都不是 TRUE，也不是 FALSE，而是 UNKNOWN

##  1.4 主键保持有序
主键应该短、稳定、递增，避免使用UUID、随机字符串、业务字段（随时可能变）

##  1.5 为“查询而设计索引，而不是为字段设计索引”  
不是因为这个字段常用，就建个索引。 而是应该考虑这条 SQL 怎么查？WHERE / ORDER BY / GROUP BY 是什么 ，考虑索引应该怎么建。

##  1.6 选择合适的字符集与排序规则
对于现代应用推荐统一使用utfmb4字符集，兼容性最好，也能存 emoji 和特殊符号。  

对于排序规则，以下是一个对比：

| **<font style="color:rgb(31, 31, 31);">排序规则</font>** | **<font style="color:rgb(31, 31, 31);">性能</font>** | **特点** | **<font style="color:rgb(31, 31, 31);">准确性</font>** | **<font style="color:rgb(31, 31, 31);">适用场景</font>** |
| --- | --- | --- | --- | --- |
| utf8mb4_0900_ai_ci | <font style="color:rgb(31, 31, 31);">极高</font> | Unicode 9.0，忽略重音、大小写 | <font style="color:rgb(31, 31, 31);">最高</font> | MySQL 8.0 默认推荐。符合现代语言标准。 |
| utf8mb4_general_ci | <font style="color:rgb(31, 31, 31);">最高</font> | 不区分大小写，简单比较 | <font style="color:rgb(31, 31, 31);">较低</font> | <font style="color:rgb(31, 31, 31);">早期为了提速舍弃了部分复杂语言规则（如德国的 ß）。现代 CPU 下性能优势已不明显。</font> |
| <font style="color:rgb(68, 71, 70);">utf8mb4_unicode_ci</font> | <font style="color:rgb(31, 31, 31);">中等</font> | 遵循 Unicode 排序规则，更精确 | <font style="color:rgb(31, 31, 31);">高</font> | <font style="color:rgb(31, 31, 31);">基于 Unicode 4.0 算法，比 general 准，但比 0900 旧。</font> |
| utf8mb4_bin | <font style="color:rgb(31, 31, 31);">极高</font> | 按二进制比较，区分大小写 | <font style="color:rgb(31, 31, 31);">严格</font> | 二进制比较。<font style="color:rgb(31, 31, 31);">区分大小写，区分重音。</font> |




# 2. 选择合适的数据类型
## 2.1 字符串类型
#### VARCHAR
VARCHAR类型大家也用的最多，倒没有什么特别要说明的，只需要针对实际情况明确合适的最大长度。也不能给过大的长度，否则会影响索引长度限制以及优化器对行大小的估算。

#### CHAR
CHAR类型存储的数据长度固定，或者数据长度相差无几就可以考虑使用CHAR类型。

由于 CHAR储存时会右填充空格，如果存储的多行数据长度相差太多，会极大的浪费空间。

比如定义了一个CHAR(20)  每一行都固定吃20个字符。

| 实际值 | 占用空间 |
| --- | --- |
| 'a' | 20(浪费了空间) |
| 'aaaaaaaaaaaaaaaaaaaa' | 20 |


在长度固定且较短的场景下，CHAR 比 VARCHAR 更优，因为它是定长存储，比较和索引时无需处理长度信息，内存布局更规整，性能会更稳定 。

#### TEXT
在使用TEXT系列的数据类型时应该慎之又慎 ，TEXT的缺点是无法高效索引、排序或分组，存储不连续占用额外 IO。不能设默认值。绝对不要出现在WHERE、JOIN、ORDER BY 、GROUP BY子句中。这种数据类型应该作为一个低频访问的数据。在列表查询类的语句中应当避免查询出这类数据。

#### 长字符串的建议

- 建立索引时优先使用前缀索引，用字符串前 N 个字符过滤大部分数据，显著降低索引体积。

  ```sql
  CREATE INDEX idx_name ON t_user(name(20));
  ```

- 查询条件与索引前缀保持一致，比较时避免对字段做函数或全量比较，必要时对参数做同样的截断处理。
- 能用数字 ID 就不要用字符串，考虑对长字符串做 hash / 映射成整数，再通过关联查询取原值。

## 2.2 数字类型
### 整数
整数类型有多种类型：

| 类型              | 占用字节 | 占用位数 | 最小值                     | 最大值                    |
| ----------------- | -------- | -------- | -------------------------- | ------------------------- |
| **TINYINT**       | 1        | 8        | -128                       | 127                       |
| **SMALLINT**      | 2        | 16       | -32,768                    | 32,767                    |
| **MEDIUMINT**     | 3        | 24       | -8,388,608                 | 8,388,607                 |
| **INT / INTEGER** | 4        | 32       | -2,147,483,648             | 2,147,483,647             |
| **BIGINT**        | 8        | 64       | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |

在设计时：

- 尽量用最小的能容纳业务范围的类型；
- 主键优先采用整数自增；
- 避免使用大整数来存短码，比如用BITINT来存0和1；
- 在存储码值时，避免使用字符串类型，而更应该考虑数值类型，比如有“开始/进行中/结束”三种状态，可以用0/1/2来代替，同时维护好字段备注信息。

**同时整数类型还具有如下优点：**

1. 占用空间小：例如TINYINT占用1字节，索引更紧凑，由于占用少，能在内存中存储更多的数据，就不用从磁盘拿数据。
2. 比较和运算速度快：CPU 内部直接整数运算
3. 索引效率高：整数类型索引 B+Tree 节点紧凑

#### 实数
| 类型              | 占用字节 | 是否精确 | 说明                                  |
| ----------------- | -------- | -------- | ------------------------------------- |
| **FLOAT**         | 4        | 不精确   | 单精度浮点数，存在精度误差            |
| **DOUBLE**        | 8        | 不精确   | 双精度浮点数，精度比 FLOAT 高         |
| **DECIMAL(M, D)** | 可变     | 精确     | 定点数，推荐用于金额、精度敏感场景    |
| **NUMERIC(M, D)** | 可变     | 精确     | DECIMAL 的同义词，与 DECIMAL 完全等价 |

**DECIMAL(M, D) 参数说明**

| 参数                    | 含义                         | 取值范围 |
| ----------------------- | ---------------------------- | -------- |
| **M（精度 Precision）** | 数字的总位数（不包括小数点） | 1 ~ 65   |
| **D（标度 Scale）**     | 小数点后的位数               | 0 ~ 30   |
| 约束                    | 标度不能大于精度             | D ≤ M    |

在设计时， 能用整数表示的金额、百分比、状态码就用整数，不行就考虑实数类型。

####  浮点数（FLOAT / DOUBLE）特点
1. 优点：存储范围大，可表示非常大或非常小的值，占用空间比同精度 DECIMAL 少

  2.  缺点：精度有限，会有舍入误差，不适合金融或精确计算（如金额）

3. 索引：可以建索引，但浮点比较容易出现精度问题

#### 精确数（DECIMAL / NUMERIC）特点
1. 优点：精确存储，不会丢失精度金融业务必选（如金额、利率）
2. 缺点：占用存储比浮点大，运算比浮点慢
3. 索引：可以建索引，精度和比较安全

## 2.3 日期类型
对于日期相关的类型，这里只讨论一下**DATETIME**和 **TIMESTAMP**  。其他的数据类型，根据业务需求选择合适的类型即可。

### DATETIME 与 TIMESTAMP 对比（仅讨论时区行为）

在 MySQL 中，DATETIME 与 TIMESTAMP 的索引性能几乎一致，不能作为选型依据。
 两者的核心差异在于：是否参与时区转换。

- **DATETIME** 存的是「你写进去的时间本身」
- **TIMESTAMP** 存的是「UTC 时间点」，显示时会根据当前 session 时区自动转换



#### 时区行为对比

**DATETIME（不关心时区）**

```sql
INSERT '2026-02-03 10:00:00'
```

写入什么，数据库就存什么; 不做任何时区转换; 读取结果始终一致

#### TIMESTAMP（自动时区转换）

```sql
INSERT '2026-02-03 10:00:00'
```

写入时：当前 session 时区 → 转成 UTC → 存储

读取时：UTC → 转成当前 session 时区 → 返回

#### 示例说明（TIMESTAMP 时区转换）

 **前置条件**

- 表 `t1`
- 字段：`create_time TIMESTAMP`
- 当前 session / server 时区：`Asia/Shanghai`（UTC+8）

 **写入数据**

```sql
INSERT INTO t1(create_time)
VALUES ('2026-02-03 10:00:00');
```

- MySQL 处理逻辑：认为 2026-02-03 10:00:00 是 UTC+8
- 转换为 UTC：2026-02-03 02:00:00
- 数据库实际存储的是 UTC 时间

**读取数据：情况1 - 仍然使用 UTC+8**

```sql
SELECT create_time FROM t1;
```

- 取出存储值：2026-02-03 02:00:00

- 转回 UTC+8

- 返回结果：2026-02-03 10:00:00

**读取数据：情况2 - 切换为 UTC+0 查询**

```sql
SET time_zone = '+00:00';
SELECT create_time FROM t1;
```

- 取出存储值：2026-02-03 02:00:00

- 转换为 UTC+0

- 返回结果： 2026-02-03 02:00:00

### 基础属性对比
| 类型          | 存储字节 | 本质               | 时间范围                |
| ------------- | -------- | ------------------ | ----------------------- |
| **DATETIME**  | 8 字节   | 直接存年月日时分秒 | 1000-01-01 ~ 9999-12-31 |
| **TIMESTAMP** | 4 字节   | 存 UTC Unix 时间戳 | 1970-01-01 ~ 2038-01-19 |

# 3. 查询
接下来登场的是 最帅气、也是使用频率最高的查询语句 —— SELECT。
 像它这么帅的，一般都是主角。

我们先回顾一下 SELECT 查询语句的整体语法结构，然后再逐个子句进行分析。

```sql
SELECT DISTANCT
	<select_list>
FROM 
	<LEFT_TABLE> <JOIN_TYPE> 
JOIN <RIGHT_TABLE> ON <JOIN_CONDITION>
WHERE 
	<WHERE_CONDITION>
GROUP BY 
	<GROUP_BY_LIST>
HAVING 
	<HAVING_CONDITION>
ORDER_BY 
	<ORDER_BY_CONDITION>
LIMIT <[START],LIMIT_NUMBER>
```

## SELECT
SELECT控制了查询返回结果的列数。如果站在优化的角度来讲，请严格按照用多少列查多少列的原则来进行。

如果是为了开发上的便利，而对损失小小的性能损耗无所谓的情况下，那另当别论。

## DISTANCT
DISTINCT是在结果集上进行去重的操作 ， 如果返回的结果集比较大，DISTINCT 仍然要对所有返回行做去重。  

核心思想就是能不用 DISTINCT就不用；能在“索引阶段”完成去重，就不要在“结果阶段”去重。  

下面结合场景来说明：

**场景1：如果只想取一条数据**

```sql
SELECT DISTINCT user_id
FROM orders
WHERE user_id = 123;
-- 改成
SELECT user_id
FROM orders
WHERE user_id = 123
LIMIT 1;
```

 **场景2: 尽量让索引覆盖**  

```sql
-- 如果user_id上有索引最好了，MySQL 可以直接 扫描索引并去重
SELECT DISTINCT user_id FROM orders;
```

## JOIN
1. **JOIN 列必须有索引，且类型一致**

```sql
-- 要求orders.user_id和user.id都有索引，且两者类型要一致。不然可能导致：隐式类型转换导致索引失效
SELECT *
FROM orders o
JOIN user u ON o.user_id = u.id;
```

2.  **小表驱动大表**

```sql
-- 用小表连接大表
FROM small_table s
JOIN big_table b ON s.id = b.sid
```

3.  **JOIN 前先过滤 ， 参与 JOIN 的行数尽量少**

```sql
-- 不好
FROM orders o
JOIN user u ON o.user_id = u.id
WHERE o.status = 'PAID';

-- 更好，先用子查询过滤掉大部分数据
FROM (
  SELECT id, user_id
  FROM orders
  WHERE status = 'PAID'
) o
JOIN user u ON o.user_id = u.id;
```

4. **避免函数和表达式 ，不然导致索引失效**

```sql
-- 错误示例1
ON DATE(o.create_time) = u.reg_date
-- 错误示例2
ON o.user_id + 1 = u.id
```

5. **控制 JOIN 表的数量**

当JOIN超过3~4 张表时，应当考虑一下优化手段。建议可以先查核心表，中间结果落临时表 / 程序变量 再 JOIN 其他表。或者业务上是否允许修改成多条查询语句，因为一个很复杂的SQL，即使从可读性和可维护性来讲，也比较差。

## WHERE
如果说 SELECT 决定了查哪些列，那么 WHERE 决定的就是查多少行，而且——它的重要性远远高于 SELECT。WHERE 子句几乎决定了一条 SQL 能不能用上索引、扫描多少数据  。 在写WHERE子句时请保持**只查相关记录**的原则。

1.  **让条件用上索引**

这一点主要避免对列做函数 / 计算 / 隐式转换 等操作，造成索引失效。

```sql
-- 如果有索引会失效
WHERE DATE(create_time) = '2026-02-01'
-- 如果有索引会失效
WHERE amount + 1 = 100
-- 如果有索引会失效
WHERE CAST(user_id AS CHAR) = '123'

-- 以下是正确做法
WHERE create_time >= '2026-02-01'
  AND create_time <  '2026-02-02'

WHERE amount = 99
WHERE user_id = 123

-- 隐式转换，索引可能失效
-- user_id 是 INT
WHERE user_id = '123'   

```

上述总结就是 索引列要“裸奔”在等号左边

2.  **匹配索引的最左前缀原则 ** 

 假设我们有一个联合索引：**INDEX(a, b, c)**

```sql
-- 用到索引 匹配了最左列。
WHERE a = 1
-- 用到索引 匹配了最左列。
WHERE a = 1 AND b = 2 
-- 用到索引  顺序匹配。
WHERE a = 1 AND b = 2 AND c = 3 
-- 跳过了最左列 a，索引失效
WHERE b = 2 
--  部分使用到了联合索引，仅 a 参与索引定位，由于中间列 b 未参与匹配，索引顺序在 b 处被打断，后续的 c 无法继续参与索引定位。
WHERE a = 1 AND c = 3 
-- 部分使用到了联合索引，a 使用范围查询后，b 无法继续参与索引定位。
WHERE a > 1 AND b = 2
```

3. **尽量避免 OR，必要时用 UNION ALL  **

```sql
-- OR 常导致全表扫描
WHERE status = 1 OR type = 2

-- 比OR更稳定的写法
SELECT ... WHERE status = 1
UNION ALL
SELECT ... WHERE type = 2
```

4. **范围条件尽量“缩小扫描区间”** 

```sql
WHERE create_time >= '2026-01-01'

-- 让查询范围更加精确，减少扫描的数据页
WHERE create_time >= '2026-01-01'
 AND create_time <  '2026-01-02'
```

5.  **关于IN / EXISTS 的取舍**

```sql
-- IN的执行逻辑： MySQL 会先执行子查询，把 Canceled_Orders 中 1 万个 ID 取出来放到内存（临时表）里，然后去主表里匹配
-- 适用于当子查询的结果集远小于外表时，效率很高。
SELECT * FROM Orders 
WHERE order_id IN (SELECT order_id FROM Canceled_Orders);

-- EXISTS的执行逻辑： 遍历 Orders 表，对每一行订单 ID，都去 Canceled_Orders 里看看有没有对应的记录
-- 如果 Canceled_Orders 很大，但Orders数据少，那 EXISTS 会更快。
SELECT * FROM Orders o
WHERE EXISTS (SELECT 1 FROM Canceled_Orders c WHERE c.order_id = o.order_id);
```

上述结论：

- 子查询小 → IN 更合适
-  外表小 → EXISTS 更合适
-  两边都大 → 看索引和执行计划，语法本身不重要

6. **关于NOT IN 和 NOT EXISTS 的取舍**

尽量别用NOT IN，能不用就不要用。

```sql

-- 为什么不推荐NOT IN:
-- 第一点：NOT IN 存在NULL 陷阱，对于以下SQL:
SELECT * FROM Orders 
WHERE order_id NOT IN (SELECT order_id FROM Canceled_Orders);
-- 只要子查询中存在任意一个 NULL ，那么就是order_id NOT IN (1, 2, NULL)
-- SQL 的三值逻辑结果是：UNKNOWN → 整体条件不成立，导致一条数据也查不出来
-- 第二点：NOT IN 很难利用索引优化，容易退化为全表扫描.



-- 推荐写法1
SELECT o.* FROM Orders o
LEFT JOIN Canceled_Orders c ON o.order_id = c.order_id
WHERE c.order_id IS NULL;
-- 推荐写法2
SELECT * FROM Orders o
WHERE NOT EXISTS (
    SELECT 1 FROM Canceled_Orders c WHERE c.order_id = o.order_id
);
```



##  GROUP BY
如果说GROUP BY分组慢，通常慢在这些事上: 

- 扫描数据多
- 文件排序（filesort）
- 使用了临时表(Using temporary )

1.  **优先使用索引完成分组**  

```sql
-- 假设有 INDEX (a, b)
-- 如果 GROUP BY 的列 与索引顺序一致，MySQL 可以直接按索引顺序分组：
SELECT a, COUNT(*)
FROM t
GROUP BY a;

-- GROUP BY 列顺序要和索引一致
-- 正确
GROUP BY a, b 
-- 错误
GROUP BY b, a

```

2. **WHERE 先过滤，再 GROUP BY**

```sql
-- 禁止这样写，这样是先GROUP BY再HAVING
SELECT status, COUNT(*)
FROM t
GROUP BY status
HAVING status < 4


-- 正确： 能在 WHERE 干掉的数据，绝不留给 GROUP BY
SELECT status, COUNT(*)
FROM t
WHERE status < 4
GROUP BY status;

```

3.  **分组越少越好**

```sql
-- 分组列越多，分组成本越高
GROUP BY user_id, create_time

-- 如果可以请减少分组列
GROUP BY user_id
```

4.  **避免 GROUP BY + 大量 DISTINCT**

 GROUP BY本身就需要对数据进行分组（通常涉及临时表或排序），而每一个DISTINCT字段都需要在每个分组内维护一个**独立的内存数据结构**来计算唯一值。如果数据量大，这会导致严重的 CPU 和内存消耗。 

```sql
-- 这种写法会对每一行进行多次去重判断，极慢
SELECT status, COUNT(DISTINCT user_id)
FROM orders
GROUP BY status;


-- 应该先在子查询里完成去重，外层只做单纯的聚合
SELECT status, COUNT(user_id)
FROM (
    SELECT status, user_id
    FROM orders
    GROUP BY status, user_id -- 利用复合索引或排序完成初步去重
) t
GROUP BY status;
```

 5. **GROUP BY + ORDER BY 尽量复用索引**

```sql
INDEX (a, b)

-- 正确做法
GROUP BY a
ORDER BY a 

-- 不理想的做法，容易导致文件排序
GROUP BY a
ORDER BY b

```

## HAVING
针对HAVING的优化的核心原则就是能不用就不用，能放在WHERE里面就不要放在HAVING里面。HAVING是针对结果集来操作的。

1.  **能用 WHERE，就绝不用 HAVING**

```sql
-- 错误
SELECT status, COUNT(*)
FROM t_order
GROUP BY status
HAVING status > 1;

-- 正确
SELECT status, COUNT(*)
FROM t_order
WHERE status > 1
GROUP BY status;
```

2.  **HAVING 只做“必须在分组后才能判断”的事**

 只能在 HAVING 里的典型场景 ， 涉及聚合函数，HAVING 才有存在价值  

```sql
HAVING COUNT(*) > 10
HAVING SUM(amount) >= 10000
HAVING MAX(score) < 60
```

3.  **HAVING 之前的数据量越小越好**

```sql
-- 大数据量场景性能不好
SELECT user_id, COUNT(*)
FROM t_order
GROUP BY user_id
HAVING COUNT(*) > 100;


-- 大数据量场景：用子查询减少数据量
SELECT user_id, COUNT(*)
FROM (
  SELECT user_id
  FROM t_order
  WHERE status = 1
) t
GROUP BY user_id
HAVING COUNT(*) > 100;
```

## ORDER BY 
如果业务允许，尽量避免排序（ORDER BY）因为排序会消耗 CPU 和临时表资源。 ORDER BY 的优化目标是：利用索引顺序直接返回有序结果。

1.  **ORDER BY 列必须和索引顺序一致**  

```sql
-- 假设有INDEX (a, b)
-- 正确写法
ORDER BY a
ORDER BY a, b

-- 错误写法:顺序不一致会导致filesort
ORDER BY b
ORDER BY b, a
```

2.  **ORDER BY 的方向要一致**  

```sql
-- INDEX (a, b)
-- 错误
ORDER BY a ASC, b DESC
-- 正确
ORDER BY a ASC, b ASC
```

3.  **排序字段尽量小、简单**  

```sql
-- 用 INT / DATETIME
-- 少用 VARCHAR / TEXT

ORDER BY id    -- 优于 ORDER BY name
```

4. **争取只排少量数据**

```sql
-- 分页 / 列表页一定要有 LIMIT
ORDER BY create_time DESC
LIMIT 20
```

5.  **避免 ORDER BY 表达式 / 函数**  

```sql
-- 错误：导致索引失效
ORDER BY DATE(create_time)
ORDER BY ABS(score)

-- 正确
ORDER BY create_time
```

5. **需要注意深度分页**

```sql
-- 问题： 仍需排序并丢弃前 100000 行
ORDER BY create_time DESC
LIMIT 100000, 20


-- 让前端传上一页最后一条记录的 create_time，基于索引“接着往下查”
WHERE create_time < '2026-02-01 10:00:00'
ORDER BY create_time DESC
LIMIT 20

```

# 4. 插入

在所有数据库写操作中，INSERT 往往是最热闹、也是最容易让人头疼的一个。
本节将围绕 INSERT 操作，分析插入性能变慢的常见原因，并给出相应的优化建议。

## INSERT 慢的常见原因

**索引过多**

- 每次插入都要更新表的所有索引。
- 特别是组合索引、唯一索引，开销更大。

**单条写入过于频繁**

- 每条数据都单独提交，事务开销大。
- 网络往返和日志写入频繁。

**事务太碎**

- 每次插入都单独提交事务。
- 会导致 redo log、binlog、锁频繁写入和释放。

**随机写 + 页分裂**

- 如果主键或聚集索引是非自增字段，数据会随机写入磁盘。
- 页分裂（page split）增加磁盘 IO。

**约束 / 触发器开销**

- 外键约束、唯一性约束、检查约束。

- 表触发器每次插入都会执行，增加额外 CPU/IO。

## INSERT 优化建议

1. **批量 INSERT，别一条一条插** 

```sql
INSERT INTO t_order VALUES (...);
INSERT INTO t_order VALUES (...);
-- 改成批量插入, 节约网络成本
INSERT INTO t_order VALUES (...), (...), (...);
```

2.  **减少不必要的索引**  

索引维护成本通常大于插入本身，批量插入时可先移除非必要索引

```sql
-- 初始化数据时，可以先删除索引
ALTER TABLE t_order DROP INDEX idx_x;

-- 批量插入完成后，再添加索引
ALTER TABLE t_order ADD INDEX idx_x;

```

3.  **主键设计要“顺序写”**  

使用自增主键或顺序列，保证插入在页尾追加，避免页分裂和随机 IO。

4.  **显式指定列名，避免隐式转换**  

```sql
INSERT INTO t_order (id, status, amount) VALUES (...);
```

5. **大量数据导入**

对于海量数据导入，可使用 `LOAD DATA INFILE`，比普通 INSERT 高效许多。

6.  **延迟或异步非关键写入**  

对非关键数据，可先写入主流程，非核心数据异步处理，减轻主流程负载。

7. **优化事务**

批量插入时，按合理批次提交事务（如每 1000~5000 条一提交）

避免长事务造成锁等待或 undo log 压力过大

# 四、删除

接下来要聊的是让很多工程师都格外谨慎的操作 —— DELETE。在介绍具体优化手段之前，我们先看看 DELETE 会对系统产生哪些影响，再逐条分析如何安全、高效地删除数据

## DELETE 可能带来的影响

**写 redo / undo / binlog**

- 删除操作会记录事务日志，确保可回滚和数据一致性。

**锁行**

- 会对涉及的数据行加锁，阻塞其他事务。

**产生大量 undo**

- 对大批量删除可能导致 undo log 占用大量空间，影响其他事务。

**空间不会立即回收**

- InnoDB 表删除数据后，空间不会自动释放，需后续优化。

**可能触发级联 / 触发器**

- 外键级联删除或触发器会增加额外开销。

## DELETE 优化建议

对于删除的核心原则就是： 能不删就不删（逻辑删除优先）； 别一次性删太多、别让它长事务、别让它全表扫  

1. **能不删就不删（逻辑删除优先）**

避免频繁物理删除，可使用逻辑删除

```sql
UPDATE t_order SET deleted = 1 WHERE ...;
```

2. **必须有 WHERE，且尽量命中索引**

避免全表扫描

```sql
-- 错误写法，会全表扫描
DELETE FROM t_order WHERE DATE(create_time) < '2025-01-01';

-- 正确写法，命中索引
DELETE FROM t_order WHERE create_time < '2025-01-01';
```

3. **分批删除**  

大量数据一次性删除会导致锁过多和 undo 压力

```sql
-- 错误示例：一次性删除大量行
DELETE FROM t_order WHERE status = 9;

-- 正确示例：分批删除
DELETE FROM t_order
WHERE status = 9
LIMIT 1000;
```

4. **用主键  /  覆盖索引删除**

利用索引减少扫描和锁行  

```sql
DELETE FROM t_order
WHERE id IN (
  SELECT id
  FROM t_order
  WHERE status = 9
  LIMIT 1000
);

```

5. **确认无误后再删**

避免误删和评估影响范围

```sql
SELECT COUNT(*) FROM t_order WHERE status = 9;

DELETE FROM t_order WHERE status = 9;
```

6. **快速删除方法**

分区删除：适合分区表删除历史数据

整表清空：比 DELETE 更快，且不会生成大量 undo

```sql
-- 删除分区
ALTER TABLE t_order DROP PARTITION p2024;

-- 删除整表
TRUNCATE TABLE t_order;

```



# 五、更新
最后登场的是数据库中最灵活、也最容易引发性能问题的写操作 —— UPDATE。 UPDATE 既可以精准地修改少量数据，也可能在使用不当时对数据库造成巨大压力。我们一起来看看吧

## UPDATE 可能带来的影响

**写操作开销大**

- 每次更新都要写 redo log、undo log 和 binlog，尤其是大批量更新时。

**行锁及范围锁**

- 更新操作会加行锁，部分情况下可能升级为范围锁，影响并发性能。

**索引字段更新成本高**

- 更新索引列（尤其是主键）会导致表中所有相关索引同步更新，开销极大。

**长事务问题**

- 大批量更新或长事务会导致锁等待、undo log 暴涨，影响其他事务。

**二级索引维护**

- 更新非主键索引列时，二级索引也要同步修改，增加 I/O 和 CPU 消耗。

## UPDATE 优化方法

1. **WHERE 条件必须命中索引**

 避免全表扫描，提高更新效率

```sql
-- 错误示例：使用函数导致索引失效
UPDATE t_order SET status = 2 WHERE DATE(create_time) = '2026-02-01';

-- 正确示例：范围查询命中索引
UPDATE t_order
SET status = 2
WHERE create_time >= '2026-02-01'
  AND create_time <  '2026-02-02';

```

2. 分批更新，避免长事务

大量数据一次性更新容易造成锁等待和 undo log 暴涨

```sql
-- 不建议：一次性更新大表
UPDATE t_order SET status = 9 WHERE status = 1;

-- 建议：分批更新
UPDATE t_order
SET status = 9
WHERE status = 1
LIMIT 1000;

```

3.  **避免更新索引列（尤其是主键）**

更新主键会导致表中所有索引都要重新维护，开销很大。

如果必须更新索引列，可考虑先查询主键，再按主键更新（参见下一条）。

4. **大批量变更：先查主键，再按主键更新**

利用主键定位数据，减少锁和扫描范围。

```sql
UPDATE t_order
SET status = 9
WHERE id IN (
  SELECT id
  FROM t_order
  WHERE status = 1
  LIMIT 1000
);

```

5. **合理使用条件更新（乐观锁）**

避免锁冲突，提高并发安全

只有在 version 匹配时才更新，实现乐观锁机制。

```sql
UPDATE t_order
SET status = 2, version = version + 1
WHERE id = 100
  AND version = 3;
```

# 六、其他优化
## 服务器参数调优说明
除了在 **SQL 层面**下功夫，MySQL 本身也提供了大量参数供我们调整。不过，有一点要注意：**不要轻易动这些参数**。
 MySQL 默认参数经过大量验证，是一个比较稳妥的配置，在大多数情况下已经够用。

通常，我们的精力应该放在 **表结构设计、索引优化和查询优化** 上，因为这些优化收益最大、见效最快。
 **只有当这些优化做完了，性能仍然达不到要求**，才考虑去调服务器参数。

由于篇幅有限，本文就不展开详细参数说明啦，后续可能会继续更新。需要的同学可以先自行查阅相关资料。

## 架构优化说明
除了参数层面的优化，架构设计也能显著提升数据库性能。这里给大家列几个常见方案作为参考：

1. **分库分表** —— 避免单表过大，降低热点数据压力
2. **主从架构** —— 读写分离，提高并发能力
3. **微服务** —— 将业务拆分，让数据库负载分散
4. **历史数据归档** —— 减少活跃表的数据量，提升查询性能
5. **热点数据放缓存** —— 常用数据放 Redis 或 Memcached，加速访问

参数调优和架构优化同样很重要，但涉及面较大，由于篇幅有限，本文就不展开讨论了。有兴趣的小伙伴可以查阅更多资料。
