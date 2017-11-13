# Peripheral Driver Library

# 外围设备驱动程序库


The Peripheral Driver Library offers support in integrating common sensors and actuators into your app. The library contains prewritten user drivers for popular peripherals available for supported Android Things hardware.

外围设备驱动程序库提供支持使得通用传感器和执行器结合到你的应用程序。这个库包含为流行外围设备预先写好的驱动以支持Android Things硬件。

![](https://developer.android.google.cn/things/images/driver-library.png)

Building apps for Android Things involves integrating with common hardware components and peripherals. Many of these peripherals utilize additional protocols on top of the low-level [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) that is specific to their use case. Implementing these protocols can be time consuming and error prone.

构建Android Things应用程序牵扯到使普通硬件组件与外围设备结合。这其中的的许多外围设备针对它们的用力利用在底层之上的附加协议[Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html)。实现这些协议既费时又容易出错。

The goal of the Peripheral Driver Library is to simplify the integration of these hardware components into an Android Things app. It exposes a high level API that abstracts the communication logic and state management of each peripheral. For supported types, the library also connects the peripherals with the Android framework through the [User Driver](https://developer.android.google.cn/things/sdk/drivers/index.html) APIs.

外围设备驱动程序库的目标是简化这些硬件组件结合到Android Things应用程序中。 它公开了一套针对每个外设接口通讯逻辑及状态管理进行抽象的高级接口。对于支持的类型，库还通过 [User Driver](https://developer.android.google.cn/things/sdk/drivers/index.html)接口将外围设备与Android框架链接起来。

The Peripheral Driver Library is open-sourced and available on [GitHub](https://github.com/androidthings/contrib-drivers). For examples using each driver in your apps, see the [driver samples](https://github.com/androidthings/drivers-samples).

外围设备驱动程序库在 [GitHub](https://github.com/androidthings/contrib-drivers)是开源切可用的。在你的应用程序中使用每个驱动程序的例子，看[driver samples](https://github.com/androidthings/drivers-samples)。
