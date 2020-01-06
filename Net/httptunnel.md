![img](http://www.wuzesheng.com/wp-content/uploads/2012/02/http_tunnel_1.jpg)

### Http Tunnel 工作过程

1. client向代理服务器发送要连接到Sever的请求（使用HTTP协议发送）；
2. 代理服务器向Sever发送连接请求（使用HTTP协议）；
3. 1，2步骤成功后相当于在client和server之间存在一条连通的TUNNEL（蓝线）；
4. Client和Sever使用私有的TCP/UDP协议进行数据收发。

### 实现方式

1、使用http的connect方法

2、在client和proxy 之间加个tunnel client 

​		在proxy和server之间价格tunnel server