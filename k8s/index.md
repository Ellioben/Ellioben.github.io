# K8S


> 待更新......

全名Kubernetes（虚拟容器批量管理工具之一）
学习文档 https://kubernetes.io/zh-cn/docs/tasks/tools/

Kubernetes 作为一个容器集群管理系统，用于管理云平台中多个主机上的容器应用，[Kubernetes](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMzI0MDQ4MTM5NQ%3D%3D%26mid%3D2247509120%26idx%3D2%26sn%3D7a1541742111f5faee1716aac58bbf28%26chksm%3De918c19cde6f488a1e67f9a83ced62449027412e90ab349027e458c9a68c15dfdadb2843db63%26scene%3D21%23wechat_redirect) 的目标是让<u>部署容器化的应用变得简单且高效</u>，所以 Kubernetes 提供了应用部署，规划，更新，维护的一整套完整的机制。

<u><font color='#ff4' style="font-weight:bold">比起k8s是构建容器编排工具来说，在我看来k8s更像是一个虚拟服务容器级别的生态圈。在这个生态圈里、孵化出很多的东西（学起来没有底的)</font></u>

k8s服务架构

![image-20221012174402469](/img/image-20221012174402469.png)

- Master node
  - k8s集群控制节点，对集群迸行调度管理，接受集群外用户去集群操作请求;
  - Master node由 API Server、 Scheduler、 Cluster state store（ETCD数据库）和 Controller Manger Server所组成;
- Worker node
  - 集群工作节点，运行用户业务应用容器;
  - Worker Node包含 kubelet、 kube proxy和 Container Runtime;



## 准备

> k8s是**基于docker的。要先安装docker**

### Kubeadm

> 有多台vm推荐kubeadm安装k8s环境



### minikube

> 没有vm推荐使用minikube

安装minikube（在docker跑了个ubantu跑了个minikube的节点）

```bash
brew install minikube
#启动
minikube start
```

https://k8s.easydoc.net/docs/dRiQjyTY/28366845/6GiNOzyZ/9EX8Cp45

## 主节点需要组件

- docker（也可以是其他容器运行时）
- kubectl 集群命令行交互工具



## 工具

自己本地测试需要安装的工具

- minikube
- kubectl
- k9s好用的dashboard（可选）



```bash
brew install kubectl 
or
brew install kubernetes-cli
#测试一下，确保你安装的是最新的版本：
kubectl version --client

```





## 常用的指令

```bash
#创建namespase
kubectl create ns test
# apply 创建一个k8s组建
kubectl apply -n test -f xxxx.yaml
#
kubectl get pods -n test
# 查看 pod 对外的ip、 运行状态
kubectl get pod -o wide 

# get service
kubectl get svc -n test
# 编辑对应的配置文件
kubectl edit svc xxx-service -n test
# 查看 log 
kubectl logs pod-name 
# 进入 Pod 容器终端， -c container-name 可以指定进入哪个容器。
kubectl exec -it pod-name -- bash 
# 如果这个pod有多个容器
kubectl exec -it pod-name -c container-name -- bash 



# 伸缩扩展副本 
kubectl scale deployment xxx-deplyment --replicas=5 
# 把集群内端口映射到节点  容器外port:容器里port
kubectl port-forward pod-name 8090:8080 
# 删除部署 
kubectl delete deployment test-k8s `
```



## 组件

### Pod

>重要概念 PodK8S 调度、管理的最小单位，一个 Pod 可以包含一个或多个容器，每个 Pod 有自己的虚拟IP。一个工作节点可以有多个 pod，主节点会考量负载自动调度 pod 到哪个节点运行。

pod挂了会生成一个pod，新的pod的ip就会变了。

通过

```bash
kubectl apply -f pod.yaml
```

pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
spec:
  containers:
    - name: test-k8s 
      image: ccr.ccs.tencentyun.com/k8s-tutorial/test-k8s:v1 
```

#### pod生命周期

![image-20221121002316447](/img/v3_1d24d9ab1b204909b773fdca3b3ed33c_img_000.gif)

- master节点

​	createpod -- api server-- etcd

​	scheduler --（watch）-- apiserver -- etcd-- 调度算法,把pod调度对应的node节点上

- node节点

​	kubelet --（watch）--apiserver -- 读取etcd拿到分配给当前节点pod -- docker创建容器 -- 创建完成 -- update pod status --apiserver -- etcd



### Service

- Service 通过 label 关联对应的 Pod
- Servcie 生命周期不跟 Pod 绑定，不会因为 Pod 重创改变 IP
- 提供了负载均衡功能，自动转发流量到不同 Pod
- 可对集群外部提供访问端口
- 集群内部可通过服务名字访问

`kubectl apply -f service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: test-k8s
spec:
  selector:
    app: test-k8s # (跟pod的lables关联的)
  type: ClusterIP
  ports:
    - port: 8080        # 本 Service 对外的端口
      targetPort: 8080  # service访问内部容器的端口
```

 `kubectl get svc`

查看服务详情 `kubectl describe svc test-k8s`，

可以发现 **Endpoints 是各个 Pod 的 IP**，也就是service会把流量转发到这些节点。 

服务的默认类型是`ClusterIP`，只能在集群内部访问，我们可以进入到 Pod 里面访问：
`kubectl exec -it pod-name -- bash`
`curl http://test-k8s:8080`

如果要在集群外部访问，可以通过端口转发实现（只适合临时测试用）：
`kubectl port-forward service/test-k8s 8888:8080`

