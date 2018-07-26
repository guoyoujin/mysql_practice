信息来源：https://github.com/alibaba/p3c/blob/master/p3c-gitbook/MySQL%E6%95%B0%E6%8D%AE%E5%BA%93/%E5%BB%BA%E8%A1%A8%E8%A7%84%E7%BA%A6.md

#### 1. *数据库大小写问题*
表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字。数据库字段名的修改代价很大，因为无法进行预发布，所以字段名称需要慎重考虑。 
说明：MySQL在Windows下不区分大小写，但在Linux下默认是区分大小写。因此，数据库名、表名、字段名，都不允许出现任何大写字母，避免节外生枝。 
```
正例：aliyun_admin，rdc_config，level3_name 
反例：AliyunAdmin，rdcConfig，level_3_name
```

#### 2. *主键、唯一索引、普通索引命名法则*
主键索引名为pk_字段名；唯一索引名为uk_字段名；普通索引名则为idx_字段名。 
```
说明：pk_ 即primary key；uk_ 即 unique key；idx_ 即index的简称。
```

#### 3. *表名不使用复数名词。*
表名应该仅仅表示表里面的实体内容，不应该表示实体数量，对应于DO类名也是单数形式，符合表达习惯。

#### 4. *小数类型为decimal，禁止使用float和double*
说明：float和double在存储的时候，存在精度损失的问题，很可能在值的比较时，得到不正确的结果。如果存储的数据范围超过decimal的范围，建议将数据拆成整数和小数分开存储。

#### 5. *表达是与否概念的字段，必须使用is_xxx的方式命名，数据类型是unsigned tinyint（ 1表示是，0表示否）*

说明：任何字段如果为非负数，必须是unsigned。 
正例：表达逻辑删除的字段名is_deleted，1表示删除，0表示未删除。