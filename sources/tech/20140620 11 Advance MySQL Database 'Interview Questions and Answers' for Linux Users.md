translating by johnhoow...
给linux用户的11个高级MySQL数据库面试问题和答案
================================================================================
我们已经发表了两篇MySQL的文章,非常感谢Tecmint社区的大力支持.这是MySQL面试系列的第三篇文章,并且在面试专栏中排第16.
- [15个基本的MySQL面试问题][1]
- [给中级人员的10个MySQL面试问题][1]

![11个MySQL面试问题](http://www.tecmint.com/wp-content/uploads/2014/06/MySQL-Interview-Questions.jpeg)

感谢你们这一路上对我们的支持.这篇文章主要针对MySQL的实用性,讲面试方面的问题.

### 1. 如何使用SELECT语句找到你正在运行的服务器的版本并打印出当前数据库的名称? ###
**答案**:下面的语句的结果会显示服务器的版本和当前的数据库名称 

     mysql> SELECT VERSION(), DATABASE();
     
     +-------------------------+------------+
     | VERSION()               | DATABASE() |
     +-------------------------+------------+
     | 5.5.34-0ubuntu0.13.10.1 | NULL       |
     +-------------------------+------------+
     1 row in set (0.06 sec)

在Database一栏中显示**NULL**是因为我们当前没有选择任何数据库.因此,使用下面的语句先选择一个数据库,就能看到相应的结果.

    mysql> use Tecmint;
    
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A
    
    Database changed

----------

    mysql> select VERSION(), DATABASE();
    
    +-------------------------+------------+
    | VERSION()               | DATABASE() |
    +-------------------------+------------+
    | 5.5.34-0ubuntu0.13.10.1 | tecmint    |
    +-------------------------+------------+
    1 row in set (0.00 sec)

### 2. 使用非运算符(!)从表"Tecmint"中列出除了user等于"SAM"的所有记录

**答案**:使用下面的语句 

    mysql> SELECT * FROM Tecmint WHERE user !=SAM;
    
    +---------------------+---------+---------+---------+---------+-------+ 
    | date                | user    | host 	 | root     | local   | size  | 
    +---------------------+---------+---------+---------+---------+-------+ 
    | 2001-05-14 14:42:21 | Anthony | venus  | barb     | venus   | 98151 | 
    | 2001-05-15 08:50:57 | TIM     | venus  | phil     | venus   | 978   | 
    +---------------------+---------+---------+---------+---------+-------+

### 3. 是否能够使用非运算符(!)来实现'AND'运算

**答案**: The AND operator is used when we use (=) and the operator OR is used when we use (!=). An example of (=) with AND Operator.
**答案**: 

    mysql> SELECT * FROM mail WHERE user = SAM AND root = phil

An Example of (!=) with OR Operator.

    mysql> SELECT * FROM mail WHERE user != SAM OR root != phil
    
    +---------------------+---------+---------+---------+---------+-------+ 
    | date                | user    | host    | root    | local   | size  | 
    +---------------------+---------+---------+---------+---------+-------+ 
    | 2001-05-14 14:42:21 | Anthony | venus   | barb    | venus   | 98151 | 
    +---------------------+---------+---------+---------+---------+-------+

- = : means Equal to
- != : Not Equal to
- ! : represents NOT Operator

The AND & OR are treated as joining operators in MySQL.

### 4. What IFNULL() statement is used for in MySQL? ###

**Ans**: The Query in MySQL can be written precisely using **IFNULL()** statement. The IFNULL() statement test its first argument and returns if it’s not NULL, or returns its second argument, otherwise.

    mysql> SELECT name, IFNULL(id,'Unknown') AS 'id' FROM taxpayer;
    
    +---------+---------+ 
    | name 	  | id      | 
    +---------+---------+ 
    | bernina | 198-48  | 
    | bertha  | Unknown | 
    | ben     | Unknown | 
    | bill    | 475-83  | 
    +---------+---------+

### 5. You want to see only certain rows from a result set from the beginning or end of a result set. How will you do it? ###

**Ans**: We need to use **LIMIT** clause along with ORDER BY to achieve the above described scenario.

#### Show 1 Record ####

    mysql> SELECT * FROM name LIMIT 1;
    
    +----+------+------------+-------+----------------------+------+ 
    | id | name | birth      | color | foods                | cats | 
    +----+------+------------+-------+----------------------+------+ 
    | 1  | Fred | 1970-04-13 | black | lutefisk,fadge,pizza | 0    | 
    +----+------+------------+-------+----------------------+------+

#### Show 5 Record ####

    mysql> SELECT * FROM profile LIMIT 5;
    
    +----+------+------------+-------+-----------------------+------+ 
    | id | name | birth      | color | foods                 | cats | 
    +----+------+------------+-------+-----------------------+------+ 
    | 1  | Fred | 1970-04-13 | black | lutefisk,fadge,pizza  | 0    | 
    | 2  | Mort | 1969-09-30 | white | burrito,curry,eggroll | 3    | 
    | 3  | Brit | 1957-12-01 | red   | burrito,curry,pizza   | 1    |   
    | 4  | Carl | 1973-11-02 | red   | eggroll,pizza         | 4    | 
    | 5  | Sean | 1963-07-04 | blue  | burrito,curry         | 5    | 
    +----+------+------------+-------+-----------------------+------+

----------

    mysql> SELECT * FROM profile ORDER BY birth LIMIT 1;
    
    +----+------+------------+-------+----------------+------+ 
    | id | name | birth      | color | foods          | cats | 
    +----+------+------------+-------+----------------+------+ 
    | 9  | Dick | 1952-08-20 | green | lutefisk,fadge | 0    | 
    +----+------+------------+-------+----------------+------+

### 6. Oracle Vs MySQL. Which one and Why? ###

**Ans**: Well both has its advantages and disadvantages. As a matter of time I prefer MySQL.

#### Reason for Selection MySQL Over oracle ####

- Mysql is FOSS.
- MySQL is portable.
- MYSQL supports both GUI as well as Command Prompt.
- MySQL Administration is supported over Query Browser.

### 7. How will you get current date in MySQL? ###

**Ans**: Getting current date in MySQL is as simple as executing the below SELECT Statement.

    mysql> SELECT CURRENT_DATE();
    
    +----------------+
    | CURRENT_DATE() |
    +----------------+
    | 2014-06-17     |
    +----------------+

### 8. How will you export tables as an XML file in MySQL? ###

**Ans**: We use ‘-e‘ (export) option to export MySQL table or the whole database into an XML file. With large tables we may need to implement it manually but for small tables, applications like phpMyAdmin can do the job.
A native command of MySQL can do it.

    mysql -u USER_NAME –xml -e 'SELECT * FROM table_name' > table_name.xml

Where USER_NAME is username of Database, table_name is the table we are exporting to XML and table_name.xml is the xml file where data is stored.

### 9. What is MySQL_pconnect? And how it differs from MySQL_connect? ###

**Ans**: MySQL_pconnect() opens a connection that is persistent to the MySQL Database which simply means that the database is not opened every-time the page loads and hence we can not use MySQL_close() to close a persistent connection.

A brief difference between MySQL_pconnect and MySQL_connect are.

Unlike MySQL_pconnect, MySQL_connect – Opens the Database every-time the page is loaded which can be closed any-time using statement MySQL_close().

### 10. You need to show all the indexes defined in a table say ‘user’ of Database say ‘mysql’. How will you achieve this? ###

**Ans**: The following command will show all the indexes of a table ‘user’.

    mysql> show index from user;
    +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+ 
    | Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | 
    +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+ 
    | user  |          0 | PRIMARY  |            1 | Host        | A         |        NULL |     NULL | NULL   |      | BTREE      |         |               | 
    | user  |          0 | PRIMARY  |            2 | User        | A         |           4 |     NULL | NULL   |      | BTREE      |         |               | 
    +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+ 
    2 rows in set (0.00 sec)

### 11. What are CSV tables? ###

**Ans**: CSV stands for Comma-Separated Values aka Character-Separated Values. CSV table stores data in plain text and tabular format. It typically contains one record per line.

Each record is separated by specific delimiters (Comma, Semi-colon, …) where each record has same sequence of field. CSV tables are most widely used to store phone contacts to Import and Export and can be used to store any sort of plain text data.

That’s all for now. I’ll be here again with another Interesting article, you people will love to read. Till then stay tuned and connected to Tecmint and Don’t forget to provide us with your valuable feedback in the comment section below.

--------------------------------------------------------------------------------

via: http://www.tecmint.com/mysql-advance-interview-questions/

译者：[译者ID](https://github.com/译者ID) 校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创翻译，[Linux中国](http://linux.cn/) 荣誉推出

[1]:http://www.tecmint.com/basic-mysql-interview-questions-for-database-administrators/
[2]:http://www.tecmint.com/10-mysql-database-interview-questions-for-beginners-and-intermediates/
