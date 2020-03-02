### ubuntu配置git

```shell
git config --global user.name "wby"
git config --global user.email "2829926173@qq.com"
ssh-keygen -C '2829926173@qq.com' -t rsa
ssh -T git@github.com  #检测ssh是否配的正确



```



- ssh生成到~.ssh/ ，将公钥复制到git
- 配置好后ssh不通解决办法:将下面代码拷贝到 /etc/ssh/ssh_config

```shell
Host github.com
User git
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```

