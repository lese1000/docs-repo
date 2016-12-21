远程拷贝：
（1）拷贝文件夹
scp -r 本地文件夹路径  远程用户名@IP地址:路径
（2）拷贝文件(区别，没有参数 -r)
scp 本地文件路径 远程用户名@IP地址：路径

反过来:远程拷贝文件夹到本地
scp -r 远程用户名@IP地址:路径 本地路径

连接方式：
ssh root@42.96.170.210

ssh登录不需要密码方式:
ssh-keygen

1.ssh-keygen -t [rsa|dsa]，将会生成密钥文件和私钥文件 id_rsa,id_rsa.pub或id_dsa,id_dsa.pub(提示输入密码时不输入,直接回车.否则登录时还需要密码)
2.将 .pub 文件复制到服务器的root(其他用户则在相对应的home目录)下的 .ssh 目录(如果该目录不存在可自行创建)， 
并 cat id_dsa.pub >> ~/.ssh/authorized_keys
3.直接运行 #ssh root@121.40.82.29


采用public key登录：
openssh的ssh-keygen命令用来产生这样的私钥和公钥。
[root@mail ~]# ssh-keygen -b 1024 -t dsa -C gucuiwen@myserver.com
Generating public/private dsa key pair.
#提示正在生成，如果选择4096长度，可能需要较长时间
Enter file in which to save the key (/root/.ssh/id_dsa): 
＃询问把公钥和私钥放在那里，回车用默认位置即可
Enter passphrase (empty for no passphrase): 
＃询问输入私钥密语，为了实现自动登陆，应该不要密语，直接回车
Enter same passphrase again: 
＃再次提示输入密语，再次直接回车
Your identification has been saved in /root/.ssh/id_dsa.
Your public key has been saved in /root/.ssh/id_dsa.pub.
＃提示公钥和私钥已经存放在/root/.ssh/目录下
The key fingerprint is:
71:e5:cb:15:d3:8c:05:ed:05:84:85:32:ce:b1:31:ce gucuiwen@myserver.com
＃提示key的指纹
