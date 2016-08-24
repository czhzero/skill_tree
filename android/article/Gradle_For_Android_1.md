# Gradle for Andoid 学习笔记(一) : Gradle基础


## 什么是Gradle

Gradle是一个构建系统, Android Studio默认创建的project都是基于Gradle构建脚本。在Gradle爆红之前，常用的构建工具是ANT，然后又进化到Maven。ANT和Maven这两个工具其实也还算方便，现在还有很多地方在使用。但是二者都有一些缺点，所以让更懒得人觉得不是那么方便。比如，Maven编译规则是用XML来编写的。XML虽然通俗易懂，但是很难在xml中描述if{某条件成立，编译某文件}/else{编译其他文件}这样有不同条件的任务。相比而言，Gradle使用是DSL,领域相关语言，比起xml更加方便。

Gradle具有如下特点,

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


## Android Studio 的 Gradle 构建脚本 

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


## Gradle 工作流程

![](http://o7y1sf21i.bkt.clouddn.com/blog/011/20150905194317170.png)

Gradle执行的生命周期，包含三个阶段，

- 初始化阶段：执行settings.gradle , project实例在这儿创建，如果有多个模块，即有多个build.gradle文件，多个project将会被创建。

- 配置阶段：在该阶段，解析每个project中的build.gradle。比如multi-project build例子中，解析每个子目录中的build.gradle。在这两个阶段之间，我们可以加一些定制化的Hook。这当然是通过API来添加的。

- 执行阶段：这一阶段，gradle会决定哪一个tasks会被执行，哪一个tasks会被执行完全依赖开始构建时传入的参数和当前所在的文件夹位置有关。

在settings.gradle添加如下内容,

```

gradle.beforeProject {
    Project project ->
        println project.name + " is before Project"
}


gradle.taskGraph.whenReady {
    TaskExecutionGraph graph ->
        println graph.allTasks.each {
            Task task ->
                println task.name + 'is ready'
        }
}

gradle.buildFinished {
    println 'gralde build finish'
}



```

运行结果:

```
GradleStudy is before Project		//主项目
app is before Project				//子项目

...

preBuildis ready
preDebugBuildis ready
checkDebugManifestis ready
preReleaseBuildis ready
prepareComAndroidSupportAnimatedVectorDrawable2340Libraryis ready
prepareComAndroidSupportAppcompatV72340Libraryis ready
prepareComAndroidSupportDesign2340Libraryis ready
prepareComAndroidSupportRecyclerviewV72340Libraryis ready
prepareComAndroidSupportSupportV42340Libraryis ready
prepareComAndroidSupportSupportVectorDrawable2340Libraryis ready
prepareDebugDependenciesis ready
compileDebugAidlis ready

...

:app:preBuild UP-TO-DATE
:app:preDebugBuild UP-TO-DATE
:app:checkDebugManifest
:app:preReleaseBuild UP-TO-DATE
:app:prepareComAndroidSupportAnimatedVectorDrawable2340Library UP-TO-DATE
:app:prepareComAndroidSupportAppcompatV72340Library UP-TO-DATE
:app:prepareComAndroidSupportDesign2340Library UP-TO-DATE
:app:prepareComAndroidSupportRecyclerviewV72340Library UP-TO-DATE
:app:prepareComAndroidSupportSupportV42340Library UP-TO-DATE
:app:prepareComAndroidSupportSupportVectorDrawable2340Library UP-TO-DATE
:app:prepareDebugDependencies
:app:compileDebugAidl UP-TO-DATE
:app:compileDebugRenderscript UP-TO-DATE

...

BUILD SUCCESSFUL

Total time: 6.882 secs
gralde build finish   //最后显示


```



## Gradle 基本组件

Gradle中，每一个待编译的工程都叫一个Project。每一个Project在构建的时候都包含一系列的Task。比如一个Android APK的编译可能包含：Java源码编译Task、资源编译Task、JNI编译Task、lint检查Task、打包生成APK的Task、签名Task等。
一个Project到底包含多少个Task，其实是由编译脚本指定的插件决定。插件是什么呢？插件就是用来定义Task，并具体执行这些Task的东西。
刚才说了，Gradle是一个框架，作为框架，它负责定义流程和规则。而具体的编译工作则是通过插件的方式来完成的。比如编译Java有Java插件，编译Groovy有Groovy插件，编译Android APP有Android APP插件，编译Android Library有Android Library插件
好了。到现在为止，你知道Gradle中每一个待编译的工程都是一个Project，一个具体的编译过程是由一个一个的Task来定义和执行的。


> 每一个Library和每一个App都是单独的Project。根据Gradle的要求，每一个Project在其根目录下都需要有一个build.gradle。build.gradle文件就是该Project的编译脚本，类似于Makefile。

> 对于multi-projects build，需要在根目录下也放一个build.gradle，和一个settings.gradle
> 
> 一个Project是由若干tasks来组成的，当gradlexxx的时候，实际上是要求gradle执行xxx任务。这个任务就能完成具体的工作。

### 常用命令

- ./gradlew projects

查看project数目

- ./gradlew :tasks

查看所有的任务

```
------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Android tasks
-------------
androidDependencies - Displays the Android dependencies of the project.
signingReport - Displays the signing info for each variant.
sourceSets - Prints out all the source sets defined in this project.

Build tasks
-----------
assemble - Assembles all variants of all applications and secondary packages.
assembleAndroidTest - Assembles all the Test applications.
assembleDebug - Assembles all Debug builds.
assembleRelease - Assembles all Release builds.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
clean - Deletes the build directory.
compileDebugAndroidTestSources
compileDebugSources
compileDebugUnitTestSources
compileReleaseSources
compileReleaseUnitTestSources
mockableAndroidJar - Creates a version of android.jar that's suitable for unit tests.

Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'GradleStudy'.
components - Displays the components produced by root project 'GradleStudy'. [incubating]
dependencies - Displays all dependencies declared in root project 'GradleStudy'.
dependencyInsight - Displays the insight into a specific dependency in root project 'GradleStudy'.
help - Displays a help message.
model - Displays the configuration model of root project 'GradleStudy'. [incubating]
projects - Displays the sub-projects of root project 'GradleStudy'.
properties - Displays the properties of root project 'GradleStudy'.
tasks - Displays the tasks runnable from root project 'GradleStudy' (some of the displayed tasks may belong to subprojects).

Install tasks
-------------
installDebug - Installs the Debug build.
installDebugAndroidTest - Installs the android (on device) tests for the Debug build.
uninstallAll - Uninstall all applications.
uninstallDebug - Uninstalls the Debug build.
uninstallDebugAndroidTest - Uninstalls the android (on device) tests for the Debug build.
uninstallRelease - Uninstalls the Release build.

Verification tasks
------------------
check - Runs all checks.
connectedAndroidTest - Installs and runs instrumentation tests for all flavors on connected devices.
connectedCheck - Runs all device checks on currently connected devices.
connectedDebugAndroidTest - Installs and runs the tests for debug on connected devices.
deviceAndroidTest - Installs and runs instrumentation tests using all Device Providers.
deviceCheck - Runs all device checks using Device Providers and Test Servers.
lint - Runs lint on all variants.
lintDebug - Runs lint on the Debug build.
lintRelease - Runs lint on the Release build.
test - Run unit tests for all variants.
testDebugUnitTest - Run unit tests for the debug build.
testReleaseUnitTest - Run unit tests for the release build.

Other tasks
-----------
clean
jarDebugClasses
jarReleaseClasses
transformResourcesWithMergeJavaResForDebugUnitTest
transformResourcesWithMergeJavaResForReleaseUnitTest

```


- ./gradlew build

项目代码编译

- ./gradlew clean

清除项目build临时文件

- ./gradlew assemble

打包命令，会生成两个debug和release两种类型的apk，
如果要单独生成指定类型apk. 可执行 `./gradlew assembleRelease` 
和 `./gradlew assembleDebug` 两个命令


- ./gradlew androidDependencies

显示项目所有的依赖包


### 常用命令参数

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

gradle可以通过命令行传递任意参数，例如

```
> ./gradlew assembleRelease -Psuffix=V1

```

gradle通过 `-P + 参数名称 + 参数内容` 的方式传递给project对象。
然后在build.gradle中根据参数名称，获取到属性值。

除了命令行传参之外，还有另外一种传递参数的方式。

```
//在项目的gradle.properties中添加如下代码
suffix=V1

```


### 通过Android Studio执行Task

Android Studio在界面最右侧，有个Gradle Project浮动窗，
打开后，里面将项目中所有的task都放在了里面，双击即可运行。

![](http://o7y1sf21i.bkt.clouddn.com/blog/011/lALOaFpKs80CXM0CWw_603_604.png)



## 总结

本章主要介绍了Gradle的基础知识，以及整个Android Studio结构，看完应该会对整个Android Studio的构建过程有些了解了。




## 参考资料

[Gradle for Android 第一篇( 从 Gradle 和 AS 开始 )](https://segmentfault.com/a/1190000004229002)

[Gradle Recipes for Android](https://gradle.org/getting-started-android-build/)

[深入理解Android之Gradle](http://blog.csdn.net/innost/article/details/48228651)

