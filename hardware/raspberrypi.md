# Raspberry Pi 3
# Raspberry Pi 3
Raspberry Pi 3 Model B is the latest iteration of the world's most popular single board computer. It provides a quad-core 64-bit ARM Cortex-A53 CPU running at 1.2GHz, four USB 2.0 ports, wired and wireless networking, HDMI and composite video output, and a 40-pin GPIO connector for physical interfacing projects.

Raspberry Pi 3B 型是这个全世界最受欢迎单片计算机系列的最新型号。它有一个四核64位 ARM Cortex-A53 1.2GHz 处理器、四个 USB 2.0 接口、支持有线和无线网络、HDMI口和复合端子输出、以及用来连接外界交互设备的一个40针的 GPIO 接口 

![""](https://developer.android.google.cn/things/images/raspberry-pi-3-board.png)

See [Raspberry Pi I/O](https://developer.android.google.cn/things/hardware/raspberrypi-io.html) for more details on the [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) signals on this board.

参照 [Raspberry Pi I/O](https://developer.android.google.cn/things/hardware/raspberrypi-io.html) 获取更多此板 [外围 I/O](https://developer.android.google.cn/things/sdk/pio/index.html) 的信息
## Flashing the image
## 在开发板上烧录程序映像
* * *

Before you begin flashing, you will need the following items in addition to your Raspberry Pi:

在开始烧录之前，除了 Raspberry Pi 以外，你还需要以下设备

*   HDMI cable

*	HDMI 线
*   HDMI-enabled display

*	支持 HDMI 的显示器
*   Micro-USB cable

*	Micro-USB 线
*   Ethernet cable

*	网线
*   MicroSD card reader

*	MicroSD 读卡器

To flash Android Things onto your board, download the latest preview image in the [Android Things Console](https://partner.android.com/things/console) (see the [release notes](https://developer.android.google.cn/things/preview/releases.html)) and follow these steps:

为了在板上烧录 Android Things ，请从 [Android Things 控制台](https://partner.android.com/things/console) 下载最新的预览版映像 (参见 [更新注释](https://developer.android.google.cn/things/preview/releases.html) 然后参照以下步骤：

1.  Insert an 8 GB or larger microSD card into your development computer.

    在你的用于开发的电脑上插入 8 GB 或更大的 microSD 卡
2.  Unzip the downloaded image archive on your development computer. Navigate to the unzipped image file.

    解压缩你的电脑上下载的映像。找到解压缩映像的文件。

    <aside class="note">**Note:** <span>The compressed image file expands to over 4GB. This can cause problems for the built-in tools on some platforms. If you are unable to unzip the archive, or see a message stating that it's corrupt, use [7-Zip](http://www.7-zip.org/download.html) (Windows) or [The Unarchiver](http://unarchiver.c3.cx/unarchiver) (Mac OS) instead.</span></aside>
    
    **注意** 压缩文件解压后会超过 4 GB 。这可能导致一些平台上内置的解压缩工具出现问题。如果内置工具不能解压文件，或者工具崩溃，可以尝试 [7-Zip](http://www.7-zip.org/download.html) (Windows) 或者 [The Unarchiver](http://unarchiver.c3.cx/unarchiver) (Mac OS)

3.  Follow the official Raspberry Pi instructions for writing the image to the SD card:

    按照以下官方 Raspberry Pi 指南将映像写入 SD 卡：

    *   [Linux](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)
    *   [Mac](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)
    *   [Windows](https://www.raspberrypi.org/documentation/installation/installing-images/windows.md)
4.  Insert the flashed microSD card into your board.

    将烧录好的 microSD 卡插入到你的开发板中 
    
5.  Make the following connections to your board:

    按照以下指南连接开发板上的接口


    ![""](https://developer.android.google.cn/things/images/raspberrypi-connections.png)

    1.  Connect a USB cable to **J1** for power.
	
        将 USB 线和 **J1** 口相连用来供电
    2.  Connect an Ethernet cable to your local network.
	
        将网线连到你的本地网络

        <aside class="note">**Note:** <span>If you do not have wired access to your local network, you can do either of the following:</span></aside>
		
		**注意:** <span>如果你没有有线网络，你可以从以下操作中任选其一：

        *   Connect the Ethernet cable to your development computer and assign the Raspberry Pi an IP address using DHCP.

		*	将网线连接到你的电脑上并通过 DHCP 给 Raspberry Pi 分配一个 IP 地址
        *   Connect a [serial cable](#serial_debug_console) from the Raspberry Pi to your development computer. Use a serial console to [connect to Wi-Fi](#connecting_wi-fi).
		
		*	用一根 [串行线](#serial_debug_console) 将 Raspberry Pi 和你的电脑相连。使用一个串行控制台 [连接到 Wi-Fi](#connecting_wi-fi)。

    3.  Connect an HDMI cable to an external display.
	
        将 HDMI 线连接到外置屏幕

6.  Verify that Android is running on the device. The Android Things Launcher shows information about the board on the display, including the IP address.

    确认设备 Android 系统运行正常。 Android Things 启动器在屏幕上显示包含 IP 地址在内的关于开发板的信息。

7.  Connect to this IP address using the [adb tool](https://developer.android.google.cn/tools/help/adb.html):

    使用 [adb 工具](https://developer.android.google.cn/tools/help/adb.html) 连接到以下 IP 地址：

        $ adb connect <ip-address>connected to <ip-address>:5555

    <aside class="note">**Note:** <span>Raspberry Pi broadcasts the hostname `Android.local` over Multicast DNS. If your host platform supports MDNS, you can also connect to the board using the following command:
						**注意** Raspberry Pi 会覆盖组播 DNS 传输主机名称 `Android.local` 。如果你的寄存平台支持 MDNS，你也可以通过以下操作连接开发板：
    
        $ adb connect Android.local</span></aside>

## Connecting Wi-Fi

## 连接 Wi-Fi

* * *

After flashing your board, it is strongly recommended to connect it to the internet. This allows your device to deliver crash reports and receive updates.

在烧录完你的开发板之后，强烈推荐将它连上网。这可以让你的设备发送崩溃报告和获取更新。

<aside class="note">**Note:** <span>The device doesn't need to be on the same network as your computer.</span>
					**注意**你的设备不必和你的电脑使用相同的网络。</aside>

To connect your board to Wi-Fi, first access a shell prompt on the device. You can use either of the following methods:

为了将你的开发板连到 Wi-Fi ，首先访问设备上的 shell prompt 。你可以从以下操作中任选其一：

*   Connect your board to your Wi-Fi router or development computer to assign it an IP address. Run the `adb connect <ip-address>` command to connect to this IP address using the [adb tool](https://developer.android.google.cn/tools/help/adb.html). Open a shell over adb with the `adb shell` command.

*	通过将开发板和无线路由器或者电脑相连给开发板分配 IP 地址。使用 [adb 工具](https://developer.android.google.cn/tools/help/adb.html)，运行 `adb connect <ip-address>` 命令连接到 IP 地址。用 `adb shell` 命令打开一个 adb shell 。
*   Connect to the [serial console](#serial_debug_console).

*	连接到 [串行控制台](#serial_debug_console)。

Once you can access a shell prompt, follow these steps:

访问 Shell prompt 后，参照以下步骤：
1.  Send an intent to the Wi-Fi service that includes the SSID of your local network. Your [board](https://developer.android.google.cn/things/hardware/developer-kits.html) must support the network protocol and frequency band of the wireless network in order to establish a connection.

    向 Wi-Fi 服务发送带有你的本地网络 SSID 的请求。你的 [开发板](https://developer.android.google.cn/things/hardware/developer-kits.html) 必须支持无线网络协议和无线网络频段以建立连接。

        $ am startservice \    -n com.google.wifisetup/.WifiSetupService \    -a WifiSetupService.Connect

The following arguments are supported with this command:

此命令支持以下参数：


<table>

<tbody>

<tr>

<th style="width: 240px;">Argument

参数</th>

<th>Description

具体细节</th>

</tr>

<tr>

<td>`-e ssid <var>network_ssid</var>`</td>

<td>Connect to the wireless network SSID specified by <var>network_ssid</var>. _This argument is required_.
	连接到由 <var>network_ssid</var> 指定的无线网络 SSID 。此参数为必须参数</td>

</tr>

<tr>

<td>`-e passphrase <var>network_pass</var>`</td>

<td>Optional argument to use the passcode specified by <var>network_pass</var> to connect to the network SSID. This argument is not necessary if your network doesn't require a passcode.

可选参数，通过 <var>network_pass</var> 指定的密码来连接网络 SSID 。如果你的网络不需要密码，无需添加此参数。</td>

</tr>

<tr>

<td>`-e passphrase64 <var>encoded_pass</var>`</td>

<td>Optional argument used in place of `passphrase` for passcodes with special characters (`space, !, ", $, &, ', (, ), ;, <, >, `, or |`). Use [base64 encoding](https://www.base64encode.org/) to specify the value for <var>encoded_pass</var>. 
可选参数，设置密码 `passphrase` 可用特殊字符 (`space, !, ", $, &, ', (, ), ;, <, >, `, or |`)。使用 [base64 encoding](https://www.base64encode.org/) 来指定 <var>encoded_pass</var> 的值</td>

</tr>

<tr>

<td>`--ez hidden true`</td>

<td>Optional argument to indicate that the SSID specified in this command is hidden. If omitted, this value defaults to false.
可选参数，用来表明此命令中的 SSID 不可见。如果省略，此值会被默认为 false</td>

</tr>

</tbody>

</table>

2.  Verify that the connection was successful through `logcat`:

    通过 `logcat` 确认连接成功：

        $ logcat -d | grep Wifi...V WifiWatcher: Network state changed to CONNECTEDV WifiWatcher: SSID changed: ...I WifiConfigurator: Successfully connected to ...

3.  Test that you can access a remote IP address:

    测试你可以访问远程 IP 地址：

        $ ping 8.8.8.8PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.64 bytes from 8.8.8.8: icmp_seq=1 ttl=57 time=6.67 ms64 bytes from 8.8.8.8: icmp_seq=2 ttl=57 time=55.5 ms64 bytes from 8.8.8.8: icmp_seq=3 ttl=57 time=23.0 ms64 bytes from 8.8.8.8: icmp_seq=4 ttl=57 time=245 ms

4.  Check that the date and time are set correctly on the device:

    确认设备上的日期与时间设置正确：

        $ date

    <aside class="note">**Note:** <span>An incorrect date or time may cause SSL errors. Restart the device to automatically set the correct date and time from a time server.</span>
						**注意** <span>不正确的日期或者时间可能造成 SSL 错误。重启设备从服务器自动获取正确的日期和时间</aside>

If you want to clear all of the saved networks on the board:

如果你要清空开发板上所有已存的网络，使用以下命令：

    $ am startservice \    -n com.google.wifisetup/.WifiSetupService \    -a WifiSetupService.Reset

## Serial debug console

## 串行调试控制台

* * *

The serial console is a helpful tool for debugging your board and reviewing system log information. The console is the default output location for kernel log messages (i.e. `dmesg`), and it also provides access to a full shell prompt that you can use to access commands such as [logcat](https://developer.android.google.cn/tools/help/logcat.html). This is helpful if you are unable to access ADB on your board through other means and have not yet enabled a network connection.

串行控制台是一个用于调试你的开发板和浏览系统信息非常有效的工具。控制台是默认的内核日志信息输出地址(`dmesg`)。
To access the serial console, connect a [USB to TTL Serial Cable](https://www.adafruit.com/products/954) to the device UART pins as shown below.

你可以通过以下的方式连接一条 [USB to TTL 串行线](https://www.adafruit.com/products/954) 到设备的 UART 口来访问串行控制台
![""](https://developer.android.google.cn/things/images/raspberrypi-console.png)

Open a connection to the USB serial device on your development computer using a terminal program, such as [PuTTY](http://www.putty.org/) (Windows), [Serial](https://www.decisivetactics.com/products/serial/) (Mac OS), or [Minicom](https://en.wikipedia.org/wiki/Minicom) (Linux). The serial port parameters for the console are as follows:

使用一个终端程序，比如 [PuTTY](http://www.putty.org/) (Windows)、[Serial](https://www.decisivetactics.com/products/serial/) (Mac OS)、[Minicom](https://en.wikipedia.org/wiki/Minicom) (Linux)，打开在你的电脑上到 USB 设备的连接。具体控制台串行接口参数如下：
*   **Baud Rate**: 115200

*	**波特率**： 115200
*   **Data Bits**: 8

*	**数据字节**： 8
*   **Parity**: None

*	**奇偶校检**： 无
*   **Stop Bits**: 1

*	**停止字节**： 1

## Pin multiplexing

## 串口复用

* * *

The Raspberry Pi has pins that are multiplexed between various board functions. Some board functions cannot be used simultaneously (for example, enabling Bluetooth and using the `UART0` port for peripheral I/O). For more information, see the [function mode matrix](https://developer.android.google.cn/things/hardware/raspberrypi-mode-matrix.html).

Raspberry Pi 上有复用的串口可以在不同的板载功能中切换。一些板载的功能不能同时使用(比如打开蓝牙和将 `UART0` 用为外围接口)。更多信息可以参考 [函数模式矩阵](https://developer.android.google.cn/things/hardware/raspberrypi-mode-matrix.html)

