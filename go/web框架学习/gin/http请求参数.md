- 请求参数



1. Path

   ```
   URL中从域名之后/开始的部分
   获取方法：
   gin.Context.Param(key string) 
   gin.Context.Param.Get(key string) string bool
   gin.Context.Param.ByNam(key string) string
   ```

2. 