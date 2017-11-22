# Raspberry Pi I/O      

<p>This section describes the <a href="https://developer.android.google.cn/things/sdk/pio/index.html">Peripheral I/O</a>
interfaces available to your apps running on the
<a href="https://www.raspberrypi.org/products/raspberry-pi-3-model-b/">Raspberry Pi 3</a>.</p>
<p>这一部分将介绍 <a href="https://www.raspberrypi.org/products/raspberry-pi-3-model-b/">Raspberry Pi 3</a> 上所有可供你的 apps 使用的 <a href="https://developer.android.google.cn/things/sdk/pio/index.html">外围接口</a>。</p>

<p>The Raspberry Pi has pins that are multiplexed between various board functions.
Some board functions cannot be used simultaneously (for example, enabling
Bluetooth and using the <code>UART0</code> port for peripheral I/O). For more information,
see the <a href="https://developer.android.google.cn/things/hardware/raspberrypi-mode-matrix.html">function mode matrix</a>.</p>
<p>Raspberry Pi 上的串口支持多种功能。但有一些功能不能够同时使用（比如打开蓝牙和将串口<code>UART0</code>作为外围接口就不能同时使用）。更多信息可以参照 <a href="https://developer.android.google.cn/things/hardware/raspberrypi-mode-matrix.html">函数模式矩阵</a>。</p>

<p>The following pinout diagram illustrates the locations of the available ports
exposed by the breakout connectors of this board:</p>
<p>下图展示了所有可用串口的位置关系：</p>
<p><img alt="&quot;&quot;" src="https://developer.android.google.cn/things/images/pinout-legend.png"></p>
<p><img alt="&quot;&quot;" src="https://developer.android.google.cn/things/images/pinout-raspberrypi.png" style="float: left;"></p>
<table style="float: right; width: 45%">
  <thead>
    <tr>
      <th>GPIO Signal</th>
      <th colspan="2">Alternate Functions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>BCM2</td><td>I2C1 (SDA)</td><td></td>
    </tr>
    <tr>
      <td>BCM3</td><td>I2C1 (SCL)</td><td></td>
    </tr>
    <tr>
      <td>BCM7</td><td>SPI0 (SS1)</td><td></td>
    </tr>
    <tr>
      <td>BCM8</td><td>SPI0 (SS0)</td><td></td>
    </tr>
    <tr>
      <td>BCM9</td><td>SPI0 (MISO)</td><td></td>
    </tr>
    <tr>
      <td>BCM10</td><td>SPI0 (MOSI)</td><td></td>
    </tr>
    <tr>
      <td>BCM11</td><td>SPI0 (SCLK)</td><td></td>
    </tr>
    <tr>
      <td>BCM13</td><td>PWM1</td><td></td>
    </tr>
    <tr>
      <td>BCM14</td><td>UART0 (TXD)</td><td>MINIUART (TXD)</td>
    </tr>
    <tr>
      <td>BCM15</td><td>UART0 (RXD)</td><td>MINIUART (RXD)</td>
    </tr>
    <tr>
      <td>BCM18</td><td>I2S1 (BCLK)</td><td>PWM0</td>
    </tr>
    <tr>
      <td>BCM19</td><td>I2S1 (LRCLK)</td><td></td>
    </tr>
    <tr>
      <td>BCM20</td><td>I2S1 (SDIN)</td><td></td>
    </tr>
    <tr>
      <td>BCM21</td><td>I2S1 (SDOUT)</td><td></td>
    </tr>
  </tbody>
</table>

<p><br style="clear: both;"></p>

