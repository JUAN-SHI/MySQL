# MySQL
### 使用mySQL
- 1.1 选择数据库：
```
例如：为了使用mySQL(mySql)数据库，应该输入以下内容：
输入： Use  mySql;(use 并不返回任何结果，依赖于使用的客户机，显示某种形式的通知。)
输出： Database changed (mysql命令行实用程序在数据库选择成功后显示的)
* 必须先使用use打开数据库，才能读取其中的数据。
```
- 1.2 了解数据库和表
```
  数据库、表、列、用户、权限等的信息被存储在数据库和表中（MySQL使用MySQL来存储这些信息）
可以使用MySQL的SHOW命令来显示这些信息
输入：SHOW DATABASES;(返回可用数据库的一个列表)

为了获得一个数据库内的表的列表，使用SHOW TABLES；
输入： SHOW TABLES（返回当前选择的数据库内可用表的列表）

SHOW也可以用来显示表列；
SHOW COLUMNS FROM 表名；(它对每个字段返回一行，行中包含字段名、数据类型、是否允许为NULL、键信息、默认值以及其他信息)

支持其他SHOW的语句：
SHOW STATUS 用于显示广泛的服务器状态信息
SHOW CREATE DATABASE和SHOW CREATE TABLE，分别用来表示创建特定数据库或表的MySQL语句；
SHOW GRANTS，用来显示授予用户(所有用户或特定用户)的安全权限；
SHOW ERRORS和SHOW WARNINGS，用来显示服务器错误的消息
```
### 检索数据
#### 2.1 SELECT语句
- 2.1.1 检索单个列
```
SELECT name(列) FROM user;
```
- 2.1.2 检索多个列
```
SELECT id,name(列),age FROM user;(列名之间用逗号隔开)
```
- 2.1.3 检索所有列
```
SELECT *(通配符) FROM user;（返回表中所有列）
```
- 2.1.4 检索不同的行
```
输入：SELECT DISTINCT id FROM products;(DISTINCT关键字指示MySQL只返回不同的值)
```
- 2.1.5 限制结果
```
SELECT name FROM user LIMIT 5;
此语句使用SELECT 语句检索单个列。 LIMIT 5指示MySQL返回不多于5行。

为得出下一个5行，可指定要检索的开始行和行数，如下：
SELECT name FROM user LIMIT 5,5;(指示MySQL返回从行5开始的5行。第一个数为开始位置，第二个数为要检索的行数。)
* 行0 检索出来的第一行为行0而不是行1.因此，LIMIT 1，1将检索出第二行而不是第一行
在MySQL5中支持LIMIT的另一种替代语法。LIMIT 4 OFFSET 3 意为从行3开始取4行，就像LIMIT 3，4一样。
```
### 排序检索数据
- 3.1 排序数据
```
ORDER BY子句取一个或多个列的名字，据此对输出进行排序。
输入：SELECT name FROM user ORDER BY name
```
- 3.2 按多个列排序
```
为了按多个列排序，只要指定列名，列名之间用逗号分开即可
下面列子检索三个列，并且按其中两个列对结果进行排序——首先按价格，然后再按名称排序
输入：SELECT id,price,name FROM products ORDER BY price,name;
仅在多个行具有相同的price值时才对产品按name进行排序。如果price列的值是唯一的，则不会按name排序。
```
- 3.3 指定排序方向

```
数据的排序不限于升序排序。这只是默认的排序顺序，还可以使用ORDER BY子句以降序的顺序排序，必须指定DESC关键字。
SELECT id,price,name FROM products ORDER BY price DESC;
DESC关键字只应用到直接位于其前面的列名。对于不指定的列以正常标准升序。* 在多个列上降序排序 如果想在多个列上进行降序排序，
必须对每个列指定DESC关键字。与DESC相反的关键字是ASC，在升序排序时可以指定它。
```

### 过滤数据
#### 4.1 使用WHERE子句
```
在SELECT语句中，数据根据WHERE子句中指定的搜索条件进行过滤。WHERE子句在表名（FROM子句）之后给出，
输入：SELECT name,price FROM products WHERE price=2.50;
注意：ORDER BY位于WHERE子句之后
```
#### 4.2 WHERE子句操作符
- 4.1.1 检查单个值
```
输入：SELECT name,price FROM products WHERE name='jack';
它返回的是name值为Jack的这一行，MySQL在执行匹配时默认不区分大小写。

1.列出价格小于10美元的所以产品
输入：SELECT name,price FROM products WHERE price <10;
2.列出价格小于等于10美元的所以产品
输入：SELECT name,price FROM products WHERE price <=10;
```

