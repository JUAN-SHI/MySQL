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

