#Java学习笔记-多线程(一)

如果对什么是线程、什么是进程仍存有疑惑，请先Google之，因为这两个概念不在本文的范围之内。用多线程只有一个目的，那就是更好的利用cpu的资源。

先明确几个基本的概念:

- 多线程

指的是这个程序（一个进程）运行时产生了不止一个线程。

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

上述主要讲解了三种常见的线程创建方式，对于线程的启动而言，都是调用线程对象的start()方法，需要特别注意的是：**不能对同一线程对象两次调用start()方法**。


## 参考文献

- [Java中的多线程你只要看这一篇就够了](http://www.cnblogs.com/wxd0108/p/5479442.html)
- [Java总结篇系列：Java多线程（一）](http://www.cnblogs.com/lwbqqyumidi/p/3804883.html)
- [Java并发编程系列](http://blog.csdn.net/column/details/concurrency.html)