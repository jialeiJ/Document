# Linux安装Nacos-Server教程
	
### 一、移动到usr/local/目录下解压
	tar -zxvf nacos-server-1.4.0.tar.gz
	
### 二、建nacos数据库
	执行解压出来的nacos-mysql.sql文件
	/usr/local/nacos/conf/nacos-mysql.sql
	
### 三、配置nacos数据库
	/usr/local/nacos/conf/application.properties
	配置信息如下：
	
	#*************** Config Module Related Configurations ***************#
    ### If use MySQL as datasource:
    spring.datasource.platform=mysql
    
    ### Count of DB:
    db.num=1
    
    ### Connect URL of DB:
    db.url.0=jdbc:mysql://192.168.1.104:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
    db.user=****
    db.password=****
	
### 四、启动
	1、单机启动
	    nohup sh /usr/local/nacos/bin/startup.sh -m standalone &
	2、单机关闭
		sh /usr/local/nacos/bin/shutdown.sh