# I2C
# 内部集成电路

The [Inter-Integrated Circuit](https://en.wikipedia.org/wiki/I%C2%B2C) (IIC or I<sup>2</sup>C) bus connects simple peripheral devices with small data payloads. Sensors and actuators are common use cases for I<sup>2</sup>C. Examples include accelerometers, thermometers, LCD displays, and motor drivers.

内部集成电路[Inter-Integrated Circuit](https://en.wikipedia.org/wiki/I%C2%B2C) (IIC 或 I<sup>2</sup>C) 总线用于连接数据传输量不大的简单的外设。这类传感器和动作器包括加速度传感器，温度传感器，液晶显示器和电机驱动器。

I<sup>2</sup>C is a **_synchronous_** serial interface, which means it relies on a shared clock signal to synchronize data transfer between devices. The device in control of triggering the clock signal is known as the **_master_**. All other connected peripherals are known as **_slaves_**. Each device is connected to the same set of data signals to form a **_bus_**.

I<sup>2</sup>C 是一种 **同步** 串行接口，数据传输的时候需要依赖设备间共享的时钟信号来做同步。 控制触发时钟信号源的设备为**_主设备_**。所以其他接连的外部设备为**_从设备_**。由每个设备到的同一组数据信号构成了**_总线_**。


I<sup>2</sup>C devices connect using a 3-Wire interface consisting of:

*   Shared clock signal (SCL)
*   Shared data line (SDA)
*   Common ground reference (GND)

I<sup>2</sup>C 设备使用的三个接线的接口定义如下:

*   共享时钟信号 (SCL)
*   共享数据线 (SDA)
*   共用参考地线 (GND)

![alt_text](https://developer.android.google.cn/things/images/i2c-connections.png "image_tooltip")

Because all data is transferred over one wire, I<sup>2</sup>C only supports **_half-duplex_** communication. All communication is initiated by the master device, and the slave must respond once the master's transmission is complete.

由于所有的数据都通过同一根线来传输，I<sup>2</sup>C只支持**_半双工_**的通信模式。所有的通信都得由主设备发起，然后从设备只能在主设备传输结束后才能做响应。

I<sup>2</sup>C supports multiple slave devices connected along the same bus. Unlike [SPI](https://developer.android.google.cn/things/sdk/pio/spi.html), slave devices are addressed using the I<sup>2</sup>C software protocol. Each device is programmed with a unique address and only responds to transmissions the master sends to that address. Every slave device must have an address, even if the bus contains only a single slave.

I<sup>2</sup>C 支持多个从设备连接到同一个总线上。与使用SPI的外设不同，从设备的寻址模式使用I<sup>2</sup>C的软件协议。每个设备都可以被程序写为唯一的访问地址并只对主设备发向这个地址的传输做出响应。每个从设备都必须配置地址，即使总线上只连接了一个从设备，也需要这么做。

## Managing the slave device connection
* * *

## 管理从设备连接
* * *

In order to open a connection to a particular I<sup>2</sup>C slave, you need to know the unique name of the bus. During the initial stages of development, or when porting an app to new hardware, it's helpful to discover all the available device names from `PeripheralManagerService` using `getI2cBusList()`:

当需要建立访问到某从设备的连接时，你需要获知总线名称。在发开的早期阶段，或者当你在将应用移植到某个新硬件的时候，你可以通过由PeripheralManagerService`提供的`getI2cBusList()`来发现所有可用的设备名称。

    PeripheralManagerService manager = new PeripheralManagerService();List<String> deviceList = manager.getI2cBusList();if (deviceList.isEmpty()) {    Log.i(TAG, "No I2C bus available on this device.");} else {    Log.i(TAG, "List of available devices: " + deviceList);}

Once you know the target device name, use `PeripheralManagerService` to connect to that device. When you are done communicating with the peripheral device, close the connection to free up resources. Additionally, you cannot open a new connection to the device until the existing connection is closed. To close the connection, use the device's `close()` method.

当你取得目标设备名称后，使用`PeripheralManagerService`去连接该设备。当你和该外设的通信结束后，请记得及时关闭连接以便释放相关资源。另外请注意，在已有连接的情况下，你无法再建立对该设备的新连接了。如需关闭连接，调用该设备的`close()`方法。

    public class HomeActivity extends Activity {    // I2C Device Name    private static final String I2C_DEVICE_NAME = ...;    // I2C Slave Address    private static final int I2C_ADDRESS = ...;    private I2cDevice mDevice;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Attempt to access the I2C device        try {            PeripheralManagerService manager = new PeripheralManagerService();            mDevice = manager.openI2cDevice(I2C_DEVICE_NAME, I2C_ADDRESS);        } catch (IOException e) {            Log.w(TAG, "Unable to access I2C device", e);        }    }    @Override    protected void onDestroy() {        super.onDestroy();        if (mDevice != null) {            try {                mDevice.close();                mDevice = null;            } catch (IOException e) {                Log.w(TAG, "Unable to close I2C device", e);            }        }    }}

<aside class="note">**Note:** <span>The device name represents the I<sup>2</sup>C bus, and the address represents the individual slave on that bus. Therefore, an `I2cDevice` is a connection to a specific slave device on the corresponding I<sup>2</sup>C bus.</span></aside>

<aside class="note">**注意:** <span>设备名称代表I<sup>2</sup>C总线，访问地址则代表了总线上的每个从设备. 因此，一个`I2c设备`是指一个连接到对应的I<sup>2</sup>C总线上的特定从设备。</span></aside>

## Interacting with registers

* * *

## 与寄存器交互

* * *

I<sup>2</sup>C slave devices organize their contents into either readable or writable registers (individual bytes of data referenced by an address value):

I<sup>2</sup>C从设备的内容组成由可读或可写寄存器（由某地址值引用的数据中的单个字节）组成：

*   Readable registers - Contains data the slave wants to report to the master, such as sensor values or status flags.
*   Writable registers - Contains configuration data that the master can control.

* 可读寄存器 - 包含从设备想要发往主设备的数据，例如传感器读取值或状态标记。
* 可写寄存器 - 包含主设备可控制的配置数据。

A common protocol implementation known as [System Management Bus](https://en.wikipedia.org/wiki/System_Management_Bus) (SMBus) exists on top of I<sup>2</sup>C to interact with register data in a standard way. SMBus commands consist of two I<sup>2</sup>C transactions as follows:

系统管理总线[System Management Bus](https://en.wikipedia.org/wiki/System_Management_Bus)（SMBus）被用来作为与I<sup>2</sup>C总线寄存器数据交互的通用协议。SMBus命令由两个I<sup>2</sup>C事务操作来组成：

![""](https://developer.android.google.cn/things/images/i2c-smbus.png)

The first transaction identifies the register address to access, and the second reads or writes the data at that address. Logical data on a slave device may often take up multiple bytes, and thus encompass multiple register addresses. The register address provided to the API is always the first register to reference.

第一个事务操作标明了需要访问的寄存器地址，第二个事务操作用来对该地址进行数据的读或者写操作。从设备上的逻辑数据一般会占用多个字节，并涵盖了多个寄存器地址。提供给API调用的寄存器地址总是引用的第一个寄存器。

<aside class="note">**Note:** <span>Per SMBus protocol, the device will send a "repeated start" condition between the address and data transactions.</span></aside>

<aside class="注意">**Note:** <span>按照SMBus协议，设备将在地址和数据事务操作之间发送“重复开始”条件。</span></aside>

Peripheral I/O provides three types of SMBus commands for accessing register data:

外部设备输入输出提供了三类SMBus命令用于访问寄存器数据：

*   **Byte Data** - `readRegByte()` and `writeRegByte()` Read or write a single 8-bit register value.

* **字节数据** - `readRegByte()` and `writeRegByte()` 用于读取或写入单个8位寄存器值。

*   **Word Data** - `readRegWord()` and `writeRegWord()` Read or write two consecutive register values as a 16-bit little-endian word. The first register address corresponds to the least significant byte (LSB) in the word, followed by the most significant byte (MSB).

* **字数据** - `readRegWord()` and `writeRegWord()` 用于读取或者写入16位按小端排列的字到两个连续寄存器。第一个寄存器对应为该字中的最低有效字节（LSB），然后依次对应最高有效字节（MSB）。

*   **Block Data** - `readRegBuffer()` and `writeRegBuffer()` Read or write up to 32 consecutive register values as an array.

* **块数据** - `readRegBuffer()` and `writeRegBuffer()` 用于读取或者写入多至32个连续寄存器的数组操作。

    // Modify the contents of a single registerpublic void setRegisterFlag(I2cDevice device, int address) throws IOException {    // Read one register from slave    byte value = device.readRegByte(address);    // Set bit 6    value |= 0x40;    // Write the updated value back to slave    device.writeRegByte(address, value);}// Read a register blockpublic byte[] readCalibration(I2cDevice device, int startAddress) throws IOException {    // Read three consecutive register values    byte[] data = new byte[3];    device.readRegBuffer(startAddress, data, data.length);    return data;}

## Transferring raw data

* * *

## 传输原始数据

When interacting with an I<sup>2</sup>C peripheral that defines its registers differently than SMBus -- or perhaps doesn't use registers at all -- use the raw `read()` and `write()` methods for full control over the data bytes transmitted across the wire. These methods will execute a single I<sup>2</sup>C transaction as follows:

当与I<sup>2</sup>C寄存器定义和SMBus不同的外设进行交互式，或者这类外设不使用任何寄存器，使用原 `read()` and `write()` 方法可以获得对连接线上传输的所有数据的控制。这些方法将按照以下的方式执行单个I<sup>2</sup>C事务性操作：

![""](https://developer.android.google.cn/things/images/i2c-raw.png)

With raw transfers, the device will send a single start condition before the transfer and a single stop condition after. It is not possible to combine multiple transactions with a "repeated start" condition.

进行原始数据传输时，设备将发起事务传输之前发送单个开始条件并在传输结束后发送停止条件。多个事务传输无法同时使用”重复开始“条件进行共同传输。

<aside class="note">**Note:** <span>There is no explicit maximum length that a raw transaction can handle, but the I<sup>2</sup>C controller hardware on your device may have a limit on the number of bytes it can process. Consult your device hardware documentation if your peripheral requires large data transfers.</span></aside>

<aside class="note">**注意:** <span> 对于原始数据传输的最大长度没有明确的限定, 但你设备上的I<sup>2</sup>C控制器硬件本身可能有对于处理数据的字节数有限制。如果外设需要进行大量数据传输，请查阅你的设备硬件相关文档。</span></aside>

The following code sample show you how to construct a raw byte buffer and write it to an I<sup>2</sup>C slave:

以下代码实例如何构造原始数据字节缓冲并写入I<sup>2</sup>C从设备：

    public void writeBuffer(I2cDevice device, byte[] buffer) throws IOException {    int count = device.write(buffer, buffer.length);    Log.d(TAG, "Wrote " + count + " bytes over I2C.");}

