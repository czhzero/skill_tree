# Android性能优化


## 性能问题指标

- 响应时间

指从用户操作开始到系统给用户以正确反馈的时间。一般包括逻辑处理时间,网络传输时间,展现时间。对于非网络类应用不包括网络传输时间。展现时间即网页或App界面渲染时间。响应时间是用户对性能最直接的感受。

- TPS(Transaction Per Second)

TPS为每秒处理的事务数，是系统吞吐量的指标，在搜索系统中也用QPS(Query Per Second)衡量。TPS一般与响应时间反相关。通常所说的性能问题就是指响应时间过长、系统吞吐量过低。

- 系统内存使用

- 耗电量

## 性能调优方式总括

- 利用多线程并发或分布式，提高TPS
- 同步改异步，提高TPS
- 提前或延迟操作，错峰提高TPS
- 缓存(包括对象缓存、IO 缓存、网络缓存等)
- 数据结构和算法优化
- 性能更优的底层接口调用，如 JNI 实现
- 逻辑优化
- 需求优化
- 布局优化


## 缓存优化

### 线程池

- Android图片缓存
- Android图片Sdcard缓存
- 数据预取缓存

### 消息缓存

通过handler.obtainMessage复用之前的message，如下：

```
handler.sendMessage(handler.obtainMessage(0, object));

```

### ListView缓存

### 网络缓存

数据库缓存http response，根据http头信息中的Cache-Control域确定缓存过期时间。

### 文件IO缓存

使用具有缓存策略的输入流，BufferedInputStream替代InputStream，BufferedReader替代Reader，BufferedReader替代BufferedInputStream.对文件、网络IO皆适用。

### layout缓存

### 其他需要频繁访问或访问一次消耗较大的数据缓存


### 数据存储优化

- 数据类型选择

字符串拼接用StringBuilder代替String，在非并发情况下用StringBuilder代替StringBuffer。如果你对字符串的长度有大致了解，如100字符左右，可以直接new StringBuilder(128)指定初始大小，减少空间不够时的再次分配。
64位类型如long double的处理比32位如int慢
使用SoftReference、WeakReference相对正常的强应用来说更有利于系统垃圾回收
final类型存储在常量区中读取效率更高
LocalBroadcastManager代替普通BroadcastReceiver，效率和安全性都更高

- 数据结构选择

常见的数据结构选择如：
ArrayList和LinkedList的选择，ArrayList根据index取值更快，LinkedList更占内存、随机插入删除更快速、扩容效率更高。一般推荐ArrayList。
ArrayList、HashMap、LinkedHashMap、HashSet的选择，hash系列数据结构查询速度更优，ArrayList存储有序元素，HashMap为键值对数据结构，LinkedHashMap可以记住加入次序的hashMap，HashSet不允许重复元素。
HashMap、WeakHashMap选择，WeakHashMap中元素可在适当时候被系统垃圾回收器自动回收，所以适合在内存紧张型中使用。
Collections.synchronizedMap和ConcurrentHashMap的选择，ConcurrentHashMap为细分锁，锁粒度更小，并发性能更优。Collections.synchronizedMap为对象锁，自己添加函数进行锁控制更方便。
 
Android也提供了一些性能更优的数据类型，如SparseArray、SparseBooleanArray、SparseIntArray、Pair。
Sparse系列的数据结构是为key为int情况的特殊处理，采用二分查找及简单的数组存储，加上不需要泛型转换的开销，相对Map来说性能更优。不过我不太明白为啥默认的容量大小是10，是做过数据统计吗，还是说现在的内存优化不需要考虑这些东西，写16会死吗，还是建议大家根据自己可能的容量设置初始值。

## 算法优化

这个主题比较大，需要具体问题具体分析，尽量不用O(n*n)时间复杂度以上的算法，必要时候可用空间换时间。
查询考虑hash和二分，尽量不用递归。可以从结构之法 算法之道或微软、Google等面试题学习。

## JNI

## 逻辑优化

## 需求优化

这个就不说了，对于sb的需求可能带来的性能问题，只能说做为一个合格的程序员不能只是执行者，要学会说NO。不过不能拿这种接口敷衍产品经理哦

## 异步，利用多线程提高TPS

充分利用多核Cpu优势，利用线程解决密集型计算、IO、网络等操作。
关于多线程可参考：Java线程池
在Android应用程序中由于系统ANR的限制，将可能造成主线程超时操作放入另外的工作线程中。在工作线程中可以通过handler和主线程交互。

## 提前或延迟操作，错开时间段提高TPS

- 延迟操作

不在Activity、Service、BroadcastReceiver的生命周期等对响应时间敏感函数中执行耗时操作，可适当delay。
Java中延迟操作可使用ScheduledExecutorService，不推荐使用Timer.schedule;
Android中除了支持ScheduledExecutorService之外，还有一些delay操作，如
handler.postDelayed，handler.postAtTime，handler.sendMessageDelayed，View.postDelayed，AlarmManager定时等。

- 提前操作

对于第一次调用较耗时操作，可统一放到初始化中，将耗时提前。如得到壁纸wallpaperManager.getDrawable();


## 网络优化

以下是网络优化中一些客户端和服务器端需要尽量遵守的准则：
a. 图片必须缓存，最好根据机型做图片做图片适配
b. 所有http请求必须添加httptimeout
c. 开启gzip压缩
d. api接口数据以json格式返回，而不是xml或html
e. 根据http头信息中的Cache-Control及expires域确定是否缓存请求结果。
f. 确定网络请求的connection是否keep-alive
g. 减少网络请求次数，服务器端适当做请求合并。
h. 减少重定向次数
i. api接口服务器端响应时间不超过100ms
google正在做将移动端网页速度降至1秒的项目，关注中https://developers.google.com/speed/docs/insights/mobile

## 数据库优化