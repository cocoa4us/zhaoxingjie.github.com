---
layout: post
---

##数据存储方式
人工管理-文件系统-数据库系统

##数据库范型
数据库应该遵循的规则,也称为范式。最常用的4种范式：第一范式，第二范式，第三范式，BCN范式。

##SQL语法组成：
全称为结构化查询语言（Structured Query Language)，数据库管理系统通过SQL语言管理数据库中的数据。

分为3个部分：
- 数据定义语言：CREATE TABLE、 DROP TABLE、 ALTER TABLE
- 数据操作语言：查询、插入、删除、修改（SELECT、INSERT、DELETE、UPDATE）
- 数据控制语言：存取许可、存取权限（GRANT、REVOKE）

##常见数据库系统
- 甲骨文的Oracle
- IBM的DB2
- 微软的Access和SQL Server
- 开源PostgreSQL
- 开源MySQL
- 文件数据库SQLite
- 内存数据库HQL

##Mysql数据类型
- 整数类型：整型可以由十进制和十六进制表示。十六进制：0x[1-9A-F],0x中的x必须小写。包括tinyint,smallint,mediumint,int,bigint。
- 浮点数：float,double,decimal(m,d)。decimal(6,2)定义的数字如1234.56，指总长6位，小数点后精确到2位。
- 字符串:char定长，varchar(m)不定长,tinytext，text，mediumtext，longtext。
- 日期和时间：year年，date日期，time时间，datetime时间，timestamp时间。
- 二进制：binary(m),varbinary(m),bit(m),tinyblob,blob,mediumblob,longdlob。
- NULL值。

##登录语法： 
    mysql [-u username] [-h host] [-p[password]] [dbname] 
    #示例：(初始管理帐号是root，没有密码）
    mysql -uroot -hlocalhost -p123456 zarkpy

##Bash内直接打开目标数据库：
    alias mszarkpy = 'mysql -uroot --default-character-set=utf8 --database zarkpy'

##mysql的几个重要目录：
mysql的数据库文件、配置文件、命令文件分别在不同的目录。

相关命令：/usr/bin,如mysql,mysqladmin,mysqldump等命令。

数据库目录：/var/lib/mysql/

配置文件：/usr/share/mysql

启动脚本：/etc/init.d

##修改登陆密码：
格式：mysqladmin -u用户名 -p旧密码 password 新密码 

初始root没有密码时：mysqladmin -uroot password 123456

更改旧密码：mysqladmin -uroot -p123456 password 654321

##启动与停止
启动：/etc/init.d/mysql start 

停止：/usr/bin/mysqladmin -u root -p shutdown 

##操作数据库
    create database database_name;    #创建数据库
    show databases;    #显示所有数据库
    use database_name;    #打开某个数据库
    select database();    #查看当前使用的数据库
    drop database [if exist] database_name;    #删除，给出if exists子句，删除不存在的数据库时不会出错。


##数据库存储引擎
innoDB：支持事务，回滚，外键约束、崩溃恢复。表结构存在.frm文件中，数据和索引存在表空间中，读写效率稍差，占用空间大。

MyISAM：不支持事务和并发。表结构存在.frm文件中，.myd存储数据，.myi存储索引。快速，占用空间小，成熟稳定，易于管理。

    #在创建一个新的MySQL数据表时，可以为它设置存储引擎:
    create table tmp(…)ENGINE=MyISAM

    show engines \G;    #当前mysql支持的所有engine，row列表形式显示
    show engines;    #当前mysql支持的所有engine，表格形式显示
    show variables like '%engine%';    #查看当前库的engine

##创建表
    show tables   #显示数据库中的所有表

    #在当前数据库中创建表：
    create table 表名(
       列名1 数据类型 [<列的完整性约束>],
       列名2 数据类型 [<列的完整性约束>],
       ...
    );

    #建立一个表school,其由两列组成，第一列属性为非空，并做为主键,并自增:
    create table school(
       id int(10) not null auto_increment primary key,
       name varchar(20)
    );

完整性约束条件表：
- PRIMARY  KEY       主键
- FOREIGN  KEY       外键
- UNIQUE             唯一性约束
- NOT  NULL          不能为空          
- AUTO_INCREMENT     整数列默认自增1
- DEFAULT default_value       默认值约束
- UNSIGNED    无符号整数
- DEFAULT cur_timestamp 创建新记录时默认保存当前时间（仅适用timestamp数据列）
- ON UPDATE cur_timestamp 修改记录时默认保存当前时间（仅适用timestamp数据列）
- CHARACTER SET name 指定字符集（仅适用字符串）


