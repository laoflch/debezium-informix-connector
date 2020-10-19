# Debezium-informix-connector 

Debezium-informix-connector 是Debezium针对informix数据库的connector插件



## 测试开发环境准备

### 准备informix数据库环境

#### docker方式

导入informix数据库镜像

```bash
docker load -i infromix-11.7-img.tar
```

运行informix容器

```bash
docker run -it -p 9998:9998 --name informix informix:11.7
```



### 准备maven私库

### docker方式

### 导入nexus3的docker镜像

```bash
xz -d sonatype-nexus3-img.tar
docker load ./sonatype-nexus3-img.tar sonatype/nexus3
```

### 运行nexus3容器

（可根据具体情况，设置对外映射的端口号）

```bash
docker run -d --name nexus3-maven -p 8081:8081 nexus3-maven:1.0.0
```

### 停止nexus3容器

```bash
docker container stop  nexus3-maven
```

### 导入容器的volum

```bash
xz -d nexus3-maven-vol-20200727-1.tar.zx

tar xvf nexus3-maven-vol-20200727-1.tar
```

### 查找容器vol位置

```bash
docker container inspect nexus3-maven | grep -e "\"Source\"\: \".*volumes.*\_data"
```

### 替换vol内容

```bash
rm -r <nexus3-maven卷所在位置>/_data

chown -R 200 ./_data

chgrp -R 200 ./_data

docker container start  nexus3-maven
```

### 检查nexus3-maven

通过浏览器访问http://localhost:8081

### 设置maven仓库地址

在settings.xml中设置

```xml
<mirror>
      <id>nexus-local</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus local</name>
        <url>http://localhost:8082/repository/maven-public/</url>
      </mirror>
  </mirrors>
```

通过maven编译和测试debezium-informix-connector

```bash
tar -zxvf debezium-informix-connector.tar.gz

cd debezium-informix-connector && maven package
```

