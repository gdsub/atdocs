# GPIO


[General Purpose Input/Output](https://en.wikipedia.org/wiki/General-purpose_input/output) (GPIO) pins provide a programmable interface to read the state of a binary input device (such as a pushbutton switch) or control the on/off state of a binary output device (such as an LED).

You can configure GPIO pins as an input or output with either a high or low state. As an input, an external source determines the state, and your app can read the current value or react to changes in state. As an output, your app configures the state of the pin.

<aside class="note">**Note:** <span>To avoid damage to the GPIO pins, review the input and output limits of your hardware before making wire connections. See [Hardware 101](https://developer.android.google.cn/things/hardware/hardware-101.html) and consult the documentation for your hardware.</span></aside>

## Managing the connection

* * *

In order to open a connection to a GPIO port, you need to know the unique port name. During the initial stages of development, or when porting an app to new hardware, it's helpful to discover all the available port names from `PeripheralManagerService` using `getGpioList()`:

    PeripheralManagerService manager = new PeripheralManagerService();List<String> portList = manager.getGpioList();if (portList.isEmpty()) {    Log.i(TAG, "No GPIO port available on this device.");} else {    Log.i(TAG, "List of available ports: " + portList);}

Once you know the target name, use `PeripheralManagerService` to connect to that port. When you're done communicating with the GPIO port, close the connection to free up resources. Additionally, you cannot open a new connection to the same port until the existing connection is closed. To close the connection, use the port's `close()` method.

    public class HomeActivity extends Activity {    // GPIO Pin Name    private static final String GPIO_NAME = ...;    private Gpio mGpio;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Attempt to access the GPIO        try {            PeripheralManagerService manager = new PeripheralManagerService();            mGpio = manager.openGpio(GPIO_NAME);        } catch (IOException e) {             Log.w(TAG, "Unable to access GPIO", e);        }    }    @Override    protected void onDestroy() {        super.onDestroy();        if (mGpio != null) {            try {                mGpio.close();                mGpio = null;            } catch (IOException e) {                Log.w(TAG, "Unable to close GPIO", e);            }        }    }}

## Reading from an input

* * *

To read a GPIO port as an input:

1.  Configure it as an input using `setDirection()` with the mode `DIRECTION_IN`.
2.  Configure either a high (near IOREF) or low (near zero) voltage signal to be returned as `true` (active), by calling `setActiveType()` with `ACTIVE_HIGH` or `ACTIVE_LOW`.
3.  Access the current state with the `getValue()` method.

The following code shows you how to set up an input with an active state associated with a high voltage level:

    public void configureInput(Gpio gpio) throws IOException {    // Initialize the pin as an input    gpio.setDirection(Gpio.DIRECTION_IN);    // High voltage is considered active    gpio.setActiveType(Gpio.ACTIVE_HIGH);    ...    // Read the active high pin state    if (gpio.getValue()) {        // Pin is HIGH    } else {        // Pin is LOW    }}

### Listening for input state changes

A GPIO port configured as an input can notify your app when its state changes between high and low. To register for these change events:

1.  Attach a `GpioCallback` to the active port connection.
2.  Declare the state changes that trigger an interrupt event using the `setEdgeTriggerType()` method. The edge trigger supports the following four types:

    *   `EDGE_NONE`: No interrupt events. **_This is the default value._**
    *   `EDGE_RISING`: Interrupt on a transition from low to high
    *   `EDGE_FALLING`: Interrupt on a transition from high to low
    *   `EDGE_BOTH`: Interrupt on all state transitions
3.  Return `true` from within `onGpioEdge()` to indicate that the listener should continue receiving events for each port state change.

    The following code registers an interrupt listener for all state changes on the given input port:

        public void configureInput(Gpio gpio) throws IOException {    // Initialize the pin as an input    gpio.setDirection(Gpio.DIRECTION_IN);    // Low voltage is considered active    gpio.setActiveType(Gpio.ACTIVE_LOW);    // Register for all state changes    gpio.setEdgeTriggerType(Gpio.EDGE_BOTH);    gpio.registerGpioCallback(mGpioCallback);}private GpioCallback mGpioCallback = new GpioCallback() {    @Override    public boolean onGpioEdge(Gpio gpio) {        // Read the active low pin state        if (gpio.getValue()) {            // Pin is LOW        } else {            // Pin is HIGH        }        // Continue listening for more interrupts        return true;    }    @Override    public void onGpioError(Gpio gpio, int error) {        Log.w(TAG, gpio + ": Error event " + error);    }};

4.  Unregister any interrupt handlers when your app is no longer listening for incoming events:

        public class HomeActivity extends Activity {    private Gpio mGpio;    ...    @Override    protected void onStart() {        super.onStart();        // Begin listening for interrupt events        mGpio.registerGpioCallback(mGpioCallback);    }    @Override    protected void onStop() {        super.onStop();        // Interrupt events no longer necessary        mGpio.unregisterGpioCallback(mGpioCallback);    }}

## Writing to an output

* * *

To programmatically control the state of a GPIO port:

1.  Configure it as an output using `setDirection()` with the mode `DIRECTION_OUT_INITIALLY_HIGH` or `DIRECTION_OUT_INITIALLY_LOW`. These modes ensure that the port's initial state is also set correctly at configuration time.
2.  Configure either a high (near IOREF) or low (near zero) voltage signal to be returned as `true` (active), by calling `setActiveType()` with `ACTIVE_HIGH` or `ACTIVE_LOW`.
3.  Set the current state with the `setValue()` method.

The following code shows you how to set up an output to initially be high, then toggle its state to low using the `setValue()` method:

    public void configureOutput(Gpio gpio) throws IOException {    // Initialize the pin as a high output    gpio.setDirection(Gpio.DIRECTION_OUT_INITIALLY_HIGH);    // Low voltage is considered active    gpio.setActiveType(Gpio.ACTIVE_LOW);    ...    // Toggle the value to be LOW    gpio.setValue(true);}

