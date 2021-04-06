```go
/* runtime/chan.go*/
type hchan struct {
	qcount   uint           // total data in the queue 当前队列已经存储的元素数目
	dataqsiz uint           // size of the circular queue  环形队列的长度
	buf      unsafe.Pointer // points to an array of dataqsiz elements
	elemsize uint16 // 每个元素的大小
	closed   uint32 // 标识关闭状态
	elemtype *_type // element type
	sendx    uint   // send index 往chan中发送 队列尾部发送，channel发送操作处理到的位置
	recvx    uint   // receive index  从chan中读取，队列头部取数据， channel接收操作处理到的位置
	recvq    waitq  // list of recv waiters //缓冲区为空时候：等待读消息的协程队列
	sendq    waitq  // list of send waiters 、、 缓冲区满时：等待写消息的协程队列 chan缓冲区为nil， 则数据放入sendq队列中，包含被阻塞的goroutine

	// lock protects all fields in hchan, as well as several
	// fields in sudogs blocked on this channel.

	// Do not change another G's status while holding this lock
	// (in particular, do not ready a G), as this can deadlock
	// with stack shrinking.
    // 不允许并发读写，一个管道只允许被一个协程读写
	lock mutex // 线程安全如何实现的：
}
```

- 因为读阻塞的协程被向管道中写入数据的协程唤醒
- 因为写阻塞的协程被从管道中读取数据的协程唤醒
- 一般情况下recvq和sendq至少有一个为空，只有当同一个协程使用select语句向chan一边读数据一边写数据时，该协程同时位于recvq和sendq中
- make（chan interface{}）向管道中传入任何数据类型
- 从chan中读取数据不一定从buf中读取， 因为如果recvq队列中有goroutine时候，数据会直接传递给该goroutine， 而不必再写入buf



### 向管道中读写数据

#### 写数据

[readChan]: https://www.processon.com/diagraming/5efc684b7d9c084420425521	"写数据"

#### 写数据

[writeChan ]（https://www.processon.com/diagraming/5efc684b7d9c084420425521) "读数据"

#### 关闭管道

关闭管道时会把recvq中的协程全部唤醒，这些协程获取到的数据都为nil，同时把sendq中的协程全部唤醒，这些协程会触发panic

所以尽量在发送方关闭chan

#### 造成G阻塞的chan操作

- 从缓冲区无数据的chan中读取数据
- 向缓冲区满的chan中写入数据
- 无缓冲区chan中，读写都会阻塞
- 向值为nil的chan读写数据，会造成永久性阻塞

#### 造成panic的chan操作

1. 向已被closed的chan中写入数据
2. 关闭值为nil也就是没初始化的chan
3. 关闭已经closed的chan

#### 单向管道只是函数参数的限制，并没有真正的单向管道,单向管道中 <-chan int (不可close)   chan <- int（可以close） 

#### select语句

1、select的case语句读取管道时候不会阻塞，因为不会将当前协程加入recvq中而是直接返回



#### foreach语句

for range 在管道被关闭时候可以正常处理，若没关闭，该goroutine会被加入recvq中，被阻塞，造成deadlock

```go
	ch := make(chan int, 10)
	for i := 0; i < 10; i++ {
		ch <- i
	}
	close(ch) // 没有close ：fatal error: all goroutines are asleep - deadlock!
	for e := range ch  {
		fmt.Printf("Get elemnt from ch %d\n", e)
	}
```

