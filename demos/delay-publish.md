> ![warning](../sources/images/check.gif)如果你的服务需要warmup时间，比如初始化缓存，等待相关资源就位等，可以使用delay进行延迟暴露。

延迟5秒暴露服务

```xml
<dubbo:service delay="5000" />
```

延迟到Spring初始化完成后，再暴露服务：(基于Spring的ContextRefreshedEvent事件触发暴露)

```xml
<dubbo:service delay="-1" />
```

> ![warning](../sources/images/warning-3.gif)**Spring2.x初始化死锁问题**

> !在Spring解析到<dubbo:service />时，就已经向外暴露了服务，而Spring还在接着初始化其它Bean。  
> !如果这时有请求进来，并且服务的实现类里有调用applicationContext.getBean()的用法。

> !1\. 请求线程的applicationContext.getBean()调用，先同步singletonObjects判断Bean是否存在，不存在就同步beanDefinitionMap进行初始化，并再次同步singletonObjects写入Bean实例缓存。  
> ![deadlock](../sources/images/lock-get-bean.jpg)  
> 2\. 而Spring初始化线程，因不需要判断Bean的存在，直接同步beanDefinitionMap进行初始化，并同步singletonObjects写入Bean实例缓存。  
> ![/user-guide/images/lock-init-context.jpg](../sources/images/lock-init-context.jpg)  
> 
> 这样就导致getBean线程，先锁singletonObjects，再锁beanDefinitionMap，再次锁singletonObjects。  
> 而Spring初始化线程，先锁beanDefinitionMap，再锁singletonObjects。反向锁导致线程死锁，不能提供服务，启动不了。  


> ![warning](../sources/images/check.gif)**规避办法**
> 
> 1. 强烈建议不要在服务的实现类中有applicationContext.getBean()的调用，全部采用IoC注入的方式使用Spring的Bean。
> 2. 如果实在要调getBean()，可以将Dubbo的配置放在Spring的最后加载。
> 3. 如果不想依赖配置顺序，可以使用 `<dubbo:provider deplay=”-1” />`，使Dubbo在Spring容器初始化完后，再暴露服务。
> 4. 如果大量使用getBean()，相当于已经把Spring退化为工厂模式在用，可以将Dubbo的服务隔离单独的Spring容器。
