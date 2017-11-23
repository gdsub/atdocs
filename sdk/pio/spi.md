# SPI
# 串行外设接口


[Serial Peripheral Interface](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus) (SPI) devices are typically found where fast data transfer rates are required. SPI is well suited for high-bandwidth use cases such as external non-volatile memory and graphical displays. Many sensor devices support SPI in addition to [I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html).

[串行外设接口](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus)（SPI）设备通常被用于需要快速数据传输的工作场合。尤其适合需要高带宽的使用场景，例如外部非易失性存储器（NVM）和图形显示器。许多传感器设备除了支持[I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html)以外也支持SPI。

SPI is a **_synchronous_** serial interface, which means it relies on a shared clock signal to synchronize data transfer between devices. A master device controls the triggering of the clock signal and all other connected peripherals are known as **_slaves_**. Each device is connected to the same set of data signals to form a **_bus_**.

SPI是**_同步_**串行接口，这意味着设备之间依赖于共享的时钟信号来对数据传输进行同步。主设备控制着时钟信号的触发，其他所有连接的外部设备都被认为是**_从设备_**。每个设备都连接到相同的数据信号集而形成**_总线_**。

Theoretically, the data transfer rate for SPI is only limited by how fast the master can toggle the clock signal. Clock speeds are typically in the 16MHz to 25MHz range. This high-speed shared clock allows SPI peripherals to transfer data more quickly and with fewer errors than [UART](https://developer.android.google.cn/things/sdk/pio/uart.html).

SPI总线的理论传输速度仅受到主设备切换时钟信号快慢的限制。通常时钟速度在16MHz到25MHz的范围内。这类高速共享的时钟可以允许SPI外设比[UART](https://developer.android.google.cn/things/sdk/pio/uart.html)设备传输速率更快且错误率更低。

![""](https://developer.android.google.cn/things/images/spi-connections.png)

SPI supports **_full-duplex_** data transfer, meaning the master and slave can simultaneously exchange information. To support full-duplex transfer, the bus must provide the following separate signals, which makes SPI a minimum 4-Wire interface:

SPI支持**_全双工_**数据传输，意味着主从设备之间可以连续不断的进行信息交换。为了能支持全双工传输，总线接口最少必须提供以下四根单独的信号接线：

*   Master Out Slave In (MOSI)
*   Master In Slave Out (MISO)
*   Shared clock signal (CLK)
*   Common ground reference (GND)

* 主出从入（MOSI）
* 主入从出（MISO）
* 共享时钟信号（CLK）
* 共用参考地线（GND）

SPI supports multiple slave devices connected along the same bus. Unlike [I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html), slave devices are addressed using hardware. An external **_chip select_** signal is required for each slave to allow the master to address that particular device as the data transfer target. This signal is not necessary if only using a single slave.

SPI支持同时将多个从设备连接在同一总线上。与[I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html)的连接方式区别在于，从设备的寻址采用硬件方式。每个从设备都需要连接外部**_片选_**信号到主设备用于寻址到该设备进行数据传输。只有在总线上只接一个从设备的时候可以不接片选信号。

## Managing the device connection

* * *

## 管理设备连接

* * *

In order to open a connection to a particular SPI slave, you need to know the unique name of the bus. During the initial stages of development, or when porting an app to new hardware, it is helpful to discover all the available device names from `PeripheralManagerService` using `getSpiBusList()`:

当需要建立访问某从设备的连接时，你需要获知总线的唯一名称。在发开的早期阶段，或者当你在将应用移植到某个新硬件的时候，你可以通过由`PeripheralManagerService`提供的`getSpiBusList()`来发现所有可用的设备名称：

    PeripheralManagerService manager = new PeripheralManagerService();List<String> deviceList = manager.getSpiBusList();if (deviceList.isEmpty()) {    Log.i(TAG, "No SPI bus available on this device.");} else {    Log.i(TAG, "List of available devices: " + deviceList);}

SPI controllers that support multiple hardware chip selects will report each available slave port as a separate device name. For example, a board that supports _CS0_, _CS1_, and _CS2_ on the same SPI bus will return names similar to `"SPI0.0"`, `"SPI0.1"`, and `"SPI0.2"` from `getSpiBusList()`.

支持多个硬件片选的SPI控制器将把每一个从端口当做一个独立的设备名来汇报。例如在一块支持将_CS0_, _CS1_, 和_CS2_连到同一个SPI总线上的板子上，`getSpiBusList()`将返回形如`"SPI0.0"`, `"SPI0.1"`, 和`"SPI0.2"`的设备名称。

Once you know the target name, use `PeripheralManagerService` to connect to that device. When you are done communicating with the peripheral device, close the connection to free up resources. Additionally, you cannot open a new connection to the device until the existing connection is closed. To close the connection, use the device's `close()` method.

当你取得目标设备名称后，使用`PeripheralManagerService`去连接该设备。当你和该外设的通信结束后，请记得及时关闭连接以便释放相关资源。另外请注意，在已有连接的情况下，你无法再建立对该设备的新连接了。如需关闭连接，调用该设备的`close()`方法。

    public class HomeActivity extends Activity {    // SPI Device Name    private static final String SPI_DEVICE_NAME = ...;    private SpiDevice mDevice;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Attempt to access the SPI device        try {            PeripheralManagerService manager = new PeripheralManagerService();            mDevice = manager.openSpiDevice(SPI_DEVICE_NAME);        } catch (IOException e) {             Log.w(TAG, "Unable to access SPI device", e);        }    }    @Override    protected void onDestroy() {        super.onDestroy();        if (mDevice != null) {            try {                mDevice.close();                mDevice = null;            } catch (IOException e) {                Log.w(TAG, "Unable to close SPI device", e);            }        }    }}

## Configuring clock and data modes

* * *

## 配置时钟和数据模式

* * *

After a connection is established with the SPI bus, configure the data transfer rate and operation modes to match the slave devices on the same bus. For data transfer to be successful, all devices on the bus must expect the same clock and data format behavior.

在于SPI总线的连接建立之后，接下来需要配置数据传输速率和操作模式用以匹配连接到该总线上的从设备。为了保证数据能够成功的传输，所有连接到该SPI总线上的设备都应该工作在相同的时钟频率并保持数据格式一致性。

<aside class="note">**Note:** <span>Some slave devices cannot configure their SPI operation mode, so refer to the documentation for your hardware when choosing peripheral devices.</span></aside>

<aside class="note">**注意：** <span>有些从设备无法配置SPI的操作模式，所以当连接外设时请先请查阅你设备硬件的相关文档。</span></aside>

1.  Set the SPI mode, which defines the polarity and phase of the clock signal. The mode you choose is based on three attributes:

*1. 设置SPI模式，定义时钟信号的极性和相位。你可选择的模式基于三种属性：

    <div class="figure" style="width:336px" id="fig-spi-clock">![](https://developer.android.google.cn/things/images/spi-clock.png)</div>

    1.  **Idle Level**: Level of the clock signal (low or high) when no data is being transferred.
    2.  **Leading Edge**: Front edge of each clock pulse.
    3.  **Trailing Edge**: Transition opposite the leading edge in each clock pulse.

    *1. **空闲电平**：当总线空闲时的时钟信号电平（低或高）。
    *2. **采样前沿**：每次时钟脉冲跳变的前沿。
    *3. **采样后沿**：与每次时钟脉冲跳变相反的后沿。

    The following modes are supported:

    支持的模式有以下几种：

    *   `MODE0` - Clock signal idles low, data is transferred on the leading clock edge
    *   `MODE1` - Clock signal idles low, data is transferred on the trailing clock edge
    *   `MODE2` - Clock signal idles high, data is transferred on the leading clock edge
    *   `MODE3` - Clock signal idles high, data is transferred on the trailing clock edge

    * `模式0` - 时钟信号的空闲电平为低电平，数据在采样前沿进行传输
    * `模式1` - 时钟信号的空闲电平为低电平，数据在采样后沿进行传输
    * `模式2` - 时钟信号的空闲电平为高电平，数据在采样前沿进行传输
    * `模式3` - 时钟信号的空闲电平为高电平，数据在采样后沿进行传输    

2.  Set the following `SpiDevice` parameters:

*2. 配置如下的`SpiDevice`参数：

    *   **Frequency** - Specifies the shared clock signal in Hz. Clock signal capabilities will vary across device hardware. You should verify the supported frequencies of your particular device before setting this value.
    *   **Justification** - Specifies the ordering of the individual bits in each byte as they are transferred across the bus. This is also known as the endianness of the data. By default, data will be sent with the most significant bit (MSB) first.
    *   **Bits per Word** - Configures the number of bits transferred at a time in between toggling the chip select line for the given slave. The default value is 8 bits per word.

    * **频率** - 指定共享时钟信号的周期（单位为Hz）。不同设备硬件对于时钟信号的处理能力有所不同，所以你在设置你特定外设的时钟频率时，应该确认硬件上是否支持。

    * **对齐** - 指定在总线上传输的每个字节的每一个位的顺序，这又被称为数据传输的字节序。默认的是按照最高有效位（MSB）优先的字节序进行传输。

    * **每字位数** - 用于配置每次片选信号切换到指定从设备时传输的字节数。默认为8位每字。

The following code configures the SPI connection with Mode 0, 16MHz clock, 8 bits per word, and MSB first:

以下的代码配置了SPI连接工作在模式0下，设备时钟周期16MHz，8个比特每个字，最高有效位（MSB）优先：

    public void configureSpiDevice(SpiDevice device) throws IOException {    // Low clock, leading edge transfer    device.setMode(SpiDevice.MODE0);    device.setFrequency(16000000);     // 16MHz    device.setBitsPerWord(8);          // 8 BPW    device.setBitJustification(false); // MSB first}

## Transferring data

* * *

## 传输数据

* * *

SPI supports both half-duplex and full-duplex data transfer. Most apps should use the half-duplex `write()` or `read()` methods to exchange data with a slave device.

SPI支持半双工和全双工数据传输。大部分应用程序应该使用半双工模式下的`write()`或`read()`方法来与从设备进行数据交换。

    // Half-duplex data transferpublic void sendCommand(SpiDevice device, byte[] buffer) throws IOException {    // Shift data out to slave    device.write(buffer, buffer.length);    // Read the response    byte[] response = new byte[32];    device.read(response, response.length);    ...}

To perform a full-duplex exchange, use the `transfer()` method instead. This method accepts two buffers for read and write. The write buffer contains data to send to the slave, while the read buffer is empty and accepts data from the slave.

要进行全双工数据交换，应改用`transfer()`方法。这个方法能接受读和写的两个缓冲。写缓冲包含需要发给从设备的数据，同时为空的读缓冲用于接受来自于从设备发来的数据。

The data length must be less than or equal to the smallest buffer's size. It is common with full-duplex transfers for the buffer sizes to be equal.

数据长度必须要小于等于最小缓冲的大小。在全双工传输中经常将两端的缓冲设置成一样的大小。

    // Full-duplex data transferpublic void sendCommand(SpiDevice device, byte[] buffer) throws IOException {    byte[] response = new byte[buffer.length];    device.transfer(buffer, response, buffer.length);    ...}

