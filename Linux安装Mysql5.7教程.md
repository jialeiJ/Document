# Linux安装Mysql版本8教程

### 查看是否安装

	rpm -qa | grep mysql

### 一、移动到文件夹
	
	/usr/local/

### 二、解压到文件
	
	tar -zxvf mysql-5.7.28-linux-glibc2.12-x86_64.tar.gz
	
### 三、在mysql文件夹下创建data文件夹
	
	mkdir /usr/local/soft/mysql-5.7.28-linux-glibc2.12-x86_64/data

### 四、更改mysql目录下所有的目录及文件夹所属的用户组和用户，以及权限

	chown -R mysql:mysql /usr/local/soft/mysql-5.7.28-linux-glibc2.12-x86_64
	chmod -R 755 /usr/local/soft/mysql-5.7.28-linux-glibc2.12-x86_64

### 五、编译安装并初始化mysql,务必记住初始化输出日志末尾的密码（数据库管理员临时密码）

	cd /usr/local/soft/mysql-5.7.28-linux-glibc2.12-x86_64/bin
	根据配置文件初始化
	./mysqld --defaults-file=/etc/my.cnf --initialize-insecure --user=mysql
	
	配置文件如下：
	[mysqld]
	basedir=/usr/local/soft/mysql-5.7.28-linux-glibc2.12-x86_64
	datadir=/usr/local/soft/mysql-5.7.28-linux-glibc2.12-x86_64/data
	sql_mode=NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
	default-storage-engine=INNODB
	character_set_server=utf8mb4
	#collation-server=utf8_general_ci
	explicit_defaults_for_timestamp=true
	port=3306
	symbolic-links=0
	max_connections=400
	innodb_file_per_table=1
	#表名大小写不敏感
	lower_case_table_names=1
	
	[client]
	# mysql客户端默认编码
	default-character-set = utf8mb4
	

	记录日志最末尾位置root@localhost:后的字符串，此字符串为mysql管理员临时登录密码
	密码：root@localhost: D2oogQy%RUu:
### 六、启动mysql服务
	
	./usr/local/soft/mysql-5.7.28-linux-glibc2.12-x86_64/support-files/mysql.server start

### 五、添加软连接
	
	 ln -s /usr/local/soft/mysql-5.7.28-linux-glibc2.12-x86_64/support-files/mysql.server /etc/init.d/mysql
	 ln -s /usr/local/soft/mysql-5.7.28-linux-glibc2.12-x86_64/bin/mysql /usr/bin/mysql

### 六、修改初始化密码
	
	mysql -u root -p
	输入初始密码
	
	# 修改密码
 	alter user 'root'@'localhost' identified by 'xxxxx';

### 七、停止服务
	net stop mysql
	# 卸载服务
	mysqld -remove

### 到此所有配置完成。