- 4.1.2 不匹配检查
```
列出不是由供应商1003制造的所有产品
输入：SELECT id,name WHERE id <>1003;
```
- 4.1.3 范围值检查
```
检查某个范围的值，用between操作符。
例：检索价格在5美元和10美元之间的所有产品
输入：SELECT name,price FROM products WHERE price BETWEEN 5 AND 10;
```
- 4.1.4 空值检查
```
在一个列不包含值时，称其为包含空值NULL。
NULL 无值，它与字段包含0、空字符串或仅仅包含空格不同。
SELECT语句有一个特殊的WHERE子句，可用来检查具有NULL值得列。这个WHERE子句就是IS NULL子句。语法如下：
SELECT name FROM products WHERE price IS NULL;
```
### 数据过滤
#### 5.1 组合WHERE子句（以AND子句的方式或OR子句的方式使用）
- 5.1.1 AND操作符
```
查询由供应商1003制造且价格小于等于10美元的所有产品的名称和价格。
输入：SELECT id,name,price FROM products WHERE id=1003 AND price<=10;
```
- 5.1.2 OR操作符
```
OR操作符与AND操作符不同，它指示MySQL检索匹配任一条件进行的行
查询由任一个指定供应商制造的所有产品的产品名和价格
输入： SELECT name,price FROM products WHERE id=1002 OR id=1003;
```
- 5.1.3 计算次序
```
查询价格为10美元(含)以上且由1002或1003制造的所有产品。
输入：SELECT id,price,name FROM products WHERE (id=1002 OR id=1003) AND price>=10; 
(SQL在处理OR操作符前，优先处理AND操作符。AND在计算次序中优先级更高。任何时候使用具有AND和OR操作符的WHERE子句，
都应该使用圆括号明确地分组操作符，不要过分依赖默认计算次序)
```
#### 5.2 IN操作符
```
IN操作符用来指定条件范围，范围中的每个条件都可以进行匹配。IN操作符用来指定条件范围，范围中的每个条件都可以进行匹配。IN取合法值的由逗号分隔的清单，全都括在圆括号中。
如查询供应商1002和1003制造的所有产品：
SELECT id,price,name FROM products WHERE id IN (1002,1003) ORDER BY name; 
也可以用OR来写：
SELECT id,price,name FROM products WHERE (id=1002 OR id=1003) ORDER BY name;
```
#### NOT操作符
```
WHERE子句中的NOT操作符有且只有一个功能，那就是否定它之后所跟的任何条件。
查询除了1002和1003之外的所有供应商制造的产品。
输入：SELECT name,price FROM products WHERE  id NOT IN(1002,1003) ORDER BY name;
```
### 用通配符进行过滤
#### 6.1 LIKE操作符
* 通配符：用来匹配值得一部分的特殊字符。
* 搜索模式： 由字面值、通配符或两者组合构成的搜索条件
- 6.1.1 百分号（%）通配符
```
在搜索串中，%表示任何字符出现任意次数。例如为了找出所有以XXX起头的产品，可使用以下语句：
输入：SELECT id,name FROM products WHERE name LIKE 'jet%';

查询以S开头以e结尾的所有产品：
输入：SELECT name FROM products WHERE name LIKE 's%e';
注意：除了一个或多个字符外，%还能匹配0个字符。%代表搜索模式中给定位置的0个、1个或多个字符。
```

- 6.1.2 下划线 (_) 通配符
```
 用途与%一样，但下划线只匹配单个字符而不是多个字符。
 输入：SELECT id,name FROM products WHERE name LIKE '_ ton anvil';
```

