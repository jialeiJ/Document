# Linux安装Oracle11g版本教程

### 修改操作系统核心参数

	1）root用户下修改SHELL限制
	    vi /etc/security/limits.conf
	    键入以下内容：
	    oracle soft nproc 2047
	    oracle hard nproc 16384
	    oracle soft nofile 1024
	    oracle hard nofile 65536
	    
	2）修改/etc/pam.d/login文件
	    vi /etc/pam.d/login
	    键入以下内容：
	    session required /lib/security/pam_limits.so
	    session required pam_limits.so
	    
	3）修改linux内核
	    vi /etc/sysctl.conf
	    键入以下内容：
	    fs.file-max = 6815744
	    fs.aio-max-nr = 1048576
	    kernel.shmall = 2097152
	    kernel.shmmax = 2147483648
	    kernel.shmmni = 4096
	    kernel.sem = 250 3200 100 128
	    net.ipv4.ip_local_port_range = 9000 65500
	    net.core.rmem_default = 4194304
	    net.core.rmem_max = 4194304
	    net.core.wmem_default = 262144
	    net.core.wmem_max = 1048576
	    
	4）使linux内核生效
	    sysctl -p
	    输出以下内容：
	    fs.file-max = 6815744
        fs.aio-max-nr = 1048576
        kernel.shmall = 2097152
        kernel.shmmax = 2147483648
        kernel.shmmni = 4096
        kernel.sem = 250 3200 100 128
        net.ipv4.ip_local_port_range = 9000 65500
        net.core.rmem_default = 4194304
        net.core.rmem_max = 4194304
        net.core.wmem_default = 262144
        net.core.wmem_max = 1048576
        
    5）编辑/etc/profile
        vi /etc/profile
        键入以下内容：
        if [ $USER = "oracle" ]; then
            if [ $SHELL = "/bin/ksh" ]; then
                ulimit -p 16384
                ulimit -p 65536
            else
                ulimit -u 16384 -n 65536
            fi
        fi
        
    6）创建Oracle安装组oinstall，数据库管理员组dba，及oracle用户并为oracle用户创建密码
        groupadd oinstall  &&  groupadd dba   &&   useradd -g oinstall -G dba oracle
        passwd oracle（需要输入密码，输入2次，两次需保次一致）
            
    7）创建oracle安装目录
        mkdir /home/oracle/data
        mkdir /home/oracle/oracle11g
        mkdir /home/oracle/oracle11g/oraInventory
        chmod 755  /home/oracle/data
        chmod 755  /home/oracle/oracle11g
        chown -R oracle:oinstall  /home/oracle/data
        chown -R oracle:oinstall  /home/oracle/oracle11g
        
    8）设置Oracle环境变量(注释掉最后两行，在末尾添加如下参数)
        vi  .bash_profile
        
        export ORACLE_BASE=/home/oracle/oracle11g
        export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/db_1
        export PATH=$PATH:$HOME/bin:$ORACLE_HOME/bin
        export ORACLE_SID=orcl
        export ORACLE_PID=ora11g
        export NLS_LANG=AMERICAN_AMERICA.AL32UTF8
        export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/usr/lib
        
        source .bash_profile
        
    9）安装oracle
        Ⅰ 安装依赖包
            yum -y install binutils compat-libstdc++-33  elfutils-libelf  elfutils-libelf-devel  expat  gcc  gcc-c++   glibc   glibc-common  glibc-devel  glibc-headers  libaio libaio-devel libgcc libstdc++ libstdc++-devel make pdksh sysstat  unixODBC unixODBC-devel   numactl-devel  ksh    kernel-headers   pcre*   readline*  rlwrap
        Ⅱ 解压文件
            unzip linux.x64_11gR2_database_1of2.zip
            unzip linux.x64_11gR2_database_2of2.zip
            
            cp  database/response/db_install.rsp   database/response/db_install.rsp.bak
        Ⅲ  修改安装文件
            db_install.rsp
            
        静默安装
        unset DISPLAY
        /home/oracle/database/runInstaller -silent -ignorePrereq -responseFile /home/oracle/database/response/db_install.rsp
        
        切换到root执行脚本
        sh /home/oracle/oracle11g/oraInventory/orainstRoot.sh
        sh /home/oracle/oracle11g/product/11.2.0/db_1/root.sh
        
        添加环境变量
        vi  .bash_profile
        
        export TNS_ADMIN=$ORACLE_HOME/network/admin
        export PATH=.:${PATH}:$HOME/bin:$ORACLE_HOME/bin
        export PATH=${PATH}:/usr/bin:/bin:/usr/local/bin
        export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:$ORACLE_HOME/lib
        export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:$ORACLE_HOME/oracm/lib
        export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/lib:/usr/lib:/usr/local/lib
        export CLASSPATH=${CLASSPATH}:$ORACLE_HOME/jlib
        export CLASSPATH=${CLASSPATH}:$ORACLE_HOME/rdbms/jlib
        export CLASSPATH=${CLASSPATH}:$ORACLE_HOME/network/jlib
        export LIBPATH=${CLASSPATH}:$ORACLE_HOME/lib:$ORACLE_HOME/ctx/lib
        export ORACLE_OWNER=oracle
        export SPFILE_PATH=$ORACLE_HOME/dbs
        export ORA_NLS10=$ORACLE_HOME/nls/data
        
        source .bash_profile
        
        启动监听服务
            /home/oracle/oracle11g/product/11.2.0/db_1/bin/netca /silent /responseFile /home/oracle/database/response/netca.rsp
            
        重新启动监听：
            /home/oracle/oracle11g/product/11.2.0/db_1/bin/lsnrctl start LISTENER

### 二、静默创建数据库

    1）修改配置文件
        vi  /home/oracle/database/response/dbca.rsp
    
    2）创建库
        /home/oracle/oracle11g/product/11.2.0/db_1/bin/dbca   -silent   -responseFile /home/oracle/database/response/dbca.rsp
        
        
    7）启动监听服务
        --登录数据库 sqlplus / as sysdba
        export DISPLAY=:0.0
        netca -silent -responseFile /home/oracle/software/database/response/netca.rsp
        lsnrctl start
            
        检查LISTENER状态
        lsnrctl status
        
        sqlplus /nolog
        connect sysdba / as sysdba
        
        shutdown;
        startup;
