# 可运行 jar

## 方式一

**目录结构**

- target
  - MainApp.jar
  - config/
  - lib/



**所用插件**

```xml
<plugins>
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.1.2</version>
    <configuration>
      <archive>
        <manifest>
          <addClasspath>true</addClasspath>
          <mainClass>xxx.MainApp</mainClass>
          <classpathPrefix>lib/</classpathPrefix>
          <!-- SNAPSHOT 版本 Class-Path 是 XXX-x.y.z-SNAPSHOT.jar -->
          <!-- 而不是 XXX-x.y.z-20190517.110547-4.jar -->
          <!-- 避免 Class-Path 与实际文件名不一致，找不到类 -->
          <useUniqueVersions>false</useUniqueVersions>
        </manifest>
        <!-- META-INF 文档属性 -->
        <manifestEntries>
          <Rose>*</Rose>
          <!-- 将 config目录也加入到 classPath 下 -->
          <Class-Path>config/</Class-Path>
        </manifestEntries>
      </archive>
      <excludes>
        <!-- 不打到 jar 包里面的文件 -->
        <exclude>config.properties</exclude>
      </excludes>
    </configuration>
  </plugin>


  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-resources-plugin</artifactId>
    <version>3.1.0</version>
    <executions>
      <execution>
        <!-- 绑定到生命周期的compile阶段，即执行compile的时候就执行该插件 -->
        <phase>package</phase>
        <!-- 使用插件的copy-resources目标 -->
        <goals>
          <goal>copy-resources</goal>
        </goals>
        <configuration>
          <encoding>UTF-8</encoding>
          <!--拷贝到构建目录conf文件夹下 -->
          <outputDirectory>${project.build.directory}/config</outputDirectory>
          <resources>
            <resource>
              <!-- 需要拷贝的文件夹 -->
              <directory>src/main/resources</directory>
              <!-- 需要拷贝的文件 -->
              <includes>
                <!-- 注意： 【与 maven-jar-plugin 的 exclude 一致 】-->
                <include>config.properties</include>
              </includes>
            </resource>
          </resources>
        </configuration>
      </execution>
    </executions>
  </plugin>

  <!-- 拷贝依赖的jar包到lib目录 -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <version>3.1.1</version>
    <executions>
      <execution>
        <id>copy</id>
        <phase>package</phase>
        <goals>
          <!-- 拷贝依赖到指定路径下面 -->
          <goal>copy-dependencies</goal>
        </goals>
        <configuration>
          <outputDirectory>${project.build.directory}/lib</outputDirectory>
        </configuration>
      </execution>
    </executions>
  </plugin>
</plugins>
```



## Read More

- [maven-shade-plugin](plugins/maven-shade-plugin.md)
- [maven-assembly-plugin](plugins/maven-assembly-plugin.md)