### 用正则表达式进行搜索
#### 7.1 使用MySQL正则表达式
- 7.1.1 基本字符匹配
```
查询检索列name包含文本1000的所有行
输入：SELECT name FROM products WHERE name REGEXP '1000' ORDER BY name;
输出：-------------
     |    name    |
     |------------|
     |jetPack 1000|
     --------------
     
查询检索列name包含文本000的所有行
输入：SELECT name FROM products WHERE name REGEXP '.000' ORDER BY name;
输出 +------------+
     |    name    |
     |------------|
     |jetPack 1000|
     |jetPack 2000|           |
     +------------+
 分析：这里使用了正则表达式 .000。.是正则表达式语言中一个特殊的字符。它表示匹配任意一个字符。
 
 问题：如果使用关键字LIKE会如何？
 SELECT name FROM products WHERE name LIKE '1000' ORDER BY name;
 SELECT name FROM products WHERE name REGEXP '1000' ORDER BY name;
 如果执行上述两条语句，第一条语句不返回数据，第二条返回一行数据。
 原因：LIKE匹配整个列，如果被匹配的文本在列值中出现，不会找到，相应的行也不会返回。而REGEXP在列值内匹配，
 如果被匹配的文本在列值中出现，REGEXP会找到，返回相应的行。
 REGEXP能用来匹配整个列值（从而与LIKE相同的作用），使用^和$定位符即可
 MySQL中的正则表达式匹配不区分大小写。为了区分大小写可使用BINARY关键字。 如：WHERE name REGEXP BINARY 'JetPack .000'.
```
- 7.1.2 进行OR匹配（使用|）
```
输入：SELECT name FROM products WHERE name REGEXP '1000|2000' ORDER BY name;
输出 +------------+
     |    name    |
     |------------|
     |jetPack 1000|
     |jetPack 2000|           
     +------------+
 使用|从功能上类似于在SELECT语句中使用OR语句，多个OR条件可以并入单个正则表达式。
 ```
 - 7.1.3 匹配几个字符之一
 ```
 输入：SELECT name FROM products WHERE name REGEXP '[123] Ton' ORDER BY name;
 输出 +------------+
      |    name    |
      |------------|
      | 1 ton anvil|
      | 2 ton anvil|           
      +------------+   
 分析：[123]定义一组字符，意为匹配1或2或3。[]是另一种形式的OR语句。
 ```

- 7.1.4 匹配范围
*集合可用来定义要匹配的一个或多个字符。

- 7.1.5 匹配特殊字符
```
查询包含 .字符的值。
输入： SLELECT name FROM vendors WHERE name REGEXP '\\.' ORDER BY name;
分析： .匹配任意的字符，因此为了匹配特殊字符，必须用\\为前导。 \\-表示查找- 。 \\.表示查找.
```
### 创建计算字段
#### 8.1 拼接字段
- 拼接：将值联结到一起构成单个值。在MySQL的SELECT语句中，可使用Concat()函数来拼接两个列
```
输入：SELECT Concat(name, '(',country,')') FROM verdors ORDER BY name;
输出  +----------------------------------+
      |  Concat(name,  '(',country,')') |
      |---------------------------------|
      | ACME （USA)                     |
      | Anvils  R Us (USA)              |
      | Furball Inc. (USA)              |
      | Jet Set (England)               |
      | Jouets Et Ours (France)         |
      +---------------------------------+
 
 通过删除数据右侧多余的空格来整理数据，可以使用MySQL的RTrim()函数来完成。
 输入：SELECT Concat(RTrim(name), '(', RTrim(country),')') FROM verdors ORDER BY name; 
 分析：RTrim()函数去掉值右边的所有空格。通过使用RTrim(),各个列都进行了整理。
```
#### 8.2 执行算术计算
```
查询订单号为20005中的所有物品中包含订单中每项物品的单价。
输入：SELECT id,quantity,price,quantity * price AS expanded_price FROM orderitems WHERE num=20005;

输出  +--------+----------+---------+-----------------+
      |   id   | quantity |  price  | expanded_price |
      |--------+----------+---------+----------------+
      |  ANV01 |    10    |   5.99  |     59.90      |
      |  ANV02 |     3    |   9.99  |     29.97      |
      |  TNT2  |     5    |  10.00  |     50.00      |
      |   FB   |     1    |  10.00  |     10.00      |
      +--------+----------+---------+----------------+      
```

