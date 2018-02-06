任务						旧指令									新指令
使某服务自动启动	chkconfig [--level 3] httpd on		systemctl enable httpd.service
使某服务不自动启动	chkconfig [--level 3] httpd off		systemctl disable httpd.service
检查服务状态		service httpd status				systemctl status httpd.service （服务详细信息）
														systemctl is-active httpd.service （仅显示是否 Active)

加入自定义服务		chkconfig --add  test				systemctl   load test.service
删除服务			chkconfig --del  xxx				停掉应用，删除相应的配置文件
显示所有已启动的服务	chkconfig --list				systemctl list-units --type=service
启动某服务			service httpd start					systemctl start httpd.service
停止某服务			service httpd stop					systemctl stop httpd.service
重启某服务			service httpd restart				systemctl restart httpd.service


注：systemctl后的服务名可以到/usr/lib/systemd/system目录查看（opensuse下），其他发行版是位于/lib/systemd/system/ 下。

systemctl其是systemd包内的一个工具，也是该包中最为常用的工具。


http://www.361way.com/systemctl/3709.html