# Gradle for Andoid 学习笔记(二) : Groovy入门


## Groovy是什么

Groovy是一种基于JVM（Java虚拟机）的敏捷开发语言，它结合了Python、Ruby和Smalltalk的许多强大的特性，Groovy 代码能够与 Java 代码很好地结合，也能用于扩展现有代码。由于其运行在 JVM 上的特性，Groovy 可以使用其他 Java 语言编写的库。

![Gradle for Andoid](http://o7y1sf21i.bkt.clouddn.com/blog/011/20151021205159588.png)


下面四种方式都会输出“hello world”，

```

System.out.println("hello world");
System.out.println "hello world";

println("hello world")
println "hello world"

```

Groovy 语法与Java 语言的语法很相似，语法也比较灵活，例如上面一段代码，括号与分号，可有可无。

Groovy编写的文件扩展名是.groovy，学习Groovy官网是http://groovy-lang.org，当然了做Android开发的人员可以直接用Gradle构建工具来运行Groovy，这时候扩展名是.gradle。


## 基础语法

### 注释

与Java的注释语法相同，如：//注释 /* 注释 */ /** 注释 */ 都支持,唯一不同的是，Groovy支持像Shell脚本那样的首行附加注释

```
#!/usr/bin/env groovy

//注释1

/**
 * 注释2
 */

/* 注释3 */

def x=1

```

“#!”注释只允许在脚本文件的第一行出现，通过这种方式 Unix shell能定位groovy 的启动脚本并且运行这些脚本。


### 关键字

下面列表表示了Groovy语言所有的关键字, 代表意思与其他语言一致

```

as              assert           break               case
catch           class            const               continue
def             default          do                  else
enum            extends          false               finally
for             goto             if                  implements
import          in               instanceof          interface
new             null             package             return
super           switch           this                throw
throws          trait            true                try
while


```

### 标识符

#### 正常标识符

标识符以字母，美元符号或下划线开始。不能以一个数字开始。
一个字母可以在以下范围：

- ‘a’ 至 ‘z’ (小写ascii字母)
- ‘A’ 至 ‘Z’ (大写ascii字母)
- ‘\u00C0′ 至 ‘\u00D6′
- ‘\u00D8′ 至 ‘\u00F6′
- ‘\u00F8′ 至 ‘\u00FF’
- ‘\u0100′ 至 ‘\uFFFE’

后面的字符可以包括字母和数字。
这里是一些合法标识符的示例（这里是变量名称）：

```
def name
def item3
def with_underscore
def $dollarStart

```

但是以下面展示了一些非法的标识符：

```

def 3tier
def a+b
def a#b

```

当在一个点后，所有的关键字也是合法的标识符：


```
foo.as
foo.assert
foo.break
foo.case
foo.catch

```

#### 引用标识符

引用标识符出现在一个点式表达式的点后面。例如，person.name表达式中的name，能通过person.”name”,person.’name’被引用。当某些标识符包含有Java语言规范禁止的非法字符，这是相当有趣的，但被Groovy引用所允许。例如，像一个破折号，一个空格，一个感叹号等。

```
def map = [:]

map."an identifier with a space and double quotes" = "ALLOWED"
map.'with-dash-signs-and-single-quotes' = "ALLOWED"

assert map."an identifier with a space and double quotes" == "ALLOWED"
assert map.'with-dash-signs-and-single-quotes' == "ALLOWED"

```

正如我们将在字符串章节看到的，Groovy提供了不同的字符串字面量。所有不同类型的字符串都被允许出现在点后面：

```

map.'single quote'
map."double quote"
map.'''triple single quote'''
map."""triple double quote"""
map./slashy string/
map.$/dollar slashy string/$


```

普通字符串和Groovy的GStrings有一些不同（有插值的字符串），正如在后者的情况下，插入的值被插入到最后的字符串中，以计算整个标识符：

```
def firstname = "Homer"
map."Simson-${firstname}" = "Homer Simson"

assert map.'Simson-Homer' == "Homer Simson"

```


### 字符串

文本文字以字符链的形式表示被称作字符串。Groovy可以让你实例化java.lang.String对象，也可以实例化GString(groovy.lang.GString)，在其他编程语言中被称为插值字符串。


#### 单引号字符串

单引号字符串是一系列被单引号包围的字符。

单引号字符串是普通的java.lang.String，不支持插值。

```
'a single quoted string'

```


#### 字符串连接

所有Groovy字符串能使用+操作符连接：

```
assert 'ab' == 'a' + 'b'

```

#### 三单引号字符串

三单引号字符串是一列被三个单引号包围的字符：

```
'''a triple single quoted string'''

```

三单引号字符串是普通的java.lang.String，不支持插值。

三单引号字符串是多行的。你可以使字符串内容跨越行边界，不需要将字符串分割为一些片段，不需要连接，或换行转义符：

```
def aMultilineString = '''line one
line two
line three'''

```

如果你的代码是缩进的，如类中的方法体，字符串将包括缩进的空格。Groovy开发工具包含一些剥离缩进的方法，使用String#stripIndent()方法，并使用String#stripMargin()方法，需要一个分隔符来识别文本从一个字符串的开始删除。
当创建一个如下字符串：

```
def startingAndEndingWithANewline = '''
line one
line two
line three
'''

```

##### 转义特殊字符

你可以使用反斜杠字符转义单引号，避免终止字符串：

```
'an escaped single quote: \' needs a backslash'

```

你能使用双反斜杠来转义转义字符自身：

```
'an escaped escape character: \\ needs a double backslash'

```

一些特殊字符使用反斜杠作为转义字符：

```
转义序列    字符
'\t'        tabulation
'\b'        backspace
'\n'        newline
'\r'        carriage return
'\f'        formfeed
'\\'        backslash
'\''        single quote (for single quoted and triple single quoted strings)
'\"'        double quote (for double quoted and triple double quoted strings)

```


##### Unicode转义序列

对于那些键盘不能表示的字符，你能使用Unicode转义序列：一个反斜杠，后面跟着‘u’,然后是4个十六进制的数字。
例如，欧元货币标志可以使用这个表示：

```
'The Euro currency symbol: \u20AC'

```

#### 双引号字符串

双引号字符串是一些列被双引号包围的字符：

```
"a double quoted string"

```

如果没有插值表达式，双引号字符串是普通的java.lang.String，如果插值存在则是groocy.lang.GString实例。
为了转义一个双引号，你能使用反斜杠字符：”A double quote: \””。

##### 字符串插值

任何Groovy表达式可以在所有字符文本进行插值，除了单引号和三单引号字符串。插值是使用占位符上的字符串计算值替换占位符的操作。占位符表达式是被${}包围，或前缀为$的表达式。当GString被传递给一个带有一个String参数的方法时，占位符的表达式被计算值，并通过调用表达式的toString()方法以字符串形式表示。
这里是一个占位符引用局部变量的字符串：

```
def name = 'Guillaume' // a plain string
def greeting = "Hello ${name}"

assert greeting.toString() == 'Hello Guillaume'

```

而且任何Groovy表达式是合法的，正如我们在示例中使用算数表达式所见一样:

```
def sum = "The sum of 2 and 3 equals ${2 + 3}"
assert sum.toString() == 'The sum of 2 and 3 equals 5'

```

不仅任何表达式，实际上也允许${}占位符。语句也是被允许的，但一个语句等效于null。如果有多个语句插入占位符，那么最后一个语句应该返回一个有意义的值被插入占位符。例如，”The sum of 1 and 2 is equal to ${def a = 1; def b = 2; a + b}”字符串是被支持的，也能如预期一样工作，但一个好的实践是在GString占位符插入一个简单的表达式。

除了${}占位符以外，也可以使用$作为表达式前缀：


```
def person = [name: 'Guillaume', age: 36]
assert "$person.name is $person.age years old" == 'Guillaume is 36 years old'

```

但只有a.b，a.b.c等形式的前缀表达式是合法的，而包含如方法调用的圆括号，闭包的花括号，算术操作符是非法的。给出如下数值变量定义：

```
def number = 3.14

```

如下语句将会抛出groovy.lang.MissingPropertyException，因为Groovy相信你在尝试访问一个不存在数字的toString属性：

```
shouldFail(MissingPropertyException) {
  println "$number.toString()"
}

```

你可以把”$number.toString()”用解释器解释为”${number.toString}()”。
如果在GString中你需要转义$或${}占位符，使它们不出现插值，那么你只需要使用反斜杠字符转义美元符号：

```
assert '${name}' == "\${name}"

```

#### 插入闭包表达式的特殊情况

到目前为止，我们仅仅看到在${}占位符中插入任意表达式，但一个闭包表达式标记的特殊情况。当占位符包括一个箭头，${->}，这表达式实际是一个闭包表达式，你可以把它看做一个前面紧靠美元符号的闭包：

```
def sParameterLessClosure = "1 + 2 == ${-> 3}" （1）
assert sParameterLessClosure == '1 + 2 == 3'

def sOneParamClosure = "1 + 2 == ${ w -> w << 3}" （2）
assert sOneParamClosure == '1 + 2 == 3'

```

（1）这是一个不携带参数的无参闭包
（2）这里的闭包携带一个java.io.StringWrite参数，你能使用<<追加内容。在这两处，占位符被嵌入闭包。

在外观上，定义一个表达式被插入看着有一些冗长，但闭包相比表达式有一个有趣的优势：延迟计算。
让我们思考如下示例：

```
def number = 1 （1）
def eagerGString = "value == ${number}"
def lazyGString = "value == ${ -> number }"

assert eagerGString == "value == 1" （2）
assert lazyGString == "value == 1" （3）

number = 2 （4）
assert eagerGString == "value == 1" （5）
assert lazyGString == "value == 2" （6）

```

（1）我们定义一个包含1的number变量，然后插入两个GString之中，在eagerGString中作为表达式，在lazyGString中作为闭包。
（2）我们期望对于eagerGString得到的结果字符串是包含1的相同字符串
（3）lazyGString相似
（4）然后给变量赋一个新值
（5）使用纯插值表达式，这值在GString创建时结合
（6）但使用闭包表达式，GString被强转为Sring时，闭包被调用，并产生包含新数值的更新字符串。
一个嵌入的闭包表达式，携带超过一个参数，那么在运行时将会产生一个异常。闭包仅仅允许携带0个或1个参数。

#### 与Java的互操作性

当一个方法（无论是在Java或是Groovy中实现）预期需要一个java.lang.String，而我们传递了一个groovy.lang.GString实例，GString的toString()方法将会被自动透明的调用。

```
String takeString(String message) { （4）
  assert message instanceof String （5）
  return message
}

def message = "The message is ${'hello'}" （1）
assert message instanceof GString （2）

def result = takeString(message) （3）
assert result instanceof String
assert result == 'The message is hello'

```

（1）我们创建一个GSring变量
（2）我们仔细检查GString实例
（3）我们将GString传递个一个携带String参数的方法
（4）takeString()明确说明它的唯一参数是一个String
（5）我们也验证一个参数是String而不是GString

#### GString和String的hashCode

虽然插值字符串可以代替普通Java字符串，它们用一种不同的方式是字符串不同：它们的hashCode是不同的。普通Java字符串是不可变的，而一个GString依赖于插入的值，它的String是可变的。即使有相同的字符串结果，GString和String也没有相同的hashCode。

```
assert "one: ${1}".hashCode() != "one: 1".hashCode()

```

GString和String有不同的hashCode值，应该避免使用GSring作为Map的键值，我们使用String代替GString取出一个关联值。

```
def key = "a"
def m = ["${key}": "letter ${key}"] （1）

assert m["a"] == null （2）

```

（1）map被一个初始化键值对创建，其键值是GString
（2）当我们尝试使用String键值获取值时，我们并没获取对应值，因为String和GString有不同的hashCode


### 三双引号字符串


三双引号字符串与双引号字符串相同，增加多行，像三单引号字符串一样。

```
def name = 'Groovy'
def template = """
  Dear Mr ${name},

  You're the winner of the lottery!

  Yours sincerly,

  Dave
"""

assert template.toString().contains('Groovy')

```

无论是双引号还是单引号，在三双引号字符串中需要被转义。

### 斜杠字符串

除了通常的带引号字符串，groovy提供斜杠字符串，使用/作为分隔符。斜杠字符串对于定义正则表达式和模式是特别有用的，因为不需要转义反斜杠。

一个斜杠字符串：

```
def fooPattern = /.*foo.*/
assert fooPattern == '.*foo.*'

```

只有正斜杠需要反斜杠转义：

```
def escapeSlash = /The character \/ is a forward slash/
assert escapeSlash == 'The character / is a forward slash'

```

斜杠字符串是多行的：

```

def multilineSlashy = /one
two
three/

assert multilineSlashy.contains('\n')

```

斜杠字符串也能被插值（如，GString）:

```
def color = 'blue'
def interpolatedSlashy = /a ${color} car/

assert interpolatedSlashy == 'a blue car'

```

有几个陷阱需要注意：
一个空的斜杠字符串不能使用双正斜杠表示，因为它被Groovy解析器作为一个单行注释理解。这就是为什么以下断言实际上无法编译，因为它看起来像一个无终止的语句：

```
assert '' == //

```

#### 美元符修饰的斜杠字符串

美元符斜杠字符串是一个有开口$/和闭口$/界定的多行GString。这转义字符是美元符，它能转义另一个美元符，或一个正斜杠。但是双美元符和双正斜杠不用被转义，除了转义像GString占位符序列开始的字符串子序列的美元符，或者你需要转义一个序列，开头看着像闭包美元符斜杠字符串分隔符。

示例：

```
def name = "Guillaume"
def date = "April, 1st"

def dollarSlashy = $/
  Hello $name,
  today we're ${date}.

  $ dollar sign
  $$ escaped dollar sign
  \ backslash
  / forward slash
  $/ escaped forward slash
  $/$ escaped dollar slashy string delimiter
/$

assert [
  'Guillaume',
  'April, 1st',
  '$ dollar sign',
  '$ escaped dollar sign',
  '\\ backslash',
  '/ forward slash',
  '$/ escaped forward slash',
  '/$ escaped dollar slashy string delimiter'

].each { dollarSlashy.contains(it) }

```

#### 字符串总结表

```
String name              String syntax     Interpolated     Multiline    Escape character
Single quoted            '…​'                                              \
Triple single quoted     '''…​'''                            是            \
Double quoted            "…​"               是                             \
Triple double quoted     """…​"""           是               是            \
slashy                   /…​/               是               是            \
Dollar                   $/…​/$             是               是            \

```


#### 字符

与Java不同，Groovy没有显式的字符文本，然而你可以通过三种不同方式，可以将Groovy字符串实际作为一个字符使用。

```
char c1 = 'A' （1）
assert c1 instanceof Character

def c2 = 'B' as char （2）
assert c2 instanceof Character

def c3 = (char)'C' （3）
assert c3 instanceof Character

```

1）当定义变量时，通过指定char类型，使变量包含字符
（2）通过as操作符使用类型强制转换
（3）通过char操作符做类型转换
第一个选项是（1）有趣的当一个字符在一个变量中，而另外两个（2和3）是更令人关注时char值必须作为一个方法调用的参数。


### 数字

Groovy支持不同类型的整数和小数，通常以Java的Number类型返回。

#### 整数

整数类型与Java相同：

- byte
- char
- short
- int
- long
- java.lang.BigInteger

你能以如下定义创建这些类型的整数：

```
// primitive types
byte b = 1
char c = 2
short s = 3
int i = 4
long l = 5

// infinite precision
BigInteger bi = 6

```

如果你通过使用def关键字使用可选类型，那么整数的类型将是可变的：它取决于这个类型实际包含的值。

对于正数：

```
def a = 1
assert a instanceof Integer

// Integer.MAX_VALUE
def b = 2147483647
assert b instanceof Integer

// Integer.MAX_VALUE + 1
def c = 2147483648
assert c instanceof Long

// Long.MAX_VALUE
def d = 9223372036854775807
assert d instanceof Long

// Long.MAX_VALUE + 1
def e = 9223372036854775808
assert e instanceof BigInteger

```

对于负数也一样：

```
def na = -1
assert na instanceof Integer

// Integer.MIN_VALUE
def nb = -2147483648
assert nb instanceof Integer

// Integer.MIN_VALUE - 1
def nc = -2147483649
assert nc instanceof Long

// Long.MIN_VALUE
def nd = -9223372036854775808
assert nd instanceof Long

// Long.MIN_VALUE - 1
def ne = -9223372036854775809
assert ne instanceof BigInteger

```

##### 可选择的非十进制表示

二进制数

在Java6及以前和Groovy一样，数字只能使用十进制，八进制和十六进制表示，使用Java7和Groovy2你能使用0b前缀作为一个二进制符号：

```
int xInt = 0b10101111
assert xInt == 175

short xShort = 0b11001001
assert xShort == 201 as short

byte xByte = 0b11
assert xByte == 3 as byte

long xLong = 0b101101101101
assert xLong == 2925l

BigInteger xBigInteger = 0b111100100001
assert xBigInteger == 3873g

int xNegativeInt = -0b10101111
assert xNegativeInt == -175

```

八进制数

八进制数使用0后面跟八进制数的典型格式表示。

```
int xInt = 077
assert xInt == 63

short xShort = 011
assert xShort == 9 as short

byte xByte = 032
assert xByte == 26 as byte

long xLong = 0246
assert xLong == 166l

BigInteger xBigInteger = 01111
assert xBigInteger == 585g

int xNegativeInt = -077
assert xNegativeInt == -63

```

十六进制数

十六进制数使用0x后面跟十六进制数的典型格式表示。

```
int xInt = 0x77
assert xInt == 119

short xShort = 0xaa
assert xShort == 170 as short

byte xByte = 0x3a
assert xByte == 58 as byte

long xLong = 0xffff
assert xLong == 65535l

BigInteger xBigInteger = 0xaaaa
assert xBigInteger == 43690g

Double xDouble = new Double('0x1.0p0')
assert xDouble == 1.0d

int xNegativeInt = -0x77
assert xNegativeInt == -119

```

#### 小数

小数类型与Java一样：

- float
- double
- java.lang.BigDecimal

你能采用如下定义方式创建这些类型的数字：

```
// primitive types
float f = 1.234
double d = 2.345

// infinite precision
BigDecimal bd = 3.456

```

小数能使用指数，使用e或E指数字母，紧跟着一个可选符号，且有一个整数表示指数：

```
assert 1e3 == 1_000.0
assert 2E4 == 20_000.0
assert 3e+1 == 30.0
assert 4E-2 == 0.04
assert 5e-1 == 0.5

```

为了精确的进行小数计算，Groovy选择java.lang.BigDecimal作为小数类型。此外，float和double也被支持，但要求有一个显式类型定义，类型转换或后缀。即使BigDecimal是默认的小数，携带float或double作为类型参数的方法或闭包也可以接受这些数值。
小数不能使用二进制，八进制和十六进制表示。


#### 有下划线的文本

当写一个很长的数字，使用眼睛很难弄清楚有多少数字组合在一起，例如使用千，单词等组合。通过允许你在数字中添加一些下划线，更容易发现这些组合：

```
long creditCardNumber = 1234_5678_9012_3456L
long socialSecurityNumbers = 999_99_9999L
double monetaryAmount = 12_345_132.12
long hexBytes = 0xFF_EC_DE_5E
long hexWords = 0xFFEC_DE5E
long maxLong = 0x7fff_ffff_ffff_ffffL
long alsoMaxLong = 9_223_372_036_854_775_807L
long bytes = 0b11010010_01101001_10010100_10010010

```

#### 数字类型后缀

通过使用大写或小写类型后缀（见下表），我们能强制将一个数字（包括二进制，八进制，十六进制）给一个指定类型。

```
Type            Suffix
BigInteger      G 或 g
Long            L 或 l
Integer         I 或 i
BigDecimal      G 或 g
Double          D 或 d
Float           F 或 f

```

如:

```
assert 42I == new Integer('42')
assert 42i == new Integer('42') // lowercase i more readable
assert 123L == new Long("123") // uppercase L more readable
assert 2147483648 == new Long('2147483648') // Long type used, value too large for an Integer
assert 456G == new BigInteger('456')
assert 456g == new BigInteger('456')
assert 123.45 == new BigDecimal('123.45') // default BigDecimal type used
assert 1.200065D == new Double('1.200065')
assert 1.234F == new Float('1.234')
assert 1.23E23D == new Double('1.23E23')
assert 0b1111L.class == Long // binary
assert 0xFFi.class == Integer // hexadecimal
assert 034G.class == BigInteger // octal

```

#### 数学运算

尽管运算在以后会被覆盖，讨论数学运算和结果类型仍然是重要的。
除法与幂方二元操作符放在一边（下文讨论）。

- byte char short和int进行二元操作的结果是int
- 使用long与byte char short int进行二元操作的结果是long
- 使用BigInteger与任意其他整数类型进行二元操作结果是BigInteger
- float double与BigDecimal进行二元运算的结果是double
- 两个BigDecimal进行二元运算的结果是BigDecimal
- 下表总结了这些原则：

```
        byte    char    short    int    long    BigInteger    float      double    BigDecimal
byte    int     int     int      int    long    BigInteger    double     double    double
char            int     int      int    long    BigInteger    double     double    double
short                   int      int    long    BigInteger    double     double    double
int                              int    long    BigInteger    double     double    double
long                                    long    BigInteger    double     double    double
BigInteger                                      BigInteger    double     double    double
float                                                         double     double    double
double                                                                   double    double
BigDecimal                                                                         BigDecimal

```

由于Groovy操作符重载，BigInteger与BigDecimal通常也能进行运算操作，与Java不同，在Java中你不得不显式使用方法操作这些数字。

##### 除法运算符的情况

如果任何一个操作数是float或double,那么除法运算符/(和/= 用于除法和赋值)产生double结果，否则（当两个操作数是一个与整型类型short, char, byte, int, long, BigInteger or BigDecimal的任意组合）是一个BigDecimal结果。

如果除法是精确的（如，结果可以在相同的精度和标度范围内精确表示），那么BigDecimal的除法实际执行的是divide()方法，或使用两个操作数的最大精度加10,和一个最大值为10的标度的MathContext。

对于整数除法和Java相同，你应该使用intdiv()方法，因为Groovy没有专门提供一个整数操作符。

##### 幂运算情况

幂运算操作符使用**操作符，有两个参数：基数和指数。幂运算的结果取决于它的操作数以及操作的结果（特别是结果可以被表示为一个整数值）。

以下这些原则被用于决定Groovy幂运算操作结果的类型：

```
（1）如果指数是一个小数
	1.如果结果能作为一个Integer表示，那么返回一个Integer
	2..如果结果能作为一个Long表示，那么返回一个Long
	3.否则返回一个Double

（2）如果指数是一个整数
 	1.如果是一个严格的负数，那么返回一个Integer，Long或Double，结果值使用那种类型填充。
        2.如果指数是正数或0
 	  1)如果基数是BigDecimal，那么返回一个BigDecimal结果值
	  2)如果基数是BigInteger，那么返回一个BigInteger结果值
	  3)如果基数是Integer，那么返回一个Integer值，否则返回BigInteger
	  4)如果基数是Long，那么返回一个Long值，否则返回BigInteger

```

我们使用一些实例说明这些原则：

```
// base and exponent are ints and the result can be represented by an Integer
assert 2 ** 3 instanceof Integer // 8
assert 10 ** 9 instanceof Integer // 1_000_000_000

// the base is a long, so fit the result in a Long
// (although it could have fit in an Integer)
assert 5L ** 2 instanceof Long // 25

// the result can't be represented as an Integer or Long, so return a BigInteger
assert 100 ** 10 instanceof BigInteger // 10e20
assert 1234 ** 123 instanceof BigInteger // 170515806212727042875...

// the base is a BigDecimal and the exponent a negative int
// but the result can be represented as an Integer
assert 0.5 ** -2 instanceof Integer // 4

// the base is an int, and the exponent a negative float
// but again, the result can be represented as an Integer
assert 1 ** -0.3f instanceof Integer // 1

// the base is an int, and the exponent a negative int
// but the result will be calculated as a Double
// (both base and exponent are actually converted to doubles)
assert 10 ** -1 instanceof Double // 0.1

// the base is a BigDecimal, and the exponent is an int, so return a BigDecimal
assert 1.2 ** 10 instanceof BigDecimal // 6.1917364224

// the base is a float or double, and the exponent is an int
// but the result can only be represented as a Double value
assert 3.4f ** 5 instanceof Double // 454.35430372146965
assert 5.6d ** 2 instanceof Double // 31.359999999999996

// the exponent is a decimal value
// and the result can only be represented as a Double value
assert 7.8 ** 1.9 instanceof Double // 49.542708423868476
assert 2 ** 0.1f instanceof Double // 1.0717734636432956

```

#### 布尔

Boolean是一种特殊的数据类型，用于表示真值：true和false。使用这种数据类型作为跟踪真假条件的简单标志。

Boolean能被存储在变量中，成员变量中，就像其他数据类型一样：

```
def myBooleanVariable = true
boolean untypedBooleanVar = false
booleanField = true

```

true和false是仅有的两个原始布尔值。但更复杂的布尔表达式能使用逻辑操作符表示。

除此之外，Groovy有一些特殊的规则（经常因为Groovy真值涉及）用于将非布尔值对象转化为一个布尔值。

#### 列表

Groovy使用逗号分隔列表中的值，并使用方括号包围，用来指定一个列表。Groovy的列表是java.util.List，因为Groovy没有定义任何集合类。当定义一个列表常量时，默认的列表具体实现是java.util.ArrayList，除非你指定，我们将在后面看到。

```
def numbers = [1, 2, 3] （1）

assert numbers instanceof List （2）
assert numbers.size() == 3 （3）

```

（1）我们定义用逗号分隔，并用方括号包围列表数字，并将列表赋值给一个变量

（2）list是java java.util.List接口的实例

（3）列表的大小可以使用size()方法查询，表明列表有三个元素

在上面的示例中，我们使用了一个元素类型相同的列表，我们也能创建包含不同类型元素的列表：


```
def heterogeneous = [1, "a", true] （1）

```

（1）我们的列表包含一个数字，一个字符串，一个布尔值

我们提及到，默认的列表字面量实际是java.util.ArrayList的实例，但列表使用不同返回类型也是可以的，使用as操作符进行类型转换，或使用变量的定义类型：

```
def arrayList = [1, 2, 3]
assert arrayList instanceof java.util.ArrayList

def linkedList = [2, 3, 4] as LinkedList （1）
assert linkedList instanceof java.util.LinkedList

LinkedList otherLinked = [3, 4, 5] （2）
assert otherLinked instanceof java.util.LinkedList

```

（1）我们使用as操作符进行类型转换，显式请求一个java.util.LinkedList实现
（2）我们使用类型为java.util.LinkedList的变量保存列表字面量

你能通过下标操作符`[](读和写元素值)`并使用正索引值访问列表元素或负索引值从列表尾部访问元素，也可以使用范围，或使用左移<<追加列表元素：

```
def letters = ['a', 'b', 'c', 'd']

assert letters[0] == 'a' （1）
assert letters[1] == 'b'

assert letters[-1] == 'd' （2）
assert letters[-2] == 'c'

letters[2] = 'C' （3）
assert letters[2] == 'C'

letters << 'e' （4）
assert letters[ 4] == 'e'
assert letters[-1] == 'e'

assert letters[1, 3] == ['b', 'd'] （5）
assert letters[2..4] == ['C', 'd', 'e'] （6）

```

（1）访问列表的第一个元素（索引从零开始计算）

（2）使用负索引访问列表的最后一个元素：-1是列表从尾部开始的第一个元素

（3）使用赋值操作为列表的第三个元素设置一个新值

（4）使用<<左移操作符在列表尾部追加一个元素

（5）一次访问两个元素，并返回一个包含这两个元素的新列表

（6）使用范围访问列表中这个范围内的元素，从start到end元素位置

因为列表可以很自然的做到元素类型不同，因此列表也可以包含列表用于创建多维列表：

```
def multi = [[0, 1], [2, 3]] （1）
assert multi[1][0] == 2 （2）

```

（1）定义一个数字列表的列表

（2）访问顶级列表的第二个元素，内部列表的第一个元素

### 数组

Groovy使用列表标记来标记数组，但为了创建字面量数组，你需要通过类型转换或类型定义来定义数组类型。

```
String[] arrStr = ['Ananas', 'Banana', 'Kiwi'] （1）

assert arrStr instanceof String[] （2）
assert !(arrStr instanceof List)

def numArr = [1, 2, 3] as int[] （3）

assert numArr instanceof int[] （4）
assert numArr.size() == 3

```

（1）使用显式变量类型定义一个字符串数组

（2）断言说明我们创建了一个字符串数组

（3）使用as操作符创建以int数组

（4）断言表明我们创建了一个原始类型的int数组

你也能创建多维数组：

```
def matrix3 = new Integer[3][3] （1）
assert matrix3.size() == 3

Integer[][] matrix2 （2）
matrix2 = [[1, 2], [3, 4]]
assert matrix2 instanceof Integer[][]

```

（1）你能定义一个新数组的边界

（2）或不指定它的边界定义一个新数组

通过与列表相同的标记访问数组的元素：

```
String[] names = ['Cédric', 'Guillaume', 'Jochen', 'Paul']
assert names[0] == 'Cédric' （1）

names[2] = 'Blackdrag' （2）
assert names[2] == 'Blackdrag'

```

（1）取得数组的第一个元素

（2）为数组的第三个元素设置一个新值

Java数组初始化标记Groovy不支持，因为大括号会被误解为Groovy的闭包标记。

### 映射

在其它语言中，有时候称为字典或关联数组，Groovy称为映射。映射使键到值关联，使用冒号将键值分隔开，每个键值对使用逗号，整个键和值使用方括号包围。

```
def colors = [red: '#FF0000', green: '#00FF00', blue: '#0000FF'] （1）

assert colors['red'] == '#FF0000' （2）
assert colors.green == '#00FF00' （3）

colors['pink'] = '#FF00FF' （4）
colors.yellow = '#FFFF00' （5）

assert colors.pink == '#FF00FF'
assert colors['yellow'] == '#FFFF00'

assert colors instanceof java.util.LinkedHashMap

```

 (1）我们定义了一个字符串颜色名关联十六进制的html颜色的映射
 
（2）我们使用下标标记检查red键值关联的内容

（3）我们也能使用属性标记访问绿颜色十六进制表达式

（4）相似的，我们也能使用下标标记添加一个新的键值对

（5）或者使用属性标记添加yellow颜色

当使用这些键的名字时，我们实际上在映射中定义了一个键值。
Groovy创建的映射实际是java.util.LinkedHashMap的实例。

如果你尝试在映射中访问不存在的键：

```
assert colors.unknown == null

```

你将取回null。

在上面的示例中我们使用字符串键值，你也可以使用其他类型作为键值：

```
def numbers = [1: 'one', 2: 'two']

assert numbers[1] == 'one'

```

这里我们使用数字作为键值，作为数字能清楚的识别数字，因此Groovy不会像之前的示例一样创建一个字符串的键。但是考虑你想传递一个变量代替键的情况下，有一个变量值将会作为键：

```
def key = 'name'
def person = [key: 'Guillaume'] （1）

assert !person.containsKey('name') （2）
assert person.containsKey('key') （3）

```

（1）key同’Guillaume’关联，名字将会变为”key”字符串，而不是其值

（2）这个映射不包括”name”键

（3）代替的是，映射包括一个”key”键

你也能通过引用的字符串以及键: [“name”: “Guillaume”]。如果你的见字符串不是一个合法的标识符，这是强制的，例如，如果你想创建一个字符串键像哈希：

```
["street-name": "Main street"]。


```

在映射定义中需要传递变量值，你必须使用圆括号包围这个变量或表达式：

```
person = [(key): 'Guillaume'] （1）

assert person.containsKey('name') （2）
assert !person.containsKey('key') （3）

```

（1）这次，我们使用圆括号包围key变量，指示解析器我们传递一个变量，而不是定义一个字符串键

（2）映射包含name键

（3）但映射不像之前包含key键


## Groovy 函数与闭包

### 函数

有些人可能对函数和方法的叫法一直不太清楚。事实上没有本质区别，在面向过程的语言中一般称为函数，在函数式编程中一般都叫做函数。方法的一般是类的方法，在某一个类中定义的称之为方法。

在Groovy中除非指定了确定的返回类型，void也可以作为返回值的一种，否则定义函数必须加上关键字def。

```
String getString(){
    return "hello world"
}
 
def getDef(){
    return "hello world"
}
 
void printSomething(){
    println "hello worlld"
}
 
//错误的定义，编译可以通过，但是运行时报错
//getString(){
//    return "hello world"
//}

```

在定义函数需要传参时可以不设置设置参数的类型，默认是Object类型的，如果用def关键字设置参数类型，事实上也是使用的Object定义参数的。

```
def printSomething01(param){
    println param
}
 
def printSomething02(int param){
    println param
}
 
def printSomething03(def param){
    println param
}

```

在调用函数时，如果所定义的函数有参数，在使用的时候可以不使用括号，但是必须得传入参数，参数与函数名以空格隔开。

如果不出入参数必须添加括号，否则代码编译可以通过，但是运行时会出错，会将函数误认为是一个属性。

如果所定义的函数没有参数，在调用的时候必须添加括号，否则运行出错，会将函数误认为是一个属性。

函数调用的时候参数的个数必须匹配，否则也会报错，提示没有定义该函数，如果单个参数除外，可以不用输入参数，系统默认赋值null。

```

def printSomething(param01,param02){
    println param01+param02
}

printSomething ("hello","world")//helloworld
printSomething "hello","world"//helloworld

//参数个数不对，报错
//printSomething ("hello")

def printOne(param){
    println param
}
printOne()//null


```

函数可以有返回值，如果有显示地使用return关键字，则返回return指定的返回值，其后面的语句不再执行。

如果没有显式地使用return关键字，则返回函数最后一行语句的运行结果。

如果使用void关键字代替def关键字定义函数，则函数的返回值将为null。

```
def printSomething(){
    return "hello"
    println "world"
}
println printSomething()//hello
 
def printSomething01(){
    "hello world"
    1
}
println printSomething01()//1
 
def printSomething02(){
    1
    "hello world"
}
println printSomething02()//hello world

```

支持函数重载，当参数个数不同时，函数名称可以同名。

```
def printSomething(param){
    println param
}
 
def printSomething(param01,param02){
    println param01+" "+param02
}
 
printSomething("hello")
printSomething("hello","world")

```

函数内不可以访问函数外的变量

```
int m=1
def n="hello"
//error
def printSomething(){
    println m//error
    println n//error
}
printSomething()

```

Groovy支持不定长参数。

```
def printSomething(... params) {
    println(params[0])
}
 
printSomething("hello")
printSomething("hello","world")

```

函数可以赋值给其它函数，使用语法标记&将函数赋予新的函数。

```
def printSomething() {
    println("hello world")
}
 
//printSomething不可以加括号
def printHello=this.&printSomething
 
printHello()          //hello world
printSomething()      //hello world

```

### 闭包

Groovy中闭包是这么定义的:可以用作函数参数和方法参数的代码块。可以把这个代码块理解为一个函数指针。

闭包的定义格式：

```
def xxx = { params -> code }
//或者
def xxx={code}

```

闭包可以访问外部的变量，记住一点方法是不能访问外部变量的。

```
def str="hello world"
 
def closure={
    println str
}
 
closure()//hello world

```

闭包是有返回值的，默认最后一行语句就是该闭包的返回值，如果最后一行语句没有不输入任何类型，闭包将返回null。

```
def closure = {
    println "hello world"
    return "I'm callback"
}
//hello world
//I'm callback
println closure()
 
def noReturn={
    println "hello world"
}
//hello world
//null
println noReturn()

```

闭包可以有参数，如果没有定义参数，会有一个隐式的默认参数it，如果没有参数可以将[参数]和[->]省略。

如果存在参数，在[->]之前的就是参数，如果只有一个参数，参数可以省略。

闭包中参数名称不能与闭包内或闭包外的参数名重名

```
def closure={
    println "hello $it"
}
closure("admin")
 
def closure={
    param01,param02,param03->println param01+param02+param03
}
closure "hello","world","ok"
 
def closure={
    println it
}
closure "hello world"
 
//编译时就不通过
//def param
//def closure={
//    param->println param
//}

```

闭包可以作为一个参数传递给另一个闭包，也可以在闭包中返回一个闭包。

```
def toTriple = { n -> n * 3 }
def runTwice = { a, c -> c(c(a)) }
println runTwice(5, toTriple)//45
 
def times = { x -> { y -> x * y } }
println times(3)(4)//12

```


闭包的一些快捷写法，当闭包作为闭包或方法的最后一个参数，可以将闭包从参数圆括号中提取出来接在最后。

如果闭包中不包含闭包，则闭包或方法参数所在的圆括号也可以省略。

对于有多个闭包参数的，只要是在参数声明最后的，均可以按上述方式省略。

```
def runTwice = { a, c -> c(c(a)) }
println runTwice(5, { it * 3 }) //45 usual syntax
println runTwice(5) { it * 3 } //45
 
def closure = {
    param -> println param
}
closure "hello world"
 
def runTwoClosures = { a, c1, c2 -> c1(c2(a)) }
//when more than one closure as last params
assert runTwoClosures(5, { it * 3 }, { it * 4 }) == 60 //usual syntax
assert runTwoClosures(5) { it * 3 } { it * 4 } == 60 //shortcut form

```

闭包接受参数的规则，会将参数列表中所有有键值关系的参数，作为一个map组装，传入闭包作为调用闭包的第一个参数。

```
def f= {m, i, j-> i + j + m.x + m.y }
println f(6, x:4, y:3, 7)//20

```

如果闭包的参数声明中没有list，那么传入参数可以设置为list，里面的参数将分别传入闭包参数。

```
def c = { a, b, c -> a + b + c }
def list = [1, 2, 3]
println c(list) // 6

```

## Groovy 类与对象

Groovy类与Java类似，在字节码级都被编译成Java类，由于其在定义变量上面的灵活性，所以在新建一个Groovy类时还是有一些不同的，增加了许多灵活性。由于Groovy是松散型语言，它并不强制你给属性、方法参数和返回值定义类型。如果没有指定类型，在字节码级别会被编译成Object。在定义类的属性时不用刻意加上权限修饰符，默认就是public的。

```
class Book{
    def title
    String author
	private int price
	
    public Book(title){
        this.title=title
    }
    boolean order(int isbn){
        true
    }
    def title(){
        "Booke Title"
    }
}

Book book=new Book("Hello Groovy")
book.order(1001)

book.title//获取属性
book.title()//访问方法

```

如果我们将Book类看做是一个JavaBean，事实上Groovy在编译完成后会自动帮助我们生成getter与setter方法，但是私有属性除外也就是说price属性我们不能使用getter与setter方法。

```
Book book=new Book("Hello Groovy")
println book.getTitle()//Hello Groovy
 
book.setTitle("New Groovy")
println book.getTitle()//New Groovy
 
println book.title////New Groovy

```

在Groovy中类名和文件名并不需要严格的映射关系，我们知道在Java中主类名必须与文件同名，但是在Groovy中一个文件可以定义多个public类。
在Groovy中可以定义与任何类不相关的方法和语句，这些方法通常称为独立方法或者松方法。

```

class Hello{
    public static String hello(){
        return "hello"
    }
}
class World{
    public static String world(){
        return "world"
    }
}
 
println Hello.hello()+World.world()
 
def helloWorld(){
	return "hello world"
}

```

上面一个文件名定义为Structure.groovy，在这个文件中包含了类的定义和独立方法声明，它编译之后会发生什么呢。首先会生成一个与文件同名的class文件。所有的松语句都集中在run方法中，并且run方法被该类的main方法调用。独立方法被编译成了类的静态方法。与Java相似，每一个独立的类都会被编译成一个单独的class文件。因此编译Structure.groovy文件最后会被编译成Hello.class、World.class和Structure.class。上面一个文件名定义为Structure.groovy，在这个文件中包含了类的定义和独立方法声明，它编译之后会发生什么呢。首先会生成一个与文件同名的class文件。所有的松语句都集中在run方法中，并且run方法被该类的main方法调用。独立方法被编译成了类的静态方法。与Java相似，每一个独立的类都会被编译成一个单独的class文件。因此编译Structure.groovy文件最后会被编译成Hello.class、World.class和Structure.class。


## 参考文献

[Groovy进阶之函数、闭包和类](http://www.sunnyang.com/522.html)
[《Groovy语言规范》-语法](http://ifeve.com/groovy-syntax/)