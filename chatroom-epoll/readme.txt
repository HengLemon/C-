服务器端：

1.socket()创建监听Socket
2.bind()绑定服务器端口
3.listen()监听客户端连接
4.accept()接受连接
5.recv/send接收及发送数据
6.close()关闭socket

客户端：

1.socket()创建监听Socket
2.connect()连接服务器
3.recv/send接收及发送数据
4.close()关闭socket

服务端类的分析，我们需要下面的接口：

1.初始化 Init()
2.关闭服务 Close()
3.启动服务 Start()
4.广播消息给所有客户端 SendBoradcastMessage()

1.初始化 Init()
// 初始化服务端并启动监听
void Server::Init() {

    // Step 1：创建监听socket
    // 使用socket()

    // Step 2：绑定地址
    // 使用bind()

    // Step 3：监听连接
    // 使用listen()

    // Step 4：创建事件表
    // epoll_create()

    // Step 5：添加监听fd到epoll fd
    // addfd()
}


2.Close()
关闭所有打开的文件描述符

3.启动服务 Start()

// 启动服务端
void Server::Start() {
    // Step 1：初始化服务端
    // Init()

    // Step 2：进入主循环

    // Step 3：获取就绪的事件
    // epoll_wait()

    // Step 4：循环处理所有就绪的事件
    // 4.1 如果是新连接则接受连接并将连接添加到epoll fd
    // accept() addfd()

    // 4.2 如果是新消息则广播给其他客户端
    // SendBroadcastMessage
}


4.广播消息给所有客户端 SendBoradcastMessage()

// 发送广播消息给所有客户端
int Server::SendBroadcastMessage(int clientfd) {
    // Step 1：接收新消息
    // recv()

    // Step 2：判断是否是客户端中止连接

    // Step 3：判断是否聊天室还有其他客户端

    // Step 4：格式化发送的消息内容
    // sprintf()

    // Step 5：遍历客户端列表依次发送消息
    // send()
}




客户端类分析，我们需要下面的接口：

1.连接服务端Connect()
2.退出连接Close()
3.启动客户端Start()


1.连接服务端Connect()
// 连接服务器
void Server::Connect() {

    // Step 1：创建socket
    // 使用socket()

    // Step 2：连接服务端
    // connect()

    // Step 3：创建管道，其中fd[0]用于父进程读，fd[1]用于子进程写
    // 使用pipe()

    // Step 4：创建epoll
    // epoll_create()

    // Step 5：将sock和管道读端描述符都添加到内核事件表中
    // addfd()
}

2.退出连接Close()

需要关闭所有打开的文件描述符就可以。

需要注意判断是在父进程还是子进程，客户端中的两个进程打开的管道文件描述符是不同的。


3.启动客户端Start()

// 启动客户端
void Client::Start() {
    // Step 1：连接服务器
    // Connect()

    // Step 2：创建子进程
    // fork()

    // Step 3：进入子进程执行流程
    // 子进程负责收集用户输入的消息并写入管道
    // fgets() write(pipe_fd[1])

    // Step 4：进入父进程执行流程
    // 父进程负责读管道数据及epoll事件
    // 4.1 获取就绪事件
    // epoll_wait()
    // 4.2 处理就绪事件
    // 接收服务器端消息并显示 recv()
    // 读取管道消息并发给服务端 read() send()
}


Makefile 文件里的内容为我们上述每个编译和链接步骤的整合：

CC = g++
CFLAGS = -std=c++11

all: ClientMain.cpp ServerMain.cpp Server.o Client.o
    $(CC) $(CFLAGS) ServerMain.cpp  Server.o -o chatroom_server
    $(CC) $(CFLAGS) ClientMain.cpp Client.o -o chatroom_client

Server.o: Server.cpp Server.h Common.h
    $(CC) $(CFLAGS) -c Server.cpp

Client.o: Client.cpp Client.h Common.h
    $(CC) $(CFLAGS) -c Client.cpp

clean:
    rm -f *.o chatroom_server chatroom_client



再次注意$(CC) $(CFLAGS) ...前为一个tab，不是空格。

保存 Makefile 后，我们只需要在目录下执行make就可以生成可执行文件chatroom_server和chatroom_client。

现在进入运行测试阶段，首先启动服务端：

./chatroom_server
然后再打开新的XFCE终端，启动客户端：

./chatroom_client
可以看到服务端和客户端分别有一些日志输出，客户端也会收到欢迎信息。

为了加入更多的客户，可以打开新的XFCE终端，启动新的客户，每个客户的clientfd是不同的，发出去的消息在其他客户界面都可以看到来源。
