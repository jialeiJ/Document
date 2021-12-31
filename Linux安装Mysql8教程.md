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
	
	# 创建数据库并导入执行sql
		mysql -u username -p;

		创建数据库
		create database database_name;

		导入表
		use database_name;
		执行sql
		source /tmp/*.sql

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
	注：若windows能ping通linux，linux能ping通windows，但是xshell连不上则需要修改网络适配器

### 十二、配置文件位置

    vi /etc/my.cnf

    添加或修改expire_logs_days的值 (这里设置的自动删除时间为10天, 默认为0不自动删除)
    expire_logs_days=10(需要重启)

    # 核心参数含义:
    innodb_buffer_pool 
    # 注：缓冲池位于主内存中，InnoDB用它来缓存被访问过的表和索引文件，使常用数据可以直接在内存中被处理，从而提升处理速度；
    innodb_buffer_pool_instance
    # 注：MySQL5.6.6之后可以调整为多个。表示InnoDB缓冲区可以被划分为多个区域，也可以理解为把innodb_buffer_pool划分为多个实例，可以提高并发性，避免在高并发环境下，出现内存的争用问题；
    innodb_data_file_path
    # 注：该参数可以指定系统表空间文件的路径和ibdata1文件的大小。默认大小是10MB，这里建议调整为1GB
    transaction_isolation
    # 注：MySQL数据库的事务隔离级别有四种，分别为READ-UNCOMMITTED、READ-COMMITTED、REPEATABLE-READ和SERIALIZABLE。默认采用REPEATABLE-READ（可重复读）
    innodb_log_buffer_size
    # 注：是日志缓冲的大小，InnoDB改变数据的时候，它会把这次改动的记录先写到日志缓冲中
    innodb_log_file_size
    # 注：是指Redo log日志的大小，该值设置不宜过大也不宜过小，如果设置太大，实例恢复的时候需要较长时间，如果设置太小，会造成redo log 切换频繁，产生无用的I/O消耗，影响数据库性能
    innodb_log_files_in_group
    # 注：redo log文件组中日志文件的数量，默认情况下至少有2个
    max_connections
    # 该参数代表MySQL数据库的最大连接数
    expire_logs_days
    # 注：该参数代表binlog的过期时间，单位是天
    slow_query_log
    # 注：慢查询日志的开关，该参数等于1代表开启慢查询
    long_query_time
    # 注：慢查询的时间，某条SQL语句超过该参数设置的时间，就会记录到慢查询日志中。单位是秒
    binlog_format
    # 注：该参数代表二进制日志的格式。binlog格式有三种statement、row和mixed。生产环境中使用row这种格式更安全，不会出现跨库复制丢数据的情况
    lower_case_table_names
    # 注：表名是否区分大小的参数。默认是值为0。0代表区分大小写，1代表不区分大小写，以小写存储
    interactive_timeout
    # 注：是服务器关闭交互式连接前等待活动的时间,默认是28800s（8小时）
    wait_timeout
    # 注：是服务器关闭非交互式连接之前等待活动的时间，默认是28800s（8小时）
    innodb_flush_method
    # 注：这个参数影响InnoDB数据文件，redo log文件的打开刷写模式
    log_queries_not_using_indexes
    # 注：如果运行的SQL语句没有使用索引，则MySQL数据库同样会将这条SQL语句记录到慢查询日志文件中
