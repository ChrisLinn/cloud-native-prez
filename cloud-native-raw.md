# Cloud Native



+ https://jimmysong.io/posts/what-is-cloud-native-application-architecture/
+ https://zhuanlan.zhihu.com/p/28663432

# k8s 的方便强大

* Deployment
* Service Discovery
* DevOps: 开发测试环境一致、CI/CD，监控


# deployment 示例
* 滚动升级和回滚应用
    - `kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1`
* 扩容和缩容
    - `kubectl scale deployment nginx-deployment --replicas 10`
    - `kubectl autoscale deployment nginx-deployment --min=10 --max=15 --cpu-percent=80`

---

# Cloud Native 的定义
+ 在动态环境中构建和运行可弹性扩展的应用
+ 代表技术包括容器、服务网格、微服务、不可变基础设施和声明式 API
+ 能够构建容错性好、易于管理和便于观察的松耦合系统
+ 结合可靠的自动化手段，可以使开发者轻松地对系统进行频繁并可预测的重大变更


---
# bycoin 目前的架构
+ 镜像
+ 使用阿里云提供的 RDS、负载均衡

# 存在的问题
+ 不利于升级管理
+ 依赖性
+ 不利于迁移
    * 之前的 青云 -> 阿里
        - 流量


---
# 虚拟机
https://stackoverflow.com/questions/16047306/how-is-docker-different-from-a-virtual-machine

虚拟机中需要模拟一台物理机的所有资源，比如你要模拟出有多少CPU、网卡、显卡等等，这些都是在软件层面通过计算资源实现的，这就给物理机凭空增加了不必要的计算量。

# Kubernetes (k8s)

+ 让容器应用进入大规模工业生产
+ 在单机上运行容器，无法发挥它的最大效能，只有形成集群，才能最大程度发挥容器的良好隔离、资源分配与编排管理的优势

---
# k8s
+ 容器编排调度引擎/系统
+ 一个规范
    * 描述集群的架构
    * 定义服务的最终状态
        - 将系统自动地达到和维持在这个状态
+ 云原生应用的基石
    * 相当于一个云操作系统

---

组件之间交互的接口CNI、CRI、OCI等，将Kubernetes与某款具体产品解耦，给用户最大的定制程度，
---
# 核心组件
+ etcd 保存集群所有的网络配置和对象的状态信息
+ apiserver 提供了资源操作唯一入口，并提供认证、授权、访问控制、API注册和发现等机制
+ controller manager 维护集群的状态，比如故障检测、自动扩展、滚动更新等
+ scheduler负责资源的调度，按照预定的调度策略将Pod调度到相应的机器上
+ kubelet 维护容器的生命周期，同时也负责Volume（CVI）和网络（CNI）的管理
+ Container runtime 负责镜像管理以及Pod和容器的真正运行（CRI）
+ kube-proxy 为Service提供cluster内部的服务发现和负载均衡


---
# cloud native 的核心目标
+ 云已经可以为我们提供稳定可以唾手可得的基础设施，但是业务上云成了一个难题
+ Kubernetes的出现与其说是从最初的容器编排解决方案，倒不如说是为了解决应用上云这个难题
    * 不是为了部署和管理容器而用Kubernetes，承载其上的应用才是 [价值所在]
    * 基于k8s“操作系统”之上构建的适用于不同场景的应用将成为发展方向

---
# 云原生应用应该具备
+ 敏捷
+ 可靠
+ 高弹性
+ 易扩展
+ 故障隔离保护
+ 不中断业务持续更新

---
# 云原生所需要的能力和特征

