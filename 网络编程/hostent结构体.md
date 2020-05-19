### hostent结构体解析和使用

```c++
struct  hostent {
        char    FAR * h_name;           /* official name of host */
        char    FAR * FAR * h_aliases;  /* alias list */
        short   h_addrtype;             /* host address type */
        short   h_length;               /* length of address */
        char    FAR * FAR * h_addr_list; /* list of addresses */
#define h_addr  h_addr_list[0]          /* address, for backward compat */
};
```



### 解析

根据域名获取主机相关信息

### 使用

```c

#include <stdio.h>
#include <stdlib.h>  
#include <netdb.h>  
#include <string.h>
#include <sys/socket.h> 
#include <unistd.h>
#include <arpa/inet.h>
 
 int main(int argc, char** argv)  
 {  
     
     /*windows 需要添加以下处理*/
     /*
    为了在应用程序当中调用任何一个Winsock API函数，首先第一件事情就是必须通过WSAStartup函数完成对Winsock服务的初始化，
    因此需要调用WSAStartup函数。使用Socket的程序在使用Socket之前必须调用WSAStartup函数。
    */
  #if windows
    WORD wVersionRequested;
    WSADATA wsaData;
    int ret;
 
    //WinSock初始化
    wVersionRequested = MAKEWORD(2, 2); //希望使用的WinSock DLL的版本
    ret = WSAStartup(wVersionRequested, &wsaData);
    if(ret != 0)
    {
        appLog("WSAStartup() failed!");
 
        return FALSE;
    }
 
    //确认WinSock DLL支持版本2.2
    if(LOBYTE(wsaData.wVersion) != 2 || HIBYTE(wsaData.wVersion) != 2)
    {
        appLog("Invalid WinSock version!");
        WSACleanup();
 
        return FALSE;
    }
#endif //end deal windows
	struct hostent *host = NULL;
	char **pptr; 
	char domain[1024]={0};	
	if(argc != 2)
	{
		printf("Usage: hostname <domain>\n");
		exit(0);
	}	
	memset(domain,0,sizeof(domain));
	sprintf(domain,"%s",argv[1]);
	if((host = gethostbyname(domain)) == NULL || host->h_addr_list == NULL || host->h_addr_list[0] == NULL)
	{
		printf("can't parse the domain\n");
		exit(0);		
	}
	else
	{
		printf("official hostname:%s\n",host->h_name);
		for(pptr = host->h_aliases; *pptr != NULL; pptr++)
			 printf("alias:%s\n",*pptr);
		switch(host->h_addrtype)	
		{	
			case AF_INET:	
			case AF_INET6:
				pptr = host->h_addr_list;  
				for(; *pptr!=NULL; pptr++)
                   printf("address:%s\n",inet_ntop(host->h_addrtype, *pptr, domain,64));
			    printf("first address: %s\n",inet_ntop(host->h_addrtype, host->h_addr,domain,64));				
				break;
			default:
				printf("Unkown address type\n");
		}
	}
	return 0;
 }
```

