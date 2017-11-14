# Intel® Joule™


Intel® Joule™, Intel’s highest-performing system-on-module, packs powerful compute capabilities in a thumb-sized, low-power package. The Intel® Joule™ 550x/570x developer kit enables developers to rapidly prototype all manner of autonomous robots and IoT applications requiring computer vision or edge processing.  
Intel® Joule™ 是英特尔性能最卓越的模块化系统，其在一个拇指大小的低功耗封装中实现了强大的包计算能力。对于那些需要机器视觉或边缘计算能力的机器人和物联网应用，Intel® Joule™ 550x/570x 开发套件可以快速的将创意转变成原型乃至产品。

![""](https://developer.android.google.cn/things/images/intel-joule-dev-kit.png)

## Flashing the image
## 烧录镜像

* * *

Before you begin flashing, you will need the following items in addition to your Joule board:  
开发者烧录镜像前，需要准备下面的工具:


*   Micro-USB cable

    Micro-USB 线
*   USB-C cable

    USB-C 线
*   12V 3A power adapter

    12V 3A 电源适配器
*   Micro-HDMI cable

    Micro-HDMI 线
*   MicroSD card reader

    MicroSD 读卡器
*   USB keyboard (optional)

    USB 键盘(可选)

To flash Android Things onto your board, download the preview image in the [Android Things Console](https://partner.android.com/things/console) (see the [release notes](https://developer.android.google.cn/things/preview/releases.html#developer_preview_5)) and follow these steps:

### Step 1: Install Fastboot
### 第一步：安装 Fastboot

If this is your first time installing Android Things on the Joule, you need to upgrade the BIOS and bootloader to be Fastboot capable. Follow the Intel [Getting Started Guide](https://software.intel.com/en-us/articles/installing-android-things-on-intel-joule-module) to perform the required one-time setup steps on your board.   
如果您是第一次在 Joule 上安装 Android Things ，就需要更新模块的 BIOS (基本输入输出系统)和 bootloader (引导程序)来安装快速启动功能。具体可参考 Intel  [入门手册](https://software.intel.com/en-us/articles/installing-android-things-on-intel-joule-module) ，在模组上执行一次性设置步骤。

### Step 2: Connect the Hardware
### 第二步: 连接硬件

Connect the board to your host computer as shown below:  
Joule 和计算机的连接方法，请参考以下步骤:

![""](https://developer.android.google.cn/things/images/joule-connections.png)

1.  Connect a 12V adapter to the power input connector.

      使用 12V 电源适配器和电源连接。
2.  Connect a USB-C cable from your host computer for USB OTG.

      使用 USB-C 线和计算机连接，起到 USB OTG 的作用。
3.  Connect a Micro-HDMI cable to an external display.

      使用 Micro-HDMI 线和显示器连接。
4.  Optionally, connect a USB keyboard for BIOS setup. 

      [可选] 使用 USB 键盘设置 Joule BIOS 。



### Step 3: Flash Android Things
### 第三步: 烧录 Android Things

Once you have loaded the proper bootloader on your device, use the following steps to flash the Android image:  
一旦您在设备上加载了正确的 bootloader ，请使用以下步骤来烧录 Android Things 镜像：

1.  Download and install [Android Studio](https://developer.android.google.cn/studio/index.html) or the [`sdkmanager`](https://developer.android.google.cn/studio/command-line/sdkmanager.html) command-line tool. Update the Android SDK Platform Tools to version 25.0.3 or later from the [SDK Manager](https://developer.android.google.cn/studio/intro/update.html#sdk-manager).
      下载和安装 [Android Studio](https://developer.android.google.cn/studio/index.html) 或 [`sdkmanager`](https://developer.android.google.cn/studio/command-line/sdkmanager.html) 命令行工具。从 [SDK 管理器](https://developer.android.google.cn/studio/intro/update.html#sdk-manager) 中获取 Android SDK 平台工具，将其版本更新到 25.0.3 或者更高。  

    *   Navigate to the Android SDK location on your computer; the path can be found in the system settings for Android Studio. Verify that the `fastboot` binary is installed in the `platform-tools/` directory.   

        使用计算机导航到 Android SDK 所在路径，验证 `fastboot` 二进制文件是否已经安装在 `platform-tools/` 目录下。Android SDK 路径可以在 Android Studio 的系统设置中找到。  

    *   After you have the fastboot tool, add it to your `PATH` [environment variable](https://developer.android.google.cn/studio/command-line/variables.html#set). This command should be similar to the following:  

        将 fastboot 工具增添到计算机的 `PATH` [环境变量](https://developer.android.google.cn/studio/command-line/variables.html#set)里面。命令行可参考如下：  


          `export PATH=$PATH:"path/to/fastboot"`

1.  Open a command line terminal and navigate to the unzipped image directory.

      在计算机中打开一个命令行窗口，并导航到镜像所在的解压缩目录。

2.  Verify that the device has booted into Fastboot mode by executing the following command:

      通过执行以下命令，验证设备已经进入 Fastboot 模式。  

         $ fastboot devices1b2f21d4e1fe0129        fastboot
     <aside class="note">**Note:** <span>Your device will not boot into Fastboot mode if it was previously flashed with Android Things. You need to first execute the following command using the [adb tool](https://developer.android.google.cn/tools/help/adb.html) to reboot the device into Fastboot mode.   

     <aside class="note">**注:** <span>如果设备之前烧录过 android Things ，设备开机将不会进入 Fastboot 模式。您需要使用 [adb 工具](https://developer.android.google.cn/tools/help/adb.html) 执行以下命令，重启设备进入 Fastboot 模式。


        $ adb reboot bootloader</span></aside>

1.  Execute the `flash-all.sh` script. This script installs the necessary bootloader, baseband firmware(s), and operating system. (On Windows systems, use `flash-all.bat` instead).

      执行 `flash-all.sh` 脚本，安装 bootloader ，基带固件和操作系统。( Windows 系统可以使用 `flash-all.bat` 脚本)。  

     <aside class="note">**Note:** <span>The device automatically reboots into Android Things when the process is complete.</span></aside>  

     <br /> <aside class="note">**注:** <span>安装完成后，设备将自动重启并进入 android thing。</span></aside>


1.  To verify that Android is running on the device, discover it using the [adb tool](https://developer.android.google.cn/tools/help/adb.html):

      使用 [adb tool](https://developer.android.google.cn/tools/help/adb.html) 去验证 Android 是否已经在设备在运行。

         $ adb wait-for-device...$ adb devicesList of devices attached1b2f21d4e1fe0129        device

## Connecting Wi-Fi

## 连接 Wi-Fi

* * *

After flashing your board, it is strongly recommended to connect it to the internet. This allows your device to deliver crash reports and receive updates.  

设备烧录镜像后，强烈建议您把它连接到互联网。这将有助于设备提交崩溃报告到云端，并接收云端的更新。



<aside class="note">**Note:** <span>The device doesn't need to be on the same network as your computer.</span></aside>  

<aside class="note">**注:** <span>设备和计算机不需要在同一个网络中</span></aside>

Before connecting your board to a Wi-Fi network, ensure the provided antennas are attached to the u.FL Wi-Fi connectors on your board as shown:  

设备连接 Wi-Fi 网络前，需要确保天线已经连接到u.FL Wi-Fi 连接器上，如下图所示：

![""](https://developer.android.google.cn/things/images/joule-antenna.png)

<aside class="note">**Note:** <span>The Joule can't resolve Wi-Fi signals if you proceed without connecting an antenna.</span></aside>  

<aside class="note">**注:** <span>如果您没有连接天线， Joule 就无法接收 Wi-Fi 信号。</span></aside>

To connect your board to Wi-Fi, first access a shell prompt on the device. You can use either of the following methods:  

在设备上访问 shell 提示符，将设备连接到 Wi-Fi 。您可以使用以下任何一种方法：  

*   Open a shell over adb with the `adb shell` command.  

    用 `adb shell` 命令在 adb 上打开一个 shell 。


*   Connect to the [serial console](#serial_debug_console).  

    连接[串口控制台](#serial_debug_console)。

Once you can access a shell prompt, follow these steps:  

一旦您可以访问 shell 提示符，请执行以下步骤:  

1.  Send an intent to the Wi-Fi service that includes the SSID of your local network. Your [board](https://developer.android.google.cn/things/hardware/developer-kits.html) must support the network protocol and frequency band of the wireless network in order to establish a connection.  

    为了和 Wi-Fi 建立连接，设备必须支持相关的无线网络协议和频带，将您本地网络的 SSID 通过 Wi-Fi 数据包发送出去？？？？  

                 $ am startservice \    -n com.google.wifisetup/.WifiSetupService \    -a WifiSetupService.Connect

             The following arguments are supported with this command:
             该命令支持如下参数:
             
             <table>
         
             <tbody>
         
             <tr>
         
             <th style="width: 240px;">Argument</th>
         
             <th>Description</th>
         
             </tr>
         
             <tr>
         
             <td>`-e ssid <var>network_ssid</var>`</td>
         
             <td>Connect to the wireless network SSID specified by <var>network_ssid</var>. _This argument is required_. 
             <br /> 
             设置参数<var>network_ssid</var>，连接到指定 SSID 的无线网络，参数是必选的。
             </td>
         
             </tr>
         
             <tr>
         
             <td>`-e passphrase <var>network_pass</var>`</td>
         
             <td>Optional argument to use the passcode specified by <var>network_pass</var> to connect to the network SSID. This argument is not necessary if your network doesn't require a passcode.
             <br /> 
             设置参数<var>network_ssid</var>，连接到指定 SSID 的无线网络，参数是必选的。
             </td>
         
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

2.  Verify that the connection was successful through `logcat`:

                 $ logcat -d | grep Wifi...V WifiWatcher: Network state changed to CONNECTEDV WifiWatcher: SSID changed: ...I WifiConfigurator: Successfully connected to ...

3.  Test that you can access a remote IP address:

                 $ ping 8.8.8.8PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.64 bytes from 8.8.8.8: icmp_seq=1 ttl=57 time=6.67 ms64 bytes from 8.8.8.8: icmp_seq=2 ttl=57 time=55.5 ms64 bytes from 8.8.8.8: icmp_seq=3 ttl=57 time=23.0 ms64 bytes from 8.8.8.8: icmp_seq=4 ttl=57 time=245 ms

4.  Check that the date and time are set correctly on the device:

                 $ date

             <aside class="note">**Note:** <span>An incorrect date or time may cause SSL errors. Restart the device to automatically set the correct date and time from a time server.</span></aside>

If you want to clear all of the saved networks on the board:

    $ am startservice \    -n com.google.wifisetup/.WifiSetupService \    -a WifiSetupService.Reset

## Serial debug console

* * *

The serial console is a helpful tool for debugging your board and reviewing system log information. The console is the default output location for kernel log messages (i.e. `dmesg`), and it also provides access to a full shell prompt that you can use to access commands such as [logcat](https://developer.android.google.cn/tools/help/logcat.html). This is helpful if you are unable to access ADB on your board through other means and have not yet enabled a network connection.

To access the serial console, connect a micro USB cable to the board as shown below.

![""](https://developer.android.google.cn/things/images/joule-console.png)

Open a connection to the USB serial device on your development computer using a terminal program, such as [PuTTY](http://www.putty.org/) (Windows), [Serial](https://www.decisivetactics.com/products/serial/) (Mac OS), or [Minicom](https://en.wikipedia.org/wiki/Minicom) (Linux). The serial port parameters for the console are as follows:

*   **Baud Rate**: 115200
*   **Data Bits**: 8
*   **Parity**: None
*   **Stop Bits**: 1

