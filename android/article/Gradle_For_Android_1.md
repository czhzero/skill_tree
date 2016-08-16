# Gradle for Andoid 学习笔记(一) : Gradle基础


## 什么是Gradle

Gradle是一个构建系统, Android Studio默认创建的project都是基于Gradle构建脚本。Gradle具有如下特点,

1. 声明构建和协议构建

	Gradle的核心是基于Groovy的丰富的可扩展的领域特定语言（DSL），通过提供你自定义的声明语言元素，Gradle把依赖关系传递到下一层中，这些元素也提供了对很多语言的协议构建支持，比如java、Groovy，OSGI，Web和Scala项目。甚至，声明语言是可扩展的，你可以有自己的新语言元素或者是加强现有的。这样，就提供了一个简洁、易于维护和理解的构建过程。2. 依赖型编程语言

	声明式的语言构建于一个通用的任务图之上，在你的构建任务重可以充分的利用。它提供了适应你独特需求的最大灵活度的工具。

3. 结构化构建

	Gradle的灵活性和和丰富性允许你用通用的设计原则来构建项目。比如说，构建可重用的的逻辑块的逻辑非常简单。不要试图去强迫拆散本应该在一起的东西，比如说是项目的层次结构。这样可以避免项目太分散，因为分散的项目会导致你的构建过程变成一个噩梦！最后，你可以构建一个结构良好、易于维护、理解的构建过程。

4. 深度API

	在构建执行的整个生命周期里，你可以嵌入很多的钩子，Gradle允许你检测和自定义配置和执行非常核心的行为。

5. Gradle Scales

	Gradle Scales 非常好，它可以增加你的生产力。从简单的单项目到企业的多项目构建都可以。随着功能的增加，它可以解决很多大型企业构建过程中的问题。

6. 多项目构建

	  Gradle对多项目构建的支持非常好，Gradle还提供了部分构建，如果你构建一个单项目，那么Gradle会构建整个项目的目录，在多项目的构建中，你可以选择构建部分项目，增量构建可以大大的节省你的时间。

7. 很多方式管理依赖

	不同的团队喜欢使用不同的方式来管理外部依赖，Gradle为所有的方式都提供了支持。你可以使用远程的依赖管理库，比如Maven和ivy或者是本地的文件系统。

8. Gradle是第一个集成的构建工具

	Ant的task是支持的，更有趣的事，Ant的project也是支持的。Gradle为Ant的项目提供了深度支持，在运行时可以把Ant的 targets转换城本地的Gradle任务。你可以在Gradle中依赖或者是改进Ant，你甚至可以在Gradle的任务重宣布对build.xml 的依赖。Gradle支持现有的Maven仓库和Ivy仓库，Gradle还提供了将Maven的pom.xml转换成Gradle脚本的工具。 Maven项目的运行时导入时代即将到来。

9. 容易迁移

	Gradle可以适应你有的任何结构，因此你可以在项目运行的生产环境中进行项目构建，我们通常建议写一个测试程序来保证项目正常运行，使用Gradle可以尽可能的减少项目迁移出现的问题，这也是进行项目重构的最佳实践，也就是“baby steps”。
	
10. Groovy

	Gradle使用Grovvy来写脚本，而不是XML，这是因为Groovy比XML的可读性更好。Gradle的设计并不是要提供一个严格的框架。 Gradle提供了一些标准，但是并不是不能修改的。这是Gradle和其他声明性构建系统的区别和特色。Groovy不仅仅是糖衣，添加Groovy得到了一个愉快和富有成效的经验。
	
11. Gradle Wrapper

	Gradle Wrapper 允许你在没有安装Gradle的机器上运行Gradle脚本，在一些持续性的集成服务器上是非常有用的。

12. 免费开源

	Gradle是开源项目，并且采用是ASL协议授权。


## Android Studio 的Gradle构建脚本 

通过Android Studio新建一个Android项目，项目中会自动帮你创建三个gradle文件，settings.gradle,build.gradle,app/build.gradle.


### Android Studio 项目结构

