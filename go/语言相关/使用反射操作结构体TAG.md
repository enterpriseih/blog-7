1、使用原始字符串给结构体字段打标签
#包含tag标签的结构体#
3、使用Type.FieldByName 获取StructField 结构值， 直接使用符号  **.**获取结构体TAG标签值

```$xslt
type StructField struct {
	// Name is the field name.
	Name string
	// PkgPath is the package path that qualifies a lower case (unexported)
	// field name. It is empty for upper case (exported) field names.
	// See https://golang.org/ref/spec#Uniqueness_of_identifiers
	PkgPath string

	Type      Type      // field type
	Tag       StructTag // field tag string
	Offset    uintptr   // offset within struct, in bytes
	Index     []int     // index sequence for Type.FieldByIndex
	Anonymous bool      // is an embedded field
}
```

4、对标签键值对的操作函数
    lookup ： 返回2个值， 第二个返回值为 bool
    get    ： 返回一个值，舍弃掉bool判断

    ```
通过key获取value注意事项：
1、value必须包含在双引号中
    ```

5、使用TAG不影响结构体之间的相互转换

6、标签长度没有限制