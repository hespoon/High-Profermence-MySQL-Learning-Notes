# 登录本机的 MySQL 数据库 
  mysql -u root -p

# 登录 MySQL 服务器
  mysql -h 主机名 -u 用户名 -p
  -h：指定客户端需要登陆的 MySQL 主机名，登录本机时可以省略。
  -u：登录的用户名
  -p：告诉服务器将会使用一个密码登录。如果密码为空，可以忽略此选项。

# GRANT 命令

  * grant 作用在整个 MySQL 服务器上

    ``` SQL
    grant select on *.* to username@host;
    grant all on *.* to username@host;
    ```

  * grant 作用在单个数据库上

    ```sql
    grant select on database.* to username@host;
    ```

  * grant 作用在单个数据表上

    ```sql
    grant select on database.table to username@host;
    ```

  * grant 作用在表中的列上

    ```sql
    grant select(cloumn1,column2) on database.table to username@host;
    ```

  * grant 作用在存储过程或函数上

    ```sql
    grant execute on procedure database.table to username@host;
    grant execute on function database.table to username@host;
    ```
# Join 子句

  在关系数据库中，许多有关系的表通过外键相互关联起来。因此，从业务角度来看，每个表中的数据都是不完整的，需要同时查询多个表才能得到完整的业务数据。这就需要用到 Join 子句。

  Join 子句是在一个（self-join）或多个表中连接数据的方法，这个连接基于这些表之间的公共列。

  MySQL 支持四种类型的连接：`Inner Join`、`Left Join`、`Right Join` 和 `Cross Join`

  MySQL 目前还不支持 `FULL OUTER JOIN`

  Join 子句使用在 SELECT 语句的 FROM 子句后面。

