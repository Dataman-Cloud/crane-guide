# Swarm 模式

为了使用 Docker 内置的 swarm 模式，你需要安装 Docker Engine v1.12.0 或者更新版本的 Docker，或者，相应的安装最新的 Docker for Mac, Docker for Windows Beta。

Docker Engine 1.12 内置了 swarm 模式来让用户便捷的管理集群。 通过 Docker CLI 可以创建一个 swarm 集群，部署应用到一个 swarm 集群，或者管理一个 swarm 集群的行为。

如果你正在使用 v1.12.0 以前版本的 Docker ，请参考 [Docker Swarm](https://docs.docker.com/swarm)。

## 主要特性

### 内置于 Docker Engine 的集群管理

可以直接用 Docker Engine CLI 来创建 Swarm 集群，并在该集群上部署服务。你不再需要额外的编排软件来创建或管理 Swarm 集群了。

### 去中心化设计

不同于在部署时就确定节点之间的关系, 新的 Swarm 模式选择在运行时动态地处理这些关系, 你可以用 Docker Engine 部署 manager 和 worker 这两种不同的节点。 这意味着你可以从一个磁盘镜像搭建整个 Swarm 。

### 声明式服务模型

Docker Engine 使用一种声明式方法来定义各种服务的状态。譬如，你可以描述一个由 web 前端服务，消息队列服务和数据库后台组成的应用。

### 服务扩缩

你可以通过 docker service scale 命令轻松地增加或减少某个服务的任务数。

### 集群状态维护

Swarm 管理节点会一直监控集群状态，并依据你给出的期望状态与集群真实状态间的区别来进行调节。譬如，你为一个服务设置了10个任务副本，如果某台运行着该服务两个副本的工作节点停止工作了，管理节点会创建两个新的副本来替掉上述异常终止的副本。 Swarm 管理节点这个新的副本分配到了正常运行的工作节点上。

### 跨主机网络

你可以为你的服务指定一个 overlay 网络。在服务初始化或着更新时，Swarm 管理节点自动的为容器在 overlay 网络上分配地址。

### 服务发现

Swarm 管理节点在集群中自动的为每个服务分配唯一的 DNS name 并为容器配置负载均衡。利用内嵌在 Swarm 中的 DNS 服务器你可以找到每个运行在集群中的容器。

### 负载均衡

你可以把服务的端口暴露给一个集群外部的负载均衡器。 在 Swarm 集群内部你可以决定如何在节点间分发服务的容器。

### 默认 TLS 加密

Swarm 集群中的节点间通信是强制加密的。你可以选择使用自签名的根证书或者来自第三方认证的证书。

### 滚动更新

docker service 允许你自定义更新的间隔时间, 并依次更新你的容器, docker 会按照你设置的更新时间依次更新你的容器, 如果发生了错误, 还可以回滚到之前的状态.

* [原文链接](https://docs.docker.com/engine/swarm/)
