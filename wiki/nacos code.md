# Nacos 架构

## 基本架构及概念

> ![nacos_arch.jpg](/Users/galminzhou/Documents/MarkDown/nacos-Multi-Datacenter Nacos Cluster.png)

### 服务 (Service)

服务是指一个或一组软件功能（例如特定信息的检索或一组操作的执行），其目的是不同的客户端可以为不同的目的重用（例如通过跨进程的网络调用）。

### 服务注册中心 (Service Registry)

服务注册中心，它是服务，其实例及元数据的数据库。服务实例在启动时注册到服务注册表，并在关闭时注销。服务和路由器的客户端查询服务注册表以查找服务的可用实例。服务注册中心可能会调用服务实例的健康检查 API 来验证它是否能够处理请求。

### 服务元数据 (Service Metadata)

服务元数据是指包括服务端点(endpoints)、服务标签、服务版本号、服务实例权重、路由规则、安全策略等描述服务的数据

### 服务提供方 (Service Provider)

是指提供可复用和可调用服务的应用方

### 服务消费方 (Service Consumer)

是指会发起对某个服务调用的应用方

### 配置 (Configuration)

在系统开发过程中通常会将一些需要变更的参数、变量等从代码中分离出来独立管理，以独立的配置文件的形式存在。目的是让静态的系统工件或者交付物（如 WAR，JAR 包等）更好地和实际的物理运行环境进行适配。配置管理一般包含在系统部署的过程中，由系统管理员或者运维人员完成这个步骤。配置变更是调整系统运行时的行为的有效手段之一。

### 配置管理 (Configuration Management)

在数据中心中，系统中所有配置的编辑、存储、分发、变更管理、历史版本管理、变更审计等所有与配置相关的活动统称为配置管理。

### 名字服务 (Naming Service)

提供分布式系统中所有对象(Object)、实体(Entity)的“名字”到关联的元数据之间的映射管理服务，例如 ServiceName -> Endpoints Info, Distributed Lock Name -> Lock Owner/Status Info, DNS Domain Name -> IP List, 服务发现和 DNS 就是名字服务的2大场景。

### 配置服务 (Configuration Service)

在服务或者应用运行过程中，提供动态配置或者元数据以及配置管理的服务提供者。

### 地域

物理的数据中心，资源创建成功后不能更换。

### 可用区

同一地域内，电力和网络互相独立的物理区域。同一可用区内，实例的网络延迟较低。

### 接入点

地域的某个服务的入口域名。

### 命名空间

用于进行租户粒度的配置隔离。不同的命名空间下，可以存在相同的 Group 或 Data ID 的配置。Namespace 的常用场景之一是不同环境的配置的区分隔离，例如开发测试环境和生产环境的资源（如配置、服务）隔离等。

### 配置

在系统开发过程中，开发者通常会将一些需要变更的参数、变量等从代码中分离出来独立管理，以独立的配置文件的形式存在。目的是让静态的系统工件或者交付物（如 WAR，JAR 包等）更好地和实际的物理运行环境进行适配。配置管理一般包含在系统部署的过程中，由系统管理员或者运维人员完成。配置变更是调整系统运行时的行为的有效手段。

### 配置管理

系统配置的编辑、存储、分发、变更管理、历史版本管理、变更审计等所有与配置相关的活动。

### 配置项

一个具体的可配置的参数与其值域，通常以 param-key=param-value 的形式存在。例如我们常配置系统的日志输出级别（logLevel=INFO|WARN|ERROR） 就是一个配置项。

### 配置集

一组相关或者不相关的配置项的集合称为配置集。在系统中，一个配置文件通常就是一个配置集，包含了系统各个方面的配置。例如，一个配置集可能包含了数据源、线程池、日志级别等配置项。

### 配置集 ID

Nacos 中的某个配置集的 ID。配置集 ID 是组织划分配置的维度之一。Data ID 通常用于组织划分系统的配置集。一个系统或者应用可以包含多个配置集，每个配置集都可以被一个有意义的名称标识。Data ID 通常采用类 Java 包（如 com.taobao.tc.refund.log.level）的命名规则保证全局唯一性。此命名规则非强制。

### 配置分组

Nacos 中的一组配置集，是组织配置的维度之一。通过一个有意义的字符串（如 Buy 或 Trade ）对配置集进行分组，从而区分 Data ID 相同的配置集。当您在 Nacos 上创建一个配置时，如果未填写配置分组的名称，则配置分组的名称默认采用 DEFAULT_GROUP 。配置分组的常见场景：不同的应用或组件使用了相同的配置类型，如 database_url 配置和 MQ_topic 配置。

### 配置快照

Nacos 的客户端 SDK 会在本地生成配置的快照。当客户端无法连接到 Nacos Server 时，可以使用配置快照显示系统的整体容灾能力。配置快照类似于 Git 中的本地 commit，也类似于缓存，会在适当的时机更新，但是并没有缓存过期（expiration）的概念。

### 服务

通过预定义接口网络访问的提供给客户端的软件功能。

### 服务名

服务提供的标识，通过该标识可以唯一确定其指代的服务。

### 服务注册中心

存储服务实例和服务负载均衡策略的数据库。

### 服务发现

在计算机网络上，（通常使用服务名）对服务下的实例的地址和元数据进行探测，并以预先定义的接口提供给客户端进行查询。

### 元信息

Nacos数据（如配置和服务）描述信息，如服务版本、权重、容灾策略、负载均衡策略、鉴权配置、各种自定义标签 (label)，从作用范围来看，分为服务级别的元信息、集群的元信息及实例的元信息。

### 应用

用于标识服务提供方的服务的属性。

### 服务分组

不同的服务可以归类到同一分组。

### 虚拟集群

同一个服务下的所有服务实例组成一个默认集群, 集群可以被进一步按需求划分，划分的单位可以是虚拟集群。

### 实例

提供一个或多个服务的具有可访问网络地址（IP:Port）的进程。

### 权重

实例级别的配置。权重为浮点数。权重越大，分配给该实例的流量越大。

### 健康检查

以指定方式检查服务下挂载的实例 (Instance) 的健康度，从而确认该实例 (Instance) 是否能提供服务。根据检查结果，实例 (Instance) 会被判断为健康或不健康。对服务发起解析请求时，不健康的实例 (Instance) 不会返回给客户端。

### 健康保护阈值

为了防止因过多实例 (Instance) 不健康导致流量全部流向健康实例 (Instance) ，继而造成流量压力把健康 健康实例 (Instance) 压垮并形成雪崩效应，应将健康保护阈值定义为一个 0 到 1 之间的浮点数。当域名健康实例 (Instance) 占总服务实例 (Instance) 的比例小于该值时，无论实例 (Instance) 是否健康，都会将这个实例 (Instance) 返回给客户端。这样做虽然损失了一部分流量，但是保证了集群的剩余健康实例 (Instance) 能正常工作。

## 逻辑架构及其组件介绍

