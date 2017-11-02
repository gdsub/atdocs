# Pico Pro Maker Kit

Follow these instructions to set up your [Pico Pro Maker Kit](https://www.technexion.com/solutions/iot-development-platform/android-things/).

## What's in the box

* * *

Open the box and make sure you have all of the components in the kit. You will also need a small Phillips screwdriver (not provided).

<aside class="note">**Note:** <span>Some versions of the Pico Pro Maker Kit do not include the Rainbow HAT, camera, and/or multi-touch display. The full kit is shown below.</span></aside>

![inventory](https://developer.android.google.cn/things/images/imx7d-kit/inventory.jpg)

<table>

<tbody>

<tr>

<td style="width:450px;">

![Bubble_number_01](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_01.png)

Pico i.MX7Dual development board
</td>

<td>

![Bubble_number_02](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_02.png)

Standoffs (x2) and screws (x5) for the Rainbow HAT</td>

</tr>

<tr>

<td>

![Bubble_number_03](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_03.png)

Rainbow HAT</td>

<td>

![Bubble_number_04](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_04.png)

USB-C cable</td>

</tr>

<tr>

<td>

![Bubble_number_05](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_05.png)

Wifi antenna</td>

<td>

![Bubble_number_06](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_06.png)

Antenna extender cable</td>

</tr>

<tr>

<td>

![Bubble_number_07](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_07.png)

Camera module</td>

<td>

![Bubble_number_08](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_08.png)

Camera module cable</td>

</tr>

<tr>

<td>

![Bubble_number_09](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_09.png)

5" multi-touch display</td>

<td>

![Bubble_number_10](https://developer.android.google.cn/things/images/common/bubble-numbers/Bubble_number_10.png)

Display 6-wire cable</td>

</tr>

</tbody>

</table>

## Connect the parts

* * *

Connect the parts in the following order. Note that some versions of the Pico Pro Maker Kit do not include the Rainbow HAT, camera, and/or multi-touch display.

### Wifi antenna

Connect the wifi antenna to the development board:

<table>

<tbody>

<tr>

<td style="width:450px;">

![antenna_step1](https://developer.android.google.cn/things/images/imx7d-kit/antenna_step1.jpg)

<span style="font-size:1.2em; vertical-align: middle;">①</span> Locate the wifi antenna and extender cable. Screw the cable into the base of the antenna.</td>

<td style="width:450px;">

![antenna_step2](https://developer.android.google.cn/things/images/imx7d-kit/antenna_step2.jpg)

<span style="font-size:1.2em; vertical-align: middle;">②</span> Locate the round antenna pin on the development board (pictured above). Press the small round connector at the end of the extender cable onto this pin.</td>

</tr>

</tbody>

</table>

### Camera

Connect the camera to the development board:

<table>

<tbody>

<tr>

<td style="width:450px;">

![camera_step1](https://developer.android.google.cn/things/images/imx7d-kit/camera_step1.jpg)

<span style="font-size:1.2em; vertical-align: middle;">①</span> Locate the camera module and cable. Turn the board over to reveal a white connector near the edge of the board. Swivel the black retaining clip upward. Insert one end of the camera module cable into the white connector. The silver pins on the cable should be facing down. Swivel the retaining clip back down to hold the cable in place.</td>

<td style="width:450px;">

![camera_step2](https://developer.android.google.cn/things/images/imx7d-kit/camera_step2.jpg)

<span style="font-size:1.2em; vertical-align: middle;">②</span> Turn over the camera module to reveal a white connector near the edge of the board. Swivel the black retaining clip upward. Insert one end of the camera module cable into the white connector. The silver pins on the cable should be facing down. Swivel the retaining clip back down to hold the cable in place.</td>

</tr>

</tbody>

</table>

### Rainbow HAT

Connect the Rainbow HAT to the development board:

<table>

<tbody>

<tr>

<td style="width:450px;">

![hat_step1](https://developer.android.google.cn/things/images/imx7d-kit/hat_step1.jpg)

<span style="font-size:1.2em; vertical-align: middle;">①</span> Locate the standoffs and screws. Screw the standoffs into the two holes on the development board (pictured above). One hole is located near the USB-C connector; the other hole is located next to the audio connector.</td>

<td style="width:450px;">

![hat_step2](https://developer.android.google.cn/things/images/imx7d-kit/hat_step2.jpg)

<span style="font-size:1.2em; vertical-align: middle;">②</span> Locate the Rainbow HAT and turn it over. The HAT has a 40-pin female connector. The development board has a 40-pin male connector. You will connect these together in the next step.</td>

</tr>

<tr>

<td>

![hat_step3](https://developer.android.google.cn/things/images/imx7d-kit/hat_step3.jpg)

<span style="font-size:1.2em; vertical-align: middle;">③</span> Gently press the connector on the back of the Rainbow HAT onto the connector on the development board. Be sure to press straight down. Note that the HAT should not come in contact with the development board, especially the small, square system on a module (SoM) board attached to the main board.</td>

<td>

![hat_step4](https://developer.android.google.cn/things/images/imx7d-kit/hat_step4.jpg)

<span style="font-size:1.2em; vertical-align: middle;">④</span> Use two screws to secure the standoffs to the Rainbow HAT.</td>

</tr>

</tbody>

</table>

### Multi-touch display

Connect the display to the development board:

<aside class="note">**Note:** <span>For illustrative purposes, the connected camera module is not pictured in the following images.</span></aside>

<table>

<tbody>

<tr>

<td style="width:450px;">

![display_step1](https://developer.android.google.cn/things/images/imx7d-kit/display_step1.jpg)

<span style="font-size:1.2em; vertical-align: middle;">①</span> Locate the i.MX7Dual development board. Turn it over to reveal a white connector near the center of the board. Swivel the black retaining clip upward.</td>

<td style="width:450px;">

![display_step2](https://developer.android.google.cn/things/images/imx7d-kit/display_step2.jpg)

<span style="font-size:1.2em; vertical-align: middle;">②</span> Locate the multi-touch display and turn it over. The display has a flat ribbon cable connected to it; you may need to remove some tape securing it to the back of the display. Insert the flat ribbon cable into the white connector on the board. Swivel the retaining clip back down to hold the cable in place.</td>

</tr>

<tr>

<td>

![display_step3](https://developer.android.google.cn/things/images/imx7d-kit/display_step3.jpg)

<span style="font-size:1.2em; vertical-align: middle;">③</span> Locate the 6-wire cable for the display. Insert one end of the cable into the connector perpendicular to the ribbon cable connector. The gold contacts on the cable should be facing up.</td>

<td>

![display_step4](https://developer.android.google.cn/things/images/imx7d-kit/display_step4.jpg)

<span style="font-size:1.2em; vertical-align: middle;">④</span> Turn both the display and the development board over. The display has a 6-wire connector; you may need to remove some tape securing it to the back of the display. Connect the remaining end of the 6-wire cable to this connector. The gold contacts on the cable should be facing up. Note that you will need to twist the 6-wire cable slightly.</td>

</tr>

</tbody>

</table>

### Final result

Your kit is now assembled. You will connect the USB-C cable when you flash Android Things on your board.

![final_result](https://developer.android.google.cn/things/images/imx7d-kit/final_result.jpg)

## Install Android Things

* * *

Follow the hardware setup [instructions](https://developer.android.google.cn/things/hardware/imx7d.html) to flash the latest version of Android Things on your i.MX7D development board.

## Blink an LED

* * *

If you have a Rainbow HAT, follow these instructions to download and run a [sample](https://github.com/androidthings/sample-button/) that blinks one of the HAT's LEDs. If you don't have a Rainbow HAT, you can [connect an LED and button](https://developer.android.google.cn/things/training/first-device/connect-hardware.html) to the development board and run the sample.

1.  Download and unzip the [sample-button](https://github.com/androidthings/sample-button/archive/master.zip) project to the directory of your choice.

2.  Run the project using either of the following:

    *   In Android Studio, select **File > Open** and select the directory where you unzipped the sample. Select **Run > Run 'app'**.

    *   From the command line:

            cd sample-button-master./gradlew assembleDebugadb install -g -r app/build/outputs/apk/app-debug.apkadb shell am start com.example.androidthings.button/.ButtonActivity

3.  Press the "A" button on the Rainbow HAT and the red LED will light up. If you connected an external button and LED, press the button to light the LED.

## Next steps

* * *

*   Try one of the [other code samples](https://developer.android.google.cn/things/sdk/samples.html). Remember to uninstall any existing samples from the development board before installing a new one, so that one does not interfere with the other. For example, to uninstall the previous samples from the command line:

        adb uninstall com.example.androidthings.button

*   Take a look at the [Peripherals codelab](https://codelabs.developers.google.com/codelabs/androidthings-peripherals) or learn how to [build your first device](https://developer.android.google.cn/things/training/first-device/index.html).

*   Connect with the community at [g.co/iotdev](https://g.co/iotdev).

## Troubleshooting

* * *

*   Some USB ports don't provide enough power for the development board. If your board will not boot (or reboot) after some time, try using a powered USB hub or a USB 3.0 port.

*   If you are using a native USB-C port on your host machine and the development board keeps rebooting, you may need to use a USB-A port on your host machine instead. You will need a USB-A to USB-C adapter cable.

You can [report bugs](https://issuetracker.google.com/issues/new?component=192720&template=847005) and [suggest new features](https://issuetracker.google.com/issues/new?component=192720&template=848805) with the Android Things issue tracker.

