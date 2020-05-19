### 为什么要用do while（0）包含宏定义

- 防止宏定义的代码在宏展开后未被执行。大括号会影响；



```c++
#define CopyOilSysRespInfo( pstOilSysTran, pcRespCode, pcRespDesc ) \
    do { \
        bStrCpy( pstOilSysTran->sRespCode, pcRespCode, RESPCODE_LEN ); \
        bStrCpy( pstOilSysTran->sRespDesc, pcRespDesc, RESPDESC_LEN ); \
    } while (0);

```

在上述宏定义中，如果使用下面的调用,如果没有用do while 包含。自己没加大括号的话，导致第二个copy语句不会执行

```c
if ()
	CopyOilSysRespInfo( pstOilSysTran, pcRespCode, pcRespDesc );
```

