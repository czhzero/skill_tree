

# Gradle DSL 介绍

Gradle脚本属于项目配置脚本，Gradle基于Groovy，Groovy又基于Java。所以，Gradle执行的时候和Groovy一样，会把脚本转换成Java对象。Gradle主要有三种对象，这三种对象和三种不同的脚本文件对应，在gradle执行的时候，会将脚本转换成对应的对端：

| 脚本类型 | 代理对象 | 描述 |
| ----- | ------ | ----- |
| Build script | [Project](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html) | 每一个build.gradle会转换成一个Project对象 |
|Init script|[Gradle](https://docs.gradle.org/current/dsl/org.gradle.api.invocation.Gradle.html)| gradle会从默认的配置脚本中构造出一个Gradle对象。在整个执行过程中，只有这么一个对象。 |
|Settings script|[Settings](https://docs.gradle.org/current/dsl/org.gradle.api.initialization.Settings.html)| 每一个settings.gradle都会转换成一个Settings对象 |

代理对象生成后，代理对象的属性和方法在脚本本中均可被使用。而且，Gradle脚本也可以执行常见Bash脚本命令，例如 `copy` `delete` `exec` 等命令。


更多内容，请查看 [Gradle Build Language Reference](https://docs.gradle.org/current/dsl/)





# Android Plugin DSL 介绍

前面说过，新建的Android Studio项目，会自动生成两个build.gradle。 build.gradle 第一句都是`apply plugin : *** ` , android插件的相关属性与方法均在`android`标签下。

```
apply plugin: 'com.android.application'

android {

   compileSdkVersion 23           #必选属性
   buildToolsVersion "23.0.3"     #必选属性

}

```

Android插件包括三种包类型,

```
apply plugin: 'com.android.application' #App Extension
apply plugin: 'com.android.library'     #Library Extension
apply plugin: 'com.android.test'        #Test Extension

```


更多内容,请查看 [Android Plugin DSL Reference](http://google.github.io/android-gradle-dsl/current)

## 属性列表

| 属性名称 | 描述 |
| :------ | :----- |
| aaptOptions | Options for aapt, tool for packaging resources.|
| adbOptions | Adb options|
|applicationVariants|The list of Application variants. Since the collections is built after evaluation, it should be used with Gradle's all iterator to process future items.|
|buildToolsVersion|Required. Version of the build tools to use.|
|buildTypes|Build types used by this project.|
|compileOptions|Compile options|
| compileSdkVersion	| Required. Compile SDK version. |
| dataBinding | Data Binding options |
| defaultConfig | Default config, shared by all flavors. |
|  defaultPublishConfig	| Name of the configuration used to build the default artifact of this project. |
| dexOptions | Dex options. |
| flavorDimensionList | The names of flavor dimensions. |
| generatePureSplits | Whether to generate pure splits or multi apk |
| jacoco | JaCoCo options. |
| lintOptions	| Lint options. |
| ndkDirectory | The NDK directory used. |
| packagingOptions | Packaging options. |
| productFlavors | All product flavors used by this project. |
| publishNonDefault	| Whether to publish artifacts for all configurations, not just the default one. |
| resourcePrefix  |  A prefix to be used when creating new resources. Used by Studio |
| sdkDirectory |  The SDK directory used.  |
| signingConfigs | 	Signing configs used by this project. |
| sourceSets |  All source sets. Note that the Android plugin uses its own implementation of source sets, AndroidSourceSet. |
| splits | APK splits |
| testOptions | Options for running tests. |
| testVariants | The list of (Android) test variants. Since the collections is built after evaluation, it should be used with Gradle's all iterator to process future items. |
| variantFilter | Callback to control which variants should be excluded. |

## 方法列表

| 方法名称 | 描述 |
| :------ | :----- |
| flavorDimensions(dimensions) | Specifies names of flavor dimensions. |
| useLibrary(name) | Request the use a of Library. The library is then added to the classpath. |
| useLibrary(name, required) | Request the use a of Library. The library is then added to the classpath.|



## 常见属性介绍

### compileSdkVersion 

设置代码编译的Android Sdk版本

### buildToolsVersion

设置build tools版本

### compileOptions

设置编译选项, compileOptions包含三个属性。

```

encoding					//设置Jave源码的编码格式，一般为utf-8

sourceCompatibility	   //Language level of the java source code.

targetCompatibility    //Version of the generated Java bytecode.

```

### defaultConfig

defaultConfig属性比较多，通常包含以下几个,

```

applicationId      //应用ID
versionCode        //App版本号
versionName        //App版本名称
minSdkVersion      //最小sdk版本
targetSdkVersion	   //目标sdk版本

```

### buildTypes



### dexOptions

### lintOptions

### signingConfigs

### productFlavors

### applicationVariants

### libraryVariants

### sourceSets





