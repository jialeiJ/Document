# Linux配置JDK

### 一、移动jdk并压缩包
	
	mv jdk-8u191-linux-x64.tar.gz /usr/local/jdk/jdk-8u191-linux-x64.tar.gz
	tar -zxvf jdk-8u191-linux-x64.tar.gz

### 二、配置环境变量

	vi /etc/profile
	
	#java 环境变量
	export JAVA_HOME=/usr/local/java/jdk1.8.0_201
	export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	export PATH=$PATH:$JAVA_HOME/bin
	
### 三、重新加载
	
	source /etc/profile
