# Android插件DSL文档

前面说过，新建的Android Studio项目，会自动生成两个build.gradle。 build.gradle 第一句都是`apply plugin : *** `
Android插件包括三种包类型,

```
apply plugin: 'com.android.application' #App Extension
apply plugin: 'com.android.library'     #Library Extension
apply plugin: 'com.android.test'        #Test Extension

```

## 属性列表

| 属性 | 描述 |
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

| 方法 | 描述 |
| :------ | :----- |
| flavorDimensions(dimensions) | Specifies names of flavor dimensions. |
| useLibrary(name) | Request the use a of Library. The library is then added to the classpath. |
| useLibrary(name, required) | Request the use a of Library. The library is then added to the classpath.
|



## 常见用属性介绍

### buildToolsVersion 

### 