> 如果你用 minikube，也可以这样`minikube service test-k8s`

对外暴露服务上面我们是通过端口转发的方式可以在外面访问到集群里的服务，如果想要直接把集群服务暴露出来，我们可以使用`NodePort` 和 `Loadbalancer` 类型的 Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: test-k8s
spec:
  selector:
    app: test-k8s
  type: NodePort
  ports:
    - port: 8080        # 本 Service 的端口
      targetPort: 8080  # 容器端口
      nodePort: 31000   # 添加节点端口，范围固定 30000 ~ 32767
```



### Service类型

**ClusterIP**

默认的，仅在集群内可用

**NodePort**

暴露端口到节点，提供了集群外部访问的入口
端口范围固定 30000 ~ 32767

**LoadBalancer**

需要负载均衡器（通常都需要云服务商提供，裸机可以安装 [METALLB](https://metallb.universe.tf/) 测试）
会额外生成一个 IP 对外服务
K8S 支持的负载均衡器：[负载均衡器](https://kubernetes.io/zh/docs/concepts/services-networking/service/#internal-load-balancer)

**Headless**

适合数据库
clusterIp 设置为 None 就变成 Headless 了，不会再分配 IP，后面会再讲到具体用法
[官网文档](https://kubernetes.io/zh/docs/concepts/services-networking/service/#headless-services)





### 常见的内置资源

#### Deployment

管理pod的

selector来识别是管理的相关pod

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-k8s 
spec:
  replicas: 2 
  selector:		
    matchLabels:
      app: test-k8s
  template:   
    metadata:
      labels:
        app: test-k8s 
    spec: 
      containers:
      - name: test-k8s 
        image: ccr.ccs.tencentyun.com/k8s-tutorial/test-k8s:v1 # 镜像
```

Deployment1

- Lable1
  - Pod1
  - Pod1

Deployment2

- lable2
  - Pod2
  - Pod2





#### StatrfuSet

什么是 StatefulSet

StatefulSet 是用来管理有状态的应用，例如数据库。
前面我们部署的应用，都是不需要存储数据，不需要记住状态的，可以随意扩充副本，每个副本都是一样的，可替代的。
而像数据库、Redis 这类有状态的，则不能随意扩充副本。
StatefulSet 会固定每个 Pod 的名字



特性：

- StatefulSet 特性Service 的 `CLUSTER-IP` 是空的，
- Pod 名字也是固定的。Pod 创建和销毁是有序的，
- 创建是顺序的，销毁是逆序的。



#### job & cron job

一个临时pod执行相应任务
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```




#### PVC和Pod

类似关系

- **PV的全称是:** PersistentVolume（持久化卷），是对底层的共享存储的一种抽象，PV由管理员进行创建和配置，它和具体的底层的共享存储技术的实现方式有关，比如ceph、G| usterFS、NFS等，都是通过插件机制完成与共享存储的对接。I
- **PVC的全称是:** PersistentVolume claim（持久化卷声明），PVC是用户存储的一种声明，PVC和Pod比较类似，Pod消耗的是节点，PVC消耗的是PV资源，Pod可以请求CPU和内存，而PVC可以请求特定的存储空间和访问模式。对于真正使用存储的用户不需要关心底层的存储实现细节，只需要直接使用PVC即可。

但是通过PvC请求到一定的存储空间也很有可能不足以满足应用对于存储设备的各种需求，而且不同的应用程序对于存储性能的要求可能也不尽相同，比如读写速度、并发性能等，为了解决这一问题， Kubernetes又为我们引入了一个新的资源对象: StorageClass，通过 StorageClass的定义，管理员可以将存储资源定义为某种类型的资源，比如快速存储、慢速存储等，用户根据StorageClass的描述就可以非常直观的知道各种存储资源的具体特性了，这样就可以根据应用的特性去申请合适的存储资源了。



### 自定义资源
```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  # 名字必需与下面的 spec 字段匹配，并且格式为 '<名称的复数形式>.<组名>'
  name: demos.example.com
spec:
  # 组名称，用于 REST API: /apis/<组>/<版本>
  group: example.com
  names:
    # 名称的复数形式，用于 URL：/apis/<组>/<版本>/<名称的复数形式>
    plural: demos
    # 名称的单数形式，作为命令行使用时和显示时的别名
    singular: demo
    # kind 通常是单数形式的帕斯卡编码（PascalCased）形式。你的资源清单会使用这一形式。
    kind: Demo
    # shortNames 允许你在命令行使用较短的字符串来匹配资源
    shortNames:
    - dm
    categories:
    - all
  # 可以是 Namespaced 或 Cluster
  scope: Namespaced
  # 列举此 CustomResourceDefinition 所支持的版本
  versions: #*****是个数组*****>阿尔法，bate版本....
    - name: v1
      # 每个版本都可以通过 served 标志来独立启用或禁止
      served: true
      # 其中一个且只有一个版本必需被标记为存储版本存在ETCD里
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                name:
                  type: string
                  pattern: '^test$'
      additionalPrinterColumns:
      - name: CR-Name
        type: string
        jsonPath: .spec.name
```

#### 使用自定义资源

待CRD创建完成之后，我们就可以使用它来创建我们的自定义资源了，其创建方式跟内置的资源如Pod这些是一样的，只是需要将kind、apiVersion指定为我们CRD中声明的值，比如使用上面例子中的CRD定义资源：

```yaml
apiVersion: "example.com/v1"
kind: Demo
metadata:
  name: crd-demo
spec:
  name: test
```
也可以通过一些工具去生成
