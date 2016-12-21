文档地址：http://www.linuxidc.com/Linux/2014-09/106682.htm

（svn已经包含客户端和服务端）
SVN添加文件时的错误处理：...\conf\svnserve.conf:12: Option expected  http://www.linuxidc.com/Linux/2014-09/106683.htm

本文是小编亲自整理、测试、验证过的方法，也可以算是最全、最简易的SVN安装配置方法！
下面文档分为四个部分：

    1、在Ubuntu 14.0.4系统中安装SVN

    2、配置SVN

    3、启动和关闭svnservice

    4、简要的使用SVN 

一、在Ubuntu14.0.4 中安装SVN

1、首先，需要准备好软件工具，Ubuntu中安装软件十分方便

$sudo apt-get install subversion 

Ok 安装完毕

2、建立仓库文件夹

cd  /home 
sudo  mkdir svn 
/usr/local/svn# sudo chown -R 777 svn 
/usr/local/svn# sudo chmod -R 777 svn 

最后的一条命令赋予组成员对所有新加入文件仓库的文件拥有相应的权限。正常情况下应该是添加相应的组权限的，但是既然是针对于新手段white paper,所以才简单点，就省略了那么一点，呵呵 

3、创建仓库

$ sudo svnadmin create /home/svn
二、 配置SVN

1、配置/home/svn/conf目录下的 svnserve.conf文件

    修改svnserve.conf文件

    a、去掉#[general]前面的#号

[general]

b、#匿名访问的权限，可以使read/write/none,默认是read

anon-access = none

c、#认证用户的权限，可以使read/write/none，默认为write

auth-access = write

d、#密码数据库的路径，去掉前面的#

passw-db = passwd

注意：其中 anon-access 和auth-access 分别为匿名和有权限用户的权限，默认给匿名用户只读的权限,但如果想拒绝匿名用户的访问，只需把 read 改成 none 就能达到目的。

2、修改/home/svn/conf目录下的配置文件passwd 文件

如修改成

[users]
root    =    123
hfkj    =    12345678
test1  =    test1
test2  =    test2

注意：这里设置了四个用户root,hfkj,test1，test2，密码分别为123,12345678，test1,test2

3、修改 /home/svn/conf/ 目录下的  配置文件authz  如下：

注意：这里设置了四个用户root,hfkj,test1，test2,密码分别为。。。

其中root,hfkj数据admin组，有读和写的权限；test1和test2数据test组只有读的权限。

三、启动与关闭SVN服务,并导入工程

1、启动 svnserve 服务

$sudo svnserve  –d  –r  /home/svn

描述说明：

    -d: 表示 svnserve 以“守护”进程模式运行

    -r: 指定文件系统的根位置（版本库的根目录），这样客户端不用输入全路径，就可以访问版本库：如：svn://192.168.12.118/svn

2、然后导入 svn  工程

#sudo  svn import -m "New import"  /home/test  svn://localhost/svn

这样 /home/test 文件夹下的项目就导入到了 svn 中了，而 –m 参数的意思是：message 也就是今后查看svn log 时候看到的东东了。

3、之后，我们测试一下

$ sudo mkdir  /home/svn_down
$ sudo cd    /home/svn_down
$ sudo svn  co  svn://localhost/svn

4、看一下 SVN中的文件是否 被下载下来了？

5、OK 这就是以上的全部过程啦！其实是非常简单的！

 

注意：查看 svnserve 服务是否启动

      $ sduo ps  -ef |  grep svn

          关闭服务

       $ Kill -9 pid    :pid  即svnserve  的PID

    亦或是  $  sudo  killall  svn  也是可以的

然后再使用$ sduo  ps  -ef | grep svn  命令查看 svnserve 是否已经被关闭啦！ 

总结：

本文是在综合了网上众多的 Ubuntu下安装、配置SVN后，经过小编亲自测试、验证后的经验总结，系统对于仍处于迷茫中的小伙伴们有所帮助，尽快能够愉快地玩耍起来。

上传的文件放在SVN服务器的哪个目录下

SVN服务器版本库有两种格式,
一种为FSFS,
一种为BDB
把文件上传到SVN版本库后,上传的文件不再以文件原来的格式存储,而是被svn以它自定义的格式压缩成版本库数据,存放在版本库中。
如果是FSFS格式，这些数据存放在版本库的db目录中，里面的revs和revprops分别存放着每次提交的差异数据和日志等信息

Linux中Subversion配置实例 http://www.linuxidc.com/Linux/2012-02/53109.htm

CentOS 6.2 SVN搭建 (YUM安装) http://www.linuxidc.com/Linux/2013-10/91903.htm

Apache+SVN搭建SVN服务器 http://www.linuxidc.com/Linux/2013-03/81379.htm

Windows下SVN服务器搭建和使用 + 客户端重新设置密码 http://www.linuxidc.com/Linux/2013-05/85189p5.htm

Ubuntu Server 12.04 安装 SVN 并迁移 Virtual SVN数据 http://www.linuxidc.com/Linux/2013-05/84695.htm

Ubuntu Server搭建svn服务以及迁移方法 http://www.linuxidc.com/Linux/2013-05/84693.htm

借助网盘搭建SVN服务器 http://www.linuxidc.com/Linux/2013-10/91271.htm
