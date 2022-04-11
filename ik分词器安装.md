## 1.准备环境

### 1).设置 JAVA_HOME

```
vi /etc/profile

#在profile文件末尾添加
export JAVA_HOME=/opt/elasticsearch-7.4.0/jdk
export PATH=$PATH:${JAVA_HOME}/bin

#保存后退出, 重新加载profile
source /etc/profile
```

### 2).下载 maven 安装包

```
cd /opt
wget http://mirror.cc.columbia.edu/pub/software/apache/maven/maven-3/3.1.1/binaries/apache-maven-3.1.1-bin.tar.gz #这是课程中地址，无法访问

wget https://archive.apache.org/dist/maven/maven-3/3.1.1/binaries/apache-maven-3.1.1-bin.tar.gz #使用官网地址
```

### 3).解压

```
tar xzf apache-maven-3.1.1-bin.tar.gz
```

### 4). 设置软连接

```
ln -s apache-maven-3.1.1 maven
```

### 5).设置 path

#### 打开文件

```
vi /etc/profile.d/maven.sh
```

#### 将下面内容复制到文件

```
export MAVEN_HOME=/opt/maven
export PATH=${MAVEN_HOME}/bin:${PATH}
```

#### 使其 maven 路径生效

```
source /etc/profile.d/maven.sh
```

### 6). 验证 maven 是否安装成功

```
mvn -v
```

#### 设置阿里云 maven 镜像

```
vi /opt/maven/conf/settings.xml
```

找到 `<mirrors></mirrors>`添加如下内容

```xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/<url>
    <mirrorOf>central</mirrorOf>
</mirror>

```

## 2.安装 IK 分词器

### 1) 下载 IK

```
wget https://github.com/medcl/elasticsearch-analysis-ik/archive/v7.4.0.zip
```

### 2）解压

```
unzip v7.4.0.zip
```

### 3). 编译 jar 包

```
cd elasticsearch-analysis-ik-7.4.0/
mvn package
```

### 4) jar 包移动

```
cd /opt/elasticsearch-7.4.0/plugin

mkdir analysis-ik

cd analysis-ik

cp -R /opt/elasticsearch-analysis-ik-7.4.0/target/releases/elasticsearch-analysis-ik-7.4.0.zip /opt/elasticsearch-7.4.0/plugins/analysis-ik/

unzip elasticsearch-analysis-ik-7.4.0.zip
```

### 5) 拷贝词典

```
cp -R /opt/elasticsearch-analysis-ik-7.4.0/config/* /opt/elasticsearch-7.4.0/config/
```
