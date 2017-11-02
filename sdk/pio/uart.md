# UART


Complex peripheral devices such as GPS modules, LCD displays, and XBee radios typically use [Universal Asynchronous Receiver Transmitter](https://en.wikipedia.org/wiki/Universal_asynchronous_receiver/transmitter) (UART) ports (often simply called **_serial ports_**) to communicate.

The UART is a generic interface for exchanging raw data with a peripheral device. It is **_universal_** because both the data transfer speed and data byte format are configurable. It is **_asynchronous_** in that there are no clock signals present to synchronize the data transfer between the two devices. The device hardware collects all incoming data in a first-in first-out (FIFO) buffer until read by your app.

UART data transfer is **_full-duplex_**, meaning data can be sent and received at the same time. It is typically faster than I<sup>2</sup>C, but the lack of a shared clock means that both devices must agree on a common data transfer rate that each device can adhere to independently with minimal timing error.

![""](https://developer.android.google.cn/things/images/uart-connections.png)

UART peripherals typically come in two flavors:

*   3-Wire ports include data receive (RX), data transmit (TX), and ground reference (GND) signals.
*   5-Wire ports add request to send (RTS) and clear to send (CTS) signals used for **_hardware flow control_**. Flow control allows the receiving device to indicate that its FIFO buffer is temporarily full and the transmitting device should wait before sending any more data.

Unlike [SPI](https://developer.android.google.cn/things/sdk/pio/spi.html) and [I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html), UART only supports point-to-point communication between two devices.

## Managing the connection

* * *

In order to open a connection to a particular UART, you need to know the unique port name. During the initial stages of development, or when porting an app to new hardware, it's helpful to discover all the available device names from `PeripheralManagerService` using `getUartDeviceList()`:

    PeripheralManagerService manager = new PeripheralManagerService();List<String> deviceList = manager.getUartDeviceList();if (deviceList.isEmpty()) {    Log.i(TAG, "No UART port available on this device.");} else {    Log.i(TAG, "List of available devices: " + deviceList);}

Once you know the target name, use `PeripheralManagerService` to connect to that device. When you are done communicating with the peripheral device, close the connection to free up resources. Additionally, you cannot open a new connection to the device until the existing connection is closed. To close the connection, use the device's `close()` method.

    public class HomeActivity extends Activity {    // UART Device Name    private static final String UART_DEVICE_NAME = ...;    private UartDevice mDevice;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Attempt to access the UART device        try {            PeripheralManagerService manager = new PeripheralManagerService();            mDevice = manager.openUartDevice(UART_DEVICE_NAME);        } catch (IOException e) {            Log.w(TAG, "Unable to access UART device", e);        }    }    @Override    protected void onDestroy() {        super.onDestroy();        if (mDevice != null) {            try {                mDevice.close();                mDevice = null;            } catch (IOException e) {                Log.w(TAG, "Unable to close UART device", e);            }        }    }}

## Configuring port parameters

* * *

After a connection is established, configure the data transfer rate and frame format to match the connected peripheral device.

### The Data Frame

Every character sent across the UART is wrapped in a **_data frame_**, which contains the following components:

![""](https://developer.android.google.cn/things/images/uart-frame.png)

*   **Start Bit** - Before sending data, the line is held active for a fixed time interval of 1 bit duration to indicate the start of a new character.
*   **Data Bits** - Individual bits representing the data character. The UART may be configured to send between 5-9 data bits to represent the character. Fewer bits reduces the range of data, but can increase the effective data transfer rate.
*   **Parity Bit** - Optional error checking value. If the UART is configured for **_even_** or **_odd_** parity, an extra bit will be added to the frame to indicate if the contents of the data bits sum to an even or odd value. Setting this to **_none_** removes the bit from the frame.
*   **Stop Bits** - After all data is transmitted, the line is reset for a configurable time interval to indicate the end of that character. This can be configured to remain idle for 1- or 2-bit durations.

<aside class="note">**Note:** <span>The default configuration for most UART devices is 8 data bits, no parity, and 1 stop bit (8N1).</span></aside>

The data transfer rate across the UART is called the **_baud rate_**. It represents the speed, in bits per second, for both receive and transmit. Since there is no shared clock between two devices connected over UART, both must be configured ahead of time to use the same baud rate for the data to be decoded correctly.

Common baud rates include 9600, 19200, 38400, 57600, 115200, and 921600. This rate includes the overhead of the data frame (start, stop, and parity bits), so the effective data transfer rate will be slightly lower and vary based on the number of frame bits you have configured.

The following code configures the UART connection to operate at 115200 baud, 8 data bits, no parity, and 1 stop bit (8N1):

    public void configureUartFrame(UartDevice uart) throws IOException {    // Configure the UART port    uart.setBaudrate(115200);    uart.setDataSize(8);    uart.setParity(UartDevice.PARITY_NONE);    uart.setStopBits(1);}

<aside class="note">**Note:** <span>Choosing uncommon baud rates can lead to high error rates in data transmission. You should always verify whether your chosen baud rate is well supported by your device hardware.</span></aside>

### Hardware Flow Control

If your device supports 5-wire UART ports, enabling hardware flow control can increase the reliability of the data transfer. Often this also means you can safely use faster baud rates with a much lower chance of missing incoming data.

![""](https://developer.android.google.cn/things/images/uart-flow-control.png)

With hardware flow control enabled, the UART asserts the Request to Send (RTS) signal when the receive buffer on the device is full and cannot accept any more data. The signal will be cleared once the buffer has been drained. Similarly, the UART monitors the Clear to Send (CTS) signal, and will pause transmitting data if it sees that line asserted by the peripheral device.

To enable hardware flow control, use the `setHardwareFlowControl()` method with `HW_FLOW_CONTROL_AUTO_RTSCTS`. The default value is `HW_FLOW_CONTROL_NONE`, which indicates that flow control is disabled.

    public void setFlowControlEnabled(UartDevice uart, boolean enable) throws IOException {    if (enable) {        // Enable hardware flow control        uart.setHardwareFlowControl(UartDevice.HW_FLOW_CONTROL_AUTO_RTSCTS);    } else {        // Disable flow control        uart.setHardwareFlowControl(UartDevice.HW_FLOW_CONTROL_NONE);    }}

## Transmitting outgoing data

* * *

To transmit a buffer of data over the UART to a peripheral, use the `write()` method:

    public void writeUartData(UartDevice uart) throws IOException {    byte[] buffer = {...};    int count = uart.write(buffer, buffer.length);    Log.d(TAG, "Wrote " + count + " bytes to peripheral");}

<aside class="note">**Note:** <span>A Java `byte` is an 8-bit value. If you configure a smaller data width using `setDataSize()`, the upper bits of each byte will be truncated.</span></aside>

## Listening for incoming data

* * *

Use the `read()` method to pull incoming data from the UART FIFO buffer into your application. This method accepts an empty buffer to fill with the incoming data and a maximum number of bytes to read. UART reads are non-blocking, and will return immediately if there is no data available in the FIFO.

`UartDevice` will return the number of available bytes in the FIFO at the time of the read, up to the requested count. To ensure that all data was recovered, loop over the UART until it reports that no more data is present:

    public void readUartBuffer(UartDevice uart) throws IOException {    // Maximum amount of data to read at one time    final int maxCount = ...;    byte[] buffer = new byte[maxCount];    int count;    while ((count = uart.read(buffer, buffer.length)) > 0) {        Log.d(TAG, "Read " + count + " bytes from peripheral");    }}

To avoid polling the UART unnecessarily when the buffer is empty, register a `UartDeviceCallback` with the `UartDevice`. This callback invokes the `onUartDeviceDataAvailable()` method when there is data available to read. You should unregister the callback when your application is no longer listening for incoming data.

    public class HomeActivity extends Activity {    private UartDevice mDevice;    ...    @Override    protected void onStart() {        super.onStart();        // Begin listening for interrupt events        mDevice.registerUartDeviceCallback(mUartCallback);    }    @Override    protected void onStop() {        super.onStop();        // Interrupt events no longer necessary        mDevice.unregisterUartDeviceCallback(mUartCallback);    }    private UartDeviceCallback mUartCallback = new UartDeviceCallback() {        @Override        public boolean onUartDeviceDataAvailable(UartDevice uart) {            // Read available data from the UART device            try {                readUartBuffer(uart);            } catch (IOException e) {                Log.w(TAG, "Unable to access UART device", e);            }            // Continue listening for more interrupts            return true;        }        @Override        public void onUartDeviceError(UartDevice uart, int error) {            Log.w(TAG, uart + ": Error event " + error);        }    };}

The `onUartDeviceDataAvailable()` callback returns a boolean value, indicating whether the callback should be automatically unregistered from receiving future interrupt events. Return `true` here to continue receiving events each time data appears in the UART FIFO.

