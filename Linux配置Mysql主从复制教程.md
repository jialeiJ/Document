# Linux配置Mysql主从复制教程

### 一、主机配置(host:104)
	
	1、修改配置文件 vi /etc/my.cnf
	# 主服务器唯一id
	server-id=1
	# 启用二进制日志
	log-bin=mysql-bin
	# 设置不要复制的数据库(可设置多个)
	binlog-ignore-db=mysql
	binlog-ignore-db=information_schema
	# 设置需要复制的数据库
	#binlog-do-db=jaray-database
	# 设置logbin格式
	binlog_format=STATEMENT
	

### 二、从机配置(host:105)
	
	1、修改配置文件 vi /etc/my.cnf
	# 从服务器唯一id
	server-id=2
	# 启用中继日志
	relay-log=mysql-relay
	
### 三、重启服务

	systemctl restart mysqld
	
### 四、关闭防火墙

	
### 五、主机建立用于复制的账户并授权

	1、登录主机
	mysql -uroot -p
	2、创建用户
	create user 'repl_user'@'%' identified with mysql_native_password by 'repl_user';
	3、授权
	grant replication slave,file on *.* to 'repl_user'@'192.168.1.105';

	# 或者对现有的超级用户修改可访问主机IP：
	update user set host='%' where user='root';　　　　# '%' 代表所有局域IP
	
	4、刷新权限
	flush privileges;

### 六、从机配置需要复制的主机

	1、登录从机
		mysql -uroot -p
	2、从机配置
	# 这里的 master日志文件和位置必须与主服务器当前状态一致！(show master status)
		change master to master_host='192.168.1.104',master_port=3306,
		master_user='repl_user',master_password='repl_user',
		master_log_file='mysql-bin.000001',master_log_pos=2216;
	
		# 如果之前搭建过主从复制则执行如下命令：
			(1)、stop slave;
			(2)、reset master;
		
	3、启动
		start slave;

### 七、查看是否成功
	
	show slave status\G;
		
		Slave_IO_Running: Yes
        Slave_SQL_Running: Yes
        
### 八、Mysql 主从同步失效的解决方法
	
	方法一
		登录从机
		mysql -uroot -p
		1、停止mysql从服务slave
			stop slave;
		2、设置跳过1个事务
			set global sql_slave_skip_counter=1;
		3、开启slave
			start slave;
	方法二
		登录从机
		mysql -uroot -p
		1、停止mysql从服务slave
			stop slave;
		2、主 mysql服务器上查看主机状态，记录 File 和 Position 对应的值
			show master status;
		3、从 mysql 服务器上执行手动同步
			change master to master_host='192.168.1.104',master_port=3306,
			master_user='repl_user',master_password='repl_user',
			master_log_file='mysql-bin.000001',master_log_pos=2216;
		4、启动
			slave start;
		5、查看是否成功
			show slave status\G;
			
				Slave_IO_Running: Yes
        		Slave_SQL_Running: Yes

### 九、验证读写分离

	1、写主机插入
		insert into complaint('cid', 'title', 'desc', 'cFileIds', 'createTime', 'updateTime') values (null, @@hostname, null, null, null, null);
	2、mycat查询
		select * from complaint;