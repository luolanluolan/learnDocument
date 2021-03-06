# 一、Java内存区域

## 1、运行时数据区域

   ![image-20191128144744377](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191128144744377.png)

### 1.1、程序计数器

​		程序计数器是一块较小的内存空间，它可以看作是当前线程所执行的字节码的行号指示器。字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令。分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。

 		每个线程都需要一个独立的程序计数器。因为Java虚拟机的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现的，任何一个确定的时刻，一个处理器都只会执行一条线程中的指令，为了线程切换后能恢复到正确的执行位置，每条线程都需要一个独立的程序计数器。各条线程之间计数器互不影响，独立存储，是**线程私有的**内存。

​		如果线程正在执行Java方法，则计数器记录的是正在执行的虚拟机字节码指令的地址；如果线程正在执行native方法，则计数器的值为空（Undefined)。此内存区域是唯一一个在Java虚拟机规范中没有规定任何OutOfMemoryError情况的区域。

```tex
总结：1.字节码解释器通过改变计数器的值读取指令，从而实现流程控制，如顺        序执行、循环、跳转、异常处理等。
	 2.多线程情况下，程序计数器用于记录当前线程执行的位置，从而当线程        被切换回来的时候能够知道该线程上次运行到哪儿。
```



### 1.2、Java虚拟机栈

​		Java虚拟机栈也是**线程私有的**，它的生命周期和线程相同，虚拟机栈描述的是Java方法执行的内存模型：每个方法在执行的同时都会创建一个栈帧用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机中入栈到出栈的过程。

​		Java内存粗糙的区分为堆内存和栈内存，其中栈就是现在说的虚拟机栈，或者说的是虚拟机栈中局部变量表部分。

​		局部变量表主要存放了编译期可知的各种基本数据类型（boolean、byte、char、short、int、float、long、double)、对象引用（reference类型，它不等同于对象本身，，可能是一个指向对象起始地址的引用指针，也可能是指向一个代表对象的句柄或其他与此对象相关的位置）和returnAddress类型(指向了一条字节码指令的地址)。

​		局部变量表所需的内存空间在编译期间完成分配，当进入一个方法时，这个方法需要在帧中分配多大的局部变量空间是完全确定的，在方法运行期间不会改变局部变量表的大小。

​		Java虚拟机栈会出现两种异常：

1. StackOverflowError异常：线程请求的栈深度大于虚拟机所允许的深度。

   				2. OutOfMemoryError异常：虚拟机动态扩展，如果扩展时无法申请到足够的内存，就会抛出OutOfMemoryError异常

![image-20191128173008727](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191128173008727.png)

```tex
总结：1.每次方法调用的数据通过栈传递的（每个方法在执行的同时创建一个栈        帧，方法调用到执行完成，对应着一个栈帧在虚拟机栈中入栈到出栈的        过程。栈帧存储了局部变量表、操作数、动态链接、方法出口等）。
     2.局部变量表主要存放了编译器可知的各种数据类型、对象引用。
     3.Java虚拟机栈会出现两种异常。
     4.Java虚拟机栈线程私有的，每个线程都有各自的Java虚拟机栈，而且        随着线程的创建而创建，随着线程的死亡而死亡。
     5.虚拟机栈为执行Java方法服务 
```



### 1.3、本地方法栈

​		

```
 本地方法栈也是**线程私有的**，本地方法栈**为虚拟机使用到的Native方法服务**。在HotSpot虚拟机中把虚拟机栈和本地方法栈合二为一。

​ 本地方法被执行的时候，在本地方法栈也会创建一个栈帧，用于存放该本地方法的局部变量表、操作数栈、动态链接、出口信息。

​ 本地方法栈会出现两种异常：StackOverFlowError和OutOfMemoryError。
```



### 1.4、堆

​		

```tex
 JVM所管理的内存中最大的一块，Java堆是线程共享的区域，虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例和数组。
​ Java堆是垃圾收集器管理的主要区域，被称为GC堆。从垃圾回收的角度，现在收集器基本都是采用分代收集算法，所以Java堆还可以细分为：新生代(eden、s0、s1)和老年代(tentired);再细分：Eden空间、From Survivor、To Survivor空间等。进一步划分的目的是更好地回收内存，或者更快地分配内存。
​ 堆中没有内存完成实例分配，并且堆也无法再扩展时，将抛出OutOfMemoryError异常。
```

![image-20191129150702878](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191129150702878.png)

更改：s0和S1是两块大小一样的内存区域，两块是因为回收算法是复制回收算法，新生代回收后存活的要么在S0要么在S1，其中一个，当再经过垃圾回收，存活的复制到另一块

### 1.5、方法区

```
方法区与Java堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

当方法区无法满足内存分配需求时，将抛出OutOfMemoryError异常。
```

JDK1.8的时候方法区（HotSpot的永久代）被彻底移除了，取而代之是元空间，元空间使用的是直接内存。

将永久代替换为元空间：可使用-XX:MaxMetaspaceSize设置最大元空间大小，默认值为unlimited，-XX：MetaspaceSize调整标志定义元空间的初始化大小。

![image-20191129164912488](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191129164912488.png)

​										Jdk1.8之前（上图）和之后（下图）

![image-20191202091258245](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191202091258245.png)



### 1.6、运行时常量池

​		方法区的一部分。Class文件中除了有类的版本、字段、方法、接口等描述信息外，还有常量池信息，**用于存放编译期生成的各种字面量和符号引用**。

​		运行时常量池是方法区的一部分，受到内存限制，当**常量池无法再申请到内存时会抛出OutOfMemoryError异常。**

​		**JDK1.7及之后版本**的JVM将运行时常量池从方法区中移出来了，，在**Java堆**中开辟了一块区域**存放运行时常量池**。

​		![image-20191129171108136](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191129171108136.png)



### 1.7、直接内存

​		并不是虚拟机运行时数据区的一部分，这部分内存也被频繁使用，也可能导致OutOfMemoryError异常。





### 视频学习：

![image-20191210101848900](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191210101848900.png)



堆分配参数：

-XX:+PrintGC 使用这个参数，虚拟机启动后，只要遇到GC就会打印日志

-XX:+UseSerialGC 配置串行回收器

-XX:+PrintGCDetails 可以查看详细信息，包括各个区的情况

-Xms: 设置java程序启动时初始堆大小

-Xmx:设置java程序能获得的最大堆大小

