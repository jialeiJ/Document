# RabbitMQ的简单命令

### 1、rabbitmq-plugins.bat" enable rabbitmq_management

### 2、停止并启动服务

	net stop RabbitMQ && net start RabbitMQ

### 3、查看已有用户及用户的角色

	rabbitmqctl.bat list_users

### 默认

	RabbitMQ地址：http://localhost:15672
	username: guest
	password: guest