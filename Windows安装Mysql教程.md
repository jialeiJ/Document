# Windows10安装Mysql版本8教程

### 一、解压到文件夹
	
	我的目录是：F:\Program Files (x86)\WorkSoftware\Mysql-8.0.17-winx64

### 二、与bin同级的目录下新建my.ini配置文件
	
	[mysqld]
	# 设置3306端口
	port=3306
	# 设置mysql的安装目录
	basedir=F:\Program Files (x86)\WorkSoftware\Mysql-8.0.17-winx64
	# 设置mysql数据库的数据的存放目录
	datadir=F:\Program Files (x86)\WorkSoftware\Mysql-8.0.17-winx64\Data
	# 允许最大连接数
	max_connections=200
	# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
	max_connect_errors=10
	# 服务端使用的字符集默认为utf8mb4
	character-set-server=utf8mb4
	#使用–skip-external-locking MySQL选项以避免外部锁定。该选项默认开启
	external-locking = FALSE
	# 创建新表时将使用的默认存储引擎
	default-storage-engine=INNODB 
	# 默认使用“mysql_native_password”插件认证
	default_authentication_plugin=mysql_native_password
	
	[mysqld_safe]
	log-error=F:\Program Files (x86)\WorkSoftware\Mysql-8.0.17-winx64\mysql_oldboy.err
	pid-file=F:\Program Files (x86)\WorkSoftware\Mysql-8.0.17-winx64\mysqld.pid
	# 定义mysql应该支持的sql语法，数据校验
	sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
	[mysql]
	# 设置mysql客户端默认字符集
	default-character-set=utf8mb4
	[client]
	# 设置mysql客户端连接服务端时默认使用的端口
	port=3306
	default-character-set=utf8mb4


### 三、配置环境变量
	
	右键我的电脑  ->  属性  ->  高级系统设置  ->  环境变量  ->从系统变量中找到Path 
	添加mysql目录下的bin所在路径到Path的末端（不要覆盖Path原值）： 
	F:\Program Files (x86)\WorkSoftware\Mysql-8.0.16-winx64\bin

### 四、初始化数据库以管理员身份运行cmd
	
	点击“开始”菜单，在搜索框中输入“cmd”，点击搜索结果中的“cmd.exe”(非管理员要，按右键选择“以管理员身份运行”)。
	mysqld --initialize --console
	
	执行完成后，会打印 root 用户的初始默认密码，比如：
	A temporary password is generated for root@localhost: a5eyJl&ed5q+

### 五、启动mysql
	net start mysql

### 六、修改初始化密码
	
	mysql -u root -p
	输入初始密码
	
	# 修改密码
 	alter user 'root'@'localhost' identified by 'xxxxx';

### 七、停止服务
	net stop mysql
	# 卸载服务
	mysqld -remove

### 到此所有配置完成。
