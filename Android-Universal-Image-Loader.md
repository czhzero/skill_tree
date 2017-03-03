[Android 三大图片缓存原理、特性对比](http://www.trinea.cn/android/android-image-cache-compare/)

# Android-Universal-Image-Loader

优点:

- View滚动中暂停图片加载

通过 PauseOnScrollListener 接口里面的onScrollStateChanged方法和onScroll方法，可以在 View 滚动中暂停图片加载。

- 支持下载进度监听


- 多种内存缓存算法

ImageLoader 默认实现了较多缓存算法，如 Size 最大先删除、使用最少先删除、最近最少使用、先进先删除、时间最长先删除等。

- 支持本地缓存文件名规则定义


# Glide

(1) 图片缓存->媒体缓存
Glide 不仅是一个图片缓存，它支持 Gif、WebP、缩略图。甚至是 Video，所以更该当做一个媒体缓存。
 
(2) 支持优先级处理
 
(3) 与 Activity/Fragment 生命周期一致，支持 trimMemory
Glide 对每个 context 都保持一个 RequestManager，通过 FragmentTransaction 保持与 Activity/Fragment 生命周期一致，并且有对应的 trimMemory 接口实现可供调用。
 
(4) 支持 okhttp、Volley
Glide 默认通过 UrlConnection 获取数据，可以配合 okhttp 或是 Volley 使用。实际 ImageLoader、Picasso 也都支持 okhttp、Volley。
 
(5) 内存友好
① Glide 的内存缓存有个 active 的设计
从内存缓存中取数据时，不像一般的实现用 get，而是用 remove，再将这个缓存数据放到一个 value 为软引用的 activeResources map 中，并计数引用数，在图片加载完成后进行判断，如果引用计数为空则回收掉。
 
② 内存缓存更小图片
Glide 以 url、view_width、view_height、屏幕的分辨率等做为联合 key，将处理后的图片缓存在内存缓存中，而不是原始图片以节省大小
 
③ 与 Activity/Fragment 生命周期一致，支持 trimMemory
 
④ 图片默认使用默认 RGB_565 而不是 ARGB_888
虽然清晰度差些，但图片更小，也可配置到 ARGB_888。
 
其他：Glide 可以通过 signature 或不使用本地缓存支持 url 过期


# Picasso


(1) 自带统计监控功能
支持图片缓存使用的监控，包括缓存命中率、已使用内存大小、节省的流量等。
 
(2) 支持优先级处理
每次任务调度前会选择优先级高的任务，比如 App 页面中 Banner 的优先级高于 Icon 时就很适用。
 
(3) 支持延迟到图片尺寸计算完成加载
 
(4) 支持飞行模式、并发线程数根据网络类型而变
手机切换到飞行模式或网络类型变换时会自动调整线程池最大并发数，比如 wifi 最大并发为 4， 4g 为 3，3g 为 2。
这里 Picasso 根据网络类型来决定最大并发数，而不是 CPU 核数。
 
(5) “无”本地缓存
无”本地缓存，不是说没有本地缓存，而是 Picasso 自己没有实现，交给了 Square 的另外一个网络库 okhttp 去实现，这样的好处是可以通过请求 Response Header 中的 Cache-Control 及 Expired 控制图片的过期时间。


