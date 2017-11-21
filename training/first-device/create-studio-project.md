# Create an Android Things Project

# 创建一个 Android Things 项目

## This lesson teaches you to

## 这门课让你学会

1.  Create a Things Project

1. 创建一个 Android Things 项目

2.  Create a Home Activity

2. 创建一个主页面

## You should also read

## 你需要去阅读

*   [Projects Overview](https://developer.android.google.cn/studio/projects/index.html)

* [项目概览](https://developer.android.google.cn/studio/projects/index.html)


Things apps use the same structure as those designed for phones and tablets. This similarity means you can modify your existing apps to also run on embedded things or create new apps based on what you already know about building apps for Android.

Android Things 应用的结构和为手机或平板所设计的应用的结构一样。它们之间的相似性意味着你可以把你现存的应用修改并移植在嵌入式 Android Things 设备上，或者是可以基于你对 Android 应用开发的认知来创建一个全新的 Android Things 应用。

This lesson describes how to prepare your development environment for Android Things, and the required changes to enable app to run on embedded things.

这节课将描述如何准备 Android Things 开发环境，以及一些让应用可以在嵌入式设备上运行所必须的修改项。

## 前提

* * *

Before you begin building apps for Things, you must:

在你为构建 Android Things 应用之前，你必须

*   [Update your SDK tools to version 25.0.3 or higher](https://developer.android.google.cn/studio/intro/update.html#sdk-manager) The updated SDK tools enable you to build and test apps for Things.

*   [更新你的 SDK 工具版本至 25.0.3 或者更高](https://developer.android.google.cn/studio/intro/update.html#sdk-manager) 更新后的 SDK 工具可以让你构建并测试 Android Things 应用

*   [Update your SDK with Android 8.0 (Oreo), API 26 or higher](https://developer.android.google.cn/studio/intro/update.html#sdk-manager) The updated platform version provides new APIs for Things apps.

*   [更新你的 SDK 至 Android 8.0 (Oreo) 所支持的 API 26 或更高版本](https://developer.android.google.cn/studio/intro/update.html#sdk-manager) 更新后的平台将会为 Android Things .

## 开始

* * *

To get started quickly, [create your app project](https://developer.android.google.cn/studio/projects/create-project.html) using the new project wizard:

你可以使用这个新建项目引导来快速创建[应用项目](https://developer.android.google.cn/studio/projects/create-project.html)：

*   Select **Android Things** as the only form factor.

* 选择项目类型为 **Android Things**。

*   In order to access new APIs for Things, target **Android 8.0 (Oreo)**, API level 26 or higher.

* 为了使用最新的 API ，请确保构建目标为 **Android 8.0 (Oreo)**, API 26 ，或者更高。

*   Name the new empty activity **HomeActivity**.

* 将新建的空白页面命名为 **HomeActivity**。


The new project wizard automatically adds two necessary items to your Android Studio project: a dependency to the Android Things support [library](#library) and a default launch [activity](#activity).

新建项目引导将自动为你的 Android Studio 项目添加两个必须项: 一个是 Android Things 中的[支持库](#library) 的依赖关系，以及一个默认[页面](#activity).

## 添加库

* * *

Android Things devices expose APIs through support libraries that are not part of the Android SDK. The new project wizard automatically adds a dependency to the support library to your app-level `build.gradle` file:

Android Things 利用不属于 Android SDK 一部分的支持库来暴露 API 接口。新建项目引导将自动在应用层的 `build.gradle` 添加支持库的依赖关系。

```
dependencies {
    compileOnly 'com.google.android.things:androidthings:+'    
}
```

The wizard adds `<uses-library>` to your app's manifest file to make this prebuilt library available to the app's classpath at run time.

该项目引导将 `<uses-library>` 添加到应用的 `mainfest` 文件中，以确保这些预构建的库在应用运行时处于可用状态。

## 添加一个主页面

* * *

An application intending to run on an embedded device must declare an activity in its manifest as the main entry point after the device boots. Note that the wizard applies an intent filter containing the following attributes:

一个为嵌入式设备所构建的应用，需要在其 `mainfest` 文件中指定一个主页面作为设备启动后的入口。请注意，向导通过包含下面这些属性去申请一个 `intent filter` ：(请校对者帮忙参考)

*   **Action**: [ACTION_MAIN](https://developer.android.google.cn/reference/android/content/Intent.html#ACTION_MAIN)
*   **Category**: [CATEGORY_DEFAULT](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_DEFAULT)
*   **Category**: `IOT_LAUNCHER`

For ease of development, this same activity includes a [CATEGORY_LAUNCHER](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_LAUNCHER) intent filter so Android Studio can launch it as the default activity when deploying or debugging.

为了便于开发, 相同的页面包括了这个 [CATEGORY_LAUNCHER](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_LAUNCHER) `intent filter` ， 所以在部署或者调试时，Android Studio 可以加载这个页面作为默认页面。

~~~xml

<application>    
    <uses-library android:name="com.google.android.things"/>    
    <activity android:name=".HomeActivity">        
        <!-- Launch activity as default from Android Studio -->        
        <intent-filter>            
            <action android:name="android.intent.action.MAIN"/>            
            <category android:name="android.intent.category.LAUNCHER"/>        
        </intent-filter>        
        <!-- Launch activity automatically on boot -->        
        <intent-filter>            
            <action android:name="android.intent.action.MAIN"/>            
            <category android:name="android.intent.category.IOT_LAUNCHER"/>            
            <category android:name="android.intent.category.DEFAULT"/>        
        </intent-filter>    
    </activity>
</application>

~~~

