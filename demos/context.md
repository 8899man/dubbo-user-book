> ![warning](../sources/images/check.gif)上下文中存放的是当前调用过程中所需的环境信息。

> ![warning](../sources/images/check.gif)所有配置信息都将转换为URL的参数，参见 [配置项一览表](../reference-xmlconf/introduction.md) 中的“对应URL参数”一列。

> **注意**
RpcContext是一个ThreadLocal的临时状态记录器，当接收到RPC请求，或发起RPC请求时，RpcContext的状态都会变化。
比如：A调B，B再调C，则B机器上，在B调C之前，RpcContext记录的是A调B的信息，在B调C之后，RpcContext记录的是B调C的信息。

##### (1) 服务消费方

```java
xxxService.xxx(); // 远程调用
boolean isConsumerSide = RpcContext.getContext().isConsumerSide(); // 本端是否为消费端，这里会返回true
String serverIP = RpcContext.getContext().getRemoteHost(); // 获取最后一次调用的提供方IP地址
String application = RpcContext.getContext().getUrl().getParameter("application"); // 获取当前服务配置信息，所有配置信息都将转换为URL的参数
// ...
yyyService.yyy(); // 注意：每发起RPC调用，上下文状态会变化
// ...
```

##### (2) 服务提供方

```java
public class XxxServiceImpl implements XxxService {
 
    public void xxx() { // 服务方法实现
        boolean isProviderSide = RpcContext.getContext().isProviderSide(); // 本端是否为提供端，这里会返回true
        String clientIP = RpcContext.getContext().getRemoteHost(); // 获取调用方IP地址
        String application = RpcContext.getContext().getUrl().getParameter("application"); // 获取当前服务配置信息，所有配置信息都将转换为URL的参数
        // ...
        yyyService.yyy(); // 注意：每发起RPC调用，上下文状态会变化
        boolean isProviderSide = RpcContext.getContext().isProviderSide(); // 此时本端变成消费端，这里会返回false
        // ...
    } 
}
```
