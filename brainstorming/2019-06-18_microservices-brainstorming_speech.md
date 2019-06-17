# 目的

## 系统架构遵循的标准和业务驱动力

+ 提高敏捷性
+ 提升客户体验
+ 降低成本

##### 旁白

+ 提高敏捷性：及时响应业务需求，促进企业发展
+ 提升客户体验：提升客户体验，减少客户流失
+ 降低成本：降低增加产品，客户或业务方案的成本

## 拥抱变化

+ 软件开发流程和实践

敏捷开发（XP, Scrum, 精益, 看板）

+ 软件技术和架构

RPC，RESTFul, Frontend–Backend, SPA, HTTP/2, container ...

##### 旁白

传统的软件工程开发过程，总要按需求分析，可行性分析，概要设计，详细设计，测试，维护的软件周期来进行，
从01年Martin Fowler等开发专家忍受不了这些低效率提出敏捷宣言至今，已经有越来越多的敏捷实施方式，如
随着敏捷开发方法和敏捷开发工具和技巧的发展，软件过程中的一些步骤被新的开发颠覆甚至忽略。
敏捷开发的特点就是：拥抱市场变化，积极响应市场需求，利用变化为产品创造竞争优势。
从软件技术和架构上考虑，随着以上技术的不断出现，我们也有了对技术架构的不断演进及实践

# 历史演进

