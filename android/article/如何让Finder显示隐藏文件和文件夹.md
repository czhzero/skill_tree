# 如何让Finder显示隐藏文件和文件夹


## 显示

- 第一步 : 打开「终端」应用程序。


- 第二步 : 输入如下命令,

```
> defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder

```

- 第三步 : 按下「Return」键确认。


## 隐藏

- 第一步 : 打开「终端」应用程序。


- 第二步 : 输入如下命令,

```
> defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder

```

- 第三步 : 按下「Return」键确认。