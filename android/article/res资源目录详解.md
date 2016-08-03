开源做为Android优点的同时也是它的缺点，不同品牌商不同的硬件配置、不同的屏幕大小和分辨率。都需要一一调试。有时候为了解决在某个特定手机上的一个bug，而大费周章已屡见不鲜。

Android项目目录里有一个目录叫做res目录，这个目录为我们进行多机型适配提供了便利。这个目录用的好，有时候可以省下一大段java代码。

<!-- more -->

# res目录结构
|目录|资源类型|
|:-----:|:-----|
|animator|用于定义属性动画的 XML 文件|
| anim |定义渐变动画的 XML 文件。（属性动画也可以保存在此目录中，但是为了区分这两种类型，属性动画首选 animator/ 目录。）|
| color |用于定义颜色状态列表的 XML 文件。|
| drawable | 位图文件（.png、.9.png、.jpg、.gif）或编译为以下 Drawable 资源子类型的 XML 文件：位图文件,九宫格（可调整大小的位图）,状态列表,形状,动画 Drawable,其他 Drawable。|
| mipmap |适用于不同启动器图标密度的 Drawable 文件。|
| layout |用于定义用户界面布局的 XML 文件。 |
| menu |用于定义应用菜单（如选项菜单、上下文菜单或子菜单）的 XML 文件。|
|raw|用于定义属性动画的 XML 文件|
|value|包含字符串、整型数和颜色等简单值的 XML 文件。其他res子目录中的 XML 资源文件是根据 XML 文件名定义单个资源，而目录中的values文件可描述多个资源。对于此目录中的文件，resources元素的每个子元素均定义一个资源。例如，string元素创建R.string 资源,color 元素创建 R.color 资源。由于每个资源均用其自己的 XML 元素定义，因此您可以根据自己的需要命名文件，并将不同的资源类型放在一个文件中。但是，为了清晰起见，您可能需要将独特的资源类型放在不同的文件中。 例如，对于可在此目录中创建的资源，下面给出了相应的文件名约定：(1) arrays.xml，用于资源数组（类型化数组）。(2) colors.xml：颜色值。(3) dimens.xml：尺寸值。(4) strings.xml：字符串值。(5)styles.xml：样式。|
|xml|可以在运行时通过调用 Resources.getXML() 读取的任意 XML 文件。各种 XML 配置文件（如可搜索配置）都必须保存在此处。|


> 注意：切勿将资源文件直接保存在 res/ 目录内，这会导致出现编译错误。

# 资源限定符

几乎每个应用都应提供备用资源以支持特定的设备配置。 例如，对于不同的屏幕密度和语言，您应分别包括备用 Drawable 资源和备用字符串资源。 在运行时，Android 会检测当前设备配置并为应用加载合适的资源。

Android 支持若干配置限定符，您可以通过使用短划线分隔每个限定符，向一个目录名称添加多个限定符。下面表按优先顺序列出了有效的配置限定符；如果对资源目录使用多个限定符，则必须按照表中列出的顺序将它们添加到目录名称。

|配置|限定符合值|
|:-----:|:-----|
|MCC 和 MNC|mcc310、mcc310-mnc004、mcc208-mnc00|
|语言和区域|en、fr、en-rUS、values-zh-rCN|
|布局方向| ldrtl 、ldltr |
| smallestWidth | sw320dp 、sw600dp |
| 可用宽度 | w720dp 、w1024dp |
| 可用高度 | h720dp 、h1024dp |
| 屏幕尺寸 | small 、normal、large、xlarge |
| 屏幕纵横比 | long、notlong |
| 屏幕方向 | port、land |
| UI 模式 | car、desk、television|
| 夜间模式 | night、notnight |
| 屏幕像素密度 (dpi) |ldpi、mdpi、hdpi、xhdpi、xxhdpi、xxxhdpi、nodpi、tvdpi|
| 触摸屏类型 | notouch、finger |
| 键盘可用性 | keysexposed、keyshidden、keyssoft |
| 主要文本输入法 | nokeys、qwerty、12key |
| 导航键可用性 | navexposed、navhidden|
| 主要非触摸导航方法 | nonav、dpad 、trackball 、wheel |
| 平台版本 | v3、v4 、v7 |



> 注1：要为应用启用从右到左的布局功能，必须将 supportsRtl 设
> 置为 "true"，并将 targetSdkVersion 设置为 17 或更高。


