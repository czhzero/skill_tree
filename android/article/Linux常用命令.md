# Linux入门命令

## Ubuntu桌面常用

```
gnome-system-monitor                    #打开任务管理器
Ctrl + Alt + L			        		#快速锁屏
Ctrl + H								#文件管理器下使用，查看隐藏文件
Alt + Enter								#文件管理器里选择文件/文件夹，选中后，查看属性。
Super + w                               #Super键就是空格左边的Win键，会启动“展览”切换模式
Alt + Tab				#当前桌面的不同程序之间切换
Ctrl + Alt + T			        #打开新的终端窗口
Ctrl + Supter + D                       #隐藏所有窗口
```



## 安装与卸载



### 1.apt方式

```
apt-cache search package  #搜索包

apt-cache show package    #获取包的相关信息，如说明、大小、版本等

sudo apt-get install package    #安装包

sudo apt-get install package --reinstall 重新安装包

sudo apt-get -f install 修复安装"-f = ——fix-missing"

sudo apt-get remove package 删除包

sudo apt-get remove package - - purge 删除包，包括删除配置文件等

sudo apt-get update 更新源

sudo apt-get upgrade 更新已安装的包

sudo apt-get dist-upgrade 升级系统

sudo apt-get dselect-upgrade 使用 dselect 升级

apt-cache depends package 了解使用依赖

apt-cache rdepends package 是查看该包被哪些包依赖

sudo apt-get build-dep package 安装相关的编译环境

apt-get source package 下载该包的源代码

sudo apt-get clean && sudo apt-get autoclean 清理无用的包

sudo apt-get check 检查是否有损坏的依赖
```



### 2.Dpkg方式



```
dpkg -i package_name.deb    #普通安装
dpkg -r pkg1 pkg2           #移除式卸载
dpkg -P pkg1 pkg2           #清除式卸载
```



### 3.源码安装（tar, tar.gz, tar.bz2）

```
首先解压缩源码压缩包然后通过tar命令来完成

a．解xx.tar.gz：tar zxf xx.tar.gz 
b．解xx.tar.Z：tar zxf xx.tar.Z 
c．解xx.tgz：tar zxf xx.tgz 
d．解xx.bz2：bunzip2 xx.bz2 
e．解xx.tar：tar xf xx.tar

然后进入到解压出的目录中，建议先读一下README之类的说明文件，因为此时不同源代码包或者预编译包可能存在差异，然后建议使用ls -F --color或者ls -F命令（实际上我的只需要 l 命令即可）查看一下可执行文件，可执行文件会以*号的尾部标志。

一般依次执行./configure
            make
            sudo make install

即可完成安装。
```

## 压缩与解压缩

````
    tar命令

    　　解包：tar zxvf FileName.tar

    　　打包：tar czvf FileName.tar DirName

    gz命令

    　　解压1：gunzip FileName.gz

    　　解压2：gzip -d FileName.gz

    　　压缩：gzip FileName

    　　.tar.gz 和 .tgz

    　　解压：tar zxvf FileName.tar.gz

    　　压缩：tar zcvf FileName.tar.gz DirName

       压缩多个文件：tar zcvf FileName.tar.gz DirName1 DirName2 DirName3 ...

    bz2命令

    　　解压1：bzip2 -d FileName.bz2

    　　解压2：bunzip2 FileName.bz2

    　　压缩： bzip2 -z FileName

    　　.tar.bz2

    　　解压：tar jxvf FileName.tar.bz2

    　　压缩：tar jcvf FileName.tar.bz2 DirName

    bz命令

    　　解压1：bzip2 -d FileName.bz

    　　解压2：bunzip2 FileName.bz

    　　压缩：未知

    　　.tar.bz

    　　解压：tar jxvf FileName.tar.bz

    Z命令

    　　解压：uncompress FileName.Z

    　　压缩：compress FileName

    　　.tar.Z

    　　解压：tar Zxvf FileName.tar.Z

    　　压缩：tar Zcvf FileName.tar.Z DirName

    zip命令

    　　解压：unzip FileName.zip

    　　压缩：zip FileName.zip DirName
````



## Terminal终端

```
CTRL + ALT + T: 打开终端
TAB: 自动补全命令或文件名
CTRL + SHIFT + V: 粘贴（Linux中不需要复制的动作，文本被选择就自动被复制）
CTRL + SHIFT + T: 新建标签页
CTRL + D: 关闭标签页
CTRL + L: 清除屏幕
CTRL + R + 文本: 在输入历史中搜索
CTRL + A: 移动到行首
CTRL + E: 移动到行末
CTRL + C: 终止当前任务
CTRL + Z: 把当前任务放到后台运行（相当于运行命令时后面加&）
~: 表示用户目录路径
```



## 用户与组



### 1.用户

linux用户的分类:

- 管理员 root  :具有使用系统所有权限的用户,其UID 为0.
- 普通用户  : 即一般用户,其使用系统的权限受限,其UID为500-60000之间.
- 系统用户 :保障系统运行的用户,一般不提供密码登录系统,其UID为1-499之间



与用户有关的文件/etc/passwd，/etc/shadow



```
/etc/passwd文件

其格式：account：password：UID:GID:GECOS:diretory:shell

        account: 用户名或帐号

        password ：用户密码占位符

        UID：用户的ID号

        GID：用户所在组的ID号

        GECOS:用户的详细信息（如姓名，年龄，电话等）

        diretory：用户所的家目录

        shell：用户所在的编程环境

```



```
/etc/shadow文件
   其格式：account：password：最近更改密码的日期：密码不可更该的天数：密码需要重新更改的天数：密码更改前的警告期限：密码过期的宽限时间：帐号失效日期：保留 
```



