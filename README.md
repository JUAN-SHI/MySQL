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
- 2.1 SELECT语句
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
数据的排序不限于