[timeline](http://assets.processon.com/chart_image/5cc26d08e4b09eb4ac285ab9.png)

# 系统架构演进

## Monolithic

[Monolithic架构图](http://assets.processon.com/chart_image/5cf78cbfe4b06e3f4fadb4f9.png)

### 优劣

#### 强项

+ 开发简单，集中式管理
+ 基本不会重复开发
+ 功能都在本地，没有分布式的管理和调用消耗

#### 弱项

+ 效率低：开发都在同一个项目改代码，相互等待，冲突不断
+ 维护难：代码功功能耦合在一起，新人不知道何从下手
+ 不灵活：构建时间长，任何小修改都要重构整个项目，耗时
+ 稳定性差：一个微小的问题，都可能导致整个应用挂掉
+ 扩展性不够：无法满足高并发下的业务需求

## SOA

[SOA应用架构图](http://assets.processon.com/chart_image/5d04c74ee4b0cbb88a5e94cc.png)

##### 旁白

SOA的主要目的是为了企业各个系统更加容易地融合在一起。
说到SOA就得说到ESB。 ESB是一个连接所有企业级服务的脚手架。
ESB更多地关注应用流程方面的信息，将业务流程剥离出来并将其交由ESB来统一管理,当然，在此基础上也衍生出了EDA, CEP等相关组件。

在架构设计上SOA有一个重要概念，叫服务合同，设计时一般讲究自上而下，在设计开始时会先定义好服务合同。
SOA架构通常会预先把每个模块服务接口都定义好。模块系统间的通讯必须遵守这些接口，各服务是针对他们的调用者。这种设计模式相对来说不是很敏捷。
另一方面有别于微服务，一般采用水平分层，如服务层模式，实体层等。 这种设计要求所有的服务都通过这个Entity服务层来获取数据。这种设计较为不灵活，比如每次数据层的改动都可能影响到所有业务层的服务。

### ESB

+ 服务的虚拟化,支持虚拟化通讯参与方之间的服务交互并对其进行管理：意思就是服务只需要关注完成自己的功能，不需要关心哪个服务调用它以及它需要调用哪个服务。
+ 服务的转化,包装以及桥接：在过去的应用中基本上分为基础服务总线（Socket),企业服务总线（JMS/MQ)，标准服务总线(SOAP/WebService)
+ 消息的传递,过滤以及路由
+ 服务编制

##### 旁白

它可以把不同数据格式或模型转成统一格式，把XML的输入转成CSV传给服务，把SOAP 1.1服务转成SOAP 1.2。
把一个服务路由到另一个服务上，也可以集中化管理业务逻辑，规则和验证等等。
它还有一个重要功能是消息队列和事件驱动的消息传递，比如把JMS服务转化成SOAP协议。各服务间可能有复杂的依赖关系。

## microservices

### 定义

In short, the microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API. These services are built around business capabilities and independently deployable by fully automated deployment machinery. – James Lewis and Martin Fowler

简而言之，微服务架构风格是一种将单个应用程序开发为一套小型服务的方法，每个小型服务都在自己的流程中运行，并与轻量级机制（通常是HTTP资源API）进行通信。这些服务围绕业务功能构建，可通过全自动部署机制独立部署。 – 詹姆斯·里维斯，马丁·福勒

##### 旁白

这里有几个关键词，小型服务，独立部署，全自动部署，
小型服务就是我们通常意义上指的微服务，微服务的一个重要设计原则就是应该设计成边界清晰，数据独享，也就是我们常说的高内聚、低耦合，
保证了这点才能做到独立部署，而实现全自动部署，要靠的就是DevOps文化

### 架构图

[microservices](https://e5ce463uma323hyvrr4xumqs-wpengine.netdna-ssl.com/wp-content/uploads/2018/11/microservice.png)

##### 旁白

从本质上来看，相对单体应用，微服务是以牺牲强一致性、提高部署复杂性为代价，换取更彻底的分布式特性，比如异构性和强隔离性。
异构性比较容易理解，通过定义统一的API规范（一般采用REST风格），每个微服务团队可以根据各自的能力矩阵选用最适合的技术栈，而不是所有人必须使用相同的技术栈。
强隔离性指的是，对于一个典型的单体应用，隔离性最高只能体现到模块级别，由于共享同一个代码仓库，模块的边界往往比较模糊，而到了微服务阶段，自带应用级别的隔离性，无需任何规范，架构本身就保证了隔离性。
另一方面，由于采用了分布式架构，微服务无法再简单的通过数据库事务来保证强一致性，而是通过消息中间件或者事务补偿机制来保证最终一致性。
其次，在微服务阶段，随着应用数量的激增，一次发布往往涉及多个应用，加上异构性带来的部署方式的多样性，对团队的运维水平尤其是自动化水平提出了更高的要求，运维和开发的边界进一步模糊。

### 优劣

#### 强项

+ 强模块化边界
+ 可独立部署
+ 技术多样性

#### 弱项

+ 分布式复杂性
+ 最终一致性
+ 运维复杂性
+ 测试复杂性

## service mesh

### 现有微服务的问题：

+ 技术门槛高: 随着微服务实施水平的不断深化，除了基础的服务发现、配置中心和授权管理之外，团队将不可避免的在服务治理层面面临各类新的挑战，包括但不限于分布式跟踪、熔断降级、灰度发布、故障切换等，这对团队提出了非常高的技术要求。
+ 多语言支持不足: 对于稍具规模的团队，尤其在高速成长的互联网创业公司，多语言的技术栈是常态，跨语言的服务调用也是常态，但目前开源社区上并没有一套统一的、跨语言的微服务技术栈。
+ 代码侵入性强: 主流的微服务框架（比如Spring Cloud、Dubbo）或多或少都对业务代码有一定的侵入性，框架替换成本高，导致业务团队配合意愿低，微服务落地困难。

### 定义

A service mesh is a dedicated infrastructure layer for handling service-to-service communication. It’s responsible for the reliable delivery of requests through the complex topology of services that comprise a modern, cloud native application. In practice, the service mesh is typically implemented as an array of lightweight network proxies that are deployed alongside application code, without the application needing to be aware. - William Morgan

Service Mesh 是一个基础设施层，用于处理服务间通信。云原生应用有着复杂的服务拓扑，Service Mesh 保证请求可以在这些拓扑中可靠地穿梭。在实际应用当中，Service Mesh 通常是由一系列轻量级的网络代理组成的，它们与应用程序部署在一起，但应用程序不需要知道它们的存在。- 威廉·摩根

##### 旁白

这里有几个关键词，服务间通讯，基础设施层，轻量网络代理
处理服务间通讯：指明了服务网格的指责，这正是服务治理的核心所在
基础设施层：将服务网格和之前的微服务框架划清了界限，也即服务网格独立于具体的服务而存在
这从根本上解决了前面提到的老的微服务框架在多语言支持和代码侵入方面存在的问题。
业务团队不再需要操心服务治理相关的复杂度，全权交给服务网格处理即可。

### 架构图

[架构图](https://jimmysong.io/istio-handbook/images/00704eQkly1fswh7dbs1pj30id0bpmxl.jpg)

##### 旁白

Service Mesh 是服务的前置代理层的实现，采用 sidecar 设计模式
图中应用作为服务的发起方，只需要用最简单的方式将请求发送给本地的服务网格代理，然后网格代理会进行后续的操作，如服务发现，负载均衡，最后将请求转发给目标服务
针对每一个服务实例，服务网格都会在同一主机上一对一并行部署一个 sidecar 进程，接管该服务实例所有对外的网络通讯
这样就实现了路由转发，策略控制，认证授权，数据监测等功能
这样就去除了代理模式下中心化架构的瓶颈。同时，借助于良好的框架封装，运维成本也可以得到有效的控制

[服务网格](https://jimmysong.io/istio-handbook/images/006tNc79ly1fz73xstibij30b409cmyh.jpg)

##### 旁白

在上图中绿色方块为服务，蓝色方块为 sidecar 部署的服务网格，蓝色线条为服务间通讯。可以看到蓝色的方块和线条组成了整个网格
当有大量服务相互调用时，它们之间的服务调用关系就会形成网格


<!--->

16年1月，离开Twitter的工程师William Morgan，在github上发布了Linkerd 0.0.7版本，业界第一个Service Mesh项目就此诞生。Linkerd基于Twitter的开源项目，但是实现了通用性，成为了业界第一个Service Mesh项目。而Envoy是第二个Service Mesh项目，两者的开发时间差不多，在2017年都相继成为CNCF项目。
17年5月，Google/IBM/Lyft联手发布Istio 0.1版本
在第一代Service Mesh产品如Linkerd、Envoy刚刚发展成熟，正要开始逐渐推广时，以Istio为代表的第二代Service Mesh产品就突然登场，直接改变市场格局。
第二代Service Mesh和第一代Service Mesh的差异在于是否有控制平面：
第一代Service Mesh只有数据平面（即Sidecar），所有功能都在Sidecar中实现
第二代Service Mesh增加了控制平面，带来了远超第一代的控制力，功能也更加丰富

Istio最大的创新是它为Service Mesh带来了前所未有的控制力：
以Sidecar方式部署的Service Mesh控制了服务间所有的流量
Istio增加了控制面板来控制系统中所有的Sidecar
这样Istio就能够控制所有的流量，也就是控制系统中的所有请求的发送
Istio出现之后，Linkerd陷入困境，Envoy则作为Istio的数据平面和Istio一起发展。随后Buoyant公司推出了全新的Conduit应对Istio的强力竞争，我们将在下一章中详细讲述Service Mesh的开源产品和市场竞争。

<--->

# 架构浅析

# 服务注册发现

## 概念

微服务实例的网络位置发生变化是一种常态，所以必须提供一种机制，使得服务消费者在服务提供者的网络位置发生变化时，能够及时获得最新的位置信息，服务注册中心本质上是为了解耦服务提供者和服务消费者。

##### 旁白

其实，我们日常的很多普通操作，都是在做服务发现
比如通过域名访问服务
我们在浏览器输入域名，DNS 服务器会根据我们的域名解析出一个 IP 地址，这就是最常见的服务发现。
而在微服务的领域，我们将应用拆分成一个个的微服务之后，服务发现，则变成了微服务之间相互获取彼此的信息。
一般是提供一个网络位置稳定的服务注册中心，服务提供者的网络位置被注册到注册中心，并在网络位置发生变化的时候及时更新，
而消费者定期向注册中心获取服务提供者的最新位置信息，这就是最基本的服务发现机制。
较为复杂的服务发现实现除了服务提供者的位置信息外，还可以向服务消费者提供服务提供者的描述信息、状态信息和资源使用信息，以供服务消费者实现更为复杂的服务选择逻辑
<!--->
然而，在微服务的场景下，使用DNS服务器作为服务发现会存在一些问题。
DNS服务器不支持动态变更，不能够随着服务的状态变更（上线、下线、故障）而对域名映射变更
DNS只能支持域名和ip地址的一一映射，但在微服务的场景中，很多微服务都会部署多个实例，这也就要求标志与服务要有一对多的映射
DNS服务无法解决多数据中心的问题。
<--->

# 服务发现机制

|客户端模式|服务端模式|
|-|-|
|只需要周期性获取列表，在调用服务时可以直接调用少了一个RT。但需要在每个客户端维护获取列表的逻辑|简单，不需要在客户端维护获取服务列表的逻辑|
|可用性高，即使注册中心出现故障也能正常工作|可用性由路由器中间件决定，路由中间件故障则所有服务不可用，同时，由于所有调度以及存储都由中间件服务器完成，中间件服务器可能会面临过高的负载|
|服务上下线对调用方有影响（会出现短暂调用失败）|服务上下线调用方无感知|

##### 旁白

两者的本质区别在于，客户端是否保存服务列表信息

1. 客户端发现：直接集成到应用中，依赖于应用自身完成服务的注册与发现，ZooKeeper或者Etcd, 最典型的是Netflix提供的Eureka
2. 服务端发现：在服务端模式下，调用方直接向服务注册中心进行请求，服务注册中心再通过自身负载均衡策略，对微服务进行调用。
这个模式下，调用方不需要在自身节点维护服务发现逻辑以及服务注册信息，
这个模式相对来说比较类似DNS模式。把应用当成黑盒，通过应用外的某种机制将服务注册到注册中心，最小化对应用的侵入性，比如Consul

# 主流方案对比

## Zookeeper&Dubbo

[zookeeper](https://s3.amazonaws.com/files.dezyre.com/images/Tutorials/service.png)

##### 旁白

ZK 是 apache 下的，用 java 写的，提供 rpc 接口，最早从 hadoop 项目中孵化出来，是一致性元信息存储，并且提供watch机制用于变更通知和分发
ZK 的设计原则是 CP ，即强一致性和分区容错性。他保证数据的强一致性，但舍弃了可用性，如果出现网络问题可能会影响ZK的选举，导致 ZK 注册中心的不可用。
在粗粒度分布式锁，分布式选主，主备高可用切换等不需要高TPS支持的场景下有不可替代的作用，而这些需求往往多集中在大数据、离线任务等相关的业务领域，
因为大数据领域，讲究分割数据集，并且大部分时间分任务多进程/线程并行处理这些数据集，但是总是有一些点上需要将这些任务和进程统一协调，这时候就是 ZK 发挥巨大作用的用武之地。
但是在交易场景交易链路上，在主业务数据存取，大规模服务发现、大规模健康监测等方面有天然的短板，应该竭力避免在这些场景下引入 ZK，应用对 ZK 申请使用的时候要进行严格的场景、容量、SLA需求的评估。

ZK质量比较好的客户端仅有 java 和 c 的客户端
Client/Session 的状态机难以理解
连接丢失异常不好处理

[Dubbo](http://www.talkwithtrend.com/home/attachment/201711/16/418679_151079877963626.png)

##### 旁白

dubbo 支持了 ZK、Redis等，官方推荐 ZK
dubbo 核心部件如上图所示
Provider：暴露服务的提供方，可以通过jar或者容器的方式启动服务
Consumer：调用远程服务的服务消费方。
Registry：服务注册中心和发现中心。
Monitor：统计服务和调用次数，调用时间监控中心。（dubbo的控制台页面中可以显示，目前只有一个简单版本）
Container：服务运行的容器。

<!--->
Etcd，Zookeeper，Consul 比较
这三个产品是经常被人拿来做选型比较的。 Etcd 和 Zookeeper 提供的能力非常相似，都是通用的一致性元信息存储，都提供watch机制用于变更通知和分发，也都被分布式系统用来作为共享信息存储，在软件生态中所处的位置也几乎是一样的，可以互相替代的。二者除了实现细节，语言，一致性协议上的区别，最大的区别在周边生态圈。Zookeeper 是apache下的，用java写的，提供rpc接口，最早从hadoop项目中孵化出来，在分布式系统中得到广泛使用（hadoop, solr, kafka, mesos 等）。Etcd 是coreos公司旗下的开源产品，比较新，以其简单好用的rest接口以及活跃的社区俘获了一批用户，在新的一些集群中得到使用（比如kubernetes）。虽然v3为了性能也改成二进制rpc接口了，但其易用性上比 Zookeeper 还是好一些。 而 Consul 的目标则更为具体一些，Etcd 和 Zookeeper 提供的是分布式一致性存储能力，具体的业务场景需要用户自己实现，比如服务发现，比如配置变更。而Consul 则以服务发现和配置变更为主要目标，同时附带了kv存储。 在软件生态中，越抽象的组件适用范围越广，但同时对具体业务场景需求的满足上肯定有不足之处。

<--->

### Eureka&Spring Cloud

![eureka](https://raw.githubusercontent.com/Netflix/eureka/master/images/eureka_architecture.png)

##### 旁白

Eureka是netflix开源的服务发现组件，采用 Java 实现
设计原则是 AP，即可用性和分区容错性。他保证了注册中心的可用性，但舍弃了数据一致性，各节点上的数据有可能是不一致的（会最终一致）。

如上图所示，Eureka采用了客户端的模式，服务首先需要向注册中心注册，调用方则需要在本地维护一个服务注册列表。
具体的操作为：client/server通过RESTful Api向server进行服务注册，并且定期调用renew接口来更新服务的注册状态，若server在60s内没有收到服务的renew信息，则该服务就会被标志为下线。而如果服务需要主动下线的话，向server调用cancel就可以了。
Eureka同时支持多个注册中心，以保证注册中心的高可用性。
在多注册中心（server）的情况下，单个server在接收到服务的注册/更新信息的时候，它还会将这些信息同步给同样为server的peer(replicate to peer)，为了避免广播风暴，这些信息只会传递一次，也就是说，接收到的server，不会再同步给自身的peer。
服务注册完成之后，当client需要进行服务调用的时候，就可以向server获取当前的服务列表，再根据服务列表中的ip地址以及端口号进行调用了。
由于eureka的相关配置都是存储在配置文件中，因此如果需要动态增加server数量的话，就必须修改配置重启client，以确保server的一致性。所以在服务器端弹性配置上不够灵活。还有一个缺点是，由于一个client需要维持与多个server之间的联系，这个会占用额外的系统资源，另外这个框架在github上也有将近一年没有更新了。
Eureka 客户端负责处理服务实例注册与注销的所有方面。实现了包括服务发现在内的多种模式的 Spring Cloud 项目可以轻松地使用 Eureka 自动注册服务实例。

Application Service： 作为Eureka Client，扮演了服务的提供者，提供业务服务，向Eureka Server注册和更新自己的信息，同时能从Eureka Server的注册表中获取到其他服务的信息。
Eureka Server：扮演服务注册中心的角色，提供服务注册和发现的功能，每个Eureka Cient向Eureka Server注册自己的信息，也可以通过Eureka Server获取到其他服务的信息达到发现和调用其他服务的目的。
Application Client：作为Eureka Client，扮演了服务消费者，通过Eureka Server获取到注册到上面的其他服务的信息，从而根据信息找到所需的服务发起远程调用。
Replicate： Eureka Server中的注册表信息的同步拷贝，保持不同的Eureka Server集群中的注册表中的服务实例信息的一致性。提供了数据的最终一致性。
Make Remote Call： 服务之间的远程调用。
Register： 注册服务实例，Client端向Server端注册自身的元数据以进行服务发现。
Renew：续约，通过发送心跳到Server维持和更新注册表中的服务实例元数据的有效性。当在一定时长内Server没有收到Client的心跳信息，将默认服务下线，将服务实例的信息从注册表中删除。
Cancel：服务下线，Client在关闭时主动向Server注销服务实例元数据，这时Client的的服务实例数据将从Server的注册表中删除。

Eureka中没有使用任何的数据强一致性算法保证不同集群间的Server的数据一致，仅通过数据拷贝的方式争取注册中心数据的最终一致性，虽然放弃数据强一致性但是换来了Server的可用性，降低了注册的代价，提高了集群运行的健壮性。

![spring cloud调用图](https://cloud.githubusercontent.com/assets/6069066/13906840/365c0d94-eefa-11e5-90ad-9d74804ca412.png)

##### 旁白

Spring Cloud支持了Zookeeper、Consul和Eureka，官方推荐Eureka, Eureka 也不是单独使用的，一般会配合 ribbon 一起使用，ribbon 作为路由和负载均衡。
Ribbon提供多种内建的负载均衡规则：
平均加权响应时间，随机，可用性过滤（避免跳闸线路和高并发链接数），自定义负载均衡插件系统
整体流程就是请求统一通过API网关（Zuul）来访问内部服务，在网关这一步做认证和鉴权
认证和鉴权后，网关从注册中心（Eureka）获取可用服务
由 ribbon 进行均衡负载后，分发到后端具体实例
微服务之间通过 feign 进行通信处理业务
Hystrix负责处理服务超时熔断，它支持基于失败比率的熔断方式
Turbine监控服务间的调用和熔断相关指标
Sleuth + Zipkin将所有的请求数据记录下来，方便我们进行后续分析（分布式链路跟踪）


### coreDNS&Kubernetes

![kuberproxy](https://img2018.cnblogs.com/blog/907596/201903/907596-20190325170554554-1168234966.png)

coreDNS是采用Go语言，Raft算法，基于插件的解决方案取代了早期的skyDNS和kubeDNS的方式，skyDNS体系不够灵活，较难添加新功能或进行扩展，而kubeDNS在运行时分3个容器，容易出现dnsmasq漏洞。
coreDNS是基于DNS的动态服务发现的解决方案，它是将服务注册为DNS的SRV记录的方式来实现服务发现，与常见的A记录、cname不同的是还记录了服务的端口,并且可以设置每个服务地址的优先级和权重，
coreDNS运行时只需启动一个容器，也没有漏洞方面的问题，提供的ConfigMap工具可动态更新配置。基于插件的架构使用户可以插件的方式随时添加或删除功能。目前，CoreDNS已包括30多个自有插件和20个以上的外部插件。
通过链接插件，用户可以启用Prometheus监控、Jaeger日志跟踪、Fluentd日志记录、Kubernetes API或etcd配置以及其他的高级DNS特性和集成功能。


还不能支持较为复杂的LB特性，例如会话保持，nginx controller扩展了ingress可以做会话保持但必须使用nginx plus。

业务规则发生变化，会导致频繁修改ingress配置，且所变化都会引起nginx reload配置，这在大并发，复杂系统环境下可能会产生较大问题

外部访问的入口依赖使用controller所在node的宿主IP，且端口暴露在node IP上，对于多个应用需要同时使用相同端口时候会导致端口冲突

外部访问入口分散在多个node IP上，使得外部统一访问变得困难，且存在潜在的单点故障，一个node 失败，客户端只有重新发起到新的node节点的访问（虽然可以使用dns来轮询这些IP），因此有必要为这些入口访问点构建统一的虚拟IP（漂移IP），这可以通过在k8s环境外部使用其它高级LB来实现，比如F5。或者将多个提供访问的node节点构建为一个集群（比如使用keepalived）

所以基本上ingress的方案还得需要一个外部LB来做统一的高容量请求入口，ingress只能作为k8s内部的二级LB（且功能有限）。与其这样，不如直接通过将pod暴露给外部LB来直接做负载均衡

nginx无图形界面统一管理，nginx plus有 但是是商业版本。

ingress没有业务监控检查功能

只支持http L7

不能混合支持L4/L7 LB

不支持TLS的SNI，以及SSL re-encrypt

### Envoy&Istio

![envoy架构](https://jimmysong.io/kubernetes-handbook/images/envoy-arch.png)

Envoy 是 Istio Service Mesh 中默认的 Sidecar，Istio 在 Enovy 的基础上按照 Envoy 的 xDS 协议扩展了其控制平面，
Envoy 是一款由 Lyft 开源的，使用 C++ 编写的 L7 代理和通信总线，目前是 CNCF 旗下的开源项目且已经毕业，代码托管在 GitHub 上，它也是 Istio service mesh 中默认的 data plane

上图是 Envoy proxy 的架构图，显示了 host B 经过 Envoy 访问 host A 的过程。每个 host 上都可能运行多个 service，Envoy 中也可能有多个 Listener，每个 Listener 中可能会有多个 filter 组成了 chain

Host：能够进行网络通信的实体（在手机或服务器等上的应用程序）。在 Envoy 中主机是指逻辑网络应用程序。只要每台主机都可以独立寻址，一块物理硬件上就运行多个主机。

Downstream：下游（downstream）主机连接到 Envoy，发送请求并或获得响应。

Upstream：上游（upstream）主机获取来自 Envoy 的链接请求和响应。

Cluster: 集群（cluster）是 Envoy 连接到的一组逻辑上相似的上游主机。Envoy 通过服务发现发现集群中的成员。Envoy 可以通过主动运行状况检查来确定集群成员的健康状况。Envoy 如何将请求路由到集群成员由负载均衡策略确定。

Mesh：一组互相协调以提供一致网络拓扑的主机。Envoy mesh 是指一组 Envoy 代理，它们构成了由多种不同服务和应用程序平台组成的分布式系统的消息传递基础。

运行时配置：与 Envoy 一起部署的带外实时配置系统。可以在无需重启 Envoy 或 更改 Envoy 主配置的情况下，通过更改设置来影响操作。

Listener: 监听器（listener）是可以由下游客户端连接的命名网络位置（例如，端口、unix域套接字等）。Envoy 公开一个或多个下游主机连接的侦听器。一般是每台主机运行一个 Envoy，使用单进程运行，但是每个进程中可以启动任意数量的 Listener（监听器），目前只监听 TCP，每个监听器都独立配置一定数量的（L3/L4）网络过滤器。Listenter 也可以通过 Listener Discovery Service（LDS）动态获取。

Listener filter：Listener 使用 listener filter（监听器过滤器）来操作链接的元数据。它的作用是在不更改 Envoy 的核心功能的情况下添加更多的集成功能。Listener filter 的 API 相对简单，因为这些过滤器最终是在新接受的套接字上运行。在链中可以互相衔接以支持更复杂的场景，例如调用速率限制。Envoy 已经包含了多个监听器过滤器。

Http Route Table：HTTP 的路由规则，例如请求的域名，Path 符合什么规则，转发给哪个 Cluster。

Health checking：健康检查会与SDS服务发现配合使用。但是，即使使用其他服务发现方式，也有相应需要进行主动健康检查的情况。详见 health checking。

![Istio service mesh 架构图](https://jimmysong.io/istio-handbook/images/006tNc79ly1fz73sprcdlj31580u046j.jpg)


##### 旁白

该图中描绘了以下内容：

Istio 可以在虚拟机和容器中运行
Istio 的组成
Pilot：服务发现、流量管理
Mixer：访问控制、遥测
Citadel：终端用户认证、流量加密
Service mesh 关注的方面
可观察性
安全性
可运维性
Istio 是可定制可扩展的，组建是可拔插的
Istio 作为控制平面，在每个服务中注入一个 Envoy 代理以 Sidecar 形式运行来拦截所有进出服务的流量，同时对流量加以控制
应用程序应该关注于业务逻辑（这才能生钱），非功能性需求交给 Service Mesh



# 什么时候引入微服务

## 康威定律

Any organization that design a system (defined broadly) will produce a design whose structure is a copy of the organization’s communication structure. - Melvin Conway

设计系统的架构受制于产生这些设计的组织的沟通结构。- Melvin Conway

##### 旁白

即系统设计本质上反映了企业的组织机构。
系统各个模块间的接口也反映了企业各个部门之间的信息流动和合作方式。
康威定律源于模块的设计者需要互相之间频繁沟通。而跨部门交流比较难。
架构的本质时管理复杂性，微服务本身也是架构演化的结果，与单体应用拆分为微服务的过程类似，随着公司规模的不断扩大，一个组织势必会分化出多个更小的组织。
根据康威定律，组织结构决定系统结构，因此，从这个层面来说，微服务也是一种必然。

## 领域知识

DDD

##### 旁白

除了组织架构和技术取舍，领域知识是另一个非常重要的决策因素。
对于不熟悉的业务领域，很难第一次就把各个微服务的边界和接口定义正确，一旦开始开发，重构成本就会非常可观。
反过来说，当对领域知识有了一定的积累，再重构一个单体应用就会容易的多。
综上所述，虽然微服务看上去很美，但在决定采用微服务架构之前，不仅要仔细考量团队的技术水平（包括知识结构，理论深度，经验积累和技术氛围），还应综合考虑项目的时间范围，领域知识的熟悉程度，以及所在组织的规模架构。




