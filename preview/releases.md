# Release Notes

## In this document

#发行说明

## 在这份文档中

1.  [Developer Preview 5.1](#preview-5-1)
2.  [Developer Preview 5](#preview-5)
3.  [Developer Preview 4.1](#preview-4-1)
4.  [Developer Preview 4](#preview-4)
5.  [Developer Preview 3](#preview-3)
6.  [Developer Preview 2](#preview-2)
7.  [Developer Preview 1](#preview-1)

1.  [开发者预览版 5.1](#preview-5-1)
2.  [开发者预览版 5](#preview-5)
3.  [开发者预览版 4.1](#preview-4-1)
4.  [开发者预览版 4](#preview-4)
5.  [开发者预览版 3](#preview-3)
6.  [开发者预览版 2](#preview-2)
7.  [开发者预览版 1](#preview-1)

This document outlines issues and fixes related to each release of the Android Things developer preview. We are committed to providing regular updates to developers, and aim to have new preview releases approximately every 6-8 weeks.

此文档列出了 Android Things 开发者预览版每一个版本的问题及修复。我们承诺为开发者提供定期更新，目标是每 6至8 周发布更新后的预览版本。

Please file tickets in the Android issue tracker for issues discovered in the system, hardware support, and documentation:

对于在系统、硬件支持和文档中发现的问题，请在 Android 问题跟踪系统中提交问题单。

*   [Android Things bug report](https://issuetracker.google.com/issues/new?component=192720&template=847005)
*   [Android Things feature request](https://issuetracker.google.com/issues/new?component=192720&template=848805)

*   [Android Things bug 报告](https://issuetracker.google.com/issues/new?component=192720&template=847005)
*   [Android Things 功能请求](https://issuetracker.google.com/issues/new?component=192720&template=848805)

<aside class="note">**Note:** <span>Before filing a bug, please check this page for known issues and ensure you are running the [latest version](https://partner.android.com/things/console) of the preview system image.</span></aside>

<aside class="note">**注意:** <span>在提交 bug 之前，请查看本页面核对已知问题，并确保您正在运行 [最新版本](https://partner.android.com/things/console) 的预览版系统镜像。</span></aside>

To ask questions and discuss ideas with other developers working on Android Things, join the [IoT Developers Google+ community](http://g.co/iotdev).

要与其他开发 Android Things 的开发者讨论问题或想法, 请加入 [Google+ 物联网开发者社区](http://g.co/iotdev).

## Developer Preview 5.1.1


## 开发者预览版 5.1.1

* * *

_Date: October 2017_  
_Build Number: OIR1.170720.018_  
_Play Services: 11.0.4_

_日期: 2017 年 10 月_  
_版本号: OIR1.170720.018_  
_Play 服务版本: 11.0.4_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Developer Preview 5.1.1 is available on **only** the NXP i.MX7D.

此预览版仅供**开发者和尝鲜者**用来开发和测试硬件平台的兼容性。请注意以下关于预览版的准则：

*   此版本在支持的硬件上可能存在**各种稳定性问题**。请记录并提交您发现的程序错误。
*   开发者预览版 5.1.1 **仅适用** 于 NXP i.MX7D.

### New in Preview 5.1.1

#### Bug Fixes

### 预览版 5.1.1 的更新

#### Bug 修复

*   Corrected an issue affecting camera initialization, causing the camera device to be inaccessible to apps.

*   修复了一个影响相机初始化并导致应用程序无法访问照相机设备的问题。

### Known Issues

### 已知问题

See the Known Issues list for [Developer Preview 5.1](#preview-5-1).

请参阅 [开发者预览版 5.1](#preview-5-1) 的已知问题列表

## Developer Preview 5.1

* * *

## 开发者预览版 5.1

* * *

_Date: August 2017_  
_Build Number: OIR1.170720.017_  
_Play Services: 11.0.4_

_日期: 2017 年 8 月_  
_版本号: OIR1.170720.017_  
_Play 服务版本: 11.0.4_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 5.1 is available on the NXP i.MX7D, NXP i.MX6UL, and Raspberry Pi 3 development boards.

此预览版仅供**开发者和尝鲜者**用来开发和测试硬件平台的兼容性。请注意以下关于预览版的准则：

*   此版本在支持的硬件上可能存在**各种稳定性问题**。请记录并提交您发现的程序错误。
*   不是所有的 API 都在这个预览版中启用。已知禁用的 API 在 "已知问题" 部分中有记录。
*   开发者预览版 5.1 适用于 NXP i.MX7D 、 NXP i.MX6UL 和 Raspberry Pi 3 开发板。

### New in Preview 5.1

### 预览版 5.1 的更新

#### Bug Fixes

*   Automatically detect display settings on Raspberry Pi 3 without the need for edits to the root `config.txt` file.
*   Remove shadowing artifacts observed when pop-up windows (such as dialogs) are shown.
*   Fixed an issue causing the cursor associated with a USB mouse to hide/show incorrectly.

#### Bug 修复

*  自动检测 Raspberry Pi 3 的显示设置，而无需编辑根 `config.txt` 文件。
*  移除弹出窗口（如对话框）的阴影效果。
*  修复了一个导致 USB 鼠标所关联的光标的正确显示、隐藏的问题。

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Input events from [user drivers](https://developer.android.google.cn/things/sdk/drivers/index.html) will not wake the display from sleep.
*   Devices cannot currently act as an A2DP audio source over Bluetooth.

### 已知问题

*   系统电源管理当前已被禁用。设备不会休眠挂起，也无需唤醒服务。
*   Google Play 服务 在第一次启动时需要 2-3 分钟的 DEX 预优化。优化完成后才能正常的安装应用。
*   来自 [用户驱动程序](https://developer.android.google.cn/things/sdk/drivers/index.html) 的输入事件不会从睡眠中唤醒显示屏。
*   设备当前不能通过蓝牙作为 A2DP 音频源。

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### 外设 I/O

*   在调用 `close()` 之后，外设不会被清除或重置。 输出将保留它们的状态, 并且串行端口可能继续传输以前缓冲的数据。
*   如果 GPIO 引脚在上次启动后作为边缘触发器的输入, 则 GPIO 引脚不能用作输出。

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### 用户驱动程序

*   当前用户传感器目前不能被手动注销。当应用程序进程终止时, 它们将自动注销。
*   用户传感器只支持 [连续变化传感器](https://source.android.google.cn/devices/sensors/report-modes.html)。单次和特殊报告模式无法正常工作。

#### Pico i.MX7D

*   OpenGL 2.0 is not yet fully supported. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.


#### Pico i.MX7D

*   OpenGL 2.0 仍未完美支持. 基于此功能的 API（例如 [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) 将无法使用。

#### Argon i.MX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.

#### Argon i.MX6UL

*   **USB:** 在设备板启动后插入的 USB 设备将不被识别。

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** Preview is currently slow to render when using a DSI display.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.

#### Raspberry Pi

*   **音频:** 当无线网络和蓝牙同时启用的时候，可能会出现音频质量问题。
*   **网络:** 如果以太网口连接到一个无互联网接入的网络，无线网络将也无法接入互联网。
*   **摄像头:** 当使用 DSI 显示屏时，显示渲染会变慢。
*   **摄像头:** 无法创建具有多个输出目标的 `CameraCaptureSession` 。
*   **摄像头:** 任何 `CameraCaptureSession` 的第一个请求总是队列两幅图像。这可能导致同一会话中的每个后续 `CaptureRequest` 返回先前捕获的缓冲帧。
*   **I/O:** GPIO 引脚 **BCM4** 、 **BCM5** 和 **BCM6** 在作为输入时会在内部拉伸至 3.3伏。

## Developer Preview 5

* * *

## 开发者预览版 5

* * *

_Date: August 2017_  
_Build Number: OIR1.170720.015_  
_Play Services: 11.0.4_


_日期: 2017 年 8 月_  
_版本号: OIR1.170720.015_  
_Play 服务版本: 11.0.4_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 5 is available on the NXP i.MX7D, NXP i.MX6UL, and Raspberry Pi 3 development boards.

此预览版仅供**开发者和尝鲜者**用来开发和测试硬件平台的兼容性。请注意以下关于预览版的准则：

*   此版本在支持的硬件上可能存在**各种稳定性问题**。请记录并提交您发现的程序错误。
*   不是所有的 API 都在这个预览版中启用。已知禁用的 API 在 "已知问题" 部分中有记录。
*   开发者预览版 5 适用于 NXP i.MX7D 、 NXP i.MX6UL 和 Raspberry Pi 3 开发板。

### New in Preview 5

### 预览版 5 的更新

#### Android O

#### Android O

Android O is currently under Developer Preview for phones and tablets, and DP5 is now based on this upcoming release. You should update your apps to target API level 26 to work correctly on the platform with our support libraries.

Android O 目前是为手机和平板电脑设计的开发者预览版, DP5 基于这个版本制作。您应该将应用程序目标 API 级别设置为 26 ，以便与支持库一起在平台上正常工作。

#### NXP i.MX6UL SprIoT

Android Things is now supported on the [NXP® SprIoT i.MX6UL](http://wireless.murata.com/eng/products/wireless-connectivity-platforms/iot-system-on-module/spriot-6ul.html) development platform. Learn more about this device and its capabilities on the [developer kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

#### NXP i.MX6UL SprIoT

Android Things 现在支持 [NXP® SprIoT i.MX6UL](http://wireless.murata.com/eng/products/wireless-connectivity-platforms/iot-system-on-module/spriot-6ul.html) 开发平台。详细了解此设备及其功能请访问 [开发工具包页面](https://developer.android.google.cn/things/hardware/developer-kits.html) 。

#### Legacy Board Support

With Intel discontinuing the Edison and Joule hardware designs, these platforms are moving to legacy support. They will not continue to receive the latest platform updates, but developers may still access the Preview 4.1 images through the [Android Things Console](https://partner.android.com/things/console).

#### 遗留主板支持

由于 Intel 停止对 Edison 和 Joule 硬件设计的支持，这些平台将作为遗留支持，将不会再收到最新的平台更新。如有需要，开发者仍可以通过 [Android Things Console](https://partner.android.com/things/console) 访问预览版 4.1 的镜像。

#### Device Management APIs

This release introduces a new `DeviceManager` service for apps to control device state, such as executing a factory reset or device reboot. See the [reference documentation](https://developer.android.google.cn/things/reference/com/google/android/things/devicemanagement/package-summary.html) to learn more.

#### 设备管理 API

此版本引入了新的 `DeviceManager` 服务以控制设备状态，可实现执行恢复出厂设置或者设备重启等。请参阅 [参考文档](https://developer.android.google.cn/things/reference/com/google/android/things/devicemanagement/package-summary.html) 了解更多信息。

#### User Driver Permissions

Apps are now required to request permission to manage user drivers registered with the framework. Review the updated [user driver API guides](https://developer.android.google.cn/things/sdk/drivers/index.html) for more details.

#### 用户驱动权限

当应用程序需要使用注册到框架中的用户驱动，应用程序需请求相应权限。有关详细信息, 请查看更新的 [用户驱动程序 API 指南](https://developer.android.google.cn/things/sdk/drivers/index.html)。

#### OpenGL 2.0 Support

With the update to Android O, [OpenGL ES 2.0](https://developer.android.google.cn/guide/topics/graphics/opengl.html) is now supported. Platforms with a GPU (such as Raspberry Pi 3) also now support [hardware acceleration](https://developer.android.google.cn/guide/topics/graphics/hardware-accel.html).

#### OpenGL 2.0 支持

由于 Android O 的更新，现在已经支持 [OpenGL ES 2.0](https://developer.android.google.cn/guide/topics/graphics/opengl.html) 。有一个GPU的平台（如Raspberry Pi 3）也开始支持 [硬件加速](https://developer.android.google.cn/guide/topics/graphics/hardware-accel.html).

#### Runtime Pin Configuration

Raspberry Pi 3 now supports runtime pin configuration for [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html). This enables apps to configure pins as [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html) that were previously reserved for other peripheral functions, and removes the need to edit the `config.txt` file to enable configuration modes for UART and audio. Review the updated [I/O pinout](https://developer.android.google.cn/things/hardware/raspberrypi-io.html) and [mode matrix](https://developer.android.google.cn/things/hardware/raspberrypi-mode-matrix.html) for more information.

#### 运行时引脚管理

Raspberry Pi 3 现在能支持 [外设 I/O](https://developer.android.google.cn/things/sdk/pio/index.html) 的运行时引脚管理。这使得应用程序可以配置之前为其他外设方法保留的引脚，作为 [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html) 使用。同时不在需要编辑 `config.txt` 文件来激活 UART 和音频的设置模式。有关详细信息，请参阅更新的 [I/O 引脚分配](https://developer.android.google.cn/things/hardware/raspberrypi-io.html) 和 [模式矩阵](https://developer.android.google.cn/things/hardware/raspberrypi-mode-matrix.html).

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Input events from [user drivers](https://developer.android.google.cn/things/sdk/drivers/index.html) will not wake the display from sleep.
*   Devices cannot currently act as an A2DP audio source over Bluetooth.


### 已知问题

*   系统电源管理当前已被禁用。设备不会休眠挂起，也无需唤醒服务。
*   Google Play 服务 在第一次启动时需要 2-3 分钟的 DEX 预优化。优化完成后才能正常的安装应用。
*   来自 [用户驱动程序](https://developer.android.google.cn/things/sdk/drivers/index.html) 的输入事件不会从睡眠中唤醒显示屏。
*   设备当前不能通过蓝牙作为 A2DP 音频源。

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### 外设 I/O

*   在调用 `close()` 之后，外设不会被清除或重置。 输出将保留它们的状态, 并且串行端口可能继续传输以前缓冲的数据。
*   如果 GPIO 引脚在上次启动后作为边缘触发器的输入, 则 GPIO 引脚不能用作输出。

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### 用户驱动程序

*   当前用户传感器目前不能被手动注销。当应用程序进程终止时, 它们将自动注销。
*   用户传感器只支持 [连续变化传感器](https://source.android.google.cn/devices/sensors/report-modes.html)。单次和特殊报告模式无法正常工作。

#### Pico i.MX7D

*   OpenGL 2.0 is not yet fully supported. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.

#### Pico i.MX7D

*   OpenGL 2.0 仍未完美支持. 基于此功能的 API（例如 [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) 将无法使用。

#### Argon i.MX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.


#### Argon i.MX6UL

*   **USB:** 在设备板启动后插入的 USB 设备将不被识别。

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** Preview is currently slow to render when using a DSI display.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.

#### Raspberry Pi

*   **音频:** 当无线网络和蓝牙同时启用的时候，可能会出现音频质量问题。
*   **网络:** 如果以太网口连接到一个无互联网接入的网络，无线网络将也无法接入互联网。
*   **摄像头:** 当使用 DSI 显示屏时，显示渲染会变慢。
*   **摄像头:** 无法创建具有多个输出目标的 `CameraCaptureSession` 。
*   **摄像头:** 任何 `CameraCaptureSession` 的第一个请求总是队列两幅图像。这可能导致同一会话中的每个后续 `CaptureRequest` 返回先前捕获的缓冲帧。
*   **I/O:** GPIO 引脚 **BCM4** 、 **BCM5** 和 **BCM6** 在作为输入时会在内部拉伸至 3.3伏。

## Developer Preview 4.1

* * *

_Date: June 2017_  
_Build Number: NIH40K_  
_Play Services: 11.0.0_

## 开发者预览版 4.1

* * *

_日期: 2017 年 6 月_  
_版本号: NIH40K_  
_Play 服务版本: 11.0.0_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 4.1 is available on the Intel Edison, Intel Joule, NXP i.MX7D, NXP i.MX6UL, and Raspberry Pi 3 development boards.


此预览版仅供**开发者和尝鲜者**用来开发和测试硬件平台的兼容性。请注意以下关于预览版的准则：

*   此版本在支持的硬件上可能存在**各种稳定性问题**。请记录并提交您发现的程序错误。
*   不是所有的 API 都在这个预览版中启用。已知禁用的 API 在 "已知问题" 部分中有记录。
*   开发者预览版 4.1 适用于 Intel Edison 、 Intel Joule 、 NXP i.MX7D 、 NXP i.MX6UL 和 Raspberry Pi 3 开发板。

### New in Preview 4.1

### 预览版 4.1 的更新

#### NXP i.MX6UL Pico

NXP has released an updated [Pico developer kit](http://www.technexion.com/solutions/iot-development-platform/android-things/) for Android Things which requires at least Preview 4.1\. The previous [Wandboard kit](http://www.wandboard.org/details/pico-imx6ul) has been deprecated and will not be supported in future versions of Android Things.


#### NXP i.MX6UL Pico

NXP 为 Android Things 发布了一个新的 [Pico 开发者工具包](http://www.technexion.com/solutions/iot-development-platform/android-things/) ， 需要至少使用预览版 4.1 以上版本。之前的 [Wandboard 工具包](http://www.wandboard.org/details/pico-imx6ul) 已经被弃用，不会再后续的 Android Things 版本中支持。

#### Play Services for IoT

This release includes a new variant of [Google Play Services](https://developers.google.cn/android/) with a reduced foorprint targeted for IoT devices. Learn more about the current Google API support on the [SDK overview](https://developer.android.google.cn/things/sdk/index.html#google-services) page.


#### Play 服务的物联网使用

这次更新包括一个新 [Google Play 服务](https://developers.google.cn/android/) 的变种，它被设计为给物联网设备使用，优化了功能。了解更多 Google API 的支持，请参阅 [SDK 概述](https://developer.android.google.cn/things/sdk/index.html#google-services)。

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Hardware graphics acceleration (OpenGL) is not currently enabled. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.


### Known Issues

*   系统电源管理当前已被禁用。设备不会休眠挂起，也无需唤醒服务。
*   Google Play 服务 在第一次启动时需要 2-3 分钟的 DEX 预优化。优化完成后才能正常的安装应用。
*   硬件图形加速 (OpenGL) 当前未开启。基于此功能的 API (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) 现在不可用。

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### 外设 I/O

*   在调用 `close()` 之后，外设不会被清除或重置。 输出将保留它们的状态, 并且串行端口可能继续传输以前缓冲的数据。
*   如果 GPIO 引脚在上次启动后作为边缘触发器的输入, 则 GPIO 引脚不能用作输出。

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### 用户驱动程序

*   当前用户传感器目前不能被手动注销。当应用程序进程终止时, 它们将自动注销。
*   用户传感器只支持 [连续变化传感器](https://source.android.google.cn/devices/sensors/report-modes.html)。单次和特殊报告模式无法正常工作。

#### Edison

*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Edison

*   **重置:** 当 GPIO 引脚被重命名为 GPXX 而不是 IOXX 时，使用 Arduino 上的重置按钮会变成一个不确定的状态，直至断电。所以请使用断电重连来代替重置按钮的使用。

#### Argon IMX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.

#### Argon i.MX6UL

*   **USB:** 在设备板启动后插入的 USB 设备将不被识别。

#### Joule

*   **Camera:** Camera support is not yet enabled.
*   **I/O:** Edge triggers are currently only supported on the following GPIO ports: **J7_58**, **J7_64**, **J7_68**, **J7_71**, **J7_73**, **J7_75**.
*   **I/O:** Shared pins initially configured for GPIO cannot be re-used for any other function (SPI, UART, etc.) until after the next reboot.

#### Joule

*   **摄像头:** 还未支持摄像头。
*   **I/O:** 边缘触发器目前只支持以下 GPIO 引脚: **J7_58**, **J7_64**, **J7_68**, **J7_71**, **J7_73**, **J7_75**.
*   **I/O:** 共享引脚一旦初始化配置给 GPIO ，将无法为其他功能（ SPI 、 UART 等）重用，除非断电重启。

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.

#### Raspberry Pi

*   **音频:** 当无线网络和蓝牙同时启用的时候，可能会出现音频质量问题。
*   **网络:** 如果以太网口连接到一个无互联网接入的网络，无线网络将也无法接入互联网。
*   **摄像头:** 无法创建具有多个输出目标的 `CameraCaptureSession` 。
*   **摄像头:** 任何 `CameraCaptureSession` 的第一个请求总是队列两幅图像。这可能导致同一会话中的每个后续 `CaptureRequest` 返回先前捕获的缓冲帧。
*   **I/O:** 共享引脚 **BCM13/PWM1** 和 **BCM18/PWM0** 如果在上次启动中配置为 PWM 使用，将无法再配置为 GPIO。
*   **I/O:** GPIO 引脚 **BCM4** 、 **BCM5** 和 **BCM6** 在作为输入时会在内部拉伸至 3.3伏。

## Developer Preview 4

* * *

## 开发者预览版 4

* * *

_Date: May 2017_  
_Build Number: NIH40E_  
_Play Services: 10.0.0_

_日期: 2017 年 5月_  
_版本号: NIH40E_  
_Play 服务版本: 10.0.0_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 4 is available on the Intel Edison, Intel Joule, NXP i.MX7D, NXP i.MX6UL, and Raspberry Pi 3 development boards.


此预览版仅供**开发者和尝鲜者**用来开发和测试硬件平台的兼容性。请注意以下关于预览版的准则：

*   此版本在支持的硬件上可能存在**各种稳定性问题**。请记录并提交您发现的程序错误。
*   不是所有的 API 都在这个预览版中启用。已知禁用的 API 在 "已知问题" 部分中有记录。
*   开发者预览版 4 适用于 Intel Edison 、 Intel Joule 、 NXP i.MX7D 、 NXP i.MX6UL 和 Raspberry Pi 3 开发板。

### New in Preview 4

### 预览版 4 的更新

#### NXP i.MX7D support

Android Things is now supported on the [NXP® i.MX7D Pico](http://www.nxp.com/AndroidThingsGS) development platform. Learn more about this device and its capabilities on the [developer kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

#### NXP i.MX7D 支持

Android Things 现在支持 [NXP® i.MX7D Pico](http://www.nxp.com/AndroidThingsGS) 开发平台。 详细了解此设备及其功能请访问 [开发者工具包](https://developer.android.google.cn/things/hardware/developer-kits.html) 。

#### Audio APIs

Developers can now connect to digital audio devices over Inter-IC Sound (I2S) using Peripheral I/O and bind those devices to the media framework using the new audio user-space drivers. Review the new API guides for [I2S](https://developer.android.google.cn/things/sdk/pio/i2s.html) and [audio drivers](https://developer.android.google.cn/things/sdk/drivers/audio.html) for more details.

#### 音频 API

开发者现在可以使用外设 I/O 通过内部声卡（I2S）链接到数字音频设备，并使用新的音频用户空间驱动将这些设备绑定到媒体框架。了解更多，请参阅新的 [I2S API 手册](https://developer.android.google.cn/things/sdk/pio/i2s.html) 和 [声卡驱动](https://developer.android.google.cn/things/sdk/drivers/audio.html) 。

#### Peripheral drivers

Peripheral I/O now supports runtime registration of additional interfaces through the `PioDriverManager`. This enables registration of peripheral bus expansion devices as well as stub interfaces for unit testing. To learn more, see the [reference documentation](https://developer.android.google.cn/things/reference/com/google/android/things/pio/PioDriverManager.html).

#### 外设驱动

外设 I/O 现在支持通过 `PioDriverManager` 运行时注册附加接口。这样可以注册外设总线扩展设备以及用于单元测试的桩接口。若要了解详细资料, 请参 [阅参考文档](https://developer.android.google.cn/things/reference/com/google/android/things/pio/PioDriverManager.html).

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Hardware graphics acceleration (OpenGL) is not currently enabled. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.

### 已知问题

*   系统电源管理当前已被禁用。设备不会休眠挂起，也无需唤醒服务。
*   Google Play 服务 在第一次启动时需要 2-3 分钟的 DEX 预优化。优化完成后才能正常的安装应用。
*   硬件图形加速 (OpenGL) 当前未开启。基于此功能的 API (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) 现在不可用。

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### 外设 I/O

*   在调用 `close()` 之后，外设不会被清除或重置。 输出将保留它们的状态, 并且串行端口可能继续传输以前缓冲的数据。
*   如果 GPIO 引脚在上次启动后作为边缘触发器的输入, 则 GPIO 引脚不能用作输出。

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### 用户驱动程序

*   当前用户传感器目前不能被手动注销。当应用程序进程终止时, 它们将自动注销。
*   用户传感器只支持 [连续变化传感器](https://source.android.google.cn/devices/sensors/report-modes.html)。单次和特殊报告模式无法正常工作。

#### Edison

*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Edison

*   **重置:** 当 GPIO 引脚被重命名为 GPXX 而不是 IOXX 时，使用 Arduino 上的重置按钮会变成一个不确定的状态，直至断电。所以请使用断电重连来代替重置按钮的使用。

#### Argon IMX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.

#### Argon i.MX6UL

*   **USB:** 在设备板启动后插入的 USB 设备将不被识别。

#### Joule

*   **Camera:** Camera support is not yet enabled.
*   **I/O:** Edge triggers are currently only supported on the following GPIO ports: **J7_58**, **J7_64**, **J7_68**, **J7_71**, **J7_73**, **J7_75**.
*   **I/O:** Shared pins initially configured for GPIO cannot be re-used for any other function (SPI, UART, etc.) until after the next reboot.

#### Joule

*   **摄像头:** 还未支持摄像头。
*   **I/O:** 边缘触发器只支持以下 GPIO 引脚: **J7_58**, **J7_64**, **J7_68**, **J7_71**, **J7_73**, **J7_75**.
*   **I/O:** 共享引脚一旦初始化配置给 GPIO ，将无法为其他功能（ SPI 、 UART 等）重用，除非断电重启。

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.
*   **Audio:** Onboard analog audio cannot be used simultaneously with PWM.

#### Raspberry Pi

*   **音频:** 当无线网络和蓝牙同时启用的时候，可能会出现音频质量问题。
*   **网络:** 如果以太网口连接到一个无互联网接入的网络，无线网络将也无法接入互联网。
*   **摄像头:** 无法创建具有多个输出目标的 `CameraCaptureSession` 。
*   **摄像头:** 任何 `CameraCaptureSession` 的第一个请求总是队列两幅图像。这可能导致同一会话中的每个后续 `CaptureRequest` 返回先前捕获的缓冲帧。
*   **I/O:** 共享引脚 **BCM13/PWM1** 和 **BCM18/PWM0** 如果在上次启动中配置为 PWM 使用，将无法再配置为 GPIO。
*   **I/O:** GPIO 引脚 **BCM4** 、 **BCM5** 和 **BCM6** 在作为输入时会在内部拉伸至 3.3伏。
*   **音频:** 板载模拟音频无法同时和 PWM 使用。

## Developer Preview 3

* * *

_Date: April 2017_  
_Build Number: NIG86E_  
_Play Services: 10.0.0_

## 开发者预览版 3

* * *

_日期: 2017 年 4月_  
_版本号: NIG86E_  
_Play 服务版本: 10.0.0_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 3 is available on the Intel Edison, Intel Joule, NXP Pico, NXP Argon, and Raspberry Pi 3 development boards.

此预览版仅供**开发者和尝鲜者**用来开发和测试硬件平台的兼容性。请注意以下关于预览版的准则：

*   此版本在支持的硬件上可能存在**各种稳定性问题**。请记录并提交您发现的程序错误。
*   不是所有的 API 都在这个预览版中启用。已知禁用的 API 在 "已知问题" 部分中有记录。
*   开发者预览版 3 适用于 Intel Edison 、 Intel Joule 、 NXP Pico 、 NXP Argon 和 Raspberry Pi 3 开发板。

### New in Preview 3

### 预览版 3 的更新

#### NXP Argon i.MX6UL support

Android Things is now supported on the [NXP® Argon i.MX6UL](http://www.nxp.com/AndroidThingsGS) development platform. Learn more about this device and its capabilities on the [developer kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

#### NXP Argon i.MX6UL 支持

Android Things 现在支持 [NXP® Argon i.MX6UL](http://www.nxp.com/AndroidThingsGS) 开发平台。 详细了解此设备及其功能请访问 [开发者工具包](https://developer.android.google.cn/things/hardware/developer-kits.html)。

#### Android Bluetooth APIs support

Developers can now use the [Android Bluetooth](https://developer.android.google.cn/guide/topics/connectivity/bluetooth.html) APIs across all Android Things supported hardware. These APIs can be used to interact with both Classic Bluetooth and Bluetooth Low Energy (BLE) devices. See the [Samples](https://developer.android.google.cn/things/sdk/samples.html) page for Bluetooth audio and Bluetooth GATT server code samples.

#### Android Bluetooth APIs 支持

开发者现在可以在所有支持硬件中使用 [Android 蓝牙](https://developer.android.google.cn/guide/topics/connectivity/bluetooth.html) API。 这些 API 可用于与传统蓝牙和低功耗蓝牙（BLE）进行交互。 参阅 [示例](https://developer.android.google.cn/things/sdk/samples.html) 页，以了解蓝牙音频和蓝牙 GATT 服务器代码示例。

#### USB host support

Android Things devices can now operate in [USB host](https://developer.android.google.cn/guide/topics/connectivity/usb/host.html) mode. We have created a [USB Enumerator](https://github.com/androidthings/sample-usbenum) sample that demonstrates how to iterate over and print the interfaces and endpoints for each USB device connected to the host.

#### USB host 支持

Android Things 设备现在可以用 [USB host](https://developer.android.google.cn/guide/topics/connectivity/usb/host.html) 模式工作。我们创建了一个 [USB Enumerator](https://github.com/androidthings/sample-usbenum) 示例演示如何循环访问和打印连接到主机的每个 USB 设备的接口和端点。

#### Access to USB-serial devices

USB-serial devices are now exposed as a `UartDevice` when plugged in. You can discover these devices by name from `getUartDeviceList()`.

#### 接入 USB 串行设备

现在, USB 串行设备在插入时将作为 `UartDevice` 公开。可以通过 `getUartDeviceList()` 中的名称来发现这些设备.

#### Reference documentation

You can now view [reference documentation](https://developer.android.google.cn/things/reference/index.html) online.

#### 参考文档

现在可以浏览在线的 [参考文档](https://developer.android.google.cn/things/reference/index.html)。

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   [Dangerous permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) requested by apps are not granted until the next device reboot. This includes new app installs and new `<uses-permission>` elements in existing apps.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Hardware graphics acceleration (OpenGL) is not currently enabled. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.
*   A2DP Bluetooth profile is set to sink mode. We will provide a method for configuring Bluetooth profiles in a future preview release. Because of this, `BluetoothAdapter.getProfileProxy()` currently throws an error if the `BluetoothProfile.A2DP` argument is used.

*   系统电源管理当前已被禁用。设备不会休眠挂起，也无需唤醒服务。
*   在下一次设备启动之前，来自应用程序的[危险的权限](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) 请求不会被授予。这些请求包括新装应用程序和已装应用的新 `<uses-permission>` 元素。
*   Google Play 服务 在第一次启动时需要 2-3 分钟的 DEX 预优化。优化完成后才能正常的安装应用。
*   硬件图形加速 (OpenGL) 当前未开启。基于此功能的 API (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) 现在不可用。
*   A2DP 蓝牙配置设置为接受模式。在未来的预览版中，我们将提供一个配置蓝牙的方法。因此，如果调用 `BluetoothAdapter.getProfileProxy()` 时使用了 `BluetoothProfile.A2DP` 参数，将会抛出错误异常。

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### 外设 I/O

*   在调用 `close()` 之后，外设不会被清除或重置。 输出将保留它们的状态, 并且串行端口可能继续传输以前缓冲的数据。
*   如果 GPIO 引脚在上次启动后作为边缘触发器的输入, 则 GPIO 引脚不能用作输出。

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### 用户驱动程序

*   当前用户传感器目前不能被手动注销。当应用程序进程终止时, 它们将自动注销。
*   用户传感器只支持 [连续变化传感器](https://source.android.google.cn/devices/sensors/report-modes.html)。单次和特殊报告模式无法正常工作。

#### Edison

*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Edison

*   **重置:** 当 GPIO 引脚被重命名为 GPXX 而不是 IOXX 时，使用 Arduino 上的重置按钮会变成一个不确定的状态，直至断电。所以请使用断电重连来代替重置按钮的使用。

#### Argon IMX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.

re not recognized.

#### Argon i.MX6UL

*   **USB:** 在设备板启动后插入的 USB 设备将不被识别。

#### Joule

*   **Camera:** Camera support is not yet enabled.
*   **I/O:** Edge triggers are currently only supported on the following GPIO ports: **DISPLAY_0_BIAS_EN**, **DISPLAY_0_BKLT_EN**, **DISPLAY_0_RST_N**, **FLASH_RST_N**, **FLASH_TORCH**, **FLASH_TRIGGER**.
*   **I/O:** Shared pins initially configured for GPIO cannot be re-used for any other function (SPI, UART, etc.) until after the next reboot.

#### Joule

*   **摄像头:** 还未支持摄像头。
*   **I/O:** 边缘触发器目前只支持以下 GPIO 引脚: **DISPLAY_0_BIAS_EN**, **DISPLAY_0_BKLT_EN**, **DISPLAY_0_RST_N**, **FLASH_RST_N**, **FLASH_TORCH**, **FLASH_TRIGGER**.
*   **I/O:** 共享引脚一旦初始化配置给 GPIO ，将无法为其他功能（ SPI 、 UART 等）重用，除非断电重启。

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.
*   **Audio:** Onboard analog audio cannot be used simultaneously with PWM.

#### Raspberry Pi

*   **音频:** 当无线网络和蓝牙同时启用的时候，可能会出现音频质量问题。
*   **网络:** 如果以太网口连接到一个无互联网接入的网络，无线网络将也无法接入互联网。
*   **摄像头:** 无法创建具有多个输出目标的 `CameraCaptureSession` 。
*   **摄像头:** 任何 `CameraCaptureSession` 的第一个请求总是队列两幅图像。这可能导致同一会话中的每个后续 `CaptureRequest` 返回先前捕获的缓冲帧。
*   **I/O:** 共享引脚 **BCM13/PWM1** 和 **BCM18/PWM0** 如果在上次启动中配置为 PWM 使用，将无法再配置为 GPIO。
*   **I/O:** GPIO 引脚 **BCM4** 、 **BCM5** 和 **BCM6** 在作为输入时会在内部拉伸至 3.3伏。
*   **音频:** 板载模拟音频无法同时和 PWM 使用。

## Developer Preview 2

* * *

_Date: February 2017_  
_Build Number: NIG40_  
_Play Services: 10.0.0_

## 开发者预览版 2

* * *

_日期: 2017 年 2 月_  
_版本号: NIG40_  
_Play 服务版本: 10.0.0_

Preview APIs [Javadoc reference](https://developer.android.google.cn/things/downloads/com.google.android.things-docs-dp2.zip).

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 2 is available on the Intel Edison, Intel Joule, NXP Pico, and Raspberry Pi 3 development boards.

预览版 API [Javadoc 参考](https://developer.android.google.cn/things/downloads/com.google.android.things-docs-dp2.zip).

此预览版仅供**开发者和尝鲜者**用来开发和测试硬件平台的兼容性。请注意以下关于预览版的准则：

*   此版本在支持的硬件上可能存在**各种稳定性问题**。请记录并提交您发现的程序错误。
*   不是所有的 API 都在这个预览版中启用。已知禁用的 API 在 "已知问题" 部分中有记录。
*   开发者预览版 2 适用于 NXP i.MX7D 、 NXP i.MX6UL 和 Raspberry Pi 3 开发板。

### New in Preview 2

### 预览版 2 的更新

#### Intel Joule support

Android Things is now supported on the [Intel® Joule compute module](https://software.intel.com/en-us/iot/hardware/joule). Learn more about this device and its capabilities on the [developer kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

#### Intel Joule 支持

Android Things 现在支持 [Intel® Joule 计算模块](https://software.intel.com/en-us/iot/hardware/joule)。详细了解此设备及其功能请访问 [开发工具包](https://developer.android.google.cn/things/hardware/developer-kits.html) 页面。

#### Native peripheral API

Access to peripheral I/O from C/C++ code is now supported using the [Native PIO library](https://developer.android.google.cn/things/sdk/pio/native.html) for the [Android NDK](https://developer.android.google.cn/ndk/index.html). Explore the new Native PIO sample on the [samples page](https://developer.android.google.cn/things/sdk/samples.html) to get started.

#### 本地外设 API

现在,支持使用 C/C++ 代码，通过 [本地 PIO 库](https://developer.android.google.cn/things/sdk/pio/native.html)连接 [Android NDK](https://developer.android.google.cn/ndk/index.html)， 接入外设 I/O。想要了解更多，请探索新的原生 [PIO 示例](https://developer.android.google.cn/things/sdk/samples.html)。

#### USB audio support

Devices without on-board analog audio capabilities now support USB microphones and speakers for audio recording and playback. For Preview 2, this includes the following platforms:

*   Intel® Edison
*   Intel® Joule
*   Raspberry Pi

#### USB 音频支持

没有板载模拟音频功能的设备，现在支持 USB 麦克风和扬声器进行音频录制和播放。预览版 2 中, 包括以下平台:

*   Intel® Edison
*   Intel® Joule
*   Raspberry Pi

#### TensorFlow sample

We have created a sample that shows how to use TensorFlow on Android Things devices. This sample demonstrates accessing the camera, performing object recognition and image classification, and speaking out the results using text-to-speech (TTS).

Visit the [samples page](https://developer.android.google.cn/things/sdk/samples.html) to learn more.

#### TensorFlow 示例

我们已经创建了一个示例, 演示如何在 Android Things 上使用 TensorFlow。此示例演示如何访问照相机、执行物体识别和图像分类, 以及使用文本到语音转换 (TTS) 说出结果。

访问 [示例页](https://developer.android.google.cn/things/sdk/samples.html) 了解更多。

#### Peripheral manager reporting

Developers can now inspect the state of active peripheral ports on the device during development and debugging using the `dumpsys` command:

    $ adb shell dumpsys com.google.android.things.pio.IPeripheralManager

#### 外设管理器报告

现在, 开发者可以使用 `dumpsys` 命令在开发和调试过程中检查设备上的活动外设端口的状态:

    $ adb shell dumpsys com.google.android.things.pio.IPeripheralManager

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Bluetooth APIs are currently disabled.
*   USB APIs are currently disabled.
*   [Dangerous permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) requested by apps are not granted until the next device reboot. This includes new app installs and new `<uses-permission>` elements in existing apps.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Hardware graphics acceleration (OpenGL) is not currently enabled. APIs depedent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.

*   系统电源管理当前已被禁用。设备不会休眠挂起，也无需唤醒服务。
*   蓝牙 API 当前不可用。
*   USB API 当前不可用
*   在下一次设备启动之前，来自应用程序的[危险的权限](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) 请求不会被授予。这些请求包括新装应用程序和已装应用的新 `<uses-permission>` 元素。
*   Google Play 服务 在第一次启动时需要 2-3 分钟的 DEX 预优化。优化完成后才能正常的安装应用。
*   硬件图形加速 (OpenGL) 当前未开启。基于此功能的 API (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) 现在不可用。

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### 外设 I/O

*   在调用 `close()` 之后，外设不会被清除或重置。 输出将保留它们的状态, 并且串行端口可能继续传输以前缓冲的数据。
*   如果 GPIO 引脚在上次启动后作为边缘触发器的输入, 则 GPIO 引脚不能用作输出。

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### 用户驱动程序

*   当前用户传感器目前不能被手动注销。当应用程序进程终止时, 它们将自动注销。
*   用户传感器只支持 [连续变化传感器](https://source.android.google.cn/devices/sensors/report-modes.html)。单次和特殊报告模式无法正常工作。

#### Edison

*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Edison

*   **重置:** 当 GPIO 引脚被重命名为 GPXX 而不是 IOXX 时，使用 Arduino 上的重置按钮会变成一个不确定的状态，直至断电。所以请使用断电重连来代替重置按钮的使用。

#### Joule

*   **Camera:** Camera support is not yet enabled.
*   **I/O:** Edge triggers are currently only supported on the following GPIO ports: **DISPLAY_0_BIAS_EN**, **DISPLAY_0_BKLT_EN**, **DISPLAY_0_RST_N**, **FLASH_RST_N**, **FLASH_TORCH**, **FLASH_TRIGGER**.
*   **I/O:** Shared pins initially configured for GPIO cannot be re-used for any other function (SPI, UART, etc.) until after the next reboot.

#### Joule

*   **摄像头:** 还未支持摄像头。
*   **I/O:** 边缘触发器目前只支持以下 GPIO 引脚: **DISPLAY_0_BIAS_EN**, **DISPLAY_0_BKLT_EN**, **DISPLAY_0_RST_N**, **FLASH_RST_N**, **FLASH_TORCH**, **FLASH_TRIGGER**.
*   **I/O:** 共享引脚一旦初始化配置给 GPIO ，将无法为其他功能（ SPI 、 UART 等）重用，除非断电重启。

#### Pico

*   **Network:** Ethernet is currently disabled.
*   **I/O:** `Gpio.getValue()` always returns `false` when the pin is configured as an output.

#### Pico

*   **网络** 以太网当前不可用。
*   **I/O:** 当引脚被配置为输出，`Gpio.getValue()` 总是返回 `false` 。

#### Raspberry Pi

*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.
*   **Audio:** Onboard analog audio cannot be used simultaneously with PWM.

#### Raspberry Pi

*   **网络:** 如果以太网口连接到一个无互联网接入的网络，无线网络将也无法接入互联网。
*   **摄像头:** 无法创建具有多个输出目标的 `CameraCaptureSession` 。
*   **摄像头:** 任何 `CameraCaptureSession` 的第一个请求总是队列两幅图像。这可能导致同一会话中的每个后续 `CaptureRequest` 返回先前捕获的缓冲帧。
*   **I/O:** 共享引脚 **BCM13/PWM1** 和 **BCM18/PWM0** 如果在上次启动中配置为 PWM 使用，将无法再配置为 GPIO。
*   **I/O:** GPIO 引脚 **BCM4** 、 **BCM5** 和 **BCM6** 在作为输入时会在内部拉伸至 3.3伏。
*   **音频:** 板载模拟音频无法同时和 PWM 使用。

## Developer Preview 1

* * *

## 开发者预览版 1

* * *

_Date: December 2016_  
_Build Number: NIF73/NIF74_  
_Play Services: 10.0.0_

_日期: 2016 年 12月_  
_版本号: NIF73/NIF74_  
_Play 服务版本: 10.0.0_

Preview APIs [Javadoc reference](https://developer.android.google.cn/things/downloads/com.google.android.things-docs-dp1.zip).

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 1 is available on the Intel Edison, NXP Pico, and Raspberry Pi 3 development boards.


预览版 API [Javadoc 参考](https://developer.android.google.cn/things/downloads/com.google.android.things-docs-dp1.zip)。

此预览版仅供**开发者和尝鲜者**用来开发和测试硬件平台的兼容性。请注意以下关于预览版的准则：

*   此版本在支持的硬件上可能存在**各种稳定性问题**。请记录并提交您发现的程序错误。
*   不是所有的 API 都在这个预览版中启用。已知禁用的 API 在 "已知问题" 部分中有记录。
*   开发者预览版 1 适用于 Intel Edison 、 NXP Pico 和 Raspberry Pi 3 开发板。

### Known issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Bluetooth APIs are currently disabled.
*   USB APIs are currently disabled.
*   [Dangerous permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) requested by apps are not granted until the next device reboot. This includes new app installs and new `<uses-permission>` elements in existing apps.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   When multiple activities contain an intent filter for the `IOT_LAUNCHER` category, the system displays an app chooser that isn't accessible on devices without display support. Android Things only supports a single launcher app, and this behavior will be disabled in a future release.

*   系统电源管理当前已被禁用。设备不会休眠挂起，也无需唤醒服务。
*   蓝牙 API 当前不可用。
*   USB API 当前不可用
*   在下一次设备启动之前，来自应用程序的[危险的权限](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) 请求不会被授予。这些请求包括新装应用程序和已装应用的新 `<uses-permission>` 元素。
*   Google Play 服务 在第一次启动时需要 2-3 分钟的 DEX 预优化。优化完成后才能正常的安装应用。
*   当多个 Activity 包含一个 Intent 具有 `IOT_LAUNCHER` 类别的的筛选器时, 在没有显示支持的设备中，应用程序选择器将不可用。Android Things 只支持单一的启动应用程序, 这种行为在未来的版本中将被禁用。

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### 外设 I/O

*   在调用 `close()` 之后，外设不会被清除或重置。 输出将保留它们的状态, 并且串行端口可能继续传输以前缓冲的数据。
*   如果 GPIO 引脚在上次启动后作为边缘触发器的输入, 则 GPIO 引脚不能用作输出。

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### 用户驱动程序

*   当前用户传感器目前不能被手动注销。当应用程序进程终止时, 它们将自动注销。
*   用户传感器只支持 [连续变化传感器](https://source.android.google.cn/devices/sensors/report-modes.html)。单次和特殊报告模式无法正常工作。

#### Edison

*   **Audio:** Recording is not currently supported.
*   **I/O:** GPIO pin **GP77** is listed by `PeripheralManagerService`, but is not accessible to apps.
*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Edison

*   **音频:** 当前不支持录音。
*   **I/O:** GPIO 引脚 **GP77** 会被 `PeripheralManagerService` 列出，但对应用程序不可用。
*   **重置:** 当 GPIO 引脚被重命名为 GPXX 而不是 IOXX 时，使用 Arduino 上的重置按钮会变成一个不确定的状态，直至断电。所以请使用断电重连来代替重置按钮的使用。

#### Pico

*   **Network:** Ethernet is currently disabled.
*   **I/O:** `Gpio.getValue()` always returns `false` when the pin is configured as an output.

#### Pico

*   **网络** 以太网当前不可用。
*   **I/O:** 当引脚被配置为输出，`Gpio.getValue()` 总是返回 `false` 。

#### Raspberry Pi

*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Graphics:** Hardware graphics acceleration is not currently enabled.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM23** and **BCM24** are both currently mapped to control **BCM23** (physical pin J8-16).
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.

#### Raspberry Pi

*   **网络:** 如果以太网口连接到一个无互联网接入的网络，无线网络将也无法接入互联网。
*   **图形:** 目前不支持硬件图形加速。
*   **摄像头:** 无法创建具有多个输出目标的 `CameraCaptureSession` 。
*   **摄像头:** 任何 `CameraCaptureSession` 的第一个请求总是队列两幅图像。这可能导致同一会话中的每个后续 `CaptureRequest` 返回先前捕获的缓冲帧。
*   **I/O:** 共享引脚 **BCM13/PWM1** 和 **BCM18/PWM0** 如果在上次启动中配置为 PWM 使用，将无法再配置为 GPIO。
*   **I/O:** GPIO 引脚 **BCM23** 和 **BCM24** 同时映射为控制 **BCM23** (物理引脚 J8-16)。
*   **I/O:** GPIO 引脚 **BCM4** 、 **BCM5** 和 **BCM6** 在作为输入时会在内部拉伸至 3.3伏。

