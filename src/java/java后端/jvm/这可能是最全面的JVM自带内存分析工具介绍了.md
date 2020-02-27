# JVM常用内存分析工具详解

> *不积跬步，无以至千里；不积小流，无以成江海。——《荀子·劝学》*

## 前言

在进行java程序问题定位时，内存问题定位是很关键的一招。虽然现在成熟的JVM调优工具有很多，比如jconsole、大名鼎鼎的VisualVM，以及最常用的IBM的Memory Analyzer等等，但是在碰到线上问题的时候，这些工具却使用起来不是很方便，这个时候java自带的命令工具，就非常有用了。

Sun JDK监控和故障处理命令有`jps jstat jmap jhat jstack jinfo`下面做一一介绍。

### jps

JVM Process Status Tool，显示指定系统内所有的HotSpot虚拟机进程。

##### 命令格式

```java
jps [option] [hostid]
```

##### options参数

```java
-l : 输出主类全名或jar路径
-q : 只输出LVMID
-m : 输出JVM启动时传递给main()的参数
-v : 输出JVM启动时显示指定的JVM参数
```

其中 [option]和[hostid]可以不写。

##### 示例

```java
$ jps -l
57296
65941 org.jetbrains.kotlin.daemon.KotlinCompileDaemon
66296 org.apache.catalina.startup.Bootstrap
66312 sun.tools.jps.Jps
66219 org.jetbrains.jps.cmdline.Launcher
66301 org.jetbrains.jps.cmdline.Launcher
```

### jstat

jstat(JVM statistics Monitoring)是用于监视虚拟机运行时状态信息的命令，它可以显示出虚拟机进程中的类装载、内存、垃圾收集、JIT编译(Just In Time Compiler, 即时编译器)等运行数据。

##### 命令格式

```java
jstat [option] LVMID [interval] [count]
```

##### 参数

```java
[option] : 操作参数
LVMID : 本地虚拟机进程ID
[interval] : 连续输出的时间间隔
[count] : 连续输出的次数	
```

##### option参数总览

```java
-class 	class loader的行为统计
-compiler 	HotSpt JIT编译器行为统计
-gc		垃圾回收堆的行为统计
-gccapacity 	各个垃圾回收代容量(young,old,perm)和他们相应的空间统计
-gcutil		垃圾回收统计概述
-gccause		垃圾收集统计概述（同-gcutil），附加最近两次垃圾回收事件的原因
-gcnew		新生代行为统计
-gcnewcapacity		新生代与其相应的内存空间的统计
-gcold		年老代和永生代行为统计
-gcoldcapacity		年老代行为统计
-gcpermcapacity		 永生代行为统计
-printcompilation		HotSpot编译方法统计
```

### jmap

jmap(JVM Memory Map)命令用于生成heap dump文件，如果不使用这个命令，还阔以使用-XX:+HeapDumpOnOutOfMemoryError参数来让虚拟机出现OOM的时候·自动生成dump文件。 jmap不仅能生成dump文件，还阔以查询finalize执行队列、Java堆和永久代的详细信息，如当前使用率、当前使用的是哪种收集器等。

##### 命令格式

```java
jmap [option] pid
```

##### option参数

```java
dump : 生成堆转储快照
finalizerinfo : 显示在F-Queue队列等待Finalizer线程执行finalizer方法的对象
heap : 显示Java堆详细信息
histo : 显示堆中对象的统计信息
permstat : to print permanent generation statistics
F : 当-dump没有响应时，强制生成dump快照
```

##### 示例

```java
jmap -dump:live,format=b,file=<filename> pid 
```

这样就可以导出pid对应java进程的dump文件了。

### jhat

jhat(JVM Heap Analysis Tool)命令是与jmap搭配使用，用来分析jmap生成的dump，jhat内置了一个微型的HTTP/HTML服务器，生成dump的分析结果后，可以在浏览器中查看。在此要注意，一般不会直接在服务器上进行分析，因为jhat是一个耗时并且耗费硬件资源的过程，一般把服务器生成的dump文件复制到本地或其他机器上进行分析。

##### 命令格式

```java
jhat [dumpfile]
```

##### 参数

