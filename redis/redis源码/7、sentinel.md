#### sentinel任务

- 监控
- 选主 在线状态+网络状态----》优先级 > 复制进度 （主库master_repli_offst 和slave_repli_offset）> ID号
- 通知
- 配置提供者：客户端初始化连接sentinel节点集合，获取主节点信息。



