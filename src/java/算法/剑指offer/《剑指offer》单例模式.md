# 《剑指offer》系列--单例模式

> *永远年轻，永远热泪盈眶。*

### 题目

写一个单例模式

### 解析

单例模式有多种写法，以下整理了六种写法，你都学会了吗？

```java
public class Test01 {
    /**
     * 饿汉式
     */
    public static class Singleton1{
        private static final Singleton1 instance = new Singleton1();
        private Singleton1(){}
        public Singleton1 getInstance(){
            return instance;
        }
    }

    /**
     * 懒汉式--非线程安全的
     */
    public static class Singleton2{
        private static Singleton2 instance = null;
        private Singleton2(){}
        public Singleton2 getInstance(){
            if (instance == null){
                instance = new Singleton2();
            }
            return instance;
        }
    }

    /**
     * 懒汉式--线程安全的（效率低，每次都要上锁）
     */
    public static class Singleton3{
        private static Singleton3 instance = null;
        private Singleton3(){}
        public synchronized Singleton3 getInstance(){
            if (instance == null){
                instance = new Singleton3();
            }
            return instance;
        }
    }

    /**
     * 懒汉式--线程安全的（效率高，推荐使用）
     */
    public static class Singleton4{
        private static Singleton4 instance = null;
        private Singleton4(){}
        public static Singleton4 getInstance(){
            if (instance == null){
                synchronized (Singleton4.class){
                    if (instance == null){
                        instance = new Singleton4();
                    }
                }
            }
            return instance;
        }
    }

    /**
     * 静态内部类--线程安全（效率高，推荐）
     */
    public static class Singleton5{
        public static class SingletonHolder{
            private static final Singleton5 instance = new Singleton5();
        }
        private Singleton5(){}
        public static Singleton5 getInstance(){
            return SingletonHolder.instance;
        }
    }

    /**
     * 枚举方式
     */
    public enum Singleton6{
        INSTANCE;
        public void method(){}
    }
}

```

