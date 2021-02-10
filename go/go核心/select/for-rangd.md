### 管道没有数据时，for-range阻塞直到管道被关闭

```golang
// chan no value, pro sleep
func RangeNovalueChan()  {
	c := make(chan int, 2)
	c <- 2
	go func() {
		close(c)
	}()
	for v := range c {
		fmt.Println(v)
	}
}
```

### for-range可以使用数组指针但是不能使用切片指针

### for-range每次对迭代副本进行拷贝，对于元数据比较大的，尽量用索引下标来访问

```golang
s := []string
for i, v := range s {
	if v != "hello" // 不可取，因为复制string
    if s[i] != "hello"  // for-range 优化
	
}
```
