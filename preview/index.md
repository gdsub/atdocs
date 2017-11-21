# Getting Started with the SDK Preview

# SDK 预览版入门指南

Welcome to the Android Things SDK Preview! Android Things development is very similar to traditional Android mobile development and involves writing apps using the Android framework and tools. All you need is a development board flashed with the Android Things OS and the required peripherals for your device.

欢迎使用 Android Things SDK 预览版！Android Things 的开发与传统的 Android 开发非常相似，都需要应用 Android 框架和工具来编写应用程序。您只需要准备一块刷入 Android Thing 操作系统的开发板，和设备必需的外设即可。

This guide gives you all the information you need to get started quickly with a supported board and set up your initial development environment.

本指南会介绍到如何快速入门开发板，以及如何搭建开发环境。

## Get Familiar with Android Development

## 熟悉 Android 开发

* * *

If you've never developed an Android app, start by building your first Android mobile app. The basic concepts and general workflow of core Android development transfer over well to Android Things development. If you don't have a mobile device, the Android SDK comes with a software emulator.

如果您还没有过 Android 应用的开发经验，建议您先去构建您的第一个 Android 应用。Android 开发的核心基础知识和工作流程可以很好的迁移到 Android Things 的开发中来。如果您还没有一部移动设备，Android SDK 中会提供模拟器。

See [Building Your First App](https://developer.android.google.cn/training/basics/firstapp/index.html) in the Android OS documentation to get started and come back to Android Things when you're ready.

您可以在 Android 文档中从[构建您的第一个应用](https://developer.android.google.cn/training/basics/firstapp/index.html)开始入门。当您准备好了，再回到 Android Things 开发中来。

## Understand the Android Things Platform

## 熟悉 Android Things 平台

* * *

Because Android Things has a few key differences compared to the core Android OS, read the [Overview](https://developer.android.google.cn/things/sdk/index.html) to understand key concepts that you'll need to understand.

由于 Android Things 与 Android 系统有一些关键性的差异，请先阅读[概览](https://developer.android.google.cn/things/sdk/index.html)以理解必知的主要概念。

## Get Hardware

## 获得硬件支持

* * *

Before you begin, you need a supported development board. You can compare the available boards on the [Developer Kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

在开始之前，您还需要一块受支持的开发板。您可以在[开发者套件](https://developer.android.google.cn/things/hardware/developer-kits.html)页参照可用的开发板。

## Flash Android Things

## 刷入 Android Things

* * *

Once you select a board, flash and bring up your hardware with the instructions in the Hardware Getting Started guide for your particular board:

一旦您选择好了开发板，就可以按照相对应的硬件入门指南，刷入 Android Things。

*   [NXP i.MX7D](https://developer.android.google.cn/things/hardware/imx7d.html)
*   [NXP i.MX6UL](https://developer.android.google.cn/things/hardware/imx6ul.html)
*   [Raspberry Pi 3](https://developer.android.google.cn/things/hardware/raspberrypi.html)
*   [Intel Edison](https://developer.android.google.cn/things/hardware/edison.html)
*   [Intel Joule](https://developer.android.google.cn/things/hardware/joule.html)





- [NXP i.MX7D](https://developer.android.google.cn/things/hardware/imx7d.html)
- [NXP i.MX6UL](https://developer.android.google.cn/things/hardware/imx6ul.html)
- [Raspberry Pi 3](https://developer.android.google.cn/things/hardware/raspberrypi.html)
- [Intel Edison](https://developer.android.google.cn/things/hardware/edison.html)
- [Intel Joule](https://developer.android.google.cn/things/hardware/joule.html)

## Set Up Your Development Environment

## 搭建开发环境

* * *

1.  [Download](https://developer.android.google.cn/studio/index.html) or update to the latest version of Android Studio.

2.  Open Android Studio and start a new project. In the new project wizard, keep the default settings except for the form factors:
    *   Select **Android Things** as the form factor on which to run your application.
    *   Select **API 26: Android 8.0 (Oreo)**.

3.  Connect your board and verify you can access the device via `adb`:

        $ adb devicesList of devices attached4560736843791520041    device

4.  [Deploy](https://developer.android.google.cn/studio/run/index.html) the sample project to your board and verify that you can see the activity messages with `logcat`.



1. [下载](https://developer.android.google.cn/studio/index.html)或更新最新版本的 Android Studio。
2. 打开 Android Studio 并创建一个新项目。在新项目向导中，一切均可保持默认设置，但选择机型时需作如下更改：
    *   选择 **Android Things** 作为您运行应用程序的机型。
    *   选择 **API 26: Android 8.0 (Oreo)**。
3.  连接您的开发板，并使用 `adb` 命令验证您是否可以操作该设备：

        $ adb devicesList of devices attached4560736843791520041    device

4.  将示例项目[部署](https://developer.android.google.cn/studio/run/index.html)到您的开发板上，并验证是否可以在 `logcat` 中看到 Activity 的消息。

## Build your First Device

## 开发您的第一台设备

* * *

Now that you have your environment set up, see the [Building Your First Device](https://developer.android.google.cn/things/training/first-device/index.html) training class to start developing for Android Things.

现在您已经搭建好了开发环境。请阅读[开发您的第一台设备](https://developer.android.google.cn/things/training/first-device/index.html)培训教程，开始您的 Android Things 开发之旅吧。

## More samples

## 更多示例

* * *

For assistance building more complex applications with Android Things, review additional examples in the [Samples](https://developer.android.google.cn/things/sdk/samples.html) section.

为了帮助您能够开发出更强大的 Android Things 应用，可在[示例](https://developer.android.google.cn/things/sdk/samples.html)部分中查阅更多的示例。
