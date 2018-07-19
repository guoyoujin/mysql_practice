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
SHOW INDEX FROM DB_NAME.TABLE_NAME;
SHOW KEYS FROM DB_NAME.TABLE_NAME;
SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA = 'DB_NAME' AND TABLE_NAME = 'TABLE_NAME'
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

#### 22. *查看表的创建信息，返回的是sql创建 语句*
```mysql
SHOW CREATE TABLE DB_NAME.TABLE_NAME;
```

#### 23. *查看数据库表的状态详情*
```mysql
SHOW TABLE STATUS FROM DB_NAME
```

#### 24. *相等连接、内连接，即左、右连接的交集*
```mysql
SELECT *FROM doctors AS d,orders AS o
where d.id = o.doctor_id AND d.id<=10

等价于

SELECT *FROM doctors
JOIN orders AS o ON o.doctor_id = doctors.id AND d.id<=10

SELECT *FROM doctors
JOIN orders AS o ON o.doctor_id = doctors.id
WHERE doctors.id <10

SELECT *FROM doctors
JOIN orders AS o ON o.doctor_id = doctors.id AND doctors.id <10
WHERE doctors.id <10
```
主要是满足d.id = o.id AND  d.id<=10的条件都会出现在结果集里面，也即是根据id相同取交集

#### 25. *外连接*
1、左连接
```mysql
SELECT *FROM doctors AS d
LEFT JOIN orders AS o ON o.doctor_id = doctors.id
```
1、右连接
```mysql
SELECT *FROM doctors AS d
RIGHT JOIN orders AS o ON o.doctor_id = doctors.id
```

#### 26. *集和运算*
1、使用UNION求并集并排序
```mysql
SELECT a.id FROM
(
SELECT orders.id FROM orders WHERE orders.id <10
UNION
SELECT doctors.id FROM doctors WHERE doctors.id <10
) AS a
ORDER BY a.id desc
```
UNION:求并集，合并两个操作的结果，去掉重复的部分 ，使用时列必须相同

2、使用UNION ALL求并集并排序
```mysql
SELECT a.id FROM
(
SELECT orders.id FROM orders where orders.id <20
UNION ALL
SELECT doctors.id FROM doctors where doctors.id <10
) AS a
ORDER BY a.id desc
```
UNION ALL:并集，合并两个操作的结果，保留重复的部分 

#### 27. *取余数*
```mysql
SELECT MOD(234, 10);
结果：4
```

#### 28. *取模运算*
```mysql
SELECT 234 div 10; 
结果：2
```

#### 29. *聚合函数*
```mysql
avg(col)返回指定列的平均值
count(col)返回指定列中非null值的个数
min(col)返回指定列的最小值
max(col)返回指定列的最大值
sum(col)返回指定列的所有值之和
group_concat(col) 返回由属于一组的列值连接组合而成的结果
```

#### 30. *数学函数*
```mysql
abs(x)   返回x的绝对值
bin(x)   返回x的二进制（oct返回八进制，hex返回十六进制）
ceiling(x)   返回大于x的最小整数值
exp(x)   返回值e（自然对数的底）的x次方
floor(x)   返回小于x的最大整数值
greatest(x1,x2,...,xn)返回集合中最大的值
least(x1,x2,...,xn)      返回集合中最小的值
ln(x)                    返回x的自然对数
log(x,y)返回x的以y为底的对数
mod(x,y)                 返回x/y的模（余数）
pi()返回pi的值（圆周率）
rand()返回０到１内的随机值,可以通过提供一个参数(种子)使rand()随机数生成器生成一个指定的值。
round(x,y)返回参数x的四舍五入的有y位小数的值
sign(x) 返回代表数字x的符号的值
sqrt(x) 返回一个数的平方根
truncate(x,y)            返回数字x截短为y位小数的结果
```

#### 31. *字符串函数*
```mysql
ascii(char)返回字符的ascii码值
bit_length(str)返回字符串的比特长度
concat(s1,s2...,sn)将s1,s2...,sn连接成字符串
concat_ws(sep,s1,s2...,sn)将s1,s2...,sn连接成字符串，并用sep字符间隔
insert(str,x,y,instr) 将字符串str从第x位置开始，y个字符长的子串替换为字符串instr，返回结果
find_in_set(str,list)分析逗号分隔的list列表，如果发现str，返回str在list中的位置
lcase(str)或lower(str) 返回将字符串str中所有字符改变为小写后的结果
left(str,x)返回字符串str中最左边的x个字符
length(s)返回字符串str中的字符数
ltrim(str) 从字符串str中切掉开头的空格
position(substr,str) 返回子串substr在字符串str中第一次出现的位置
quote(str) 用反斜杠转义str中的单引号
repeat(str,srchstr,rplcstr)返回字符串str重复x次的结果
reverse(str) 返回颠倒字符串str的结果
right(str,x) 返回字符串str中最右边的x个字符
rtrim(str) 返回字符串str尾部的空格
strcmp(s1,s2)比较字符串s1和s2
trim(str)去除字符串首部和尾部的所有空格
ucase(str)或upper(str) 返回将字符串str中所有字符转变为大写后的结果
```

#### 32. *日期和时间函数*
```mysql
curdate()或current_date() 返回当前的日期
curtime()或current_time() 返回当前的时间
date_add(date,interval int keyword)返回日期date加上间隔时间int的结果(int必须按照关键字进行格式化),如：selectdate_add(current_date,interval 6 month);
date_format(date,fmt)  依照指定的fmt格式格式化日期date值
date_sub(date,interval int keyword)返回日期date加上间隔时间int的结果(int必须按照关键字进行格式化),如：selectdate_sub(current_date,interval 6 month);
dayofweek(date)   返回date所代表的一星期中的第几天(1~7)
dayofmonth(date)  返回date是一个月的第几天(1~31)
dayofyear(date)   返回date是一年的第几天(1~366)
dayname(date)   返回date的星期名，如：select dayname(current_date);
from_unixtime(ts,fmt)  根据指定的fmt格式，格式化unix时间戳ts
hour(time)   返回time的小时值(0~23)
minute(time)   返回time的分钟值(0~59)
month(date)   返回date的月份值(1~12)
monthname(date)   返回date的月份名，如：select monthname(current_date);
now()    返回当前的日期和时间
quarter(date)   返回date在一年中的季度(1~4)，如select quarter(current_date);
week(date)   返回日期date为一年中第几周(0~53)
year(date)   返回日期date的年份(1000~9999)
一些示例：
获取当前系统时间：select from_unixtime(unix_timestamp());
select extract(year_month from current_date);
select extract(day_second from current_date);
select extract(hour_minute from current_date);
返回两个日期值之间的差值(月数)：select period_diff(200302,199802);
在mysql中计算年龄：
select date_format(from_days(to_days(now())-to_days(birthday)),'%y')+0 as age from employee;
这样，如果brithday是未来的年月日的话，计算结果为0。
下面的sql语句计算员工的绝对年龄，即当birthday是未来的日期时，将得到负值。
select date_format(now(), '%y') - date_format(birthday, '%y') -(date_format(now(), '00-%m-%d') <date_format(birthday, '00-%m-%d')) as age from employee
```