# 一文带你弄懂mybatis中SqlSession、SqlSessionTemplate、SessionFactory和SqlSessionFactoryBean之间的关系



我潇洒的灰大狼又回来啦。今天送大家的一句话是：

> ***保持耐心，永远年轻，永远热泪盈眶。***

### 前言

先容我哭一会儿，呜呜呜~昨晚写了一半的文章，还没保存就盖上盖子准备回家，拔下电源准备把电脑塞进书包带回家完成时，懒惰阻止了我，最终还是没带回家，于是，遭报应了，今天早上来，电脑直接就是没电关机了，开机后写的文章**再也找不回来了**...（不争气的mac真是对不起我前面特地写了一篇文章来夸赞mac真香啊...）

Thread.sleep(5000)；//crying...

回归正题，开始含泪创作了。

很多初学者在学习mabatis或者看公司的mybatis项目时，总是搞不清楚SqlSessionSql、SqlSessionTemplate、SessionFactory和SqlSessionFactoryBean这几者之间的关系，尤其是我们在看别人的代码时，不同的项目都是不同的人完成的，风格迥异，有人用spring配置的形式，有人用springboot硬编码的形式，更有复杂点的项目，使用了读写分离等等，很容易让人懵圈。这篇文章的目的就是用通俗易懂的方式给大家理清这几者之间的关系。如若您发现文章某些部分难以理解，或者有错误，还望不惜笔墨，吾当虚心接受，感激涕零。如若您觉得有帮助，**点个赞支持下**，感谢感谢~

来，打起精神来。

### 正文

> #### SqlSession和SqlSessionTemplate

首先给大家带来的是SqlSession和SqlSessionTemplate之间的关系，**先看图，再看描述**。

<img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gaoysowja9j30bw0deaak.jpg" style="zoom:67%;" />

SqlSession实现了Closeable接口，代表SqlSession是可以关闭的，那也就是说SqlSession代表一种可关闭的连接。正如他的名字，session表示的是一个会话，用来维护无状态请求之间的状态信息，放在数据库这里，**SqlSession表示的是数据库客户端和数据库服务端之间的一种会话，并维护了两者之间的状态信息。**

<img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gaoz1shbcej30xa0u0q98.jpg" style="zoom:50%;" />

我们看到，**SqlSession是一个接口**，里面有我们熟悉的操作数据库执行sql语句的select、insert、update等方法，是不是很熟悉。

