# sql指令



## MySql服务

- 启动服务

  ```shell
  net start mysql
  ```

- 关闭服务

  ```shell
  net stop mysql
  ```

- 账号登入

  ```shell
  mysql -u root -p
  ```



## 库操作

### 创建库

```mysql
create DATABASE 'database_name';
```

### 删除库

```sql
drop database 'database_name';
```

### 查看所有库

```mysql
show database;
```

### 使用库

```mysql
use 'database_name';
```



## 表操作

### 创建表

``` mysql
create TABLE 'table_name' (column_name,column_type);
```

- column_type

  | 值                     | 描述              |
  | ---------------------- | ----------------- |
  | INT、VARCHAR(number)等 | 字段类型          |
  | AUTO_INCREMENT         | 自增，值会自动加1 |
  | PRIMARY KEY            | 自定义主键删除表  |

### 删除表

```mysql
drop TABLE 'table_name' ;
```

### 修改表

```mysql
// 添加新列
ALTER TABLE 'table_name' ADD newFieldName newFieldType;
// 删除某一列
ALTER TABLE 'table_name' DROP field;

// 修改字段类型
ALTER TABLE 'table_name' MODIFY fieldName newFieldType;
// 修改字段名称
ALTER TABLE 'table_name' CHANGE fieldName newFieldName newFieldType;
// 修改字段的默认值
ALTER TABLE 'table_name' ALTER fieldName SET DEFAULT defaultValue;
// 修改表名
ALTER TABLE 'table_name' RENAME TO 'new_table_name';
// 修改存储引擎
ALTER TABLE 'table_name' engine= 'newEngine';
// 删除外键约束
ALTER TABLE 'table_name' DROP FOREIGN KEY keyName;
```

### 查看所有表

```mysql
show tables;
```

### 查看表结构

```mysql
decrible 'table_name';
```



## 数据操作

### 增加数据

```mysql
INSERT INTO 'table_name' ( field1, field2,...fieldN )
                  		 VALUES
                         ( value1, value2,...valueN );
```

### 删除

```mysql
DELETE FROM 'table_name' [WHERE Clause];
```

### 修改数据

```mysql
UPDATE 'table_name' SET field1=new-value1, field2=new-value2
[WHERE Clause];
```

### 查询数据

```mysql
SELECT column_name,column_name
FROM 'table_name'
[WHERE Clause]
[LIMIT N]
[OFFSET M];
```

- WHERE

  带查询条件

- LIMIT

  设定返回的条数

- OFFSET

  设定查询的距离0的偏移量



## SQL子句

### WHERE子句

where子句是一个过滤子句,你可以使用 ``AND``或者 ``OR`` 指定一个或多个条件。

```mysql
SELECT * FROM 'table_name' WHERE (field1 = 1 AND field2 = 2)
```

| 操作符 | 描述                                                         | 实例                 |
| :----- | :----------------------------------------------------------- | :------------------- |
| =      | 等号，检测两个值是否相等，如果相等返回true                   | (A = B) 返回false。  |
| <>, != | 不等于，检测两个值是否相等，如果不相等返回true               | (A != B) 返回 true。 |
| >      | 大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true | (A > B) 返回false。  |
| <      | 小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true | (A < B) 返回 true。  |
| >=     | 大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true | (A >= B) 返回false。 |
| <=     | 小于等于号，检测左边的值是否小于或等于右边的值, 如果左边的值小于或等于右边的值返回true | (A <= B) 返回 true。 |

### LIKE子句

like子句用来模糊搜索,你可以使用 ``AND`` 或者 ``OR``指定一个或多个条件。用``%``来替代模糊文字。

```mysql
SELECT * FROM 'table_name' WHERE field1 LIKE '%kevin%'
```

### ORDER BY子句

order by子句用来对查询结果进行排序。

```mysql
SELECT * FROM 'table_name' ORDER BY field1[ASC]
```

- ``ASC``升序
- ``DESC``降序

### GROUP BY子句

group by子句用来对结果进行分组。在分组上可以使用函数。

```mysql
SELECT column_name, function(column_name)
FROM 'table_name'
WHERE column_name operator value
GROUP BY column_name;
```

- 函数 ``COUNT()``,``SUM``，``AVG``

## 高阶用法

### 主键

- 主键

  ```mysql
  CREATE TABLE 'table_name'
  (
     id INT NOT NULL AUTO_INCREMENT,
     PRIMARY KEY (id)
  );
  ```

- 联合主键

  ```mysql
  CREATE TABLE 'table_name'
  (
     id INT NOT NULL AUTO_INCREMENT,
     id2 INT NOT NULL AUTO_INCREMENT,
     PRIMARY KEY (id,id2)
  );
  ```

### 联表

- INNER  JOIN 内连接

  inner join内连接会通过``on``条件取交集。

  ```mysql
  SELECT table1.filed1,table2.filed3 FROM 'table1_name' table1 INNER JOIN 'table2_name' table2 on table1.field1 = table2.field1 
  ```

- LEFT JOIN 左连接

  left join左连接会读取所有左表数据。

  ```mysql
  SELECT table1.runoob_id, table1.runoob_author, table2.runoob_count FROM 'table1_name' table1 LEFT JOIN 'table2_name' table2 ON table1.author = table2.author;
  ```

- RIGHT JOIN 右连接

  right join右连接会读取所有右表数据。

  ```mysql
  SELECT table1.runoob_id, table1.runoob_author, table2.runoob_count FROM 'table1_name' table1 RIGHT JOIN 'table2_name' table2 ON table1.author = table2.author;
  ```



### 运算符

#### NULL值相关

| IS NULL     | 当列的值是 NULL,此运算符返回 true             |
| ----------- | --------------------------------------------- |
| IS NOT NULL | 当列的值不为 NULL, 运算符返回 true            |
| <=>         | 当比较的的两个值相等或者都为 NULL 时返回 true |



### 索引

#### 创建索引

- 普通索引

  ```mysql
  // 创建索引
  CREATE INDEX 'index_name' ON 'table_name' (column_name)
  
  // 插入索引
  ALTER table 'table_name' ADD INDEX indexName(column_name)
  ```

- 唯一索引

  ```mysql
  // 创建索引
  CREATE UNIQUE INDEX 'index_name' ON 'table_name' (column_name)
  
  // 插入索引
  ALTER table 'table_name' ADD UNIQUE INDEX indexName(column_name)
  ```
  

#### 查看索引

```mysql
SHOW INDEX FROM 'table_name'\G
```

#### 删除索引

```mysql
DROP INDEX [indexName] ON 'table_name'; 
```



### 序列

#### 创建自增序列

```mysql
CREATE TABLE 'table_name' INSERT ( id INT NOT NULL AUTO_INCREMENT)
```

#### 设置序列开始值

```mysql
CREATE TABLE 'table_name' INSERT ( id INT NOT NULL AUTO_INCREMENT)
auto_increment = 'startValue'
```
