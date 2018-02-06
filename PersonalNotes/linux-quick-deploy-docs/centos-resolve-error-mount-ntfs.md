CentOS默认源里没有ntfs3g，想要添加ntfs支持，要么下载编译安装或者加源yum安装。
1.下载编译安装
下载 ntfs-3g_ntfsprogs-2013.1.13.tgz
　　转到下载的位置，
　　# tar -xzf ntfs-3g_ntfsprogs-2013.1.13.tgz
　　# cd ntfs-3g_ntfsprogs-2013.1.13
　　# ./configure
　　# make
　　# make install ntfs-3g
　　但是这个的挂载方法不是，mount -t ntfs /dev/sda1 /media 而是mount -t ntfs-3g /dev/sda1 /media

2.yum安装
1）、加源
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
(修改源记得清除旧缓存重新生成：yum clean all && yum makecached)

2）、安装
yum update;yum install ntfs-3g

说明：个人测试在安装完 ntfs-3g后不需要手动挂载，直接就可以打开移动硬盘了。

挂载方法：
1）fdisk -l 查看 ntfs格式的盘
   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1           16065   976768064   488376000    f  W95 Ext'd (LBA)
/dev/sdb5           16128   488524864   244254368+   7  HPFS/NTFS/exFAT   --ntfs
/dev/sdb6       488525824   735105023   123289600    7  HPFS/NTFS/exFAT   --ntfs
/dev/sdb7       735107072   976766975   120829952    7  HPFS/NTFS/exFAT   --ntfs

2)挂载NTFS分区

    # mount –t ntfs-3g /dev/sdb1 /mnt/usbhd

    注意:

    A、在/mnt下，需要建立文件夹usbhd（个人喜好建立）,否则会失败

    B、/dev/sdb5 ，是移动硬盘的卷名

    5、解挂

    #umount /mnt/usbhd