完整性约束示例:

    #设置表的单字段主键：属性名 数据类型 PRIMARY KEY
    create table school(
       id int(10) primary key,
       name varchar(20)
    );

    #设置表的多字段主键：PRIMARY KEY(属性名1,属性名2,属性名3...)
    create table school(
       id int(10),
       name varchar(20),
       primary key(id,name)
    );

    #设置表的外键：FOREIGN KEY(属性名1,属性名2...) REFERENCES 表名(属性名1,属性名2...)
    create table score(
       cid int(10) not null auto_increment primary key,
       score int, 
       sid int,
       foreign key(sid) references student(sid)
       #定义外键别名
       #constraint score_foreign foreign key(sid) references student(sid)
    );

    #设置表的非空约束：属性名 数据类型 NOT NULL
    #设置表的唯一性约束：属性名 数据类型 UNIQUE
    #设置表的属性值自动增加：属性名 数据类型 AUTO_INCREMENT
    #设置表的属性的默认值：属性名 数据类型 DEFAULT 默认值
    create table school(
       id int(10) not null auto_increment primary key,
       name varchar(200)  charset utf8 not null default '',

    );

##显示表结构：

    describe User;
    desc User;
    show columns from User;
    show create table table_name;   #查看创建表的详细结构语句
    show create table table_name\G;   #查看创建表的详细结构语句


##修改表：

    ALTER TABLE 旧表名 RENAME [TO] 新表名;  #修改表名
    RENAME TABLE 原表名 TO 新表名;    #修改表名


    ALTER TABLE 表名 MODIFY 属性名 数据类型;    #修改字段的数据类型
    ALTER TABLE 表名 CHANGE 旧属性名 新属性名 新数据类型   #修改字段名及数据类型
    ALTER TABLE 表名 ADD 属性名1 数据类型 [完整性约束条件] [FIRST | AFTER 属性名2];    #增加字段
    ALTER TABLE 表名 DROP 属性名    #删除字段

    ALTER TABLE 表名 ENGINE=INNODB | MYISAM | MEMOERY;   #更改表的存储引擎

    ALTER TABLE 表名 DROP FOREIGN KEY 外键别名;   #删除表的外键约束

##删除表
    DROP TABLE 表名;   #删除没有被关联的普通表

    #删除被其他表关联的父表
    DROP TABLE 表名;    #直接删除会报错
    ALTER TABLE 表名 DROP FOREIGN KEY 外键别名;   #需要先删除其他表的外键约束
    DROP TABLE 表名;


##常用操作


查询记录：select * from User;

增加记录：insert into User values('','','')

增加多条记录：insert into User values('','',''),('','',''),('','','')

修改记录：update User set name = 'ajie' where id ='1';

删除记录：delete from User where id ='1';

清空表：delete from User;

在表中增加字段：alter table 表名 add 字段 类型 其他; 
                alter table User add test int(4) default '0'

##索引
Mysql中所有数据类型都可以被索引。

分类：普通索引，唯一性索引，全文索引，单列索引，多列索引，空间索引。

优点：提高查询、分级和排序的时间。

缺点：占空间，创建、更新记录时额外耗费时间。

使用原则:
+ 选择唯一性索引
+ 为经常需要排序、分组、联合操作的字段建立索引，如ORDER BY,GROUP BY,DISTINCT,UNION等操作的字段
+ 为经常作为查询条件的字段建立索引（WHERE）
+ 限制索引数目，避免过多的浪费空间
+ 尽量使用数据量少的索引
+ 删除不再使用或很少使用的索引

创建索引（3种方式）：

