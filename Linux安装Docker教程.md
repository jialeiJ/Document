# Linux安装Docker教程

### 一、查看centos7版
	
	uname -r

### 二、更新
	
	yum update -y

### 三、安装 docker

	yum -y install docker
	
### 四、启动docker

	systemctl start docker
	
### 五、设为开机自启

	chkconfig docker on
	
	#LCTT 译注：此处采用了旧式的 sysv 语法，如采用CentOS 7中支持的新式 systemd 语法，如下：
	systemctl  start docker.service
	systemctl  enable docker.service
	
### 五、设置镜像（使用Docker 中国加速器）

	vi /etc/docker/daemon.json

	{
	    "registry-mirrors":["https://6kx4zyno.mirror.aliyuncs.com"]
	}
	
### 六、编辑docker宿主机文件/lib/systemd/system/docker.service

	vi /lib/systemd/system/docker.service
	修改以ExecStart为开头的行
	ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock \
	
### 六、重新启动并验证

	1、systemctl restart docker
	2、docker version
	
### 七、设置docker开机自启

	1、设置开机启动
	systemctl enable docker
	2、将指定用户添加到用户组
	usermod -aG docker root
	
	
### 常用命令
	* 关闭防火墙
	sudo systemctl stop firewalld 临时关闭
	sudo systemctl disable firewalld ，然后reboot 永久关闭
	sudo systemctl status  firewalld 查看防火墙状态
	* 打包
	mvn clean package docker:build -Dmaven.skip.test=true
	* 启动镜像
	docker run -d -p 8081(映射端口):8080(实际端口) jaray/docker:latest
	* 访问
	http://192.168.5.101:8081/docker
	
	1、停止docker
		service docker stop	
	2、重启docker
		service docker restart
	3、查看镜像
		docker images
	4、启动镜像
		docker run -p 8080(映射端口):8080(实际端口) -d jaray/docker:latest(REPOSITORY:TAG)
	5、测试
		http://192.168.5.101:8080/docker
	6、查看容器
		docker ps -a
	7、停止容器
		docker stop ffb64ae139ec(容器ID)
	8、启动容器
		docker start ffb64ae139ec(容器ID)
	9、进入容器
		docker exec -it ffb64ae139ec(容器ID) /bin/bash
	10、从容器中拷出文件
		docker cp ffb64ae139ec(容器ID):/docker.jar /home/jaray/docker.jar
	11、往容器中拷入文件
		docker cp /home/jaray/backup/防火墙.txt ffb64ae139ec(容器ID):/usr/local/
		

### 命令及用法
	
	yum -y install docker-ce 下载最新版的docker
	service docker start 启动Docker服务
	service docker stop 停止Docker服务
	service docker restart 重新启动Docker服务
	docker version 查看Docker的版本号
	docker pull 镜像地址:版本 从镜像仓库中下载
	docker save a2a69ca5184a > jt-centOS6.tar 根据镜像id导出镜像
	docker save -o redis-3.2.8.tar redis:3.2.8 根据镜像名称导出镜像
	docker load -i docker-centos-6.5.tar 指定jar包导入镜像文件
	docker rmi a2a69ca5184a 根据Id号删除镜像文件
	docker rmi -f a2a69ca5184a 强制删除镜像文件 删除镜像前需要先关闭容器
	docker images 查询所有镜像文件
	docker inspect index.alauda.cn/tutum/centos:6.5 查看镜像文件细节信息
	docker tag 旧镜像名称和端口 redis-ali:0.0.1 修改镜像的名称
	docker build -t 镜像名称:版本号 根据dockerfile来创建镜像文件
	docker run -d --name 容器名 镜像名:版本号 根据镜像名称启动容器
	docker run -d --name 容器名(自定) 镜像id号 根据镜像id启动容器
	docker run -d -p 虚拟机端口:镜像端口 --name 容器名 镜像名:版本号 启动容器,并指定暴露端口
	例如：docker run -d -p 8093:8080 --name tomcat03 tomcat:7.0
	
	参数说明：
		-d，则containter将会运行在后台模式(Detached mode)
		--name 实例名称
		-p 对外程序访问端口8093，宿主机映射的tomcat端口8080  
		最后的tomcat为镜像的名称
		访问过程
	
	docker ps 查看活动的docker容器进程
	Docker ps -a/-all 查看全部的容器
	docker exec -it 容器id bash 进入指定的容器
	docker stop 容器Id号 停止指定容器
	docker start 容器Id号 启动创建好的容器
	docker stop $(docker ps -q) & docker rm $(docker ps -aq) 关闭和删除所有的容器
	docker rm 容器Id 删除指定的容器
	
	启动mysql：
		docker run --name=mysql-5.7 -it -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql:5.7   --character-set-server=utf8
		
	构建容器并启动
		docker run -p 8080(映射端口):8080(实际端口) --name demo -d jaray/docker:latest(REPOSITORY:TAG)
	查看日志
		docker logs -f demo(容器名称)
		
### 镜像操作
	1、加载镜像到本地
		docker load -i [镜像文件名].tar
		eg：docker load -i platform-prometheus-image.tar
	2、查找镜像
		docker images|grep [镜像名]
		eg：docker images|grep prometheus
	3、标记本地镜像，将其归入某一仓库
		docker tag [镜像/镜像id] [镜像仓库]
		eg：docker tag **/prometheus **/monitoring/prometheus:[版本号]
	4、将本地的镜像上传到镜像仓库
		docker push [镜像仓库]
		docker push **/monitoring/prometheus:[版本号]