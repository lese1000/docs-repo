centos 属于redhead系列

http://www.linuxidc.com/Linux/2015-04/116003.htm（CentOS6.5系统下RPM包安装MySQL5.6）

1.rpm方式安葬：

MySQL安装后涉及的目录如下：
目录 	目录中的内容
/usr/bin 	客户端程序和脚本
/usr/sbin 	Mysqld服务器
/var/lib/mysql 	数据库的日志文件
/usr/share/info 	信息格式手册
/usr/share/man 	Unix 手册页
/usr/include/mysql 	包括 （标题） 的文件
/usr/lib/mysql 	mysql的lib包
/usr/share/mysql 	杂项的支持文件，包括错误消息） 字符设置的文件，示例配置文件，SQL 数据库安装
/usr/share/sql-bench 	基准

64位下载:server - devel - client
[root@linuxidc tools]# wget http://dev.mysql.com/Downloads/MySQL-5.6/MySQL-server-5.6.21-1.rhel5.x86_64.rpm  -ok

[root@linuxidc tools]# wget http://dev.mysql.com/Downloads/MySQL-5.6/MySQL-devel-5.6.21-1.rhel5.x86_64.rpm  --ok

[root@linuxidc tools]# wget http://dev.mysql.com/Downloads/MySQL-5.6/MySQL-client-5.6.21-1.rhel5.x86_64.rpm  --ok

32位下载
wget http://dev.mysql.com/Downloads/MySQL-5.6/MySQL-shared-compat-5.6.21-1.el6.i686.rpm   --ok
wget http://dev.mysql.com/Downloads/MySQL-5.6/MySQL-5.6.21-1.el6.i686.rpm-bundle.tar --ok(程序集合包,包含上述三个文件)

5.7-32位
wget http://dev.mysql.com/Downloads/MySQL-5.7/Mysql-5.7.15-1.el6.i686.rpm-bundle.tar

5.7-64位
wget http://dev.mysql.com/Downloads/MySQL-5.7/Mysql-5.7.15-1.el7.x86_64.rpm-bundle.tar  --ok



（卸载之前安装的版本命令：
1.查询: rpm -qa | grep -i mysql   
MySQL-client-5.6.35-1.el7.x86_64
MySQL-server-5.6.35-1.el7.x86_64
MySQL-devel-5.6.35-1.el7.x86_64

2.卸载：rpm -e ）
rpm -e MySQL-client-5.6.35-1.el7.x86_64
rpm -e MySQL-server-5.6.35-1.el7.x86_64
rpm -e MySQL-devel-5.6.35-1.el7.x86_64

3.删除配置信息。find / -name mysql 注：清空相关mysql的所有目录以及文件和其他配置和设置等。如果有，则删除。也必须考虑其他软件不去影响。

注:删除MySQL数据库目录(关键) ，否则password不更新（默认安装，如果自定义安装路径和链接路径ln -s ……请删除。）
rm -rf /var/lib/mysql

清空相关mysql的所有目录
rm -rf /usr/lib/mysql
rm -rf /usr/share/mysql
rm -rf /usr/lib64/mysql
4.（自启服务）：
[root@localhost usr]# chkconfig --list | grep -i mysql  
[root@localhost usr]# chkconfig --del mysqld 

6.开始安装
[root@linuxidc tools]# rpm -ivh MySQL-server-5.6.21-1.rhel5.x86_64.rpm

err1:
error: Failed dependencies:
        libaio.so.1()(64bit) is needed by MySQL-server-5.6.21-1.rhel5.x86_64
        libaio.so.1(LIBAIO_0.1)(64bit) is needed by MySQL-server-5.6.21-1.rhel5.x86_64
        libaio.so.1(LIBAIO_0.4)(64bit) is needed by MySQL-server-5.6.21-1.rhel5.x86_64
安装MySQL-server报错，原因是没有安装libaio，系统缺少libaio.so此软件包，下边yum安装一下libaio.so软件包。
[root@linuxidc tools]# yum install -y libaio

err2:
FATAL ERROR: please install the following Perl modules before executing /usr/bin/mysql_install_db:
[root@linuxidc tools]# yum install -y perl 
(如果提示安装perl，安装mysql依然报错，则使用：yum install -y perl-Module-Install.noarch)

===>开始安装Server
[root@linuxidc tools]# rpm -ivh MySQL-server-5.6.21-1.rhel5.x86_64.rpm
[root@iZ0xi0upwm5ow4eq0qyp05Z devtools]# rpm -ivh MySQL-server-5.6.35-1.el7.x86_64.rpm
warning: MySQL-server-5.6.35-1.el7.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:MySQL-server-5.6.35-1.el7        ################################# [100%]

===>该处安装时有时生成随机密码，这个看情况。不同版本或者系统之前有安装过，配置文件未删除。具体看安装时的提示信息。

