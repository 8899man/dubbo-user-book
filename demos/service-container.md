> ![warning](../sources/images/check.gif)服务容器是一个standalone的启动程序，因为后台服务不需要Tomcat或JBoss等Web容器的功能，如果硬要用Web容器去加载服务提供方，增加复杂性，也浪费资源。

> ![warning](../sources/images/check.gif)服务容器只是一个简单的Main方法，并加载一个简单的Spring容器，用于暴露服务。

> ![warning](../sources/images/check.gif)服务容器的加载内容可以扩展，内置了spring, jetty, log4j等加载，可通过Container扩展点进行扩展，参见：[Container](http://dubbo.io/developer-guide/SPI%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C/%E5%AE%B9%E5%99%A8%E6%89%A9%E5%B1%95.html)。配置配在java命令-D参数或者dubbo.properties中

##### Spring Container

* 自动加载 META-INF/spring 目录下的所有 Spring 配置。
* 配置spring配置加载位置：`dubbo.spring.config=classpath*:META-INF/spring/*.xml`

##### Jetty Container

* 启动一个内嵌Jetty，用于汇报状态。
* 配置：
    * dubbo.jetty.port=8080 ----配置jetty启动端口
    * dubbo.jetty.directory=/foo/bar ----配置可通过jetty直接访问的目录，用于存放静态文件
    * dubbo.jetty.page=log,status,system ----配置显示的页面，缺省加载所有页面
        
##### Log4j Container

* 自动配置log4j的配置，在多进程启动时，自动给日志文件按进程分目录。
* 配置：
    * dubbo.log4j.file=/foo/bar.log ----配置日志文件路径
    * dubbo.log4j.level=WARN ----配置日志级别
    * dubbo.log4j.subdirectory=20880 ----配置日志子目录，用于多进程启动，避免冲突

##### 容器启动

如：(缺省只加载spring)

```sh
java com.alibaba.dubbo.container.Main
```

或：(通过main函数参数传入要加载的容器)

```sh
java com.alibaba.dubbo.container.Main spring jetty log4j
```

或：(通过JVM启动参数传入要加载的容器)

```sh
java com.alibaba.dubbo.container.Main -Ddubbo.container=spring,jetty,log4j
```

或：(通过classpath下的dubbo.properties配置传入要加载的容器)

**dubbo.properties**

```
dubbo.container=spring,jetty,log4j
```
