### RPC简介

**三个要点**

- Call ID 映射
- 序列化与反序列化
- 网络传输

**影响因素**

- 客户端能否与服务端（快速）建立连接
- 序列化和反序列化速度



**为什么是rpc而不是http**

- 提高传输效率和安全，rpc是一种思想可以使用多种技术来实现；
- http头有很多无用的数据，影响效率
- http三次握手和四次挥手一些规矩增加了网络开销





#### 调用过程

![img](https://pic1.zhimg.com/v2-d6ca7eb4f979b77a4aaac9c3b6315508_b.jpg)



UML时序图

![img](https://pic2.zhimg.com/v2-56b109fcc45ac57f3e436f9ea29a630d_b.jpg)