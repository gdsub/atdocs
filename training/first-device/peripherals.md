# Interact with Peripherals

# 与外设进行交互

## 这一部分内容将告诉你

1.  List Available Peripherals

1. 列出可用外设

2.  Communicate with GPIO Ports

2. 与 GPIO 接口进行通信

## 试试这个

*   [Simple PIO sample app](https://github.com/androidthings/sample-simplepio)


The power of Android Things is realized when developers begin connecting hardware peripherals directly to apps.

当 Android 开发者开始将硬件外设直接连接到应用程序时，Android Things 的功能就实现了。

In this lesson, you will learn how to use the basic Peripheral I/O APIs to discover and communicate with General Purpose Input Ouput (GPIO) ports.

在这节课中，你将学习怎样利用基础的外设 I/O API 去发现通用输入输出接口（GPIO）设备，并与其通信。

## List available peripherals

## 列出可用外设

* * *

The system service responsible for managing peripheral connections is `PeripheralManagerService`. You can use this service to list the available ports for all known peripheral types.

负责管理外设链接的系统服务是 `PeripheralManagerService`。你可以利用这个服务来列出目前处于可用状态的已知类型的外设。

The following code writes the list of available GPIO ports to `logcat`:

下面的代码将可用的 GPIO 接口写入到日志中：

~~~java
public class HomeActivity extends Activity {
    private static final String TAG = "HomeActivity";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        PeripheralManagerService service = new PeripheralManagerService();
        Log.d(TAG, "Available GPIO: " + service.getGpioList());
    }
}

~~~

## Handle button events

## 处理按钮事件

* * *

To receive events when a button connected to GPIO is pressed:

为了接收到一个已经连接 GPIO 接口被按下时所触发的事件，你需要做以下工作：

1.  Use `PeripheralManagerService` to open a connection with the GPIO port wired to the button.

1. 利用 `PeripheralManagerService` 开启一个 GPIO 端口与按键的连接。

2.  Configure the port with `DIRECTION_IN`.

2. 配置端口为 `DIRECTION_IN` 模式。

3.  Configure which state transitions will generate callback events with `setEdgeTriggerType()`.

3. 利用 `setEdgeTriggerType()` 来配置哪些状态转换将触发回调事件。

4.  Register a `GpioCallback` to receive edge trigger events.

4. 利用 `GpioCallback` 注册一个函数来接收边缘触发事件。

5.  Return `true` within `onGpioEdge()` to continue receiving future edge trigger events.

5. 在 `onGpioEdge()` 函数中返回 `true` 来确保能继续接收到后续的边缘触发事件。

6.  When the application no longer needs the GPIO connection, close the `Gpio` resource.

6. 当应用程序不再需要 GPIO 连接时，关闭 `GPIO` 资源。

~~~java
public class ButtonActivity extends Activity {
    private static final String TAG = "ButtonActivity";
    private static final String BUTTON_PIN_NAME = ...; // GPIO port wired to the button

    private Gpio mButtonGpio;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        PeripheralManagerService service = new PeripheralManagerService();
        try {
            // Step 1. Create GPIO connection.
            mButtonGpio = service.openGpio(BUTTON_PIN_NAME);
            // Step 2. Configure as an input.
            mButtonGpio.setDirection(Gpio.DIRECTION_IN);
            // Step 3. Enable edge trigger events.
            mButtonGpio.setEdgeTriggerType(Gpio.EDGE_FALLING);
            // Step 4. Register an event callback.
            mButtonGpio.registerGpioCallback(mCallback);
        } catch (IOException e) {
            Log.e(TAG, "Error on PeripheralIO API", e);
        }
    }

    // Step 4. Register an event callback.
    private GpioCallback mCallback = new GpioCallback() {
        @Override
        public boolean onGpioEdge(Gpio gpio) {
            Log.i(TAG, "GPIO changed, button pressed");

            // Step 5. Return true to keep callback active.
            return true;
        }
    };

    @Override
    protected void onDestroy() {
        super.onDestroy();

        // Step 6. Close the resource
        if (mButtonGpio != null) {
            mButtonGpio.unregisterGpioCallback(mCallback);
            try {
                mButtonGpio.close();
            } catch (IOException e) {
                Log.e(TAG, "Error on PeripheralIO API", e);
            }
        }
    }
}

~~~

## 点亮 LED

* * *

To execute a blinking pattern on an LED connected to GPIO:

为了让一个已经连接到 GPIO 接口的 LED 进入闪烁模式，你需要：

1.  Use `PeripheralManagerService` to open a connection with the GPIO port wired to the LED.

1. 利用 `PeripheralManagerService` 开启 GPIO 端口与 LED 的连接。

2.  Configure the port with `DIRECTION_OUT_INITIALLY_LOW`.

2. 配置端口为 `DIRECTION_OUT_INITIALLY_LOW` 模式。

3.  Toggle the state of the LED by passing the inverse of `getValue()` to the `setValue()` method.

3. 通过 `getValue()` 对应的 `setValue()` 函数来切换 LED 的状态。

4.  Use a `Handler` to schedule an event to toggle the GPIO again after a short delay.

4. 利用一个 `Handler` 来设定一个在短暂的延迟之后再次切换 GPIO 状态的事件。

5.  When the application no longer needs the GPIO connection, close the `Gpio` resource.

5. 当应用程序不再需要 GPIO 连接时，关闭 `GPIO` 资源。


~~~java
public class BlinkActivity extends Activity {
    private static final String TAG = "BlinkActivity";
    private static final int INTERVAL_BETWEEN_BLINKS_MS = 1000;
    private static final String LED_PIN_NAME = ...; // GPIO port wired to the LED

    private Handler mHandler = new Handler();

    private Gpio mLedGpio;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Step 1. Create GPIO connection.
        PeripheralManagerService service = new PeripheralManagerService();
        try {
            mLedGpio = service.openGpio(LED_PIN_NAME);
            // Step 2. Configure as an output.
            mLedGpio.setDirection(Gpio.DIRECTION_OUT_INITIALLY_LOW);

            // Step 4. Repeat using a handler.
            mHandler.post(mBlinkRunnable);
        } catch (IOException e) {
            Log.e(TAG, "Error on PeripheralIO API", e);
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();

        // Step 4. Remove handler events on close.
        mHandler.removeCallbacks(mBlinkRunnable);

        // Step 5. Close the resource.
        if (mLedGpio != null) {
            try {
                mLedGpio.close();
            } catch (IOException e) {
                Log.e(TAG, "Error on PeripheralIO API", e);
            }
        }
    }

    private Runnable mBlinkRunnable = new Runnable() {
        @Override
        public void run() {
            // Exit if the GPIO is already closed
            if (mLedGpio == null) {
                return;
            }

            try {
                // Step 3. Toggle the LED state
                mLedGpio.setValue(!mLedGpio.getValue());

                // Step 4. Schedule another event after delay.
                mHandler.postDelayed(mBlinkRunnable, INTERVAL_BETWEEN_BLINKS_MS);
            } catch (IOException e) {
                Log.e(TAG, "Error on PeripheralIO API", e);
            }
        }
    };
}
~~~
