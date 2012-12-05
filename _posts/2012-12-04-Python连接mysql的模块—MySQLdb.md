---
layout: post
---


6月份进公司至今，python+前端并行开发，做的小组、@好友、投票、通知中心主要是一些数据库的读写操作，对比着Django和webpy来看，对python实现web应用的底层还是不甚明了，索性先抛弃框架来从头过一遍。

##显示所有数据库：

    #! /usr/bin/env python
    #coding=utf-8

    import MySQLdb

    #建立和数据库系统的连接
    conn = MySQLdb.connect(host='localhost',user='root',passwd='xxx')

    #获取操作游标
    cursor = conn.cursor()

    #执行SQL,显示所有数据库
    cursor.execute('show databases;')

    for data in cursor.fetchall():
        print data

    #关闭游标
    cursor.close()

    #关闭连接，释放资源
    conn.close()

##创建数据库、创建表、插入数据：

    #创建数据库
    cursor.execute('create database if not exists python;')
    #选择数据库
    conn.select_db('python')
    #创建数据表
    cursor.execute('create table test(id int,info varchar(100))')

    #插入一条记录
    value = [1,'inserted content']
    cursor.execute('insert into test values(%s,%s)',value)

    #插入多条记录
    values = []
    for i in range(20):
        values.append((i,'i am record'+str(i)))
    cursor.executemany('insert into test values(%s,%s)',values)

##查询
    
    #记录有多少条
    #cursor.execute返回值为受影响的行数
    count = cursor.execute('select * from User;')
    print '总共有%s条记录' % count

    #只获取一条记录
    cursor.execute('select * from User;')
    result = cursor.fetchone()
    print result #返回元组，如"(1L,'ajiex')"
    print 'id:%s,name:%s' % (result[0],result[1])
    
    #获取多条记录,由于之前执行了fetchone，游标从第2条记录开始执行
    results = cursor.fetchmany(5)
    for r in results:
        print r
    
    #获取所有记录
    #重置游标位置为,0为偏移量,mode默认为relative,可选absolute
    cursor.scroll(0,mode='absolute')
    results = cursor.fetchall()
    for r in results:
        print r 

##默认MySQLdb返回的是元组，不利于维护，解决如下：
    import MySQLdb, MySQLdb.cursors
    conn = MySQLdb.connect(host='localhost', user='root', db='aoaola', passwd='', 
    compress=1, cursorclass=MySQLdb.cursors.DictCursor, charset='utf8')
    cursor =conn.cursor()
    cursor.execute('select id,name from User')
    results = cursor.fetchall()
    for r in results:
        print r['id'], r['name']

    #或者
    conn = MySQLdb.connect(host='localhost', user='root', db='aoaola', passwd='', 
    compress=1, charset='utf8')
    cursor =conn.cursor(cursorclass=MySQLdb.cursors.DictCursor)

