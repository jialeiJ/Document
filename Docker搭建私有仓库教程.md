# 搭建私有仓库
    
#### 下载registry镜像

    docker pull registry

#### 防火墙添加运行5000端口（跳过）

    iptables -I INPUT 1 -p tcp --dport 5000 -j ACCEPT

#### 通过镜像启动一个容器

    docker run -itd -p 192.168.5.105:5000:5000 -v /data/docker/registry:/var/lib/registry -e REGISTRY_STORAGE_DELETE_ENABLED=true --restart=always --name registry registry:latest

#### 编辑启动服务

    1、vim /etc/docker/daemon.json
    2、添加
        "insecure-registries": ["192.168.5.105:5000"]
	3、添加可删除镜像配置
		docker exec -it 5692e97b54e5 cat /etc/docker/registry/config.yml
			delete:
				enabled: true
		
		docker exec -it 5692e97b54e5 vi /etc/docker/registry/config.yml
			version: 0.1
			log:
			  fields:
				service: registry
			storage:
			  cache:
				blobdescriptor: inmemory
			  filesystem:
				rootdirectory: /var/lib/registry
			  delete:
				enabled: true
			http:
			  addr: :5000
			  headers:
				X-Content-Type-Options: [nosniff]
			health:
			  storagedriver:
				enabled: true
				interval: 10s
				threshold: 3
	4、重启容器
		docker restart 5692e97b54e5
#### 重启

	systemctl daemon-reload
    systemctl restart docker

#### 访问

    http://192.168.5.105:5000/v2/_catalog

#### 安装python-requests用于查询仓库镜像
    
    1、yum install python-requests -y
	
### 删除仓库镜像

	1、列出私有仓库的所有镜像
		curl http://192.168.118.16:5000/v2/_catalog
		{"repositories":["centos"]}
	2、查看镜像版本
		curl http://192.168.118.16:5000/v2/centos/tags/list
		{"name":"centos","tags":["v1"]}
	3、获取sha
		curl --header "Accept: application/vnd.docker.distribution.manifest.v2+json" -I -X GET http://192.168.5.105:5000/v2/test(镜像路径)/test(镜像名)/manifests/1.2(tag-版本)
	4、删除
		curl -X DELETE http://192.168.5.105:5000/v2/test/test/manifests/sha256:
	5、仍能查到问题
		5.1、 进入docker镜像仓库
			cd /data/docker/registry/docker/registry/v2/repositories/
		5.2、删除对应的镜像路径或文件名的文件夹
			rm -rf test
	
