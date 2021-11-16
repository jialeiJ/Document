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
    
    
