# Getting Started with the SDK Preview


Welcome to the Android Things SDK Preview! Android Things development is very similar to traditional Android mobile development and involves writing apps using the Android framework and tools. All you need is a development board flashed with the Android Things OS and the required peripherals for your device.

This guide gives you all the information you need to get started quickly with a supported board and set up your initial development environment.

## Get Familiar with Android Development

* * *

If you've never developed an Android app, start by building your first Android mobile app. The basic concepts and general workflow of core Android development transfer over well to Android Things development. If you don't have a mobile device, the Android SDK comes with a software emulator.

See [Building Your First App](https://developer.android.google.cn/training/basics/firstapp/index.html) in the Android OS documentation to get started and come back to Android Things when you're ready.

## Understand the Android Things Platform

* * *

Because Android Things has a few key differences compared to the core Android OS, read the [Overview](https://developer.android.google.cn/things/sdk/index.html) to understand key concepts that you'll need to understand.

## Get Hardware

* * *

Before you begin, you need a supported development board. You can compare the available boards on the [Developer Kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

## Flash Android Things

* * *

Once you select a board, flash and bring up your hardware with the instructions in the Hardware Getting Started guide for your particular board:

*   [NXP i.MX7D](https://developer.android.google.cn/things/hardware/imx7d.html)
*   [NXP i.MX6UL](https://developer.android.google.cn/things/hardware/imx6ul.html)
*   [Raspberry Pi 3](https://developer.android.google.cn/things/hardware/raspberrypi.html)
*   [Intel Edison](https://developer.android.google.cn/things/hardware/edison.html)
*   [Intel Joule](https://developer.android.google.cn/things/hardware/joule.html)

## Set Up Your Development Environment

* * *

1.  [Download](https://developer.android.google.cn/studio/index.html) or update to the latest version of Android Studio.
2.  Open Android Studio and start a new project. In the new project wizard, keep the default settings except for the form factors:
    *   Select **Android Things** as the form factor on which to run your application.
    *   Select **API 26: Android 8.0 (Oreo)**.
3.  Connect your board and verify you can access the device via `adb`:

        $ adb devicesList of devices attached4560736843791520041    device

4.  [Deploy](https://developer.android.google.cn/studio/run/index.html) the sample project to your board and verify that you can see the activity messages with `logcat`.

## Build your First Device

* * *

Now that you have your environment set up, see the [Building Your First Device](https://developer.android.google.cn/things/training/first-device/index.html) training class to start developing for Android Things.

## More samples

* * *

For assistance building more complex applications with Android Things, review additional examples in the [Samples](https://developer.android.google.cn/things/sdk/samples.html) section.

