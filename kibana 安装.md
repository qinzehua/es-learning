### 1.上传 kibana 安装包

### 2.解压 kibana

```
tar -zxvf kibana-oss-7.4.0-linux-x86_64.tar.gz -C /opt/
```

### 3.修改 kibana 配置

```
vi /opt/kibana-7.4.0-linux-x86_64/config/kibana.yml
```

```
server.port: 5601
server.host: '0.0.0.0'
server.name: 'kibana-itcast'
elasticsearch.hosts: ['http://127.0.0.1:9200']
elasticsearch.requestTimeout: 99999
```

### 4.启动kibana
```
cd vi /opt/kibana-7.4.0-linux-x86_64/config/bin
./kibana --allow-root
```
