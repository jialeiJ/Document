# Linux配置MyCat

### 一、移动mycat并压缩包
	
	mv Mycat-server-1.6.7.1-release-20190627191042-linux.tar.gz /usr/local/Mycat-server-1.6.7.1-release-20190627191042-linux.tar.gz
	tar -zxvf Mycat-server-1.6.7.1-release-20190627191042-linux.tar.gz

### 二、三个配置文件

	/mycat/conf/
	
	1、schema.xml：定义逻辑库，表、分片节点等内容
	2、rule.xml：定义分片规则
	3、server.xml：定义用户以及系统相关变量，如端口等
	
### 三、编辑配置文件

	1、修改server.xml用户配置信息
	<user name="root" defaultAccount="true">
		<property name="password">root</property>
		<property name="schemas">TESTDB</property>
		
		<!-- 表级 DML 权限设置 -->
		<!-- 		
		<privileges check="false">
			<schema name="TESTDB" dml="0110" >
				<table name="tb01" dml="0000"></table>
				<table name="tb02" dml="1111"></table>
			</schema>
		</privileges>		
		 -->
	</user>
	2、修改schema.xml数据库配置信息
	<schema name="TESTDB" checkSQLschema="false" sqlMaxLimit="100" dataNode="dn1">
	</schema>
	<dataNode name="dn1" dataHost="host1" database="jaray-database" />
	
	<!-- 
		1、balance="0",不开启读写分离，所有读操作都发送到当前可用的writeHost上
		2、balance="1",全部的readHost与stand by writeHost参与select语句的负载均衡，简单的说，当双主双从模式(M1->S1,M2->S2,并且M1与M2互为主备),正常情况下,M2,S1,S2都参与select语句的负载均衡
		3、balance="2",所有读操作都随机的在writeHost、readHost上分发
		4、balance="3",所有读请求随机分发到readHost执行,writeHost不负担读压力
		
		测试读写分离：balance="2",正式：balance="1" or balance="3"
	 -->
	
	
	<dataHost name="host1" maxCon="1000" minCon="10" balance="0"
			  writeType="0" dbType="mysql" dbDriver="native" switchType="1"  slaveThreshold="100">
		<heartbeat>select user()</heartbeat>
		<!-- can have multi write hosts -->
		<writeHost host="hostM1" url="192.168.1.104:3306" user="root"
				   password="root">
			<!-- can have multi read hosts -->
			<readHost host="hostS1" url="192.168.1.105:3306" user="root" password="root" />
		</writeHost>
	</dataHost>
	3、rule.xml暂时不修改
	
### 四、远程访问

	1、在192.168.1.104下访问192.168.1.105
	mysql -uroot -proot -h 192.168.1.105 -P 3306
	
	2、在192.168.1.105下访问192.168.1.104
	mysql -uroot -proot -h 192.168.1.104 -P 3306
	
### 五、启动MyCat

	1、控制台启动：在mycqt/bin目录下执行 ./mycat console
	2、后台启动：在mycat/bin目录下 ./mycat start
		为了能看到启动日志，方便定位问题，推荐控制台启动
		
### 五、登录MyCat

	1、登录后台管理窗口(用于管理维护MyCat)
	mysql -umycat -pmycat -P 9066 -h 192.168.1.104(无法登陆)
	mysql -umycat -pmycat -P 9066 -h 192.168.1.104 --default_auth=mysql_native_password(指定加密方式)
	
	2、登录数据窗口
	mysql -umycat -pmycat -P 8066 -h 192.168.1.104(无法登陆)
	mysql -umycat -pmycat -P 8066 -h 192.168.1.104 --default_auth=mysql_native_password

### 六、验证读写分离参见Linux配置主从复制教程