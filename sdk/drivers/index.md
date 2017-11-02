# User-Space Drivers

To allow app developers to register new device drivers with the framework, Android Things introduces the concept of a **_user driver_**. User drivers are components registered from within apps that extend existing Android framework services. They allow any application to inject hardware events into the framework that other apps can process using the standard Android APIs.

![](https://developer.android.google.cn/things/images/driver-stack.png)

<aside class="note">**Note:** <span>You cannot customize the behavior of device drivers in the [Linux kernel](https://source.android.google.cn/devices/#Linux%20kernel) or [Hardware Abstraction Layer](https://source.android.google.cn/devices/#Hardware%20Abstraction%20Layer) (HAL) to add new functionality to a device.</span></aside>

## Benefits

* * *

In many apps, using [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) to communicate directly with external hardware devices is sufficient. However, there are some benefits to integrating your hardware with the rest of the Android framework:

*   **Portability**: Application code that purely targets the Android framework can run on a variety of different boards and configurations without additional abstractions for the device driver implementation.
*   **Reuse**: You can pull existing Android code snippets and libraries into your application without the need to modify or fork them to handle your specific hardware implementation.
*   **Integration**: Android often combines data from various services together to enhance the information reported to apps or create new virtual data sets. User drivers can contribute to this process.

## Device Driver Types

* * *

**[GPS](https://developer.android.google.cn/things/sdk/drivers/gps.html)** - GPS provides high accuracy physical location information to apps. Integrating the location results from GPS devices with a user driver allows the framework to connect that data with other location sources, such as WiFi, and Google's [Fused Location Provider](https://developers.google.cn/android/reference/com/google/android/gms/location/FusedLocationProviderApi).

**[HID](https://developer.android.google.cn/things/sdk/drivers/input.html)** - Human Interface Devices (HID) provide user input to apps. Touch pads, keyboards, and game controllers are all examples of devices that provide this type of input. Input user drivers let devices interact with the enhanced input framework APIs, such as [Gesture Support](https://developer.android.google.cn/training/gestures/index.html) or [Drag and Drop](https://developer.android.google.cn/guide/topics/ui/drag-drop.html).

**[Sensors](https://developer.android.google.cn/things/sdk/drivers/sensors.html)** - Sensors measure and report the conditions of the physical environment. The Android sensor framework implements **_sensor fusion_** to combine the raw data from multiple physical sensors into a single virtual sensor. This is particularly common with motion sensors, such as accelerometers and gyroscopes. Connecting your sensor to the framework with a user driver allows the data it produces to be included in sensor fusion.

**[Audio](https://developer.android.google.cn/things/sdk/drivers/audio.html)** - The Android media framework provides a rich API surface for audio playback and recording. Using audio drivers, you can extend the set of input/output devices available to Android. Your app can also define which audio device should be considered the default.

## Suggested Usage

* * *

If you separate driver functionality into multiple apps, use foreground services for all apps except the primary `IOT_LAUNCHER` target. In doing so, you avoid limitations placed on background apps. For more information, see the [Background Execution Limits](https://developer.android.google.cn/preview/features/background.html) guide.

