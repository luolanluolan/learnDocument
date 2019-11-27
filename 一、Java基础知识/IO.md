

# 二、IO

## 1、IO概要

整个java.io包中最重要的就是5个类和一个接口。5个类指的是File、OutputStream、InputStream、Write、Reader;一个接口指的是Serializable.

Java I/O主要包含如下几个层次：

a. <u>流式部分</u>——IO的主体部分

b. <u>非流式部分</u>——主要包含一些辅助流式部分的类，如：File类、RandomAccessFile类和FileDescriptor类。

c. <u>其他类</u>——文件读取部分的与安全相关的类，如：SerializablePermission类，以及与本地操作系统相关的文件系统的类，如FileSystem 和Win32FileSystem类和WinNTFileSystem类 

## 2、IO体系

![image-20191121152100366](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191121152100366.png)

1、<u>File</u>(文件特征与管理)：用于文件或者目录的描述信息，例如生成新目录，修改文件名，删除文件，判断文件所在路径等。

2、<u>InputStream</u>(二进制格式操作)：抽象类，基于字节的输入操作，是所有输入流的父类 ，定义了所有输入流都具有的共同特征。

3、<u>OutputStream</u>(二进制格式操作)：抽象类，基于字节的输出操作，是所有输出流的父类 ，定义了所有输出流都具有的共同特征。

4、<u>Reader</u>(文件格式操作)：抽象类，基于字符的输入操作。

5、<u>Writer</u>(文件格式操作)：抽象类，基于字符的输入操作。

6、<u>RandomAccesFile</u>(随机文件操作)：一个独立的类，直接继承至Object，它的功能丰富，可以从文件的任意位置进行存取。

![20160421004327228](C:\Users\l\Desktop\20160421004327228.png)



## 3、字节流和字符流的区别

字节流在操作时本身不会用到缓冲区（内存），是文件本身直接操作的，而字符流在操作时使用了缓冲区，通过缓冲区再操作文件。使用字节流不用关闭流，字符流需要调用close()关闭流，不调用close()文件不输出。



## 4、选择使用

a、明确IO流中体系，字节输入流（InputStream）、字节输出流（OutputStream）、字符输入流（Reader）、字符输出流（Writer）

b、明确将要操作的数据是否是纯文本数据，如果数据源是纯文本选Reader;不是纯文本数据选择InputStream；如果数据目的地是纯文本选Writer,不是纯文本选OutputStream.

<img src="C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191121171736992.png" alt="image-20191121171736992" style="zoom:50%;" />



c、明确具体的设备，即数据源从哪个设备来的：是硬盘就加File;是键盘用System.in；是内存用数组；是网络用Socket流。目的地是哪个设备：是硬盘加File；是键盘用System.out；是内存用数组，是网络用Socket流。

<img src="C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191121172242131.png" alt="image-20191121172242131" style="zoom:50%;" />



 d、使用缓冲区，是就加上Buffered；②是否需要转换，是，就使用转换流，InputStreamReader 和OutputStreamWriter 

## 5、IO模型

IO有内存IO、网络IO、磁盘IO

IO模型有五种：

 **·**   <u>阻塞IO模型、非阻塞IO模型、IO复用模型、信号驱动的IO模型</u>（同步IO操作）

  **· ** <u>异步IO模型</u>（异步IO操作）

##### 1）阻塞IO模型

![image-20191126153027297](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191126153027297.png)                                                                                                                                                                                        

​       **内核数据准备好之前，系统调用会一直等待所有的套接字**（A拿着一支鱼竿在河边钓鱼，并且一直在鱼竿前等，等的时候不做其他的事情，只有鱼上钩的时候，才结束等的动作）

```tex
IO执行的两个阶段（等待数据和拷贝数据两个阶段）都被阻塞了

典型应用：阻塞Socket、Java BIO

进程阻塞挂起不消耗CPU资源、及时响应每个操作

实现难度低、开发应用较容易

适用并发量小的网络应用开发

不适用并发量大的应用、因为一个请求IO会阻塞进程，所以得为每个请求分配一个处理进程以及时响应、系统开销大。
```



##### 2）非阻塞IO模型

![image-20191126155000842](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191126155000842.png)



​        **每次客户询问内核是否有数据准备好，即文件描述符缓冲区是否就绪，当有数据报准备好时，就进行拷贝数据的操作。当没有数据报准备好时，也不阻塞程序，内核直接返回未准备就绪的信号，等待用户程序的下一个轮询,轮询对于CPU来说较大的浪费**（B也在河边钓鱼，等鱼上钩的时间段，B也在做其他的事情，但做其他事情的时候，每隔一个固定时间检查鱼是否上钩，一旦检查到有鱼上钩，就结束等动作，B检查鱼上钩是轮询）

```tex
典型应用：Socket设置NONBLOCK

进程轮询调用、消耗CPU的资源

实现难度低、开发应用相对阻塞IO模式较难

适用并发量较小、且不需要及时响应的网络应用开发
```



##### 3）IO复用模型

![image-20191126163729955](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191126163729955.png)

****

D同样也在河边钓鱼，但是D生活水平比较好，D拿了很多的鱼竿，一次性有很多鱼竿在等，D不断的查看每个鱼竿是否有鱼上钩。增加了效率，减少了等待的时间。 

```tex
典型应用：Java NIO、Nginx(epoll、poll、select)
专一进程解决多个进程IO的阻塞问题、性能好、Reactor模式
实现、开发应用难度较大
适用高并发服务应用开发、一个进程/线程响应多个请求
```



##### 4）信号驱动的IO模型

![image-20191126172138974](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191126172138974.png)

**当数据报准备好的时候，给我发送一个信号，对SIGIO信号进行捕捉，并且调用我的信号处理函数来获取数据报**（C也在河边钓鱼，他给鱼竿上挂一个铃铛，当有鱼上钩的时候，这个铃铛就会被碰响，C就会将鱼钓上）

##### 5）异步IO模型

![image-20191127090807549](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191127090807549.png)

**当应用程序调用aio_read时，内核一方面去取数据报内容返回，另一方面将程序控制权还给应用进程，应用进程继续处理其他事情，是一种非阻塞的状态。当内核中有数据报就绪时，由内核将数据报拷贝到应用程序中，返回aio_read中定义好的函数处理程序。**(D 同样也在河边钓鱼，但是D生活水平比较好，D拿了很多的鱼竿，一次性有很多鱼竿在等，D不断的查看每个鱼竿是否有鱼上钩。增加了效率，减少了等待的时间。  )