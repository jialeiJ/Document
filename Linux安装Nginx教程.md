# Linux安装Nginx教程

### 安装依赖库

    yum install -y pcre pcre-devel
    
### zlib安装，用于解压

    yum install -y zlib zlib-devel

### 一、解压nginx到/usr/local/下

    unzip nginx-1.18.0.zip
    mv nginx-1.18.0 nginx
    cd nginx

### 二、配置

    ./configure

### 三、创建日志路径

    mkdir /usr/local/nginx/logs/

### 四、配置编译安装

    make
    make install

### 五、启动、关闭、重启命令

    ./nginx 
    ./nginx -s stop
    ./nginx -s quit
    ./nginx -s reload

### 六、设置nginx开机自启动

    1、进入到/lib/systemd/system/目录
        cd /lib/systemd/system/
    2、创建nginx-server.service文件，并编辑
        vi nginx-server.service
    3、加入开机自启动
        systemctl daemon-reload
        systemctl enable nginx-server
    4、取消开机自启动
        systemctl disable nginx-server

    # systemctl start nginx-server.service　         启动nginx服务
    # systemctl stop nginx-server.service　          停止服务
    # systemctl restart nginx-server.service　       重新启动服务
    # systemctl list-units --type=service     查看所有已启动的服务
    # systemctl status nginx-server.service          查看服务当前状态
    # systemctl enable nginx-server.service          设置开机自启动
    # systemctl disable nginx-server.service         停止开机自启动

### Nginx报502问题
    
    Nginx底层采用的IO多路复用模型epoll，其epoll又是基于linux内核实现，于是看了下/var/log/messages内核日志，于是发现大量如下丢包日志：
    kernel: nf_conntrack: table full, dropping packet.
    为了证实问题，再次进行压力数据发送，发现只有再进行压力测试时才会出现这种内核丢包日志。怀疑是服务器访问量大，内核netfilter模块conntrack相关参数配置不合理，最终导致新连接被drop掉。

    看netfilter参数配置 sudo sysctl -a | grep conntrack 发现65536，于是我直接提升了5倍
    sudo sysctl -w net.netfilter.nf_conntrack_max=327680
    sudo sysctl -w net.nf_conntrack_max=327680
    执行sudo sysctl -p使其立即生效，再次执行压力测试，发现内核不在出现丢包，nginx也不会出现如上错误日志，问题解决。
    如果问题仍存在则继续扩大倍数。
    
    
