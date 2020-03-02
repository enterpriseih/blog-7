

**大纲***问题引出原因和出处*       go代码如何规范编写，使得自己的代码规范能否和人看懂的需求和go特有需求，满足目标公司需求*问题是否解决*        开始解决。。。

**目录***问题概述*        源自于uber go编程规范，参考例文，以形成正规的编程规范
*具体内容*     1、编译器设置       [R]所有代码应该通过go lint 和go vet 检查          goland装go lint 和go vet并设置快捷键：         

#### 事前必做

- [R]所有代码应该通过go lint 和go vet 检查

  **goland装go lint 和go vet并设置快捷键， 保存时自动运行goimports, 自动运行go lint 和go vet**

  1. *goimports*
     - 作用:自动添加和删除import的包
     - 安装:
     - 用法