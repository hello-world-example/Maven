# 常见故障排查



## 确认环境

首先确认 Maven 安装是否正常，很多问题跟自身环境有关。

确认 `mvn` 命令是否可正常使用，可通过 `mvn -v` 确认，输出信息可以看出

- 版本
- 安装路径
- JDK 版本 和 位置
- ...

```bash
~ ᐅ mvn -v
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:47+08:00)
Maven home: /Users/kail/java/apache-maven-3.3.9
Java version: 1.8.0_151, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home/jre
...
```

如果 `mvn` 命令不正常，请先确保 Maven 安装正常，Maven 的安装很简单，只需两步

- 从 http://maven.apache.org/download.cgi 下载任意格式压缩包（不带 src 的），解压到任意目录
- 配置 `JAVA_HOME` 和 `M2_HOME` 环境变量即可，配置的值详见 上面 `mvn -v` 的输出



**注意**：一般对于开发人员来说，建议都保持默认配置即可，**无需**再进行额外的配置，如：

- **无需** 设置本地仓库位置到非系统盘，现在已经不是资源紧张的年代，几个 G 的空间还是有的，为这点空间浪费时间不值得
- **无需** 把 `settings.xml` 复制到本地仓库的根目录
  - 一是 开发的时候，基本没有**用户级别设置**覆盖**系统级别设置**的场景
  - 二是容易搞混， `settings.xml` 文件寻址不是先从本地仓库下找的，而是 `${user.home}/.m2/settings.xml`，如果没有找到，则是 `${maven.home}/conf/settings.xml`。有些人已经把仓库位置改了， `settings.xml` 文件也跟着仓库走，修改实际上是没有效果的
- **无需** 额外的自定配置，使用 IDE 的时候又得设置 IDE，有些人 IDE 和 使用安装的还不统一，**不如全都使用默认配置省事**
- settings.xml 的远程仓库也不用配置，仓库跟着项目走即可
- 镜像也不用配置



## 确认配置

### 生效的设置

```xml
~ ᐅ mvn help:effective-settings                              
...
<settings ...>
  <localRepository>/Users/kail/.m2/repository</localRepository>
 ...
</settings>
...
```

### 生效的 pom

实际的输出会比实例 pom 多很多，关注重点即可，如 **依赖**、**仓库**、**插件**、**编码** 等

```bash
~ ᐅ mvn help:effective-pom
```

### 生效的 Profile

```bash
# 所有支持的 Profile
~ ᐅ mvn help:all-profiles 

# 激活的 Profile（ -P 激活 ）
~ ᐅ mvn help:active-profiles
```

### 其他

```bash
# 查看 maven-help-plugin 支持的 goals
~ ᐅ mvn help:help
```

> 详见 [maven-help-plugin](/Maven/docs/plugins/maven-help-plugin/)



## 依赖问题

```bash
# 查看依赖列表
~ ᐅ mvn dependency:list

# 树形依赖关系
~ ᐅ mvn dependency:tree 
# 查看指定模块
~ ᐅ mvn dependency:tree -pl 模块名
# 详细信息
~ ᐅ mvn dependency:tree -pl 模块名 -Dverbose

# 查看 dependency 插件的其他 goals
~ ᐅ mvn help:describe -Dplugin=org.apache.maven.plugins:maven-dependency-plugin
```

如果依赖的 jar 所在的 pom 有 parent ，会先下载 parent pom。

查看依赖主要有以下几个作用

- 了解有哪些**传递依赖**
- 了解传递依赖的版本是否统一，是否冲突
- 查看发布的模块打包后的依赖



## 关注 scope

```xml
<dependency>
  <groupId>x.x.x</groupId>
  <artifactId>xx-xx-xx</artifactId>
  <version>x.x.x</version>
  <!-- 关注这里 -->
  <scope>provided</scope>
</dependency>
```

因为编码习惯，可能很多人不太关注传递依赖，但是当 jar 包是提供给别人的时候，scope 就要重点关注，否则会给他人困扰，如：**jar 包冲突**、**无用的传递** 等

- compile : 默认，会被传递依赖
- **`provided`** :  **重点关注**，提供非他人的依赖，需要权衡使用 `provided`，避免可能造成困扰的传递依赖
- test : 参与测试
- runtime :  不参与打包，运行时用到
- system :  配合 `systemPath`，不从仓库哪，使用指定的本地文件



## jar 包内容不一致

- 先查看  jar 包是否是正式版
  - 正式版：只能删除本地仓库
  - 快照版：尝试使用 `-U` 参数 `mvn xxx -U`，详见 [SNAPSHOT 更新策略](/Maven/docs/FAQ/SNAPSHOT-U/)
- 养成只要内容变动就修改版本号的习惯，会避免上面的问题
- 通过 `mvn versions:set` 来统一修改版本，减少漏的可能
- ...



## 编译错误

- 如果提示信息不够，使用 `-X` 参数查看详细错误信息
- 



## 更新中...

- ..



## 小结

- 确认环境问题，排除配置错误导致的问题
- 不要完全依赖 IDE 对 Maven 的集成，适当使用 Maven 原生命令，减少 IDE 自身的问题带来的误区
- 了解添加 Maven 依赖的作用，以及都有哪些传递依赖