### 使用数据处理函数
#### 9.1 函数
- 9.1.1 常用的文本处理函数
```
——————————————————————————————————————————————————————————————————————————
           函数                                       说明
——————————————————————————————————————————————————————————————————————————
          Left()                                   返回串左边的字符
          Length()                                 返回串的长度
          Locate()                                 找出串的一个子串
          LTrim()                                  去掉串左边的空格
          Right()                                  返回串右边的字符
          RTrim()                                  去掉串右边的空格
          Soundex()                                返回串的SOUNDEX值
          SubString()                              返回子串的字符
          Upper()                                  将串转换为大写
——————————————————————————————————————————————————————————————————————————
SOUNDEX是一个将任何文本串转换为描述其语音表示的字母数字模式算法。SOUNDEX考虑了类似发音字符和音节，使得对串进行发音比较而不是字母比较。
customers表中有一个顾客Coyote Inc.,其联系名为Y.Lee ,但是如果输入错误，此联系名实际应该是Y.Lie。输入后不会返回数据
输入：SELECT name,contact FROM customers WHERE contact='Y.Lie'
输出：+------------+-------------+
     |    name     |  contact    |
     +-------------|-------------+ 
     

用过Soundex()函数进行搜索，它匹配所有发音类似于Y.Lie的联系名：
输入：SELECT name,concat FROM customers WHERE Soundex(contact)=Soundex('Y.Lie');
输出：+------------+--------------+
     |    name     |  contact    |
     +-------------+-------------+ 
     | Coyote Inc. |   Y.Lee     |
     +--------------+------------+ 
```

- 9.1.2 日期和时间处理函数
```
输入：SELECT id,num FROM orders WHERE Date(date)='2005-09-01';
查询2005年9月下的所有订单
输入：SELECT id,num FROM orders WHERE Date(date) BETWEEN '2005-09-01' AND '2005-09-30';
或者：SELECT id,num FROM orders WHERE Year(date)=2005 AND Month(date)=9 ;(检索出2015年9月的所有行)
```

### 汇总数据
#### 10.1 聚集函数
```
运行在行组上，计算和返回单个值得函数
SQL聚集函数
——————————————————————————————————————————————————————————————————————————
           函数                                       说明
——————————————————————————————————————————————————————————————————————————
           AVG()                                   返回某列的平均值
           COUNT()                                 返回某列的行数
           MAX()                                   返回某列的最大值
           MIN()                                   返回某列的最小值
           SUM()                                   返回某列值之和
——————————————————————————————————————————————————————————————————————————
```
###  分组数据
#### 11.1 数据分组
```
输入：SELECT id,COUNT(*)  As num FROM products GROUP BY id;

输出：+------------+---------------+
     |      id     |     num      |
     +-------------+--------------+ 
     |     1001    |      3       |
     |     1002    |      2       |
     |     1003    |      7       |
     |     1005    |      2       |
     +-------------+--------------+ 
分析：GROUP BY 子句指示MySQL按id排序并分组数据
```

#### 11.2 过滤分组
```
目前为止所学过的所有类型的WHERE子句都可以用HAVING来代替。唯一的差别是WHERE过滤行，而HAVING过滤分组
WHERE在数据分组前进行过滤，HAVING在数据分组后进行过滤。
查询具有2个以上、价格为10以上的产品的供应商。
输入：SELECT id,COUNT(*) AS num FROM products WHERE price>=10 GROUP BY id HAVING COUNT(*)>=2; 
```

#### 11.3 分组和排序
```
查询总计订单价格大于等于50的订单的订单号和总计订单价格并按订单总价格排序输出：
输入：SELECT num,SUM(quantity * price) AS ordertotal 
     FROM orderitems 
     GROUP BY num 
     HAVING SUM(quantity * price)>=50 
     ORDER BY ordertotal；
```
##### 11.4 SELECT子句顺序
```
————————————————————————————————————————————————————————————————————————————————————————————
         子句                        说明                         是否必须使用
————————————————————————————————————————————————————————————————————————————————————————————
        SELECT                要返回的列或表达式                      是
        FROM                  从中检索数据的表                        仅在从表选择数据时使用
        WHERE                 行级过滤                               否
        GROUP                 分组说明                               仅在按组计算聚集时使用
        HAVING                组级过滤                               否
        ORDER BY              输出排序顺序                            否                           
        LIMIT                 要检索的行数                            否
————————————————————————————————————————————————————————————————————————————————————————————
```

