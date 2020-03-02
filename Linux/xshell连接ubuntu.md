### 安装openssh



- 检查openssh_server是否安装

```shell
		sudo apt install openssh-server
		#查看是否启动shh
		ps -e | grep sshd 
		#启动ssh
		sudo /etc/init.d/ssh status
```

- 查看防火墙状态
- sftp 端口 22





### ubuntu关闭防火墙

- 查看防火墙状态 ： sudo ufw status  --->inactive 为关闭
- 开启防火墙：         sudo ufw enable
- 关闭防火墙：         sudo ufw disable
- 外来访问默认：      ufw default allow/deny
- 定义阻挡规则：      ufw allow/deny PORT /TCP||UDP
- 根据服务名阻挡：  ufw allow/deny SERVICENAME  从/etc/services 中找到对应的service的端口
- 对特定ip port 协议（tcp/udp）进行设置：ufw allow/deny proto TCP from IP/PORT to  本机 IP PORT
- 删除规则：            ufw delete allow/deny PORT

**ubuntu上没有ftp服务端**



