# SPI


[Serial Peripheral Interface](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus) (SPI) devices are typically found where fast data transfer rates are required. SPI is well suited for high-bandwidth use cases such as external non-volatile memory and graphical displays. Many sensor devices support SPI in addition to [I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html).

SPI is a **_synchronous_** serial interface, which means it relies on a shared clock signal to synchronize data transfer between devices. A master device controls the triggering of the clock signal and all other connected peripherals are known as **_slaves_**. Each device is connected to the same set of data signals to form a **_bus_**.

Theoretically, the data transfer rate for SPI is only limited by how fast the master can toggle the clock signal. Clock speeds are typically in the 16MHz to 25MHz range. This high-speed shared clock allows SPI peripherals to transfer data more quickly and with fewer errors than [UART](https://developer.android.google.cn/things/sdk/pio/uart.html).

![""](https://developer.android.google.cn/things/images/spi-connections.png)

SPI supports **_full-duplex_** data transfer, meaning the master and slave can simultaneously exchange information. To support full-duplex transfer, the bus must provide the following separate signals, which makes SPI a minimum 4-Wire interface:

*   Master Out Slave In (MOSI)
*   Master In Slave Out (MISO)
*   Shared clock signal (CLK)
*   Common ground reference (GND)

SPI supports multiple slave devices connected along the same bus. Unlike [I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html), slave devices are addressed using hardware. An external **_chip select_** signal is required for each slave to allow the master to address that particular device as the data transfer target. This signal is not necessary if only using a single slave.

## Managing the device connection

* * *

In order to open a connection to a particular SPI slave, you need to know the unique name of the bus. During the initial stages of development, or when porting an app to new hardware, it is helpful to discover all the available device names from `PeripheralManagerService` using `getSpiBusList()`:

    PeripheralManagerService manager = new PeripheralManagerService();List<String> deviceList = manager.getSpiBusList();if (deviceList.isEmpty()) {    Log.i(TAG, "No SPI bus available on this device.");} else {    Log.i(TAG, "List of available devices: " + deviceList);}

SPI controllers that support multiple hardware chip selects will report each available slave port as a separate device name. For example, a board that supports _CS0_, _CS1_, and _CS2_ on the same SPI bus will return names similar to `"SPI0.0"`, `"SPI0.1"`, and `"SPI0.2"` from `getSpiBusList()`.

Once you know the target name, use `PeripheralManagerService` to connect to that device. When you are done communicating with the peripheral device, close the connection to free up resources. Additionally, you cannot open a new connection to the device until the existing connection is closed. To close the connection, use the device's `close()` method.

    public class HomeActivity extends Activity {    // SPI Device Name    private static final String SPI_DEVICE_NAME = ...;    private SpiDevice mDevice;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Attempt to access the SPI device        try {            PeripheralManagerService manager = new PeripheralManagerService();            mDevice = manager.openSpiDevice(SPI_DEVICE_NAME);        } catch (IOException e) {             Log.w(TAG, "Unable to access SPI device", e);        }    }    @Override    protected void onDestroy() {        super.onDestroy();        if (mDevice != null) {            try {                mDevice.close();                mDevice = null;            } catch (IOException e) {                Log.w(TAG, "Unable to close SPI device", e);            }        }    }}

## Configuring clock and data modes

* * *

After a connection is established with the SPI bus, configure the data transfer rate and operation modes to match the slave devices on the same bus. For data transfer to be successful, all devices on the bus must expect the same clock and data format behavior.

<aside class="note">**Note:** <span>Some slave devices cannot configure their SPI operation mode, so refer to the documentation for your hardware when choosing peripheral devices.</span></aside>

1.  Set the SPI mode, which defines the polarity and phase of the clock signal. The mode you choose is based on three attributes:

    <div class="figure" style="width:336px" id="fig-spi-clock">![](https://developer.android.google.cn/things/images/spi-clock.png)</div>

    1.  **Idle Level**: Level of the clock signal (low or high) when no data is being transferred.
    2.  **Leading Edge**: Front edge of each clock pulse.
    3.  **Trailing Edge**: Transition opposite the leading edge in each clock pulse.

    The following modes are supported:

    *   `MODE0` - Clock signal idles low, data is transferred on the leading clock edge
    *   `MODE1` - Clock signal idles low, data is transferred on the trailing clock edge
    *   `MODE2` - Clock signal idles high, data is transferred on the leading clock edge
    *   `MODE3` - Clock signal idles high, data is transferred on the trailing clock edge
2.  Set the following `SpiDevice` parameters:

    *   **Frequency** - Specifies the shared clock signal in Hz. Clock signal capabilities will vary across device hardware. You should verify the supported frequencies of your particular device before setting this value.
    *   **Justification** - Specifies the ordering of the individual bits in each byte as they are transferred across the bus. This is also known as the endianness of the data. By default, data will be sent with the most significant bit (MSB) first.
    *   **Bits per Word** - Configures the number of bits transferred at a time in between toggling the chip select line for the given slave. The default value is 8 bits per word.

The following code configures the SPI connection with Mode 0, 16MHz clock, 8 bits per word, and MSB first:

    public void configureSpiDevice(SpiDevice device) throws IOException {    // Low clock, leading edge transfer    device.setMode(SpiDevice.MODE0);    device.setFrequency(16000000);     // 16MHz    device.setBitsPerWord(8);          // 8 BPW    device.setBitJustification(false); // MSB first}

## Transferring data

* * *

SPI supports both half-duplex and full-duplex data transfer. Most apps should use the half-duplex `write()` or `read()` methods to exchange data with a slave device.

    // Half-duplex data transferpublic void sendCommand(SpiDevice device, byte[] buffer) throws IOException {    // Shift data out to slave    device.write(buffer, buffer.length);    // Read the response    byte[] response = new byte[32];    device.read(response, response.length);    ...}

To perform a full-duplex exchange, use the `transfer()` method instead. This method accepts two buffers for read and write. The write buffer contains data to send to the slave, while the read buffer is empty and accepts data from the slave.

The data length must be less than or equal to the smallest buffer's size. It is common with full-duplex transfers for the buffer sizes to be equal.

    // Full-duplex data transferpublic void sendCommand(SpiDevice device, byte[] buffer) throws IOException {    byte[] response = new byte[buffer.length];    device.transfer(buffer, response, buffer.length);    ...}

