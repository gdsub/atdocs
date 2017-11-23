# GPIO
# 通用输入输出（GPIO）

[General Purpose Input/Output](https://en.wikipedia.org/wiki/General-purpose_input/output) (GPIO) pins provide a programmable interface to read the state of a binary input device (such as a pushbutton switch) or control the on/off state of a binary output device (such as an LED).

[General Purpose Input/Output](https://en.wikipedia.org/wiki/General-purpose_input/output) (GPIO) 管脚提供了可编程接口用于获取二进制量（如按键开关）的状态或者用于控制二进制输出设备（如发光二级体）的闭/合状态。

You can configure GPIO pins as an input or output with either a high or low state. As an input, an external source determines the state, and your app can read the current value or react to changes in state. As an output, your app configures the state of the pin.

你可以配置GPIO管脚来定义数据传输方向输入或输出高电平或低电平。当管脚做为输入使用时，可以检测到外部信号源的状态变化，你的应用程序可以获得当前状态或者对状态变化做出反应。当管脚做为输出使用时，你的应用程序可以对管脚状态的高低进行配置。

<aside class="note">**Note:** <span>To avoid damage to the GPIO pins, review the input and output limits of your hardware before making wire connections. See [Hardware 101](https://developer.android.google.cn/things/hardware/hardware-101.html) and consult the documentation for your hardware.</span></aside>

<aside class="note">**注意:** <span>为避免造成对GPIO管脚的损坏，请在对你的硬件进行接线时检查有关GPIO输入和输出的限制。查看[硬件 101](https://developer.android.google.cn/things/hardware/hardware-101.html) 并查阅你的硬件的相关文档。</span></aside>

## Managing the connection
## 管理连接
* * *

In order to open a connection to a GPIO port, you need to know the unique port name. During the initial stages of development, or when porting an app to new hardware, it's helpful to discover all the available port names from `PeripheralManagerService` using `getGpioList()`:

当需要对某个GPIO接口进行连接是，你需要获知对应的GPIO端口号。在开发的早期阶段，或者当你在将应用移植到某个新硬件的时候，你可以通过由`PeripheralManagerService`提供的 `getGpioList()`来发现所有可用的GPIO端口号：

    PeripheralManagerService manager = new PeripheralManagerService();List<String> portList = manager.getGpioList();if (portList.isEmpty()) {    Log.i(TAG, "No GPIO port available on this device.");} else {    Log.i(TAG, "List of available ports: " + portList);}

Once you know the target name, use `PeripheralManagerService` to connect to that port. When you're done communicating with the GPIO port, close the connection to free up resources. Additionally, you cannot open a new connection to the same port until the existing connection is closed. To close the connection, use the port's `close()` method.

当你获得了端口名称后，可以使用`PeripheralManagerService`来连接到这个端口。在你完成对这个GPIO端口的通信后，请记得及时关闭连接以便释放相关资源。另外请注意，对同一个GPIO端口只能建立一个连接，在该连接关闭前你无法再对同一个GPIO端口发起新的连接。如需关闭连接，调用端口的`close()`方法。

    public class HomeActivity extends Activity {    // GPIO Pin Name    private static final String GPIO_NAME = ...;    private Gpio mGpio;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Attempt to access the GPIO        try {            PeripheralManagerService manager = new PeripheralManagerService();            mGpio = manager.openGpio(GPIO_NAME);        } catch (IOException e) {             Log.w(TAG, "Unable to access GPIO", e);        }    }    @Override    protected void onDestroy() {        super.onDestroy();        if (mGpio != null) {            try {                mGpio.close();                mGpio = null;            } catch (IOException e) {                Log.w(TAG, "Unable to close GPIO", e);            }        }    }}

## Reading from an input
## 获得输入
* * *

To read a GPIO port as an input:
从某个GPIO端口获得输入：
1.  Configure it as an input using `setDirection()` with the mode `DIRECTION_IN`.

*1.  使用`setDirection()` 方法配置端口的输入方向为`DIRECTION_IN`。

2.  Configure either a high (near IOREF) or low (near zero) voltage signal to be returned as `true` (active), by calling `setActiveType()` with `ACTIVE_HIGH` or `ACTIVE_LOW`.

*2.  通过配置高电平（接近IOREF）或者低电平（接近零）信号返回为`true` (使能)，并调用`setActiveType()`方法来相应的配置为`ACTIVE_HIGH` 或 `ACTIVE_LOW`。

3.  Access the current state with the `getValue()` method.

*3. 通过调用`getValue()`方法来访问到当前状态。

The following code shows you how to set up an input with an active state associated with a high voltage level:

以下代码为设置GPIO端口的方向为输入并配置使能状态为高电平。

    public void configureInput(Gpio gpio) throws IOException {    // Initialize the pin as an input    gpio.setDirection(Gpio.DIRECTION_IN);    // High voltage is considered active    gpio.setActiveType(Gpio.ACTIVE_HIGH);    ...    // Read the active high pin state    if (gpio.getValue()) {        // Pin is HIGH    } else {        // Pin is LOW    }}

### Listening for input state changes
### 获得输入状态的变化
A GPIO port configured as an input can notify your app when its state changes between high and low. To register for these change events:

GPIO端口可以被配置为输入状态时可以在端口状态高低变化时及时通知到你的应用程序。在使用时对如下事件进行注册：

1.  Attach a `GpioCallback` to the active port connection.

*1. 将`GpioCallback`附加到已经打开的端口连接上。

2.  Declare the state changes that trigger an interrupt event using the `setEdgeTriggerType()` method. The edge trigger supports the following four types:

    *   `EDGE_NONE`: No interrupt events. **_This is the default value._**
    *   `EDGE_RISING`: Interrupt on a transition from low to high
    *   `EDGE_FALLING`: Interrupt on a transition from high to low
    *   `EDGE_BOTH`: Interrupt on all state transitions

*2. 使用`setEdgeTriggerType()`方法将状态声明为触发。边沿触发器支持一下四种类型：
 
    *   `EDGE_NONE`: No interrupt events. **_This is the default value._**
    *   `EDGE_NONE`: 无中断事件. **_模式配置._**
    *   `EDGE_RISING`: 当从低电平跳变到高电平的上升沿触发中断
    *   `EDGE_FALLING`: 当从高电平下降为低电平的下降沿触发中断
    *   `EDGE_BOTH`: 只要状态变化就触发中断
3.  Return `true` from within `onGpioEdge()` to indicate that the listener should continue receiving events for each port state change.

*3. 当`onGpioEdge()`返回为`true`时，即代表监听程序需要继续接收相关端口的状态变化事件。
  
    The following code registers an interrupt listener for all state changes on the given input port:
  如下代码为给定的用于输入的端口注册了相应的中断监听：

        public void configureInput(Gpio gpio) throws IOException {    // Initialize the pin as an input    gpio.setDirection(Gpio.DIRECTION_IN);    // Low voltage is considered active    gpio.setActiveType(Gpio.ACTIVE_LOW);    // Register for all state changes    gpio.setEdgeTriggerType(Gpio.EDGE_BOTH);    gpio.registerGpioCallback(mGpioCallback);}private GpioCallback mGpioCallback = new GpioCallback() {    @Override    public boolean onGpioEdge(Gpio gpio) {        // Read the active low pin state        if (gpio.getValue()) {            // Pin is LOW        } else {            // Pin is HIGH        }        // Continue listening for more interrupts        return true;    }    @Override    public void onGpioError(Gpio gpio, int error) {        Log.w(TAG, gpio + ": Error event " + error);    }};

4.  Unregister any interrupt handlers when your app is no longer listening for incoming events:

*4. 当你的应用不再对某一GPIO端口的状态变化进行监听时，请及时注销掉相关的中断处理函数。

        public class HomeActivity extends Activity {    private Gpio mGpio;    ...    @Override    protected void onStart() {        super.onStart();        // Begin listening for interrupt events        mGpio.registerGpioCallback(mGpioCallback);    }    @Override    protected void onStop() {        super.onStop();        // Interrupt events no longer necessary        mGpio.unregisterGpioCallback(mGpioCallback);    }}

## Writing to an output
## 提供输出

* * *

To programmatically control the state of a GPIO port:

通过程序化控制GPIO口的输出状态：

1.  Configure it as an output using `setDirection()` with the mode `DIRECTION_OUT_INITIALLY_HIGH` or `DIRECTION_OUT_INITIALLY_LOW`. These modes ensure that the port's initial state is also set correctly at configuration time.

*1. 通过调用`setDirection()`方法来配置GPIO的工作方向为输出，其中`DIRECTION_OUT_INITIALLY_HIGH`或`DIRECTION_OUT_INITIALLY_LOW`参数分别用来配置端口的起始状态的高或低。

2.  Configure either a high (near IOREF) or low (near zero) voltage signal to be returned as `true` (active), by calling `setActiveType()` with `ACTIVE_HIGH` or `ACTIVE_LOW`.

*2. 通过配置高电平（接近IOREF）或者低电平（接近零）信号返回为`true` (使能)，并调用`setActiveType()`方法来相应的配置为`ACTIVE_HIGH` 或 `ACTIVE_LOW`。
 
3.  Set the current state with the `setValue()` method.

*3. 使用`setValue()`方法来对当前状态进行设置。

The following code shows you how to set up an output to initially be high, then toggle its state to low using the `setValue()` method:

以下代码为配置GPIO端口起始状态为高，然后通过调用`setValue()`方法切换状态为低：

    public void configureOutput(Gpio gpio) throws IOException {    // Initialize the pin as a high output    gpio.setDirection(Gpio.DIRECTION_OUT_INITIALLY_HIGH);    // Low voltage is considered active    gpio.setActiveType(Gpio.ACTIVE_LOW);    ...    // Toggle the value to be LOW    gpio.setValue(true);}

