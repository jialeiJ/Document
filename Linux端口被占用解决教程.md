# Linux端口被占用解决教程

### 一、查看端口使用情况
	
	netstat -tunlp
    netstat -tunlp | grep 端口

### 二、查看单个端口使用情况

	netstat -tunlp | grep 端口
	
### 三、kill命令杀死进程
	
	kill -9 PID
	
### 注意：无法看到PID请切换root用户