![](https://tva1.sinaimg.cn/large/006tNbRwly1gaoz5zbhd5j30vc054my8.jpg)

SqlSession有三个实现类，当然，你也可以自己实现。DefaultSqlSession是它的默认实现类，当然，还有我们熟悉的SqlSessionTemplate实现类。小伙伴们是不是已经打开代码开始看了，如果还没有，我**建议你打开源码**，跟着我的思路往下走。

<img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gaozbd0oemj30v406smyb.jpg"  />

<img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gap3i1magcj313a072ta8.jpg"  />

聪明的你是不是早就已经发现了，DefaultSqlSession和SqlSessionTemplate差别很大，但是咱们这次讨论的重点是SqlSessionTemplate，就不展开讲述他们之间的区别了，其实如果你能认真看到最后，你就自然就清楚区别了。

<img src="https://tva1.sinaimg.cn/large/006tNbRwly1gaozsa1fuwj30mu0i8wfi.jpg" style="zoom:50%;" />

值得一提的是，SqlSessionTemplate除了实现了Sqlsession接口之外，还实现了DisposableBean接口，这就意味着，SqlSessionTemplate的实例被Bean工厂发现后，会把他们纳入整个spring bean生命周期的管理过程之中，当BeanFactory尝试销毁时，Beans的管理者会以回调的方式调用SqlSessionTemplate的destroy()方法。

![](https://tva1.sinaimg.cn/large/006tNbRwly1gap00ily3zj30xi05y3z7.jpg)

默认实现是空方法，具体你可以自己重写。

总结一下，SqlSessionTemplate是SqlSession的实现类，如其名，是sqlSession模板，有了SqlSessionTemplate，我们就能用来执行Dao层的Sql语句。说了这么多，其实关键就一点，**SqlSessionTmplate是SqlSession的实现类，而这个实现类中有一个关键的类就是SqlSessionFactory。**

> #### SqlSessionFactory和SqlSessionFactoryBean

![](https://tva1.sinaimg.cn/large/006tNbRwgy1gap3les9m4j319o0kcgpl.jpg)



SqlSessionFactory也是一个接口，是SqlSession工厂，他的能力就是打开一个SqlSession会话，而且重载了许多不同的参数，你可以改变这些参数自定义会话过程中的一些默认行为。例如：可以设置自动提交事务或是关闭自动提交；可以设置获取数据库连接的线程的类型（重用，每次新产生等等）；也可以获取整个Mybatis的配置信息的Configuration对象实例等等。

![](https://tva1.sinaimg.cn/large/006tNbRwgy1gap0l4y1qtj310a03uwfa.jpg)

SqlSessionFactory默认也有两个实现类，当然你也可以**自定义实现类**。默认实现是DefaultSqlSessionFactory。

总而言之，SqlSessionFactory就是生产SqlSession对象的工厂。那也就是说整个Mybatis中，如果只有一个数据库Server要连接，那么只需要一个工厂就够了（只有一个SqlSessionFactory的实例对象），而SqlSession可以自由的被关闭，也就代表SqlSession是需要反复被创建的。上面说到SqlSession是关联到具体数据库连接的，但是如果每次创建和销毁都直接操作物理连接的话，那么这个资源浪费很高，效率很低。请看DefaultSqlSessionFactory的方法:

![](https://tva1.sinaimg.cn/large/006tNbRwgy1gap0pxi29tj31f60c0mzm.jpg)

上图是**基于数据库连接池实现**的，也就是说一次连接用完关闭SqlSession实例时，只是把数据库连接对象放回到对象池中，并没有直接销毁，**使用池技术，大大提高了物力资源利用率，缩短连接时间、减少了资源利用**等。

讲到这里，细心的小伙伴们可能有个疑问，SqlSessionFactory是怎么创建SqlSession的，或者更具体点，是怎么创建SqlSessionTemplate的，这就不得不说**动态代理**了。这部分是在SqlSessionTemplate中实现的，具体细节我下期再从源码角度给大家分享。

所剩不多了，再坚持坚持，坚持看完。

接下来要说的是SqlSessionFactoryBean，老规律，类图如下：

![](https://tva1.sinaimg.cn/large/006tNbRwly1gap3drb4qvj31620dy75h.jpg)



实现了ApplicationListener接口，代表SqlSessionFactoryBean有能力监控 Application发出的一些事件通知。

实现了FactoryBean接口，代表SqlSessionFactoryBean的实例不再是一个普通的bean对象，而是可以产生自己Bean的一个工厂，并且产生的Bean会被纳入spring的生命周期，这里产生的Bean指的就是SqlSessionFactory。

实现了InitializingBean接口，代表SqlSessionFactoryBean中的afterPropertiesSet()方法会在Bean初始化属性完成后立即被调用。

如其名，SqlSessionFactoryBean是生产SqlSessionFactory的工厂bean。

### 综上所述

SqlSessionFactoryBean是生产SqlSessionFactory的一种工厂bean。

SqlSessionFactory是打开SqlSession会话的工厂，是一个接口，可以根据需求自己实现，它的默认实现类DefaultSqlSessionFactory使用了数据库连接池技术。

SqlSession是客户端和数据库服务端之间的会话信息，里面有许多操作数据库的方法。

SqlSessionTemplate是SqlSession的一个具体实现。

### 写在最后

写到这里，不知道小伙伴们是否已经搞清楚他们之间的关系了呢。上面讲的其实比较浅显，主要是从结构上给大家梳理了下他们之间的关系，以及它们每一个的作用，但是这还远远不够，就像上面提到的，SqlSessionTemplate和SqlSession、SqlSessionFactory之间的纠缠到底是怎么样的？DataSource、Connection是怎么发挥作用的？以及我们的代码中是如何产生Sql语句并发送给数据库Server的？

爱学习的你们，是不是迫不及待想要一探究竟了，**关注我**，且听我灰大狼下期通过源码的方式给你们一一拆解。

***有帮助？点个赞再走呗~感谢各位老铁的支持~***