### acrorsync网络架构分析

- session层
- connection层
- channel层
- socket层

#### session层

- session层介绍：

  - 属于业务层， 具体的业务数据状态记录和业务逻辑处理。
  - 如果用到网络通信，使用connection层进行数据收发
  - session层不控制connection生命周期，因为connection可能由于网络问题引起connection对象销毁，从而引起session层销毁；

- rsync的session层分析

  - ```c++
    //组织收发数据的逻辑
    int Client::readIndex()/*protocol is 29or30*/
    {
        return d_protocol < 30 ? d_stream->readInt32() : d_stream->readIndex();
    }
    
    void Client::writeIndex(int index)
    {
        if (d_protocol < 30) {
            d_stream->writeInt32(index);
        } else {
            d_stream->writeIndex(index);
        }
    }
    ```

  - ```c++
    //调用网络层
    void Client::start(const char *remotePath, bool isDownloading, bool recursive, bool isDeleting)/*command configration*/
    {
        ...
        d_io->createChannel(command.c_str(), &d_protocol);
        ...
    }
    ```

  - ```c++
    //收发函数
        void sendChecksum(int index, const char *oldFile);
    
        void sendEntry(Entry *entry, bool isTop, bool noDirContent = false);
        bool sendFile(int index, const char *remotePath, const char *localPath);
        bool receiveEntry(std::string *path, bool *isDir, int64_t *size, int64_t *time, uint32_t *mode,std::string *symlink);
        bool receiveFile(const char *remotePath, const char *newFile, const char *oldFile, int64_t *fileSize);
    
    ```



#### connection层

- connection层介绍：
  - 网络框架中技术层的最上层，一个客户端对应一个connection
  - 记录该路链接的各种状态
    - 连接状态
    - 数据收发缓冲区信息
    - 数据流量状态
    - 本端和对端地址和端口
  - connection层含有channel层对象，并掌握该对象的申请和释放（生命周期）；

- rsync 的channel层分析

  ```C++
  class SocketIO : public IO
  {
  public:
      SocketIO();
      ~SocketIO();
  
      void connect(const char *serverList, int port, const char *user, const char *password, const char *module);
      void getConnectInfo(std::string *username, std::string *password, std::string *module);
      void setModule(const char *module);
      
      void createChannel(const char *remoteCommand, int *protocol);
      void closeChannel();
  
      virtual int read(char *buffer, int size);
      virtual int write(const char *buffer, int size);
      virtual void flush();
      virtual bool isClosed();
  
      virtual bool isReadable(int timeoutInMilliSeconds);
      virtual bool isWritable(int timeoutInMilliSeconds);
      
  private:
      // NOT IMPLEMENTED
      SocketIO(const SocketIO&);
      SocketIO& operator=(const SocketIO&);
  
      int d_socket;
  
      std::string d_serverList;
      int d_port;
      std::string d_user;
      std::string d_password;
      std::string d_module;
      
  };
  ```

  

#### channel层

- channel层介绍
  - 持有一个socketFd
  - 实际收发数据的地方
  - 记录当前记录监听的各种网络事件（读写出错）
  - 如果channel管理socket层生命周期：该层提供socket创建和关闭接口
  - 一个channel对象在一个线程中处理

- rsync 的channel层

  ```c++
  class IO
  {
  public:
      IO();
      virtual ~IO();
  
  	// NOT IMPLEMENTED
  	IO(const IO&) = delete;
  	IO& operator=(const IO&) = delete;
  
      // Create a communication channel.  'remoteCommand' is the rsync command to execute needed by an SSH connection.
      // '*protocol' gives the rsync protocol version.
      virtual void createChannel(const char *remoteCommand, int *protocol) = 0;
  
      // Close the channel (and this IO)
      virtual void closeChannel() = 0;
  
      // Return the account credentials previously stored in the object.
      virtual void getConnectInfo(std::string *username, std::string *password, std::string *module) = 0;
      
      // These two calls are blocking.
      virtual int read(char *buffer, int size) = 0;
      virtual int write(const char *buffer, int size) = 0;
  
      virtual void flush() = 0;
  
      // If the channel has been closed
      virtual bool isClosed() = 0;
  
      // Check if the channel is accessible.  Wait util the specified time elapsed.
      virtual bool isReadable(int timeoutInMilliSeconds) = 0;
      virtual bool isWritable(int timeoutInMilliSeconds) = 0;
  };
  
  ```

  

#### socket层

- 基础的操作系统的socket API封装

- rsync 的socket层

  ```c++
  class SocketUtil
  {
  public:
      // Methods for working with sockets.
      static void startup();
      static void cleanup();
  
      static int create(const char *host, int port, std::stringstream *error);
      static void close(int socket);
  
      static int read(int socket, char *buffer, int size);
      static int write(int socket, const char *buffer, int size);
      static bool isReadable(int socket, int timeoutInMilliSeconds);
      static bool isWritable(int socket, int timeoutInMilliSeconds);
  };
  ```

### 总结

- 严格来说acrorsync并不是一个网络库，只是符合网络编程的基本分层规范
- acrorsync 是一个客户端
- 具体需要分析rsync库--->网络通信部分不是rsync重点，重点是文件处理（读写校验对比）