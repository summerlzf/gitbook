# Netty的特性

1. 基于NIO的网络通讯框架，具有高并发性
2. 使用堆外直接内存进行socket通讯，因此传输速度快
3. 封装了NIO操作的诸多细节，提供易于调用的接口



# Netty的重要组件

- Channel：网络操作抽象类，包含基本的I/O操作
- EventLoop：配合Channel处理I/O操作，处理连接的生命周期中发生的事情
- ChannelFuture：Netty中所有I/O均为异步，ChannelFuture提供事件监听的注册，执行完毕后触发返回结果
- ChannelHandler：主要用于处理各种事件，是事件逻辑处理的地方
- ChannelPipeline：为ChannelHandler提供容器，Channel创建时，会被分配专属的ChannelPipeline



# Netty和Tomcat的区别

- Tomcat是servlet容器，web服务器；Netty是基于异步事件驱动的网络程序框架和工具
- Tomcat基于HTTP协议；Netty可以编解码字节流，因此可以自定义各种协议，从而实现HTTP、FTP、RPC等服务器

