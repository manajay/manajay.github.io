---
layout: post
title:  dubbo系列 01 - dubbo与相关技术
tag: dubbo
date: 2018-01-10 15:22:50 +09:00
---

![dubbo-architecture](/assets/post/dubbo-architecture.png)

`Dubbo`是一个基于`java`的分布式服务框架，致力于提供高性能和透明化的`RPC`远程服务调用方案，以及`SOA`服务治理方案. 
和大多数`RPC`系统一样, `dubbo`也是基于服务的想法打造的,特别是可以远程调用有参数和返回值的方法. 

> Dubbo |ˈdʌbəʊ| is a high-performance, java based RPC framework open-sourced by Alibaba. As in many RPC systems, dubbo is based around the idea of defining a service, specifying the methods that can be called remotely with their parameters and return types. On the server side, the server implements this interface and runs a dubbo server to handle client calls. On the client side, the client has a stub that provides the same methods as the server.


### dubbo 相关技术与比较

* 阿里巴巴集团内部使用的分布式服务框架 HSF（High Speed Framework，也有人戏称“好舒服”）,没有开源.
* dubbo, 阿里的服务治理框架，2017年又开始维护，开源,推荐使用
* dubboX, 当当基于dubbo搞的，还在维护可以一用，开源, 推荐。
* Motan, 微博的服务治理矿建， 开源，需要学习一下， 推荐。
* Edas, 阿里云服务，要收钱，侵入型很强，不推荐

目前公司使用的是`Dubbo`, 关于阿里两个框架的选择, `InfoQ`上有谈:

```
InfoQ: 我们知道阿里内部现在基本上没有在使用 Dubbo，而是用了 Dubbo 之后开发的第三代 RPC 服务框架 HSF（High-speed Service Framework），那现在还将 Dubbo 重启维护，大家不免疑惑。

罗毅: Dubbo 和 HSF 都是阿里巴巴集团自研的 RPC 服务框架，在不同时期都很好的支持了集团业务的发展。目前，HSF2 主要服务于集团内部业务，而 Dubbo2 主要以开源的形式服务社会，它们之间的关系与 Google 内部使用的 Stubby 和开源的 gRPC 类似。

- 从功能及使用方式上来说，HSF2 和 Dubbo2 都是十分优秀的 RPC 框架，都能很好地满足搭建分布式服务化系统的诉求。内部坚持使用 HSF2 而不是开源版本的 Dubbo2 与业务属性和规模有关。

- 此外也出于以下几方面的考虑：内部统一技术框架、统一运维方面的诉求，应用迁移成本，内部服务注册发现、配置推送以及链路追踪等外围系统的集成度等。总的来说，HSF2 在服务治理、超大规模集群、多机房几个方面比 Dubbo2 更有优势，并且在使用层面保持了对 Dubbo2 的兼容性。

> 为什么我们还要重启 Dubbo 的维护呢？

- 道理和 Google 开源 gRPC 是一样的。开源不仅仅是赋能社会的方式，我们也可以通过社区反馈提升产品和技术能力。

- Dubbo2 作为一款优秀的开源产品，由于面向的用户群体非常广泛，这就决定了它的设计原则强调扩展性、使用轻量、以及对开源外围系统和协议的适配。通过开源社区的建议，目前 Dubbo 已经具备了一些特有功能，例如对 REST 的支持和对 Spring Boot 的集成。

- 值得注意的是，目前负责 Dubbo 的团队和内部负责 HSF 的是同一个团队，在聆听外部用户反馈之余，我们也会把大规模领域里的服务运维经验反哺回 Dubbo 社区，形成良性循环，做到真正意义上的内外统一。
```

## dubbo 角色

- Provider: 在启动时，向注册中心注册自己提供的服务,也就是暴露服务的服务提供方。

- Consumer: 在启动时，向注册中心订阅自己所需的服务,也就是调用远程服务的服务消费方。

- Registry: 服务注册与发现的注册中心。 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者; 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用目前使用`zookeeper`

- Monitor: 统计服务的调用次调和调用时间的监控中心。服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

- Container: 服务运行容器,负责启动，加载，运行服务提供者。
      

## dubbo 特性 学习

* 连通性
* 健壮性
* 伸缩性
* 向未来架构的升级性

### 连通性

- 注册中心负责服务地址的注册与查找，相当于目录服务，服务提供者和消费者只在启动时与注册中心交互，注册中心不转发请求，压力较小
- 监控中心负责统计各服务调用次数，调用时间等，统计先在内存汇总后每分钟一次发送到监控中心服务器，并以报表展示
- 服务提供者向注册中心注册其提供的服务，并汇报调用时间到监控中心，此时间不包含网络开销
- 服务消费者向注册中心获取服务提供者地址列表，并根据负载算法直接调用提供者，同时汇报调用时间到监控中心，此时间包含网络开销
- 注册中心，服务提供者，服务消费者三者之间均为长连接，监控中心除外
- 注册中心通过长连接感知服务提供者的存在，服务提供者宕机，注册中心将立即推送事件通知消费者
- 注册中心和监控中心全部宕机，不影响已运行的提供者和消费者，消费者在本地缓存了提供者列表
- 注册中心和监控中心都是可选的，服务消费者可以直连服务提供者

### 健状性

- 监控中心宕掉不影响使用，只是丢失部分采样数据
- 数据库宕掉后，注册中心仍能通过缓存提供服务列表查询，但不能注册新服务
- 注册中心对等集群，任意一台宕掉后，将自动切换到另一台
- 注册中心全部宕掉后，服务提供者和服务消费者仍能通过本地缓存通讯
- 服务提供者无状态，任意一台宕掉后，不影响使用
- 服务提供者全部宕掉后，服务消费者应用将无法使用，并无限次重连等待服务提供者恢复

### 伸缩性

- 注册中心为对等集群，可动态增加机器部署实例，所有客户端将自动发现新的注册中心
- 服务提供者无状态，可动态增加机器部署实例，注册中心将推送新的服务提供者信息给消费者

### 升级性

当服务集群规模进一步扩大，带动IT治理结构进一步升级，需要实现动态部署，进行流动计算，现有分布式服务架构不会带来阻力。


### 相关链接

* [DUBBO 官方](http://dubbo.io/?spm=5176.100239.blogcont30996.9.40e7989cpg5SaI)
* [Dubbo 入门](https://yq.aliyun.com/articles/30996)
* [Dubbo 入门](https://yq.aliyun.com/articles/30996)
* [分布式RPC框架性能大比拼](http://colobu.com/2016/09/05/benchmarks-of-popular-rpc-frameworks/)
* [性能测试应该怎么做？](https://coolshell.cn/articles/17381.html)
* [分布式服务框架之服务化最佳实践](http://www.infoq.com/cn/articles/servitization-best-practice)
* [分布式服务框架选型：面对Dubbo，阿里巴巴为什么选择了HSF？](http://www.yunweipai.com/archives/17973.html)
* [阿里重启维护Dubbo了](http://www.infoq.com/cn/news/2017/11/Ali-restart-maintenance-Dubbo)

