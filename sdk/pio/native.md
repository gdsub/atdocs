# Native PIO


The native PIO APIs let you write C/C++ code to control GPIO, PWM, I<sup>2</sup>C, SPI and UART peripherals that access the same underlying peripheral service as the standard PIO APIs. This lets you write an Android Things app purely in C/C++ or extend a Java-based Android Things app with C or C++ code that uses the native PIO APIs (for example, porting existing drivers written for other embedded platforms).

## Getting started with the NDK

* * *

If you've never used the NDK, see the [Android NDK Getting Started](https://developer.android.google.cn/ndk/guides/index.html#download-ndk) guide to download and install the NDK. The documentation also has detailed information on how to use the NDK.

## Get the Android Things native library

* * *

The native PIO APIs are available in the [Android Things native library](https://github.com/androidthings/native-libandroidthings/releases). You copy the entire directory into the root directory of your Android Studio project. The directory structure looks like this:

    libandroidthings/  ${ABI}/    include/      pio/        *.h    lib/      libandroidthings.so

You'll include the header files in the `include/pio` directory to compile your app and link the `libandroidthings.so` shared object for the appropriate ABI when you package your app. The [`FindAndroidThings.cmake`](https://github.com/androidthings/native-libandroidthings/tree/master/FindAndroidThings.cmake) CMake module file is also available to help you configure new NDK projects to use the Android Things native library.

See the header files for the library in the [Github repository](https://github.com/androidthings/native-libandroidthings/tree/master/x86/include/pio) for more information and documentation.

## Native PIO sample

* * *

The native PIO sample (see [Github repository](https://github.com/androidthings/sample-nativepio)) shows you how to blink an LED, get input from a button, and actuate a PWM speaker by calling the native PIO APIs inside a [NativeActivity](https://developer.android.google.cn/reference/android/app/NativeActivity.html), which lets you create an activity using only C/C++.

To run the sample:

1.  Clone or download the sample from [Github](https://github.com/androidthings/sample-nativepio).
2.  See the `README.md` file for prerequisites to running the sample.
3.  Extract or copy the native PIO libraries into the project's root directory.
4.  Connect your device to the development machine and run one of the sample's modules: blink, button, or speaker.

    *   **In Android Studio**, select the module in the drop-down menu by the **Run** button, then click **Run** button.
    *   **On the command line**, run the following commands from your project root directory:

            ./gradlew [blink|button|speaker]:installDebugadb shell am start com.example.androidthings.nativepio/android.app.NativeActivity

