### INSTALL

#### install cmd

```shell
--prefix=<install-root-dir>
		./configure --prefix=/usr/local
--enable-debug
--with-jemalloc-prefix=<prefix>
		malloc()         --> prefix_malloc()
		malloc_conf      --> prefix_malloc_conf
		/etc/malloc.conf --> /etc/prefix_malloc.conf
		MALLOC_CONF      --> PREFIX_MALLOC_CONF
```

./configure --prefix=/usr/local --enable-debug --with-jemalloc-prefix=<je>