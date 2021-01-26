# Linux安装Maven教程

### 一、下载Maven
    http://maven.apache.org/download.cgi
    apache-maven-3.6.3-bin.tar.gz

### 二、移动到/usr/local/
	
	cp -rf /home/jaray/backup/apache-maven-3.6.3-bin.tar.gz /usr/local/apache-maven-3.6.3-bin.tar.gz

### 三、解压Maven

	tar -zxvf apache-maven-3.6.3-bin.tar.gz
	
### 四、配置环境变量

    vi /etc/profile
    
    export MAVEN_HOME=/usr/local/apache-maven-3.6.3
    export PATH=$MAVEN_HOME/bin:$PATH
	
### 五、刷新环境变量

	source /etc/profile
	
### 五、检查maven的版本

	mvn -version