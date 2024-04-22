
## SQL

SQL语言分为四大类：

### 数据定义语言DDL

***维护结构***

主要用于**维护存储数据的结构**，如 数据库 、表、索引等
代表指令
- create
- drop
- alter

### 数据操纵语言 DML

***维护数据***

主要用于对数据库对象中包含的数据进行操作
代表指令
- insert
- delete
- update




### 数据查询语言DQL

***查询数据***

查询数据库中数据
代表指令
- select
- from
- where


### 数据控制语言DCL

***控制 权限***

控制数据库对象的权限管理、事物和实时监控
代表指令
- grant
- revoke
- rollback
- commit


## DATABASE


### 创建数据库
```shell
CREATE DATABASE [IF NOT EXISTS]dbname
[CHARACTER SET charaset_name]
[COLLATE collation_name];
```

### 显示数据库

```shell
SHOW DATABASE;
```
### 删除数据库

```shell
DROP DATABASE [IF EXISTS] dbname;
```

### 选择数据库
进入该数据库后，后续的SQL查询和操作均在指定的数据库上进行
```shell
USE dbname;
```
## TABLE

### 创建数据表

创建数据表需要以下信息：
- 表名
- 表字段名
- 定义每个表字段的数据类型

```shell
CREATE TABLE tbname(
col_name1 datatype,
col_name2 datatype,
...
);


```

增加了约束的表
```shell
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    birthdate DATE,
    is_active BOOLEAN DEFAULT TRUE
);
```
- AUTO_INCREMENT：自增
- PRIMARY KEY ：主键约束
- NOT NULL：非空约束
- DEFAULT TRUE：默认值

```shell
create table tea(id int auto_increment primary key,
					name varchar(20) not null,
                expire date not null)character set utf8mb4;
```

```shell
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
-  PRIMARY KEY ( `runoob_id` ) 可以使用多列定义主键，用`,`隔开


### 删除数据表

```shell
DROP DATATABLE [IF EXIST] tbname;
```

### 显示数据表

##### 显示当前database下所有数据表
```shell
SHOW TABLES;
```

#### 显示某个数据表的列
```shell
SHOW COLUMNS FROM tbname;
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240408113659.png)




### 插入数据

```shell
INSERT tbname (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```
```shell
insert INTO tea(id,name,expire)values(1,"John","2025-10-08");
insert into tea(name,expire)value("Tom",'2025-10-26');
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240408114500.png)
不在insert语句中显式地插入自增字段也可以

如果想插入所有列的数据
```shell
INSERT INTO users
VALUES (NULL,'test', 'test@runoob.com', '1990-01-01', true);
```
这里的null是自增字段




### 更新数据

```shell
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```
需要指定更新的表名，列，以及更新条件

```shell
UPDATE tea
SET name='CAT'
where id=1;
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240408114815.png)
可以更新多列，根据多个条件去更新

```shell
UPDATE products
SET price = price * 1.1
WHERE category = 'Electronics';
```

```shell
UPDATE customers
SET total_purchases = (
    SELECT SUM(amount)
    FROM orders
    WHERE orders.customer_id = customers.customer_id
)
WHERE customer_type = 'Premium';
```

### 删除数据

```shell
DELETE FROM table_name
WHERE condition;
```
删除表中所有数据
```shell
DELETE FROM tbname;
```

### 修改字段

#### 添加列

```shell
ALTER TABLE tbname
ADD COLUMN new_column_name datatype;
```


#### 修改列数据类型

```shell
ALTER TABLE tbname
MODIFY COLUMN column_name new_datatype;
```

#### 修改列名

```shell
ALTER TABLE table_name
CHANGE COLUMN old_column_name new_column_name datatype;
```

#### 删除列(字段)

```shell
ALTER TABLE table_name
DROP COLUMN column_name;
```
如果数据表中只剩余一个字段则无法使用DROP来删除字段

#### 添加约束

添加主键约束
```shell
ALTER TABLE table_name
ADD PRIMARY KEY (column_name);
```
添加外键约束
```shell
ALTER TABLE child_table
ADD CONSTRAINT fk_name
FOREIGN KEY (column_name)
REFERENCES parent_table (column_name);
```

#### 修改表名

```shell
ALTER TABLE old_table_name
RENAME TO new_table_name;
```


### 导出数据


```shell
SELECT column1, column2, ...
INTO OUTFILE 'file_path'
FROM your_table
WHERE your_conditions;
```

```shell
SELECT * INTO OUTFILE '/tmp/result.text'
FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
FROM tea;

```

#### 导出整个数据库(bash环境)

不能在表里面执行，在cli下执行
```shell
mysqldump -u root -p dbname > dbname.sql
```

#### 导出特定表(bash环境)

```shell
mysqldump -u root -p dbname tbname > mytable_backup.sql
```

#### 只导出格式(bash环境)

`--no-data`
```shell
mysqldump -u username -p password --no-data database_name > output_file.sql
```

### 导入数据

搞不懂，以后再研究

#### 重定向导入(bash)

```shell
mysql -u root -p dbname < dbname.sql
```



#### mysqlimport(bash)

从文件 dump.txt 中将数据导入到 mytbl 数据表中
```shell
 $ mysqlimport -u root -p --local mytbl dump.txt

```



#### source(sql)
```shell
mysql> create database abc;      # 创建数据库
mysql> use abc;                  # 使用已创建的数据库 
mysql> set names utf8;           # 设置编码
mysql> source /home/abc/abc.sql  # 导入备份数据库
```

#### LOAD DATA(sql)
```shell
mysql> LOAD DATA LOCAL INFILE 'dump.txt' INTO TABLE mytbl
  -> FIELDS TERMINATED BY ':'
  -> LINES TERMINATED BY '\r\n';

```