> ![nacos-logic.jpg](/Users/galminzhou/Documents/MarkDown/nacos-逻辑架构.png)
>
> ----
>
> - **服务管理**：实现服务CRUD，域名CRUD，服务健康状态检查，服务权重管理等功能
> - **配置管理**：实现配置管CRUD，版本管理，灰度管理，监听管理，推送轨迹，聚合数据等功能
> - 元数据管理：提供元数据CURD 和打标能力
> - 插件机制：实现三个模块可分可合能力，实现扩展点SPI机制
> - 事件机制：实现异步化事件通知，sdk数据变化异步通知等逻辑
> - 日志模块：管理日志分类，日志级别，日志可移植性（尤其避免冲突），日志格式，异常码+帮助文档
> - 回调机制：sdk通知数据，通过统一的模式回调用户处理。接口和数据结构需要具备可扩展性
> - 寻址模式：解决ip，域名，nameserver、广播等多种寻址模式，需要可扩展
> - 推送通道：解决server与存储、server间、server与sdk间推送性能问题
> - 容量管理：管理每个租户，分组下的容量，防止存储被写爆，影响服务可用性
> - 流量管理：按照租户，分组等多个维度对请求频率，长链接个数，报文大小，请求流控进行控制
> - 缓存机制：容灾目录，本地缓存，server缓存机制。容灾目录使用需要工具
> - 启动模式：按照单机模式，配置模式，**服务模式**，dns模式，或者all模式，启动不同的程序+UI
> - 一致性协议：解决不同数据，不同一致性要求情况下，不同一致性机制
> - 存储模块：解决数据持久化、非持久化存储，解决数据分片问题
> - Nameserver：解决namespace到clusterid的路由问题，解决用户环境与nacos物理环境映射问题
> - CMDB：解决元数据存储，与三方cmdb系统对接问题，解决应用，人，资源关系
> - Metrics：暴露标准metrics数据，方便与三方监控系统打通
> - Trace：暴露标准trace，方便与SLA系统打通，日志白平化，推送轨迹等能力，并且可以和计量计费系统打通
> - 接入管理：相当于阿里云开通服务，分配身份、容量、权限过程
> - 用户管理：解决用户管理，登录，sso等问题
> - 权限管理：解决身份识别，访问控制，角色管理等问题
> - 审计系统：扩展接口方便与不同公司审计系统打通
> - 通知系统：核心数据变更，或者操作，方便通过SMS系统打通，通知到对应人数据变更
> - OpenAPI：暴露标准Rest风格HTTP接口，简单易用，方便多语言集成
> - Console：易用控制台，做服务管理、配置管理等操作
> - SDK：多语言sdk
> - Agent：dns-f类似模式，或者与mesh等方案集成
> - CLI：命令行对产品进行轻量化管理，如：GIT一样

## 领域模型

### 数据模型

  ```markdown
Nacos 数据模型 Key 由三元组唯一确定, Namespace默认是空串，公共命名空间（public），分组默认是 DEFAULT_GROUP。
  ```

>**Nacos data model** 
>
>![nacos_data_model](/Users/galminzhou/Documents/MarkDown/nacos-nacos data model.png)

-----

### 服务领域模型

> ![nacos_naming_data_model](/Users/galminzhou/Documents/MarkDown/nacos-服务领域模型.png)

----

# NACOS 服务发现 - 注册实例

```markdown
# 注册一个实例到服务 POST /nacos/v1/ns/instance
* 请求示例：
curl -X POST 'http://127.0.0.1:8848/nacos/v1/ns/instance?port=8848&healthy=true&ip=192.168.96.147&weight=1.0&serviceName=nacos.test.1&encoding=UTF-8&namespaceId=test'

----------------------------------------------
curl -X POST 'http://127.0.0.1:8848/nacos/v1/ns/instance?port=8848&healthy=true&ip=192.168.96.147&weight=1.0&serviceName=nacos.test.09&encoding=UTF-8&namespaceId=test&groupName=test_hk_01&ephemeral=false'
```

```markdown
# Overview
* 在Nacos1.0版本之后，NACOS在CAP（Consistency Availability Pertition tolerance）理论模型中，
* 在实际中CAP不可能同时满足，因此采用 AP（可用性+分区容错性）和CP（一致性+分区容错性）模型并存方式保存实例数据。
# CAP [Consistency:一致性 Availability:可用性 Partition tolerance:分区容错性]
* CP[强一致性][PersistentConsistencyService]-持久性实例-协议[ProtoBuf][JRaft+Disruptor]
* AP[弱一致性][EphemeralConsistencyService] -临时性实例-协议[Distro][Distro+]
# KEY --- com.alibaba.nacos.naming.iplist+{namespaceId}+##+{serviceName}
* public class PersistentConsistencyServiceDelegateImpl implements PersistentConsistencyService {
		// 在注册持久实例的情况下使用 CP模型，使用SOFAJRaft实现，
		// 通过Raft-日志复制（Log Replication）的机制实现数据的强一致性，Raft使用同步数据，刷新数据到磁盘or数据库。
}
# KEY --- com.alibaba.nacos.naming.iplist.ephemeral+{namespaceId}+##+{serviceName}
* public class DistroConsistencyServiceImpl implements EphemeralConsistencyService, DistroDataProcessor {
		// 在注册临时实例的情况下使用 AP模型，此种类型不需要将数据保存在磁盘或者数据库，
		// 因为临时数据通常和服务器保持一个SESSION会话，此会话只要存在，则数据就不会丢失。
}
```

```markdown
# Workflow
* 1. 解析实例请求参数，并校验参数是否合法（IP，权重分配等），若存在元数据则需解析JSON数据内容等；
* 2. 创建一个实例（若实例不存在），并将实例添加到对应的实例列表中；
*  2.1 PUT数据至内存注册表（service - cluster - instance）[Map(namespace, Map(group::serviceName, Service))]
*  2.2 实例初始化
*    2.2.1 创建实例的状态检查的延迟线程【start ClientBeatCheckTask 】，启动线程之后延迟5秒运行，以后每隔5秒执行一次定时检查；
*    2.2.2 定时执行任务 - 若某个实例超过15秒没有收到心跳，则将它的`healthy`属性设置为`false`;
*          超过15秒（系统默认） - 开发人员可通过在元数据中设置 - preserved.heart.beat.timeout
*    2.2.3 定时执行任务 - 若某个实例超过30秒没有收到心跳，则直接剔除此实例（被剔除的实例若重新发送心跳则重新注册）；
*          超过30秒（系统默认） - 开发人员可通过在元数据中设置 - preserved.ip.delete.timeout
*    元数据的KEYS值可参考 - PreservedMetadataKeys 类
* 2.3 注册临时服务监听（Map<String, ConcurrentLinkedQueue<RecordListener>>）
* 2.4 注册持久服务监听（Map<String, ConcurrentHashSet<RecordListener>> ）并通知数据CHANGE
*    2.4.1 若不存在Cluster，则创建Cluster，并初始化并启动健康检查线程，启动线程之后延迟（2000ms～7000ms）执行，并且只执行一次；
*         【HealthCheckTask】，若(volatile boolean cancelled)属性不是true，则继续启动健康检查线程。
* 3. 如果是持久服务- 则采用 JRaft[日志复制] + Disruptor[高并发队列] +ProtoBuf[二进制序列消息]的方式，同步数据至其它节点。
* ?. 再次检测服务是否有效，若不存在（实例状态心跳检测可能会移除不健康的服务）则直接异常
* ?. 比较并获取最新的服务列表
*    在一次启动Cluster的健康检查线程
*    // Cluster cluster = new Cluster(instance.getClusterName(), service); cluster.init();
* 4. mapConsistencyService(key).put(key, record); --- CP[强一致性=一致性+分区容错性] or AP[弱一致性=可用性+分区容错性]
*   4.1 【AP】将注册实例更新到内存注册表，【Notifier】将任务添加至TASKS阻塞队列中；
*     4.1.1 Notifier是一个单调度执行器服务线程，一直处于Runing状态，若Tasks队列中有数据，则通过Take获取实例数据处理；
*           异步通知处理：采用线程+队列模型 
*           CopyOnWrite思想：/Service#updateIPs
*     4.1.2 同步数据至所有服务器
*   4.2 【CP】如果是持久实例
*     4.2.1 则采用 JRaft[日志复制] + Disruptor[高并发队列] +ProtoBuf[二进制序列消息]的方式，同步数据至其它节点。
*
*
*
*
*
*
```



```java
/****Code** com.alibaba.nacos.naming.controllers.InstanceController#register*/
@RestController
@RequestMapping(UtilsAndCommons.NACOS_NAMING_CONTEXT + "/instance")
public class InstanceController {
    @Autowired
    private SwitchDomain switchDomain;
    
    @Autowired
    private PushService pushService;
    
    @Autowired
    private ServiceManager serviceManager;
    
    @CanDistro
    @PostMapping
    @Secured(parser = NamingResourceParser.class, action = ActionTypes.WRITE)
    public String register(HttpServletRequest request) throws Exception {
        /*略...*/
        // 1. 解析请求参数，分析合法性
        final Instance instance = parseInstance(request);
        // 2. 注册服务实例【核心类 ServiceManager】
        this.serviceManager.registerInstance(namespaceId, serviceName, instance);
    }
}
```



