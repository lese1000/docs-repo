centos 7 配置iptables

环境：阿里云ECS、centos 7

一、防火墙配置


1、检测并关闭firewall

systemctl status firewalld.service #检测是否开启了firewall
 
systemctl stop firewalld.service #关闭firewall
 
sytsemctl disable firewalld.service #禁止firewall开机自启

2、检测并安装iptables　

yum install iptables-services
将规则写入iptables配置文件

vi /etc/sysconfig/iptables

iptables文件内容：

*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80  -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT

3.重启：
systemctl restart iptables.service　　

4.设置开机启动
systemctl enable iptables.service　　

二、关闭SELINUX(至于为什么关闭selinux，请看知乎网友的回答:https://www.zhihu.com/question/20559538)

vi /etc/selinux/config

#SELINUX=enforcing #注释掉

#SELINUXTYPE=targeted #注释掉

SELINUX=disabled #增加

:wq! #保存退出 

setenforce 0 #使配置立即生效



http://www.cnblogs.com/qbyyqhcz/p/6007053.html