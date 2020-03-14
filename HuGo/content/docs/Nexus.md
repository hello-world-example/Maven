# Nexus 私服



## Docker 安装 nexus3

```bash
$ docker pull sonatype/nexus3

# 5000 镜像仓库的服务端口
# 8081 nexus的服务端口
# 默认账号密码为admin/admin123
$ docker run -d --name nexus  --restart=always -p 5000:5000 -p 8081:8081 sonatype/nexus3
```



## 三种仓库类型

- `hosted` : 本地存储
- `proxy` : 代理其他仓库
- `group` : 组合多个仓库为一个地址



## pom - distributionManagement

```xml
<distributionManagement>

    <repository>
        <id>MyNexusPublic</id>
        <url>http://localhost:8081/repository/maven-releases/</url>
    </repository>

    <snapshotRepository>
        <id>MyNexusSnapshot</id>
        <url>http://localhost:8081/repository/maven-snapshots/</url>
    </snapshotRepository>

</distributionManagement>
```

​    

## settings - servers

设置发布私服时的密码

```xml
<servers>
  
    <server>
        <id>MyNexusPublic</id>
        <username>admin</username>
        <password>admin123</password>
    </server>

    <server>
        <id>MyNexusSnapshot</id>
        <username>admin</username>
        <password>admin123</password>
    </server>

</servers>

```



## Read More

- [Maven2部署构件到Nexus时出现的Failed to transfer file错误](https://www.javatang.com/archives/2010/01/23/4518375.html)
- [docker部署Nexus](https://blog.csdn.net/m0_37444820/article/details/82721372)