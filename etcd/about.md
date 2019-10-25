# Etcd+Confd实现Nginx配置文件动态更新

## 简介
 ![](https://carey-akhack-com.oss-cn-hangzhou.aliyuncs.com/images/20181023/1.png
)

1. 如上图是一个很简单的架构，生产环境中经常会进行灰度发布，需要下掉一部分的节点
，如果靠人工操作很容易出错，这里通过Etcd和Confd来实现nginx upstream更  
2. Etcd是一个分布式kv存储系统，一般用于共享配置和服务注册与发现，使用 Raft 算法
维护一致性的 key-value 存储系统，与其类似产品有 Zookeeper、Consul 等，Etcd 相对 
Zookeeper，更加轻量、易运维。同时，Etcd 支持 TLS 通信，具备高性能的写入能力。
3. Confd 是一个轻量级的配置管理工具，源码地址：https://github.com/kelseyhightowe
r/confd，它可以将配置信息存储在etcd、consul、dynamodb、redis以及zookeeper等。con
fd定期会从这些存储节点pull最新的配置，然后重新加载服务，完成配置文件的更新。