> 注2：有些配置限定符是从 Android 1.0 才开始添加，因此并非所有版本的 Android 系统都支持所有限定符。使用新限定符会隐式添加平台版本限定符，因此较旧版本系统的设备必然会忽略它。 例如，使用 w600dp 限定符会自动包括 v13 限定符，因为可用宽度限定符是 API 级别 13 中的新增配置。为了避免出现任何问题，请始终包含一组默认资源（一组“不带限定符”的资源）。 如需了解详细信息，请参阅利用资源提供最佳设备兼容性部分。
> 


# Android 如何找到最匹配资源

当您请求要为其提供备用资源的资源时，Android 会根据当前的设备配置选择要在运行时使用的备用资源。为演示 Android 如何选择备用资源，假设以下 Drawable 目录分别包含相同图像的不同版本：


```

drawable/
drawable-en/
drawable-fr-rCA/
drawable-en-port/
drawable-en-notouch-12key/
drawable-port-ldpi/
drawable-port-notouch-12key/


```

同时，假设设备配置如下：

```

区域设置 = en-GB 
屏幕方向 = port 
屏幕像素密度 = hdpi 
触摸屏类型 = notouch 
主要文本输入法 = 12key

```

通过将设备配置与可用的备用资源进行比较，Android 从 drawable-en-port 中选择 Drawable。

系统使用以下逻辑决定要使用的资源：

- 1.淘汰与设备配置冲突的资源文件。

```
drawable-fr-rCA/ 目录与 en-GB 区域设置冲突，因而被淘汰。
drawable/
drawable-en/
drawable-fr-rCA/     --> 淘汰
drawable-en-port/
drawable-en-notouch-12key/
drawable-port-ldpi/
drawable-port-notouch-12key/

```
> 例外：屏幕像素密度是唯一一个未因冲突而被淘汰的限定符。 尽管设备的屏幕密度为 hdpi，但是 drawable-port-ldpi/ 未被淘汰，因为此时每个屏幕密度均视为匹配。

- 2.选择列表中（下一个）优先级最高的限定符。（先从 MCC 开始，然后下移。）

- 3.是否有资源目录包括此限定符？

	- 若无，请返回到第 2 步，看看下一个限定符。（在该示例中，除非达到语言限定符，否则答案始终为“否”。
	- 若有，请继续执行第 4 步
- 4.淘汰不含此限定符的资源目录。在该示例中，系统会淘汰所有不含语言限定符的目录。

```
drawable/		--> 淘汰
drawable-en/
drawable-en-port/
drawable-en-notouch-12key/
drawable-port-ldpi/           --> 淘汰
drawable-port-notouch-12key/	--> 淘汰

```

> 例外：如果涉及的限定符是屏幕像素密度，则 Android 会选择最接近设备屏幕密度的选项。通常，Android 倾向于缩小大型原始图像，而不是放大小型原始图像。请参阅支持多个屏幕。

- 5.返回并重复第 2 步、第 3 步和第 4 步，直到只剩下一个目录为止。在此示例中，屏幕方向是下一个判断是否匹配的限定符。因此，未指定屏幕方向的资源被淘汰：

```
drawable-en/    --> 淘汰
drawable-en-port/
drawable-en-notouch-12key/  --> 淘汰

```
尽管对所请求的每个资源均执行此程序，但是系统仍会对某些方面做进一步优化。 例如，系统一旦知道设备配置，即会淘汰可能永远无法匹配的备用资源。 比如说，如果配置语言是英语（“en”），则系统绝不会将语言限定符设置为非英语的任何资源目录包含在选中的资源池中（不过，仍会将不带语言限定符的资源目录包含在该池中）。

根据屏幕尺寸限定符选择资源时，如果没有更好的匹配资源，则系统将使用专为小于当前屏幕的屏幕而设计的资源（例如，如有必要，大尺寸屏幕将使用标准尺寸的屏幕资源）。 但是，如果唯一可用的资源大于当前屏幕，则系统不会使用这些资源，并且如果没有其他资源与设备配置匹配，应用将会崩溃（例如，如果所有布局资源均用 xlarge 限定符标记，但设备是标准尺寸的屏幕）。

> 注：限定符的优先顺序（表 2 中）比与设备完全匹配的限定符数量更加重要。例如，在上面的第 4 步中，列表剩下的最后选项包括三个与设备完全匹配的限定符（方向、触摸屏类型和输入法），而 drawable-en 只有一个匹配参数（语言）。但是，语言的优先顺序高于其他两个限定符，因此 drawable-port-notouch-12key 被淘汰。
> 

更多关于资源限定符的描述，请参考[Android Developers](https://developer.android.com/guide/topics/resources/providing-resources.html)



