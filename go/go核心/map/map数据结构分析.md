#### 对未初始化的map进行写操作会造成panic，但读和删除不会报错

#### 从map中取值最好判断key是否存在

#### make创建map时候尽量指定容量，避免频繁分配内存

#### map是非线程安全的，一旦发现读写冲突，会panic（未测试成功）

```golang
var m map[int]string
m = make(map[int]string, 5, 10)
if v，ok := m[1]; ok {
 	// do someting
}

```

### map数据结构

```golang
// A header for a Go map.
type hmap struct {
	count     int // # live cells == size of map.  Must be first (used by len() builtin)
	flags     uint8
	B         uint8  // 桶的个数为2的B次方
	noverflow uint16 // approximate number of overflow buckets; see incrnoverflow for details
	hash0     uint32 // hash seed

	buckets    unsafe.Pointer // hash桶的起始地址
	oldbuckets unsafe.Pointer // previous bucket array of half the size, non-nil only when growing
	nevacuate  uintptr        // progress counter for evacuation (buckets less than this have been evacuated)

	extra *mapextra // optional fields
}
```

#### bmap

```golang
// A bucket for a Go map.
// 后两个字段并没有在结构体中显式声明，
type bmap struct {
	// tophash generally contains the top byte of the hash value
	// for each key in this bucket. If tophash[0] < minTopHash,
	// tophash[0] is a bucket evacuation state instead.
	tophash [bucketCnt]uint8 //hash值相同的key，会将hash值高8位存储在该数组中，所以一个bucket最多存储8个key
	// Followed by bucketCnt keys and then bucketCnt elems.
	// NOTE: packing all the keys together and then all the elems together makes the
	// code a bit more complicated than alternating key/elem/key/elem/... but it allows
	// us to eliminate padding which would be needed for, e.g., map[int64]int8.
	// Followed by an overflow pointer.
    data   []byte   //key/key..../value/value.... key和walue连续存储，为了减少结构体对齐产生的空隙，每个bucket可以放8个键值对
    overflow *bmap // 溢出buket的地址
}
```

### 扩容原因

- 负载因子：num（key）/num(buckets), 从结构体可以看出，当没有溢出时候，最大负载因子为8
- 但是go  map在负载因子大于6.5时候就触发扩容
- 或者在overflow个数大于2的15次方

#### 增量扩容

- 申请新的buckets 空间，为原来的2倍，oldbuckets指向原来的buckets， buckets指向新的buckets

- 并非一次性复制完成，否则数据过大会造成程序性能下降，由于bmap的结构组织，所以每次对map进行

  访问时候，复制两个数据到新的buckets中，新增加的数据直接写道新的buckets中。

- 等oldbuckets中的数据迁移完成，就可以安全的删除oldbuckets指针指向的区域。



#### 等量扩容

- 当由于删除操作造成每个bucket中的数据分散
- bucket的数量不变，重新做一次rehash的动作。

#### hash值计算和使用

- 根据hash函数计算出key的hash值
- 取出hash值低八位对hmap.B进行取模，计算出数据放在哪个bucket中
- 将hash值的高八位放在bmap.tophash中，当下次查找数据时候对比直接获取key/value
- 在rehash过程中，添加元素直接加入新的bucket中，删除元素先从旧的bucket中查找