```java
/**
 * Core manager storing all services in Nacos.
 */
@Component
public class ServiceManager implements RecordListener<Service> {
    //Map(namespace, Map(group::serviceName, Service)).
    private final Map<String, Map<String, Service>> serviceMap = new ConcurrentHashMap<>();
    
    @PostConstruct
    public void init() {
    }
    public void registerInstance(String namespaceId, String serviceName, Instance instance) throws NacosException {
        
        //创建服务（若服务不存在）【createServiceIfAbsent(namespaceId, serviceName, local, null)】
        createEmptyService(namespaceId, serviceName, instance.isEphemeral());
        
        Service service = getService(namespaceId, serviceName);
        if (service == null) { // throw new NacosException(...); }
        
        // 将实例添加到服务
        addInstance(namespaceId, serviceName, instance.isEphemeral(), instance);
    }
}

/**
 * 、启动实例的状态检查延迟线程【HealthCheckReactor】、
        // ephemeral:true
        // ephemeral:false非临时实例同步数据提交
 */
private void createServiceIfAbsent(/*...*/) throws NacosException {
    Service service = getService(namespaceId, serviceName);
    if (service == null) {
        // 创建服务实例对象【重点proterty: ClientBeatCheckTask 】
        service = new Service();
        /* set proterty，重新生成MD5值，并判断服务是否有效*/
        service.validate();
      
        // 1. serviceMap.get(namespaceId).put(serviceName, service)
        // 2. 启动实例健康状态心跳检测延迟线程【HealthCheckReactor.scheduleCheck(clientBeatCheckTask)】
        //    initialDelay:5000ms, delay:5000ms 
        //    key == HEART_BEAT_TIMEOUT.HEART_BEAT_TIMEOUT = "preserved.heart.beat.timeout" 
        //        请求参数：metadata JSON 扩展信息中设置，通过key设置一个大于5000ms的值，默认15000ms（15秒）
        // 2. 实例数据同步提交
        // 2. consistencyService.listen->notifier.registerListener(...)【PersistentNotifier 观察者模式】
        putServiceAndInit(service);
    }
}

  
private void addInstance(/*...*/) {
		Service service = getService(namespaceId, serviceName);
    synchronized (service) {
        
    }
}
```

## 持久实例 CP (强一致性)[日志复制]

```markdown




```



## 临时实例 AP (弱一致性)[通知器Notifier]

```markdown





```



---

