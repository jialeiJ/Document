# Elasticsearch部署

### 1、上传文件
        /usr/local/middleware/

### 2、解压文件
        tar -zxvf elasticsearch-7.17.8-linux-x86_64.tar.gz
        tar -zxvf kibana-7.17.8-linux-x86_64.tar.gz

### 3、创建用户
        因为安全问题，Elasticsearch不允许root用户直接运行，所以要创建新用户，root用户中创建新用户
        useradd es #新增 es 用户
        passwd es #为 es 用户设置密码
        userdel -r es #如果错了，可以删除再加
        chown -R es:es /usr/local/middleware/elasticsearch-7.17.8 #文件夹所有者

### 4、修改配置文件
        # 加入如下配置
        cluster.name: elasticsearch
        node.name: node-1
        network.host: 0.0.0.0
        http.port: 9200
        cluster.initial_master_nodes: ["node-1"]

### 5、修改/etc/security/limits.conf
        # 在文件末尾中增加下面内容
        # 每个进程可以打开的文件数的限制
        es soft nofile 65536
        es hard nofile 65536

### 6、修改/etc/sysctl.conf
        # 在文件中增加下面内容
        # 一个进程可以拥有的 VMA(虚拟内存区域)的数量,默认值为 65536
        vm.max_map_count=655360

### 7、重新加载
        sysctl -p

### 8、启动es
        #启动
        bin/elasticsearch
        #后台启动
        bin/elasticsearch -d