```go
/* runtime/slice.go*/
type slice struct {
	array unsafe.Pointer
	len   int
	cap   int
}
```

- make([]int , 5 ,10),使用make创建切片时候，底层数组长度等于切片容量大小
- 从数组中创建切片时候，共享底层数组，cap为切片起始位置到数组结束位置大小

### slice扩容机制

- append函数调用时候如果容量不够， 发生扩容
- cap（slice） < 1024 ,新slice扩容为原来2倍
- cap(slice) ?= 1024, 新slice扩容为原来1.25倍
- 代码：runtime/slice.go/growslice

#### slice缩容

- 一般使用copy函数而不是自动缩容，

### append(slice)步骤

- 原来slice容量够用，直接将新元素加入，slice.len++, 返回原slice
- 原slice容量不够，先将slice扩容，得到新slice（slice地址已经发生变化）
- 将新元素追加入新的slice， slice.len++, 返回新的slice，赋值给原来的slice

### copy(slice)

- 只按照最小长度得slice进行按位复制，不会发生扩容

### 切片表达式

- value[low:high]
- value[low:high:max]  max用于限制生成新切片容量，new slice 容量为max-low， 定容切片。这种切片增加数据时候，容量不足产生一个新的切片不会覆盖原有的切片和数组
- 若value为字符串或数组，  0 < low < high <= len(value),否则触发panic
- 若value为切片。0 < low < high <= cap(value)

### 切取string

- 切取出来的仍为string， slice支持随机读写， string不可以（实现决定）

#### 空切片和nil切片

- 空切片 make([]int, 0)
- nil切片 ， var s []int
- nil切片可以len cap append操作