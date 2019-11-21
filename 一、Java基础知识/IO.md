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

http://www.360doc.com/content/18/0624/05/56167096_764796454.shtml



http://www.360doc.com/content/16/0616/21/11962419_568358287.shtml

未完啊…… ……