1)增加用户组： sudo groupadd newgroup
2)增加用户： sudo useradd -r -g newgroup newuser

相对的删除用户组和用户： groupdel userdel

其他：
useradd 参 数 描 述
----------------------------------------------------------------------------
-c comment 给新用户添加备注
-d home_dir 为主目录指定一个名字（如果不想用登录名作为主目录名的话）
-e expire_date 用YYYYY-MM-DD格式指定一个账户过期的日期
-f inactive_days 指定这个帐户密码过期后多少天这个账户被禁用；0表示密码一过期就立即禁
用，-1表示禁用这个功能
-g initial_group 指定用户登录组的GID或组名
-G group ... 指定用户除登录组之外所属的一个或多个附加组
-k 必须和-m一起使用，将/etc/skel目录的内容复制到用户的HOME目录
-m 创建用户的HOME目录
-M 不创建用户的HOME目录（当默认设置里指定创建时，才用到）
-n 创建一个同用户登录名同名的新组
-r 创建系统账户
-p passwd 为用户账户指定默认密码
-s shell 指定默认登录shell
-u uid 为账户指定一个唯一的UID 