- 创建表的时候创建索引
- 在已经存在的表上创建索引
- 使用ALTER TABLE语句来创建索引

    #创建表的时候创建索引
    create table 表名(
        属性名 数据类型 [完整性约束],
        属性名 数据类型 [完整性约束],
        ...
        [UNIQUE | FULLTEXT | SPATIAL INDEX | KEY [别名] （属性名1[(长度)] [ASC | SESC])]
    );


    #1、创建普通索引
    CREATE TABLE index1 (
        id INT,
        name VARCHAR(20),
        sex BOOLEAN,
        INDEX(id)
    );
    SHOW CREATE TABLE index1\G;


    2、创建唯一性索引
    CREATE TABLE index2(
        id INT UNIQUE,
        name VARCHAR(20),
        UNIQUE INDEX index2_id(id ASC)
    );
    SHOW CREATE TABLE index2\G;
    
    3、创建全文索引
    CREATE TABLE index3 (
        id INT,
        info VARCHAR(20),
        FULLTEXT INDEX index3_info(info)
    ) ENGINE=MyISAM;


    4、创建单列索引
    CREATE TABLE index4 (
        id INT,
        subject VARCHAR(30),
        INDEX index4_st(subject(10))
    );
    注意:叧索引 subject 前 10 个字符

    5、创建多列索引
    CREATE TABLE index5 (
        id INT,
        name VARCHAR(20),
        sex CHAR(4),
        INDEX index5_ns(name, sex)
    );
    EXPLAIN select * from index5 where name=’123’\G;
    EXPLAIN select * from index5 where name=’123’and sex=’N’\G;

    6、创建空间索引
    CREATE TABLE index6 (
        id INT,
        Space GEOMETRY NOT NULL,
        SPATIAL INDEX index6_sp(space)
    )ENGINE=MyISAM;



    #在已经存在的表上创建索引
    CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX 索引名 ON 表名 (属性名[(长度)] [ASC|DESC]);
    1、创建普通索引
    CREATE INDEX index7_id on example0(id);
    2、创建唯一性索引
    CREATE UNIQUE INDEX index_8_id ON index8(course_id);
    3、创建全文索引
    CREATE FULLTEXT INDEX index9_info ON index9(info);
    4、创建单列索引
    CREATE INDEX index10_addr ON index10(address(4));
    5、创建多列索引
    CREATE INDEX index11_na ON index11(name, address);
    6、创建空间索引
    CREATE SPATIAL INDEX index12_line on index12(line);




    #用 ALTER TABLE 语句来创建索引
    ALTER TABLE 表名 ADD [UNIQUE|FULLTEXT|SPATIAL] INDEX 索引名 (属性名[(长度)] [ASC|DESC]);
    1、创建普通索引
    ALTER TABLE example0 ADD INDEX index12_name(name(20));
    2、创建唯一性索引
    ALTER TABLE index14 ADD UNIQUE INDEX index14_id(course_id);
    3、创建全文索引
    ALTER TABLE index15 ADD INDEX index15_info(info);
    4、创建单列索引
    ALTER TABLE index 16 ADD INDEX index16_addr(address(4));
    5、创建多列索引
    ALTER TABLE index17 ADD INDEX index17_na(name, address);
    6、创建空间索引
    ALTER TABLE index18 ADD INDEX index18_line(line);


    #删除索引
    DROP INDEX 索引名 ON 表名;

##增加Mysql用户
格式：grant select on 数据库.* to 用户名@登录主机 identified by "密码" 

    #增加一个用户user_1，密码为123，可以在任何主机上登录，
    #并对所有数据库有查询、插入、修改、删除的权限
    grant select,insert,update,delete on *.* to user_1@'%' identified by '123'
    
    #增加一个用户user_2，密码为123，只可以在localhost上登录,
    #并可以对数据库aaa进行查询、插入、修改、删除的操作
    grant select,insert,update,delete on aaa.* to user_2@localhost identified by '123'

    #允许做任何事，同root一样
    grant all on ...

    #只允许登录，其他什么也不允许做
    grant usage on ...

##用户管理
刚安装好的MySql包含一个含空密码的root帐户和一个匿名帐户，这是很大的安全隐患，在这里应把匿名帐户删除、 root帐户设置密码，可用如下命令进行：

    use mysql;
    delete from User where User="";
    update User set Password=PASSWORD('newpassword') where User='root';

如果要对用户所用的登录终端进行限制，可以更新User表中相应用户的Host字段。

##备份数据库
    #导出整个数据库
    #mysqldump -u 用户名 -p --default-character-set=latin1 数据库名 > 导出的文件名（数据库默认编码是latin1）
    mysqldump -uroot --default-character-set=latin1 aoaola > aoaola_backup.sql

    #导出一个表
    #mysqldump -u 用户名 -p 数据库名 表名 > 导出的文件名
    mysqldump -uroot aoaola User > aoaola_user.sql

    #导出一个数据库结构
    mysqldump -uroot -d –add-drop-table aoaola > aoaola_struc.sql

    #使用source命令导入数据库
    mysql -uroot aoaola
    source aoaola_backup.sql

    #使用mysqldump命令导入数据库
    mysqldump -uroot aoaola < aoaola_backup.sql

    #使用mysql命令导入数据库
    mysql -uroot -D aoaola < aoaola_backup.sql
    


