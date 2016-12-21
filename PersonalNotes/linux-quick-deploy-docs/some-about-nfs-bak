mount -t nfs 121.40.212.5:/dxb_resources /home/nfsmnt
mount  121.40.212.5:/dxb_resources /home/nfsmnt

umount 121.40.212.5:/dxb_resources


开机需要启动的三个服务.
service rpcbind start
service nfs start
service autofs start

chkconfig xxx on 默认开启 2,3,4,5级别,一般只需要3,5级别即可.

[root@iZ2346usuqkZ ~]# chkconfig | grep nfs
nfs            	0:关闭	1:关闭	2:关闭	3:关闭	4:关闭	5:关闭	6:关闭
nfslock        	0:关闭	1:关闭	2:关闭	3:启用	4:启用	5:启用	6:关闭
[root@iZ2346usuqkZ ~]# chkconfig nfs on
[root@iZ2346usuqkZ ~]# chkconfig | grep nfs
nfs            	0:关闭	1:关闭	2:启用	3:启用	4:启用	5:启用	6:关闭
nfslock        	0:关闭	1:关闭	2:关闭	3:启用	4:启用	5:启用	6:关闭
[root@iZ2346usuqkZ ~]# chkconfig | grep rpcbind
rpcbind        	0:关闭	1:关闭	2:启用	3:启用	4:启用	5:启用	6:关闭
[root@iZ2346usuqkZ ~]# chkconfig | grep autofs
autofs         	0:关闭	1:关闭	2:关闭	3:启用	4:启用	5:启用	6:关闭
[root@iZ2346usuqkZ ~]# chkconfig autofs on
[root@iZ2346usuqkZ ~]# chkconfig | grep autofs
autofs         	0:关闭	1:关闭	2:启用	3:启用	4:启用	5:启用	6:关闭
[root@iZ2346usuqkZ ~]# 
