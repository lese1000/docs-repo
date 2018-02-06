/etc/fstab

# <file system>        <dir>         <type>    <options>             <dump> <pass>
tmpfs                  /tmp          tmpfs     nodev,nosuid          0      0
/dev/sda1              /             ext4      defaults,noatime      0      1
/dev/sda2              none          swap      defaults              0      0
/dev/sda3              /home         ext4      defaults,noatime      0      2




192.168.0.1:/home/dirk/tomcat/webapps/      /home/nfsmnt     nfs     rw     0     0
