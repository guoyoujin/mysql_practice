建议大家在学习的时候安装mysql命令行控制台插件，此插件效果很棒
https://github.com/dbcli/mycli

mysql建表注意点：
在设计数据库的时候能用Int表示的数据，尽量用Int，不能用Int表示的可以设计一张统一的字典表，然后用字典表的id去使用，去关联数据，因为Id是唯一不变的，我们可以随时修改字典里面的某些内容，而不改变他们的关联关系，这样在更新字典的时候基本上更新量就非常小了.


#### 1. *创建数据库*
```mysql
CREATE DATABASE DB_NAME;
```

#### 2. *指定带数据库编码*
```mysql
CREATE DATABASE DB_NAME DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

#### 3. *查看数据库创建时候编码信息*
```mysql
SHOW CREATE DATABASE DB_NAME;
```

#### 4. *查看数据库编码*
```mysql
USE DB_NAME
SHOW VARIABLES WHERE Variable_name LIKE 'character_set_%' OR Variable_name LIKE 'collation%';
```

#### 5. *删除数据库*
```mysql
DROP DATABASE DB_NAME;
```

#### 6. *查看所有数据库*
```mysql
SHOW DATABASES;
```

#### 7. *进入、使用数据库*
```mysql
USE DB_NAME;
```

#### 8. *查看数据库里面的所有表*
```mysql
SHOW TABLES;
```

#### 9. *查看TABLE表字段信息*
```mysql
SHOW FULL COLUMNS FROM DB_NAME.TABLE_NAME;
```

#### 10. *创建表的时候指定表的编码*
```mysql
CREATE TABLE `TABLE_NAME` (
  `id` int(11) DEFAULT NULL,
  `name` varchar(100) DEFAULT NULL
) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

#### 11. *创建表的时候指定列的编码*
```mysql
CREATE TABLE `TABLE_NAME` (
  `id` int(11) DEFAULT NULL,
  `name` varchar(100) CHARACTER SET utf8mb4 DEFAULT NULL
) CHARACTER SET utf8 collate utf8_general_ci;
```

#### 12. *创建用户*
```mysql
CREATE USER 'USERNAME' IDENTIFIED BY 'PASSWORD';
CREATE USER 'USERNAME'@'HOST' IDENTIFIED BY 'PASSWORD';          //标准设置,host可以是127.0.0.1  localhost  主机ip
CREATE USER 'USERNAME'@'192.168.1.1' IDENDIFIED BY 'PASSWORD';   //绑定主机ip连接
CREATE USER 'USERNAME'@'%' IDENTIFIED BY 'PASSWORD';             //未绑定host连接
CREATE USER 'USERNAME'@'%' IDENTIFIED BY '';                     //通配符，任何ip都可连接
CREATE USER 'USERNAME'@'%';                                      //通配符，任何ip都可连接，无需密码
```

#### 13. *删除用户*
```mysql
DELETE FROM MYSQL.USER WHERE User = "USERNAME"
DROP User 'USERNAME'@'HOST';
```

#### 14. *返回MYSQL.USER列的所有信息*
```mysql
SHOW FULL COLUMNS FROM MYSQL.USER //返回MYSQL.USER列的所有信息
```

#### 15. *查询数据库用户*
```mysql
SELECT *FROM  MYSQL.USER    //返回所有用户
```

#### 16. *给用户授权*
```mysql
GRANT ALL PRIVILEGES ON DB_NAME.* TO 'USERNAME'@'HOST' IDENTIFIED BY 'PASSWORD';           //授权USERNAME用户拥有DB_NAME数据库的所有权限
GRANT ALL PRIVILEGES ON DB_NAME.TABLE_NAME TO 'USERNAME'@'HOST' IDENTIFIED BY 'PASSWORD'; //授权USERNAME用户拥有DB_NAME数据库里面TABLE_NAME表的所有权限
GRANT ALL PRIVILEGES ON DB_NAME.TABLE_NAME TO 'USERNAME'@'HOST' IDENTIFIED BY 'PASSWORD'  WITH GRANT OPTION;; //授权USERNAME用户拥有DB_NAME数据库里面TABLE_NAME表的所有权限，并且可以授权给其他用户
```

#### 17. *查看授权*
```mysql
SHOW GRANTS //查看当前用户授权
SHOW GRANTS FOR 'USERNAME'@'HOST'; //查看指定用户的授权
```

#### 18. *撤销授权*
```mysql
REVOKE ALL PRIVILEGES ON *.* FROM 'username'@'localhost';               //公式
REVOKE ALL PRIVILEGES ON DB_NAME.TABLE_NAME FROM 'USERNAME'@'HOST';     //DB_NAME.TABLE_NAME
```

#### 19. *授权USERNAME用户拥有所有数据库的某些权限*
```mysql
GRANT SELECT,DELETE,UPDATE,CREATE,DROP ON . TO 'USERNAME'@'%' IDENDIFIED BY 'PASSWORD';               //% 允许所有主机合法的ip连接， IDENDIFIED BY 密码可选，如若不需要去掉即可
GRANT SELECT,DELETE,UPDATE,CREATE,DROP ON . TO 'USERNAME'@'localhost' IDENDIFIED BY 'PASSWORD';       //% 授权主机localhost连接， IDENDIFIED BY 密码可选，如若不需要去掉即可
GRANT SELECT,DELETE,UPDATE,CREATE,DROP ON . TO 'USERNAME'@'127.0.0.1' IDENDIFIED BY 'PASSWORD';       //% 授权主机127.0.0.1连接， IDENDIFIED BY 密码可选，如若不需要去掉即可
GRANT SELECT,DELETE,UPDATE,CREATE,DROP ON . TO 'USERNAME'@'192.168.0.1' IDENDIFIED BY 'PASSWORD';     //% 授权主机192.168.0.1连击， IDENDIFIED BY 密码可选，如若不需要去掉即可
```

#### 20. *修改用户密码*
```mysql
SET PASSWORD FOR 'USERNAME'@'HOST' = PASSWORD('PASSWORD');
```
主要看''号里面的变量'USERNAME'@'HOST'即为要修改的人的查询条件

#### 21. *刷新系统配置*
```mysql
FLUSH PRIVILEGES; 
```
刷新系统配置,一般在修改了一些数据库的基本配置之后都需要执行此操作操作才能生效