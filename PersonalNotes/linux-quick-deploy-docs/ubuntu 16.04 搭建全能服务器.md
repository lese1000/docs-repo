本文教你如何在 Ubuntu 16.04 上安装 Apache、PHP、MySQL、PureFTPD、BIND、Postfix、Dovecot 和 ISPConfig 3.1 搭建一个网站、邮件、邮件列表、DNS和FTP服务器。ISPConfig 3是一个虚拟主机控制面板，使您可以通过网络浏览器配置以下服务：Apache 或 nginx web 服务器，Postfix 电子邮件服务，Courier 或 Dovecot IMAP/POP3 服务，MySQL，BIND 或 MyDNS 域名服务，PureFTPd，SpamAssassin，ClamAV，等等和更多的服务。

注意：本教程中使用的ISPConfig 3.1版目前正处于测试状态下，ISPConfig 3.1 最终将在2016年6月发布，旧的 ISPConfig old stable 3.0.5p9 无法在 Ubuntu 16.04 中使用，而且不兼容PHP 7。

1.初步说明

 在本教程中，使用的IP地址为192.168.1.100和网关192.168.1.1主机名server1.example.com。这些设置可能与你的不同，所以你必须根据你的情况更换。进一步讨论之前，你需要有一个基本的最小安装。

2. 编辑 /etc/apt/sources.list 并更新 Linux。

编辑/etc/apt/sources.list。注释掉或从文件中删除安装光盘，并确保库启用。应该是这样设置：

nano /etc/apt/sources.list

内容如下：

#

# deb cdrom:[Ubuntu-Server 16.04 LTS _Xenial Xerus_ – Release amd64 (20160420)]/ xenial main restricted

#deb cdrom:[Ubuntu-Server 16.04 LTS _Xenial Xerus_ – Release amd64 (20160420)]/ xenial main restricted

# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://de.archive.ubuntu.com/ubuntu/ xenial main restricted
# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial main restricted

## Major bug fix updates produced after the final release of the
## distribution.
deb http://de.archive.ubuntu.com/ubuntu/ xenial-updates main restricted
# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial-updates main restricted

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team, and may not be under a free licence. Please satisfy yourself as to
## your rights to use the software. Also, please note that software in
## universe WILL NOT receive any review or updates from the Ubuntu security
## team.
deb http://de.archive.ubuntu.com/ubuntu/ xenial universe
# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial universe
deb http://de.archive.ubuntu.com/ubuntu/ xenial-updates universe
# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial-updates universe

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team, and may not be under a free licence. Please satisfy yourself as to
## your rights to use the software. Also, please note that software in
## multiverse WILL NOT receive any review or updates from the Ubuntu
## security team.
deb http://de.archive.ubuntu.com/ubuntu/ xenial multiverse
# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial multiverse
deb http://de.archive.ubuntu.com/ubuntu/ xenial-updates multiverse
# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial-updates multiverse

## N.B. software from this repository may not have been tested as
## extensively as that contained in the main release, although it includes
## newer versions of some applications which may provide useful features.
## Also, please note that software in backports WILL NOT receive any review
## or updates from the Ubuntu security team.
deb http://de.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse
# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse
## Uncomment the following two lines to add software from Canonical’s
## ‘partner’ repository.
## This software is not part of Ubuntu, but is offered by Canonical and the
## respective vendors as a service to Ubuntu users.
# deb http://archive.canonical.com/ubuntu xenial partner
# deb-src http://archive.canonical.com/ubuntu xenial partner

deb http://security.ubuntu.com/ubuntu xenial-security main restricted
# deb-src http://security.ubuntu.com/ubuntu xenial-security main restricted
deb http://security.ubuntu.com/ubuntu xenial-security universe
# deb-src http://security.ubuntu.com/ubuntu xenial-security universe
deb http://security.ubuntu.com/ubuntu xenial-security multiverse
# deb-src http://security.ubuntu.com/ubuntu xenial-security multiverse

然后运行：

apt-get update

更新apt软件包和数据库：

apt-get upgrade

安装最新的更新（如果有的话）。如果你看到一个新的内核被安装作为更新的一部分，重新引导系统：

reboot

3.更改默认的shell

dpkg-reconfigure dash

Use dash as the default system shell (/bin/sh)?

如果你不这样做，ISPConfig安装会失败。

--------------------------------------分割线 --------------------------------------

Ubuntu 16.04 LTS正式发布下载，长达5年技术支持  http://www.linuxidc.com/Linux/2016-04/130508.htm

Ubuntu 16.04 U盘安装图文教程 http://www.linuxidc.com/Linux/2016-04/130520.htm

Ubuntu 16.04 LTS安装好需要设置的15件事 http://www.linuxidc.com/Linux/2016-04/130519.htm

