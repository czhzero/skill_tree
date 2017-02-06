# Android性能优化(一)－布局优化


## 性能问题指标

- 响应时间

指从用户操作开始到系统给用户以正确反馈的时间。一般包括逻辑处理时间,网络传输时间,展现时间。对于非网络类应用不包括网络传输时间。展现时间即网页或App界面渲染时间。响应时间是用户对性能最直接的感受。

- TPS(Transaction Per Second)

TPS为每秒处理的事务数，是系统吞吐量的指标，在搜索系统中也用QPS(Query Per Second)衡量。TPS一般与响应时间反相关。通常所说的性能问题就是指响应时间过长、系统吞吐量过低。

- 系统内存占用

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


## 布局优化

### 抽象布局标签

- <include>标签

include标签常用于将布局中的公共部分提取出来供其他layout共用，以实现布局模块化，这在布局编写方便提供了大大的便利。
下面以在一个布局main.xml中用include引入另一个布局foot.xml为例。main.mxl代码如下：

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <TextView
        android:id="@+id/tv_text"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_marginBottom="@dimen/dp_80" />

    <include layout="@layout/foot.xml" />

</RelativeLayout>

```

其中include引入的foot.xml为公用的页面底部，代码如下：

[](http://blog.csdn.net/christopher_411524/article/details/50582740)