### Ubuntu安装go

- 将golang解压到 /usr/local 目录中

  ```shell
  sudo tar -C /usr/local -xzf go1.12.5.linux-amd64.tar.gz 
  ```

- 创建工程目录 src bin pkg

- 设置环境变量

  ```shell
  export GOROOT=/usr/local/go
  export GOPATH=$HOME/workspace
  export GOBIN=$HOME/workspace/bin
  export PATH=$PATH:$GOROOT/bin #将go配置到系统环境变量中， 启动go
  ```

  

### 安装goland

- 解压到/usr/local

- 启动goland.sh

- 添加桌面图标方法：

```shell
        sh goland.sh

        goland打开后点击任务栏：

        tools---->create desktop entry.
```

