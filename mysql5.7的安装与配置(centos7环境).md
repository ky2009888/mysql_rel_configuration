### 第一步：下载mysql

```
[root@MiWiFi-R3-srv ~]
```

1:检查是否本地已经安装了mysql

```
rpm -qa | grep mysql
```

2:卸载以前的mysql

```
rpm -e 已经存在的MySQL全名
```

### 第二步：解压文件

```
[root@MiWiFi-R3-srv ~]
```

**文件名修改为mysql：**　　

```
[root@MiWiFi-R3-srv local]
```

### 第三步：配置启动文件

然后去到mysql的support-files目录下,复制my.cnf到 /etc/my.cnf(mysqld启动时自动读取)

```
[root@MiWiFi-R3-srv local]
[root@MiWiFi-R3-srv support-files]
cp: overwrite ‘/etc/my.cnf’? yes 
```

> 注意：如果你在安装时Linux虚拟机时同时安装了默认的mysql，此时操作以上步骤，终端将会提示你文件已存在是否覆盖，输入yes覆盖即可。

#### 2、配置数据库编码

```
[root@MiWiFi-R3-srv support-files]# vim /etc/my.cnf
```

**添加以下内容:**

```
[mysql]
default-character-set=utf8

[mysqld]
default-storage-engine=INNODB
character_set_server=utf8
```

#### 3、复制mysql.server到/etc/init.d/目录下(目的想实现开机自动执行效果)

```
[root@MiWiFi-R3-srv support-files]# cp mysql.server /etc/init.d/mysql
```

#### 4、修改/etc/init.d/mysql参数

```
[root@MiWiFi-R3-srv support-files]# vim /etc/init.d/mysql
```

**修改以下内容:**

```
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
```

#### 5、出于安全便利，创建一个操作数据库的专门用户

**建立一个mysql的组:**

```
[root@MiWiFi-R3-srv support-files]# groupadd mysql
```

**建立mysql用户，并且把用户放到mysql组:**

```
[root@MiWiFi-R3-srv support-files]# useradd -r -g mysql mysql
```

**给mysql用户设置一个密码:**

```
[root@MiWiFi-R3-srv support-files]# passwd mysql
```

**给目录/usr/local/mysql 更改拥有者:**

```
[root@MiWiFi-R3-srv support-files]# chown -R mysql:mysql /usr/local/mysql/
```

### 第四步：初始化 mysql 的数据库

```
[root@MiWiFi-R3-srv support-files]# cd /usr/local/mysql/bin/
[root@MiWiFi-R3-srv bin]# ./mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
```

初始化后会生成一个临时密码 root@localhost:：_\*_(最好先记录这个临时密码)

#### 2.给数据库加密

```
[root@MiWiFi-R3-srv bin]
```

#### 3.启动mysql

```
[root@MiWiFi-R3-srv bin]
```

#### 4.检查mysql是否启动

```
[root@MiWiFi-R3-srv bin]
```

**发现有进程便代表启动成功。**

### 第五步：进入客户端

#### 1.登录:

```
 [root@MiWiFi-R3-srv bin]
```

```
Enter password:这里输入之前的临时密码
```

#### 2.修改密码

```
mysql> set password=password('新密码');
```

### 第六步：设置远程访问

##### 1:打开mysql的默认端口3306:

```
[-- ] - -- --- --

[-- ] - --
```

#### 2:设置mysql的远程访问

**设置远程访问账号:grant all privileges on _._ to 远程访问用户名@’%’ identified by ‘用户密码’;**

```
mysql> grant all privileges on *.* to root@'%' identified by 'root';
```

**刷新:**

```
mysql> flush privileges;
```

### 第七步：设置开机自启动

#### 1、添加服务mysql

```
[root@MiWiFi-R3-srv bin]
```

#### 2、设置mysql服务为自启动

```
[root@MiWiFi-R3-srv bin]
```

### 第八步：配置环境变量

```
[root@MiWiFi-R3-srv ~]
```

**最后一行添加:**

```
export PATH=$JAVA_HOME/bin:/usr/local/mysql/bin:$PATH
```

**使修改生效:**

```
[root@MiWiFi-R3-srv ~]
```

至此,mysql5.7的安装就完成了!!!