### 使用子查询  
#### 12.1 利用子查询进行过滤
```
查询id为TNT2的所有订单物品。
输入：SELECT num FROM orderties WHERE id='TNT2';
输出 +------------+
     |    num     |
     |------------|
     |   20005    |
     |   20007    |           
     +------------+
     
 查询具有订单20005和20007的客户id.
 输入：SELECT cust_id FROM orders WHERE order_num IN (20005,20007);
 输出+------------+
     |   cust_id  |
     |------------|
     |   10001    |
     |   10004    |           
     +------------+
     
 查询具有id为TNT2的所有订单物品的客户ID信息
 输入： SELECT cust_id FROM  orders WHERE num IN (SELECT num FROM orders WHERE id='TNT2');
 输出 +------------+
      |   cust_id  |
      |------------|
      |   10001    |
      |   10004    |           
      +------------+

 查询订购物品为TNT2的所有客户id所对应的客户信息。
 输入：SELECT cust_name,cust_contact FROM orders WHERE cust_id IN 
 (SELECT cust_id FROM orders WHERE cust_num IN (SELECT cust_num FROM orderitems WHERE id='TNT2'));
 输出 +--------------+--------------+
      |  cust_name   | cust_contact |
      +--------------+--------------+ 
      | Coyote Inc.  |   Y Lee      |
      +--------------+--------------+
      |Yosemite Place|   Y Sam      |
      +--------------+--------------+ 
```

#### 12.2 作为计算字段使用子查询
```
查询customers表中每个客户的订单总数。订单与相应的客户ID存储在orders表中
输入：SELECT cust_name,cust_state,(SELECT count( * )  FROM  orders WHERE orders.cust_id=customers.cust_id) total_order 
     FROM customers ORDER BY cust_name; 
 输出 +--------------+--------------+--------------+
      |  cust_name   | cust_state   | total_order  |
      +--------------+--------------+--------------+ 
      | Coyote Inc.  |      MI      |      2       |
      | E Fudd       |      IL      |      1       |
      | Mouse House  |      OH      |      0       |   
      | Wascals      |      IN      |      1       |
      |Yosemite Place|      AZ      |      1       |
      +--------------+--------------+--------------+
```

### 联结表
#### 13.1 联结单个表
- 13.1.1 笛卡尔积
- 由没有联结条件的表关系返回的结果为笛卡尔积，检索出的行的数目将是第一个表中的行数乘以第二个表中的行数
  
- 13.1.2 内部联结
- 目前为止所用的联结称为等值联结，它基于两个表之间的相等测试。这种联结也称为内部联结。
```
输入：SELECT vend_name,prod_name,prod_price FROM vendors INNER JOIN products ON vendors.vend_id=products.vend_id;
```

#### 13.2 联结多个表
```
 查询编号为20005的订单中的物品的prod_name,vend_name,prod_price,quantity
 输入：SELECT prod_name,vend_name,prod_price, quantity 
       FROM  ordertiems,products,vendors 
       WHERE prodcuts.vend_id=vendors.vend_id 
       AND orderitems.prod_id=products.prod_id 
       AND order_num=20005;
```
### 创建高级联结
#### 14.1 自联结
```
普通子查询：
查询ID为DINTR的物品的供应商生产的其他物品
 输入：SELECT prod_id,prod_name FROM products WHERE vend_id=(SELECT vend_id FROM products WHERE prod_id='DINTR');
 自联结查询：
 输入：SELECT p1.prod_id,p1.prod_name FROM products p1,products p2 WHERE p1.vend_id=p2.vend_id AND p2.prod_id='DTNTR';
```

#### 14.2 外部联结

```
为了检索所有客户，包括那些没有订单的客户。
SELECT customers.cust_id,orders.order_num FROM customers LEFT OUTER JOIN orders ON customers.cust_id=orders.cust_id;
在使用OUTER  JOIN语法时，必须使用RIGHT或者LEFT关键字指定包括其所有行的表(RIGHT指出的是OUTER JOIN 右边的表，而LEFT指出的是OUTER JOIN左边的表)
```

### 组合查询
####  15.1 组合查询
- MySQL也允许执行多个查询，并将结果作为单个查询结果集返回。这些组合查询通常称为并或复合查询
- 有两种基本情况，其中需要使用组合查询：
1. 在单个查询中从不同的表返回类似结构的数据；
2. 对单个表执行多个查询，按单个查询返回数据。