Ubuntu 16.04 LTS 今日发布 Canonical公布系统新特性 http://www.linuxidc.com/Linux/2016-04/130466.htm

将Ubuntu 15.10升级到Ubuntu 16.04  http://www.linuxidc.com/Linux/2016-03/129158.htm

Ubuntu 16.04安装Lua游戏引擎Love http://www.linuxidc.com/Linux/2016-03/129108.htm

Ubuntu 16.04 LTS如何使用Snap软件包 http://www.linuxidc.com/Linux/2016-04/130679.htm

Ubuntu 16.04 需要你的帮助，让 GNOME Software 更美观 http://www.linuxidc.com/Linux/2016-03/129237.htm

--------------------------------------分割线 --------------------------------------

4.禁用 AppArmor

AppArmor的是一个安全扩展（类似于SELinux）的应提供扩展的安全性。在我看来，你不需要它来配置一个安全的系统，它通常会导致更多的问题比优点（想想看你做了一个星期后，故障排除，因为预期有些服务不能正常工作，然后你发现一切正常，只是AppArmor配置是造成问题）。因此，我禁用它（这是必须的，如果你想稍后安装ISPConfig）。
我们可以像这样禁用它：

service apparmor stop
update-rc.d -f apparmor remove
apt-get remove apparmor apparmor-utils

5.同步系统时钟
这是当你运行一个物理服务器系统时钟在互联网上的NTP（网络时间协议）服务器同步是一个好主意。如果你运行一个虚拟服务器，那么你应该跳过此步骤。 运行：

apt-get -y install ntp ntpdate

和你的系统时间就会始终保持同步。

6. 安装 Postfix, Dovecot, MariaDB, phpMyAdmin, rkhunter 和 binutils

对于安装postfix，我们需要确保的sendmail未安装和运行。要停止并删除sendmail的运行以下命令：

service sendmail stop; update-rc.d -f sendmail remove

错误信息：

Failed to stop sendmail.service: Unit sendmail.service not loaded.

确定的，它只是意味着没有安装sendmail的，所以没有什么被删除。
现在我们可以安装Postfix，Dovecot，MariaDB（如MySQL的替代品），rkhunter和binutil用一个命令：

apt-get install postfix postfix-mysql postfix-doc mariadb-client mariadb-server openssl getmail4 rkhunter binutils dovecot-imapd dovecot-pop3d dovecot-mysql dovecot-sieve sudo

你会被问以下几个问题：

General type of mail configuration: System mail name:

您使用一个子域作为要为以后的电子邮件域名（例如yourdomain.tld）使用“系统邮件名称为”像server1.example.com或server1.yourdomain.com，域名不是非常重要的。
接下来，打开后缀的TLS/ SSL和提交端口：

nano /etc/postfix/master.cf

取消对提交和SMTPS部分如下： – 添加一行-o

smtpd_client_restrictions=permit_sasl_authenticated,reject 两行设置：

[...]
submission inet n - - - - smtpd
-o syslog_name=postfix/submission
-o smtpd_tls_security_level=encrypt
-o smtpd_sasl_auth_enable=yes
-o smtpd_client_restrictions=permit_sasl_authenticated,reject
# -o smtpd_reject_unlisted_recipient=no
# -o smtpd_client_restrictions=$mua_client_restrictions
# -o smtpd_helo_restrictions=$mua_helo_restrictions
# -o smtpd_sender_restrictions=$mua_sender_restrictions
# -o smtpd_recipient_restrictions=permit_sasl_authenticated,reject
# -o milter_macro_daemon_name=ORIGINATING
smtps inet n - - - - smtpd
-o syslog_name=postfix/smtps
-o smtpd_tls_wrappermode=yes
-o smtpd_sasl_auth_enable=yes
-o smtpd_client_restrictions=permit_sasl_authenticated,reject
# -o smtpd_reject_unlisted_recipient=no
# -o smtpd_client_restrictions=$mua_client_restrictions
# -o smtpd_helo_restrictions=$mua_helo_restrictions
# -o smtpd_sender_restrictions=$mua_sender_restrictions
# -o smtpd_recipient_restrictions=permit_sasl_authenticated,reject
# -o milter_macro_daemon_name=ORIGINATING
[...]

注：空格在前面的“-o……”行很重要！

重新启动 Postfix：

service postfix restart

我们希望MySQL监听所有的接口，而不仅仅是本地主机。因此，我们编辑：

/etc/mysql/mariadb.conf.d/50-server.cnf 并注释掉线 bind-address = 127.0.0.1:

nano /etc/mysql/mariadb.conf.d/50-server.cnf

[...]
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
#bind-address = 127.0.0.1
[...]

现在，我们在MariaDB设置root密码。 运行：

mysql_secure_installation

将会被问以下问题：

