> ![warning](../sources/images/check.gif)按组合并返回结果，比如菜单服务，接口一样，但有多种实现，用group区分，现在消费方需从每种group中调用一次返回结果，合并结果返回，这样就可以实现聚合菜单项。  

> ![warning](../sources/images/warning-3.gif)从2.1.0版本开始支持,

代码参见：https://github.com/alibaba/dubbo/tree/master/dubbo-test/dubbo-test-examples/src/main/java/com/alibaba/dubbo/examples/merge

配置如：

**搜索所有分组**

```xml
<dubbo:reference interface="com.xxx.MenuService" group="*" merger="true" />
```

**合并指定分组**

```xml
<dubbo:reference interface="com.xxx.MenuService" group="aaa,bbb" merger="true" />
```

**指定方法合并结果，其它未指定的方法，将只调用一个Group**

```xml
<dubbo:reference interface="com.xxx.MenuService" group="*">
    <dubbo:method name="getMenuItems" merger="true" />
</dubbo:service>
```

**某个方法不合并结果，其它都合并结果**

```xml
<dubbo:reference interface="com.xxx.MenuService" group="*" merger="true">
    <dubbo:method name="getMenuItems" merger="false" />
</dubbo:service>
```

**指定合并策略，缺省根据返回值类型自动匹配，如果同一类型有两个合并器时，需指定合并器的名称**
参见：[合并结果扩展](http://dubbo.io/developer-guide/SPI%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C/%E5%90%88%E5%B9%B6%E7%BB%93%E6%9E%9C%E6%89%A9%E5%B1%95.html)

```xml
<dubbo:reference interface="com.xxx.MenuService" group="*">
    <dubbo:method name="getMenuItems" merger="mymerge" />
</dubbo:service>
```

**指定合并方法，将调用返回结果的指定方法进行合并，合并方法的参数类型必须是返回结果类型本身**

```xml
<dubbo:reference interface="com.xxx.MenuService" group="*">
    <dubbo:method name="getMenuItems" merger=".addAll" />
</dubbo:service>
```