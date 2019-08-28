### 类型分类

1. 命名类型：
   - 基础类型 int、float、string、bool等预定义类型
   - type自定义类型
2. 未命名类型
   - arr、struct、map、slice、pointer、interface、function、channel 字面量类型

### 底层类型

- 预定义类型底层类型就是本身
- 字面量类型底层类型就是类型本身（type A map[s]int, 底层类型就是map[s]int）
- type自定义类型是她引用类型的底层类型

### 可赋值性

- 两个变量有相同的底层类型， 并且至少其中一个不是命名类型；
- 底层类型不同，但是可以相互转化；



### 类型一致性

- 已定义类型和其他任何类型都不相同，所以int64 != int32  

  ```
  type A int64
  type B int64
  A != B
  ```

  