#### 15.2 创建组合查询
```
可用UNION操作符来组合数条SQL查询。利用UNION，可给出多条SELECT语句，将他们的结果组合成单个结果集。
需要查询价格小于等于5的所有物品的一个列表，而且还想包括供应商1001和1002生产的所有物品(不考虑价格)
WHERE 语句
输入：SELECT vend_id，prod_id,prod_price FROM products WHERE prod_price <= 5 OR vend_id IN (1001,1002);
UNION规则
UNION必须由两条或两条以上的SELECT语句组成，语句之间用关键字UNION分隔。
UNION中的每个查询必须包含相同的列、表达式或聚集函数
列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以隐含地转换的类型
输入：SELECT vend_id, prod_id, prod_price FROM products WHERE prod_price <=5 
     UNION 
     SELECT vend_id, prod_id, prod_price FROM products WHERE vend_id IN （1001，1002）；
```

#### 15.3 包含或取消重复的行
- UNION从查询结果集中自动去除了重复的行。这是UNION的默认行为。使用UNION ALL，MySQL不取消重复的行，

#### 15.4 对组合查询结果排序
- SELCT 语句的输出用ORDER BY子句排序。在用UNION组合查询时，只能使用一条ORDER BY子句，它必须出现在最后一条SELECT 语句之后。


### 插入数据
#### 16.1 插入完整的行 

```
把数据插入表中的最简单方法是使用基本的INSERT语法，它要求指定表名和被插入到新行中的值。
输入：INSERT INTO customers VALUES（'zhangsan','男'，20）;
INSERT 语句一般不会产生输出
```



#### 16.2 插入多个行
```
只要每条INSERT语句中的列名相同，可如下：
输入：INSERT INTO customers（cust_name,cust_address,cust_city,cust_zip,cust_country) 
VALUES ('Ped','100 Main Street','Los Angelas'，'CA', '90046', 'USA'),
('Martian','42 Galaxy Street','New York'，'NY', '11213', 'USA');
输入：INSERT INTO customers（cust_name,cust_address,cust_city,cust_zip,cust_country) VALUES ('Ped','100 Main Street','Los Angelas'，'CA', '90046', 'USA'), ('Martian','42 Galaxy Street','New York'，'NY', '11213', 'USA');
其中单条INSERT语句有多组值，每组值用一对圆括号括起来，用逗号分隔。
```

### 更新和删除数据
#### 17.1 更新数据
- 为了更新（修改）表中的数据，可使用UPDATE语句。可采用两种方式使用过UPDATE：
1. 更新表中特定行。
2. 更新表中所有行。
- 基本的UPDATE语句由三部分组成，分别是：
1. 要更新的表；
2. 列名和他们的新值；
3. 确定要更新行的过滤条件
```
查询客户10005现在有了电子邮件地址，因此他的记录需要更新
输入：UPDATE customers SET cust_email= 'elmer@fudd.com'
     WHERE cust_id=10005;
 UPDATE语句总是以要更新的表的名字开始。在此例子中，要更新的表的名字为customers。SET命令用来将新值赋给被更新的列。
 UPDATE语句以WHERE子句结束，它告诉MySQL更新哪一行。没有WHERE子句，MySQL将会更新customers表中所有行。
 
 更新多个列：
 输入：UPDATE customers SET cust_name='The Fudds', cust_email='elmer@fudd.com' WHERE cust_id=10005;
 在更新多个列时，只需要使用单个SET命令，每个“列=值”对之间用逗号分隔。
```

#### 17.2 删除数据
 - 为了从一个表中删除数据，使用DELETE语句。可以使用两种方式
 1. 从表中删除特定的行，
 2. 从表中删除所有行。
 - 不要省略WHERE子句，否则就会删除表中所有的行。
```
 输入：从customers表中删除一行：DELETE FROM customers WHERE cust_id=10006;
 DELETE 不需要列名或通配符。DELETE删除整行而不是整列。为了删除指定的列，使用UPDATE语句。
```
- 删除表的内容还不是表：DELETE语句从表中删除行，甚至是删除表中所有行，但是，DELETE 不删除表本身。
 

### 创建和操作表
#### 18.1 利用CREATE TABLE 创建表：
 - 新表的名字，在关键字CREATE TABLE之后给出；
 - 表列的名字个定义，用逗号分隔.
```
 用MySQL语句创建customers表：
 CREATE TABLE customers
 (
    cust_id      int       NOT NULL AUTO_INCREMENT,
    cust_name    char(50)  NOT NULL,
    cust_address char(50)  NULL,
    cust_country char(50)  NULL,
    cust_email   char(255) NULL,
    PRIMARY KEY (cust_id)
 ）
```