### 2.用户组

linux用户组分类：

- 普通用户组:可以加入多个用户
- 系统组:一般加入一些系统用户
- 私有组(也称基本组):当创建用户时,如果没有为其指明所属组,则就为其定义一个私有的用户组,起名称与用户名同名.

注: 私有组可以变成普通用户组,当把其他用户加入到该组中,则其就变成了普通组

组是权限的容器，如普通用户 a,b,c 所属组grp,则它们会继承组grp的权限



与组有关的文件:/etc/group，/etc/gshadow

```
    /etc/group文件： 其格式:group_name:passwoerd:GID:user_list

     group_name:组名

     passwoerd:组密码

     GID:组的ID号

     user_list：以group_name为附加组的用户列表
```

### 3.修改用户及用户组的命令

```
groups 查看当前登录用户的组内成员
groups gliethttp 查看gliethttp用户所在的组,以及组内成员
whoami 查看当前登录用户名
```

```
增加用户 ：useradd [options] username

             options：

                   1．-u ：UID

                   2．-g ：GID

                   3．-d ：指定用户家目录，默认是/home/username

                   4．-s ：指定用户所在的shell环境

                   5．-G：指定用户的附加组

           例如增加一用户wendy UID为1888 家目录/home/oracle，shell为/bin/sh

           #useradd –u 1888 –d /home/oracle –s /bin/sh wendy
```



```
修改用户：usermod  [options] username

            options：

                   1．-u ：UID

                   2．-g ：GID

                   3．-d ：指定用户家目录，默认是/home/username

                         -m 与-b 一起用表示把用户家目录的内容也移走

                   4．-s ：指定用户所在的shell环境

                   5．-G：指定用户的附加组

 

             例如修改用户wendy UID为1000 家目录/oracle，shell为/bin/bash

             #usermod –u 1000 –d  /oracle –s /bin/bash -m wendy
```



```
增加用户组：groupadd   [options] groupname

            options

                   1．-g ：GID

             例如增加用户组grp UID为1001

              #groupadd –g 1001 grp
```



```
删除用户：userdel   [options]username

            options

                   1．-r ：连同家目录一起删除

             例如删除用户wendy及家目录

              #userdel –r wendy


    
```

 

## 文件权限



每个Linux文件具有四种访问权限：可读(r)、可写(w)、可执行(x)和无权限(-)。
利用ls -l命令可以看到某个文件或目录的权限，它以显示数据的第一个字段为准。第一个字段由10个字符组成，如下：

> ```
> -rwxr-xr-x
> ```

    ```

第一位表示文件类型，-表示文件，d表示目录
2-4位：　　表示文件所有者的权限，u权限
5-7位：　　表示文件所有者所属组成员的权限，g权限
8-10位：　　表示所有者所属组之外的用户的权限，o权限   
2-10位：　　的权限总和有时称为a权限

以上例子中，表示这是一个文件(非目录），文件所有者具有读、写和执行的权限，所有者所属组成员和所属组之外的用户具有读和执行的权限而没有写的权限。
    ```





## 文件权限修改

- 用数字表示法修改权限

所谓数字表示法，是指将r、w和x分别用4、2、1来代表，没有授予权限的则为0，然后把权限相加，如下：

| 原始权限      | 转换为数字           | 数字表示法 |
| --------- | --------------- | ----- |
| rwxrwxr-x | (421)(421)(401) | 775   |
| rwxr-xr-x | (421)(401)(401) | 755   |

　　修改权限的例子：将文件test的权限修改为所有者和组成员具有读写的权限，其他人只有读权限

```
chmod 664 test
```



- 用文本表示法修改权限

```
文本表示法用4个字母表示不同的用户：
  - u：所有者
  - g：组成员
  - o：其他成员
  - a：所有人
　　权限仍用r、w和x表示
```



和数字表示法不同，文本表示法不仅可以重新指定权限，也可以在原来权限的基础上增加或减少权限，如下：

```
=：重新制定权限
-：对目前的设置减少权限
+：对目前的设置增加权限
```

例子：讲上述例子中，所有者加上执行权限，组成员减少执行权限，其他成员设置为执行权限，执行以下命令：

```
chmod u+x,g-x,o=x test
#注意：逗号前后不能有空格
```



### 目录权限



目录权限的修改和文件权限修改不同，只是四种权限代表的含义如下：

- r：可列出目录中的内容
- w：可在目录中创建、删除和修改文件
- x：可以使用cd命令切换到此目录
- -：没有任何此目录的访问权限

注意：目录可以使用通配符"*"来表示目录中的所有文件，如将/test目录中的所有文件的权限设置为任何人都可以读写

```
chmod 666 /test/*
```

### 指定文件的默认权限掩码-----umask

权限掩码有4个八进制的数字组成，讲现有的权限减掉权限掩码后，即可产生此文件建立时的默认权限。
一般来说，新建文件的默认值是0666，
新建目录的默认值是0777，如果将全线掩码设置为0002，则每个新建文件的默认权限为0666-0002=0664，而目录的默认权限则为775。可
以直接输入umask命令来检查目前的默认权限掩码，或输入"umask 权限掩码"来指定默认权限掩码。
用umask的方式指定默认权限掩码，可以避免添加访问权限过大的文件或目录。



## 参考文献

* [浅谈linux用户与用户组的概念](http://linuxme.blog.51cto.com/1850814/347086/)
* [Linux文件权限](http://www.cnblogs.com/TsengYuen/archive/2012/05/16/2504084.html)
