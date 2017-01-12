# JVM-运行时数据区域

Java虚拟机（Java virtualmachine）实现了Java语言最重要的特征：即平台无关性。
平台无关性原理：编译后的 Java程序（.class文件）由 JVM执行。JVM屏蔽了与具体平台相关的信息，使程序可以在多种平台上不加修改地运行。Java虚拟机在执行字节码时，把字节码解释成具体平台上的机器指令执行。因此实现Java平台无关性。


![](http://o7y1sf21i.bkt.clouddn.com/blog/017/20160514120258073.png)



![](http://o7y1sf21i.bkt.clouddn.com/blog/017/20160819150110871.png)

[JVM——Java虚拟机架构](http://blog.csdn.net/seu_calvin/article/details/51404589)

[String的Intern方法详解](http://www.cnblogs.com/wxgblogs/p/5635099.html)