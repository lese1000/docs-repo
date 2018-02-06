ssh-keygen做密码验证可以使在向对方机器上ssh ,scp不用使用密码.

具体方法如下:
1、登录A机器
2、 ssh-keygen -t rsa   然后全部回车,采用默认值.(ssh-keygen -t [rsa|dsa]，将会生成密钥文件和私钥文件 id_rsa,id_rsa.pub或id_dsa,id_dsa.pub。)
3、将 .pub 文件复制到B机器的root下的 .ssh目录 (其他用户则在相对应的home目录，如果该目录不存在可自行创建)， 并 cat id_dsa.pub >> ~/.ssh/authorized_keys
 
3.直接运行 #ssh root@121.40.82.29

要保证.ssh和authorized_keys都只有用户自己有写权限。否则验证无效。

说明：
再更换系统时或在其他电脑进行免密登陆，只需要将密钥id_rsa拷贝到相应新系统下的.ssh/目录下即可。
