# User-Space Drivers

# 用户层驱动

To allow app developers to register new device drivers with the framework, Android Things introduces the concept of a **_user driver_**. User drivers are components registered from within apps that extend existing Android framework services. They allow any application to inject hardware events into the framework that other apps can process using the standard Android APIs.

为了让应用开发者们在 framework 内注册新的设备驱动，Android Things 引入了**用户驱动**这一概念。用户驱动即那些继承自 Android Framework 中已存在服务，并且在应用内部注册的组件。它能够让任何应用注入硬件事件到 framework 中，于是其它应用能够使用标准的 Android API 来处理这些事件。

![](https://developer.android.google.cn/things/images/driver-stack.png)

<aside class="note">**Note:** <span>You cannot customize the behavior of device drivers in the [Linux kernel](https://source.android.google.cn/devices/#Linux%20kernel) or [Hardware Abstraction Layer](https://source.android.google.cn/devices/#Hardware%20Abstraction%20Layer) (HAL) to add new functionality to a device.</span></aside>

<aside class="note">**注意:** <span>你不能在 [Linux kernel](https://source.android.google.cn/devices/#Linux%20kernel) 中定制设备驱动的行为或者在 [硬件抽象层](https://source.android.google.cn/devices/#Hardware%20Abstraction%20Layer) (HAL) 为设备添加新的功能。</span></aside>

## Benefits

## 优势

* * *

In many apps, using [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) to communicate directly with external hardware devices is sufficient. However, there are some benefits to integrating your hardware with the rest of the Android framework:

在许多应用中，使用 [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) 来和外部硬件进行通信是完全足够的。然而，集成你的硬件到 Android framework 中是有一些好处的：

*   **Portability**: Application code that purely targets the Android framework can run on a variety of different boards and configurations without additional abstractions for the device driver implementation.

*   **可移植性**: 纯粹面向 Android framework 的应用代码可以在许多不同的开发板上运行，且不需要额外地对设备驱动的实现进行抽象。


*   **Reuse**: You can pull existing Android code snippets and libraries into your application without the need to modify or fork them to handle your specific hardware implementation.

*   **重用性**: 你可以直接使用已有的 Android 代码段或库到你的应用中，不需要经过修改或者复制它们来处理对于特定硬件的实现。


*   **Integration**: Android often combines data from various services together to enhance the information reported to apps or create new virtual data sets. User drivers can contribute to this process.

*   **集成性**: Android 经常整合各种服务中的数据到一起，以便更好地将信息上报到应用中，或者创建虚拟的数据集。用户驱动能够对这方面起到帮助。

## Device Driver Types

## 设备驱动类型

* * *

**[GPS](https://developer.android.google.cn/things/sdk/drivers/gps.html)** - GPS provides high accuracy physical location information to apps. Integrating the location results from GPS devices with a user driver allows the framework to connect that data with other location sources, such as WiFi, and Google's [Fused Location Provider](https://developers.google.cn/android/reference/com/google/android/gms/location/FusedLocationProviderApi).

**[GPS](https://developer.android.google.cn/things/sdk/drivers/gps.html)** - GPS 为应用提供高精度的地理位置信息。使用用户驱动从 GPS 设备集成位置信息，能够使 framework 像连接其它位置源的数据一样。例如 WiFi, 或者 Google 的 [Fused Location Provider](https://developers.google.cn/android/reference/com/google/android/gms/location/FusedLocationProviderApi).

**[HID](https://developer.android.google.cn/things/sdk/drivers/input.html)** - Human Interface Devices (HID) provide user input to apps. Touch pads, keyboards, and game controllers are all examples of devices that provide this type of input. Input user drivers let devices interact with the enhanced input framework APIs, such as [Gesture Support](https://developer.android.google.cn/training/gestures/index.html) or [Drag and Drop](https://developer.android.google.cn/guide/topics/ui/drag-drop.html).

**[HID](https://developer.android.google.cn/things/sdk/drivers/input.html)** - 人体接口设备 (HID) 提供人体输入给应用。触控板，键盘以及其它游戏控制器是提供此类型输入的一些例子。 “输入用户驱动” 能够让设备与加强的输入框架 API 间进行交互, 例如 [Gesture Support](https://developer.android.google.cn/training/gestures/index.html) or [Drag and Drop](https://developer.android.google.cn/guide/topics/ui/drag-drop.html).

**[Sensors](https://developer.android.google.cn/things/sdk/drivers/sensors.html)** - Sensors measure and report the conditions of the physical environment. The Android sensor framework implements **_sensor fusion_** to combine the raw data from multiple physical sensors into a single virtual sensor. This is particularly common with motion sensors, such as accelerometers and gyroscopes. Connecting your sensor to the framework with a user driver allows the data it produces to be included in sensor fusion.

**[传感器](https://developer.android.google.cn/things/sdk/drivers/sensors.html)** - 传感器测量并上报物理环境的情况。Android 的传感器框架实现了**传感器融合**，即将各种物理传感器中的原始数据合并到一个虚拟传感器上。这与运动传感器，例如加速度计和陀螺仪的做法部分类似。使用用户驱动连接传感器到 framework 中能够让传感器上报的数据被包含进融合传感器中。

**[Audio](https://developer.android.google.cn/things/sdk/drivers/audio.html)** - The Android media framework provides a rich API surface for audio playback and recording. Using audio drivers, you can extend the set of input/output devices available to Android. Your app can also define which audio device should be considered the default.

**[声音](https://developer.android.google.cn/things/sdk/drivers/audio.html)** - Android 媒体框架提供了大量 API 以供播放声音和录音。使用声音驱动后，你能够以 Android 中的方式使用这些输入/输出设备。你的应用还能确定哪一个设备作为默认设备。

## Suggested Usage

## 建议用法

* * *

If you separate driver functionality into multiple apps, use foreground services for all apps except the primary `IOT_LAUNCHER` target. In doing so, you avoid limitations placed on background apps. For more information, see the [Background Execution Limits](https://developer.android.google.cn/preview/features/background.html) guide.

如果你将驱动的功能分割到多个应用中，那么需要在除了首要 `IOT_LAUNCHER` 之外的每个应用中使用前台服务，。 这样做之后，你就能避免后台应用的一些限制。详情请见[后台运行限制](https://developer.android.google.cn/preview/features/background.html)指导.

