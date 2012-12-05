---
layout: post
---
##Bash内直接打开目标数据库：
    alias mszarkpy = 'mysql -uroot --default-character-set=utf8 --database zarkpy'

##登录语法： 
    mysql [-u username] [-h host] [-p[password]] [dbname] 
    #示例：(初始管理帐号是root，没有密码）
    mysql -uroot -hlocalhost -p123456 zarkpy

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

##常用操作
显示数据库:show databases;

打开数据库：use xxx_database;

查看当前使用的数据库：select database();

显示表：show tables;

显示表结构：describe User; desc User; show columns from User;

查询记录：select * from User;


建库：create database database_name;

建表：create table table_name(字段设定列表)

增加记录：insert into User values('','','')

增加多条记录：insert into User values('','',''),('','',''),('','','')

修改记录：update User set name = 'ajie' where id ='1';

删除记录：delete from User where id ='1';

清空表：delete from User;

删除库：drop database database_name;

删除表：drop table table_name;

在表中增加字段：alter table 表名 add 字段 类型 其他; 
                alter table User add test int(4) default '0'

更改表名：rename table 原表名 to 新表名

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
    


