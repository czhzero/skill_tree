# Java多线程(一) - 多线程的入门和使用

多线程可以减轻系统性能方面的瓶颈，提高CPU的处理器的效率，在多线程中，通过优先级管理，可以使重要的程序优先操作，提高了任务管理的灵活性；另一方面，在多CPU系统中，可以把不同的线程在不同的CPU中执行，真正做到同时处理多任务。

先明确几个基本的概念:

- 进程

运行中的应用程序称为进程，拥有系统资源（cpu、内存）

- 线程

进程中的一段代码，一个进程中可以有多段代码。本身不拥有资源（共享所在进程的资源）。

线程主要特点是，

①、不能以一个文件名的方式独立存在在磁盘中。

②、不能单独执行，只有在进程启动后才可启动。

③、线程可以共享进程相同的内存（代码与数据）。

线程的主要用途是，

①、利用它可以完成重复性的工作（如实现动画、声音等的播放）。

②、从事一次性较费时的初始化工作（如网络连接、声音数据文件的加载）。

③、并发执行的运行效果（一个进程多个线程）以实现更复杂的功能。

- 多进程

在操作系统中能同时运行多个任务(程序)

- 多线程

指的是这个程序（一个进程）运行时产生了不止一个线程，有多个功能流同时执行。

- 并行

多个cpu实例或者多台机器同时执行一段处理逻辑，是真正的同时。

- 并发

通过cpu调度算法，让用户看上去同时执行，实际上从cpu操作层面不是真正的同时。并发往往在场景中有公用的资源，那么针对这个公用的资源往往产生瓶颈，我们会用TPS或者QPS来反应这个系统的处理能力。

- 线程安全

经常用来描绘一段代码。指在并发的情况之下，该代码经过多线程使用，线程的调度顺序不影响任何结果。这个时候使用多线程，我们只需要关注系统的内存，cpu是不是够用即可。反过来，线程不安全就意味着线程的调度顺序会影响最终结果。

- 同步

Java中的同步指的是通过人为的控制和调度，保证共享资源的多线程访问成为线程安全，来保证结果的准确。如上面的代码简单加入@synchronized关键字。在保证结果准确的同时，提高性能，才是优秀的程序。线程安全的优先级高于性能。

## 线程的状态

关于Java中线程的生命周期，首先看一下下面这张较为经典的图：