![Gradle for Andoid](http://o7y1sf21i.bkt.clouddn.com/blog/011/lALOaELhKM0Cas0BeA_376_618.png)


### Android Studio 项目目录结构

```
GradleStudy/
├── .gitignore
├── .gradle
│   └── 2.10
│       └── taskArtifacts
├── GradleStudy.iml
├── app
│   ├── .gitignore
│   ├── app.iml
│   ├── build
│   │   ├── generated
│   │   ├── intermediates
│   │   ├── outputs
│   │   └── tmp
│   ├── build.gradle
│   ├── libs
│   ├── proguard-rules.pro
│   └── src
│       ├── androidTest
│       ├── main
│       └── test
├── build
│   ├── generated
│   │   └── mockable-android-23.jar
│   └── intermediates
│       └── dex-cache
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradle.properties
├── gradlew
├── gradlew.bat
├── local.properties
└── settings.gradle

```

### 主要构建文件介绍

- settings.gradle 

```
include ':app'

```

settings.gradle用来指定项目中所包含的子项目。'app'就是默认的子项目。你也可以通过File/New Module菜单创建新的子项目。

- Top-level build.gradle 

```
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.0.0'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

```

//TODO

- SubProject build.gradle 

```
apply plugin: 'com.android.application'

android {
    compileSdkVersion 23
    buildToolsVersion "23.0.3"

    defaultConfig {
        applicationId "com.chen.gradle"
        minSdkVersion 15
        targetSdkVersion 23
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:23.4.0'
    compile 'com.android.support:design:23.4.0'
}

```


- Gradle Wrapper

grade只是一个构建工具，而新版本总是在更迭，所以使用Gradle Wrapper将会是一个好的选择去避免由于gradle版本更新导致的问题。Gradle Wrapper提供了一个windows的batch文件和其他系统的shell文件，当你使用这些脚本的时候，当前gradle版本将会被下载，并且会被自动用在项目的构建，所以每个开发者在构建自己app的时候只需要使用Wrapper。所以开发者不需要为你的电脑安装任何gradle版本，在mac上你只需要运行gradlew，而在windows上你只需要运行gradlew.bat。

你也可以利用命令行./gradlew -v来查看当前gradle版本。下列是wrapper的文件夹：

```
  GradleStudy/
   ├── gradlew
   ├── gradlew.bat
   └── gradle/wrapper/
       ├── gradle-wrapper.jar
       └── gradle-wrapper.properties

```

可以看到一个bat文件针对windows系统，一个shell脚本针对mac系统，一个jar文件，一个配置文件。配置文件包含以下信息：

```
#Mon Dec 28 10:00:20 PST 2015
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip

```

修改配置这个配置文件，就可以更改gradle版本。

另外通过Android Studio, 也可以修改Gradle版本。

![Gradle Version](http://o7y1sf21i.bkt.clouddn.com/blog/005/lALOXyEtXczizQUC_1282_226.png)



## Gradle 命令介绍

- 常用命令

```

## 编译整个项目
> ./gradlew build


## 编译指定子项目, 子项目从settings.gradle中获取
> ./gradlew :app:build

## 输出所有Gradle任务
> ./gradlew :tasks

## 打包命令
> ./gradlew assemble
> ./gradlew assembleRelease
> ./gradlew assembleDebug


```

- 命令参数

gradle命令可以同时执行多个任务, 多任务之间用空格隔开,
若包含多个相同任务，也只会执行一次。

```
> ./gradlew lint assembleDebug

```

gradle执行task时，可以提前指定执行task的模块,

```

## 执行task的help指令

> ./gradlew help --task androidDependencies
> 

输出:

:help
Detailed task information for androidDependencies

Paths
     :account:androidDependencies
     :app:androidDependencies
     :base:androidDependencies
     :task:androidDependencies
     :team:androidDependencies
     :tools:androidDependencies
     :libraries:android-library-fengxin:androidDependencies

...
     

## 从上述命令结果，可知，每个子模块，都存在一个androidDependencies任务
## 因此可以针对单个模块执行对应的任务

> ./gradlew :account:androidDependencies
> ./gradlew :task:androidDependencies



```


gradle可以通过 -x 命令排除制定的任务, 多个任务间用逗号隔开。

```
> ./gradlew assembleDebug -x lintDebug

```

如果你的gradle文件没有在build.gradle中调用，你可以使用 -b 命令
直接指定需要运行的gradle文件

```

> ./gradlew -b config/dependencies.gradle

```

gradle的任务名称，可以进行简写，只要能够唯一确定该任务，即可。

```

## 下面这三个命名就是等价的
> ./gradlew assembleRelease
> ./gradlew aR
> ./gradlew aRel
> 

```

- 通过Android Studio执行

Android Studio在界面最右侧，有个Gradle Project浮动窗，
打开后，里面将项目中所有的task都放在了里面，双击即可运行。

![](http://o7y1sf21i.bkt.clouddn.com/blog/011/lALOaFpKs80CXM0CWw_603_604.png)



## 参考资料

[Gradle for Android 第一篇( 从 Gradle 和 AS 开始 )](https://segmentfault.com/a/1190000004229002)

[Gradle Recipes for Android](https://gradle.org/getting-started-android-build/)


