# 1.看程序说结果

```java
public class VolatileThread extends Thread {

    // 定义成员变量
    private boolean flag = false ;
    public boolean isFlag() { return flag;}

    @Override
    public void run() {

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 将flag的值更改为true
        this.flag = true ;
        System.out.println("flag=" + flag);

    }
}
```

测试类：

```java
public class VolatileThreadDemo {
    
    public static void main(String[] args) {

        // 创建VolatileThread线程对象
        VolatileThread volatileThread = new VolatileThread() ;
        volatileThread.start();

        // main方法
        while(true) {
            if(volatileThread.isFlag()) {
                System.out.println("执行了======");
            }
        }
    }
}
```

结果：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200617091602.png)

我们看到，VolatileThread线程中已经将flag设置为true，但main()方法中始终没有读到，从而没有打印。

# 2.JMM

概述：JMM(Java Memory Model)Java内存模型,是java虚拟机规范中所定义的一种内存模型。

Java内存模型(Java Memory Model)描述了Java程序中各种变量(线程共享变量)的访问规则，以及在JVM中将变量存储到内存和从内存中读取变量这样的底层细节。

所有的共享变量都存储于主内存。这里所说的变量指的是实例变量和类变量。不包含局部变量，因为局部变量是线程私有的，因此不存在竞争问题。每一个线程还存在自己的工作内存，线程的工作内存，保留了被线程使用的变量的工作副本。线程对变量的所有的操作(读，取)都必须在工作内存中完成，而不能直接读写主内存中的变量，不同线程之间也不能直接访问对方工作内存中的变量，线程间变量的值的传递需要通过主内存完成。

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200617092043.png)

# 3. 问题分析

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200617092127.png)

1. VolatileThread线程从主内存读取到数据放入其对应的工作内存

2. 将flag的值更改为true，但是这个时候flag的值还没有写会主内存

3. 此时main方法读取到了flag的值为false

4. 当VolatileThread线程将flag的值写回去后，但是main函数里面的while(true)调用的是系统比较底层的代码，速度快，快到没有时间再去读取主存中的值，

   所以while(true)读取到的值一直是false。(如果有一个时刻main线程从主内存中读取到了主内存中flag的最新值，那么if语句就可以执行，main线程何时从主内存中读取最新的值，我们无法控制)

# 4.问题处理

## 加锁

```java
   // main方法
   while(true) {
       synchronized (volatileThread) {
           if(volatileThread.isFlag()) {
               System.out.println("执行了======");
           }
       }
   }
```

   某一个线程进入synchronized代码块前后，执行过程入如下：

   a.线程获得锁

   b.清空工作内存

   c.从主内存拷贝共享变量最新的值到工作内存成为副本

   d.执行代码

   e.将修改后的副本的值刷新回主内存中

   f.线程释放锁

## volatile关键字

   使用volatile关键字：

```java
   private volatile boolean flag ;
```

   工作原理：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200617092415.png)

1. VolatileThread线程从主内存读取到数据放入其对应的工作内存

2. 将flag的值更改为true，但是这个时候flag的值还没有写会主内存

3. 此时main方法main方法读取到了flag的值为false

4. 当VolatileThread线程将flag的值写回去后，失效其他线程对此变量副本

5. 再次对flag进行操作的时候线程会从主内存读取最新的值，放入到工作内存中

   

> 总结： volatile保证不同线程对共享变量操作的可见性，也就是说一个线程修改了volatile修饰的变量，当修改写回主内存时，另外一个线程立即看到最新的值。
>
> 但是volatile不保证原子性。

## volatile与synchronized

- volatile只能修饰实例变量和类变量，而synchronized可以修饰方法，以及代码块。

- volatile保证数据的可见性，但是不保证原子性(多线程进行写操作，不保证线程安全);而synchronized是一种排他（互斥）的机制，