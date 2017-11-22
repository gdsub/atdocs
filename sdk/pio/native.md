# Native PIO

# 本地 PIO


The native PIO APIs let you write C/C++ code to control GPIO, PWM, I<sup>2</sup>C, SPI and UART peripherals that access the same underlying peripheral service as the standard PIO APIs. This lets you write an Android Things app purely in C/C++ or extend a Java-based Android Things app with C or C++ code that uses the native PIO APIs (for example, porting existing drivers written for other embedded platforms).

与通用输入输出（GPIO）应用开发接口一样，本地输入输出（native PIO） API 可以允许你通过 C/C++ 代码去操作 GPIO, PWM, I<sup>2</sup>C, SPI 和 UART 这些底层外围服务。这样你就可以用完全的C/C++语言来开发Android Things应用，或者是把C/C++代码做为以Java为基础的Android Things应用的扩展(例如，从其它的嵌入式平台上导入已经存在的驱动程序)。

## Getting started with the NDK

## 开始 NDK

* * *

If you've never used the NDK, see the [Android NDK Getting Started](https://developer.android.google.cn/ndk/guides/index.html#download-ndk) guide to download and install the NDK. The documentation also has detailed information on how to use the NDK.

如果你从来没用过NDK， 可以看看 [开始Android NDK ](https://developer.android.google.cn/ndk/guides/index.html#download-ndk) 来下载并且安装 NDK。链接文档也详细介绍了怎么使用NDK。

## Get the Android Things native library

## 获得 Android Things 本地开发库

* * *

The native PIO APIs are available in the [Android Things native library](https://github.com/androidthings/native-libandroidthings/releases). You copy the entire directory into the root directory of your Android Studio project. The directory structure looks like this:

本地 PIO 应用开发接口可以从 [Android Things 本地开发库](https://github.com/androidthings/native-libandroidthings/releases) 获得。你可以把整个目录拷贝到Android Studio的项目的根目录下面。整个目录的结构如下所示：

`    libandroidthings/  ${ABI}/    include/      pio/        *.h    lib/      libandroidthings.so `

You'll include the header files in the `include/pio` directory to compile your app and link the `libandroidthings.so` shared object for the appropriate ABI when you package your app. The [`FindAndroidThings.cmake`](https://github.com/androidthings/native-libandroidthings/tree/master/FindAndroidThings.cmake) CMake module file is also available to help you configure new NDK projects to use the Android Things native library.

开发过程中，需要包含头文件 `include/pio` 目录并且链接 `libandroidthings.so` 使得在打包你的应用的时候，能自动去适配应用程序二进制接口 (ABI)。[`FindAndroidThings.cmake`](https://github.com/androidthings/native-libandroidthings/tree/master/FindAndroidThings.cmake) CMake 模块也可以帮助你配置新的 NDK 项目来使用 Android Things 的本地开发库。

See the header files for the library in the [Github repository](https://github.com/androidthings/native-libandroidthings/tree/master/x86/include/pio) for more information and documentation.

从 [Github 仓库](https://github.com/androidthings/native-libandroidthings/tree/master/x86/include/pio) 中可以找到关于开发库的头文件的更多信息和文档。

## Native PIO sample

## 本地 PIO 示例

* * *

The native PIO sample (see [Github repository](https://github.com/androidthings/sample-nativepio)) shows you how to blink an LED, get input from a button, and actuate a PWM speaker by calling the native PIO APIs inside a [NativeActivity](https://developer.android.google.cn/reference/android/app/NativeActivity.html), which lets you create an activity using only C/C++.

本地的 PIO [Github 仓库](https://github.com/androidthings/sample-nativepio) 提供了仅仅使用C/C++语言来创建 Activity [NativeActivity](https://developer.android.google.cn/reference/android/app/NativeActivity.html)，通过 PIO 点亮LED， 从按钮获得输入信息，操作PWM扬声器发声等示例。

To run the sample:

运行示例：

* Clone or download the sample from [Github](https://github.com/androidthings/sample-nativepio).

* 从 [Github](https://github.com/androidthings/sample-nativepio) 下载示例代码。

* See the `README.md` file for prerequisites to running the sample.

* 阅读 README.md 文档并做好运行代码前的准备工作。

* Extract or copy the native PIO libraries into the project's root directory.

* 解压缩或者是拷贝本地的 PIO 库文件到项目的根目录下。

* Connect your device to the development machine and run one of the sample's modules: blink, button, or speaker.

* 用开发用的电脑连接外设，然后运行示例中的 blink, button 或者是 speaker 模块。


* **In Android Studio**, select the module in the drop-down menu by the **Run** button, then click **Run** button.

* **在Android Studio中**, 从 **Run** 按钮旁边的下拉菜单中选择模块, 然后点击 **Run** 按钮。

* **On the command line**, run the following commands from your project root directory:

* **在命令行中**, 在项目的根目录下运行下列命令:
```
  ./gradlew [blink|button|speaker]:installDebugadb shell am start com.example.androidthings.nativepio/android.app.NativeActivity
```