[root@linuxidc tools]# rpm -ivh MySQL-client-5.6.21-1.rhel5.x86_64.rpm 
Preparing...                ########################################### [100%]
  1:MySQL-client          ########################################### [100%]
[root@linuxidc tools]# rpm -ivh MySQL-devel-5.6.21-1.rhel5.x86_64.rpm 
Preparing...                ########################################### [100%]
  1:MySQL-devel            ########################################### [100%]

6.修改配置文件位置。
[root@linuxidc tools]# cp /usr/share/mysql/my-default.cnf /etc/my.cnf

7.初始化MySQL及修改MySQL默认的root密码。==>如果在安装mysql-server时已经随机生成密码，则 可直接启动 service mysql start ;
[root@linuxidc tools]# /usr/bin/mysql_install_db

8.启动MySql，并查看相关信息
[root@linuxidc tools]# service mysql start 

[root@linuxidc tools]# ps -ef | grep mysql
root      2188    1  0 14:48 pts/1    00:00:00 /bin/sh /usr/bin/mysqld_safe --datadir=/var/lib/mysql --pid-file=/var/lib/mysql/linuxidc.pid
mysql    2303  2188 30 14:48 pts/1    00:00:02 /usr/sbin/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/plugin --user=mysql --log-error=/var/lib/mysql/linuxidc.err --pid-file=/var/lib/mysql/linuxidc.pid
root      2331  1853  0 14:49 pts/1    00:00:00 grep mysql

[root@linuxidc tools]# netstat -anpt | grep 3306
tcp        0      0 :::3306                    :::*                        LISTEN      2303/mysqld

 
9.登录mysql，并设置root密码。 
1）查看默认密码
[root@linuxidc tools]# more /root/.mysql_secret
# The random password set for the root user at Thu Apr  9 14:43:59 2015 (local time): F6K3v_xggFoLQeiN
2）登录到mysql
[root@linuxidc tools]# mysql -uroot -pF6K3v_xggFoLQeiN
Warning: Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.6.21
 
Copyright (c) 2000, 2014, Oracle and/or its affiliates. All rights reserved.
 
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
 
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

3）修改默认密码。 
mysql> SET PASSWORD = PASSWORD('123.com'); 
mysql> exit
Bye
[root@linuxidc tools]# mysql -uroot -p123.com
Warning: Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 6
Server version: 5.6.21 MySQL Community Server (GPL)
 
Copyright (c) 2000, 2014, Oracle and/or its affiliates. All rights reserved.
 
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
 
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
 
mysql>

10.设置MySQL服务开机自启动。
[root@linuxidc tools]# chkconfig mysql on
[root@linuxidc tools]# chkconfig mysql --list
mysql          0:off  1:off  2:on    3:on    4:on    5:on    6:off

11.创建用户并授权
登录root账户后执行下面操作：

（设置root可远程登录：grant all privileges on *.* to 'root'@'%' identified by 'root-password';需要设置远程访问的密码。）

方式1，先创建数据库，并授权给指定用户。（允许远程登录=>'%',本地=>'localhost'）
mysql> create database print_db DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
grant all privileges on `print_db`.* to 'printer'@'%' identified by 'p@int2017'; 
方式2，创建用户并授权所有权限
mysql> grant all privileges on *.* to 'printer'@'%' identified by 'p@int2017';
mysql> flush privileges;
用户：printer
密码：p@int2017

mysql>  select user,password,host from user;
+---------+-------------------------------------------+-------------------------+
| user    | password                                  | host                    |
+---------+-------------------------------------------+-------------------------+
| root    | *481EC6F1EDDF36DBD798A5750E66FBD3DFFFFBE2 | localhost               |
| root    | *2A621B031A447334B3B94BA7CD1782AA6466BDD0 | iz0xi0upwm5ow4eq0qyp05z |
| root    | *2A621B031A447334B3B94BA7CD1782AA6466BDD0 | 127.0.0.1               |
| root    | *2A621B031A447334B3B94BA7CD1782AA6466BDD0 | ::1                     |
| printer | *F9FCC308BB10CFD284D6D21F8866DB84359AED97 | %                       |
| root    | *481EC6F1EDDF36DBD798A5750E66FBD3DFFFFBE2 | %                       |
+---------+-------------------------------------------+-------------------------+


10.其他：mysql日志存放处：/var/lib/mysql
iZ0xi0upwm5ow4eq0qyp05Z.err =》实例名.err
























http://www.linuxidc.com/Linux/2016-04/130414.htm（CentOS 7下MySQL 5.7安装、配置与应用）
2.解压方式安装：
本文描述的安装是采用通用的二进制压缩包（linux - Generic）以解压方式安装，相当于绿色安装了。
 
一、下载通用安装二进制包
 
先下载mysql安装包：打开 http://dev.mysql.com/downloads/mysql/
选择 linux - Generic并在其下选择
Linux - Generic (glibc 2.5) (x86, 64-bit), Compressed TAR Archive
进行下载。可以先下载到一个临时目录里，解压后，得到两个包：
mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz 
mysql-test-5.7.11-linux-glibc2.5-x86_64.tar.gz
只需要mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz 这个包就行了。
 
