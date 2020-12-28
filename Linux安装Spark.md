# Linux安装Spark教程
	
### 一、解压
	tar -zxvf spark-2.4.4-bin-hadoop2.6.tgz
	
### 二、配置SSH无迷登录
	ssh-keygen  之后一直回车即可
	.ssh目录下 
	touch authorized_keys
	cat xxx_rsa.pub >> authorized_keys
	chmod 600 authorized_keys
	
### 三、启动master
	./sbin/start-master.sh
	
### 四、启动worker
	./bin/spark-class org.apache.spark.deploy.worker.Worker spark:localhost.localdomain:7070
	
### 五、提交作业
	./bin/spark-submit --master spark://localhost.localdomain:7070 --class Spark /usr/local/soft/Spark.jar