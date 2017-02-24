# Android-Universal-Image-Loader

优点:

- View滚动中暂停图片加载

通过 PauseOnScrollListener 接口里面的onScrollStateChanged方法和onScroll方法，可以在 View 滚动中暂停图片加载。

- 支持下载进度监听


- 多种内存缓存算法

ImageLoader 默认实现了较多缓存算法，如 Size 最大先删除、使用最少先删除、最近最少使用、先进先删除、时间最长先删除等。

- 支持本地缓存文件名规则定义