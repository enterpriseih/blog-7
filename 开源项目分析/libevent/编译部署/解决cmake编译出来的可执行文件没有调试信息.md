### 解决cmake编译出来的可执行文件没有调试信息

```cmake
SET(CMAKE_BUILD_TYPE "Debug")
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g2 -ggdb")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
```

### 

### 使用cmake命令

```cmake
cmake ..  -D CMAKE_BUILD_TYPE=DEBUG
```



