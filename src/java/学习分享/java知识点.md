## 会这些Java工程师面试知识点，月薪至少3W

> *本文已收录至公众号：**灰太狼学爪哇**。（一个java程序员都在关注的公众号）*

花了一下午撸了这份完整的java工程师知识（面试）清单，很负责任的说，掌握了这些知识点秒杀一切java面试，迎娶白富美走上人生巅峰是迟早的事。

整理这些知识点的目的是，第一，帮助自己梳理下我们需要掌握的知识点；第二，发现自己的不足，有针对性查缺补漏；**第三，明确了自己接下来公众号分享内容的方向。**

**接下来公众号的分享我会根据小伙伴的留言或者随机选择下面专题中的某个问题，采用模拟面试的方式，随着面试官的层层逼问，用近乎完美的答案吊打面试官，欢迎大家有所期待。**

# 基础

## javaSE

- 多态
- 接口和抽象类
- 序列化
- 集合
- 代理（动态代理、静态代理）
- 反射
- 设计模式
- IO

## javaWeb

- cookie和session
- servet知识
- tomcat知识

## JVM

- java类加载机制
- 双亲委派模型
- 常用的GC算法
- 常用垃圾收集器及其原理
- JVM内存模型
- 常见JVM内存分析工具及分析方法
- CUP到了100%怎么排查
- JDK1.7和JDK1.8区别
- JDK1.8新特性
- 常用JVM调优参数
- 判断对象是否存活算法
- 引用的四种级别
- finalize()方法
- 碰到过什么样的JVM线上问题，怎么排查的（最喜欢问的）

## 网络编程

- tcp和udp
- tcp的三次握手和四次挥手
- AIO、BIO和NIO
- 描述一个请求完整的执行过程（最常问）
- http和https

## 互联网工具

- git
- maven
- linux

# 并发专题

## JMM内存模型

- 线程和程序，并行和并发
- volatile
- 指令重排序，原子性，可见性

## 并发同步处理

- synchronized内置锁实现原理
- AQS原理
- CAS算法乐观锁 
- atomic原子类
- threadLocal类
- 各种锁（悲观锁、乐观锁、可重入锁、公平锁、非公平锁等）
- ReentrantLock/ReentrantReadWriteLock/ReentrantWriteLock详解
- Condition条件队列、同步队列

## JUC并发包

- CountDownlatch详解
- Semaphore详解
- CyclicBarrier详解
- 阻塞队列（ArrayBlockingQueue、CurrentLinkedQueue、DelayQueue、PriorityBlockingQueue）
- ForkJoin框架
- 线程池详解
- HashMap和ConcurrentHashMap详解
- ArrayList、LinkedList、CopyOnWriteArrayList详解
- Set、CopyOnWriteArraySet详解

# 分布式专题

## Redis缓存

- 什么是redis？
- 为什么要用redis？使用redis的好处
- 怎么用redis
- redis五种数据结构以及使用场景
- 使用redis会有哪些问题？以及怎么解决
- redis集群的搭建和原理
- redis五种数据结构底层实现是怎样的

## RocketMq（RabbitMq、kafka）消息队列

- RocketMq、RabbitMq和kafka三者有什么不同？优缺点以及使用场景
- RocketMq是什么？
- RocketMq使用场景？
- RocketMq消息发送原理
- RocketMq消息丢失怎么办？
- RocketMq集群同步机制
- RocketMq事务原理

## Zookeeper

- 什么是zookeeper？
- 为什么要使用zookeeper？（优缺点）
- zookeeper使用场景
- zookeeper原理

## Dubbo

- dubbo是什么？
- 为什么要使用dubbo？
- dubbo使用场景
- dubbo的rpc调用和http调用的区别，有什么优点
- dubbo原理
- 使用dubbo有没有碰到什么问题？怎么解决？（如：dubbo重试）
- dubbo怎么做到高可用
- dubbo注册中心的服务发现
- dubbo源码分析

## netty

- 为什么使用netty
- 内部实现原理
- netty我还没用过，后面再总结

## mysql分库分表

- 什么是分库分表
- 分库分表应用场景
- 有什么优缺点
- 分库分表规则（算法），为什么考虑设计这样的规则，有没有可替代的其他规则
- 如何做到从一个单库应用平滑过渡到分库分表的应用
- 如果做到分库分表的数据，按条件分页查询

# 框架源码专题

## spring源码

- spring IOC 容器设计原理
- spring的bean的生命周期
- spring Context的装载过程源码分析
- FactoryBean和BeanFactory区别
- AOP注解编程
- AOP源码分析
- spring事务控制与源码分析
- spring拦截器

## springMVC源码

- MVC设计思想
- springMVC原理（请求过程）
- springMVC组成的体系结构（映射器、执行器、视图解析器、异常捕捉等）
- springMVC中的常用注解

## springboot

- springboot原理（启动流程）
- springboot优缺点，使用它有什么好处
- springboot常用注解

## mybatis源码

- 使用mybatis解决了什么问题？有什么好处
- mybatis原理
- 初始化过程
- 一条sql语句执行过程
- Configuration、Mapper、SqlSession、Excutor源码分析
- mybatis的一级缓存和二级缓存
- 如果让你手写一套mybatis，你会怎么设计（面试被问过）

# 数据结构和算法

## 数据结构

- 数组
- 链表
- 跳跃表
- 堆栈
- 队列
- 各种树
- 矩阵

## 算法

- 常用排序算法以及时间、空间复杂度
- 《剑指offer》
- 《LeetCode》
- 常见top K问题

# 数据库专题

## mysql

- 数据库设计三范式
- 事务（特性和隔离级别）
- MVCC多版本并发控制
- 事务并发问题
- 事务日志（binlog、redo log、undo log）
- 索引数据结构类型
- 索引分类（聚簇索引和非聚簇索引）
- mysam和InnoDb区别
- 架构原理（一条SQL的执行过程）
- SQL调优
- mysql高可用（MMM、MHA、PXC）

## ElasticSearch

- es集群原理
- 一条查询请求的完整过程
- es的优缺点
- es应用场景
- 滚动查询
- es的倒排索引原理
- 怎么保证es和mysql的数据一致性

## MongoDB

- mongoDB的特性
- mongoDB的使用场景
- mongoDB的优缺点
- 使用过程中有没有碰到什么问题

# 项目

## 常用项目的设计

- 登录注册
- 单点登录
- 短链接

## 描述自己项目

- 你在这个项目中参与了哪些部分？
- 你是怎么设计的？
- 为什么这么设计？
- 有没有其他方案替代？
- 用到了什么技术？为什么要用这个技术？有没有可替代的？
- 开发过程中有没有碰到什么（没考虑到的）难点？你是怎么解决的
- 还能不能优化？
- 你有什么收获？
- 让你来做跟别人来做，你的优势在哪？（这个有点扯淡，但是有些面试官还是会问）



# 最后

有些人花了三五年就能掌握这些知识点，成功面上高薪架构师，

有些人花了十多年却只知道些皮毛，每天做着CRUD的工作。

**相信点赞关注我的都是第一种** ^_^

**我是灰太狼，一个95后自我精进的java程序员。个人运营的公众号：灰太狼学爪哇。你想知道的都在这里。（关注免费领取100G各阶段的学习、面试资料）**

![](https://tva1.sinaimg.cn/large/00831rSTgy1gcmgifa2yhj30b00b0tbf.jpg)



