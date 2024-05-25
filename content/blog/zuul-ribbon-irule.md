---
title: zuul 自定义负载策略
categories:
  - 开发
tags:
  - springboot
  - java
  - debug
date: 2020-07-27 22:21:48.263000
---
## 定制IRule

使前后端开发可定制匹配, 学习并配置了网关的负载策略，zuul 底层使用的是 ribbon 的负载组件, ribbon 可以从 eureka 获取服务列表, 所以想要自定义负载, 得从 ribbon 入手, 查看并搜索 loadBalance 相关的类, 可以看到 AbstractLoadBalancerRule 这个类, 查看类树, 可以看到顶级接口为 IRule

```java
/**
* 决定最终的服务提供者
*/
public Server choose(Object key);
/**
* 设置负载均衡器
*/
public void setLoadBalancer(ILoadBalancer lb);
/**
* 拿到负载均衡器
*/
public ILoadBalancer getLoadBalancer();    
```

debug 分析, 得知 ribbon 默认的负载策略为: ZoneAvoidanceRule, 这个类继承自 AbstractLoadBalancerRule, AbstractLoadBalancerRule中存在 ILoadBalancer 属性, 可以获取负载器, 负载器中又可以获取服务列表等信息

所以我们只需要继承ZoneAvoidanceRule, 重写 choose 方法即可

ribbon 默认的负载器为: DynamicServerListLoadBalancer, 其中包括节点健康状态服务支持, 节点状态的更新(ServerListUpdater), 负载策略, 可用区域的分析, 可以从此类详细了解 ribbon 的运行机制

## 解决
自定义路由策略, 继承ZoneAvoidanceRule, 当有特定标记的请求过来时, 走定义的服务, 如果没有, 则还是走默认的路由策略，自定义路由策略, 继承ZoneAvoidanceRule, 当有特定标记的请求过来时, 走定义的服务, 如果没有, 则还是走默认的路由策略。

```java
import com.gomyck.util.ResponseWriter;
import com.gomyck.util.StringJudge;
import com.gomyck.util.TokenTake;
import com.netflix.loadbalancer.ILoadBalancer;
import com.netflix.loadbalancer.Server;
import com.netflix.loadbalancer.ZoneAvoidanceRule;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.List;

/**
 * @author gomyck
 * @version 1.0.0
 * @contact qq: 474798383
 * @blog https://blog.gomyck.com
 * @since 2020-03-16
 */
public class CkLoadBalanceRule extends ZoneAvoidanceRule {

    Logger log = LoggerFactory.getLogger(CkLoadBalanceRule.class);

    @Override
    public Server choose(Object key) {
        ILoadBalancer loadBalancer = getLoadBalancer();
        List<Server> allServers = loadBalancer.getReachableServers();
        String ckDev = TokenTake.getToken(ResponseWriter.getRequest(), "CkDev");
        try {//todo 兼容了同一个前端项目  存在请求多个服务的情况, 只对比名字, 忽略项目名称, 在此拿到的服务列表为已匹配的路由表, 不需要根据服务名二次匹配
            if (StringJudge.notNull(ckDev)) {
                log.info("CkLoadBalanceRule will route server name is: " + ckDev);
                final String _ckDev = ckDev.split("-")[0];
                Server server = allServers.stream().filter(e -> {
                    String instanceId = e.getMetaInfo().getInstanceId().split("-")[0];
                    return _ckDev.equals(instanceId);
                }).findFirst().get();
                if (server.isAlive() && (server.isReadyToServe())) {
                    return (server);
                } else {
                    log.error("server :" + server.getMetaInfo().getAppName() + " is not available!");
                }
            }
        } catch (Exception e) {
        }
        return super.choose(key);
    }
}
```

- 在 zuul 服务的 yml 文件中配置需要使用该负载策略的服务 ID, 格式为:
```properties
server-name:
  ribbon:
    NFLoadBalancerRuleClassName: com.gomyck.gateway.config.CkLoadBalanceRule
```

#### 疑惑解答

在最开始, 我把自定义负载策略注册成 bean, 发现所有的路由都失效了, debug 发现, 策略 bean 拿到的服务, 缺失, 注释掉 bean, 查看之前的运行逻辑, 发现每个服务都对应一个路由策略实例, 也就是每个服务对应各自的 IRule 实例