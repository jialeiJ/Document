# Linux静态IP配置教程

### 模式：NAT模式
		vm虚拟机中 按照  编辑->虚拟网络编辑器
		选中VMnet8
		NET设置：
			网关IP: 192.168.1.2

### 一、进入到网卡配置目录
	
	cd /etc/sysconfig/network-scripts/

### 二、找到eth0的配置文件ifcfg-ens32要修改的地方

	vi ifcfg-ens32
	
	BOOTPROTO="static"    #设置为静态地址
	IPADDR=192.168.1.104
	#NETMASK=255.255.255.0
	GATEWAY=192.168.1.2   # 与上述网关IP一致
	ONBOOT=yes            # 网络开机运行
	PREFIX=24
	DNS=8.8.8.8
	
### 三、设置的DNS值进行相对应的修改

	vi /etc/resolv.conf
	nameserver 8.8.8.8
	
	vi /etc/sysconfig/network
	NETWORKING=yes
	HOSTNAME=centos7
	GATEWAY=192.168.1.2   # 与上述网关IP一致
	
	
### 三、重启网卡

	service network restart 
	systemctl restart network
	
### 四、重启系统
