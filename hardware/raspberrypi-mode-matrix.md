# Raspberry Pi 3 Function Mode Matrix

The following modes in each table are mutually exclusive on the Raspberry Pi 3.

## UART modes

* * *

The Raspberry Pi has a single full-speed UART (**UART0**) and a mini UART (**MINIUART**); see the [official docs](https://www.raspberrypi.org/documentation/configuration/uart.md) for information on their differences. These UARTs are multiplexed between various board functions and cannot be used simultaneously. The following modes are supported:

<table>

<thead>

<tr>

<th>Mode</th>

<th>Activated By</th>

<th>Bluetooth</th>

<th>Pin Functions</th>

</tr>

</thead>

<tbody>

<tr>

<td>Debug console</td>

<td>Default mode; no PIO connections</td>

<td>Enabled</td>

<td>Pins BCM14/BCM15 expose RX/TX of the [serial debug console](https://developer.android.google.cn/things/hardware/raspberrypi.html#serial_debug_console)</td>

</tr>

<tr>

<td>UART0</td>

<td>UART0 opened by PIO</td>

<td>Disabled</td>

<td>Pins BCM14/BCM15 expose RX/TX of [UART0](https://developer.android.google.cn/things/sdk/pio/uart.html)</td>

</tr>

<tr>

<td>MINIUART</td>

<td>MINIUART opened by PIO</td>

<td>Enabled</td>

<td>Pins BCM14/BCM15 expose RX/TX of [MINIUART](https://developer.android.google.cn/things/sdk/pio/uart.html)</td>

</tr>

<tr>

<td>BCM14 or BCM15</td>

<td>Pin opened by PIO</td>

<td>Enabled</td>

<td>Named pin (BCM14 or BCM15) is [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html), other pin is idle</td>

</tr>

</tbody>

</table>

An `IOException` error is thrown if you try to open an active pin (from above) using a different UART mode.

## Audio modes

* * *

The Raspberry Pi uses a shared clock signal for the PWM drivers and the audio subsystem (I2S and analog). Analog audio is transmitted through the 3.5mm audio jack. The following modes are supported:

<table>

<thead>

<tr>

<th>Mode</th>

<th>Activated By</th>

<th>Analog Audio</th>

<th>Pin Functions</th>

</tr>

</thead>

<tbody>

<tr>

<td>Audio</td>

<td>Default mode; no PIO connections</td>

<td>Enabled</td>

<td>N/A</td>

</tr>

<tr>

<td>PWM0</td>

<td>PWM0 opened by PIO</td>

<td>Disabled</td>

<td>Pin BCM18 enabled as [PWM](https://developer.android.google.cn/things/sdk/pio/pwm.html)</td>

</tr>

<tr>

<td>I2S1</td>

<td>I2S1 opened by PIO</td>

<td>Enabled</td>

<td>Pin BCM18 enabled as [I2S](https://developer.android.google.cn/things/sdk/pio/i2s.html) BCLK</td>

</tr>

<tr>

<td>BCM18</td>

<td>Pin opened by PIO</td>

<td>Enabled</td>

<td>Named pin (BCM18) is [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html)</td>

</tr>

</tbody>

</table>

<aside class="note">**Note:** <span>Switching from PWM0 mode to Audio mode requires a device reboot due to limitations of the hardware.</span></aside>

