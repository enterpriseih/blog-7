### 包管理工具（go mod）

- 用途简介
- 使用方法
- 使用技巧

#### go mod用途和简介

- 版本控制
- 去除掉对gopath的依赖

**go mod文件中指令**

- module ： 标识项目的根目录，可以在gopath之外的路径（基于go.mod文件路径）
- require  ： 具体包对应的路径和版本， 指示包的导入路径
- replace ： 取代当前项目的某些依赖包
- exclude ：排除某些包的标记的版本  旧包 （版本号）=>新包 （版本号）

**注意事项**

1. 项目中要使用完整的绝对路径来导入包
2. 相同的指令动作可以放在一个括号中
3. 使用//进行注释



#### 使用方法

- go版本最低为Go 1.11
- 安装go的工具链



**步骤**

- 开启go mod （默认是auto）

  ```
  export GO111MODULE=on  //在任何地方都开启mod
  export GO111MODULE=auto//在除GOPATH之外的地方开启mod
  ```

- mkdir 创建项目目录在GOPATH之外(也可以在GOPATH内创建)

  ```
  依据GO111MODULE 的值确定
  ```

  

- 初始化go.mod (.mod文件不能存在)

  ```
  go mod init [modlueName]
  ```

- go mod tidy：添加缺失的或者移除不需要的包，生成go.sum文件（记录模块下载条目）

  ```
  [如果工程中存在不确定版本的包，需要执行该条命令]
  go mod tidy -v //显示执行的信息，将删除和添加的包打印出来
  ```

- go mod verify

  ```
  检查当前模块的依赖是否全部下载， 是否被修改过，若果没修改过
  该命令返回下列语句
  all modules verified
  ```

  

- go mod vendor 

  ```
  vendor 目录存放go.mod 中存放的依赖包， 项目会先从vendor目录中索引库；
  生成mod.txt ,列出项目所有依赖的包
  go mod vendor -v 
  如果存在vendor目录，应该先删除掉
  ```





#### 使用技巧

**虚拟版本号**

- 