#### 18.2 更新表
- 为更新表定义，可使用ALTER TABLE语句。在理想状态下，当表中存储数据之后，该表就不应该再被更新。
```
输入: ALTER TABLE venders ADD vend_phone CHAR(20);
删除刚刚添加的列，可以这样做：
输入：ALTER TABLE vendors DROP COLUMN vend_phone;
```

#### 18.3 删除表


- 使用DROP TABLE语句即可
```
输入： DROP TABLE custmoers;
```

#### 18.4 重命名表
- 使用RENAME TABLE 语句可以重命名一个表：


```
输入：RENAME TABLE customer2 TO customers;
```

### 使用视图
#### 19.1 视图
- 视图是虚拟的表，与包含数据的表不一样，视图只包含使用时动态检索数据的查询。

#### 19.2 视图的规则和限制
- 与表一样，视图必须唯一命名（不能给视图取与别的视图或表相同的名字）
- 对于可以创建的视图数目没有限制
- 为了创建视图，必须具有足够的访问权限。这些限制通常由数据库管理人员授予。
- 视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造一个视图。
- ORDER BY 可以用在视图中，但如果从该视图检索数据SELECT中也含有ORDER BY,那么该视图中的ORDER BY将被覆盖。
- 视图不能索引，也不能有关联的触发器或默认值。
- 视图可以和表一起使用。例如：编写一条联结表和视图的SELECT语句。

#### 19.3 使用视图
- 视图用CREATE VIEW 语句来创建
- 使用SHOW CREATE VIEW viewname;来查看创建视图的语句。
- 用DROP删除视图，其语法为DROP VIEW viewname;
- 更新视图时，可以先用DROP再用CREATE，也可以直接用CREATE OR REPLACE VIEW 。如果要更新的视图不存在，则第二条更新语句会创建一个视图；如果存在，则会替换原来的视图。

#### 19.4 利用视图简化复杂的联结
```
输入：CREATE VIEW productcustomers AS 
     SELECT cust_name,cust_contact,prod_id FROM customers,orders,orderitems 
     WHERE customers.cust_id=orders.cust_id 
     AND orderitems.order_num=orders.order_num;
这条语句创建一个名为productcustomers的视图，它联结三个表，以返回已订购了任意产品的所有客户的列表。如果执行了
SELECT * FROM productcustomers，将列出订购了任意产品的客户。
```
#### 19.5 用视图重新格式化检索出的数据
```
输入：SELECT Concat(name, '(',country,')') AS vend_title FROM verdors ORDER BY name;
输出  +----------------------------------+
      |            vend_title           |
      |---------------------------------|
      | ACME （USA)                     |
      | Anvils  R Us (USA)              |
      | Furball Inc. (USA)              |
      | Jet Set (England)               |
      | Jouets Et Ours (France)         |
      +---------------------------------+
  现在，假如经常需要这个格式的结果。不必在每次需要时执行联结，创建一个视图，每次需要时使用它即可。将此语句转化成视图，可按下进行：
  输入：CREATE VIEW vendorlocations AS 
        SELECT Concat(name, '(',country,')') AS vend_title FROM verdors ORDER BY name;
  分析：这条语句使用与以前的SELECT语句相同的查询创建视图。为了检索出以创建所有邮件标签的数据，可如下进行：
  输入：SELECT * FROM vendorlocations；
  输出：同上。
 ```
 
 #### 19.6 用视图过滤不想要的数据
 - 视图对于应用普通的WHERE子句也很有用。例如，可以定义customeremaillist视图，它过滤没有电子邮件地址的客户。
 ```
 输入：CREATE VIEW customeremaillist AS SELECT cust_id,cust_name,cust_email FROM customers WHERE cust_email IS NOT NULL;
 分析：显然，在发送邮件到邮件列表时，需要排除没有电子邮件地址的用户。这里的WHERE子句过滤了cust_email列中具有NULL值的那些行，使他们不被检索出来
 输入：SELECT * FROM customeremaillist ；
 ```
