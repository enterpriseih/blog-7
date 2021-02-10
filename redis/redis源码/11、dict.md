### 数据组织

```c
typedef struct dictType {
    uint64_t (*hashFunction)(const void *key);
    void *(*keyDup)(void *privdata, const void *key);
    void *(*valDup)(void *privdata, const void *obj);
    int (*keyCompare)(void *privdata, const void *key1, const void *key2);
    void (*keyDestructor)(void *privdata, void *key);
    void (*valDestructor)(void *privdata, void *obj);
} dictType;

//存储key/value的结构
typedef struct dictEntry {
    void *key;
    union {
        void *val;
        uint64_t u64;
        int64_t s64;
        double d;
    } v;
    struct dictEntry *next; ////如果key散列后相同，使用next指针。链接法,没有冲突时候，next指向自身。
} dictEntry;

//
typedef struct dictht {
    dictEntry **table; 
    unsigned long size;
    unsigned long sizemask;
    unsigned long used;
} dictht;

typedef struct dict {
    dictType *type; 
    void *privdata;
    dictht ht[2];
    long rehashidx; 
    unsigned long iterators;
}dict;

// dict的迭代器， 在发生扩容时候，只有dictnext（）可以安全使用
typedef struct dictIterator {
    dict *d;  // 迭代的字典
    long index; // 当前迭代到字典的哪个索引值
    int table, safe; // table 指明迭代的为ht[0]还是ht[1], safe表名创建的迭代器是否安全
    dictEntry *entry, *nextEntry; // nextEntry指明当前节点冲突链表上下一个节点
    long long fingerprint; //表明字典是否改变。对字典中所有字段值组合在一起生成的hashkey
} dictIterator;
```



### hash计算

- 使用times 33 散列算法，将输入的key转化成string

  ```c
  uint64_t siphash(const uint8_t *in, const size_t inlen, const uint8_t *k) {
  #ifndef UNALIGNED_LE_CPU
      uint64_t hash;
      uint8_t *out = (uint8_t*) &hash;
  #endif
      uint64_t v0 = 0x736f6d6570736575ULL;
      uint64_t v1 = 0x646f72616e646f6dULL;
      uint64_t v2 = 0x6c7967656e657261ULL;
      uint64_t v3 = 0x7465646279746573ULL;
      uint64_t k0 = U8TO64_LE(k);
      uint64_t k1 = U8TO64_LE(k + 8);
      uint64_t m;
      const uint8_t *end = in + inlen - (inlen % sizeof(uint64_t));
      const int left = inlen & 7;
      uint64_t b = ((uint64_t)inlen) << 56;
      v3 ^= k1;
      v2 ^= k0;
      v1 ^= k1;
      v0 ^= k0;
  
      for (; in != end; in += 8) {
          m = U8TO64_LE(in);
          v3 ^= m;
  
          SIPROUND;
  
          v0 ^= m;
      }
  
      switch (left) {
      case 7: b |= ((uint64_t)in[6]) << 48; /* fall-thru */
      case 6: b |= ((uint64_t)in[5]) << 40; /* fall-thru */
      case 5: b |= ((uint64_t)in[4]) << 32; /* fall-thru */
      case 4: b |= ((uint64_t)in[3]) << 24; /* fall-thru */
      case 3: b |= ((uint64_t)in[2]) << 16; /* fall-thru */
      case 2: b |= ((uint64_t)in[1]) << 8; /* fall-thru */
      case 1: b |= ((uint64_t)in[0]); break;
      case 0: break;
      }
  
      v3 ^= b;
  
      SIPROUND;
  
      v0 ^= b;
      v2 ^= 0xff;
  
      SIPROUND;
      SIPROUND;
  
      b = v0 ^ v1 ^ v2 ^ v3;
  #ifndef UNALIGNED_LE_CPU
      U64TO8_LE(out, b);
      return hash;
  #else
      return b;
  #endif
  }
  ```

  

### 扩容策略

- 时机：used > size 扩容， used/size < 10% 缩容

- 如何扩容：渐进式rehash--》ht[1]大小置为2的n次方，当进行增删改查时候，判断是否正在扩容中，进行中调用rehash操作，会从ht[0]移入1个数据给ht[1]

  ，当服务空闲时候，每次执行一秒rehash，从ht[0]移入100个数据给ht[1],ht[0] nil的时候，对调ht[1]和ht[0];

#### 字典的遍历

- 迭代器遍历
- 间断遍历 scan

### redis中的使用

### 常用接口

