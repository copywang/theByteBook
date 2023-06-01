# 物理资源抽象

Kubernetes 将管理的物理资源抽象成指定的类型和对应的计量单位，如 CPU (millicores)、内存 (B)、磁盘和 GPU 等。这些资源可以分类两类，一种是可压缩的资源，譬如 CPU， 当 Pod 使用超过 limits 限制时， Pod 中进程使用 CPU 会被限制，但不会被Kill，另一种是不可压缩资源，譬如 内存，当资源不足时，会先 kill 掉 优先级低的 Pod。

## CPU 资源

Kubernetes 中的一个 CPU 在不同运行环境是不同的， 在阿里云代表一个 vCPU，在物理裸机中代表采用 Intel 超线程处理器上运行的超线程。
一个CPU 被”毫“分隔成了1000份，也就是1000m

CPU 资源设定为资源的绝对数量而非相对数量值。 例如，无论容器运行在单核、双核或者 48 核的机器上，500m CPU 表示的是大约相同的计算能力。

## 内存

内存的约束和请求以字节为单位，你可以使用普通的不带单位的字节，或者带有具体单位的数字来表示内存：E、P、T、G、M、k。 你也可以使用对应的 2 的幂数：Ei、Pi、Ti、Gi、Mi、Ki。 例如，以下表达式所代表的是大致相同的值：

```
128974848, 129e6, 129M, 123Mi
```

注意 `123Mi = 123*1204*1204B = 129 M`

什么是2的幂数？举个例子 M 和 Mi ，1M就是`1*1000*1000`的字节，1Mi就是`1*1024*1024`字节，所以1M < 1Mi，显然带这个小i的更准确。



这些资源都是在 kubelet 启动的节点上面，有 kublet 负责上报，节点定义的 Node Capacity 是每个节点固定的资源总和,触发硬件发生变化, 譬如内存,可以认为这是一个常量。