#### 19.7 使用视图与计算字段
```
检索某个特定订单中的物品，计算每种物品的总价格：
输入：SELECT prod_id,quantity,item_price,quantity * item_price AS expanded_price WHERE order_num=20005;
输出：+--------+----------+---------+-----------------+
      |   id   | quantity |  price  | expanded_price |
      |--------+----------+---------+----------------+
      |  ANV01 |    10    |   5.99  |     59.90      |
      |  ANV02 |     3    |   9.99  |     29.97      |
      |  TNT2  |     5    |  10.00  |     50.00      |
      |   FB   |     1    |  10.00  |     10.00      |
      +--------+----------+---------+----------------+      
为将其转换成一个视图，如下进行：
输入：CREATE VIEW orderitemsexpanded AS 
      SELECT order_num,prod_id,quantity,item_price,quantity * item_price AS expanded_price FROM orderitems;
      
为检索订单20005的详细内容（上面的输出），如下进行：
输入：SELECT * FROM orderitemsexpanded WHERE order_num=20005;
输出：+-------------+---------+-----------+---------+----------------+
      | order_num   |   id    | quantity |  price  | expanded_price |
      +-------------|-------- +----------+---------+----------------+
      |    20005    |  ANV01  |    10    |   5.99  |     59.90      |
      |    20005    |  ANV02  |     3    |   9.99  |     29.97      |
      |    20005    |  TNT2   |     5    |  10.00  |     50.00      |
      |    20005    |   FB    |     1    |  10.00  |     10.00      |
      +-------------+---------+----------+---------+----------------+  
```
#### 19.8 更新视图
- 分组：（使用GROUP BY和HAVING）
- 联结；
- 子查询；
- 并；
- 聚集函数（Min()、Count()、Sum()等);
- DISTINCT;
- 导出(计算) 列。

### 管理事务处理
#### 20.1 事务处理
- 事务处理可以用来维护数据库的完整性，它保证成批的MySQL操作要么完全执行，要么完全不执行。
- 在使用事务和事务处理时，下面是关于事务处理需要知道的几个术语：
1. 事务（transaction）指一组SQL语句；
2. 回退（rollback）指撤销指定SQL语句的过程；
3. 提交（commit）指将未存储的SQL语句结果写入数据库表；
4. 保留点（savepoint）指事务处理中设置的临时占位符，你可以对他发布回退（与回退整个事务处理不同）。
#### 20.2 控制事务处理
- 管理事务处理的关键在于将SQL语句组分解为逻辑块，并明确规定数据何时应该回退，何时不应该回退。
- MySQL使用下面的语句来标识事务的开始：
```
输入：START TRANSATION
```
#### 20.2.1 使用ROLLBACK
- MySQL的ROLLBACK命令用来回退（撤销）MySQL语句：
```
输入：SELECT * FROM ordertotals;
     START TRANSATION;
     DELETE FROM ordertotals;
     SELECT * FROM ordertotals;
     ROLLBACK;
     SELECT * FROM ordertotals;
分析：这个例子从显示ordertotals表的内容开始。首先执行一条SELECT以显示该表不为空。然后开始一个事务处理，用一条DELETE语句删除ordertotals表中的所有行，另一条SELECT语句验证ordertotals确实为空，这时用一条ROLLBACK语句回退START TRANSATION 之后的所有语句，最后一条SELECT语句显示该表不为空。
显然，ROLLBACK只能在一个事务处理内使用（在执行一条START TRANSATION命令之后。
   
- 哪些语句可以回退？
事务处理用来管理INSERT、UPDATE和DELETE，不能回退SELECT语句。不能回退CREATE或DROP操作。事务处理块中可以使用这两条语句，但如果你执行回退，它们不会被撤。
```
#### 20.2.2 使用COMMIT
- 一般的MySQL语句都是直接针对数据库表执行和编写的，这就是所谓的隐含提交，即提交（写或保存）操作是自动进行的。但是在事务处理块中，提交不会隐含的进行。为进行明确的提交，使用commit语句，如下所示：
```
输入：START TRANSACTION；
      DELETE FROM orderitems WHERE order_num=20010;
      DELETE FROM orders WHERE order_num=20010;
      COMMIT;
分析：在这个例子中，从系统中完全删除订单20010.因为涉及更新两个数据库表orders和orderItems，所以使用事务处理块来保证订单不被部分删除。最后的COMMIT语句仅仅在不出错时写出更改。如果第一条DELETE起作用，但第二条失败，则DELETE不会提交（实际上，它是被自动撤销的）
* 隐含事务关闭  当COMMIT 或ROLLBACK 语句执行后，事务会自动关闭

      
