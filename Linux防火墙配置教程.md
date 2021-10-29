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

