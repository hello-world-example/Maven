# SNAPSHOT



## 强制更新 SNAPSHOP 的几种方式

### -U 参数

```bash
mvn clean compile -U
```



### pom.xml 配置

```xml
<repositories>
    <repository>
        <id>xxx</id>
        <url>xxx</url>
        <snapshots>
            <updatePolicy>always</updatePolicy>
        </snapshots>
    </repository>
</repositories>
```

可选的测略

- "always"
- "daily" (default)
- "interval:XXX" (in minutes) 
- "never" (only if it doesn't exist locally).



> 参见 Maven 官方文档 [Repositories](http://maven.apache.org/pom.html#Repositories)



### settings.xml 配置

> 参见 Maven 官方文档 [Settings](https://maven.apache.org/ref/3.6.1/maven-settings/settings.html) 文件配置格式



## Read More

- [Maven updatePolicy vs -U](https://stackoverflow.com/questions/40372591/maven-updatepolicy-vs-u)
- [How does the updatePolicy in maven really work?](https://stackoverflow.com/questions/3805329/how-does-the-updatepolicy-in-maven-really-work)