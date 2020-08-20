### linger结构体

```C++
控制close/shutdown函数关闭选项结构体（仅适用于TCP SCTP）
struct  linger {
        u_short l_onoff;                /* option on/off */
        u_short l_linger;               /* linger time */
};
```

#### 使用函数setsockopt设置

```
#ifdef __GNUC__
    setsockopt(sock,SOL_SOCKET,SO_REUSEADDR,&on,sizeof(int));
    setsockopt(sock,SOL_SOCKET,SO_KEEPALIVE,&on,sizeof(int));
    setsockopt(sock,SOL_SOCKET,SO_LINGER,&ling,sizeof(struct linger));
  #elif defined WIN32
    setsockopt(sock,SOL_SOCKET,SO_REUSEADDR,(char *)&on,sizeof(int));
    setsockopt(sock,SOL_SOCKET,SO_KEEPALIVE,(char *)&on,sizeof(int));
    setsockopt(sock,SOL_SOCKET,SO_LINGER,(char *)&ling,sizeof(struct linger));
```

#### 设置选项

```
三种断开方式：

1. l_onoff = 0; l_linger的值被忽略
close()立刻返回，底层会将未发送完的数据发送完成后再释放资源，即优雅退出。

2. l_onoff != 0; l_linger = 0;
close()立刻返回，但不会发送未发送完成的数据，而是通过一个RST包强制的关闭socket描述符，即强制退出。

3. l_onoff != 0; l_linger > 0;
close()不会立刻返回，内核会延迟一段时间，这个时间就由l_linger的值来决定。如果超时时间到达之前，发送完未发送的数据(包括FIN包)并得到另一端的确认，close()会返回正确，socket描述符优雅性退出。否则，close()会直接返回错误值，未发送数据丢失，socket描述符被强制性退出。需要注意的时，如果socket描述符被设置为非堵塞型，则close()会直接返回值。

 
```

#### 参考文档

https://www.cnblogs.com/my_life/articles/5174585.html