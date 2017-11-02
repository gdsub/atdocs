# NXP i.MX6UL

Expanding the i.MX 6 series, the i.MX 6UltraLite is a high performance, ultra-efficient processor family featuring an advanced implementation of a single ARM® Cortex®-A7 core. The Pico variant is pin-compatible with the Intel® Edison for sensors and low-speed I/O, but also adds additional expansion possibilities for multimedia and connectivity, giving you cutting edge technology that can easily be expanded and implemented for IoT designs.

![](https://developer.android.google.cn/things/images/nxp-pico7-board.png) ![](https://developer.android.google.cn/things/images/nxp-spriot-board.png) ![](https://developer.android.google.cn/things/images/nxp-argon-board.png)

## Flashing the image

* * *

Before you begin flashing, you will need the following items in addition to your board:

*   USB-C or Micro-USB cable
*   5V DC power adapter (not needed for the Pico i.MX6UL board)

To flash Android Things onto your board, download the latest preview image in the [Android Things Console](https://partner.android.com/things/console) (see the [release notes](https://developer.android.google.cn/things/preview/releases.html)) and follow these steps:

### Step 1: Connect the Hardware

Connect the board to your host computer:

**For Pico i.MX6UL:**

![""](https://developer.android.google.cn/things/images/pico7-connections.png)

1.  Connect a USB-C cable from your host computer to the USB OTG connector.

**For SprIoT i.MX6UL:**

![""](https://developer.android.google.cn/things/images/spriot-connections.png)

1.  Connect a Micro-USB cable to the USB OTG connector.
2.  Connect a 5V power adapter to the power input connector.

**For Argon i.MX6UL:**

![""](https://developer.android.google.cn/things/images/vvdn-connections.png)

1.  Ensure switch **SW1** is in the **OFF** position.
2.  Connect a Micro-USB cable to the **OTG** (**J7**) connector.
3.  Connect a 5V power adapter to the power input (**J2**) connector.
4.  Move **SW1** to the **ON** position to power the board.

### Step 2: Flash Android Things

Use the following steps to flash the Android image:

1.  Download and install [Android Studio](https://developer.android.google.cn/studio/index.html) or the [`sdkmanager`](https://developer.android.google.cn/studio/command-line/sdkmanager.html) command-line tool. Update the Android SDK Platform Tools to version 25.0.3 or later from the [SDK Manager](https://developer.android.google.cn/studio/intro/update.html#sdk-manager).

    *   Navigate to the Android SDK location on your computer; the path can be found in the system settings for Android Studio. Verify that the `fastboot` binary is installed in the `platform-tools/` directory.

    *   After you have the fastboot tool, add it to your `PATH` [environment variable](https://developer.android.google.cn/studio/command-line/variables.html#set). This command should be similar to the following:

        `export PATH=$PATH:"path/to/fastboot"`

2.  Open a command line terminal and navigate to the unzipped image directory.

3.  Verify that the device has booted into Fastboot mode by executing the following command:

        $ fastboot devices1b2f21d4e1fe0129        fastboot

    <aside class="note">**Note:** <span>Your device will not boot into Fastboot mode if it was previously flashed with Android Things. You need to first execute the following command using the [adb tool](https://developer.android.google.cn/tools/help/adb.html) to reboot the device into Fastboot mode.

        $ adb reboot bootloader</span></aside>

4.  Execute the `flash-all.sh` script. This script installs the necessary bootloader, baseband firmware(s), and operating system. (On Windows systems, use `flash-all.bat` instead).

    <aside class="note">**Note:** <span>The device automatically reboots into Android Things when the process is complete.</span></aside>

5.  To verify that Android is running on the device, discover it using the [adb tool](https://developer.android.google.cn/tools/help/adb.html):

        $ adb wait-for-device...$ adb devicesList of devices attached1b2f21d4e1fe0129        device

## Connecting Wi-Fi

* * *

After flashing your board, it is strongly recommended to connect it to the internet. This allows your device to deliver crash reports and receive updates.

<aside class="note">**Note:** <span>The device doesn't need to be on the same network as your computer.</span></aside>

Before connecting your board to a Wi-Fi network, attach an external IPEX or u.FL Wi-Fi antenna to your board as shown:

**For Pico i.MX6UL:**

![""](https://developer.android.google.cn/things/images/pico7-antenna.png)

**For SprIoT i.MX6UL:**

![""](https://developer.android.google.cn/things/images/spriot-antenna.png)

**For Argon i.MX6UL:**

![""](https://developer.android.google.cn/things/images/vvdn-antenna.png)

<aside class="note">**Note:** <span>The module can't resolve Wi-Fi signals if you proceed without connecting an antenna.</span></aside>

To connect your board to Wi-Fi, first access a shell prompt on the device. You can use either of the following methods:

*   Open a shell over adb with the `adb shell` command.
*   Connect to the [serial console](#serial_debug_console).

Once you can access a shell prompt, follow these steps:

1.  Send an intent to the Wi-Fi service that includes the SSID of your local network. Your [board](https://developer.android.google.cn/things/hardware/developer-kits.html) must support the network protocol and frequency band of the wireless network in order to establish a connection.

        $ am startservice \    -n com.google.wifisetup/.WifiSetupService \    -a WifiSetupService.Connect

    The following arguments are supported with this command:

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

To access the serial console:

**For Pico i.MX6UL:** Connect a micro USB cable to the debug interface as shown below.

![""](https://developer.android.google.cn/things/images/pico7-console.png)

**For SprIoT i.MX6UL:** Connect a Micro-USB cable to the board as shown below.

![""](https://developer.android.google.cn/things/images/spriot-console.png)

**For Argon i.MX6UL:** Connect a USB Type B cable to the board as shown below.

![""](https://developer.android.google.cn/things/images/vvdn-console.png)

Open a connection to the USB serial device on your development computer using a terminal program, such as [PuTTY](http://www.putty.org/) (Windows), [Serial](https://www.decisivetactics.com/products/serial/) (Mac OS), or [Minicom](https://en.wikipedia.org/wiki/Minicom) (Linux). The serial port parameters for the console are as follows:

*   **Baud Rate**: 115200
*   **Data Bits**: 8
*   **Parity**: None
*   **Stop Bits**: 1

