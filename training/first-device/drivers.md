# Integrate Peripheral Drivers

## This lesson teaches you to

1.  Integrate Driver Libraries
2.  Register Framework Drivers

## Try it out

*   [Button sample app](https://github.com/androidthings/sample-button)


Get up and running quickly with pre-built drivers from the [Peripheral Driver Library](https://developer.android.google.cn/things/sdk/driver-library.html). These drivers abstract the low-level communication details associated with many common hardware peripherals.

In this lesson, you will learn to integrate library drivers into your app and bind peripherals to the Android framework through the `UserDriverManager`.

## Initialize the driver library

* * *

To import a library driver into your app:

1.  Search the [driver index](https://developer.android.google.cn/things/sdk/driver-library.html) for a driver that matches your peripheral.
2.  Add the driver artifact dependency to your app-level `build.gradle` file:

        dependencies {    ...    compile 'com.google.android.things.contrib:driver-button:0.3'}

3.  Add the required permissions for the driver to your app's manifest file:

        <uses-permission android:name="com.google.android.things.permission.MANAGE_INPUT_DRIVERS" />

    Different drivers may require different permissions. If you aren't sure which permissions are required for a specific driver, locate the driver's README file. See [User-Space Drivers](https://developer.android.google.cn/things/sdk/drivers/index.html) for more information.

4.  Initialize the driver class with the appropriate Peripheral I/O resources.

5.  When your app no longer needs the resource, close the connection.

```
        import com.google.android.things.contrib.driver.button.Button;import com.google.android.things.contrib.driver.button.ButtonInputDriver;...public class ButtonActivity extends Activity {    private static final String TAG = "ButtonActivity";    private static final String BUTTON_PIN_NAME = ...; // GPIO port wired to the button    private ButtonInputDriver mButtonInputDriver;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        try {            // Step 3\. Initialize button driver with selected GPIO pin            mButtonInputDriver = new ButtonInputDriver(                    BUTTON_PIN_NAME,                    Button.LogicState.PRESSED_WHEN_LOW,                    KeyEvent.KEYCODE_SPACE);        } catch (IOException e) {            Log.e(TAG, "Error configuring GPIO pin", e);        }    }    @Override    protected void onDestroy(){        super.onDestroy();        // Step 5\. Close the driver and unregister        if (mButtonInputDriver != null) {            try {                mButtonInputDriver.close();            } catch (IOException e) {                Log.e(TAG, "Error closing Button driver", e);            }        }    }}
```

## Bind to the framework

* * *

The driver library includes [user drivers](https://developer.android.google.cn/things/sdk/drivers/index.html) for supported peripheral types. When these drivers are available, they allow your app to interact with the standard Android framework APIs instead of the Peripheral I/O APIs.

The following code registers a `ButtonInputDriver` peripheral as a key input with the framework. The driver will generate key events using the provided _key code_ each time the button is pressed or released.

```
    import com.google.android.things.userdriver.InputDriver;import com.google.android.things.userdriver.UserDriverManager;...public class ButtonActivity extends Activity {    private static final String TAG = "ButtonActivity";    ...    private ButtonInputDriver mButtonInputDriver;    ...    @Override    protected void onStart() {        super.onStart();        mButtonInputDriver.register();    }    @Override    protected void onStop() {        super.onStop();        mButtonInputDriver.unregister();    }    @Override    public boolean onKeyDown(int keyCode, KeyEvent event) {        if (keyCode == KeyEvent.KEYCODE_SPACE) {            // Handle button pressed event            return true;        }        return super.onKeyDown(keyCode, event);    }    @Override    public boolean onKeyUp(int keyCode, KeyEvent event) {        if (keyCode == KeyEvent.KEYCODE_SPACE) {            // Handle button released event            return true;        }        return super.onKeyUp(keyCode, event);    }}
```


