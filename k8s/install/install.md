# preinstall

## 关闭交换页

- 临时关闭

  ```bash
  swapoff -a
  ```

  

- 配置永久关闭

  ```bash
  swapoff -a
  vim /etc/fstab
  # 注释掉该行
  /swapfile                                 none            swap    sw              0       0
  
  # 重启 查看swap是否关闭
  swapon --show
  ```

  

## 关闭防火墙



## 安装配置containerd

### 安装

- 安装链接： https://docs.docker.com/engine/install/debian/#install-using-the-repository

#### 安装前准备

- 配置gpg（阿里源）

  ```bash
  sudo mkdir -p /etc/apt/keyrings
  
  sudo apt-get install \
      ca-certificates \
      curl \
      gnupg \
      lsb-release
  
  curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/debian/gpg |  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
  ```

  

- 配置docker container源

  ```bash
  echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```

- 安装contrainerd

  ```bash
  sudo apt-get update
  
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
  ```

  

### 配置

- 删除contrainer配置

  ```bash
  sudo rm /etc/containerd/config.toml
  ```

- 配置pause yuan

  ```bash
  containerd config default > /etc/containerd/config.toml   
  更改containerd 的/etc/containerd/config.toml 中的pause 源 为阿里云 registry.aliyuncs.com/google_containers
  ```

- 重启containerd

  ```bash
  systemctl enable --now containerd
  systemctl restart containerd.service
  ```

  

## k8s相关配置

### 安装源配置

```bash
# 使得 apt 支持 ssl 传输
apt-get update && apt-get install -y apt-transport-https
# 下载 gpg 密钥
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 
# 添加 k8s 镜像源
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF
# 更新源列表
apt-get update
# 下载 kubectl，kubeadm以及 kubelet
# apt-get install -y kubelet kubeadm kubectl 现在会下载最新版本，最新版使用contained
apt-get install -y kubelet kubeadm kubectl
```



### kubeadm 配置修改

```bash
kubeadm config print init-defaults

# 修改ip，node名称， 以及对应的镜像源
apiVersion: kubeadm.k8s.io/v1beta3
bootstrapTokens:
- groups:
  - system:bootstrappers:kubeadm:default-node-token
  token: abcdef.0123456789abcdef
  ttl: 24h0m0s
  usages:
  - signing
  - authentication
kind: InitConfiguration
localAPIEndpoint:
  advertiseAddress: 192.168.3.20  #改为自己主机IP
  bindPort: 6443
nodeRegistration:
  criSocket: unix:///var/run/containerd/containerd.sock
  imagePullPolicy: IfNotPresent
  name: master  # 修改node名称
  taints: null
---
apiServer:
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta3
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controllerManager: {}
dns: {}
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: registry.aliyuncs.com/google_containers #更改为阿里源
kind: ClusterConfiguration
kubernetesVersion: 1.25.0
networking:
  dnsDomain: cluster.local
  serviceSubnet: 10.96.0.0/12
scheduler: {}


```





# install

## 安装命令

```bash
kubeadm init --config [init.yaml]
```



## node节点添加

### master节点执行该命令

```bash
kubeadm token create --print-join-command

# node节点执行join命令即可，不用再init一次
```



## 网络插件安装

```bash
wget --no-check-certificate https://docs.projectcalico.org/manifests/calico.yaml

# 更改pod ip段        
# The default IPv4 pool to create on startup if none exists. Pod IPs will be
# chosen from this range. Changing this value after installation will have
# no effect. This should fall within `--cluster-cidr`.
- name: CALICO_IPV4POOL_CIDR
  value: "192.168.3.0/16"


```



## 安装失败处理

```bash
# 重置安装
kubeadm reset 
# 删除manifest
rm -rf /etc/kubernets/manifest
```



## 安装完成配置kubectl

```bash
cp /etc/kubernets/admin.config  ~/.kube/config
chmod +x ~/.kube/config
```



# 安装问题排查

## 日志查看

```bash
# 查看kebelet是否启动
systemctl status kubelet

# 启动日志, 日志可以重定向到文件中查看
journalctl -xeu kubelet
```



