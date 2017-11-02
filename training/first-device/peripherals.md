# Interact with Peripherals
## This lesson teaches you to

1.  List Available Peripherals
2.  Communicate with GPIO Ports

## Try it out

*   [Simple PIO sample app](https://github.com/androidthings/sample-simplepio)


The power of Android Things is realized when developers begin connecting hardware peripherals directly to apps.

In this lesson, you will learn how to use the basic Peripheral I/O APIs to discover and communicate with General Purpose Input Ouput (GPIO) ports.

## List available peripherals

* * *

The system service responsible for managing peripheral connections is `PeripheralManagerService`. You can use this service to list the available ports for all known peripheral types.

The following code writes the list of available GPIO ports to `logcat`:

    public class HomeActivity extends Activity {    private static final String TAG = "HomeActivity";    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        PeripheralManagerService service = new PeripheralManagerService();        Log.d(TAG, "Available GPIO: " + service.getGpioList());    }}

## Handle button events

* * *

To receive events when a button connected to GPIO is pressed:

1.  Use `PeripheralManagerService` to open a connection with the GPIO port wired to the button.
2.  Configure the port with `DIRECTION_IN`.
3.  Configure which state transitions will generate callback events with `setEdgeTriggerType()`.
4.  Register a `GpioCallback` to receive edge trigger events.
5.  Return `true` within `onGpioEdge()` to continue receiving future edge trigger events.
6.  When the application no longer needs the GPIO connection, close the `Gpio` resource.

```
    public class ButtonActivity extends Activity {    private static final String TAG = "ButtonActivity";    private static final String BUTTON_PIN_NAME = ...; // GPIO port wired to the button    private Gpio mButtonGpio;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        PeripheralManagerService service = new PeripheralManagerService();        try {            // Step 1\. Create GPIO connection.            mButtonGpio = service.openGpio(BUTTON_PIN_NAME);            // Step 2\. Configure as an input.            mButtonGpio.setDirection(Gpio.DIRECTION_IN);            // Step 3\. Enable edge trigger events.            mButtonGpio.setEdgeTriggerType(Gpio.EDGE_FALLING);            // Step 4\. Register an event callback.            mButtonGpio.registerGpioCallback(mCallback);        } catch (IOException e) {            Log.e(TAG, "Error on PeripheralIO API", e);        }    }    // Step 4\. Register an event callback.    private GpioCallback mCallback = new GpioCallback() {        @Override        public boolean onGpioEdge(Gpio gpio) {            Log.i(TAG, "GPIO changed, button pressed");            // Step 5\. Return true to keep callback active.            return true;        }    };    @Override    protected void onDestroy() {        super.onDestroy();        // Step 6\. Close the resource        if (mButtonGpio != null) {            mButtonGpio.unregisterGpioCallback(mCallback);            try {                mButtonGpio.close();            } catch (IOException e) {                Log.e(TAG, "Error on PeripheralIO API", e);            }        }    }}
```

## Blink an LED

* * *

To execute a blinking pattern on an LED connected to GPIO:

1.  Use `PeripheralManagerService` to open a connection with the GPIO port wired to the LED.
2.  Configure the port with `DIRECTION_OUT_INITIALLY_LOW`.
3.  Toggle the state of the LED by passing the inverse of `getValue()` to the `setValue()` method.
4.  Use a `Handler` to schedule an event to toggle the GPIO again after a short delay.
5.  When the application no longer needs the GPIO connection, close the `Gpio` resource.

```
    public class BlinkActivity extends Activity {    private static final String TAG = "BlinkActivity";    private static final int INTERVAL_BETWEEN_BLINKS_MS = 1000;    private static final String LED_PIN_NAME = ...; // GPIO port wired to the LED    private Handler mHandler = new Handler();    private Gpio mLedGpio;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Step 1\. Create GPIO connection.        PeripheralManagerService service = new PeripheralManagerService();        try {            mLedGpio = service.openGpio(LED_PIN_NAME);            // Step 2\. Configure as an output.            mLedGpio.setDirection(Gpio.DIRECTION_OUT_INITIALLY_LOW);            // Step 4\. Repeat using a handler.            mHandler.post(mBlinkRunnable);        } catch (IOException e) {            Log.e(TAG, "Error on PeripheralIO API", e);        }    }    @Override    protected void onDestroy() {        super.onDestroy();        // Step 4\. Remove handler events on close.        mHandler.removeCallbacks(mBlinkRunnable);        // Step 5\. Close the resource.        if (mLedGpio != null) {            try {                mLedGpio.close();            } catch (IOException e) {                Log.e(TAG, "Error on PeripheralIO API", e);            }        }    }    private Runnable mBlinkRunnable = new Runnable() {        @Override        public void run() {            // Exit if the GPIO is already closed            if (mLedGpio == null) {                return;            }            try {                // Step 3\. Toggle the LED state                mLedGpio.setValue(!mLedGpio.getValue());                // Step 4\. Schedule another event after delay.                mHandler.postDelayed(mBlinkRunnable, INTERVAL_BETWEEN_BLINKS_MS);            } catch (IOException e) {                Log.e(TAG, "Error on PeripheralIO API", e);            }        }    };}
```
