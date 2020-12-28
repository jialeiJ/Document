# Linux防火墙配置教程

### 关闭firewall

	systemctl stop firewalld.service;
	systemctl disable firewalld.service;
	systemctl mask firewalld.service;

### 一、安装iptables防火墙
	
	yum -y install iptables-services

### 二、启动设置防火墙
	
	systemctl enable iptables;
	systemctl start iptables;
	
### 三、编辑防火墙，添加端口

	vi /etc/sysconfig/iptables

### 四、键入i，输入以下信息(mysql8)

	-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 8090 -j ACCEPT

### 五、重启防火墙使配置生效

	systemctl restart iptables.service

### 六、设置防火墙开机启动
	
	systemctl enable iptables.service

### 七、进入数据库登陆界面并修改密码
	
	mysql -uroot -p 
	 注：输入第六步查到的密码，进行数据库的登陆，复制粘贴就行，MySQL 的登陆密码也是不显示的
	 
	# 修改密码
 	alter user 'root'@'localhost' identified with mysql_native_password by 'root';

### 八、退出再次登录

	exit;
	
	mysql -uroot -p 

### 九、远程访问的授权
	
	create user 'root'@'%' identified with mysql_native_password by 'root';
	grant all privileges on *.* to 'root'@'%' with grant option;
	flush privileges;
	
### 十、修改加密规则(MySql8版本 和 5的加密规则不一样，而现在的可视化工具只支持旧的加密方式)

	alter user 'root'@'localhost' identified by 'root' password expire never;
	刷新权限
	flush privileges;

### 到此所有配置完成。
