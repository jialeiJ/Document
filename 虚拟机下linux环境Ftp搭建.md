# 虚拟机下linux环境Ftp搭建

### 1、安装linux系统完成后，打开终端

* [执行sudo apt-get update(每次安装前执行，不行再执行sudo apt-get upgrade)]
* [执行 sudo intall apt net-tools]
* [输入ifconfig即可查看到ip地址]

### 2、安装SSH服务

* [sudo apt-get install openssh-server(安装完默认启动)]
* [service sshd restart(启动服务)    service sshd stop(停止服务)]
* [netstat –nat|grep 22(查看进程(端口)是否启动)]

### 3、关闭防火墙

* [sudo ufw disable(每次重启都要关闭防火墙，关闭后能看出SSH服务)]

### 4、设置开机自启

* [chkconfig sshd on(设置SSH服务开机自启)    chkconfig sshd on(禁止SSH服务开机自启)]

### 5、必须
* [ssh ip(ssh 192.168.2.103)]

### 6、安装FTP服务器

* [sudo apt-get update(sudo apt-get upgrade)(执行)]
* [sudo apt-get install vsftp(安装vsftp)]
* [sudo service vsftpd restart(判断vsftpd是否安装成功)]
* [sudo mkdir /home/uftp(新建/home/uftp目录)]
* [sudo ls /home(查看新建uftp目录是否成功)]
* [sudo useradd –d /home/uftp –s /bin/bash uftp(新建用户uftp)]
* [sudo passwd uftp(为uftp目录设置密码)]
* [sudo vi /etc/vsftpd.conf(使用gedit修改vsftpd.conf配置文件)]
		
		项文件中添加userlist_deny=No
             userlist_enable=YES
             userlist_file=/etc/allowed_users
             seccomp_sandbox=No
             local_enable=YES
		
* sudo touch /etc/allowed_users(使用touch新建/etc/allowed_users文件)
* vi /etc/ftpusers(查看ftpusers文件中有uftp用户名吗，有删除，没有退出，文件中记录不能访问的用户清单)
* xftp连接FTP服务器(ifconfig可查看ip地址  用户名uftp  密码000000)
	
