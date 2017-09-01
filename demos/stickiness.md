> ![warning](../sources/images/check.gif)粘滞连接用于有状态服务，尽可能让客户端总是向同一提供者发起调用，除非该提供者挂了，再连另一台。

> ![warning](../sources/images/check.gif)粘滞连接将自动开启延迟连接，以减少长连接数, 参见：[延迟连接](./延迟连接.md)

```xml
<dubbo:protocol name="dubbo" sticky="true" />
```

