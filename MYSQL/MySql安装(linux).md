## Centos 安装mysql

### 安装mariadb

*yum install mariadb mariadb-server mariadb-devel*

### 安装mysql

- *rpm -qa | grep MySql* 查看系统是否安装mysql

- 下载MySQL源安装包

  *wget <http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm>*

- 安装MySQL源

  *yum localinstall mysql57-community-release-el7-8.noarch.rpm*

- 查看MySQL源是否安装成功

  *yum repolist enabled | grep "mysql.*-community.*"*

- 修改MySQL安装版本

  ***vim /etc/yum.repos.d/mysql-community.repo***

  将对应版本的enable=0 改为enable=1；

- 安装MySQL

  *yum install mysql-community-server*

- 启动MySQL服务

  *sudo systemctl start mysqld*

- 开机启动

  *sudo system enable mysqld*

  *sudo system daemon-reload*

- 修改登录密码（mysql会初始化一个随机密码用于首次登录（rpm安装），保存在/var/log/mysql.log中）

  *cat /var/log/mysqld.log | grep password*

- 登录mysql并修改登录密码

  - msyql 限制密码安全性， validate_password_policy值限定了最小密码长度， 修改使之可以设置简单密码
  - *set global validate_password_policy=0;* 使得密码判断长度基于下面的变量
  - *set global validate_password_length=1;*

- **MySQL5.7 中user表中password字段改成authentication_string了**

#### 安装出现的问题：

> Job for mysqld.service failed because the control process exited with error code. See "systemctl status mysqld.service" and "journalctl -xe" for details.

- 删除/var/lib/mysql  重启mysql服务 （<https://blog.csdn.net/aiyowei1106/article/details/88703746>）

**学习别人解决问题的思路**



