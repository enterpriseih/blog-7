### 主从配置和命令

#### 配置：

- 从节点redis.conf 中配置：replicaof masterIP masterPORT
- redis-cli 客户端中使用：replicaof IP PORT /slaveof IP PORT(也可以重建主节点。会清空原来该从节点数据)
- 断开： replicaof no one / slaveof no one
- 主从建立后查看相关状态：info replication

#### 作用

- 可以将耗时的操作配置在从节点上（慢查询，生成aof rdb 文件等）降低master压力，提高读写性能

