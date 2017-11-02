# Peripheral Driver Library


The Peripheral Driver Library offers support in integrating common sensors and actuators into your app. The library contains prewritten user drivers for popular peripherals available for supported Android Things hardware.

![](https://developer.android.google.cn/things/images/driver-library.png)

Building apps for Android Things involves integrating with common hardware components and peripherals. Many of these peripherals utilize additional protocols on top of the low-level [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) that is specific to their use case. Implementing these protocols can be time consuming and error prone.

The goal of the Peripheral Driver Library is to simplify the integration of these hardware components into an Android Things app. It exposes a high level API that abstracts the communication logic and state management of each peripheral. For supported types, the library also connects the peripherals with the Android framework through the [User Driver](https://developer.android.google.cn/things/sdk/drivers/index.html) APIs.

The Peripheral Driver Library is open-sourced and available on [GitHub](https://github.com/androidthings/contrib-drivers). For examples using each driver in your apps, see the [driver samples](https://github.com/androidthings/drivers-samples).

