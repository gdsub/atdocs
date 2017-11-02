# Peripheral I/O

Android Things provides Peripheral I/O APIs to communicate with sensors and actuators using industry standard protocols and interfaces.

**[General Purpose Input/Output](https://developer.android.google.cn/things/sdk/pio/gpio.html) (GPIO)** - Use this API for simple sensors such as motion detectors, proximity detectors, and level switches that report their current state as a binary value—high or low.

**[Pulse Width Modulation](https://developer.android.google.cn/things/sdk/pio/pwm.html) (PWM)** - Use this API for servo motors, DC motors, and lights that require a proportional signal to provide fine-grained control over the output.

**Serial Communication** - Use these APIs to transfer larger payloads of data between two or more smart devices connected on the same local bus. The following table outlines the basic attributes of each supported serial protocol:

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

