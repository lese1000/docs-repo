1.查看JDK信息：
[root@localhost ~]#  rpm -qa | grep java
javapackages-tools-3.4.1-6.el7_0.noarch
tzdata-java-2014i-1.el7.noarch
java-1.7.0-openjdk-headless-1.7.0.71-2.5.3.1.el7_0.x86_64
java-1.7.0-openjdk-1.7.0.71-2.5.3.1.el7_0.x86_64
python-javapackages-3.4.1-6.el7_0.noarch

2.卸载OpenJDK，执行以下操作：
[root@localhost ~]# rpm -e --nodeps tzdata-java-2014i-1.el7.noarch
[root@localhost ~]# rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.71-2.5.3.1.el7_0.x86_64
[root@localhost ~]# rpm -e --nodeps java-1.7.0-openjdk-1.7.0.71-2.5.3.1.el7_0.x86_64

注意：上面这两步可以一次性完成:

rpm -e --nodeps `rpm -qa | grep java`

3.安装：
1）rpm -ivh jdk-8u25-linux-x64.rpm
2）tar -xzvf jdk-8u45-linux-x64.tar.gz


说明：
1.rpm方式安装JDK默认安装在/usr/java中。而且不需要设置环境变量，java，javac指令可正常执行。
2.加压方式必须手动配置jdk环境变量。
配置方式：

向文件/etc/profile里面追加以下内容：

JAVA_HOME=/usr/java/jdk1.8.0_101
JRE_HOME=/usr/java/jdk1.8.0_101/jre
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
export JAVA_HOME JRE_HOME PATH CLASSPATH


使修改生效
[root@localhost ~]# source /etc/profile   //使修改立即生效
[root@localhost ~]#        echo $PATH   //查看PATH值


查看系统环境状态
[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin:/usr/java/jdk1.8.0_25/bin:/usr/java/jdk1.8.0_25/jre/bin



JDK官网下载
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-linux-x64.rpm


