zookeeper，顾名思义，在分布式系统中，多半是承担协调、管理的角色的。

## 多种服务角色
zk中通常含有以下三种角色：leader、follower、observer。

- leader：事务请求的唯一调度者和处理者。

- follower：
    - 处理客户端的非事务请求，转发事务请求给Leader服务器
    - 参与事务请求Proposal的投票
    - 参与Leader选举投票

- observer：
    - 3.3.0版本以后引入的一个服务器角色，在不影响集群事务处理能力的基础上提升集群的非事务处理能力
    - 处理客户端的非事务请求，转发事务请求给Leader服务器
    - 不参与任何形式的投票

## 如何实现分布式锁
独占，读写，可重入