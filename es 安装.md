### 注: 我使用的是开源版本，即压缩包名字中带有 -oss 。和视频中使用的版本功能一样。只要保证 elasticearch 和 kibana 的版本一致即可。

### 1.上传 ElasticSearch 安装包

```
小伙伴们自行使用各种传输工具
```

### 2.执行解压操作

```
tar -zxvf elasticsearch-oss-7.4.0-linux-x86_64.tar.gz -C /opt
```

### 3.创建普通用户

#### 因为安全问题，Elasticsearch 不允许 root 用户直接运行, 所以要创建

新用户, 在 root 用户中创建，执行如下操作

```
useradd itheima #新增itheima用户
passwd itheima #为itheima用户设置密码
```

### 4.为新用户授权

```
chown -R itheima:itheima /opt/elasticsearch-7.4.0 #文件夹所有者
```

注: 以上授权可能会漏掉一些文件，启动的时候会报错。注意看报错信息，再次对漏掉的文件执行授权即可。

### 5.修改 elasticsearch.yml 文件

```
vi /opt/elasticsearch-7.4.0/config/elasticsearch.yml
```

增加如下内容：

```
cluster.name: my-application
node.name: node-1
network.host: 0.0.0.0
http.port: 9200
cluster.initial_master_nodes: ["node-1"]
```

### 6.修改配置文件

$\color{orange} { 切换到 root 用户 } $

```
su root
```

$\color{orange} {1. ====最大可创建文件数太小======} $

```
vi /etc/security/limits.conf
```

$\color{orange} {在文件末尾怎加下面内容}$

```
itheima soft nofile 65536
itheima hard nofile 65536
```

$\color{orange} {====}$

```
vi /etc/security/limits.d/20-nproc.conf
```

$\color{orange} {在文件末尾怎加下面内容}$

```
itheima soft nofile 65536
itheima hard nofile 65536
*  hard nproc 4096

```

$\color{orange} {2. ====最大虚拟内存太小======} $

```
vi /etc/sysctl.conf
```

$\color{orange} {在文件末尾怎加下面内容}$

```
vm.max_map_count=655360
```

$\color{orange} {重新加载，输入下面命令} $

```
sysctl -p
```

### 7. 启动 elasticsearch

```
su itheima
cd /opt/elasticsearch-7.4.0/bin
./elasticsearch
```

### 8. 关闭防火墙

```
systemctl stop firewalld #暂时关闭防火墙

或者

systemctl enable firewalld.service #永久打开
systemctl disable firewalld.service #永久关闭
```

$\color{orange} {如果是腾讯或阿里云还需在安全组设置里面允许 9200 端口访问} $
