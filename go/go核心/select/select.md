#### select结构

```golang
// select 中case结构体 /src/runtime/select.go
type scase struct {
	c           *hchan         // chan
	elem        unsafe.Pointer // data element， 从管道中接收数据存放的地址，和向管道中写入数据存放的地址
	kind        uint16
	pc          uintptr // race pc (for race detector / msan)
	releasetime int64
}

kind:
const (
	caseNil = iota //nil 管道
	caseRecv // 从管道读取数据
	caseSend // 向管道写入数据
	caseDefault // default语句
)
```



- 每个结构体都含有hchan，所以每个case中必须含有管道操作

- 只含有一个hchan，所以每个case只能处理一个管道

- caseNil表示空管道，不能读写，所以这类管道永远不会命中，当对nil管道进行case操作时候，不会引起panic

- default特殊类型，不操作管道，每个select中只含有一个。

  ```golang
  case caseDefault:
  		dfli = casi
  		dfl = cas
  ```



#### case选择顺序

- 多个case都准备好，随机选择一个case执行

- 在for循环中，

  1、select的每个case都会先求值，在执行读取或写入数据操作；求值顺序从左到右，
  
  每次select执行完成后，选中的case中将数据从管道中读入到变量。从上到下，跳出for循环使用goto或者break loop
  
  2、如果选中的某个管道判断出已经关闭，可以将管道重新赋值为nil， 因为nil管道的case默认不执行， 可以屏蔽掉
  
  对该case的检查。