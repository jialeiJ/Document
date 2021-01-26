# Linux安装Git教程

### 一、下载最新版本
    yum install -y wget
    wget -O /tmp/git-2.9.5.tar.gz https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz

### 二、解压编译
	
	# 安装编译依赖
    yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
    
    # 解压
    tar -zxf /tmp/git-2.9.5.tar.gz -C /tmp/
    
    cd /tmp/git-2.9.5
    
    # 检验相关依赖，设置安装路径
    ./configure --prefix=/usr/local/git
    
    # 编译安装
    make && make install
	

### 三、配置全局环境变量

	# 删除已有的 git
    yum remove git
    
    # 配置环境变量
    vi /etc/profile
    
    # GIT_HOME
    GIT_HOME=/usr/local/git
    export PATH=$PATH:$GIT_HOME/bin
    
    # 刷新
    source /etc/profile
	
### 四、检查git的版本

	git --version