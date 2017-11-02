# Sensor


The Android [sensor framework](https://developer.android.google.cn/guide/topics/sensors/sensors_overview.html) supports a wide variety of sensor types to measure the conditions of the physical environment and read the raw data from apps. Using sensor drivers, your apps can extend this framework and add new sensor devices connected over [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html).

The data from these sensors is delivered through the same [SensorManager](https://developer.android.google.cn/reference/android/hardware/SensorManager.html) APIs as the built-in Android sensors. Your app can implement a driver to connect a new sensor of a known type, such as an accelerometer, or a sensor type that Android doesn't currently define, such as a blood glucose sensor.

## Implementing the driver

* * *

The framework polls your driver periodically when listeners are registered for sensor updates. To respond to poll requests for new data, extend the `UserSensorDriver` class and override the `read()` method. Each call to read should return a new `UserSensorReading` containing the current sensor data.

<aside class="note">**Note:** <span>For more information on the data format expected for known sensor types, see [sensor event values](https://developer.android.google.cn/reference/android/hardware/SensorEvent.html#values).</span></aside>

The framework may call `read()` at a time when the driver is not able to produce a sensor reading. When this occurs, your driver should throw an `IOException`.

    UserSensorDriver mDriver = new UserSensorDriver() {    // Sensor data values    float x, y, z;    @Override    public UserSensorReading read() {        try {            // ...read the sensor hardware...            // Return a new reading            return new UserSensorReading(new float[]{x, y, z});        } (catch Exception e) {            // Error occurred reading the sensor hardware            throw new IOException("Unable to read sensor");        }    }};

If your sensor supports low power or sleep modes, override the `setEnabled()` method of your driver implementation to activate them. The framework calls this method to indicate that sensors should be ramped up to deliver readings or put to sleep to save power:

    UserSensorDriver mDriver = new UserSensorDriver() {    ...    // Called by the framework to toggle low power modes    @Override    public void setEnabled(boolean enabled) {        if (enabled) {            // Exit low power mode        } else {            // Enter low power mode        }    }};

<aside class="note">**Note:** <span>Sensors without low power modes can still use this callback to increase or reduce the reporting frequency of the data in order to manage power consumption.</span></aside>

## Describing the sensor

* * *

To add a new sensor driver to the Android framework:

1.  Use the `UserSensor.Builder` to declare the sensor's type.

    For most apps, the sensor type should match one of Android's existing known [Sensor](https://developer.android.google.cn/reference/android/hardware/Sensor.html) types.

2.  Provide the sensor name and vendor name of your driver.

3.  Apply the range, resolution, update frequency (delay), and power requirements (if available) of your sensor to the builder. These values assist the framework in selecting the best sensor based on the requests received by [SensorManager](https://developer.android.google.cn/reference/android/hardware/SensorManager.html).
4.  Attach your `UserSensorDriver` implementation with the `setDriver()` method.

        UserSensor accelerometer = UserSensor.builder()        .setName("GroveAccelerometer")        .setVendor("Seeed")        .setType(Sensor.TYPE_ACCELEROMETER)        .setDriver(mDriver)        .build();

When implementing a sensor for which Android does not have a defined type:

1.  Replace `setType()` with `setCustomType()` in the builder.
2.  Provide a numeric sensor type that is `TYPE_DEVICE_PRIVATE_BASE` or larger.
3.  Include a unique string name for the sensor type. This name should be unique system-wide, so using reverse domain notation is recommended.
4.  Include the sensor reporting mode.

        UserSensor custom = UserSensor.builder()        .setName("MySensor")        .setVendor("MyCompany")        .setCustomType(Sensor.TYPE_DEVICE_PRIVATE_BASE,                "com.example.mysensor",                Sensor.REPORTING_MODE_CONTINUOUS)        .setDriver(mDriver)        .build();

## Registering the sensor

* * *

Connect your new sensor to the framework by registering it with the `UserDriverManager`:

    public class SensorDriverService extends Service {    UserSensor mAccelerometer;    @Override    public void onCreate() {        super.onCreate();        ...        UserDriverManager manager = UserDriverManager.getManager();        // Create a new driver implementation        mAccelerometer = ...;        // Register the new driver with the framework        manager.registerSensor(mAccelerometer);    }    @Override    public void onDestroy() {        super.onDestroy();        ...        UserDriverManager manager = UserDriverManager.getManager();        // Unregister the driver when finished        manager.unregisterSensor(mAccelerometer);    }}

With the driver properly registered, apps can receive updates from the associated device using the existing Android [sensor framework services](https://developer.android.google.cn/guide/topics/sensors/sensors_overview.html).

The registration process can take some time before the sensor is available to clients. Apps interested in a user sensor should register a [DynamicSensorCallback](https://developer.android.google.cn/reference/android/hardware/SensorManager.DynamicSensorCallback.html) to be notified when it is available before registering a listener for sensor readings:

    public class SensorActivity extends Activity implements SensorEventListener {    private SensorManager mSensorManager;    private SensorCallback mCallback = new SensorCallback();    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        ...        mSensorManager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);        mSensorManager.registerDynamicSensorCallback(mCallback);    }    @Override    protected void onDestroy() {        super.onDestroy();        ...        mSensorManager.unregisterDynamicSensorCallback(mCallback);    }    @Override    public void onSensorChanged(SensorEvent event) {        ...    }    @Override    public void onAccuracyChanged(Sensor sensor, int accuracy) {        ...    }    // Listen for registration events from the sensor driver    private class SensorCallback extends SensorManager.DynamicSensorCallback {        @Override        public void onDynamicSensorConnected(Sensor sensor) {            // Begin listening for sensor readings            mSensorManager.registerListener(SensorActivity.this, sensor,                    SensorManager.SENSOR_DELAY_NORMAL);        }        @Override        public void onDynamicSensorDisconnected(Sensor sensor) {            // Stop receiving sensor readings            mSensorManager.unregisterListener(SensorActivity.this);        }    }}

## Adding the required permission

* * *

Add the required permission for the user driver to your app's manifest file:

        <uses-permission android:name="com.google.android.things.permission.MANAGE_SENSOR_DRIVERS" />

