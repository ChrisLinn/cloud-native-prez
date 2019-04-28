+ https://jimmysong.io/posts/what-is-cloud-native-application-architecture/
+ https://zhuanlan.zhihu.com/p/28663432

+ 目标
    * 高并发
    * 高可用
    * 快速迭代
+ 单体架构
* 防单点 & 负载均衡
* 单体架构 存在问题
    * a 出问题，b 跟着遭殃
    - scale up
        + 单应用并发能力不成线性关系
        + 服务拆分
* 微服务
    - 提高容错性, 不会让整个系统瘫痪
    + 横向扩展更加灵活
    * 独立测试、部署、升级、发布, 持续交付,允许在频繁发布不同服务的同时保持系统其他部分的可用性和稳定性
+ 微服务 lib version
    * 虚拟机
+ 虚拟机存在问题
    - 启动慢, 万一哪个虚拟机挂了, 宕机时间
    - 吃资源
        + why
            + 软件层面, 计算资源模拟 hardware(CPU、网卡、显卡) & OS
        - when
            - 各个微服务，存储污染，想进行资源隔离，开多个虚拟机
            - 过剩计算资源部署多个服务
* 容器
    * 进程层面, 接触到的各种资源都是虚拟的隔离的
        - 轻量级, 易于配置, 易于使用
            + 11 mb
        - 构建、发布、运行更加敏捷和可控
* 想放上云
    - 弹性计算
        + 抢红包
        + 折旧率
    - 自己搞公网ip麻烦
    - b 站 app被逆向，app key 泄漏，内网api暴露在公网
+ 容器节省云厂商成本, 也便于隔离
* docker
    * 目前最流行的容器解决方案
        - namespaces 做权限的控制和隔离
            + ipc, posix
            + pid
            + network
                * device
                * protocol stack
                * firewall
                * routing
            * host, domain
            * user group
        - cgroups 进行资源的配置
            + mem
            + cpu
            + devices
            + hang process
            + bandwidth, flow_priv
        - aufs
            + 10GB*10 vs 11GB
    * 适合
        - 弹性云。可随开随关，动态扩容和缩容
        - 组建微服务架构
+ docker compose
+ 在单机上运行容器，无法发挥最大效能
+ 有形成集群，才能最大程度发挥容器的良好隔离、资源分配与管理的优势
+ 性能、稳定, 高并发高可用
+ docker swarm
+ k8s
+ 容器编排，应用上云，让容器应用进入大规模工业生产时代
+ k8s 好处
+ 微服务带来wen题
    * 可能带来代码重复
    * 提高了系统的复杂度
    - 一致性, 引入分布式系统的复杂性
    - 服务调用的开销, 网络延迟
    - 网络分区
    - 服务监控
    - 权限的管控
    - 服务的注册与发现问题
+ 微服务中最基础的服务注册发现 & 负载均衡
    * traditional
        - code/framework in app
    * k8s, kube-proxy
        - 不需要修改应用代码，直接从网络层面来实现
+ ctrl mng
    * 扩缩容
        + `kubectl scale deployment nginx-deployment --replicas 10`
        + `kubectl autoscale deployment nginx-deployment --min=10 --max=15 --cpu-percent=80`
    - 滚动升级和回滚应用
        + `kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1`
+ 可扩展、高弹性
+ 接口，解耦产品
+ 核心组件
    * etcd 保存集群所有的网络配置和对象的状态信息
    * apiserver 提供了资源操作唯一入口，并提供认证、授权、访问控制、API注册和发现等机制
    * controller manager 维护集群的状态，比如故障检测、自动扩展、滚动更新等
    * kube-proxy 为Service提供cluster内部的服务发现和负载均衡
+ sidecar
    * 返回币价，拉取币价
    * log saving
        - 面对大规模服务和节点时，进入服务器查看日志文件是不现实的
        - 超强故障恢复，重启意外停止的容器。因内存泄露导致容器在一天内宕机多次，然而自己都没有察觉到
            - 监控: 一旦问题出现，即使被及时处理了，还是想要知道
+ pod
    * 共享网络文件
    * 为什么不直接容器
        - 紧密耦合相互合作, 组合成一个微服务对外提供服务
        - 各自构建自己的容器镜像, 解耦软件依赖,独立版本管理、编译、发布、更新
+ k8s - bycoin
    * efk
+ k8s benefit
+ k8s 成本
    * etcd 
        - 重要角色, 集群可用性上限
            + 服务配置发现信息&配置信息
        - 3, 5, 7
    * docker 成本
        - 业务迁移和改造的成本
            + 对业务进行一定的改造和解耦
                * 无状态化
                    - 横向扩展, 水平伸缩
                    - 有状态也可能涉及锁，导致并发的串行化
            + 从物理机或虚拟机迁移到容器
                * 容器上线
                * 测试业务功能
                * 知会相关上下游
                * 割接流量
                * 最终下线业务并回收旧机器
        + 镜像的制作和管理成本
            * Dockerfile 的编写和维护
                - 标准化约定一定的规则
    - K8S 的学习使用成本
        + K8S 具有十几个 API，这些 API 的请求参数非常之多，用法较为复杂 
+ 传统成本
    + 非线性
    + 扩容维护成本
