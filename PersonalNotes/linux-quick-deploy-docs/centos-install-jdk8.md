1.�鿴JDK��Ϣ��
[root@localhost ~]#  rpm -qa | grep java
javapackages-tools-3.4.1-6.el7_0.noarch
tzdata-java-2014i-1.el7.noarch
java-1.7.0-openjdk-headless-1.7.0.71-2.5.3.1.el7_0.x86_64
java-1.7.0-openjdk-1.7.0.71-2.5.3.1.el7_0.x86_64
python-javapackages-3.4.1-6.el7_0.noarch

2.ж��OpenJDK��ִ�����²�����
[root@localhost ~]# rpm -e --nodeps tzdata-java-2014i-1.el7.noarch
[root@localhost ~]# rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.71-2.5.3.1.el7_0.x86_64
[root@localhost ~]# rpm -e --nodeps java-1.7.0-openjdk-1.7.0.71-2.5.3.1.el7_0.x86_64

ע�⣺��������������һ�������:

rpm -e --nodeps `rpm -qa | grep java`

3.��װ��
1��rpm -ivh jdk-8u25-linux-x64.rpm
2��tar -xzvf jdk-8u45-linux-x64.tar.gz


˵����
1.rpm��ʽ��װJDKĬ�ϰ�װ��/usr/java�С����Ҳ���Ҫ���û���������java��javacָ�������ִ�С�
2.��ѹ��ʽ�����ֶ�����jdk����������
���÷�ʽ��

���ļ�/etc/profile����׷���������ݣ�

JAVA_HOME=/usr/java/jdk1.8.0_101
JRE_HOME=/usr/java/jdk1.8.0_101/jre
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
export JAVA_HOME JRE_HOME PATH CLASSPATH


ʹ�޸���Ч
[root@localhost ~]# source /etc/profile   //ʹ�޸�������Ч
[root@localhost ~]#        echo $PATH   //�鿴PATHֵ


�鿴ϵͳ����״̬
[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin:/usr/java/jdk1.8.0_25/bin:/usr/java/jdk1.8.0_25/jre/bin



JDK��������
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-linux-x64.rpm


