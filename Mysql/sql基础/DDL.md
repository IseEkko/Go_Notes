# DDL 数据定义语言

对于这一部分就是对数据库和数据表的一些操作。

## 具体的操作

### 对数据库的操作

#### 查询数据库

对数据库的查询,查询所有的数据库

```sql
SHOW DATABASES;
```

查询当前数据库

```sql
SELECT DATABASE;
```

#### 创建数据库

创建数据库，这里括号里面的是选择填写的数据，在sql语句中[if not exists]表示的是，如果没有这个数据库就进行创建，如果有也不会报错。

```sql
CREATE DATABASE [if not exists] 数据库名称 [default charset 字符集] [collate 排序规则];
```

#### 删除数据库

对数据库进行删除

```sql
DROP DATABASES [if exits];
```

### 对数据表进行操作

#### 查看所有的表

查看数据库的所有表

```sql
SHOW TABLES;
```

#### 查询表结构

这里我们可以查看表的结构查看有哪些字段和相关信息

```sql
DESC 表名;
```

#### 查询表创建的结构

这里可以查询指定表的创建结构

```sql
SHOW CREATE TABLE 表名;
```

#### 对表的修改

1. 添加字段

   ```sql
   ALTER TABLE 表名 ADD 字段名 类型（长度） [comment 注释][约束];
   ```

2. 修改数据类型

   ```sql
   ALTER 表名 MODIFY 字段 新数据类型（长度）;
   ```

3. 修改字段名和字段类型

   ```sql
   ALTER 表名 CHANGE 旧字段名称 新字段名称 类型（长度）[comment 注释][约束];
   ```

4. 删除字段

   ```sql
   ALTER TABLE 表名 DROP 字段;
   ```

5. 修改表名

   ```sql
   ALTER TABLE 表名 RENAME TO 新表名;
   ```

6. 删除表

   * 直接删除

     ```sql
     DROP 表名;
     ```

   * 删除后，从新创建

     ```sql
     TRUNCATE 表名;
     ```

   无论是哪一个，最后删除后都没有数据了。

