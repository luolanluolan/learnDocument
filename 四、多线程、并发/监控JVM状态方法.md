# 一、监控JVM参数

## 1、程序运行参数

```java
System.out.println("=================程序运行参数==============");
RuntimeMXBean runtimeMXBean = ManagementFactory.getRuntimeMXBean();
//JVM启动参数
System.out.println("JVM启动参数: "+runtimeMXBean.getInputArguments());
//系统属性
System.out.println("系统属性: "+runtimeMXBean.getSystemProperties());
//JVM名字
System.out.println("JVM名字: "+runtimeMXBean.getVmName());
```

## 2、线程状态

```java
System.out.println("=================线程状态==============");
ThreadMXBean threadMXBean = ManagementFactory.getThreadMXBean();
//获取当前JVM内的线程数量
System.out.println("当前JVM内的线程数量: "+threadMXBean.getThreadCount());
System.out.println("当前线程的总共cpu时间（纳秒）: "+threadMXBean.getCurrentThreadCpuTime());
System.out.println("当前线程在用户态运行的总共CPU时间（纳秒）: "+threadMXBean.getCurrentThreadUserTime());
System.out.println("活跃线程数: "+Thread.activeCount());
//找到所有存活状态的线程ID
long[] allThreadIds = threadMXBean.getAllThreadIds();
for (long id : allThreadIds) {    
    System.out.println("存活线程信息---："+threadMXBean.getThreadInfo(id));
}
ThreadInfo[] threadInfos = threadMXBean.dumpAllThreads(false, false);
for (ThreadInfo threadInfo : threadInfos) {    
    System.out.println("线程信息---："+threadInfo);
}
```

## 3、类加载情况

```java
System.out.println("=================类加载情况==============");
ClassLoadingMXBean classLoadingMXBean = ManagementFactory.getClassLoadingMXBean();
//获取当前JVM加载的类数量
System.out.println("当前JVM总加载的类数量: "+classLoadingMXBean.getLoadedClassCount());
//获取JVM总加载的类数量
System.out.println("JVM总加载的类数量: "+classLoadingMXBean.getTotalLoadedClassCount());
//获取JVM卸载的类数量
System.out.println("JVM卸载的类数量："+classLoadingMXBean.getUnloadedClassCount());
```

## 4、系统状态

```java
System.out.println("=================系统状态==============");
OperatingSystemMXBean operatingSystemMXBean = ManagementFactory.getOperatingSystemMXBean();
//获取服务器的CPU个数
System.out.println("服务器的CPU个数："+operatingSystemMXBean.getAvailableProcessors());
//获取服务器的平均负载，如果load过高，说明cpu无法及时处理任务 
System.out.println("服务器的平均负载："+operatingSystemMXBean.getSystemLoadAverage());
```

## 5、内存状态

```java
System.out.println("=================内存状态==============");
MemoryMXBean memoryMXBean = ManagementFactory.getMemoryMXBean();
//获取堆内存使用情况，包括初始大小，最大大小，已使用大小等，单位是字节
System.out.println("堆内存使用情况："+memoryMXBean.getHeapMemoryUsage().toString());
//获取堆外内存使用情况
System.out.println("堆外内存使用情况："+memoryMXBean.getNonHeapMemoryUsage());
```





# 二、启动主程序main方法：

开发工具中---创建线程（6个）

threadName-->Monitor Ctrl-Break
threadName-->Attach Listener
threadName-->Signal Dispatcher
threadName-->Finalizer
threadName-->Reference Handler
threadName-->main

活跃线程：

threadName-->Monitor Ctrl-Break

threadName-->main

![image-20191227103844240](C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20191227103844240.png)



命令窗口执行---创建线程（5个）

threadName-->Attach Listener
threadName-->Signal Dispatcher
threadName-->Finalizer
threadName-->Reference Handler
threadName-->main

活跃线程：

threadName-->main