# INNER JOIN 命令

  * INNER JOIN 的维恩图

    ![](http://ww1.sinaimg.cn/large/006XJF4Oly1gc3n85eg10j309z08eq32.jpg)

    <center>INNER JOIN - Venn Diagram</center>
  
  * 内部连接将一个表和其他表匹配起来，然后可以在这些匹配的表中查询数据。
  
  * INNER JOIN 语句是 SELECT 语句的一个可选子句，它紧挨着 FROM 语句后面出现。语法格式如下：
  
    ```sql
    SELECT select_list
    FROM table1
    INNER JOIN table2 ON join_condition1
    INNER JOIN table3 ON join_condition2
    ...;
    ```
  
  * join_condition 语句指明了两个表格关联的规则。比如：

    ```sql
    SELECT select_list
    FROM t1
    INNER JOIN t2 ON join_condition;
    ```
  
    INNER JOIN 子句会将 t1 和 t2 中的每一行进行两两匹配，查看两行是否符合 join_condition 指明的条件。若符合，则创建一个新行，该行包含 t1 和 t2 中的所有列，并将该行放入查询结果集中，供 SELECT 语句选择。若不符合，则忽略这次匹配。
  
    一般来说，通常会关联有外键约束的两个表。
    
  * 如果相关联的两列有相同的名字，并且连着之间的逻辑关系是等于，则可以在 INNER JOIN 子句中使用 USING 子句，如下：
  
    ```SQL
    SELECT
    	productCode,
    	productName,
    	textDescription
    FROM
    	products
    INNER JOIN productlines USING (productline);
    ```

# LEFT JOIN

  LEFT JOIN 可以连接一个或多个表。

  引入了 Left table 和 right table 的概念

  Left Join 将左表中的行与右表中的行匹配，如果两行符合 Left Join 子句的判定条件，则创建一个包含两表所有列的新行放入结果集中，供 SELECT 语句选择。若对于选中的左表中的行，右表中没有与之匹配的行，则也创建一个包含两表所有列的新行，并将新行放入结果集中，供 SELECT 语句选择。新行中属于左表中的列有值，且值与左表中对应的行相同，新行中属于右表中的列的值全为 NULL。

  也就是说，LEFT JOIN 选择左表中的所有数据，无论右表中是否有与之匹配的行。

  基本语法如下：

  ```SQL
  SELECT column_list
  FROM table1
  LEFT JOIN table2 ON join_condition;
  ```

  LEFT JOIN 支持 USING 子句。

  LEFT JOIN 的维恩图如下：

  ![](http://ww1.sinaimg.cn/large/006XJF4Oly1gc3ps0rg78j30dw087mx8.jpg)

  <center>LEFT JOIN - Venn Diagram</center>

  可以使用 WHERE 语句选择只存在于左表中的数据。语法如下：

  ```SQL
  SELECT column_list
  FROM table1
  LEFT JOIN table2 ON join_condition
  WHERE column_table1 IS NULL;
  ```

  维恩图如下：

  ![](http://ww1.sinaimg.cn/large/006XJF4Oly1gc3pxfzmcwj30f208wt8u.jpg)

  <center>LEFT JOIN only in left table - Venn Diagram</center>

  在 LEFT JOIN 中，WHERE 子句和 ON 子句的效果不同。

# RIGHT JOIN

  RIGHT JOIN 与 LEFT JOIN 正好相反。

  RIGHT JOIN 选择所有右表中的数据，无论左表中是否存在与之匹配的行。若不存在，则左表中的列在结果集中全为 NULL。

  基本语法如下：

  ```SQL
  SELECT column_list
  FROM table1
  RIGHT JOIN table2 ON join_condition;
  ```

  维恩图如下：

  ![rightJoin](http://ww1.sinaimg.cn/large/006XJF4Oly1gc3q423f2tj30cz07jmx8.jpg)

  <center>RIGHT JOIN - Venn Diagram</center>

  可以使用 USING 子句

  可以使用 WHERE 语句选择只在右表中存在的数据，语法框架如下：

  ```SQL
  SELECT column_list
  FROM table1
  LEFT JOIN table2 ON join_condition
  WHERE column_table2 IS NULL;
  ```

  维恩图如下：

  ![rightJoin-onlyinright.png](http://ww1.sinaimg.cn/large/006XJF4Oly1gc3q792ax9j30cx07f74e.jpg)

  <center>RIGHT JOIN only in right table - Venn Diagram</center>

# CROSS JOIN

  CROSS JOIN 返回连接表的笛卡尔积。

  CROSS JOIN 没有连接条件。即没有 ON 和 USING 子句。

  CROSS JOIN 将左右两表的每一行都连接起来形成结果集。

  假设第一个表有 n 行，第二个表有 m 行，则 CROSS JOIN 后的表有 $n \times m$ 行。

  如果在 CROSS JOIN 中使用了 WHERE 子句，则 CROSS JOIN 就会和 INNER JOIN 效果一样。

  ```SQL
  SELECT * FROM table1
  CROSS JOIN table2
  WHERE table1.id = table2.id;
  ```

# SELF JOIN

  SELF JOIN 通常用来查询有多个层次的数据或者比较同一个表中的不同的行。

  使用 SELF JOIN 时，必须使用表的别名来避免在同一个查询中重复使用同一个表名。否则将会报错。

# DELIMITER 命令

  DELIMITER 命令用于修改 MySQL 中默认的分隔符。

  MySQL 中默认的分隔符是分号 `;`

  但是，当你定义一个存储过程是，有可能需要使用分号 `;` 来分割同一个存储过程中的不同句子。此时，为了使 MySQL 将包含分号 `;` 的存储过程当作一个完整的语句看待，应当在存储过程语句之前，使用 DELIMITER 命令修改 MySQL 的语句分隔符。

  命令用法如下：

  ```SQL 
  DELIMITER delimiter_character
  ```

  delimiter_character 可以是一个或多个字符，但是要避免使用反斜杠 `\`，因为反斜杠是转义字符。delimiter_character 后不用加分号。

  想要将分割符换回分号，只要使用如下语句即可：

  ```SQL
  DELIMITER ;
  ```

# Trigger

  触发器是一个预先写好的程序。当触发器关联的表格进行 INSERT、UPDATE 或者 DELETE 操作时，触发器就会自动触发执行。和生命周期函数很像。

  MySQL 中，只有 INSERT、UPDATE 或者 DELETE 这三个事件能触发触发器。

  SQL 定义了两种触发器：行级触发器和语句级触发器

  行级触发器与行绑定。只要有一行被执行 INSERT、UPDATE 或者 DELETE，行级触发器就会被触发一次。比如，有一个表格有 100 行在 INSERT、UPDATE 或者 DELETE，那该表格的行级触发器就会被触发 100 次。

  语句级触发器与事务绑定。执行一次事务，触发一次语句级触发器。

  MySQL 只支持行级触发器，不支持语句级触发器。

  触发器的优点：

  触发器提供了一种检查数据完整性的方法。

  触发器可以捕获并处理来自数据库层的错误。

  触发器提供了运行计划任务的另一种方法。管理员无需等待计划任务的发生，触发器会在该任务发生之前或之后自动执行。

  触发器对审计表格的数据修改非常有用。

  触发器的缺点：

  触发器只能对数据做一些额外的验证，而不是全部验证。对于一些简单的数据验证，可以使用 NOT NULL、UNIQUE、CHECK 和 FOREIGN KEY 实现。

  触发器的故障可能比较隐蔽。因为触发器执行在数据库里面，在客户端不可见。

  触发器会增加 MySQL 服务器的负载。

  创建触发器的语法如下：

  ```SQL
  CREATE TRIGGER trigger_name
  {BEFORE|AFTER}{INSERT|UPDATE|DELETE}
  ON table_name FOR EACH ROW
  trigger_body;
  ```

  1. 首先给触发器一个名字，触发器的名字必须在同一个数据库中是唯一的，不可以有同名的触发器。
  2. 指明触发器的触发时机。`BEFORE` 或 `AFTER`
  3. 指明触发器的触发操作。`INSERT`、`UPDATE` 或 `DELETE`
  4. 指明绑定触发器的表格。
  5. 指明触发器被触发时执行的动作。如果想要在触发器被触发时执行多条语句，要使用 `BEGIN END` 语句。

  在触发器体可以访问被修改的列的值。使用 `NEW` 和 `OLD` 区分修改后和修改前的值。

| 触发器事件 | `OLD` | `NEW` |
| :--------: | :---: | :---: |
|  `INSERT`  |  No   |  Yes  |
|  `UPDATE`  |  Yes  |  Yes  |
|  `DELETE`  |  Yes  |  No   |

# COUNT 函数

  COUNT 函数是一个聚合函数。

  COUNT 函数可以获取一个表中的总行数或者符合特定条件的行的个数。

  COUNT 函数有三种形式：`COUNT(*)`、`COUNT(expression)` 和 `COUNT(DISTINCT expression)`

  **`COUNT(*)` 函数**

  `COUNT(*)` 函数用于获取 `SELECT` 语句返回的结果集的总行数。重复行，非空行，空行都会被统计在内。

  **`COUNT(expression)` 函数**

  这个函数统计 `select` 语句结果集中使 expression 非空的行的个数。expression 可以是列名，也可以是控制流表达式和函数，如 `IF`、`IFNULL` 和 `CASE`。

  **`COUNT(DISTINCT expression)` 函数**

  这个函数返回使表达式不为 `NULL` 的非重复行的个数。DISTINCT 关键字是“不同的” 含义，重复的行将只被统计一次。

  COUNT 函数的返回值是 BIGINT 类型的整数，如果不存在符合条件的行，则返回 0。

  `COUNT(*)` 与 `GROUP BY` 联合使用时，返回各组中的行的个数。

  `COUNT(*)` 还可与 `HAVING` 关键字配合使用。

# LEFT 函数

  LEFT 函数是一个字符串函数，用于获取指定字符串的指定长度的左前缀。

  ```SQL
  LEFT(str,length);
  ```

  str 是原字符串，length 是一个正整数，指明左前缀的长度。

  当 str 或者 length 是 NULL 是，LEFT 函数返回 NULL。

  当 length 是 0 或 负数时，LEFT 函数返回一个空字符串。

  如果 length 超过了 str 的长度，LEFT 函数返回整个 str 字符串。

# OPTIMIZE TABLE 命令

  OPTIMIZE TABLE 命令用于清理表中的碎片。

  ```SQL
  OPTIMIZE TABLE table1 [, table2] ...
  ```

  命令执行时，MySQL 执行如下过程：

  1. 创建一个临时表
  2. 将原表的数据复制到临时表中，并删除原表
  3. 将临时表重命名为原表的名字

# 存储过程 Stored Procedures

  存储过程是存储在 MySQL 服务器上的一组 SQL 语句。

  下面是一个存储过称的例子：

  ```SQL
  DELIMITER $$
  CREATE PROCEDURE GetCustomers()
  BEGIN
  	SELECT 
  		customerName,
  		city,
  		state,
  		postalCode,
  		country
  	FROM
  		customers
  	ORDER BY customerName;
  END$$
  DELIMITER ;
  ```

  调用存储过程是执行如下语句即可：

  ```SQL
  CALL GetCustomers();
  ```

  当第一次调用存储过程时，MySQL 会在数据库中根据存储过程名查找存储过程。然后编译存储的代码，将编译后的代码放入缓存中并执行。当在同一个会话中第二次调用存储过程时，MySQL 会直接执行缓存中的代码，不用再编译。

  存储过程能够接受参数并能在执行后返回一些结果。

  存储过程可以包含控制流语句，如 IF，CASE，LOOP 等。

  存储过程可以调用其他存储过程或者存储函数。这样方便将存储过程模块化。

  **存储过程的优点**

  1. 存储过程可以降低客户端和 MySQL 服务器之间的网络流量

     客户端不用向 MySQL 服务器发送大量的 SQL 语句，只需发送存储过程名和参数即可。

  2. 可以将商业逻辑隐藏并集中在数据库中

  3. 可以使数据库管理更加方便，使数据库更安全

     数据库管理员可以给用户分配使用存储过程的权限，而不是使用表格的权限。

  **存储过程的缺点**

  1. 内存消耗

     使用的存储过程越多，服务器需要分配个客户端连接的内存就越大。

     在存储过程中使用过多的逻辑运算会占用大量的 CPU，MySQL 对逻辑运算的支持并不是很好。

  2. 难以 DeBug

     MySQL 没有提供调试存储过程的工具。Oracle 和 SQL Sever 都有提供。

  3. 难以维护

     维护和升级存储过程比较困难

# DISTINCT

  是 SELECT 的一个子句，用于去除 SELECT 结果集中的重复行。

  ```SQL
  SELECT DISTINCT
  	select_list
  FROM
  	table_name;
  ```

  如果某一列中有多个 NULL 值，并且在该列上使用了 DISTINCT 子句，MySQL 将只保留一个 NULL 值。

  DISTINCT 也可以用在多个列上。在这种情况下，MySQL 用这些列的组合值来确定结果集中行的唯一性。

  **DISTINCT 子句与 GROUP BY 子句**

  如果单独使用 GROUP BY 子句，没有和聚合函数连用，那么 GROUP BY 的效果与 DISTINCT 相同。GROPU BY 会将结果集中的重复行去掉。

  一般来说，DISTINCT 就是 GROUP BY 的一种特殊情况。两者之间的不同是，GROUP BY 会将结果集排序，而 DISTINCT 不会。MySQL 8.0+ 移除了 GROUP BY 的排序操作，此时 GROPU BY 返回的数据不再是排好序的。

  如果将 DISTINCT 和 GROUP BY 一起使用，那么返回的结果集将是排好序的，和单独使用 GROUP BY 是同样的效果。

  **DISTINCT 与 聚合函数**

  DISTINCT 与聚合函数一起使用，可以在结果集传递给聚合函数之前，将结果集中的重复数据去除。

  ```SQL
  SELECT
  	COUNT(DISTINCT state)
  FROM
  	customers
  WHERE
  	country='USA';
  ```

  **DISTINCT 与 LIMIT 子句**

  DISTINCT 与 LIMIT 一起使用时，当满足 LIMIT 的条件时，MySQL 就会停止搜索。

  例如，下面的查询将会返回 5 个非空的无重复的结果

  ```SQL
  SELECT DISTINCT 
  	state
  FROM
  	customers
  WHERE
  	state IS NOT NULL
  LIMIT 5;
  ```

# 视图


视图是一种虚拟存在的表，是一种逻辑表。

比如，你进行了如下查询：

  ```SQL 
  SELECT column1, column2, column3
  FROM table1
  INNRE JOIN table2 USING (column4);
  ```

 过了一段时间，你还想在进行一次同样的查询，那就需要执行相同的查询语句。你可以选择再写一遍查询语句，或则把以前的查询语句存起来，比如存成一个 .sql 文件。然而，更好的方法是将这次查询存到数据库中并给它起个名字。这个被存起来的查询就是视图。

视图就是存储在数据库中的一组以命名查询。

视图使复杂的查询写起来更简单。

视图可以保持业务逻辑的一致性。可以把复杂的业务逻辑存储为视图，从而隐藏业务逻辑的复杂性。

视图可以更好的保护私密数据。可以把对私密数据的查询存储为视图，防止存储数据的表格的细节泄露。

视图可以保证后向兼容。比如，你想把一个大表格范式化为多个小表格，但又不想影响使用该大表格的应用的运行，你可以创建和新表名称相同的视图，这样所有使用新表的应用都可以使用这个视图。

