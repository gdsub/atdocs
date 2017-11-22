# Raspberry Pi 3 Function Mode Matrix
# Raspberry Pi 3 函数模式矩阵

The following modes in each table are mutually exclusive on the Raspberry Pi 3.

以下每个表格中的模式在 Raspberry Pi 上都是互斥的

## UART modes
## UART 模式

* * *

The Raspberry Pi has a single full-speed UART (**UART0**) and a mini UART (**MINIUART**); see the [official docs](https://www.raspberrypi.org/documentation/configuration/uart.md) for information on their differences. These UARTs are multiplexed between various board functions and cannot be used simultaneously. The following modes are supported:

Raspberry Pi 有一个全速单　UART (UART0) 和一个迷你 UART (MINIUART)；更多关于它们不同的细节可以参照 [官方文件](https://www.raspberrypi.org/documentation/configuration/uart.md)。这些 UART 模式支持多路板载功能，但多个 UART 之间不能同时使用。以下是所有支持的模式：
<table>

<thead>

<tr>

<th>Mode

模式</th>

<th>Activated By

开启方式</th>

<th>Bluetooth

蓝牙</th>

<th>Pin Functions

串口功能</th>

</tr>

</thead>

<tbody>

<tr>

<td>Debug console

调试控制台</td>

<td>Default mode; no PIO connections

默认模式； 无 PIO 连接</td>

<td>Enabled

打开</td>

<td>Pins BCM14/BCM15 expose RX/TX of the [serial debug console](https://developer.android.google.cn/things/hardware/raspberrypi.html#serial_debug_console)</td>

</tr>

<tr>

<td>UART0</td>

<td>UART0 opened by PIO

由 PIO 打开的 UART0</td>

<td>Disabled

关闭</td>

<td>Pins BCM14/BCM15 expose RX/TX of [UART0](https://developer.android.google.cn/things/sdk/pio/uart.html)</td>

</tr>

<tr>

<td>MINIUART</td>

<td>MINIUART opened by PIO

由 PIO 打开的 MINIUART</td>

<td>Enabled

打开</td>

<td>Pins BCM14/BCM15 expose RX/TX of [MINIUART](https://developer.android.google.cn/things/sdk/pio/uart.html)</td>

</tr>

<tr>

<td>BCM14 or BCM15

BCM14 或 BCM15</td>

<td>Pin opened by PIO

由 PIO 打开的 串口</td>

<td>Enabled

打开</td>

<td>Named pin (BCM14 or BCM15) is [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html), other pin is idle</td>

</tr>

</tbody>

</table>

An `IOException` error is thrown if you try to open an active pin (from above) using a different UART mode.

当你尝试用一个不同的 UART 模式打开以上的串口时会出现 `IOException` 报错

## Audio modes
## 音频模式

* * *

The Raspberry Pi uses a shared clock signal for the PWM drivers and the audio subsystem (I2S and analog). Analog audio is transmitted through the 3.5mm audio jack. The following modes are supported:

Raspberry Pi 上的 PWM 驱动和音频系统 (I2S 和模拟)使用一个共享的时钟信号。模拟音频信号通过3.5毫米的音频接口输出。以下是所有支持的音频模式：

<table>

<thead>

<tr>

<th>Mode

模式</th>

<th>Activated By

开启方式</th>

<th>Analog Audio

模拟音频</th>

<th>Pin Functions

串口功能</th>

</tr>

</thead>

<tbody>

<tr>

<td>Audio

音频</td>

<td>Default mode; no PIO connections

默认模式；无 PIO 连接</td>

<td>Enabled

打开</td>

<td>N/A</td>

</tr>

<tr>

<td>PWM0</td>

<td>PWM0 opened by PIO

由　PIO 打开的 PWM0</td>

<td>Disabled

关闭</td>

<td>Pin BCM18 enabled as [PWM](https://developer.android.google.cn/things/sdk/pio/pwm.html)

BCM18 口用为 [PWM](https://developer.android.google.cn/things/sdk/pio/pwm.html)</td>

</tr>

<tr>

<td>I2S1</td>

<td>I2S1 opened by PIO

由 PIO 打开的 I2S1</td>

<td>Enabled

打开</td>

<td>Pin BCM18 enabled as [I2S](https://developer.android.google.cn/things/sdk/pio/i2s.html) BCLK

BCM18 口用为 [I2S](https://developer.android.google.cn/things/sdk/pio/i2s.html) BCLK</td>

</tr>

<tr>

<td>BCM18</td>

<td>Pin opened by PIO

由 PIO 打开的串口</td>

<td>Enabled

打开</td>

<td>Named pin (BCM18) is [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html)

命名串口 (BCM18) 是 [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html)</td>

</tr>

</tbody>

</table>

<aside class="note">**Note:** <span>Switching from PWM0 mode to Audio mode requires a device reboot due to limitations of the hardware.</span></aside>

<aside class="note">**注意:** <span>由于硬件局限，从 PWM0 模式切换到音频模式需要设备重启。</span></aside>

