# 实例揭示notify()和notifyAll()的本质区别


notify()和notifyAll()都是Object对象用于通知处在等待该对象的线程的方法。两者的最大区别在于：

notifyAll使所有原来在该对象上等待被notify的线程统统退出wait的状态，变成等待该对象上的锁，一旦该对象被解锁，他们就会去竞争。

notify则文明得多他只是选择一个wait状态线程进行通知，并使它获得该对象上的锁，但不惊动其他同样在等待被该对象notify的线程们，当第一个线程运行完毕以后释放对象上的锁此时如果该对象没有再次使用notify语句，则即便该对象已经空闲，其他wait状态等待的线程由于没有得到该对象的通知，继续处在wait状态，直到这个对象发出一个notify或notifyAll，它们等待的是被notify或notifyAll，而不是锁。


下面是一个很好的例子：

```
public class TestJava {

    /**
     * 产品类
     */
    private static class Product {
    }


    /**
     * 产品工厂类
     */
    private static class ProductFactory extends Thread {

        List<Product> finishedProducts = new ArrayList<>();

        public ProductFactory(String name) {
            super(name);
        }

        public void run() {
            try {
                //3秒钟,增加一个产品
                while (true) {
                    Thread.sleep(5000);
                    produce();
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }


        public void produce() {

            Product p = new Product();

            //也就是说需要3秒钟才能新产生一个Product，这决定了一定要用notify而不是notifyAll
            //因为上面两行代码不是同步的，如果用notifyAll则所有线程都企图冲出wait状态
            //第一个线程得到了锁，并取走了Product（这个过程的时间小于3秒，新的Product还没有生成）
            //并且解开了锁，然后第二个线程获得锁(因为用了notifyAll其他线程不再等待notify语句
            //，而是等待finishedProducts上的锁，一旦锁放开了，他们就会竞争运行)，运行
            //finishedProducts.remove(0)，但是由于finishedProducts现在还是空的，
            //于是产生异常,这就是为什么下面的那一句不能用notifyAll而是要用notify

            synchronized (finishedProducts) {
                finishedProducts.add(p);
                System.out.println(getName() + " has produced new product");
                finishedProducts.notify(); //这里只能是notify而不能是notifyAll
            }
        }


        public Product consume() {

            synchronized (finishedProducts) {
                if (finishedProducts.size() == 0) {
                    try {
                        finishedProducts.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }

                return finishedProducts.remove(0);
            }
        }
    }

    private static class ProductUser extends Thread {

        private ProductFactory maker;

        public ProductUser(String name, ProductFactory maker) {
            super(name);
            this.maker = maker;
        }

        public void run() {
            while (true) {
                maker.consume();
                System.out.println(getName() + " consumed product");
            }
        }
    }


    public static void main(String args[]) {

        ProductFactory maker = new ProductFactory("Audi");
        maker.start();
        new ProductUser("Alvin", maker).start();
        new ProductUser("Jack", maker).start();
        new ProductUser("Rose", maker).start();

    }


}

```

运行日志如下:

```
Audi has produced new product
Alvin consumed product
Audi has produced new product
Jack consumed product
Audi has produced new product
Rose consumed product
Audi has produced new product
Alvin consumed product
Audi has produced new product
Jack consumed product
Audi has produced new product
Rose consumed product
Audi has produced new product
Alvin consumed product

```

若改为notifyAll, 就会吃出现数组越界异常

> java.lang.IndexOutOfBoundsException: Index: 0, Size: 0

