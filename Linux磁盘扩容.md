# Linux磁盘扩容教程

### 关闭Vmware的centos7系统，删除快照，才能在VMWare菜单中设置需要增加到的磁盘大小

### 一、分区：如下命令依次执行并重启虚拟机,reboot
    fdisk /dev/sda

    p　　　　　　　查看已分区数量（我看到有两个 /dev/sda1 /dev/sda2） 
    n　　　　　　　新增加一个分区
    p　　　　　　　分区类型我们选择为主分区
                 分区号输入3（因为1,2已经用过了,sda1是分区1,sda2是分区2,sda3分区3）
    回车　　　　　 默认（起始扇区）
    回车　　　　　 默认（结束扇区）
    t　　　　　　　修改分区类型
                 选分区3
    8e　　　　　 　修改为LVM（8e就是LVM）
    w　　　　　  　写分区表
    q　　　　　  　完成，退出fdisk命令
	
	yum -y install iptables-services

### 二、格式化分区3 命令如下:
    
    mkfs.ext3 /dev/sda3

### 三、添加新LVM到已有的LVM组，实现扩容

    lvm　　　　　　　　　　　　           进入lvm管理

    lvm>pvcreate /dev/sda3　　           这是初始化刚才的分区3
    
    lvm>vgextend centos /dev/sda3     将初始化过的分区加入到虚拟卷组centos (卷和卷组的命令可以通过 vgdisplay )
    
    lvm>vgdisplay -v或者vgdisplay查看free PE /Site
    
    lvm>lvextend -l+6143 /dev/mapper/centos-root　　扩展已有卷的容量（6143 是通过vgdisplay查看free PE /Site的大小）
    
    lvm>pvdisplay 查看卷容量，这时你会看到一个很大的卷了
    
    lvm>quit 　退出

### 四、上面只是卷扩容，下面是文件系统的真正扩容，输入以下命令：

	xfs_growfs /dev/mapper/centos-root

    /dev/mapper/centos-root是df -h查看到根目录的挂载点
	
### 五、查找大文件

	find . -type f -size +500M  -print0 | xargs -0 du -h | sort -nr
	
### 六、查看各个分区的磁盘占用情况

	df -h
	
### 七、统计命令

	#列出当前文件夹的大小
		du -sh
	#列出当前文件夹中各个文件及子文件夹的大小：
		du -h -d1
	#列出/home文件夹中各个文件及文件夹的大小：
		du -h -d1 /home
		
### 八、清空文件释放空间

	echo "" > yourfile
	
### 九、查看已删除未释放的进程

	lsof | grep deleted

	


    

