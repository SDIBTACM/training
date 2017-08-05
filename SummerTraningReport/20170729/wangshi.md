#  数据库编程
使用SQLite3步骤:
~~~
           (1)导入相关模块：import sqlite3 
           (2)连接数据库并获取数据连接对象（connect()）
                con=sqlite3.connect(":memory:") #临时放入内存数据库
           (3)获取游标对象:cur=con.cursor()
           (4)使用游标对象的方法（execute(),enecutemany()） #进行插入，修改，删除，查询
                cur.execute('SQL语句')
           (5)关闭游标对象和数据库连接(close（）)
~~~


   