![](http://o7y1sf21i.bkt.clouddn.com/blog/019/232002051747387.jpg)


### 状态（New）

当线程对象对创建后，即进入了新建状态，如：Thread t = new MyThread()。

### 就绪状态（Runnable）

当调用线程对象的start()方法（t.start();），线程即进入就绪状态。处于就绪状态的线程，只是说明此线程已经做好了准备，随时等待CPU调度执行，并不是说执行了t.start()此线程立即就会执行。

### 运行状态（Running）

当CPU开始调度处于就绪状态的线程时，此时线程才得以真正执行，即进入到运行状态。注：就绪状态是进入到运行状态的唯一入口，也就是说，线程要想进入运行状态执行，首先必须处于就绪状态中。

### 阻塞状态（Blocked）

处于运行状态中的线程由于某种原因，暂时放弃对CPU的使用权，停止执行，此时进入阻塞状态，直到其进入到就绪状态，才 有机会再次被CPU调用以进入到运行状态。根据阻塞产生的原因不同，阻塞状态又可以分为三种：

- 等待阻塞

运行状态中的线程执行wait()方法，使本线程进入到等待阻塞状态；

- 同步阻塞 

线程在获取synchronized同步锁失败(因为锁被其它线程所占用)，它会进入同步阻塞状态；

- 其他阻塞 

通过调用线程的sleep()或join()或发出了I/O请求时，线程会进入到阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。

### 死亡状态（Dead）

线程执行完了或者因异常退出了run()方法，该线程结束生命周期。

### 线程状态转换

就绪状态转换为运行状态：当此线程得到处理器资源；

运行状态转换为就绪状态：当此线程主动调用yield()方法或在运行过程中失去处理器资源。

运行状态转换为死亡状态：当此线程线程执行体执行完毕或发生了异常。

此处需要特别注意的是：当调用线程的yield()方法时，线程从运行状态转换为就绪状态，但接下来CPU调度就绪状态中的哪个线程具有一定的随机性，因此，可能会出现A线程调用了yield()方法后，接下来CPU仍然调度了A线程的情况。

由于实际的业务需要，常常会遇到需要在特定时机终止某一线程的运行，使其进入到死亡状态。目前最通用的做法是设置一boolean型的变量，当条件满足时，使线程执行体快速执行完毕。



## 基本线程类

基本线程类指的是Thread类，Runnable接口，Callable接口，其中Thread类实现了Runnable接口。

### Thread

```
class MyThread extends Thread {
    @Override
    public void run() {
     	//线程执行体
    }
}

public class ThreadTest {
    public static void main(String[] args) {
		Thread myThread1 = new MyThread();
		//调用start()方法使得线程进入就绪状态,并不一定立即执行                                                                		myThread1.start();                             
	}
}

```

**关于中断**

它并不像stop方法那样会中断一个正在运行的线程。线程会不时地检测中断标识位，以判断线程是否应该被中断（中断标识值是否为true）。终端只会影响到wait状态、sleep状态和join状态。被打断的线程会抛出InterruptedException。
Thread.interrupted()检查当前线程是否发生中断，返回boolean
synchronized在获锁的过程中是不能被中断的。

中断是一个状态！interrupt()方法只是将这个状态置为true而已。所以说正常运行的程序不去检测状态，就不会终止，而wait等阻塞方法会去检查并抛出异常。如果在正常运行的程序中添加while(!Thread.interrupted()) ，则同样可以在中断后离开代码体

### Runnable

实现Runnable接口，并重写该接口的run()方法，该run()方法同样是线程执行体，创建Runnable实现类的实例，并以此实例作为Thread类的target来创建Thread对象，该Thread对象才是真正的线程对象。

```
class MyRunnable implements Runnable {
    @Override
    public void run() {
		//线程执行体
    }
}

public class ThreadTest {
    public static void main(String[] args) {
    	// 创建一个Runnable实现类的对象
 		Runnable myRunnable = new MyRunnable();
 		// 将myRunnable作为Thread target创建新的线程                 		Thread thread1 = new Thread(myRunnable); 
 		// 调用start()方法使得线程进入就绪状态
       thread1.start(); 
    }
}

```

Thread类本身也是实现了Runnable接口。若Thread类和Runnable类均实现了run方法，start之后，会优先执行Runnable里面的run方法，而不会走Thread里面的run方法。

我们看一下Thread类中对Runnable接口中run()方法的实现：

```
@Override
public void run() {
    if (target != null) {
        target.run(); //target即是传入Thread的Runnable对象
    }
}

```


在程序开发中只要是多线程肯定永远以实现Runnable接口为主，因为实现Runnable接口相比继承Thread类有如下好处：

- 避免点继承的局限，一个类可以继承多个接口。
- 适合于资源的共享

以卖票程序为例：

```
public static class MyThread extends Thread {
        private int ticket = 10;
        public void run() {
            while (true) {
                if (ticket > 0) {
                    System.out.println("卖票：ticket" + this.ticket--);
                } else {
                    System.out.println("票卖完了");
                    break;
                }
            }
        }
    }

    public static class MyRunnable implements Runnable {
        private int ticket = 10;
        public void run() {
            while (true) {
                if (ticket > 0) {
                    System.out.println("卖票：ticket" + this.ticket--);
                } else {
                    System.out.println("票卖完了");
                    break;
                }
            }
        }
    }


    public static void main(String args[]) {

        MyThread mt1=new MyThread();
        MyThread mt2=new MyThread();
        MyThread mt3=new MyThread();
        mt1.start();//每个线程都各卖了10张，共卖了30张票
        mt2.start();//但实际只有10张票，每个线程都卖自己的票
        mt3.start();//没有达到资源共享

        MyRunnable mr=new MyRunnable();
        new Thread(mr).start(); //三个线程共享了10张票
        new Thread(mr).start();
        new Thread(mr).start();

    }


```

### Callable

使用Callable和Future接口创建线程。具体是创建Callable接口的实现类，并实现clall()方法。并使用FutureTask类来包装Callable实现类的对象，且以此FutureTask对象作为Thread对象的target来创建线程。

```
public static class MyCallable implements Callable<Integer> {

        private int i = 0;

        // 与run()方法不同的是，call()方法具有返回值
        @Override
        public Integer call() {
            int sum = 0;
            for (; i < 100; i++) {
                System.out.println(Thread.currentThread().getName() + " " + i);
                sum += i;
            }
            return sum;
        }

    }

    public static void main(String[] args) {

        // 创建MyCallable对象
        Callable<Integer> myCallable = new MyCallable();
        //使用FutureTask来包装MyCallable对象
        FutureTask<Integer> ft = new FutureTask<>(myCallable);

        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + " " + i);
            if (i == 30) {
                Thread thread = new Thread(ft);   //FutureTask对象作为Thread对象的target创建新的线程
                thread.start();                   //线程进入到就绪状态
            }
        }

        System.out.println("主线程for循环执行完毕..");

        try {
            int sum = ft.get();            //取得新创建的新线程中的call()方法返回的结果
            System.out.println("sum = " + sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

    }

```

首先，我们发现，在实现Callable接口中，此时不再是run()方法了，而是call()方法，此call()方法作为线程执行体，同时还具有返回值！在创建新的线程时，是通过FutureTask来包装MyCallable对象，同时作为了Thread对象的target。那么看下FutureTask类的定义：

```
public class FutureTask<V> implements RunnableFuture<V> {
     
     //....
     
}
 
public interface RunnableFuture<V> extends Runnable, Future<V> {
     
     void run();
     
}

```

于是，我们发现FutureTask类实际上是同时实现了Runnable和Future接口，由此才使得其具有Future和Runnable双重特性。通过Runnable特性，可以作为Thread对象的target，而Future特性，使得其可以取得新创建线程中的call()方法的返回值。

执行下此程序，我们发现sum = 4950永远都是最后输出的。而“主线程for循环执行完毕..”则很可能是在子线程循环中间输出。由CPU的线程调度机制，我们知道，“主线程for循环执行完毕..”的输出时机是没有任何问题的，那么为什么sum =4950会永远最后输出呢？

原因在于通过ft.get()方法获取子线程call()方法的返回值时，当子线程此方法还未执行完毕，ft.get()方法会一直阻塞，直到call()方法执行完毕才能取到返回值。

future模式：并发模式的一种，可以有两种形式，即无阻塞和阻塞，分别是isDone和get。其中Future对象用来存放该线程的返回值以及状态

```
ExecutorService e = Executors.newFixedThreadPool(3);
 //submit方法有多重参数版本，及支持callable也能够支持runnable接口类型.
Future future = e.submit(new myCallable());
future.isDone() //return true,false 无阻塞
future.get() // return 返回值，阻塞直到该线程运行结束

```

上述主要讲解了三种常见的线程创建方式，对于线程的启动而言，都是调用线程对象的start()方法，需要特别注意的是：**不能对同一线程对象两次调用start()方法**。

## 高级线程控制类简介

Java1.5提供了一个非常高效实用的多线程包:java.util.concurrent, 提供了大量高级工具,可以帮助开发者编写高效、易维护、结构清晰的Java多线程程序。

### ThreadLocal类

用处：保存线程的独立变量。对一个线程类（继承自Thread)
当使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本。常用于用户登录控制，如记录session信息。

