> ![warning](../sources/images/warning-3.gif)2.2.0以上版本支持,

> ![warning](../sources/images/check.gif)参见：[配置规则](./配置规则.md)

向注册中心写入动态配置覆盖规则：(通过由监控中心或治理中心的页面完成)

```java

RegistryFactory registryFactory = ExtensionLoader.getExtensionLoader(RegistryFactory.class).getAdaptiveExtension();
Registry registry = registryFactory.getRegistry(URL.valueOf("zookeeper://10.20.153.10:2181"));
registry.register(URL.valueOf("override://0.0.0.0/com.foo.BarService?category=configurators&dynamic=false&application=foo&mock=force:return+null"));
```

其中:

```sh
mock=force:return+null
```

* 表示消费方对该服务的方法调用都直接返回null值，不发起远程调用。
* 屏蔽不重要服务不可用时对调用方的影响。

还可以改为：

```sh
mock=fail:return+null
```

* 表示消费方对该服务的方法调用在失败后，再返回null值，不抛异常。
* 容忍不重要服务不稳定时对调用方的影响。