- [SOFAJRaft Snapshot 原理剖析 | SOFAJRaft 实现原理](http://mp.weixin.qq.com/s?__biz=MzUzMzU5Mjc1Nw%3D%3D&chksm=faa0e7dacdd76ecce2d76c7f74621d38e810649144ad31238f9a43df7bd6ceb2ca6661837e1c&idx=1&mid=2247485440&scene=21&sn=8311b55d7ee88b7702fd5d36a3a97858#wechat_redirect)

- [SOFAJRaft-RheaKV 分布式锁实现剖析 | SOFAJRaft 实现原理](http://mp.weixin.qq.com/s?__biz=MzUzMzU5Mjc1Nw%3D%3D&chksm=faa0e828cdd7613e1f6b52c3dd86f72205353d0c869c1b91bff0ca1a90ff05c2d26ab10d6d68&idx=1&mid=2247485426&scene=21&sn=c1c87f5b773548e506c8839c6dbd7e0f#wechat_redirect)

- [SOFAJRaft 日志复制 - pipeline 实现剖析 | SOFAJRaft 实现原理](http://mp.weixin.qq.com/s?__biz=MzUzMzU5Mjc1Nw%3D%3D&chksm=faa0e84acdd7615cf973a4bfefab7d7642906b5eb553ef927d3a0e8c995071659dc5a10f1f1a&idx=1&mid=2247485328&scene=21&sn=d1322bcbd71855e426524ae809a2a8b8#wechat_redirect)

- [SOFAJRaft-RheaKV MULTI-RAFT-GROUP 实现分析 | SOFAJRaft 实现原理](http://mp.weixin.qq.com/s?__biz=MzUzMzU5Mjc1Nw%3D%3D&chksm=faa0e8adcdd761bb1d05b46f429c0fc87c8994038ca9f2a69531a626d4577fdaa46fdccf99e9&idx=1&mid=2247485303&scene=21&sn=376da729dc166aa81f4f0732b2fbda31#wechat_redirect)

- [SOFAJRaft 选举机制剖析 | SOFAJRaft 实现原理](http://mp.weixin.qq.com/s?__biz=MzUzMzU5Mjc1Nw%3D%3D&chksm=faa0e88bcdd7619d09f7d8c67b67f85296a493f3943de143c00e482ae30b82174345cb2f9e79&idx=1&mid=2247485265&scene=21&sn=2e905b8c018e64baa72c8641049e67b1#wechat_redirect)

- [SOFAJRaft 线性一致读实现剖析 | SOFAJRaft 实现原理](http://mp.weixin.qq.com/s?__biz=MzUzMzU5Mjc1Nw%3D%3D&chksm=faa0e8f8cdd761ee7e4ad1f6765cf91d3aa5f65c797ca2660674ce0c6aafbaeab52db37256e5&idx=1&mid=2247485218&scene=21&sn=17b0db520c2b2c04b947e1eccb704ca1#wechat_redirect)

- [SOFAJRaft-RheaKV 是如何使用 Raft 的 | SOFAJRaft 实现原理](http://mp.weixin.qq.com/s?__biz=MzUzMzU5Mjc1Nw%3D%3D&chksm=faa0e956cdd7604018b53f2749b734faa71a2748ab6b248a7eb80ebd98003706a140aad9c9f2&idx=1&mid=2247485068&scene=21&sn=8dc0913c47a7a0f76424aaea35ab538a#wechat_redirect)

- [蚂蚁金服生产级 Raft 算法库 SOFAJRaft 存储模块剖析 | SOFAJRaft 实现原理](http://mp.weixin.qq.com/s?__biz=MzUzMzU5Mjc1Nw%3D%3D&chksm=faa0e992cdd7608499b5d58a65334653059acc2e35381157724c55d6a50743ba024298c63384&idx=1&mid=2247485000&scene=21&sn=42b6f967b2ad43dd82983929d5800a33#wechat_redirect)

  

# SOFA-JRaft 源码分析

> ```markdown
> SOFAStack （Scalable Open Financial Architecture Stack）
> SOFAJRaft 是一个基于 Raft 一致性算法的生产级高性能 Java 实现，支持 MULTI-RAFT-GROUP，适用于高负载低延迟的场景。
> ```
>
> 英文[论文地址](https://ramcloud.atlassian.net/wiki/download/attachments/6586375/raft.pdf)
>
> 中文[翻译地址](https://github.com/maemual/raft-zh_cn/blob/master/raft-zh_cn.md)

---

## JRaft 定时任务调度器[RepeatedTimer]

> **HashedWheelTimer** 
>
> ![HashedWheelTimer](/Users/galminzhou/Documents/MarkDown/nacos-HashedWheelTimer.png)
>
> HashedWheelTimer通过一定的hash规则（维护一个HashedWheelBucket数组，且长度是一个`2^N`的值，`tick & 2^N-1` slot）将不同timeout的定时任务划分到HashedWheelBucket进行管理，而HashedWheelBucket利用双向链表结构维护了某一时刻需要执行的定时任务列表。
>
> **Wheel** 
>
> 时间轮，是一个HashedWheelBucket数组，数组数量越多，定时任务管理的时间精度越精确。tick每走一格都会将对应的wheel数组里面的bucket拿出来进行调度。
>
> **Worker**
>
> Worker继承自Runnable，HashedWheelTimer必须通过Worker线程操作HashedWheelTimer中的定时任务。Worker是整个HashedWheelTimer的执行流程管理者，控制了定时任务分配、全局deadline时间计算、管理未执行的定时任务、时钟计算、未执行定时任务回收处理。
>
> **HashedWheelTimeout** 
>
> HashedWheelTimer的执行单位，维护了其所属的HashedWheelTimer和HashedWheelBucket的引用、需要执行的任务逻辑、当前轮次以及当前任务的超时时间(不变)等，可以认为是自定义任务的一层Wrapper。
>
> **HashedWheelBucket** 
>
> HashedWheelBucket维护了hash到其内的所有HashedWheelTimeout结构，是一个双向队列。
>
> **HashedWheelTimer的构造器** 
>
> 在初始化RepeatedTimer实例的时候会实例化一个HashedWheelTimer：
>
> ```java
> return new HashedWheelTimer(new NamedThreadFactory(name, true), 1, TimeUnit.MILLISECONDS, 2048);
> ```

### Primary Class Overview

```markdown
# HashedWheelTimer
* 控制Worker工作线程的状态（启动线程、停止线程），添加任务等；
# Worker 
* 内部工作线程，推动时间轮tick（(wheel.length - 1) & tick++），执行Bucket链表的任务数据；
# HashedWheelTimeout
* 封装TaskTimer，一个双向链表结构的数据，主要属性 deadline，remainingRounds等；
# HashedWheelBucket
* wheel数组元素, 负责存储HashedWheelTimeout链表。
```

### HashedWheelTimer#HashedWheelTimer

```markdown
# Creates a new timer
- com.alipay.sofa.jraft.util.timer.DefaultRaftTimerFactory#createTimer
* 创建一个Wheel实例对象，在构造器中主要做一些初始化工作：
- 1. 创建一个时间轮（HashedWheelBucket）数组，且长度是不少于ticksPerWheel的2^n，并且最大不允许超过2^30的一个值；
- 2. 根据数组的长度初始化mask，mask = (HashedWheelBucket数组).length - 1; 用于使用或运算计算slot（mask & tick++）；
- 3. 获取参数【long tickDuration, TimeUnit unit】的纳秒值，this.tickDuration = unit.toNanos()；
- 4. 校验整个时间轮走完的时间（this.tickDuration）不能够过长，超过则会溢出Long类型；
- 5. 将worker包装成WorkerThread（并不会立即启动线程，获取到任务是启动，并使用CountDownLatch保证线程由Runnable -> Running）
- 6. HashedWheelTimer是一个共享资源（很消耗资源的结构），必须跨JVM重用，若实例数太多(目前实例数定义是256)，则打印error日志。
```

```java
public HashedWheelTimer(ThreadFactory threadFactory, long tickDuration, 
                        TimeUnit unit, int ticksPerWheel, long maxPendingTimeouts) {

    if (threadFactory == null) {
        throw new NullPointerException("threadFactory");
    }
    if (unit == null) {
        throw new NullPointerException("unit");
    }
    if (tickDuration <= 0) {
        throw new IllegalArgumentException("tickDuration must be greater than 0: " + tickDuration);
    }
    if (ticksPerWheel <= 0) {
        throw new IllegalArgumentException("ticksPerWheel must be greater than 0: " + ticksPerWheel);
    }

    // Normalize ticksPerWheel to power of two and initialize the wheel.
    // 创建一个时间轮数组，数组长度是不少于ticksPerWheel的2^n，并且不超过2^30，
    // 2^N 主要是用于位运算（例如：或运算（&）），
    // 因为一个2^n的值，都是1...00000（1开头后面都是0）， 2^n-1 则是 0....11111（0开头后面都是1）
    wheel = createWheel(ticksPerWheel);
    mask = wheel.length - 1;

    // Convert tickDuration to nanos.
    this.tickDuration = unit.toNanos(tickDuration);

    // Prevent overflow.
    // 检查是否溢出，即指针转动的时间间隔不能太长而导致 tickDuration*wheel.length>Long.MAX_VALUE
    if (this.tickDuration >= Long.MAX_VALUE / wheel.length) {
        throw new IllegalArgumentException(String.format(
                "tickDuration: %d (expected: 0 < tickDuration in nanos < %d", tickDuration, Long.MAX_VALUE
                        / wheel.length));
    }
    // 将worker包装成WorkerThread，并不会立即启动时间轮，而是等待第一个任务加入到时间轮的时候才会启动
    workerThread = threadFactory.newThread(worker);
    // maxPendingTimeouts = -1
    this.maxPendingTimeouts = maxPendingTimeouts;
    // 若HashedWheelTimer实例数超过256，则打印一个Error日志
    if (instanceCounter.incrementAndGet() > INSTANCE_COUNT_LIMIT
            && warnedTooManyInstances.compareAndSet(false, true)) {
        reportTooManyInstances();
    }
}
```

### HashedWheelTimer#newTimeout

```markdown
# 调度TimerTask在指定的延迟之后一次性执行
* 启动时间轮的工作线程，设置deadline，新建HashedWheelTimeout实例，加入到timeouts队列，由Worker线程处理。
- 1. 


```

```java
@Override
public Timeout newTimeout(TimerTask task, long delay, TimeUnit unit) {
    if (task == null) {
        throw new NullPointerException("task");
    }
    if (unit == null) {
        throw new NullPointerException("unit");
    }

    long pendingTimeoutsCount = pendingTimeouts.incrementAndGet();

    // maxPendingTimeouts = -1，
    if (maxPendingTimeouts > 0 && pendingTimeoutsCount > maxPendingTimeouts) {
        pendingTimeouts.decrementAndGet();
        throw new RejectedExecutionException("Number of pending timeouts (" + pendingTimeoutsCount
                + ") is greater than or equal to maximum allowed pending "
                + "timeouts (" + maxPendingTimeouts + ")");
    }

    // 如果时间轮没有启动，则启动，并初始化startTime为当前纳秒
    // 启动ThreadWorker线程，并使用CountDownLatch闩锁，等待线程由: Runnable -> Running
    start();

    // Add the timeout to the timeout queue which will be processed on the next tick.
    // During processing all the queued HashedWheelTimeouts will be added to the correct HashedWheelBucket.
    long deadline = System.nanoTime() + unit.toNanos(delay) - startTime;

    // Guard against overflow.
    // 在delay 为正数的情况下，deadline 不可能为负数，
    // 若为deadline为负数，那么一定是超出了Long类型的最大值
    if (delay > 0 && deadline < 0) {
        deadline = Long.MAX_VALUE;
    }

    // 将任务添加到并发容器队列，等待下一次tick(Long tick++, wheel & tick)，将从队列中获取数据放入到时间轮数组中
    // 每次最多取出 100000个任务 - 由 transferTimeoutsToBuckets() 实现/* for (int i = 0; i < 100000; i++) {
    //                HashedWheelTimeout timeout = timeouts.poll();*/
    // ThreadWorker 在run 中处理timeouts的数据（transferTimeoutsToBuckets）
    HashedWheelTimer.HashedWheelTimeout timeout = new HashedWheelTimer.HashedWheelTimeout(this, task, deadline);
    // HashedWheelBucket 链表
    timeouts.add(timeout);
    return timeout;
}

```

### HashedWheelTimer#start

```markdown
# HashedWheelTimer 启动WorkerThread线程
- 1. 通过AtomicIntegerFiledUpdater，获取HashedWhellTimer中工作线程的状态，然后使用CAS设置状态，若CAS=true，则启动线程；
- 2. 采用AQS-CountDownLatch，在调用start之后，使用await等待，由run中的countdown()放行；确保线程进入运行状态，返回启动结束。
```

```java
public void start() {
    //AtomicIntegerFiledUpdate:this:workerState 获取WorkedThread的状态值
    switch (workerStateUpdater.get(this)) {
        // 若 WorkerThread的状态是初始化，则通过CAS设置为启动状态，CAS=true 则表示当前线程获取到排它锁，启动线程
        case WORKER_STATE_INIT:
            if (workerStateUpdater.compareAndSet(this, WORKER_STATE_INIT, WORKER_STATE_STARTED)) {
                workerThread.start();
            }
            break;
        case WORKER_STATE_STARTED:
            break;
        case WORKER_STATE_SHUTDOWN:
            throw new IllegalStateException("cannot be started once stopped");
        default:
            throw new Error("Invalid WorkerState");
    }

    // Wait until the startTime is initialized by the worker.
    while (startTime == 0) {
        try {
            // ThreadWorker调用start之后，使用await等待run方法中的countDown（ThreadWorker已经处于Running状态）
            startTimeInitialized.await();
        } catch (InterruptedException ignore) {
            // Ignore - it will be ready very soon.
        }
    }
}
```

### HashedWheelTimer.Worker#run

```markdown
# WorkerThread线程，start->run();
```

```java
@Override
public void run() {
    // Initialize the startTime.
    startTime = System.nanoTime();
    if (startTime == 0) {
        // We use 0 as an indicator for the uninitialized value here, so make sure it's not 0 when initialized.
        startTime = 1;
    }

    // Notify the other threads waiting for the initialization at start().
    // 在start()方法中，由线程启动之后，进入await等待，使用countDown将闩锁的计数器减一（初始化为1，因此将执行await之后内容）
    startTimeInitialized.countDown();

    do {
        // 休眠到下一个tick代表的时间到来（+1ms），deadline = System.nanoTime() - startTime，
        final long deadline = waitForNextTick();
        if (deadline > 0) {
            // 使用或运算算出slot，mask = 2^N - 1 = 01...111(二进制) & tick
            int idx = (int) (tick & mask);
            // 将cancelledTimeouts队列中的任务取出来，并将当前的任务从时间轮中移除
            processCancelledTasks();
            HashedWheelTimer.HashedWheelBucket bucket = wheel[idx];
            // 将timeouts队列中缓存（newTimeout()方法中加入到待处理定时任务队列）的数据取出加入到时间轮里面
            transferTimeoutsToBuckets();
            bucket.expireTimeouts(deadline);
            tick++;
        }
        // 若workerState 一直为 started 状态，就一直循环处理数据
    } while (workerStateUpdater.get(HashedWheelTimer.this) == WORKER_STATE_STARTED);

    // Fill the unprocessedTimeouts so we can return them from stop() method.
    // 填充未处理的超时
    for (HashedWheelTimer.HashedWheelBucket bucket : wheel) {
        bucket.clearTimeouts(unprocessedTimeouts);
    }
    for (;;) {
        HashedWheelTimer.HashedWheelTimeout timeout = timeouts.poll();
        if (timeout == null) {
            break;
        }
        // 若存在未处理的timeout，则将它加入到unprocessedTimeouts队列中
        if (!timeout.isCancelled()) {
            unprocessedTimeouts.add(timeout);
        }
    }
    // 处理被取消的任务
    processCancelledTasks();
}
```



## JRaft 主节点选举[Leader Election]



## JRaft 日志复制[Log Replication]

```markdown
JRaftProtocol#submit(WriteRequest request) 
  JRaftProtocol#submitAsync(WriteRequest request)
    JRaftServer#commit(String group, Message data, CompletableFuture<Response> future)
      JRaftServer#applyOperation(Node node, Message data, FailoverClosure closure)
        NodeImpl#apply(Task task) /* Write 请求处理开始 */
# ---
https://www.colabug.com/2020/0901/7664952/
```

> ![Raft日志复制](/Users/galminzhou/Documents/MarkDown/images/JRaft- jraft日志复制状态机.png)
>
> ```js
> //下一个要发送的LogIndexId，Leader上任初始化为lastLogIndex + 1
> private volatile long                    nextIndex;
> //每次日志复制都把多个LogEntity封装进Inflight，一次发送
> private Inflight                         rpcInFly;  //这里记录最近要的一个
> private final ArrayDeque<Inflight>       inflights              = new ArrayDeque<>();
> //Raft不允许乱序日志复制，所以需要这两个字段限制某个inflight是否对应某个request和response
> private int                              reqSeq                 = 0;
> private int                              requiredNextSeq        = 0;    //限制顺序
> ```

```markdown
* 1. 获取当前状态机判断是否是leader，如果不是则返回转发请求响应，如果本地已经获取到了leader Node属性，则返回响应中附带leader信息
* 2. 创建Value响应对象ValueResponse
* 3. 创建递增闭包IncrementAndAddClosure
* 4. 创建Task任务，设置Done动作为递增闭包Closure，序列化request请求绑定至data
counterServer获取当前raft节点node应用apply任务
当前节点实现NodeImpl执行apply任务
创建日志LogEntry
创建泛型为LogEntryAndClosure的EventTranslator事件转换器
死循环使用RingBuffer尝试发布事件，重试次数为3
发布事件至Disruptor，事件处理句柄会实时消费事件处理事件
事件消费LogEntryAndClosureHandler，如果事件数量大于等于执行batch配置applyBatch（默认32）或者endOfBatch为true，则执行任务
批量执行任务executeApplyingTasks
如果当前节点不是leader，根据状态设置异常信息，如果是_STATE_TRANSFERRING状态则说明当前节点处于忙状态，状态设置为EBUSY_，将任务批量提交至线程池_CLOSURE_EXECUTOR_执行，线程池默认core为cpu数，max为CPU数*100，keepAlive超时为1分钟，队列为SynchronousQueue同步队列
如果当前expectedTerm与当前currTerm不一致，批量提交任务执行，状态为_EPERM_
将任务追加至投票箱ballotBox中的闭包队列closureQueue.appendPendingClosure,ClosureQueueImpl实现的队列实际类型是一个链表LinkedList
日志管理器logManager批量添加条目appendEntries，入参LeaderStableClosure
将LeaderStableClosure闭包封装为StableClosureEvent事件发布至磁盘RingBuffer diskQueue
稳定的闭包事件处理句柄StableClosureEventHandler消费事件
AppendBatcher批量追加写入日志，AppenderBatcher刷新flush
调用RocksDBLogStorage日志存储批量写入日志appendEntries（entries为LogEntry包装的Task.getData，当前样例中也就是客户端请求中的delta对应的是i）
遍历LeaderStableClosure闭包执行，状态为OK 
-


```

### JRaftServer#commit

```java
/**com.alibaba.nacos.core.distributed.raft.JRaftServer#commit*/
/** 
 * 由Raft Group中的Leader操作Write请求，非Leader则将请求转发至Leader。
 */
public CompletableFuture<Response> commit(final String group, final Message data,
                                          final CompletableFuture<Response> future) {
    LoggerUtils.printIfDebugEnabled(Loggers.RAFT, "data requested this time : {}", data);
    // 若在分组（naming_persistent_service）中 Store 不可用，则Store中的Region状态不健康，不可能存在Leader，则请求结束
    final JRaftServer.RaftGroupTuple tuple = findTupleByGroup(group);
    if (tuple == null) {
        future.completeExceptionally(
            new IllegalArgumentException("No corresponding Raft Group found : " + group));
        return future;
    }

    // 快速处理失败回调机制
    FailoverClosureImpl closure = new FailoverClosureImpl(future);

    final Node node = tuple.node;
    // 验证当前节点是否是Leader，若是Leader则开始处理Write请求，
    // 若不是则转发请求响应，若本地已获取Leader Node属性，则返回请求响应中附带Leader信息，然后由Leader开始处理Write请求
    if (node.isLeader()) {
        // The leader node directly applies this request
        applyOperation(node, data, closure);
    } else {
        // Forward to Leader for request processing
        invokeToLeader(group, data, rpcRequestTimeoutMs, closure);
    }
    return future;
}

/**com.alibaba.nacos.core.distributed.raft.JRaftServer#applyOperation*/
/** 
 * 构建Task，设置数据（Protobuf type: WriteRequest）和 设置done动作的闭包函数（Closure 接口）
 */
public void applyOperation(Node node, Message data, FailoverClosure closure) {
    // 构建一个JSaft的基本消息结构（
    // data: 任务数据
    // done: 任务关闭，当数据成功提交到raft group时，开发人员可通过调用done获取状态机处理的响应状态
    // expectedTerm: 若expectedTerm与此节点的当前term不匹配(如果值不是-1，默认为-1)，则拒绝此任务。）
    final Task task = new Task();
    task.setDone(new NacosClosure(data, status -> {
        NacosClosure.NacosStatus nacosStatus = (NacosClosure.NacosStatus) status;
        closure.setThrowable(nacosStatus.getThrowable());
        closure.setResponse(nacosStatus.getResponse());
        closure.run(nacosStatus);
    }));
    task.setData(ByteBuffer.wrap(data.toByteArray()));
    node.apply(task);
}
```

### NodeImpl#apply

```java
/**com.alipay.sofa.jraft.core.NodeImpl#apply*/
/**
 * 1. 创建LogEntry封装Task数据，并将LogEntry以事件的形式投递给Disruptor队列，
 * 2. 尝试发布事件至RingBuffer，若失败（RingBuffer空间不足），则重新发布，最多重试三次。
 */
@Override
public void apply(final Task task) {
    // 当前节点被关闭
    if (this.shutdownLatch != null) {
        Utils.runClosureInThread(task.getDone(), new Status(RaftError.ENODESHUTDOWN, "Node is shutting down."));
        throw new IllegalStateException("Node is shutting down");
    }
    Requires.requireNonNull(task, "Null task");

    // 创建一个LogEntry，用于封装 Task中的数据
    final LogEntry entry = new LogEntry();
    entry.setData(task.getData());
    int retryTimes = 0;
    try {
        // 将Task以及对应的LogEntry对象，以事件的形式投递给 Disruptor 队列
        final EventTranslator<LogEntryAndClosure> translator = (event, sequence) -> {
            event.reset();
            event.done = task.getDone();
            event.entry = entry;
            event.expectedTerm = task.getExpectedTerm();
        };
        while (true) {
            // 尝试将事件发布到循环缓冲区（RingBuffer），若指定容量不可用则返回false。
            // JRaft在处理请求也是采用了完全异步，apply直接把任务丢到applyQueue
            if (this.applyQueue.tryPublishEvent(translator)) {
                break; //在内部类LogEntryAndClosureHandler处理任务
            } else {
                // 循环使用RingBuffer发布事件，最多重试三次
                retryTimes++;
                if (retryTimes > MAX_APPLY_RETRY_TIMES) {
                    Utils.runClosureInThread(task.getDone(),
                        new Status(RaftError.EBUSY, "Node is busy, has too many tasks."));
                    LOG.warn("Node {} applyQueue is overload.", getNodeId());
                    this.metrics.recordTimes("apply-task-overload-times", 1);
                    return;
                }
                ThreadHelper.onSpinWait();
            }
        }

    } catch (final Exception e) {
        LOG.error("Fail to apply task.", e);
        Utils.runClosureInThread(task.getDone(), new Status(RaftError.EPERM, "Node is down."));
    }
}
```

### NodeImpl#executeApplyingTasks

```markdown
上述是将承载简单指令Task封装成LogEntry对象以事件的形式投递给Disruptor队列进行异步处理，当事件在RingBuffer可用时，
Disruptor 队列设置了一个回调的接口函数（EventHandler<T>接口 + onEvent(T event, long sequence, boolean endOfBatch) 方法），用于消费Disruptor队列中的事件。

在LogEntryAndClosureHandler implements EventHandler<LogEntryAndClosure> 类中，onEvent方法将调用executeApplyingTasks
```

```java
/**
 * 1. 若当前节点不是Leader，则直接标记Task失败，结束；否则由Leader节点处理Tasks
 * 2. 验证数据Task的term（Leader的选举任期）是否与 currTerm（当前的期限）一致（默认值-1，或者其它值）
 * 3. 为Task创建并初始化对应的投票机制，用于决策LogEntry是否允许commit
 * 
 */
private void executeApplyingTasks(final List<NodeImpl.LogEntryAndClosure> tasks) {
    this.writeLock.lock();
    try {
        final int size = tasks.size();
        // 若当前是节点不是Leader，则直接标记Task失败（权限不够or服务器忙碌状态）
        if (this.state != State.STATE_LEADER) {
            final Status st = new Status();
            if (this.state != State.STATE_TRANSFERRING) {
                st.setError(RaftError.EPERM, "Is not leader.");
            } else {
                st.setError(RaftError.EBUSY, "Is transferring leadership.");
            }
            LOG.debug("Node {} can't apply, status={}.", getNodeId(), st);
            final List<NodeImpl.LogEntryAndClosure> savedTasks = new ArrayList<>(tasks);
            Utils.runInThread(() -> {
                for (int i = 0; i < size; i++) {
                    savedTasks.get(i).done.run(st);
                }
            });
            return;
        }
      
        final List<LogEntry> entries = new ArrayList<>(size);
        // 循环处理tasks集合
        for (int i = 0; i < size; i++) {
            final NodeImpl.LogEntryAndClosure task = tasks.get(i);
            // 验证当前节点的Task term值是否与期望term是否一致（一般情况下为默认值 -1，开发的业务代码块获取不到currTerm）
            if (task.expectedTerm != -1 && task.expectedTerm != this.currTerm) {
                LOG.debug("Node {} can't apply task whose expectedTerm={} doesn't match currTerm={}.",
                          getNodeId(),
                          task.expectedTerm, this.currTerm);
                if (task.done != null) {
                    final Status st = new Status(RaftError.EPERM, 
                                                 "expected_term=%d doesn't match current_term=%d",
                                                 task.expectedTerm, this.currTerm);
                    Utils.runClosureInThread(task.done, st);
                }
                continue;
            }
            // Leader 节点调用appendPendingTask，为Task创建并初始化投票机制，决策LogEntry 是否允许commit
            if (!this.ballotBox.appendPendingTask(this.conf.getConf(),
                this.conf.isStable() ? null : this.conf.getOldConf(), task.done)) {
                Utils.runClosureInThread(task.done, new Status(RaftError.EINTERNAL, "Fail to append task."));
                continue;
            }
            // set task entry info before adding to list.
            task.entry.getId().setTerm(this.currTerm);
            task.entry.setType(EnumOutter.EntryType.ENTRY_TYPE_DATA);
            entries.add(task.entry);
        }
        // 将LogEntry数据追加到本地文件系统，之后 callbacks LeaderStableClosure，并且投票数+1
        this.logManager.appendEntries(entries, new NodeImpl.LeaderStableClosure(entries));
        // update conf.first
        checkAndSetConfiguration(true);
    } finally {
        this.writeLock.unlock();
    }
}
```

### BallotBox#appendPendingTask

```markdown


```

```java
/*com.alipay.sofa.jraft.core.BallotBox#appendPendingTask*/
public boolean appendPendingTask(final Configuration conf, final Configuration oldConf, final Closure done) {
    // 创建并初始化选票
    final Ballot bl = new Ballot();
    if (!bl.init(conf, oldConf)) {
        LOG.error("Fail to init ballot.");
        return false;
    }
    final long stamp = this.stampedLock.writeLock();
    try {
        // 当一个候选节点（condidate）成为Leader节点之后，必须调用resetPendingIndex方法，重置 pendingIndex 值。
        // 
        if (this.pendingIndex <= 0) {
            LOG.error("Fail to appendingTask, pendingIndex={}.", this.pendingIndex);
            return false;
        }
        // 记录投票，用于确认是否赢得过半的票数
        this.pendingMetaQueue.add(bl);
        // 
        this.closureQueue.appendPendingClosure(done);
        return true;
    } finally {
        this.stampedLock.unlockWrite(stamp);
    }
}
```

### LogManagerImpl#appendEntries

```markdown
将日志数据写入到本地存储系统的过程，由LogManagerImpl#appendEntries实现，该方法接收一个LeaderStableClosure类型回调，当数据处理完成之后触发执行回调。

# 
private Disruptor<StableClosureEvent>                    disruptor;
private RingBuffer<StableClosureEvent>                   diskQueue;
```

```java
/*com.alipay.sofa.jraft.storage.impl.LogManagerImpl#appendEntries*/
@Override
public void appendEntries(final List<LogEntry> entries, final LogManager.StableClosure done) {
    Requires.requireNonNull(done, "done");
    // 运行期间错误
    if (this.hasError) {
        entries.clear();
        Utils.runClosureInThread(done, new Status(RaftError.EIO, "Corrupted LogStorage"));
        return;
    }
    boolean doUnlock = true;
    this.writeLock.lock();
    try {
        // 若当前节点是Leader: 基于本地LastIndex 值设置List<LogEntry>中LogEntry的logIndex
        // (Leader节点中不存在数据冲突问题，因为Leader节点的数据总是最新的也是其他Follower节点的标杆)
        // 若当前节点是Follower: 检查待复制的日志与本地已有的日志是否存在冲突，若存在冲突则强行覆盖本地日志
        if (!entries.isEmpty() && !checkAndResolveConflict(entries, done)) {
            // If checkAndResolveConflict returns false, the done will be called in it.
            entries.clear();
            return;
        }
        for (int i = 0; i < entries.size(); i++) {
            final LogEntry entry = entries.get(i);
            // Set checksum after checkAndResolveConflict
            if (this.raftOptions.isEnableLogEntryChecksum()) {
                entry.setChecksum(entry.checksum());
            }
            // 若数据类型为：ENTRY_TYPE_CONFIGURATION，则记录集群配置信息
            if (entry.getType() == EnumOutter.EntryType.ENTRY_TYPE_CONFIGURATION) {
                Configuration oldConf = new Configuration();
                if (entry.getOldPeers() != null) {
                    oldConf = new Configuration(entry.getOldPeers(), entry.getOldLearners());
                }
                final ConfigurationEntry conf = new ConfigurationEntry(entry.getId(),
                    new Configuration(entry.getPeers(), entry.getLearners()), oldConf);
                this.configManager.add(conf);
            }
        }
        // 更新内存数据
        if (!entries.isEmpty()) {
            done.setFirstLogIndex(entries.get(0).getId().getIndex());
            this.logsInMemory.addAll(entries);
        }
        done.setEntries(entries);
				
      	// 将修正之后的LogEntry数据，封装为事件投递给Disruptor队列，事件类型是Other
        int retryTimes = 0;
        final EventTranslator<LogManagerImpl.StableClosureEvent> translator = (event, sequence) -> {
            event.reset();
            event.type = LogManagerImpl.EventType.OTHER;
            event.done = done;
        };
        while (true) {
            // 推送事件至Ring Buffer中，若缓冲区（RingBuffer）没有足够空间则失败，最大重试次数50次
            if (tryOfferEvent(done, translator)) {
                break;
            } else {
                retryTimes++;
                if (retryTimes > APPEND_LOG_RETRY_TIMES) {
                    reportError(RaftError.EBUSY.getNumber(), "LogManager is busy, disk queue overload.");
                    return;
                }
                // 将Thead Runing状态 -> Runnable（就绪状态），让出 CPU，在下一个线程执行的时候再次参与竞争CPU。
                // 向调度程序提示当前线程愿意让出其当前使用的处理器，以改善线程之间的相对进展，否则会过度利用CPU。
                ThreadHelper.onSpinWait();
            }
        }
        doUnlock = false;
        // 
        if (!wakeupAllWaiter(this.writeLock)) {
            notifyLastLogIndexListeners();
        }
    } finally {
        if (doUnlock) {
            this.writeLock.unlock();
        }
    }
}
```

### LogManagerImpl#checkAndResolveConflict

```markdown
# 日志冲突解决



```

```java
/*com.alipay.sofa.jraft.storage.impl.LogManagerImpl#checkAndResolveConflict*/


```

### LogManagerImpl.AppendBatcher#flush

```markdown
Disruptor 队列设置了一个回调的接口函数（EventHandler<T>接口 + onEvent(T event, long sequence, boolean endOfBatch) 方法），用于消费Disruptor队列中的事件。

在上述的appendEntries中，以事件的方式添加了一个StableClosureEvent，事件类型为EventType.OTHER；
class StableClosureEventHandler implements EventHandler<StableClosureEvent> 消费队列事件实现类；
在实现类中定义了一个 AppendBatcher 类型的字段，用于缓存待写入的数据，调用flush 用于执行将缓存的数据写入存储系统。
```

```java
/*com.alipay.sofa.jraft.storage.impl.LogManagerImpl.AppendBatcher#flush*/
LogId flush() {
    if (this.size > 0) {
        // 将数据落盘并返回最新的LogId
        this.lastId = appendToStorage(this.toAppend);
        for (int i = 0; i < this.size; i++) {
            // 清空缓存的LogEntry数据
            this.storage.get(i).getEntries().clear();
            Status st = null;
            try {
                // LogManager IO Exception（存储日志损坏）
                if (LogManagerImpl.this.hasError) {
                    st = new Status(RaftError.EIO, "Corrupted LogStorage");
                } else {
                    st = Status.OK();
                }
                // 应用回调（Closure接口 - LeaderStableClosure实现类）函数
                this.storage.get(i).run(st);
            } catch (Throwable t) {
                LOG.error("Fail to run closure with status: {}.", st, t);
            }
        }
        this.toAppend.clear();
        this.storage.clear();

    }
    this.size = 0;
    this.bufferSize = 0;
    return this.lastId;
}
```

### LogManagerImpl#appendToStorage

```markdown
将数据写入存储系统的逻辑，默认也就是写入 RocksDB 存储引擎

```

```java
/*com.alipay.sofa.jraft.storage.impl.LogManagerImpl#appendToStorage*/
private LogId appendToStorage(final List<LogEntry> toAppend) {
    LogId lastId = null;
    if (!this.hasError) {
        final long startMs = Utils.monotonicMs();
        final int entriesCount = toAppend.size();
        this.nodeMetrics.recordSize("append-logs-count", entriesCount);
        try {
            int writtenSize = 0;
            for (int i = 0; i < entriesCount; i++) {
                final LogEntry entry = toAppend.get(i);
                writtenSize += entry.getData() != null ? entry.getData().remaining() : 0;
            }
            this.nodeMetrics.recordSize("append-logs-bytes", writtenSize);
            final int nAppent = this.logStorage.appendEntries(toAppend);
            if (nAppent != entriesCount) {
                LOG.error("**Critical error**, fail to appendEntries, nAppent={}, toAppend={}", 
                          nAppent, toAppend.size());
                reportError(RaftError.EIO.getNumber(), "Fail to append log entries");
            }
            if (nAppent > 0) {
                lastId = toAppend.get(nAppent - 1).getId();
            }
            toAppend.clear();
        } finally {
            this.nodeMetrics.recordLatency("append-logs", Utils.monotonicMs() - startMs);
        }
    }
    return lastId;
}
```

### BallotBox#commitAt

```markdown
Leader数据刷盘之后，调用回调函数（Closure接口 - LeaderStableClosure实现类），
如果响应状态是OK，则在回调用中执行BallotBox#commitAt方法，检查该批次的日志数据是否过半节点复制成功，
若存在复制成功的日志数据，则递增 lastCommittedIndex的值，并向状态机发布 COMMITTED 事件。

----
Leader节点在将数据写入内存之后，即通知对应的复制器 Replicator 开始往目标 Follower 节点复制数据（Replicator 优先从内存中读取待复制的日志数据），
在日志数据在 Leader 节点被落盘之后回调执行BallotBox#commitAt之前，有可能日志数据已经成功的被过半的节点数复制成功。
```

```java
/**com.alipay.sofa.jraft.core.NodeImpl.LeaderStableClosure#run*/
@Override
public void run(final Status status) {
  if (status.isOk()) {
    NodeImpl.this.ballotBox.commitAt(this.firstLogIndex, this.firstLogIndex + this.nEntries - 1,
                                     NodeImpl.this.serverId);
  } else {
    LOG.error("Node {} append [{}, {}] failed, status={}.", getNodeId(), this.firstLogIndex,
              this.firstLogIndex + this.nEntries - 1, status);
  }
}
/**com.alipay.sofa.jraft.core.BallotBox#commitAt*/
public boolean commitAt(final long firstLogIndex, final long lastLogIndex, final PeerId peer) {
    // TODO  use lock-free algorithm here?
    final long stamp = this.stampedLock.writeLock();
    long lastCommittedIndex = 0;
    try {
        if (this.pendingIndex == 0) {
            return false;
        }
        if (lastLogIndex < this.pendingIndex) {
            return true;
        }

        if (lastLogIndex >= this.pendingIndex + this.pendingMetaQueue.size()) {
            throw new ArrayIndexOutOfBoundsException();
        }

        final long startAt = Math.max(this.pendingIndex, firstLogIndex);
        Ballot.PosHint hint = new Ballot.PosHint();
        // 遍历检查当前批次中的LogEntry是否已经成功被过半的节点数复制
        for (long logIndex = startAt; logIndex <= lastLogIndex; logIndex++) {
            final Ballot bl = this.pendingMetaQueue.get((int) (logIndex - this.pendingIndex));
            hint = bl.grant(peer, hint);
            // 若已经被过半的节点数成功复制，记录最新的lastCommittedIndex
            if (bl.isGranted()) {
                lastCommittedIndex = logIndex;
            }
        }
        // 若没有LogEntry被过半节点数成功复制，则先返回，之后等待Follower的granted检查。
        // （此时数据可能未成功被过半的节点数复制，
        // 将会由Follower节点完成日志数据复制的 AppendEntries 请求响应处理期间被调用，此时也会触发检查 granted 操作。）
        if (lastCommittedIndex == 0) {
            return true;
        }
        // When removing a peer off the raft group which contains even number of
        // peers, the quorum would decrease by 1, e.g. 3 of 4 changes to 2 of 3. In
        // this case, the log after removal may be committed before some previous
        // logs, since we use the new configuration to deal the quorum of the
        // removal request, we think it's safe to commit all the uncommitted
        // previous logs, which is not well proved right now
        // 剔除已经被过半数节点复制的 LogIndex 对应的选票，
        // Raft 保证一个 LogEntry 被提交之后，在此之前的 LogEntry 一定是 committed 状态。
        this.pendingMetaQueue.removeFromFirst((int) (lastCommittedIndex - this.pendingIndex) + 1);
        LOG.debug("Committed log fromIndex={}, toIndex={}.", this.pendingIndex, lastCommittedIndex);
        // 更新集群的 lastCommittedIndex 值
        this.pendingIndex = lastCommittedIndex + 1;
        this.lastCommittedIndex = lastCommittedIndex;
    } finally {
        this.stampedLock.unlockWrite(stamp);
    }
    // 向状态机发布 COMMITED 事件
    this.waiter.onCommitted(lastCommittedIndex);
    return true;
}
```

### FSMCallerImpl#doCommitted

```markdown
com.alipay.sofa.jraft.core.FSMCallerImpl#onCommitted
  com.alipay.sofa.jraft.core.FSMCallerImpl#enqueueTask
  	//封装ApplyTask发布事件至RingBuffer，Disruptor队列消费事件（ApplyTaskHandler implements EventHandler<ApplyTask>）
    com.alipay.sofa.jraft.core.FSMCallerImpl.ApplyTaskHandler#onEvent
      com.alipay.sofa.jraft.core.FSMCallerImpl#runApplyTask
        com.alipay.sofa.jraft.core.FSMCallerImpl#doCommitted
# -----
* 业务向 JRaft 集群提交的 Task 在被转换成日志并成功复制给集群中的过半数以上节点（即对应的日志被提交）之后，接下去就需要将这些日志中存储的指令透传给业务状态机，通过doCommitted实现。

* FSMCaller 本地维护了一个 lastAppliedIndex 字段，用于记录已经被应用（即已将日志中的指令透传给业务状态机）的 LogEntry 对应的 logIndex 值。因为 Raft 协议能够保证某个 committedIndex 之前的所有 LogEntry 都是被提交的，所以即使 committedIndex 的到达顺序出现乱序也不会影响正常的运行逻辑。
```

```java
/**com.alipay.sofa.jraft.core.FSMCallerImpl#doCommitted*/
private void doCommitted(final long committedIndex) {
    // 状态调度机运行异常
    if (!this.error.getStatus().isOk()) {
        return;
    }
    // 获取最新被状态机应用的LogEntry对应的LogIndex值
    final long lastAppliedIndex = this.lastAppliedIndex.get();
    // We can tolerate the disorder of committed_index
    // 当前状态机最新的LogIndex对应的LogEntry已经被处理过，无需重复处理
    if (lastAppliedIndex >= committedIndex) {
        return;
    }
    final long startMs = Utils.monotonicMs();
    try {
        final List<Closure> closures = new ArrayList<>();
        final List<TaskClosure> taskClosures = new ArrayList<>();
        // 获取commitedIndex之前的Task回调列表，填充到 closures集合中
        // 如果是TaskClosure类型的Task回调，则再记录到taskClosures集合中，主要用于回调 TaskClosure#onCommitted方法
        final long firstClosureIndex = this.closureQueue.popClosureUntil(committedIndex, closures, taskClosures);
				// 对于TaskClosure 类型的 Task 回调，应用TaskClosure#onCommitted方法
        // Calls TaskClosure#onCommitted if necessary
        onTaskCommitted(taskClosures);

        Requires.requireTrue(firstClosureIndex >= 0, "Invalid firstClosureIndex");
        // 创建LogEntry迭代器，用于迭代LogEntry
        final IteratorImpl iterImpl = new IteratorImpl(this.fsm, this.logManager, closures, firstClosureIndex,
            lastAppliedIndex, committedIndex, this.applyingIndex);
        // true：当前的日志不是最新的，并且节点状态不存在允许异常，则说明还存在可继续处理的日志数据
        while (iterImpl.isGood()) {
            final LogEntry logEntry = iterImpl.entry();
            if (logEntry.getType() != EnumOutter.EntryType.ENTRY_TYPE_DATA) {
                // 系统内部的日志数据消息
                if (logEntry.getType() == EnumOutter.EntryType.ENTRY_TYPE_CONFIGURATION) {
                    if (logEntry.getOldPeers() != null && !logEntry.getOldPeers().isEmpty()) {
                        // Joint stage is not supposed to be noticeable by end users.
                        this.fsm.onConfigurationCommitted(new Configuration(iterImpl.entry().getPeers()));
                    }
                }
                // 
                if (iterImpl.done() != null) {
                    // For other entries, we have nothing to do besides flush the
                    // pending tasks and run this closure to notify the caller that the
                    // entries before this one were successfully committed and applied.
                    iterImpl.done().run(Status.OK());
                }
                iterImpl.next();
                continue;
            }

            // Apply data task to user state machine
            // 连续处理当前批次的业务操作产生的日志数据，应用 NacosStateMachine#onApply 方法
            doApplyTasks(iterImpl);
        }

        // 状态调度机运行异常，将错误信息透传到业务和当前节点
        if (iterImpl.hasError()) {
            setError(iterImpl.getError());
            iterImpl.runTheRestClosureWithError();
        }
        final long lastIndex = iterImpl.getIndex() - 1;
        final long lastTerm = this.logManager.getTerm(lastIndex);
        final LogId lastAppliedId = new LogId(lastIndex, lastTerm);
        // 更新最新应用的LogEntry的LogIndex和Term值
        this.lastAppliedIndex.set(lastIndex);
        this.lastAppliedTerm = lastTerm;
        // 通知LogManager，当前已经被应用的LogEntry可在内存中移除了
        this.logManager.setAppliedId(lastAppliedId);
        notifyLastAppliedIndexUpdated(lastIndex);
    } finally {
        this.nodeMetrics.recordLatency("fsm-commit", Utils.monotonicMs() - startMs);
    }
}
```

### 日志复制 - Pipeline 机制

```markdown
* 日志数据复制在 Raft 协议的运行过程中是一项频繁的操作，为了在保证日志复制顺序和连续的前提下尽量提升复制的性能，除了并发的向各个 Follower 或 Learner节点批量发送数据之外，JRaft 在实现上引入了 Pipeline机制。
* Pipeline 机制简而言之，就是将请求和响应从串行改成并行，请求和响应之间互不阻塞。
* Leader节点可以连续的向Follower节点发送请求；对于那些已经发送出去还未收到响应的请求，或者已经收到但是还没来得及处理的响应对应的请求将其标记为 inflight，并在成功处理完对应的响应之后去除请求的 inflight 标记。
* 如果期间发生宕(down)机，对于这些inflight请求会尝试重新发送，以此保证日志数据在复制期间不会漏传给 Follower 节点。
* 



```



# 高性能并发队列 - Disruptor

```markdown








```







# 二进制序列消息 - ProtoBuf

```markdown
# PortoBuf - Google Protocol Buffers
# /***优点***/
* Protobuf的空间效率是JSON的2-5倍，时间效率要高，对于数据大小敏感，传输效率高的模块可以采用protobuf库。
- 通过Protobuf序列化/反序列化的过程可以得出：
- 1. protobuf是通过算法生成二进制流，序列化与反序列化不需要解析相应的节点属性和多余的描述信息，所以序列化和反序列化时间效率较高。
- 2. protobuf是由字段索引(fieldIndex)与数据类型(type)计算(fieldIndex<<3|type)得出的key维护字段之间的映射且只占一个字节，所以相比json与xml文件，protobuf的序列化字节没有过多的key与描述符信息，所以占用空间要小很多。
# /***缺点***/
* 消息结构可读性不高，序列化后的字节序列为二进制序列不能简单的分析有效性；目前使用不广泛，只支持java,C++和Python。
```

