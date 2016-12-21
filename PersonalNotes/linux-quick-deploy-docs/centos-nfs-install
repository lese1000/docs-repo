http://blog.csdn.net/achilles12345/article/details/42061515 --问题
http://www.derekchou.com/blog/article/25  --简速
http://www.cnblogs.com/mchina/archive/2013/01/03/2840040.html --详细--略老


=====A主机配置 (nfs server)

yum install nfs-utils rpcbind
将会安装所需要的依赖包.包含rpcbind.
(一般分两步骤:
1.yum install rpcbind
2. yum install nfs-utils
)

注意，NFS4不同於NFS3，不再需要安裝portmap.(rpcbind的前身为portmap.  centos6.x以后为rpcbind.)
CentOS5.X下需要使用命令：yum -y install nfs-utils portmap。CentOS6.5安装的命令是：yum -y install nfs-utils rpcbind（关于yum命令，大家可以查查资料，总之其思路和maven的设计思路挺类似）。


检查是否安装：

#rpm -qa | grep rpcbind

#rpm -qa | grep nfs


编辑共享目录：

#vim /etc/exports

添加(*代表对所有主机开放)：

/home/dirk/tomcat/webapps *(rw,sync)


启动并查看状态：

#service rpcbind start

#service rpcbind status

#service nfs start

#service nfs status


重启nfs：

#service nfs restart

检查共享目录：

showmount -e



=====B主机配置(在客户端也一定要注意安装NFS)

(Ubuntu默认并不安装nfs客户端，所以首先要安装：

sudo apt-get install nfs-common )


创建文件夹用于挂载点

mkdir /home/nfsmnt

创建挂载点(不可靠,记得配置 autofs 自动挂载)

mount -t nfs 192.168.0.1:/home/dirk/tomcat/webapps/ /home/nfsmnt  

查看挂载效果的两个方法

1.df -h

2.mount -l

删除挂载点

umount /home/nfsmnt

如果在共享目录中，去删除挂载点则会提示device is busy

设置开机自启动挂载(不能解决nfs-server重启时需要重新挂载,此时需要autofs自动挂载)

#vim /etc/fstab

192.168.0.1:/home/dirk/tomcat/webapps/      /home/nfsmnt     nfs     rw     0     0

注释：NFS共享目录     本机挂载点     文件系统     权限     是否检测     检测顺序





备注说明:
需要开机启动项目有两处:
1.A主机(nfs-server) nfs 和 rpcbind 服务需要开机启动.
2.B主机(nfs-client) 需要开机自动挂载共享目录.


用 mount 方式挂载 NFS 并不是个很可靠的方式，当遇到断网或挂载时 NFS 服务器重启，都会导致客户端上的 NFS 无法访问的问题。但这也是能解决的，使用 autofs 服务的方式挂载就不会有这个问题了.





NFS的常用目录
/etc/exports                           NFS服务的主要配置文件
/usr/sbin/exportfs                   NFS服务的管理命令
/usr/sbin/showmount              客户端的查看命令
/var/lib/nfs/etab                      记录NFS分享出来的目录的完整权限设定值
/var/lib/nfs/xtab                      记录曾经登录过的客户端信息




问题:
  	 问题1：exportfs -r，这个一定要执行，否则不生效(未遇到)；

         问题2：在客户端也一定要注意安装NFS，否则光安装服务端是不能访问的。(这个需要)

         问题3：service nfs start如果出现以下问题，可以不用理会：(这个遇到)

         Starting NFS services:  [  OK  ]
         Starting NFS quotas: [  OK  ]
         Starting NFS mountd: rpc.mountd: svc_tli_create: could not open connection for udp6
         rpc.mountd: svc_tli_create: could not open connection for tcp6
         rpc.mountd: svc_tli_create: could not open connection for udp6
         rpc.mountd: svc_tli_create: could not open connection for tcp6
         rpc.mountd: svc_tli_create: could not open connection for udp6
         rpc.mountd: svc_tli_create: could not open connection for tcp6
         [  OK  ]
         Starting NFS daemon: rpc.nfsd: address family inet6 not supported by protocol TCP
         [  OK  ]
         Starting RPC idmapd: [  OK  ]






============================安装Autofs===================================
32位系统:yum install autofs
64位系统:yum install autofs.x86_64
(ubuntu系统:apt-get install autofs)


====配置::
(安装时生成四个配置文件:该提示为在ubuntu安装时打印的提示,centos无此提示)
Creating config file /etc/auto.master with new version ----主配置文件

Creating config file /etc/auto.net with new version

Creating config file /etc/auto.misc with new version  -----从配置文件(一般从配置文件自己创建,针对具体挂载类型,如auto.nfs,nfs.misc 后缀不受影响)

Creating config file /etc/auto.smb with new version

需求：在本地目录/mnt/nfs下挂载服务端NFS目录：/var/www/html/iso

操作如下：
1.在文件/etc/auto.master文件中添加
/mnt     nfs.misc   --------------注意/mnt为挂载点，不是挂载的目录的路径！！！

2.创建nfs.misc文件并且修改：
cp auto.misc nfs.misc
在文件nfs.misc中添加
nfs        -fstype=nfs      10.0.5.155:/var/www/html/iso

nfs为挂载到的目录名
-fstype=nfs可选，不写系统也可以识别出来。

3.配置完成后，重启autofs服务： service autofs restart

4.进去目录 /mnt/nfs 就可以看见被挂载的内容了！




===说明::

设置autofs开机启动,centos默认是启动的.chkconfig autofs on


