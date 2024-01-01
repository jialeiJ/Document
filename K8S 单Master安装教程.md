### 单Master服务器规划：

| 角色         | IP            | 组件                                                         |
|:---------- | ------------- | ---------------------------------------------------------- |
| k8s-master | 192.168.5.105 | kube-apiserver，kube-controller-manager，kube-scheduler，etcd |
| k8s-node1  | 192.168.5.106 | kubelet，kube-proxy，docker，etcd                             |
| k8s-node2  | 192.168.5.107 | kubelet，kube-proxy，docker，etcd                             |

# 操作系统初始化配置
    
#### 关闭防火墙
    systemctl stop firewalld
    systemctl disable firewalld


#### 关闭selinux
    sed -i 's/enforcing/disabled/' /etc/selinux/config  # 永久
    setenforce 0  # 临时

#### 关闭swap
    swapoff -a  # 临时
    sed -ri 's/.*swap.*/#&/' /etc/fstab    # 永久

#### 



