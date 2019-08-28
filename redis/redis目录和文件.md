### redis网络相关

```
网络：anet.c anet.h networking.c endianconv.c endianconv.h
事件处理：ae.c ae.h ae_epoll.c ae_evport.c ae_kqueue.c ae_select.c blocked.c
IO操作： bio.h bio.c syncio.c
libevent网络库
```



### redis 客户端和服务端

```
config.c config.h  redis-cli.c setproctitle.c version.h redis.h redis.c 
```





### redis 数据文件相关

```
数据结构：
	adlist.h adlist.c t_list.c intset.c intset.h sds.c sds.h 		dict.c dict.h t_hash.c  t_set.c t_string.c t_zset.c
	hyperloglog.c ziplist.c ziplist.h
文件操作：
	bitops.c object.c valgrind.sup 	replication.c
	
算法	 ： 
	crc16.c  crc64.c  crc64.h lzf_c.c lzf_d.c、 lzf.h lzfP.h
	pqsort.c  pqsort.h rand.c rand.h redis-check-aof.c  redis-		check-dump.c sha1.c sha1.h
	
```



### redis辅助文件

```
makefile debug.c fmacros.h help.h msmtest.c zmalloc.c zmalloc.h redisassert.h pubsub.c release.c release.h testhelp.h util.c util.h  version.h redis-benchmar.c 
```





### redis 数据库文件

```
aof.c rdb.c rdg.h db.c multi.c notify.c redis-check-aof.c  redis-check-dump.c  slowlog.c slowlog.h
```



### redis集群

```
sentinel.c cluster.c  cluster.h
```

