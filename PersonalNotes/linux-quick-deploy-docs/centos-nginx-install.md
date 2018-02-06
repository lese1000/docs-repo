http://blog.csdn.net/grhlove123/article/details/47834673

下载模块依赖性Nginx需要依赖下面3个包(wget + 下载地址)
1.gzip 模块需要 zlib 库 ( 下载: http://www.zlib.net/ )
2.rewrite 模块需要 pcre 库 ( 下载: http://www.pcre.org/ )--->ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/
3.ssl 功能需要 openssl 库 ( 下载: http://www.openssl.org/ )--->https://www.openssl.org/source/

依赖包安装顺序依次为:openssl、zlib、pcre, 然后安装Nginx包(如果没有安装c++编译环境，还得安装，通过yum install gcc-c++完成安装)


编译安装

openssl ：

[root@localhost] tar zxvf openssl-fips-2.0.9.tar.gz

[root@localhost] cd openssl-fips-2.0.9

[root@localhost] ./config && make && make install

 

pcre:

[root@localhost] tar zxvf pcre-8.36.tar.gz

[root@localhost] cd pcre-8.36

[root@localhost]  ./configure && make && make install

 

zlib:

[root@localhost]tar zxvf zlib-1.2.8.tar.gz

[root@localhost] cd zlib-1.2.8

[root@localhost]  ./configure && make && make install

 

最后安装nginx

[root@localhost]tar zxvf nginx-1.8.0.tar.gz

[root@localhost] cd nginx-1.8.0

[root@localhost]  ./configure && make && make install




启动命令：/usr/local/nginx/sbin/nginx
发现报错了：
error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory
经网上查询，这是linux的通病:

先找到libpcre.so.1所在位置，然后做个软链接就可以了。

64位系统:
[root@localhost nginx]# whereis libpcre.so.1
libpcre.so: /lib64/libpcre.so.0 /usr/local/lib/libpcre.so /usr/local/lib/libpcre.so.1
[root@localhost nginx]# ln -s /usr/local/lib/libpcre.so.1 /lib64  (32位的centos 连接为:ln -s /usr/local/lib/libpcre.so.1 /lib )
[root@localhost nginx]# sbin/nginx   
32位系统:
[root@iZ2346usuqkZ ~]# whereis libpcre.so.1
libpcre.so: /lib/libpcre.so.1 /lib/libpcre.so.0 /usr/local/lib/libpcre.so /usr/local/lib/libpcre.so.1
[root@iZ2346usuqkZ ~]# ln -s /usr/local/lib/libpcre.so.1 /lib
                           

查看是否已启动：
ps -aux | grep nginx




ngnix配置项目:::http://www.cszhi.com/20120513/nginx_nginx-conf.html

 server虚拟主机配置
建议将对虚拟主机进行配置的内容写进另外一个文件，然后通过include指令包含进来，这样更便于维护和管理。
	

server{
listen          80;
server_name    192.168.8.18  cszhi.com;
index index.html index.htm index.php;
root  /wwwroot/www.cszhi.com
charset gb2312;
access_log  logs/www.ixdba.net.access.log  main;
}

