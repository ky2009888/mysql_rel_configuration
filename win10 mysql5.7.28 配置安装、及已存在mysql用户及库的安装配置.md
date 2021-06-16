# [win10 mysql5.7.28 配置安装、及已存在mysql用户及库的安装配置](https://www.cnblogs.com/xiaoruilin/p/11995308.html)

如果有服务，使用下面命令删除，管理员身份打开cmd :

net stop mysql  
sc delete mysql  
pause

![](https://img2018.cnblogs.com/blog/822458/201912/822458-20191206145444849-624574352.png)

1、下载 [https://dev.mysql.com/downloads/mysql/5.7.html](https://dev.mysql.com/downloads/mysql/5.7.html)

没有的Oracle帐号怕麻烦的可以在些下载：

链接：https://pan.baidu.com/s/1E46mKltT4yenF3Souk1OZg  
提取码：91et

2、解压为 D:\\SoftWare\\mysql-5.7.28-winx64，新建一个my.ini文件内容如下

[![复制代码](https://www.cnblogs.com//common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

\[mysqld\]
skip\-grant-tables # 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\\\\SoftWare\\\\mysql-5.7.28-winx64\\\\   # 切记此处一定要用双斜杠\\\\，单斜杠我这里会出错。 # 设置mysql数据库的数据的存放目录
datadir=D:\\\\SoftWare\\\\mysql-5.7.28-winx64\\\\Data   # 此处同上 # 允许最大连接数
max\_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max\_connect\_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8 # 创建新表时将使用的默认存储引擎
default\-storage-engine=INNODB # 默认使用“mysql\_native\_password”插件认证
default\_authentication\_plugin=mysql\_native\_password
\[mysql\] # 设置mysql客户端默认字符集
default\-character-set=utf8
\[client\] # 设置mysql客户端连接服务端时默认使用的端口
port=3306
default\-character-set=utf8

[![复制代码](https://www.cnblogs.com//common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

3、以管理员身份运行CMD命令窗口,并执行相应操作

D:  
cd D:\\SoftWare\\mysql-5.7.28-winx64\\bin  
mysqld --defaults\-file\=D:\\SoftWare\\mysql-5.7.28-winx64\\my.ini --initialize --user=mysql --console

![](https://img2018.cnblogs.com/blog/822458/201912/822458-20191206145557768-1742665627.png)

请把上图上初始密码记住！

4、安装MySQL服务，以管理员身份运行cmd

mysqld --install MySQL --defaults\-file\=D:\\SoftWare\\mysql-5.7.28-winx64\\my.ini

![](https://img2018.cnblogs.com/blog/822458/201912/822458-20191206145739769-1213508783.png)

5、启动mysql，两种方式

#后台运行 
net start mysql #前台运行
mysqld --defaults\-file\=D:\\mysql\\my.ini     

6、首次连接需要修改root密码

 mysql -uroot -p密码 -P端口 -hlocalhost  
#ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost' (10061) #原因没有启动服务  
 mysql\> set password=password("mysql");
 mysql\> flush privileges;

# [Mysql添加用户与授权](https://www.cnblogs.com/zhangjianqiang/p/10019809.html)

1、本地环境

```
CentOS Linux release 7.5.1804 (Core)
mysql  Ver 14.14 Distrib 5.7.22, for Linux (x86_64) using  EditLine wrapper
```

2、以root用户登录Mysql

```
mysql -uroot -proot
```

3、切换到mysql数据库

```
use mysql
```

4、添加用户

```
//只允许指定ip连接
create user '新用户名'@'localhost' identified by '密码';
//允许所有ip连接（用通配符%表示）
create user '新用户名'@'%' identified by '密码';
```

5、为新用户授权

```
//基本格式如下
grant all privileges on 数据库名.表名 to '新用户名'@'指定ip' identified by '新用户密码' ;
//示例
//允许访问所有数据库下的所有表
grant all privileges on *.* to '新用户名'@'指定ip' identified by '新用户密码' ;
//指定数据库下的指定表
grant all privileges on test.test to '新用户名'@'指定ip' identified by '新用户密码' ;
```

6、设置用户操作权限

```
//设置用户拥有所有权限也就是管理员
grant all privileges on *.* to '新用户名'@'指定ip' identified by '新用户密码' WITH GRANT OPTION;
//拥有查询权限
grant select on *.* to '新用户名'@'指定ip' identified by '新用户密码' WITH GRANT OPTION;
//其它操作权限说明,select查询 insert插入 delete删除 update修改
//设置用户拥有查询插入的权限
grant select,insert on *.* to '新用户名'@'指定ip' identified by '新用户密码' WITH GRANT OPTION;
//取消用户查询的查询权限
REVOKE select ON what FROM '新用户名';
```

7、删除用户

```
DROP USER username@localhost;
```

8、修改后刷新权限

```
FLUSH PRIVILEGES;
```

 附：可能碰到的问题

ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement

解决方法：

[![复制代码](https://www.cnblogs.com//common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)
mysql\> set password=password("mysql");(或 set password for 'root'@'localhost'=password('root');)
Query OK, 0 rows affected, 1 warning (0.00 sec)

flush privileges;

[![复制代码](https://www.cnblogs.com//common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

2、启动时一直启动不了，也停止不了

D:\\SOFTWARE\\mysql-5.7.28-winx64\\bin\\>mysqld --console  
报下面错：  
Found option without preceding group in config file  
原因:就是my.ini 文件格式为utf-8   
解决:文件文件格式修改为ANSI

 **已存在mysql用户及库的安装配置**

 背景：我有一台mysql-5.7.21的数据库，备份后始终还原还到mysql-5.7.28版本的库上，所以干脆就把整个5.7.21版本库全迁过来

拷贝过来后 注意：my.ini文件中的路径及端口（不能与其他服务占用）,进入bin下，以管理员身份安装服务启动方可。

mysqld --install MySQL\[端口\] --defaults-file=D:\\SoftWare\\mysql-5.7.21-winx64\\my.ini

 启动服务报错：本地计算机上的MYSQL 服务启动后停止。某些服务在未由其他服务或程序使用时将自动停止

注意：my.ini 中的Data的路径