二、建立用户和目录
 
建立用户mysql，组mysql。后面mysql就使用这个用户来运行（注意这也是mysql启动脚本中默认的用户，因此最好不要改名）。
#groupadd mysql
#useradd -r -g mysql mysql
（使用-r参数表示mysql用户是一个系统用户，不能登录）
 
建立目录/work/program，后面mysql就安装在这个目录下面。
#mkdir /work/program
 
三、安装
 
【解压】
将前面得到的mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz解压至/work/program目录下
#tar zxvf mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz -C /work/program
 
这时在program下得到的目录名很长，如果不想改名，则可以建立一个联接：
#ln -s mysql-5.7.11-linux-glibc2.5-x86_64 mysql
此后就可以用/work/program/mysql来找到mysql的安装目录了
 
注意，如果mysql目录下没有data目录，手动建一个。
 
【目录权限设置】
将mysql及其下所有的目录所有者和组均设为mysql:
#cd /work/program/mysql
#chown mysql:mysql -R .
 
【初始化】
#/work/program/mysql/bin/mysqld --initialize --user=mysql --datadir=/work/program/mysql/data --basedir=/work/program/mysql
注意：
1. data目录解压后没有，需要手动建立（见上文）；
2. mysql5.7和之前版本不同，很多资料上都是这个命令
...../scripts/mysql_install_db --user=mysql
 而5.7版本根本没有这个。
 
初始化成功后出现如下信息：
201x-xx-xxT07:10:13.583130Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
201x-xx-xx T07:10:13.976219Z 0 [Warning] InnoDB: New log files created, LSN=45790
201x-xx-xx T07:10:14.085666Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
201x-xx-xx T07:10:14.161899Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 1fa941f9-effd-11e5-b67d-000c2958cdc8.
201x-xx-xx T07:10:14.165534Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
201x-xx-xx T07:10:14.168555Z 1 [Note] A temporary password is generated for root@localhost: q1SLew5T_6K,
 
注意最后一行，这也是和之有版本不同的地方，它给了root一个初始密码，后面要登录的时候要用到这个密码。
 
【配置】
将mysql/support-files下的my-default.cnf改名为my.cnf，拷到/etc下（或者考到｛mysql｝下，然后作一个软链接到/etc下）：
#cp /work/program/mysql/support-files/my-default.cnf /etc/my.cnf
my.cnf中关键配置：
[mysqld]
basedir = /work/program/mysql
datadir = /work/program/mysql/data
port = 3306
socket = /work/program/mysql/tmp/mysql.sock
 
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
 
注意，tmp目录不存在，请创建之。
 
如果不把my.cnf拷到/etc下，运行时会出现：
mysqld: Can't change dir to '/usr/local/mysql/data/' (Errcode: 2 - No such file or directory)
这样的出错提示，说明它没找到my.cnf中的配置；而去找了程序编译时的默认安装位置：/usr/local/mysql
 
四、运行
 
【运行服务器程序】
#{mysql}/bin/mysqld_safe&
注：在这个启动脚本里已默认设置--user=mysql；在脚本末尾加&表示设置此进程为后台进程，区别就是在控制台输入bg，即可将当前进程转入后台，当前shell可进行其他操作。
【停止mysql】
{mysql}/bin/mysqladmin -uroot -p
(注意此时的root是指mysql的root用户)
 
五、设置mysql以服务运行并且开机启动
 
将{mysql}/ support-files/mysql.server 拷贝为/etc/init.d/mysql并设置运行权限
 
#cp mysql.server /etc/init.d/mysql
#chmod +x /etc/init.d/mysql
 
把mysql注册为开机启动的服务
#chkconfig --add mysql
 
当然也可以手动进行服务的开启和关闭：
#/etc/init.d/mysql start
#/etc/init.d/mysql stop
 
 
六、客户端连接测试
 
#{mysql}/bin/mysql -uroot -p
此时要求输入密码，就是前面初始化时生成的密码。
这时如果连接服务的时候出现错误：
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
则需要在在my.cnf中填加：
[client]
socket = /work/program/mysql/tmp/mysql.sock
 
连上后，在做任何操作前，mysql要求要改掉root的密码后才能进行操作。
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
mysql> alter user 'root'@'localhost' identified by 'xxxxxxx';
 
七、TIPS
 
【查看mysql是否运行】
ps -ef|grep mysqld
netstat -lnp | grep -i mysql
 
【mysql启动时读取配置文件my.cnf的顺序】
可以运行如下命令查看：
./bin/mysqld --verbose --help |more
Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf /usr/local/mysql/etc/my.cnf ~/.my.cnf
可以看到，启动时可以从上述目录下读取配置文件my.cnf。如果当前my.cnf文件不位于上述位置，则必须考过去或做链接。
