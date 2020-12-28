# linux下安装部署Redis

### 1、解压缩

*   tar xzf redis-3.0.5.tar.gz

### 2、编译、安装

*   cd redis-3.0.5
*   make
*   make install
*   cp redis.conf /etc/

### 3、启动redis

*   cd /usr/local/bin
*   /redis-server & /etc/redis.conf

### 4、检查是否启动成功

*   ps -ef | grep redis

### 5、必须
*   ssh ip  eg：ssh 192.168.2.103

### 6、安装FTP服务器

*   sudo apt-get update(sudo apt-get upgrade)(执行)
*   sudo apt-get install vsftp(安装vsftp)
*   sudo service vsftpd restart(判断vsftpd是否安装成功)
*   sudo mkdir /home/uftp(新建/home/uftp目录)
*   sudo ls /home(查看新建uftp目录是否成功)
*   sudo useradd –d /home/uftp –s /bin/bash uftp(新建用户uftp)
*   sudo passwd uftp(为uftp目录设置密码)
*   sudo vi /etc/vsftpd.conf(使用gedit修改vsftpd.conf配置文件)
		
        项文件中添加userlist_deny=No
             userlist_enable=YES
             userlist_file=/etc/allowed_users
             seccomp_sandbox=No
             local_enable=YES
		
*   sudo touch /etc/allowed_users(使用touch新建/etc/allowed_users文件)
*   vi /etc/ftpusers(查看ftpusers文件中有uftp用户名吗，有删除，没有退出，文件中记录不能访问的用户清单)
*   xftp连接FTP服务器(ifconfig可查看ip地址  用户名uftp  密码000000)
	
