# Connect the Hardware
## This lesson teaches you to

1.  Connect a button to GPIO
2.  Connect an LED to GPIO

## You should also read

*   [Hardware 101](https://developer.android.google.cn/things/hardware/hardware-101.html)

Before writing any code, you need to connect peripherals from your development kit to your board.

In this lesson, you will learn to wire a pushbutton switch and LED from a breadboard to your device. To connect the peripherals to your board:

1.  Connect one side of the button to the chosen GPIO input pin, and the other side to ground.
2.  Connect the same GPIO input pin to +3.3V through a pull-up resistor.
3.  Connect the chosen GPIO output pin to one side of a series resistor.
4.  Connect the other side of the resistor to the anode side (longer lead) of the LED.
5.  Connect the cathode side (shorter lead) of the LED to ground.

<aside class="note">**Note:** <span>See [Hardware 101](https://developer.android.google.cn/things/hardware/hardware-101.html) for more detail on connecting input and output components.</span></aside>

For this lesson, the following GPIO pins are assumed on each board:

<table style="width:480px;">

<tbody>

<tr>

<th width="33%">Board</th>

<th>Input Signal</th>

<th>Output Signal</th>

</tr>

<tr>

<td>

[Intel Edison Arduino](https://developer.android.google.cn/things/hardware/edison-arduino-io.html)

</td>

<td>IO12</td>

<td>IO13</td>

</tr>

<tr>

<td>

[Intel Edison Sparkfun](https://developer.android.google.cn/things/hardware/edison-sparkfun-io.html)

</td>

<td>GP44</td>

<td>GP45</td>

</tr>

<tr>

<td>

[Intel Joule](https://developer.android.google.cn/things/hardware/joule.html)

</td>

<td>J7_71</td>

<td>J6_25</td>

</tr>

<tr>

<td>

[NXP Pico i.MX6UL](https://developer.android.google.cn/things/hardware/imx6ul-pico-io.html)

</td>

<td>GPIO4_IO20</td>

<td>GPIO4_IO21</td>

</tr>

<tr>

<td>

[NXP Pico i.MX7D](https://developer.android.google.cn/things/hardware/imx7d-pico-io.html)

</td>

<td>GPIO_39</td>

<td>GPIO_34</td>

</tr>

<tr>

<td>

[Raspberry Pi](https://developer.android.google.cn/things/hardware/raspberrypi-io.html)

</td>

<td>BCM21</td>

<td>BCM6</td>

</tr>

</tbody>

</table>

![""](https://developer.android.google.cn/things/images/simplepio-wiring.png)

