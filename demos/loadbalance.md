> ![warning](../sources/images/check.gif)在集群负载均衡时，Dubbo提供了多种均衡策略，缺省为random随机调用。可以自行扩展负载均衡策略，参见：[负载均衡扩展](http://dubbo.io/developer-guide/SPI%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C/%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E6%89%A9%E5%B1%95.html)

##### Random LoadBalance

* 随机，按权重设置随机概率。
* 在一个截面上碰撞的概率高，但调用量越大分布越均匀，而且按概率使用权重后也比较均匀，有利于动态调整提供者权重。

##### RoundRobin LoadBalance

* 轮循，按公约后的权重设置轮循比率。
* 存在慢的提供者累积请求的问题，比如：第二台机器很慢，但没挂，当请求调到第二台时就卡在那，久而久之，所有请求都卡在调到第二台上。

##### LeastActive LoadBalance

* 最少活跃调用数，相同活跃数的随机，活跃数指调用前后计数差。
* 使慢的提供者收到更少请求，因为越慢的提供者的调用前后计数差会越大。

##### ConsistentHash LoadBalance

* 一致性Hash，相同参数的请求总是发到同一提供者。
* 当某一台提供者挂时，原本发往该提供者的请求，基于虚拟节点，平摊到其它提供者，不会引起剧烈变动。
* 算法参见：http://en.wikipedia.org/wiki/Consistent_hashing。
* 缺省只对第一个参数Hash，如果要修改，请配置`<dubbo:parameter key="hash.arguments" value="0,1" />`
* 缺省用160份虚拟节点，如果要修改，请配置 `<dubbo:parameter key="hash.nodes" value="320" />`

配置如：

```xml
<dubbo:service interface="..." loadbalance="roundrobin" />
```

或

```xml
<dubbo:reference interface="..." loadbalance="roundrobin" />
```

或

```xml
<dubbo:service interface="...">
    <dubbo:method name="..." loadbalance="roundrobin"/>
</dubbo:service>
```

或

```xml
<dubbo:reference interface="...">
    <dubbo:method name="..." loadbalance="roundrobin"/>
</dubbo:reference>
```