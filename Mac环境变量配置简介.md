# Mac环境变量配置简介

## Mac环境变量加载顺序

mac 一般使用bash作为默认shell , 使用bash一般配置~/.bash_profile文件，若使用zsh, 则配置~/.zshrc文件。

Mac系统的环境变量，先加载全局配置，再加载个人配置，加载顺序为：

```

/etc/profile 
/etc/paths 
/etc/bashrc 
~/.bash_profile 
~/.bash_login 
~/.profile
~/.bashrc

```

当然`/etc/profile` , `/etc/paths` `/etc/bashrc` 都是系统级别的，系统启动就会加载，后面几个是当前用户级的环境变量。

后面3个按照从前往后的顺序读取，如果`~/.bash_profile`文件存在，则后面的几个文件就会被忽略不读了，如果`~/.bash_profile`文件不存在，才会以此类推读取后面的文件。`~/.bashrc`没有上述规则，它是bash shell打开的时候载入的。

## Path变量设置方法

```

#定义一个变量路径
export ANDROID_HOME=/Users/chenzhaohua/Library/Android/sdk/
#中间用冒号隔开
export PATH=$PATH:$ANDROID_HOME/platform-tools:$ANDROID_HOME/tools

```


## 环境变量设置生效

一般环境变量更改后，重启后生效。
如果想立刻生效，则可执行下面的语句：

```
$ source 相应的文件
```

