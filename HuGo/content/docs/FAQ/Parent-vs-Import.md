# parent 和 import 的区别



## parent

示例

```xml
<!-- Inherit defaults from Spring Boot -->
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>1.5.21.RELEASE</version>
</parent>
```

> [13.2.1 Inheriting the starter parent](https://docs.spring.io/spring-boot/docs/1.5.21.RELEASE/reference/htmlsingle/#using-boot-maven-parent-pom)



## import

示例

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <!-- Import dependency management from Spring Boot -->
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-dependencies</artifactId>
      <version>1.5.21.RELEASE</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```

> [13.2.2 Using Spring Boot without the parent POM](https://docs.spring.io/spring-boot/docs/1.5.21.RELEASE/reference/htmlsingle/#using-boot-maven-without-a-parent)



## 区别

- 相同点
  - 实现复用
  - 统一管理依赖的版本
- parent
  - 优点
    - 定义默认依赖
    - 定义默认行为
  - 缺点
    - 单继承
- import
  - 优点
    - 多继承
  - 缺点
    - 只作用于 dependencyMannagement，导入 dependencyMannagement
    - 不允许属性覆盖



## Read More

- [Introduction to the Dependency Mechanism](http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html)
- [《Maven官方文档》-Maven依赖机制简介]([http://ifeve.com/maven-dependency-mechanism/](http://ifeve.com/maven-dependency-mechanism/))