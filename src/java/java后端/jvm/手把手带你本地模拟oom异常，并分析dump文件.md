# 本地模拟oom异常，并分析java Dump文件

> *本文已收录至公众号：**灰太狼学爪哇**。（一个java程序员都在关注的公众号）*

### 前言

dump文件是java虚拟机内存在某一时间点的快照文件，一般是.hprof文件，下面自己模拟一下本地内存溢出，生成dump文件，然后通过mat工具分析的过程。

### 配置虚拟机参数

要想本地模拟oom异常，那么建议将堆内存设置的小一点，那样容易触发

```java
-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=${目录} -Xms20m -Xmx20m
```

1. -XX:+HeapDumpOnOutOfMemoryError表示jvm发生oom异常时，自动生成dump文件，文件格式一般是：java_pid20804.hprof ，其中20804是java进程id
2. -XX:HeapDumpPath=${目录}，表示生成dump文件的目录，也可以指定文件名称，例如：-XX:HeapDumpPath=${目录}/java_heapdump.hprof，**此步骤实验出了点问题，暂时跳过**
3. -Xms：表示给jvm分配的初始化堆内存
4. -Xmx：表示最大堆内存

### 模拟oom异常的程序

```java
public class Test1 {
    public static void main(String[] args) {
        List<Person> personList = new ArrayList<>();
        while (true){
            Person person = new Person();
            person.setDate(new Date());
            person.setAge(20);
            person.setName("test");
            personList.add(person);
        }
    }
}
```

很明显，一直创建Person对象，却又无法释放，很快就内存溢出了。

```java
java.lang.OutOfMemoryError: GC overhead limit exceeded
Dumping heap to java_pid21892.hprof ...
Heap dump file created [33907612 bytes in 0.152 secs]
Exception in thread "main" java.lang.OutOfMemoryError: GC overhead limit exceeded
	at Test1.main(Test1.java:15)
```

生成的文件名称是java_pid21892.hprof，在项目的根目录下面，文件大小33.9M

### MAT工具分析dump文件

#### 预览

预览界面，能清楚的看到整个堆总内存18.7M，但是有18.3M的内存被一个类的对象占用了

<img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gb5hehisqyj31s20u0128.jpg" style="zoom:50%;" />

#### 功能介绍

<img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gb5je1atkkj30km08ijtw.jpg" style="zoom:50%;" />



1. **Histogram柱状图**：以class类的维度展示每个class类的实例存在的个数、 占用的 [Shallow内存] 和 [Retained内存] 大小，可以分别排序显示，从图中看到，Person类的实例占用内存最大，而它就是没有回收引起内存溢出的对象

   <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gb5ji79octj30ty0cwtd6.jpg" style="zoom:50%;" />

   

2. **Dominator Tree支配树**：该视图以实例对象的维度展示当前堆内存中Retained Heap占用最大的对象，以及依赖这些对象存活的对象的树状结构

   ![](https://tva1.sinaimg.cn/large/006tNbRwgy1gb5jl1b5n5j31570u0k9f.jpg)

   展开会展示下一层子节点，可以这么理解：父节点引用了子节点，子节点没有被回收，所以父节点也没法被回收

3. **Thread Overview：**：可以看到线程栈/线程对象信息

   <img src="https://tva1.sinaimg.cn/large/006tNbRwly1gb5jspr4dwj318y0u0dyl.jpg" style="zoom:50%;" />

   把线程进行一个排序，同样能看到引起内存溢出的是最上面线程里面的Person对象

4. 可以选择展示什么样的报告信息，我们最开始打开dump文件时，会弹出一个窗口让我们选择，如果选错了这里可以重新选择

   <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gbmozjq9i4j313a0t21kx.jpg" style="zoom:25%;" />

   

5. 这里的功能比较强大，主要用来分析GC引用关系，每个对象到GCRoot的引用，可以在柱状图或者支配树界面里选择可疑对象进行分析，比较常用的两个是：Path to GC ROOTS和Merge Shortest Paths to GC Roots，当然选中对象双击，也能进入到这样的界面

   <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gb5jl1b5n5j31570u0k9f.jpg" style="zoom:50%;" />

6. Group分组功能：在 Histogram视图 和 Domiantor Tree视图时可操作，即以什么样的维度展示

   <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gbmoyz67a9j30me0cw7ez.jpg" style="zoom:25%;" />

7. 将分析报告以什么样的格式导出，可选的有html、csv、txt。便于在团队合作分析dump文件时，无需将庞大的dump文件拷贝过去，只需要生成以上格式的文件，就能很方便的转移。



### 总结

以上简单的模拟了一下oom异常发生的场景，在发生了oom异常后怎么生成dump文件，并介绍了怎么通过MAT工具分析dum文件的过程。

**我是灰太狼，一个95后自我精进的java程序员。个人运营的公众号：灰太狼学爪哇。你想知道的都在这里。（关注免费领取100G各阶段的学习、面试资料）**

![](https://tva1.sinaimg.cn/large/00831rSTgy1gcmgifa2yhj30b00b0tbf.jpg)