实现：每个Thread都持有一个TreadLocalMap类型的变量（该类是一个轻量级的Map，功能与map一样，区别是桶里放的是entry而不是entry的链表。功能还是一个map。）以本身为key，以目标为value。
主要方法是get()和set(T a)，set之后在map里维护一个threadLocal -> a，get时将a返回。ThreadLocal是一个特殊的容器。

### 原子类

如果使用atomic wrapper class如atomicInteger，或者使用自己保证原子的操作，则等同于synchronized

```
//返回值为boolean
AtomicInteger.compareAndSet(int expect,int update)

```
该方法可用于实现乐观锁，考虑文中最初提到的如下场景：a给b付款10元，a扣了10元，b要加10元。此时c给b2元，但是b的加十元代码约为：

```
if(b.value.compareAndSet(old, value)){
   return ;
}else{
   //try again
   // if that fails, rollback and log
}

```

AtomicReference

对于AtomicReference 来讲，也许对象会出现，属性丢失的情况，即oldObject == current，但是oldObject.getPropertyA != current.getPropertyA。
这时候，AtomicStampedReference就派上用场了。这也是一个很常用的思路，即加上版本号

### Lock类

lock: 在java.util.concurrent包内。共有三个实现：

```
ReentrantLock
ReentrantReadWriteLock.ReadLock
ReentrantReadWriteLock.WriteLock

```

主要目的是和synchronized一样， 两者都是为了解决同步问题，处理资源争端而产生的技术。功能类似但有一些区别。

lock更灵活，可以自由定义多把锁的枷锁解锁顺序（synchronized要按照先加的后解顺序）
提供多种加锁方案，lock 阻塞式, trylock 无阻塞式, lockInterruptily 可打断式， 还有trylock的带超时时间版本。
本质上和监视器锁（即synchronized是一样的）
能力越大，责任越大，必须控制好加锁和解锁，否则会导致灾难。
和Condition类的结合。

### 容器类

- BlockingQueue

阻塞队列。该类是java.util.concurrent包下的重要类，通过对Queue的学习可以得知，这个queue是单向队列，可以在队列头添加元素和在队尾删除或取出元素。类似于一个管　　道，特别适用于先进先出策略的一些应用场景。普通的queue接口主要实现有PriorityQueue（优先队列），有兴趣可以研究

- ConcurrentHashMap

高效的线程安全哈希map。请对比hashTable , concurrentHashMap, HashMap

### 管理类

- ThreadPoolExecutor

## 参考文献

- [Java中的多线程你只要看这一篇就够了](http://www.cnblogs.com/wxd0108/p/5479442.html)
- [Java总结篇系列：Java多线程（一）](http://www.cnblogs.com/lwbqqyumidi/p/3804883.html)
- [Java并发编程系列](http://blog.csdn.net/column/details/concurrency.html)