+ k8s problem
    * ![microservice-concern](https://chrislinn.ink/img/cloud-native/microservice-concern.jpg)
    - 负载均衡
        + 流量入口, 高性能、高可靠
            * dns
                - 多级解析, 延迟
                - 策略过于简单
            + 软件负载均衡支持到 5 万级并发已经很困难了
            * 硬件
                - 成本
                - 只能网络层，无法内存
    * auth
        - b 站 app被逆向，app key 泄漏，内网api
    * 在2014年春节的时候，微信红包，每分钟8亿多次的请求，其实真正到它后台的请求量，只有十万左右的数量级
        - 降级
        - 遥测, 流量监控
            + 多少请求
            + 多久回复, 超时时间
            + 多少错误
    * 速率限制
        - 消息中间件
            + 可以把两个模块之间的交互异步化，其次可以把不均匀请求流量输出为匀速的输出流量，所以说消息中间件 异步化 解耦 和流量削峰的利器。
    * 容错
        - 熔断器: 阻绝该服务与其依赖的远程调用
    * 分布式追踪
        - 将单体应用拆成多个微服务之后，监控服务之间的依赖关系和调用链，以判断应用在哪个服务环节出了问题，哪些地方可以优化
+ service mesh & 微服务治理
    * 从应用层下沉到基础设施层, 更注重业务逻辑和应用的性能本身, 将服务治理的能力交给平台来解决
    + 轻量级高性能网络代理, 专用的基础设施层, 完全解耦于应用，应用可以无感知
    + 负责服务之间的网络调用、限流、熔断和监控   限流、熔断、良好的灰度发布支持等。
    * Service Mesh 的基础是透明代理，通过 sidecar proxy 拦截到微服务间流量后再通过控制平面配置管理微服务的行为。
        - 应用程序间通讯的中间层
        - 轻量级网络代理
        - 应用程序无感知
        - 安全的、快速的、可靠地服务间通讯
        - 解耦应用程序的弹性控制(重试/超时)、流量管理、监控、追踪和服务发现
    * traffic management, api gateway
        - 将流量管理从 Kubernetes 中解耦
            + 通过为更接近微服务应用层的抽象，管理服务间的流量、安全性和可观察性。
        - vs kub-proxy
            + kub-proxy: random, 而非 根据负荷状态
            + 对每个服务做细粒度的控制
            + 扩展kubernetes的应用负载均衡机制，实现灰度发布
    * observability
        - 服务调用
        - 性能分析
        - 分布式追踪
    * policy enforcement
        - 控制服务访问策略
            + 多级调用
    * service identity & security
        - 安全保护
    * 标准化的日志
        - 服务化以后，每个服务可以用不同的开发语言+ istio
+ some
    * Service discovery
    * Advanced routing
        * Auto retries
        * Retry budgets
        * Request deadlines
        * Circuit breaking
    * Lantency observed load-balancing
    * Advanced orchestration
        * Canary, blue/green
        * Per request routing
    * Standardised metrics/logging
    * Distributed tracing
    * Rate limiting
    * Authentication
+ istio
    * Istio架构分为控制层和数据层:
        - 数据层：由一组智能代理（Envoy）作为sidecar部署，协调和控制所有microservices之间的网络通信。
        - 控制层：负责管理和配置代理路由流量，以及在运行时执行的政策。
    * components
        - mixer
            + 执行访问控制和使用策略
                * 如何允许被通信
            + 收集Envoy代理和其他服务的遥测数据
        - citadel
            + 服务间和最终用户认证
            + 升级未加密流量, 颁发证书
            + 为运营商提供基于服务身份而不是网络控制的策略的能力
        - galley
            + 收集和验证配置，并将其传播到各种Istio组件
            + 隔离 istio 组件 和用户配置细节
        - pilot
            + routing rule & traffic managment rule
                * high-level -> envoy specific config
                * 传播到 components
        - envoy
            + 动态服务发现
            + 负载平衡
            + TLS终止
            + HTTP/2＆gRPC代理
            + 断路器
            + 运行状况检查
+ 开发, 测试, 质量保证, 运维 vs DevOps  
    * 质量 安全 快速迭代 交付
+ serverless, FaaS (Functions as a Service)
    + 传统的服务器端软件一般需要长时间驻留在操作系统的虚拟机或者容器中运行
    + FaaS是直接将程序部署上到平台上, 服务端逻辑运行在无状态的计算容器中，当有事件到来时触发执行，执行完了就可以卸载掉。
        * 事件驱动
        * 消息触发
        * 按需计算
        * 完全被第三方管理
        * 业务层面的状态则被开发者使用的数据库和存储资源所记录
    + Pros
        - 降低成本
        - 快速上线
            + 内建自动化部署, 只需要关注于如何更好去实现业务
        - 系统安全性
            + 无服务器，无登陆入口
        - 适应微服务架构
        - 自动扩展能力
    + Cons
        - 不适合长时间运行应用
            + 应用不运行的时候进入 “休眠状态”
                * 冷启动时间
            + 长期租车的成本肯定比买车贵
        - 依赖第三方服务
            + 不利于迁移
        - 缺乏调试和开发工具
            + 每次调试时需要一遍又一遍地上传代码
+ cloud native mindnode

---
