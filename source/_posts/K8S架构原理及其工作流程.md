K8S架构原理及其工作流程

### 前言

![](https://s3.starssr.com/1720856097638.jpg)

一、容器编排系统

二、K8S整体架构图

1、K8S Master节点

2、K8S Node节点

三、POD创建过程

四、K8S各组件工作流程

总结

k8s的组件

k8s的工作流程

前言

### 一、容器编排系统

容器编排系统需要满足的条件：

服务注册，服务发现

负载均衡

配置、存储管理

健康检查

自动扩缩容

零宕机

### 二、K8S整体架构图

![](https://s3.starssr.com/image-v1qm.png)

K8S整体架构

Kubernetes采用主从分布式架构，包括Master（主节点）、Worker（从节点或工作节点），以及客户端命令行工具kubectl和其它附加项。

#### 1、K8S Master节点

![](https://s3.starssr.com/image-pufe.png)

etcd

保存了整个集群的状态，CoreOS提供(用户期望状态)。K/V存储，只能存储Api Server中支持的数据范式存储；（相当于分布式数据库，以键值对方式存储）

Api Server

提供了资源操作的唯一入口，并提供认证、授权、访问控制、API注册和发现等机制；

controller-manager

负责维护集群的状态，比如故障检测、自动扩展、滚动更新等（确保用户期望状态与实际运行状态一致）；

scheduler

负责资源的调度，按照预定的调度策略将Pod调度到相应的机器上，pod是调度的最小单位；

#### 2、K8S Node节点

![](https://s3.starssr.com/image-3gwu.png)

kubelet

会监控Api Server上的资源变动，若变动与自己有关系，kublet就去执行任务；定期向master会报节点资源使用情况。

kube-proxy

实现service的抽象，为一组pod抽象的服务提供统一接口并提供负载均衡。

docker或rocket

容器引擎，运行容器，负责本机的容器创建和管理工作。

### 三、POD创建过程

POD创建时序图

![](https://s3.starssr.com/image-2xva.png)

1、用户提交创建POD请求

2、API Server 处理用户请求，存储Pod数据到Etcd

3、Schedule通过和 API Server的监听机制，查看到新的pod，尝试为Pod绑定Node

4、过滤主机：调度器用一组规则过滤掉不符合要求的主机，比如Pod指定了所需要的资源，那么就要过滤掉资源不够的主机

5、主机打分：对第一步筛选出的符合要求的主机进行打分，在此阶段，调度器会考虑一些整体优化策略，比如把一个Replication Controller的副本分布到不同的主机上，使用最低负载的主机等

6、选择主机：选择得分最高的主机，进行binding操作，结果存储到Etcd中

7、kubelet根据调度结果执行Pod创建操作：绑定成功后，会启动container, Docker run, scheduler会调用API Server的API在etcd中创建一个bound pod对象，描述在一个工作节点上绑定运行的所有pod信息。运行在每个工作节点上的kubelet也会定期与etcd同步bound pod信息，一旦发现应该在该工作节点上运行的bound pod对象没有更新，则调用Docker API创建并启动pod内的容器

8、POD创建完成

### 四、K8S各组件工作流程

![](https://s3.starssr.com/image-crsd.png)

工作流程

1、运维人员向kube-apiserver发出指令（我想干什么，我期望事情是什么状态）

2、api响应命令,通过一系列认证授权,把pod数据存储到etcd,创建deployment资源并初始化。(期望状态）

3、controller通过list-watch机制,监测发现新的deployment,将该资源加入到内部工作队列,发现该资源没有关联的pod和replicaset,启用deployment controller创建replicaset资源,再启用replicaset controller创建pod。

4、所有controller被创建完成后.将deployment,replicaset,pod资源更新存储到etcd。

5、scheduler通过list-watch机制,监测发现新的pod,经过主机过滤、主机打分规则,将pod绑定(binding)到合适的主机。

6、将绑定结果存储到etcd。

7、kubelet每隔 20s(可以自定义)向apiserver通过NodeName 获取自身Node上所要运行的pod清单.通过与自己的内部缓存进行比较,新增加pod。

8、kubelet创建pod。

9、kube-proxy为新创建的pod注册动态DNS到CoreOS。给pod的service添加iptables/ipvs规则，用于服务发现和负载均衡。

10、controller通过control loop（控制循环）将当前pod状态与用户所期望的状态做对比，如果当前状态与用户期望状态不同，则controller会将pod修改为用户期望状态，实在不行会将此pod删掉，然后重新创建pod。

总结

k8s的组件

k8s中有两大节点，分别是master节点和node节点，

其中master节点包含apiserver、controller-manager、scheduler

还有一个etcd作为分布式存储，保存了整个集群的状态

node节点中包含kubelet、kube-proxy以及docker等容器引擎

k8s的工作流程

k8s的工作流程大致是，运维人员像apiserver发出创建Pod请求，告诉它我想干什么，我的期望是什么

API Server 响应请求，并通过一系列认证授权，把请求存储到etcd

并通知Controller-manager，它会通过API server读取etcd，然后按照所预设的模板去创建Pod，并将pod数据写入etcd

然后Controller-manager 会通过API Server去找Scheduler 为新创建的Pod选择最适合的Node 节点。Scheduler 会通过预算策略在所有Node节点中挑选最优的。

Node 节点中还剩多少资源是通过汇报给API Server 存储在etcd 里，API Server 会调用一个方法找到etcd 里所有Node节点的剩余资源，再对比Pod 所需要的资源，在所有Node 节点中查找哪些Node节点符合要求。

如果都符合，预算策略就交给优选策略处理，优选策略再通过CPU的负载、内存的剩余量等因素选择最合适的Node 节点，并把Pod调度到这个Node节点上运行。

controller manager会通过API Server通知kubelet去创建pod，然后通过kube-proxy中的service对外提供服务接口。

————————————————

版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

原文链接：[https://blog.csdn.net/qq\_47855463/article/details/119576285](https://blog.csdn.net/qq_47855463/article/details/119576285)