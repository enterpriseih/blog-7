### vender目录

**库存放目录**

参考pholcus源码



> gopath和vender调用顺序

-  vendor的搜索优先于GOPATH的搜索。
- vendor按照路径深度向外按顺序搜索，直到$GOPATH/src/vendor为止。



> vender层级关系处理

- 从引用文件所在的vendor路径下面搜索。
- 如果没有找到，那么从上层目录的vendor路径下面搜索。
- 直到src的vendor路径下面搜索。



### govendor的使用