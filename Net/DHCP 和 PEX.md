### DHCP 和 PEX

- DHCP服务器分配IP方式

- DHCP连接过程
- PEX工作过程



> DHCP分配IP方式

- 手动分配:在手动分配中，网络管理员在DHCP服务器通过手工方法配置DHCP客户机的IP地址。当DHCP客户机要求网络服务时，DHCP服务器把手工配置的IP地址传递给DHCP客户机。
- 自动分配:地址永久给DHCP客户机
- 动态分配:租约到期, IP还给DHCP服务器



> DHCP(dynamic host configration protocol) 是一个局域网工作协议,使用UDP实现

- DHCP客户机:只要含有DHCP协议
- DHCP服务器:对IP请求作出响应, 管理IP地址;



> DHCP连接过程



![1557120540171](C:\Users\Tonglina\AppData\Roaming\Typora\typora-user-images\1557120540171.png)

IP租约期限到%50的时候,DHCP客户端向服务端发送DHCP Request续约



**问题**DHCP服务器给客户机发送响应到底是广播还是直接发送给客户机





> PEX工作过程

DHCP配置项加上字段;