```html
-stack false|true 关闭对象分配调用栈跟踪(tracking object allocation call stack)。 如果分配位置信息在堆转储中不可用. 则必须将此标志设置为 false. 默认值为 true.>
-refs false|true 关闭对象引用跟踪(tracking of references to objects)。 默认值为 true. 默认情况下, 返回的指针是指向其他特定对象的对象,如反向链接或输入引用(referrers or incoming references), 会统计/计算堆中的所有对象。>
-port port-number 设置 jhat HTTP server 的端口号. 默认值 7000.>
-exclude exclude-file 指定对象查询时需要排除的数据成员列表文件(a file that lists data members that should be excluded from the reachable objects query)。 例如, 如果文件列列出了 java.lang.String.value , 那么当从某个特定对象 Object o 计算可达的对象列表时, 引用路径涉及 java.lang.String.value 的都会被排除。>
-baseline exclude-file 指定一个基准堆转储(baseline heap dump)。 在两个 heap dumps 中有相同 object ID 的对象会被标记为不是新的(marked as not being new). 其他对象被标记为新的(new). 在比较两个不同的堆转储时很有用.>
-debug int 设置 debug 级别. 0 表示不输出调试信息。 值越大则表示输出更详细的 debug 信息.>
-version 启动后只显示版本信息就退出>
-J< flag > 因为 jhat 命令实际上会启动一个JVM来执行, 通过 -J 可以在启动JVM时传入一些启动参数. 例如, -J-Xmx512m 则指定运行 jhat 的Java虚拟机使用的最大堆内存为 512 MB. 如果需要使用多个JVM启动参数,则传入多个 -Jxxxxxx.
```

##### 示例

```java
$ jhat -J-Xmx512m dump.hprof
  eading from dump.hprof...
  Dump file created Fri Mar 11 17:13:42 CST 2016
  Snapshot read, resolving...
  Resolving 271678 objects...
  Chasing references, expect 54 dots......................................................
  Eliminating duplicate references......................................................
  Snapshot resolved.
  Started HTTP server on port 7000
  Server is ready.
```

中间的-J-Xmx512m是在dump快照很大的情况下分配512M内存去启动HTTP服务器，运行完之后就可在浏览器打开Http://localhost:7000进行快照分析 堆快照分析主要在最后面的Heap Histogram里，里面根据class列出了dump的时候所有存活对象。

分析同样一个dump快照，MAT需要的额外内存比jhat要小的多的多，所以建议使用MAT来进行分析。

### jstack

jstack用于生成java虚拟机当前时刻的线程快照。线程快照是当前java虚拟机内每一条线程正在执行的方法堆栈的集合，生成线程快照的主要目的是定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等。 线程出现停顿的时候通过jstack来查看各个线程的调用堆栈，就可以知道没有响应的线程到底在后台做什么事情，或者等待什么资源。 

##### 命令格式

```java
jstack [option] pid
```

##### 参数

```html
-F : 当正常输出请求不被响应时，强制输出线程堆栈
-l : 除堆栈外，显示关于锁的附加信息
-m : 如果调用到本地方法的话，可以显示C/C++的堆栈
```

执行完命令后，会打印出一堆堆栈信息，这里有一篇文章分析的比较好：[jstack线程栈分析](http://www.hollischuang.com/archives/110)

### jinfo

jinfo(JVM Configuration info)这个命令作用是实时查看和调整虚拟机运行参数。 之前的jps -v口令只能查看到显示指定的参数，如果想要查看未被显示指定的参数的值就要使用jinfo口令

##### 命令格式

```java
jinfo [option] [args] pid
```

##### 参数

```html
-flag : 输出指定args参数的值
-flags : 不需要args参数，输出所有JVM参数的值
-sysprops : 输出系统属性，等同于System.getProperties()
```

## 总结

本篇文章较为系统的总结了JVM自带的分析工具，但是要把这些工具命令全部使用或者是熟悉还是有难度的，只是选择一种比较熟悉的作为主要工具，其他工具可以作为定位问题的参考，另外说一句，其实jconsole还是比较好用，可以实时显示同时可以对内存进行dump操作。

本篇文章主要参考：http://www.ityouknow.com/jvm/2017/09/03/jvm-command.html