![cloud-native-architecutre-mindnode](https://chrislinn.ink/img/cloud-native/cloud-native-architecutre-mindnode.jpg)


---
# 云原生的设计理念

+ 面向分布式：容器、微服务、API 驱动的开发
+ 面向配置：一个镜像，多个环境配置
+ 面向韧性：故障容忍和自愈
+ 面向弹性：弹性扩展和对环境变化（负载）做出响应
+ 面向交付：自动拉起，缩短交付时间
+ 面向性能：响应式，并发和资源高效利用
+ 面向自动化：自动化的 DevOps
+ 面向诊断性：集群级别的日志、metric 和追踪
+ 面向安全：安全端点、API Gateway、端到端加密


---
# 微服务架构
+ 开发和部署上的灵活性和技术多样性
+ 带来了服务调用的开销、分布式系统管理、调试与服务治理方面的难题

---
![microservice-concern](https://chrislinn.ink/img/cloud-native/microservice-concern.jpg)

---
# 微服务中最基础的服务注册发现
+ 客户端服务发现
    * Java应用中常用的方式是使用Eureka和Ribbon做服务注册发现和负载均衡
+ 服务端服务发现
    * Kubernetes中可以使用DNS、Service和Ingress来实现
        - 不需要修改应用代码，直接从网络层面来实现

---
![service-discovery-in-microservices](https://chrislinn.ink/img/cloud-native/service-discovery-in-microservices.png)

---
云端架构(分布式系统)，相对单体架构来说会带来很多挑战。一致性、延迟和网络分区、服务监控的变革、服务暴露、权限的管控等。

---

+ 版本化和分布式配置
+ 服务注册发现
+ 路由和负载均衡
+ 容错
    * 熔断器: 阻绝该服务与其依赖的远程调用
    * 隔板: 将服务分区，以便限制错误影响的区域
+ API 网关/边缘服务
    * 针对其开发微服务，尝试解决延迟、往返通信开销(减少服务端调用)、设备&协议多样性

---

# Service Mesh
在现今的复杂的大型网站情况下，单体应用被分解为众多的微服务，服务之间的依赖和通讯十分复杂。

可以将它比作是应用程序或者说微服务间的 TCP/IP，负责服务之间的网络调用、限流、熔断和监控。

---

# Service Mesh 可以用来做什么
+ Traffic Management：API网关
+ Observability：服务调用和性能分析
+ Policy Enforcement：控制服务访问策略
+ Service Identity and Security：安全保护

---
# Service Mesh 的特点
+ 专用的基础设施层
+ 轻量级高性能网络代理
+ 提供安全的、快速的、可靠地服务间通讯
+ 解耦应用程序的重试/超时、监控、追踪和服务发现
+ 扩展kubernetes的应用负载均衡机制，实现灰度发布
+ 完全解耦于应用，应用可以无感知，加速应用的微服务和云原生转型



<!-- 
Istio架构分为控制层和数据层:

+ 数据层：由一组智能代理（Envoy）作为sidecar部署，协调和控制所有microservices之间的网络通信。
+ 控制层：负责管理和配置代理路由流量，以及在运行时执行的政策。

https://jimmysong.io/posts/istio-overview/
https://jimmysong.io/posts/why-do-we-need-istio/
 -->


---

Kubernetes中的应用将作为微服务运行，但是Kuberentes本身并没有给出微服务治理的解决方案，比如服务的限流、熔断、良好的灰度发布支持等。

kube-proxy里基于iptables的原生负载均衡，并且服务间的流量也没有任何控制。

---
# 分布式追踪
将单体应用拆成多个微服务之后，监控服务之间的依赖关系和调用链，以判断应用在哪个服务环节出了问题，哪些地方可以优化

OpenTracing 是 CNCF 提出的分布式追踪的标准

---
# 云原生应用的架构
+ micro-service
    * 与 cloud native 自然贴合
+ serverless
    * 也算是 微服务的一种

---
# micro-service
## pros
+ 关注于一个业务功能，松耦合，灵活性更高
+ 每个服务按需定制扩展
+ 可由不同团队独立开发，互不影响
+ 独立测试、部署、升级、发布, 持续交付,允许在频繁发布不同服务的同时保持系统其他部分的可用性和稳定性
+ 提高容错性, 不会让整个系统瘫痪
+ 不被技术栈限制

---
# micro-service
## cons
+ 可能带来代码重复
+ 提高了系统的复杂度
+ 引入分布式系统的复杂性
+ 往往使用异步编程、消息与并行机制，如果应用存在跨微服务的事务性处理，其实现机制会变得复杂化
+ 服务的注册与发现问题
+ 运维开销及成本增加

---
# serverless
Serverless（无服务器架构）指的是由开发者实现的服务端逻辑运行在无状态的计算容器中，它由事件触发， 完全被第三方管理，其业务层面的状态则被开发者使用的数据库和存储资源所记录

---
Serverless 是构建在虚拟机和容器之上的一层，与应用本身的关系更加密切。

![google-cloud](https://chrislinn.ink/img/cloud-native/google-cloud.jpg)


---
# 分类
* BaaS（Backend as a Service）
    - 一个个的API调用后端或别人已经实现好的程序逻辑
    - 身份验证服务Auth0
    - RDS
* FaaS（Functions as a Service)
    - 传统的服务器端软件一般需要长时间驻留在操作系统的虚拟机或者容器中运行
    - FaaS是直接将程序部署上到平台上，当有事件到来时触发执行，执行完了就可以卸载掉。
        + 事件驱动的由消息触发的服务
        + 按需计算

---
## Pros
+ 降低成本
    * 版本管理服务器、持续集成服务器、测试服务器、应用版本管理仓库、数据库服务器等
    * 邮件服务、短信服务
    * 降低开发成本，只需在配置文件上写下数据库的表名，数据就会存储到对应的数据库里
+ 快速上线
    * 内建自动化部署, 只需要关注于如何更好去实现业务, 使用测试来保证健壮性，结合持续集成，可以在 PUSH 代码时直接部署到生产环境
+ 系统安全性
    * 无服务器，无登陆入口
+ 适应微服务架构
+ 自动扩展能力

---
## Cons
+ 不适合长时间运行应用
    * 应用不运行的时候进入 “休眠状态”
        - 冷启动时间
    * 长期租车的成本肯定比买车贵
+ 依赖第三方服务
    * 不利于迁移
    * 建立隔离层: API 网关, 数据库层...
        - 带来的问题会比解决的问题多
+ 缺乏调试和开发工具
    * 每次调试时需要一遍又一遍地上传代码
    * 分级别纪录日志: error、warn、info、verbose、debug、silly


<!-- 
7. docker对比传统虚拟机的图？为什么微服务架构比较适合docker？比虚拟机好再哪里？
8. 在介绍使用层上带来的好处的时候，需要说下内部是什么原因造成了这个使用便利
9. 想知道k8s, docker这些的大体技术架构，单纯好处，坏处，感觉有点浅
10. 想知道是什么原因造成了云生升级的优势，从技术层，而不是直接带来的好处
16. 专注于技术架构，放弃使用方面

13. service mesh解决了什么问题，文字化核心点一下？
15. 想知道service mesh和k8s是怎么结合的，替换了k8s上的哪些模块？
16. 介绍server mesh的时候概念多听的有些模糊了，可否假设一个真实的案例来说明？
18. 听完之后缺少一种感觉，对k8s， server mesh模糊，不确定为什么要使用这一套
19. 想到点：cloude native的讲说案例可否直接使用Bycoin的服务器来解决？
20. 将cloude native的好处的时候，画个图？类似于k8s和iota是怎么样的？服务是怎么一个装备 -
 -->