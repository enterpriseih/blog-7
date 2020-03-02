#### 网络配置

虚拟机安装使用的NAT模式

```shell
#enoXXX 为自己的网卡名称， 使用ip addr 可以查看
将/etc/sysconfig/network-scripts/ifcfg-enoXXX中的ONBOOT=no 改为ONBOOT=yes
#重启网路服务
service  network restart


```



### 防火墙配置

```shell
#关闭防火墙
systemctl stop firewalld
#关闭防火墙自动启动
systemctl disable firewalld.service 
#查看防火墙状态 fire和cmd之间没空格
firewall-cmd --state
```





### 设置ntp时间同步服务 

```shell
1、安装ntp 
yum install -y ntp 
2、设置NTP服务开机启动 
chkconfig ntpd on 
service nptd start

```



