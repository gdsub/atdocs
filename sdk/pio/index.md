# Peripheral I/O
Android Things provides Peripheral I/O APIs to communicate with sensors and actuators using industry standard protocols and interfaces.

# 外围设备输入输出
Android Things提供了一组外围设备输入输出的API用于和外围设备用于和传感器以及执行器进行通信的工业标准协议与接口。

**[General Purpose Input/Output](https://developer.android.google.cn/things/sdk/pio/gpio.html) (GPIO)** - Use this API for simple sensors such as motion detectors, proximity detectors, and level switches that report their current state as a binary value—high or low.
**[General Purpose Input/Output](https://developer.android.google.cn/things/sdk/pio/gpio.html) (GPIO)** - 这套API用于获取如运动探测器，近距离传感器和信号电平开关-状态在高或低电平间切换等简单传感器的输出与输入。

**[Pulse Width Modulation](https://developer.android.google.cn/things/sdk/pio/pwm.html) (PWM)** - Use this API for servo motors, DC motors, and lights that require a proportional signal to provide fine-grained control over the output.
**[Pulse Width Modulation](https://developer.android.google.cn/things/sdk/pio/pwm.html) (PWM)** - 这套API用于和舵机，直流电机和环境光传感器等需要获得等比例信号输出来进行细粒度控制的传感器进行通信。

**Serial Communication** - Use these APIs to transfer larger payloads of data between two or more smart devices connected on the same local bus. The following table outlines the basic attributes of each supported serial protocol:
**Serial Communication** - 这套API提供在接到同一本地总线上的两台或者多台智能设备之间进行较大数据量的传输。以下的表格概述了已支持的每类串行通讯协议的基本属性：

<table>

<tbody>

<tr>

<th>Protocol</th>

<th>Transfer Type</th>

<th># of Wires</th>

<th># of Peripherals</th>

<th>Transfer Speed</th>

</tr>

<tr>

<td>[I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html)</td>

<td>Synchronous</td>

<td>2</td>

<td>Up to 127</td>

<td>Low</td>

</tr>

<tr>

<td>[SPI](https://developer.android.google.cn/things/sdk/pio/spi.html)</td>

<td>Synchronous</td>

<td>4+</td>

<td>Unlimited</td>

<td>High</td>

</tr>

<tr>

<td>[UART](https://developer.android.google.cn/things/sdk/pio/uart.html)</td>

<td>Asynchronous</td>

<td>2 or 4</td>

<td>1</td>

<td>Medium</td>

</tr>

</tbody>

</table>
<table>

<tbody>

<tr>

<th>协议</th>

<th>传输类型</th>

<th># 所需接线数量</th>

<th># 可连接外围设备数量 </th>

<th>传输速度</th>

</tr>

<tr>

<td>[I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html)</td>

<td>同步传输</td>

<td>2</td>

<td>多至 127</td>

<td>低</td>

</tr>

<tr>

<td>[SPI](https://developer.android.google.cn/things/sdk/pio/spi.html)</td>

<td>同步传输</td>

<td>4+</td>

<td>无限量</td>

<td>高</td>

</tr>

<tr>

<td>[UART](https://developer.android.google.cn/things/sdk/pio/uart.html)</td>

<td>异步</td>

<td>2 或 4</td>

<td>1</td>

<td>中</td>
</tr>

</tbody>

</table>

