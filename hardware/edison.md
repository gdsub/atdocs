# Intel® Edison

# Intel® Edison

The Intel® Edison compute module is a modular, small and powerful system on a chip (SoC) that includes a CPU, MCU, memory, storage and dual-band Wi-Fi and Bluetooth. The module can be mounted onto a system of expansion boards, enabling quick adoption and prototyping for the consumer and light industrial IoT applications.

Intel® Edison 计算模块是一款模块化，体积小而且强大的片上系统（SoC），它包括CPU，MCU，内存，存储器，双频Wi-Fi和蓝牙。该模块可以安装到扩展板的系统上，使消费者和轻工业物联网应用的快速采用和原型化成为可能。

![](https://developer.android.google.cn/things/images/intel-edison-arduino-kit.png) ![](https://developer.android.google.cn/things/images/intel-edison-sparkfun-kit.png)

## Flashing the image

## 烧录镜像

* * *

Before you begin flashing, you will need the following items in addition to your Edison board:

在烧录之前，您需要在Edison开发板上做以下几个附加步骤：

*   Micro-USB cable

*   Micro-USB 线

To flash Android Things onto your board, download the preview image in the [Android Things Console](https://partner.android.com/things/console) (see the [release notes](https://developer.android.google.cn/things/preview/releases.html#developer_preview_5)) and follow these steps:

将 Android Things 烧录到开发板之前，请先访问 [Android Things 控制台](https://partner.android.com/things/console) 下载预览版镜像，然后参考以下步骤：

### Step 1: Install Fastboot

### 步骤 1： 安装 Fastboot

If this is your first time installing Android Things on the Edison, you need to upgrade the bootloader to be Fastboot capable. Follow the Intel Getting Started Guide to perform the required one-time setup steps on your board:

如果这是您第一次在Edison开发板上安装 Android Things，您需要升级您的 bootloader 到 Fastboot，请参考 Intel 提供的入门指南来对开发板进行必要的一次性步骤设置：

*   [Arduino Expansion Board](https://software.intel.com/en-us/articles/installing-android-things-on-intel-edison-kit-for-arduino)
*   [Arduino 扩展板](https://software.intel.com/en-us/articles/installing-android-things-on-intel-edison-kit-for-arduino)
*   [Mini Breakout Board](https://software.intel.com/en-us/articles/installing-android-things-on-intel-edison-breakout-board)
*   [迷你分线板](https://software.intel.com/en-us/articles/installing-android-things-on-intel-edison-breakout-board)
*   [Sparkfun Block](https://software.intel.com/en-us/articles/installing-android-things-on-sparkfun-block-with-intel-edison-module)
*   [Sparkfun 扩展模块](https://software.intel.com/en-us/articles/installing-android-things-on-sparkfun-block-with-intel-edison-module)

### Step 2: Connect the Hardware

### 步骤 2： 连接到硬件

Connect the board to your host computer:

连接开发板到计算机：

**For Arduino Expansion Board:**

**对于 Arduino 扩展板：**

![""](https://developer.android.google.cn/things/images/edison-arduino-connections.png)

1.  Ensure switch **SW1** is in the position _towards_ the micro USB ports.
1.  确保 **SW1** 开关被置于 **朝向** Micro-USB 口的方向。
2.  Press the **FW** button and keep it pressed.
2.  长按 **FW** 键。
3.  Connect a USB cable to **J16**.
3.  连接 USB 线到 **J16**。
4.  Release the **FW** button.
4.  松开 **FW** 键。

**For Sparkfun Block:**

**对于 Sparkfun 扩展模块：**

![""](https://developer.android.google.cn/things/images/edison-sparkfun-connections.png)

1.  Connect a USB cable to the **OTG** connector.
1.  连接USB线到 **OTG** 接口。

### Step 3: Flash Android Things

### 步骤 3：烧录 Android Things

Once you have loaded the proper bootloader on your device, use the following steps to flash the Android image:

当您已经在您的设备上加载了正确的引导程序（bootloader），参考以下步骤来烧录 Android 镜像：

1.  Download and install [Android Studio](https://developer.android.google.cn/studio/index.html) or the [`sdkmanager`](https://developer.android.google.cn/studio/command-line/sdkmanager.html) command-line tool. Update the Android SDK Platform Tools to version 25.0.3 or later from the [SDK Manager](https://developer.android.google.cn/studio/intro/update.html#sdk-manager).

1. 下载并安装 [Android Studio](https://developer.android.google.cn/studio/index.html) 或者 [`sdkmanager`](https://developer.android.google.cn/studio/command-line/sdkmanager.html) 命令行工具。 使用 [SDK Manager](https://developer.android.google.cn/studio/intro/update.html#sdk-manager) 来更新 Android SDK 平台工具到 25.0.3 或者更新的版本。

    *   Navigate to the Android SDK location on your computer; the path can be found in the system settings for Android Studio. Verify that the `fastboot` binary is installed in the `platform-tools/` directory.

    *   切换到计算机上 Android SDK 的位置，该位置的路径可以从 Android Studio 的系统配置中可以找到。确认 `fastboot` 二进制文件已经安装到`platform-tools/` 目录下。

    *   After you have the fastboot tool, add it to your `PATH` [environment variable](https://developer.android.google.cn/studio/command-line/variables.html#set). This command should be similar to the following:

    *   确认了 fastboot 工具之后，把它添加到您的环境变量 `PATH` [环境变量](https://developer.android.google.cn/studio/command-line/variables.html#set) 中。添加的命令参考如下。

        `export PATH=$PATH:"path/to/fastboot"`

2.  Open a command line terminal and navigate to the unzipped image directory.
2.  打开终端命令行并切换到已解压的镜像目录。

3.  Verify that the device has booted into Fastboot mode by executing the following command:
3.  通过执行以下命令来验证设备是否已启动到 Fastboot 模式

        $ fastboot devices1b2f21d4e1fe0129        fastboot

    <aside class="note">**Note:** <span>Your device will not boot into Fastboot mode if it was previously flashed with Android Things. You need to first execute the following command using the [adb tool](https://developer.android.google.cn/tools/help/adb.html) to reboot the device into Fastboot mode.

        $ adb reboot bootloader</span></aside>

    <aside class="note">**注意：** <span>如果您的设备之前烧录过 Android Things，那么该设备无法启动到 Fastboot 模式。您需要通过 [adb tool](https://developer.android.google.cn/tools/help/adb.html) 执行以下命令来重启设备到 Fastboot 模式。

        $ adb reboot bootloader
    
    </span></aside>

4.  Execute the `flash-all.sh` script. This script installs the necessary bootloader, baseband firmware(s), and operating system. (On Windows systems, use `flash-all.bat` instead).
4.  执行 `flash-all.sh` 脚本。该脚本会安装必要的 bootloader，基带固件以及操作系统。（Windows 操作系统使用 `flash-all.bat` 替代）

    <aside class="note">**Note:** <span>The device automatically reboots into Android Things when the process is complete.</span></aside>

    <aside class="note">**注意：** <span>该过程完成后，设备会自动重新引导到 Android Things</span></aside>

5.  To verify that Android is running on the device, discover it using the [adb tool](https://developer.android.google.cn/tools/help/adb.html):
5.  要验证 Android 已经在该设备上运行, 可以使用 [adb tool](https://developer.android.google.cn/tools/help/adb.html):

        $ adb wait-for-device...$ adb devicesList of devices attached1b2f21d4e1fe0129        device

## Connecting Wi-Fi
## 连接 Wi-Fi

* * *

After flashing your board, it is strongly recommended to connect it to the internet. This allows your device to deliver crash reports and receive updates.

烧录完开发板之后, 强烈建议将其连接到互联网. 这可让您的设备提供崩溃报告以及接收更新。

<aside class="note">**Note:** <span>The device doesn't need to be on the same network as your computer.</span></aside>

<aside class="note">**注意：** <span>设备和计算机不一定要处于同一个网络。</span></aside>

To connect your board to Wi-Fi, first access a shell prompt on the device. You can use either of the following methods:

开发板连接Wi-Fi之前需要先访问设备的shell终端。 您可用通过以下任意一种方式来访问:

*   Open a shell over adb with the `adb shell` command.
*   使用 `adb shell` 来打开基于adb的终端。
*   Connect to the [serial console](#serial_debug_console).
*   连接到串口调试终端 [串口终端](#串口调试终端)。

Once you can access a shell prompt, follow these steps:

连接到终端命令行以后, 接着做以下步骤:

1.  Send an intent to the Wi-Fi service that includes the SSID of your local network. Your [board](https://developer.android.google.cn/things/hardware/developer-kits.html) must support the network protocol and frequency band of the wireless network in order to establish a connection.

1.  发送一条包含本地无线网络 SSID 等参数的消息到Wi-Fi服务。 您的 [开发板](https://developer.android.google.cn/things/hardware/developer-kits.html) 必须支持该无线网络的协议以及频段才能够建立网络连接。

        $ am startservice \    -n com.google.wifisetup/.WifiSetupService \    -a WifiSetupService.Connect

    The following arguments are supported with this command:

    该命令支持以下参数：

    <table>

    <tbody>

    <tr>

    <th style="width: 240px;">Argument</th>

    <th>Description</th>

    </tr>

    <tr>

    <td>`-e ssid <var>network_ssid</var>`</td>

    <td>Connect to the wireless network SSID specified by <var>network_ssid</var>. _This argument is required_.</td>

    </tr>

    <tr>

    <td>`-e passphrase <var>network_pass</var>`</td>

    <td>Optional argument to use the passcode specified by <var>network_pass</var> to connect to the network SSID. This argument is not necessary if your network doesn't require a passcode.</td>

    </tr>

    <tr>

    <td>`-e passphrase64 <var>encoded_pass</var>`</td>

    <td>Optional argument used in place of `passphrase` for passcodes with special characters (`space, !, ", $, &, ', (, ), ;, <, >, `, or |`). Use [base64 encoding](https://www.base64encode.org/) to specify the value for <var>encoded_pass</var>.</td>

    </tr>

    <tr>

    <td>`--ez hidden true`</td>

    <td>Optional argument to indicate that the SSID specified in this command is hidden. If omitted, this value defaults to false.</td>

    </tr>

    </tbody>

    </table>

    <table>

    <tbody>

    <tr>

    <th style="width: 240px;">参数</th>

    <th>描述</th>

    </tr>

    <tr>

    <td>`-e ssid <var>network_ssid</var>`</td>

    <td>通过 <var>network_ssid</var> 来指定被连接无线网络的SSID。 **该参数是必要参数**。</td>

    </tr>

    <tr>

    <td>`-e passphrase <var>network_pass</var>`</td>

    <td>通过 <var>network_pass</var> 来指定被连接无线网络（SSID）的密码。 该参数不是必要参数，因为无线网络可以不配置密码。</td>

    </tr>

    <tr>

    <td>`-e passphrase64 <var>encoded_pass</var>`</td>

    <td>通过 `passphrase` 来指定包含特殊字符 (`space, !, ", $, &, ', (, ), ;, <, >, `, 或者 |`) 的密码。 参数值 <var>encoded_pass</var> 采用 [base64编码](https://www.base64encode.org/)。</td>

    </tr>

    <tr>

    <td>`--ez hidden true`</td>

    <td>可选参数用来表示命令中指定的无线网络（SSID）是隐藏的。 如果省略, 这个参数的默认值为false。</td>

    </tr>

    </tbody>

    </table>

2.  Verify that the connection was successful through `logcat`:
2.  通过 `logcat` 来验证网络连接是否成功建立：

        $ logcat -d | grep Wifi...V WifiWatcher: Network state changed to CONNECTEDV WifiWatcher: SSID changed: ...I WifiConfigurator: Successfully connected to ...

3.  Test that you can access a remote IP address:
3.  测试是否可以访问远程IP地址:

        $ ping 8.8.8.8PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.64 bytes from 8.8.8.8: icmp_seq=1 ttl=57 time=6.67 ms64 bytes from 8.8.8.8: icmp_seq=2 ttl=57 time=55.5 ms64 bytes from 8.8.8.8: icmp_seq=3 ttl=57 time=23.0 ms64 bytes from 8.8.8.8: icmp_seq=4 ttl=57 time=245 ms

4.  Check that the date and time are set correctly on the device:
4.  检查设备的日期和时间是否已经正确配置:

        $ date

    <aside class="note">**Note:** <span>An incorrect date or time may cause SSL errors. Restart the device to automatically set the correct date and time from a time server.</span></aside>

    <aside class="note">**注意：** <span>日期或者时间错误可能导致SSL错误。 通过重启可以让设备自动从远程时间服务器进行日期和时间的同步</span></aside>

If you want to clear all of the saved networks on the board:

如果您想清除开发板上所有已保存的网络:

    $ am startservice \    -n com.google.wifisetup/.WifiSetupService \    -a WifiSetupService.Reset

## Serial debug console

## 串口调试终端

* * *

The serial console is a helpful tool for debugging your board and reviewing system log information. The console is the default output location for kernel log messages (i.e. `dmesg`), and it also provides access to a full shell prompt that you can use to access commands such as [logcat](https://developer.android.google.cn/tools/help/logcat.html). This is helpful if you are unable to access ADB on your board through other means and have not yet enabled a network connection.

串口终端是调试开发板和查看系统日志信息的有用工具。该终端是内核日志输出的默认终端 (比如 `dmesg`)，并且还提供对完整 shell 命令行的访问，您可以使用该命令行访问诸如 [logcat](https://developer.android.google.cn/tools/help/logcat.html) 这样的命令。 如果您无法通过其他方式访问开发板上的 ADB 且没有网络连接，这将为您提供帮助。

To access the serial console, connect a micro USB cable to the board as follows:

连接 micro USB 线到开发板并按照以下步骤来访问串口终端:

**For Arduino Breakout:** Connect to **J3**.

**对于 Arduino 扩展板：** 连接到 **J3**.

![""](https://developer.android.google.cn/things/images/edison-arduino-console.png)

**For Sparkfun Block:** Connect to **CONSOLE**.

**对于 Sparkfun 扩展模块：** 连接到 **CONSOLE**.

![""](https://developer.android.google.cn/things/images/edison-sparkfun-console.png)

Open a connection to the USB serial device on your development computer using a terminal program, such as [PuTTY](http://www.putty.org/) (Windows), [Serial](https://www.decisivetactics.com/products/serial/) (Mac OS), or [Minicom](https://en.wikipedia.org/wiki/Minicom) (Linux). The serial port parameters for the console are as follows:

选择一个终端程序来建立计算机和USB串口设备的连接，例如 [PuTTY](http://www.putty.org/) (Windows), [Serial](https://www.decisivetactics.com/products/serial/) (Mac OS), or [Minicom](https://en.wikipedia.org/wiki/Minicom) (Linux). 串口连接参数参考如下:

*   **Baud Rate**: 115200
*   **波特率**: 115200
*   **Data Bits**: 8
*   **数据位**: 8
*   **Parity**: None
*   **奇偶校验**: 无
*   **Stop Bits**: 1
*   **停止位**: 1
