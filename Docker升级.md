docker升级步骤及注意事项

node节点
1、列出包含docker字段的软件的信息
	rpm -qa | grep docker
	
2、yum remove卸载软件
	yum remove docker-1.13.1-96.gitb2f74b2.el7.centos.x86_64 docker-client-1.13.1-96.gitb2f74b2.el7.centos.x86_64 docker-common-1.13.1-96.gitb2f74b2.el7.centos.x86_64

3、curl升级到最新版
	curl -fsSL https://get.docker.com/ | sh
	
	
	mv /etc/docker/daemon.json.rpmsave /etc/docker/daemon.json

4、编辑/etc/docker/daemon.json
	vi /etc/docker/daemon.json
		{
			"registry-mirrors": [
				"https://w68hnpar.mirror.aliyuncs.com",
				"http://hub-mirror.c.163.com"
			],
			"exec-opts": ["native.cgroupdriver=systemd"]
		}

5、重启Docker
	systemctl restart docker

6、设置Docker开机自启
	systemctl enable docker

master节点
7、查看node节点列表
	kubectl get nodes
	
8、驱逐这个node节点上的pod
	kubectl drain k8s-node2 --delete-local-data --force --ignore-daemonsets
	
9、删除这个node节点
	kubectl delete nodes k8s-node2

10、生成token
	kubeadm token generate
		i3p5pb.ltvhynl9ksk5l85w

11、生成join
	kubeadm token create i3p5pb.ltvhynl9ksk5l85w --print-join-command --ttl=0
		kubeadm join 192.168.5.105:6443 --token i3p5pb.ltvhynl9ksk5l85w --discovery-token-ca-cert-hash sha256:07d2447a6c1fd0e845de7a07308b14de45366b9280c98ff37257ce1a515aeae1

node节点		
12、备份
	mv /etc/kubernetes/kubelet.conf /etc/kubernetes/kubelet.conf_bak
	mv /etc/kubernetes/pki/ca.crt /etc/kubernetes/pki/ca.crt_bak

13、关闭kubelet
	systemctl stop kubelet
	
14、node加入master
	kubeadm join 192.168.5.105:6443 --token i3p5pb.ltvhynl9ksk5l85w --discovery-token-ca-cert-hash sha256:07d2447a6c1fd0e845de7a07308b14de45366b9280c98ff37257ce1a515aeae1
	
15、重启kubelet
	systemctl start kubelet
	
16、查看日志
	journalctl -xefu kubelet




无yml重启
kubectl get pod calico-node-cvkd4 -n kube-system -o yaml | kubectl replace --force -f -