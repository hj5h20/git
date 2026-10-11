# MySQL 学习笔记

## 目录

- [数据库操作](#数据库操作)
- [表操作（DDL）](#表操作ddl)
- [数据操作（DML）](#数据操作dml)
- [用户管理（DCL）](#用户管理dcl)
- [权限控制（DCL）](#权限控制dcl)
- [注释](#注释)

## 数据库操作

### 查看所有数据库

```sql
SHOW DATABASES;
```

### 查看当前数据库

```sql
SELECT DATABASE();
```

### 创建数据库

```sql
CREATE DATABASE [IF NOT EXISTS] 数据库名
  [DEFAULT CHARACTER SET 字符集]
  [COLLATE 排序规则];
```

### 删除数据库

```sql
DROP DATABASE [IF EXISTS] 数据库名;
```

### 使用数据库

```sql
USE 数据库名;
```

> 方括号 `[]` 表示可选内容，实际执行时不要把方括号写进 SQL。

## 表操作（DDL）

### 创建表

```sql
CREATE TABLE 表名 (
    字段1 字段1类型 [COMMENT '字段1注释'],
    字段2 字段2类型 [COMMENT '字段2注释'],
    字段3 字段3类型 [COMMENT '字段3注释']
) [COMMENT '表注释'];
```

**注意：** 字段之间用逗号分隔，最后一个字段后不加逗号。

### 查询当前数据库中的所有表

```sql
SHOW TABLES;
```

### 查看表结构

```sql
DESC 表名;
```

### 查看建表语句

```sql
SHOW CREATE TABLE 表名;
```

### 修改字段数据类型

```sql
ALTER TABLE 表名 MODIFY 字段名 新数据类型;
```

### 修改字段名和数据类型

```sql
ALTER TABLE 表名
  CHANGE 旧字段名 新字段名 新数据类型 [COMMENT '字段注释'] [约束];
```

**示例：** 将 `emp` 表中的 `nickname` 字段改名为 `username`，类型改为 `VARCHAR(30)`。

```sql
ALTER TABLE emp
  CHANGE nickname username VARCHAR(30) COMMENT '名称';
```

### 添加字段

```sql
ALTER TABLE 表名
  ADD 字段名 数据类型 [COMMENT '字段注释'] [约束];
```

**示例：** 为 `emp` 表添加 `nickname` 字段。

```sql
ALTER TABLE emp
  ADD nickname VARCHAR(20) COMMENT '昵称';
```

### 修改表名

```sql
ALTER TABLE 表名 RENAME TO 新表名;
```

### 删除字段

```sql
ALTER TABLE 表名 DROP 字段名;
```

### 删除表

```sql
DROP TABLE [IF EXISTS] 表名;
```

### 删除表中所有数据

```sql
TRUNCATE TABLE 表名;
```

> `TRUNCATE TABLE` 会清空表中的所有数据，不能指定 `WHERE` 条件。

## 数据操作（DML）

### 插入数据

向指定字段插入数据：

```sql
INSERT INTO 表名 (字段1, 字段2)
VALUES (值1, 值2);
```

向全部字段插入数据：

```sql
INSERT INTO 表名
VALUES (值1, 值2, ...);
```

批量插入指定字段的数据：

```sql
INSERT INTO 表名 (字段1, 字段2)
VALUES
    (值1, 值2),
    (值1, 值2);
```

批量插入全部字段的数据：

```sql
INSERT INTO 表名
VALUES
    (值1, 值2, ...),
    (值1, 值2, ...);
```

**注意：**

1. 指定字段的顺序必须与对应的值的顺序一致。
2. 字符串和日期类型的值通常需要用引号括起来。
3. 插入值必须符合字段的数据类型、长度及约束要求。

### 修改数据

```sql
UPDATE 表名
SET 字段1 = 值1,
    字段2 = 值2
[WHERE 条件];
```

> 不写 `WHERE` 条件会修改表中的所有数据。执行前应确认条件范围。

### 删除数据

```sql
DELETE FROM 表名
[WHERE 条件];
```

**注意：**

1. 不写 `WHERE` 条件会删除表中的所有数据。
2. `DELETE` 删除的是数据行，不能用来只删除某个字段的值；修改字段值可以使用 `UPDATE`。

## 用户管理（DCL）

### 查看用户

MySQL 用户信息保存在 `mysql.user` 表中。需要具备相应权限：

```sql
SELECT user, host
FROM mysql.user;
```

### 创建用户

```sql
CREATE USER '用户名'@'主机名'
IDENTIFIED BY '密码';
```

### 修改用户密码

```sql
ALTER USER '用户名'@'主机名'
IDENTIFIED BY '新密码';
```

如需指定认证插件，可使用：

```sql
ALTER USER '用户名'@'主机名'
IDENTIFIED WITH mysql_native_password BY '新密码';
```

### 删除用户

```sql
DROP USER '用户名'@'主机名';
```

**注意：**

1. 主机名可以使用 `%` 通配，表示允许从任意主机连接；生产环境应尽量限制访问来源。
2. 用户和权限管理通常由数据库管理员（DBA，Database Administrator）负责。

**示例：** 创建只能从本机连接的 `itcast` 用户。

```sql
CREATE USER 'itcast'@'localhost'
IDENTIFIED BY '请替换为安全密码';
```

## 权限控制（DCL）

### 查询用户权限

```sql
SHOW GRANTS FOR '用户名'@'主机名';
```

### 授予权限

```sql
GRANT 权限列表
ON 数据库名.表名
TO '用户名'@'主机名';
```

### 撤销权限

```sql
REVOKE 权限列表
ON 数据库名.表名
FROM '用户名'@'主机名';
```

**注意：**

1. 多个权限之间用逗号分隔。
2. 数据库名或表名可以使用 `*` 作为通配符，例如 `数据库名.*` 表示该数据库中的所有表。
3. 可以通过 `SHOW GRANTS` 查看用户当前权限。

## 注释

单行注释：

```sql
-- 注释内容
# 注释内容
```

> `#` 是 MySQL 支持的单行注释写法。

多行注释：

```sql
/*
注释内容
*/
```