Enter current password for root (enter for none): <– press enter
Set root password? [Y/n] <– y
New password: <– Enter the new MariaDB root password here
Re-enter new password: <– Repeat the password
Remove anonymous users? [Y/n] <– y
Disallow root login remotely? [Y/n] <– y
Reload privilege tables now? [Y/n] <– y

重启 MariaDB:

service mysql restart

现在检查联网启用。运行：

netstat -tap | grep mysql

输出应该是这样的：

root@server1:~# netstat -tap | grep mysql
tcp6 0 0 [::]:mysql [::]:* LISTEN 5230/mysqld
root@server1:~#

7. 安装 Amavisd-new, SpamAssassin, 和 Clamav

安装 amavisd-new, SpamAssassin, 和 ClamAV, 运行命令：

apt-get install amavisd-new spamassassin clamav clamav-daemon zoo unzip bzip2 arj nomarch lzop cabextract apt-listchanges libnet-ldap-perl libauthen-sasl-perl clamav-docs daemon libio-string-perl libio-socket-ssl-perl libnet-ident-perl zip libnet-dns-perl postgrey

ISPConfig3设置使用的amavisd哪些负载，然后SpamAssassin过滤库内部，所以我们可以停止的SpamAssassin释放一些内存：

service spamassassin stop
update-rc.d -f spamassassin remove

编辑ClamAV的配置文件：

nano /etc/clamav/clamd.conf

修改行：

AllowSupplementaryGroups false

为：

AllowSupplementaryGroups true

保存文件。要开始使用ClamAV：

freshclam
service clamav-daemon start

下面的警告可以freshclam的第一次运行，我们开始clamd的守护程序后，我们更新了数据库被忽略。

WARNING: Clamd was NOT notified: Can't connect to clamd through /var/run/clamav/clamd.ctl: No such file or directory

7.1安装 Metronome XMPP服务器（可选）

Metronome XMPP服务器提供了一个XMPP聊天服务器。这一步是可选的，如果你并不需要一个聊天服务器，那么你可以跳过这一步。没有其他ISPConfig功能取决于该软件。
使用apt安装以下软件包。

apt-get install git lua5.1 liblua5.1-0-dev lua-filesystem libidn11-dev libssl-dev lua-zlib lua-expat lua-event lua-bitop lua-socket lua-sec luarocks luarocks

luarocks install lpc

为 Metronome 添加一个shell用户

adduser --no-create-home --disabled-login --gecos 'Metronome' metronome

下载 Metronome /opt目录并编译它。

cd /opt; git clone https://github.com/maranda/metronome.git metronome
cd ./metronome; ./configure --ostype=debian --prefix=/usr
make
make install

Metronome 现在已经安装到 /opt/metronome.

8. 安装 Apache, PHP, phpMyAdmin, FCGI, SuExec, Pear, 和 mcrypt：

apt-get install apache2 apache2-doc apache2-utils libapache2-mod-php php7.0 php7.0-common php7.0-gd php7.0-mysql php7.0-imap phpmyadmin php7.0-cli php7.0-cgi libapache2-mod-fcgid apache2-suexec-pristine php-pear php-auth php7.0-mcrypt mcrypt imagemagick libruby libapache2-mod-python php7.0-curl php7.0-intl php7.0-pspell php7.0-recode php7.0-sqlite3 php7.0-tidy php7.0-xmlrpc php7.0-xsl memcached php-memcache php-imagick php-gettext

您将看到以下问题：

Web server to reconfigure automatically: <-- apache2 Configure database for phpmyadmin with dbconfig-common? <-- Yes MySQL application password for phpmyadmin: <-- Press enter 使用以下命令配置 Apache 模块： a2enmod suexec rewrite ssl actions include cgi

a2enmod dav_fs dav auth_digest headers

重启 apache2：

service apache2 restart

如果你想通过ISPConfig创建您的网站扩展.RB Ruby文件，则必须注释掉/etc/mime.types行，运行：

nano /etc/mime.types

[...]
#application/x-ruby rb
[...]

service apache2 restart

8.1 安装 PHP Opcode cache

apt-get install php7.0-opcache php-apcu

service apache2 restart

8.2 安装 PHP-FPM

apt-get install libapache2-mod-fastcgi php7.0-fpm
a2enmod actions fastcgi alias
service apache2 restart

8.3其他PHP版本

有可能有一个服务器（通过ISPConfig可选），它可以通过的FastCGI和PHP-FPM运行在多个PHP版本。要了解如何构建额外的PHP版本（PHP-FPM和FastCGI），以及如何配置ISPConfig，请查看本教程：如何使用多个PHP版本（PHP-FPM＆的FastCGI）随着ISPConfig3（Ubuntu的12.10）（适用于Ubuntu的16.04为好）。

10.1 安装HHVM（HipHop虚拟机）

sudo apt-get install hhvm

9. 安装 Let’s Encrypt

apt-get install git

cd /opt
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt/

sudo -H ./letsencrypt-auto --help
