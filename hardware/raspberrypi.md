# Raspberry Pi 3
Raspberry Pi 3 Model B is the latest iteration of the world's most popular single board computer. It provides a quad-core 64-bit ARM Cortex-A53 CPU running at 1.2GHz, four USB 2.0 ports, wired and wireless networking, HDMI and composite video output, and a 40-pin GPIO connector for physical interfacing projects.

![""](https://developer.android.google.cn/things/images/raspberry-pi-3-board.png)

See [Raspberry Pi I/O](https://developer.android.google.cn/things/hardware/raspberrypi-io.html) for more details on the [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) signals on this board.

## Flashing the image

* * *

Before you begin flashing, you will need the following items in addition to your Raspberry Pi:

*   HDMI cable
*   HDMI-enabled display
*   Micro-USB cable
*   Ethernet cable
*   MicroSD card reader

To flash Android Things onto your board, download the latest preview image in the [Android Things Console](https://partner.android.com/things/console) (see the [release notes](https://developer.android.google.cn/things/preview/releases.html)) and follow these steps:

1.  Insert an 8 GB or larger microSD card into your development computer.
2.  Unzip the downloaded image archive on your development computer. Navigate to the unzipped image file.

    <aside class="note">**Note:** <span>The compressed image file expands to over 4GB. This can cause problems for the built-in tools on some platforms. If you are unable to unzip the archive, or see a message stating that it's corrupt, use [7-Zip](http://www.7-zip.org/download.html) (Windows) or [The Unarchiver](http://unarchiver.c3.cx/unarchiver) (Mac OS) instead.</span></aside>

3.  Follow the official Raspberry Pi instructions for writing the image to the SD card:

    *   [Linux](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)
    *   [Mac](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)
    *   [Windows](https://www.raspberrypi.org/documentation/installation/installing-images/windows.md)
4.  Insert the flashed microSD card into your board.

5.  Make the following connections to your board:

    ![""](https://developer.android.google.cn/things/images/raspberrypi-connections.png)

    1.  Connect a USB cable to **J1** for power.
    2.  Connect an Ethernet cable to your local network.

        <aside class="note">**Note:** <span>If you do not have wired access to your local network, you can do either of the following:</span></aside>

        *   Connect the Ethernet cable to your development computer and assign the Raspberry Pi an IP address using DHCP.

        *   Connect a [serial cable](#serial_debug_console) from the Raspberry Pi to your development computer. Use a serial console to [connect to Wi-Fi](#connecting_wi-fi).

    3.  Connect an HDMI cable to an external display.

6.  Verify that Android is running on the device. The Android Things Launcher shows information about the board on the display, including the IP address.

7.  Connect to this IP address using the [adb tool](https://developer.android.google.cn/tools/help/adb.html):

        $ adb connect <ip-address>connected to <ip-address>:5555

    <aside class="note">**Note:** <span>Raspberry Pi broadcasts the hostname `Android.local` over Multicast DNS. If your host platform supports MDNS, you can also connect to the board using the following command:

        $ adb connect Android.local</span></aside>

## Connecting Wi-Fi

* * *

After flashing your board, it is strongly recommended to connect it to the internet. This allows your device to deliver crash reports and receive updates.

<aside class="note">**Note:** <span>The device doesn't need to be on the same network as your computer.</span></aside>

To connect your board to Wi-Fi, first access a shell prompt on the device. You can use either of the following methods:

*   Connect your board to your Wi-Fi router or development computer to assign it an IP address. Run the `adb connect <ip-address>` command to connect to this IP address using the [adb tool](https://developer.android.google.cn/tools/help/adb.html). Open a shell over adb with the `adb shell` command.
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

To access the serial console, connect a [USB to TTL Serial Cable](https://www.adafruit.com/products/954) to the device UART pins as shown below.

![""](https://developer.android.google.cn/things/images/raspberrypi-console.png)

Open a connection to the USB serial device on your development computer using a terminal program, such as [PuTTY](http://www.putty.org/) (Windows), [Serial](https://www.decisivetactics.com/products/serial/) (Mac OS), or [Minicom](https://en.wikipedia.org/wiki/Minicom) (Linux). The serial port parameters for the console are as follows:

*   **Baud Rate**: 115200
*   **Data Bits**: 8
*   **Parity**: None
*   **Stop Bits**: 1

## Pin multiplexing

* * *

The Raspberry Pi has pins that are multiplexed between various board functions. Some board functions cannot be used simultaneously (for example, enabling Bluetooth and using the `UART0` port for peripheral I/O). For more information, see the [function mode matrix](https://developer.android.google.cn/things/hardware/raspberrypi-mode-matrix.html).

