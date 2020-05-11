### mysql连接池的文件类组成

与redis池化保持一致；

```c++
//新增的不同类型的类
class CResultSet;			//mysql 查询结果解析的抽象
class CPrepareStatement		//mysql prepare statement 接口的抽象，防止sql注入
```





- sql注入：
- prepare statement 如何防止sql注入：
- MYSQL_STMT结构作用和解析