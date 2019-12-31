# 一、JDK

## 1、命令

### 	1.1、jps

​		（Jvm process status 虚拟机进程状态）:

​			jps命令格式：jps [options] [hostid]

​			显示指定系统内所有的HotSpot虚拟机的进程。

​			可用来查询到LVMID（Local Virtual Machine Identifier）

| options |                        作用                        |
| :-----: | :------------------------------------------------: |
|   -q    |            只输出LVMID，省略主类的名称             |
|   -m    |   输出虚拟机进程启动时传递给主类main()函数的参数   |
|   -l    | 输出主类的全名，如果进程执行的是jar包，输出Jar路径 |
|   -v    |            输出虚拟机继承启动时jvm参数             |

​	

### 1.2、jstat

​	（Jvm Statistics Monitoring Tool 虚拟机统计信息监视工具）:

​		jstat命令格式：jstat [options] [VMID] [interval 查询间隔毫秒] [count 查询次数]

​		用于收集HotSpot虚拟机各方面的运行数据。

​		它可以显示本地或者远程虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据。



### 1.3、jinfo

​	（Configuration Info For Java  Java配置信息）:

​		jinfo命令格式：jinfo [option] pid

​		显示虚拟机配置信息。

​		

### 1.4、jmap

​	（Memory Map for Java  Java内存映像工具）:

​		jinfo命令格式：jinfo [option] VMID

​		用于生成对转储快照（heapdump或dump文件），还可以查询finalize执行队列、Java堆和永久代的详细信息。



### 1.5、jhat

​	（JVM Heap Analysis Tool 虚拟机堆转储快照分析工具）:

​		jinfo命令格式：jinfo [option] pid



### 1.6、jstack

​	（Stack Trace for Java  Java堆栈跟踪工具）

命令格式：jstack [option] VMID

​		生成虚拟机当前时刻的线程快照。

线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈集合，生成线程快照的主要目的是定位线程出现长时间停顿的原因。

| options |                     作用                     |
| :-----: | :------------------------------------------: |
|   -F    | 当正常输出的请求不被响应时，强制输出线程堆栈 |
|   -l    |        除堆栈外。显示关于锁的附加信息        |
|   -m    | 如果调用到本地方法的话，可以显示C/C++的堆栈  |

​	

# 二、JDK各个版本区别

### jdk1.5的新特性

```java
/*
* jdk1.5新特性
* 泛型、自动装箱/拆箱、foreach、import static、变长参数
*/
//1.5.1泛型
ArrayList oldArrayList = new ArrayList(); //原来
ArrayList<Integer> newArrayList = new ArrayList<Integer>(); //新特性

//1.5.2自动装箱/拆箱
int oldInt = oldArrayList.get(0).parseInt(); //原来
int newInt = newArrayList.get(0); //新特性

//1.5.3for-each
for(int i=0;i<oldArrayList.size();i++){} //原来
for (Integer integer : newArrayList) {} //新特性

//1.5.4静态导入 (导包格式改变)
import java.lang.Math;  //原来
import static java.lang.Math.sqrt; //新特性
Math.sqrt(4); //原来
sqrt(4);    //新特性

//1.5.5变长参数
int[] intList = {1,2,3,4};
int result = JDKDifference.sum(intList);
//1.5.5变长参数
public static int sum(int ...intList){
    int result = 0;
    for(int i=0;i<intList.length;i++){
        result = result + intList[i];
    }
    return result;
}
```



### jdk1.6的新特性

```java
/* 
* jdk1.6新特性 
*/
//1.6.1 java.awt.Desktop类和java.awt.SystemTray类
//Desktop类 类中注释如下
/*<li>launching the user-default browser to show a specified URI;</li> 
  启动用户默认的浏览器显示指定的URI链接
 *<li>launching the user-default mail client with an optional {@code mailto} URI;</li>
 启动用户默认的邮件客户端发送URI指定的邮件
 *<li>launching a registered application to open, edit or print a specified file.</li>
 启动一个注册应用程序（本地安装了的应用程序）去打开，编辑或打印一个指定的文件
 */
//1.6.2新增Desktop类和SystemTray类
//使用默认浏览器打开指定的URI
browse();
public static void browse(){
    Desktop desktop;
    if(Desktop.isDesktopSupported()){
        desktop = Desktop.getDesktop();
        try {
            desktop.browse(new URI("http://www.baidu.com"));
        } catch (IOException e) {
            e.printStackTrace();
        } catch (URISyntaxException e) {
            e.printStackTrace();
        }
    }
}
//编辑文件
https://www.cnblogs.com/android-blogs/p/5545736.html
https://www.cnblogs.com/langtianya/p/3757993.html
https://blog.csdn.net/lxz352907839/article/details/81134155
https://www.cnblogs.com/huasky/p/7649460.html

//SystemTray类可以用来在系统托盘区创建一个托盘程序



//1.6.增强for循环
int[] ids = findIds();for(int i = 0; i < ids.length; i++) {} //原来
for(int i:findIds()){} //新特性
//1.6.1增强for循环
public static int[] findIds(){
    int[] ids = {1,2,3,4};
    return ids;
}


```

2、使用JAXB2来实现对象与XML之间的映射

3、STAX

4、使用Compiler API

5、轻量级Http Server API

6、插入式注解处理API

7、用Console开发控制台程序

8、对脚本语言的支持

9、Common Annotations 

