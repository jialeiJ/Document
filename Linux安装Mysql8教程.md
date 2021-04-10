# Linux安装Mysql版本8教程

### 查看是否安装包

	rpm -qa | grep mysql
	rpm -qa | grep mariadb 
	卸载mariadb
	rpm -e [name] --nodeps

### 一、移动到文件夹并创建mysql8文件夹
	
	cd /usr/local/
	mkdir mysql8

### 二、解压到文件
	
	tar -xvf mysql-8.0.19-1.el7.x86_64.rpm-bundle.tar
	
### 三、清屏

	clear

### 四、安装
	1、安装 common
		rpm -ivh mysql-community-common-8.0.19-1.el7.x86_64.rpm --nodeps --force
	2、安装 libs
		rpm -ivh mysql-community-libs-8.0.19-1.el7.x86_64.rpm --nodeps --force
	3、安装 client
		rpm -ivh mysql-community-client-8.0.19-1.el7.x86_64.rpm --nodeps --force
	4、安装 server
		rpm -ivh mysql-community-server-8.0.19-1.el7.x86_64.rpm --nodeps --force

### 五、mysql8数据库的初始化和相关配置

	mysqld --initialize;
	chown mysql:mysql /var/lib/mysql -R;
	systemctl start mysqld.service;
	systemctl enable mysqld;

### 六、查看数据库的密码
	
	cat /var/log/mysqld.log | grep password

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
	
### 十一、每次更换新网络后操作

	需要在虚拟网络编辑器中还原默认设置(VMnet8)
	修改/etc/sysconfig/network-scripts/ifcfg-ens32的IPADDR与GATEWAY(IPADDR与GATEWAY前三段需与VMnet8还原默认设置后保持一致)
