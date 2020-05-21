- 三次握手过程（建立连接）
- 建立连接优化点
- 四次挥手过程（销毁连接）
- 销毁连接优化点
- select/poll/epoll 区别



### 三次握手过程

- 为什么需要三次握手：
- 

































```c++
#ifndef FD_SETSIZE
#define FD_SETSIZE      64
#endif /* FD_SETSIZE */

typedef struct fd_set {
        u_int fd_count;               /* how many are SET? */
        SOCKET  fd_array[FD_SETSIZE];   /* an array of SOCKETs */
} fd_set;
```



