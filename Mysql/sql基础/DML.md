# DML

数据操作语言，用来对数据库表中的数据进行增删改

## 具体的操作

### 添加数据

```sql
//根据字段进行添加数据
INSERT INTO 表名 （字段名，。。。） values (值。。。);
//全部添加数据，这里添加值的时候要对应字段进行添加
INSERT INTO 表名 values （值。。）;
//批量的添加
INSERT INTO 表名 values （值。。），（值。。），（值。。），（值。。）;
INSERT INTO 表名 （字段名，。。。） values (值。。。)，(值。。。)，(值。。。);
```

这里需要注意的是添加日期字符串的时候，需要用引号引起来

### 修改数据

```sql
UPDATE 表名 SET 字段名 = 值，。。。[where 条件];
```

如果这里不写条件，那么就是对整张表进行修改。

### 删除数据

```sql
DELECT FROM 表名 [where 条件];
```

不添加条件就是删除这个表所有的数据。

上面就是DML基础的使用方式。