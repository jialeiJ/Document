# Linux安装Redis
	
### 安装

### 1、移动redis并解压缩包

	mv redis-6.2.6.tar.gz /usr/local/redis-6.2.6.tar.gz
	tar -zxvf redis-6.2.6.tar.gz
    mv redis-6.2.6 redis

### 2、编译

	1、cd /usr/local/redis
    2、make    执行编译命令
	
### 3、安装

    make PREFIX=/usr/local/redis install

### 4、配置

    vi /usr/local/redis/redis.conf

    通过 /daemonize  查找到属性，默认是no，更改为yes即可。
    将protected-mode设置为no，可通过外网访问
    修改bind 127.0.0.1 -::1 -> bind * -::*

### 5、防火墙配置

    vi /etc/sysconfig/iptables

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 6379 -j ACCEPT

### 6、额外配置

    vi /etc/sysctl.conf

    net.core.somaxconn = 1024
    vm.overcommit_memory = 1

    执行sysctl -p生效配置
    

#   注：
    这里多了一个关键字 PREFIX= 这个关键字的作用是编译的时候用于指定程序存放的路径。
    比如我们现在就是指定了redis必须存放在/usr/local/redis目录。
    假设不添加该关键字Linux会将可执行文件存放在/usr/local/bin目录，

    库文件会存放在/usr/local/lib目录。配置文件会存放在/usr/local/etc目录。
    其他的资源文件会存放在usr/local/share目录。
    这里指定号目录也方便后续的卸载，后续直接rm -rf /usr/local/redis 即可删除redis。

### 7、设置nginx开机自启动

    1、进入到/lib/systemd/system/目录
        cd /lib/systemd/system/
    2、创建redis-server.service文件，并编辑
        vi redis-server.service
    3、加入开机自启动
        systemctl daemon-reload
        systemctl enable redis-server
    4、取消开机自启动
        systemctl disable redis-server

    # systemctl start redis-server.service　         启动redis服务
    # systemctl stop redis-server.service　          停止服务
    # systemctl list-units --type=service     查看所有已启动的服务
    # systemctl status redis-server.service          查看服务当前状态，未启动成功可看其失败原因
    # systemctl enable redis-server.service          设置开机自启动
    # systemctl disable redis-server.service         停止开机自启动
