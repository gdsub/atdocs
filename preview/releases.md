# Release Notes

## In this document

1.  [Developer Preview 5.1](#preview-5-1)
2.  [Developer Preview 5](#preview-5)
3.  [Developer Preview 4.1](#preview-4-1)
4.  [Developer Preview 4](#preview-4)
5.  [Developer Preview 3](#preview-3)
6.  [Developer Preview 2](#preview-2)
7.  [Developer Preview 1](#preview-1)

This document outlines issues and fixes related to each release of the Android Things developer preview. We are committed to providing regular updates to developers, and aim to have new preview releases approximately every 6-8 weeks.

Please file tickets in the Android issue tracker for issues discovered in the system, hardware support, and documentation:

*   [Android Things bug report](https://issuetracker.google.com/issues/new?component=192720&template=847005)
*   [Android Things feature request](https://issuetracker.google.com/issues/new?component=192720&template=848805)

<aside class="note">**Note:** <span>Before filing a bug, please check this page for known issues and ensure you are running the [latest version](https://partner.android.com/things/console) of the preview system image.</span></aside>

To ask questions and discuss ideas with other developers working on Android Things, join the [IoT Developers Google+ community](http://g.co/iotdev).

## Developer Preview 5.1.1

* * *

_Date: October 2017_  
_Build Number: OIR1.170720.018_  
_Play Services: 11.0.4_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Developer Preview 5.1.1 is available on **only** the NXP i.MX7D.

### New in Preview 5.1.1

#### Bug Fixes

*   Corrected an issue affecting camera initialization, causing the camera device to be inaccessible to apps.

### Known Issues

See the Known Issues list for [Developer Preview 5.1](#preview-5-1).

## Developer Preview 5.1

* * *

_Date: August 2017_  
_Build Number: OIR1.170720.017_  
_Play Services: 11.0.4_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 5.1 is available on the NXP i.MX7D, NXP i.MX6UL, and Raspberry Pi 3 development boards.

### New in Preview 5.1

#### Bug Fixes

*   Automatically detect display settings on Raspberry Pi 3 without the need for edits to the root `config.txt` file.
*   Remove shadowing artifacts observed when pop-up windows (such as dialogs) are shown.
*   Fixed an issue causing the cursor associated with a USB mouse to hide/show incorrectly.

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Input events from [user drivers](https://developer.android.google.cn/things/sdk/drivers/index.html) will not wake the display from sleep.
*   Devices cannot currently act as an A2DP audio source over Bluetooth.

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### Pico i.MX7D

*   OpenGL 2.0 is not yet fully supported. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.

#### Argon i.MX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** Preview is currently slow to render when using a DSI display.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.

## Developer Preview 5

* * *

_Date: August 2017_  
_Build Number: OIR1.170720.015_  
_Play Services: 11.0.4_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 5 is available on the NXP i.MX7D, NXP i.MX6UL, and Raspberry Pi 3 development boards.

### New in Preview 5

#### Android O

Android O is currently under Developer Preview for phones and tablets, and DP5 is now based on this upcoming release. You should update your apps to target API level 26 to work correctly on the platform with our support libraries.

#### NXP i.MX6UL SprIoT

Android Things is now supported on the [NXP® SprIoT i.MX6UL](http://wireless.murata.com/eng/products/wireless-connectivity-platforms/iot-system-on-module/spriot-6ul.html) development platform. Learn more about this device and its capabilities on the [developer kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

#### Legacy Board Support

With Intel discontinuing the Edison and Joule hardware designs, these platforms are moving to legacy support. They will not continue to receive the latest platform updates, but developers may still access the Preview 4.1 images through the [Android Things Console](https://partner.android.com/things/console).

#### Device Management APIs

This release introduces a new `DeviceManager` service for apps to control device state, such as executing a factory reset or device reboot. See the [reference documentation](https://developer.android.google.cn/things/reference/com/google/android/things/devicemanagement/package-summary.html) to learn more.

#### User Driver Permissions

Apps are now required to request permission to manage user drivers registered with the framework. Review the updated [user driver API guides](https://developer.android.google.cn/things/sdk/drivers/index.html) for more details.

#### OpenGL 2.0 Support

With the update to Android O, [OpenGL ES 2.0](https://developer.android.google.cn/guide/topics/graphics/opengl.html) is now supported. Platforms with a GPU (such as Raspberry Pi 3) also now support [hardware acceleration](https://developer.android.google.cn/guide/topics/graphics/hardware-accel.html).

#### Runtime Pin Configuration

Raspberry Pi 3 now supports runtime pin configuration for [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html). This enables apps to configure pins as [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html) that were previously reserved for other peripheral functions, and removes the need to edit the `config.txt` file to enable configuration modes for UART and audio. Review the updated [I/O pinout](https://developer.android.google.cn/things/hardware/raspberrypi-io.html) and [mode matrix](https://developer.android.google.cn/things/hardware/raspberrypi-mode-matrix.html) for more information.

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Input events from [user drivers](https://developer.android.google.cn/things/sdk/drivers/index.html) will not wake the display from sleep.
*   Devices cannot currently act as an A2DP audio source over Bluetooth.

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### Pico i.MX7D

*   OpenGL 2.0 is not yet fully supported. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.

#### Argon i.MX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** Preview is currently slow to render when using a DSI display.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.

## Developer Preview 4.1

* * *

_Date: June 2017_  
_Build Number: NIH40K_  
_Play Services: 11.0.0_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 4.1 is available on the Intel Edison, Intel Joule, NXP i.MX7D, NXP i.MX6UL, and Raspberry Pi 3 development boards.

### New in Preview 4.1

#### NXP i.MX6UL Pico

NXP has released an updated [Pico developer kit](http://www.technexion.com/solutions/iot-development-platform/android-things/) for Android Things which requires at least Preview 4.1\. The previous [Wandboard kit](http://www.wandboard.org/details/pico-imx6ul) has been deprecated and will not be supported in future versions of Android Things.

#### Play Services for IoT

This release includes a new variant of [Google Play Services](https://developers.google.cn/android/) with a reduced foorprint targeted for IoT devices. Learn more about the current Google API support on the [SDK overview](https://developer.android.google.cn/things/sdk/index.html#google-services) page.

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Hardware graphics acceleration (OpenGL) is not currently enabled. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### Edison

*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Argon IMX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.

#### Joule

*   **Camera:** Camera support is not yet enabled.
*   **I/O:** Edge triggers are currently only supported on the following GPIO ports: **J7_58**, **J7_64**, **J7_68**, **J7_71**, **J7_73**, **J7_75**.
*   **I/O:** Shared pins initially configured for GPIO cannot be re-used for any other function (SPI, UART, etc.) until after the next reboot.

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.

## Developer Preview 4

* * *

_Date: May 2017_  
_Build Number: NIH40E_  
_Play Services: 10.0.0_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 4 is available on the Intel Edison, Intel Joule, NXP i.MX7D, NXP i.MX6UL, and Raspberry Pi 3 development boards.

### New in Preview 4

#### NXP i.MX7D support

Android Things is now supported on the [NXP® i.MX7D Pico](http://www.nxp.com/AndroidThingsGS) development platform. Learn more about this device and its capabilities on the [developer kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

#### Audio APIs

Developers can now connect to digital audio devices over Inter-IC Sound (I2S) using Peripheral I/O and bind those devices to the media framework using the new audio user-space drivers. Review the new API guides for [I2S](https://developer.android.google.cn/things/sdk/pio/i2s.html) and [audio drivers](https://developer.android.google.cn/things/sdk/drivers/audio.html) for more details.

#### Peripheral drivers

Peripheral I/O now supports runtime registration of additional interfaces through the `PioDriverManager`. This enables registration of peripheral bus expansion devices as well as stub interfaces for unit testing. To learn more, see the [reference documentation](https://developer.android.google.cn/things/reference/com/google/android/things/pio/PioDriverManager.html).

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Hardware graphics acceleration (OpenGL) is not currently enabled. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### Edison

*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Argon IMX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.

#### Joule

*   **Camera:** Camera support is not yet enabled.
*   **I/O:** Edge triggers are currently only supported on the following GPIO ports: **J7_58**, **J7_64**, **J7_68**, **J7_71**, **J7_73**, **J7_75**.
*   **I/O:** Shared pins initially configured for GPIO cannot be re-used for any other function (SPI, UART, etc.) until after the next reboot.

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.
*   **Audio:** Onboard analog audio cannot be used simultaneously with PWM.

## Developer Preview 3

* * *

_Date: April 2017_  
_Build Number: NIG86E_  
_Play Services: 10.0.0_

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 3 is available on the Intel Edison, Intel Joule, NXP Pico, NXP Argon, and Raspberry Pi 3 development boards.

### New in Preview 3

#### NXP Argon i.MX6UL support

Android Things is now supported on the [NXP® Argon i.MX6UL](http://www.nxp.com/AndroidThingsGS) development platform. Learn more about this device and its capabilities on the [developer kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

#### Android Bluetooth APIs support

Developers can now use the [Android Bluetooth](https://developer.android.google.cn/guide/topics/connectivity/bluetooth.html) APIs across all Android Things supported hardware. These APIs can be used to interact with both Classic Bluetooth and Bluetooth Low Energy (BLE) devices. See the [Samples](https://developer.android.google.cn/things/sdk/samples.html) page for Bluetooth audio and Bluetooth GATT server code samples.

#### USB host support

Android Things devices can now operate in [USB host](https://developer.android.google.cn/guide/topics/connectivity/usb/host.html) mode. We have created a [USB Enumerator](https://github.com/androidthings/sample-usbenum) sample that demonstrates how to iterate over and print the interfaces and endpoints for each USB device connected to the host.

#### Access to USB-serial devices

USB-serial devices are now exposed as a `UartDevice` when plugged in. You can discover these devices by name from `getUartDeviceList()`.

#### Reference documentation

You can now view [reference documentation](https://developer.android.google.cn/things/reference/index.html) online.

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   [Dangerous permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) requested by apps are not granted until the next device reboot. This includes new app installs and new `<uses-permission>` elements in existing apps.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Hardware graphics acceleration (OpenGL) is not currently enabled. APIs dependent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.
*   A2DP Bluetooth profile is set to sink mode. We will provide a method for configuring Bluetooth profiles in a future preview release. Because of this, `BluetoothAdapter.getProfileProxy()` currently throws an error if the `BluetoothProfile.A2DP` argument is used.

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### Edison

*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Argon IMX6UL

*   **USB:** USB devices connected after the board has booted are not recognized.

#### Joule

*   **Camera:** Camera support is not yet enabled.
*   **I/O:** Edge triggers are currently only supported on the following GPIO ports: **DISPLAY_0_BIAS_EN**, **DISPLAY_0_BKLT_EN**, **DISPLAY_0_RST_N**, **FLASH_RST_N**, **FLASH_TORCH**, **FLASH_TRIGGER**.
*   **I/O:** Shared pins initially configured for GPIO cannot be re-used for any other function (SPI, UART, etc.) until after the next reboot.

#### Raspberry Pi

*   **Audio:** Audio quality issues may present when both WiFi and Bluetooth are enabled.
*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.
*   **Audio:** Onboard analog audio cannot be used simultaneously with PWM.

## Developer Preview 2

* * *

_Date: February 2017_  
_Build Number: NIG40_  
_Play Services: 10.0.0_

Preview APIs [Javadoc reference](https://developer.android.google.cn/things/downloads/com.google.android.things-docs-dp2.zip).

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 2 is available on the Intel Edison, Intel Joule, NXP Pico, and Raspberry Pi 3 development boards.

### New in Preview 2

#### Intel Joule support

Android Things is now supported on the [Intel® Joule compute module](https://software.intel.com/en-us/iot/hardware/joule). Learn more about this device and its capabilities on the [developer kits](https://developer.android.google.cn/things/hardware/developer-kits.html) page.

#### Native peripheral API

Access to peripheral I/O from C/C++ code is now supported using the [Native PIO library](https://developer.android.google.cn/things/sdk/pio/native.html) for the [Android NDK](https://developer.android.google.cn/ndk/index.html). Explore the new Native PIO sample on the [samples page](https://developer.android.google.cn/things/sdk/samples.html) to get started.

#### USB audio support

Devices without on-board analog audio capabilities now support USB microphones and speakers for audio recording and playback. For Preview 2, this includes the following platforms:

*   Intel® Edison
*   Intel® Joule
*   Raspberry Pi

#### TensorFlow sample

We have created a sample that shows how to use TensorFlow on Android Things devices. This sample demonstrates accessing the camera, performing object recognition and image classification, and speaking out the results using text-to-speech (TTS).

Visit the [samples page](https://developer.android.google.cn/things/sdk/samples.html) to learn more.

#### Peripheral manager reporting

Developers can now inspect the state of active peripheral ports on the device during development and debugging using the `dumpsys` command:

    $ adb shell dumpsys com.google.android.things.pio.IPeripheralManager

### Known Issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Bluetooth APIs are currently disabled.
*   USB APIs are currently disabled.
*   [Dangerous permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) requested by apps are not granted until the next device reboot. This includes new app installs and new `<uses-permission>` elements in existing apps.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   Hardware graphics acceleration (OpenGL) is not currently enabled. APIs depedent on this functionality (such as [WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)) are not available.

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### Edison

*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Joule

*   **Camera:** Camera support is not yet enabled.
*   **I/O:** Edge triggers are currently only supported on the following GPIO ports: **DISPLAY_0_BIAS_EN**, **DISPLAY_0_BKLT_EN**, **DISPLAY_0_RST_N**, **FLASH_RST_N**, **FLASH_TORCH**, **FLASH_TRIGGER**.
*   **I/O:** Shared pins initially configured for GPIO cannot be re-used for any other function (SPI, UART, etc.) until after the next reboot.

#### Pico

*   **Network:** Ethernet is currently disabled.
*   **I/O:** `Gpio.getValue()` always returns `false` when the pin is configured as an output.

#### Raspberry Pi

*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.
*   **Audio:** Onboard analog audio cannot be used simultaneously with PWM.

## Developer Preview 1

* * *

_Date: December 2016_  
_Build Number: NIF73/NIF74_  
_Play Services: 10.0.0_

Preview APIs [Javadoc reference](https://developer.android.google.cn/things/downloads/com.google.android.things-docs-dp1.zip).

This preview release is for **developers and early adopters** to use for development and compatibility testing on supported hardware platforms. Please note the following general guidelines about the preview:

*   This release may have various **stability issues** on supported hardware. Please file bugs to report issues that you discover.
*   Not all APIs are enabled in this preview. APIs known to be disabled are documented in the Known Issues section.
*   Developer Preview 1 is available on the Intel Edison, NXP Pico, and Raspberry Pi 3 development boards.

### Known issues

*   System power management is currently disabled. Devices will not suspend and wake locks are not necessary.
*   Bluetooth APIs are currently disabled.
*   USB APIs are currently disabled.
*   [Dangerous permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) requested by apps are not granted until the next device reboot. This includes new app installs and new `<uses-permission>` elements in existing apps.
*   Google Play Services requires 2-3 minutes on first boot to pre-optimize dex. App installs are blocked until this process is complete.
*   When multiple activities contain an intent filter for the `IOT_LAUNCHER` category, the system displays an app chooser that isn't accessible on devices without display support. Android Things only supports a single launcher app, and this behavior will be disabled in a future release.

#### Peripheral I/O

*   Peripherals do not clear or reset after calling `close()`. Outputs will retain their state and serial ports may continue to transmit previously buffered data.
*   GPIO pins cannot be used as an output if they were previously enabled as an input with an edge trigger enabled since the last reboot.

#### User Drivers

*   User sensors cannot currently be unregistered manually. They are unregistered automatically when the app process terminates.
*   User sensors only support [continuous and on-change](https://source.android.google.cn/devices/sensors/report-modes.html) sensors. One-shot and special reporting modes may not function as expected.

#### Edison

*   **Audio:** Recording is not currently supported.
*   **I/O:** GPIO pin **GP77** is listed by `PeripheralManagerService`, but is not accessible to apps.
*   **RESET:** The RESET button on the Arduino breakout board can temporarily leave your board in an inconsistent state where GPIO pins are named GPXX instead of IOXX until power is disconnected. Instead of using the onboard RESET button, disconnect and reconnect power to reboot.

#### Pico

*   **Network:** Ethernet is currently disabled.
*   **I/O:** `Gpio.getValue()` always returns `false` when the pin is configured as an output.

#### Raspberry Pi

*   **Network:** Wi-Fi cannot connect to the internet if Ethernet is also connected to a network without internet access.
*   **Graphics:** Hardware graphics acceleration is not currently enabled.
*   **Camera:** A new `CameraCaptureSession` cannot be created with more than one target output surface.
*   **Camera:** The first request in any `CameraCaptureSession` always queues two images. This can cause each subsequent `CaptureRequest` in the same session to return a buffered frame from a previous capture.
*   **I/O:** Shared pins **BCM13/PWM1** and **BCM18/PWM0** cannot be used for GPIO if they were previously enabled for PWM since the last reboot.
*   **I/O:** GPIO pins **BCM23** and **BCM24** are both currently mapped to control **BCM23** (physical pin J8-16).
*   **I/O:** GPIO pins **BCM4**, **BCM5**, and **BCM6** are internally pulled up to 3.3V when used as inputs.

