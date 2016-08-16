# Markdown入门指南

Markdown 是一种「电子邮件」风格的「标记语言」，简便易用。 优点如下：

- 纯文本，所以兼容性极强，可以用所有文本编辑器打开。
- 让你专注于文字而不是排版。
- 格式转换方便，Markdown 的文本你可以轻松转换为 html、电子书等。
- Markdown 的标记语法有极好的可读性。

你也可以参考:

[为什么作家应该用 Markdown 保存自己的文稿](http://www.jianshu.com/p/qqGjLN)

[Markdown写作浅谈](http://www.jianshu.com/p/PpDNMG)


Mac环境下，推荐你使用MacDown编辑器，轻量级本地编辑器，您在左侧输入 Markdown 语法的文本，右侧会立即帮您呈现最终结果，好了，让我们开始学习吧~

## 标题

代码:

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

```

效果:

> # 一级标题
> ## 二级标题
> ### 三级标题
> #### 四级标题
> ##### 五级标题
> ###### 六级标题


代码:


```
一级标题
==
二级标题
--

```

效果:

> 一级标题
> ==
> 二级标题
> --

## 列表

### 无序列表

代码:

```
- 文本1
   * 二级文本1
- 文本2
	- 二级文本2
- 文本3
   + 二级文本3

```

效果:

> - 文本1
>   * 二级文本1
> - 文本2
>	- 二级文本2
> - 文本3
> 	+ 二级文本3


### 有序列表

代码:

```
1. 文本1
2. 文本2
3. 文本3

```


效果:

> 1. 文本1
> 2. 文本2
> 3. 文本3


## 链接

### 文本链接

代码:

```
[博客地址](http://www.czhzero.com)

```

效果:

> [博客地址](http://www.czhzero.com)

### 自动链接

代码:

```
<http://exmaple.com>

```

效果:

> <http://www.czhzero.com>


### 引用链接

代码:

```
    本文参考连接如下， [博客1][1] ， [博客][2] 。 

    [1]: http://www.czhzero.com "一座小楼" 
	 [2]: http://www.czhzero.com "一座小楼" 

```

效果:

本文参考连接如下， [博客1][1] ， [博客][2] 。 

[1]: http://www.czhzero.com "一座小楼" 
[2]: http://www.czhzero.com "一座小楼" 




### 图片链接

代码:

```
![alt text](图片链接地址)

```

效果:

> ![美女图片](http://7xvouf.com1.z0.glb.clouddn.com/girl2.jpg)
> 


## 代码

代码：

```
`single code line`

 ```
 code line1
 code line2
 code line3

 ```

```

效果:

> `single code line`

>  ``` 
>   code line1
>   code line2
>   code line3
>  ```

ps: 代码块内大部分语法标签没有作用

## 引用

代码:

```
> 引用文字 
> 
>> 二级引用文字
>
> 引用文字

```

效果:

> 引用文字 
> 
>> 二级引用文字
>
> 引用文字
> 

ps:引用的区块内也可以使用其他的 Markdown 语法


## 表格

代码:

```
| Tables        | Are            | Cool       |
| -----         | :-----:        | -----:     |
| col 3 is      | right-aligned  | $1600      |
| col 2 is      | centere        | $12        |
| zebra stipes  | are neat       | $1         |

```

```
dog | bird | cat
----|------|-----
foo | foo  | foo
bar | bar  | bar
baz | baz  | baz

```

效果:

| Tables        | Are            | Cool       |
| -----         | :-----:        | -----:     |
| col 3 is      | right-aligned  | $1600      |
| col 2 is      | centere        | $12        |
| zebra stipes  | are neat       | $1         |


dog | bird | cat
----|------|-----
foo | foo  | foo
bar | bar  | bar
baz | baz  | baz



## 字体

代码:

```
*斜体*
_斜体_

**粗体**
__粗体__

***粗斜体***
___粗斜体___

<font face="STCAIYUN">我是华文彩云</font>
<font color=gray size=5>灰色字体</font>
<font color=#0099ff size=5 face="黑体">蓝色黑体</font>

<font style="TEXT-DECORATION: line-through">删除线</FONT>
<font style="TEXT-DECORATION: overline">上划线</FONT>
<font style="TEXT-DECORATION: underline">下划线</FONT>
<font style="BACKGROUND-COLOR: #cccccc" color=#ff6600>给文字加上背景色</FONT>

```

效果:

> *斜体*
> 
> _斜体_
> 
> **粗体**
> 
> __粗体__
> 
> ***粗斜体***
> 
> ___粗斜体___
> 
> <font face="STCAIYUN">我是华文彩云</font>
>
> <font color=gray size=5>灰色字体</font>
>
> <font color=#0099ff size=5 face="黑体">蓝色黑体</font>
>
> <font style="TEXT-DECORATION: line-through">删除线</FONT>
>
> <font style="TEXT-DECORATION: overline">上划线</FONT>
>
> <font style="TEXT-DECORATION: underline">下划线</FONT>
> 
> <font style="BACKGROUND-COLOR: #cccccc" color=#ff6600>给文字加上背景色</FONT>


## 注脚



代码:

```
	这是一个注脚[^footnote1]的样例 
	[^footnote1]: 我就是那个注脚
```

效果:

这是一个注脚[^footnote1]的样例
[^footnote1]: 我就是那个注脚




