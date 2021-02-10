### libevent编译过程

- 查看libevent文档即可

  ```shell
  $ mkdir build && cd build
  $ cmake .. # Default to Unix Makefiles
  $ make
  $ make verify # Optional
  ```



- #### 缺少openssl头文件

  ```shell
  sudo apt-get install openssl
  
  sudo apt-get install libssl-dev
  ```

  



### 解决cmake编译出来的可执行文件没有调试信息（该方法未实验，暂时对cmake不熟悉）

```cmake
SET(CMAKE_BUILD_TYPE "Debug")
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g2 -ggdb")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
```



### 使用cmake命令

```cmake
cmake ..  -D CMAKE_BUILD_TYPE=DEBUG
```



### 问题分析过程

- 查看cmake 文件CMakLists.txt 中关于调试的信息
- 找到相关信息，查看cmake文档查找相关命令用法