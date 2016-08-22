# Android网络抓包-tcpdump+wireshark

本文主要介绍，如何利用tcpdump抓取手机上网络数据请求，并利用Wireshark清晰的查看到网络请求的各个过程数据。

请注意，使用tcpdump的前提是, 手机已root。推荐使用root工具进行root, 例如 [KingRoot](https://kingroot.net/) 。

## 命令行安装tcpdump

下载地址: [tcpdump](https://github.com/Trinea/TrineaDownload/blob/master/tcpdump?raw=true)

安装tcpdump，命令行模式依次执行：

```
adb root
adb push \your_computer_path\tcpdump /data/local/tcpdump
adb shell chmod 6755 /data/local/tcpdump

```
其中adb push的第一个参数为本地tcpdump的路径。

如果手机root后，却没有权限执行adb root命令的话，

请下载 [超级adbd.apk](http://soft.anruan.com/4752/) 开启权限即可。


## 命令行运行tcpdump

命令行模式运行下面命令：

```
adb shell /data/local/tcpdump -n -s 0

```

这时在手机上做任何涉及到网络的操作都会在屏幕上打印出来，可以通过ctrl+c停止。

由于命令行最大输出的限制及屏幕不断滚动，查看不方便，我们可以将抓取的网络包保存到sd卡，如下命令：

```
adb shell /data/local/tcpdump -i any -p -s 0 -w /sdcard/netCapture.pcap

```
依然通过ctrl+c停止，将文件拉取到本地PC

```
adb pull /sdcard/netCapture.pcap your_computer_path

```

通过–help我们发现tcpdump支持如下参数：

```

tcpdump [-aAdDeflLnNOpqRStuUvxX] [-c count] [ -C file_size ]
[ -E algo:secret ] [ -F file ] [ -i interface ] [ -M secret ]
[ -r file ] [ -s snaplen ] [ -T type ] [ -w file ]
[ -W filecount ] [ -y datalinktype ] [ -Z user ]
[ expression ]

```

其中-c表示监控的请求个数；-C表示存储文件的最大大小；
-i表示监控的类型；-s表示抓取的网络请求返回的大小，0表示抓取整个网络包；-w表示抓取的包保存的文件路径，此时不会在标准输出打印。并且可以添加port参数表示端口。

## 利用wireshark分析数据

下载地址: [wireshark](https://www.wireshark.org/download.html)

用wireshark打开capture.pcap即可分析log

关于wireshark具体可见：[Wireshark基本介绍和学习TCP三次握手](http://www.cnblogs.com/TankXiao/archive/2012/10/10/2711777.html)

