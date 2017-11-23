# Peripheral Driver Library

# 外围设备驱动程序库


The Peripheral Driver Library offers support in integrating common sensors and actuators into your app. The library contains prewritten user drivers for popular peripherals available for supported Android Things hardware.

外围设备驱动库提供支持使得你的应用程序集成了通用传感器和执行器结合。这个库包含为大部分常见外围设备预先写好的驱动以支持 Android Things 硬件。

![](https://developer.android.google.cn/things/images/driver-library.png)

Building apps for Android Things involves integrating with common hardware components and peripherals. Many of these peripherals utilize additional protocols on top of the low-level [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) that is specific to their use case. Implementing these protocols can be time consuming and error prone.

构建 Android Things 应用程序牵扯到使普通硬件组件与外围设备结合。这其中许多外围设备针对它们自身的用例初始化在底层之上的附加协议 [外设 I/O](https://developer.android.google.cn/things/sdk/pio/index.html) 。实现这些协议既费时又容易出错。

The goal of the Peripheral Driver Library is to simplify the integration of these hardware components into an Android Things app. It exposes a high level API that abstracts the communication logic and state management of each peripheral. For supported types, the library also connects the peripherals with the Android framework through the [User Driver](https://developer.android.google.cn/things/sdk/drivers/index.html) APIs.

外围设备驱动程序库的目标是简化这些硬件组件结合到 Android Things 应用程序中。 它公开了一套针对每个外设接口通讯逻辑及状态管理进行抽象的高级接口。对于支持的类型，库还通过 [用户驱动程序](https://developer.android.google.cn/things/sdk/drivers/index.html) 接口将外围设备与 Android 框架链接起来。

The Peripheral Driver Library is open-sourced and available on [GitHub](https://github.com/androidthings/contrib-drivers). For examples using each driver in your apps, see the [driver samples](https://github.com/androidthings/drivers-samples).

外围设备驱动程序库在 [GitHub](https://github.com/androidthings/contrib-drivers) 是开源且可用的。在你的应用程序中使用任意一个驱动程序的例子，请参照 [驱动程序示例](https://github.com/androidthings/drivers-samples) 。
