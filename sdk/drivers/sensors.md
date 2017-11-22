# Sensor

# 传感器


The Android [sensor framework](https://developer.android.google.cn/guide/topics/sensors/sensors_overview.html) supports a wide variety of sensor types to measure the conditions of the physical environment and read the raw data from apps. Using sensor drivers, your apps can extend this framework and add new sensor devices connected over [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html).

Android 的[传感器框架](https://developer.android.google.cn/guide/topics/sensors/sensors_overview.html)支持的传感器类型相对广泛，使用该框架可以测量物理环境的状态并从应用中读取原始数据。而通过使用传感器驱动，你的应用可以扩展这个框架，并且可以添加连接到 [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) 上的新设备。

The data from these sensors is delivered through the same [SensorManager](https://developer.android.google.cn/reference/android/hardware/SensorManager.html) APIs as the built-in Android sensors. Your app can implement a driver to connect a new sensor of a known type, such as an accelerometer, or a sensor type that Android doesn't currently define, such as a blood glucose sensor.

从这些传感器中获取的数据被通过 和 Android 内置传感器相同的 [SensorManager](https://developer.android.google.cn/reference/android/hardware/SensorManager.html) APIs 传递。 你的应用可以实现一个驱动，用来连接一个新的已知类型的传感器设备，例如加速度计，或者是当前 Android 未定义的设备类型，比如血糖传感器。

## Implementing the driver

## 实现传感器驱动

* * *

The framework polls your driver periodically when listeners are registered for sensor updates. To respond to poll requests for new data, extend the `UserSensorDriver` class and override the `read()` method. Each call to read should return a new `UserSensorReading` containing the current sensor data.

当监听器已被注册接收传感器更新时，framework 会间歇性地拉取你的驱动。为了响应拉取请求以提供新的数据，需要继承 `UserSensorDriver` 类并覆写 `read()` 方法。每次调用这个方法时，应当返回包含当前传感器数据的 `UserSensorReading` 实例。

<aside class="note">**Note:** <span>For more information on the data format expected for known sensor types, see [sensor event values](https://developer.android.google.cn/reference/android/hardware/SensorEvent.html#values).</span></aside>

<aside class="note">**注意:** <span>想要获取更多关于已知传感器类型的数据格式信息，查阅[传感器事件值](https://developer.android.google.cn/reference/android/hardware/SensorEvent.html#values).</span></aside>

The framework may call `read()` at a time when the driver is not able to produce a sensor reading. When this occurs, your driver should throw an `IOException`.

当传感器驱动不能产生数据更新时，framework 也可能会调用 `read()` 方法，当这种情况出现时，你的驱动应当抛出一个 `IOException`。
``` java
UserSensorDriver mDriver = new UserSensorDriver() {
    // Sensor data values
    float x, y, z;

    @Override
    public UserSensorReading read() {
        try {
            // ...read the sensor hardware...

            // Return a new reading
            return new UserSensorReading(new float[]{x, y, z});
        } (catch Exception e) {
            // Error occurred reading the sensor hardware
            throw new IOException("Unable to read sensor");
        }
    }
};
```

If your sensor supports low power or sleep modes, override the `setEnabled()` method of your driver implementation to activate them. The framework calls this method to indicate that sensors should be ramped up to deliver readings or put to sleep to save power:

如果你的传感器支持低功耗或睡眠模式，重写驱动实现中的 `setEnabled()` 方法来激活该功能。Framework 调用这个方法来表征这个传感器应当被唤起传递数据或是进入睡眠模式节能：
``` java
UserSensorDriver mDriver = new UserSensorDriver() {
    ...

    // Called by the framework to toggle low power modes
    @Override
    public void setEnabled(boolean enabled) {
        if (enabled) {
            // Exit low power mode
        } else {
            // Enter low power mode
        }
    }
};
```

<aside class="note">**Note:** <span>Sensors without low power modes can still use this callback to increase or reduce the reporting frequency of the data in order to manage power consumption.</span></aside>

<aside class="note">**注意:** <span>没有低功耗模式的传感器同样能使用这个回调方法来提高或降低数据的上报频率以便管理节点的能耗。</span></aside>

## Describing the sensor

## 描述传感器

* * *

To add a new sensor driver to the Android framework:

在 Android framework 中添加一种新的传感器需要：

1.  Use the `UserSensor.Builder` to declare the sensor's type.

    For most apps, the sensor type should match one of Android's existing known [Sensor](https://developer.android.google.cn/reference/android/hardware/Sensor.html) types.

2.  Provide the sensor name and vendor name of your driver.

3.  Apply the range, resolution, update frequency (delay), and power requirements (if available) of your sensor to the builder. These values assist the framework in selecting the best sensor based on the requests received by [SensorManager](https://developer.android.google.cn/reference/android/hardware/SensorManager.html).
4.  Attach your `UserSensorDriver` implementation with the `setDriver()` method.

        UserSensor accelerometer = UserSensor.builder()        .setName("GroveAccelerometer")        .setVendor("Seeed")        .setType(Sensor.TYPE_ACCELEROMETER)        .setDriver(mDriver)        .build();
        
* * *
1.  使用 `UserSensor.Builder` 来声明一种传感器类型。

    For most apps, the sensor type should match one of Android's existing known [Sensor](https://developer.android.google.cn/reference/android/hardware/Sensor.html) types.
    
    对于大部分应用来说，传感器类型应当与 Android 系统中已有的[传感器](https://developer.android.google.cn/reference/android/hardware/Sensor.html) 类型匹配。

2.  为你的驱动提供传感器名称和供应商的名称。

3.  在构建器中提供你的传感器的范围，分辨率，更新频率（延迟），以及能耗需求（如果可用的话）。这些值可以帮助 framework 根据 [SensorManager](https://developer.android.google.cn/reference/android/hardware/SensorManager.html) 收到的需求选择最佳的传感器。
4.  使用 `setDriver()` 方法添加你的  `UserSensorDriver` 实现。
``` java
UserSensor accelerometer = UserSensor.builder()
        .setName("GroveAccelerometer")
        .setVendor("Seeed")
        .setType(Sensor.TYPE_ACCELEROMETER)
        .setDriver(mDriver)
        .build();
```

When implementing a sensor for which Android does not have a defined type:

当实现一种 Android 系统未定义的传感器时：

1.  Replace `setType()` with `setCustomType()` in the builder.
2.  Provide a numeric sensor type that is `TYPE_DEVICE_PRIVATE_BASE` or larger.
3.  Include a unique string name for the sensor type. This name should be unique system-wide, so using reverse domain notation is recommended.
4.  Include the sensor reporting mode.

        UserSensor custom = UserSensor.builder()        .setName("MySensor")        .setVendor("MyCompany")        .setCustomType(Sensor.TYPE_DEVICE_PRIVATE_BASE,                "com.example.mysensor",                Sensor.REPORTING_MODE_CONTINUOUS)        .setDriver(mDriver)        .build();
        
* * *
1.  在构建器中使用 `setCustomType()` 替换`setType()`。
2.  提供一个数值为 `TYPE_DEVICE_PRIVATE_BASE` 或更大的数字型传感器类型。
3.  为传感器类型提供一个唯一的字符名。这个名称应当在系统范围内唯一，因此推荐使用倒置的域名记号。
4.  提供传感器的上报模式。
``` java
UserSensor custom = UserSensor.builder()
        .setName("MySensor")
        .setVendor("MyCompany")
        .setCustomType(Sensor.TYPE_DEVICE_PRIVATE_BASE,
                "com.example.mysensor",
                Sensor.REPORTING_MODE_CONTINUOUS)
        .setDriver(mDriver)
        .build();

```

## Registering the sensor

## 注册传感器

* * *

Connect your new sensor to the framework by registering it with the `UserDriverManager`:

使用 `UserDriverManager` 将新传感器注册到 framework 中：
``` java
public class SensorDriverService extends Service {

    UserSensor mAccelerometer;

    @Override
    public void onCreate() {
        super.onCreate();
        ...
        UserDriverManager manager = UserDriverManager.getManager();

        // Create a new driver implementation
        mAccelerometer = ...;
        // Register the new driver with the framework
        manager.registerSensor(mAccelerometer);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        ...
        UserDriverManager manager = UserDriverManager.getManager();
        // Unregister the driver when finished
        manager.unregisterSensor(mAccelerometer);
    }
}
```

With the driver properly registered, apps can receive updates from the associated device using the existing Android [sensor framework services](https://developer.android.google.cn/guide/topics/sensors/sensors_overview.html).

当传感器被正确注册后，应用可以使用 Android 现有的[传感器框架服务](https://developer.android.google.cn/guide/topics/sensors/sensors_overview.html)从相关传感器设备处获取更新。

The registration process can take some time before the sensor is available to clients. Apps interested in a user sensor should register a [DynamicSensorCallback](https://developer.android.google.cn/reference/android/hardware/SensorManager.DynamicSensorCallback.html) to be notified when it is available before registering a listener for sensor readings:

在传感器可用之前，注册过程可能会需要一些时间。在注册传感器数据监听器之前，对用户传感器敏感的应用应当注册一个 [DynamicSensorCallback](https://developer.android.google.cn/reference/android/hardware/SensorManager.DynamicSensorCallback.html)，这样，当传感器可用时，它将会告知应用：
``` java
public class SensorActivity extends Activity implements SensorEventListener {

    private SensorManager mSensorManager;
    private SensorCallback mCallback = new SensorCallback();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ...
        mSensorManager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);
        mSensorManager.registerDynamicSensorCallback(mCallback);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        ...
        mSensorManager.unregisterDynamicSensorCallback(mCallback);
    }

    @Override
    public void onSensorChanged(SensorEvent event) {
        ...
    }

    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy) {
        ...
    }

    // Listen for registration events from the sensor driver
    private class SensorCallback extends SensorManager.DynamicSensorCallback {
        @Override
        public void onDynamicSensorConnected(Sensor sensor) {
            // Begin listening for sensor readings
            mSensorManager.registerListener(SensorActivity.this, sensor,
                    SensorManager.SENSOR_DELAY_NORMAL);
        }

        @Override
        public void onDynamicSensorDisconnected(Sensor sensor) {
            // Stop receiving sensor readings
            mSensorManager.unregisterListener(SensorActivity.this);
        }
    }
}
```

## Adding the required permission

## 添加必需权限

* * *

Add the required permission for the user driver to your app's manifest file:

在你应用的清单文件中添加用户驱动所需的权限：

        <uses-permission android:name="com.google.android.things.permission.MANAGE_SENSOR_DRIVERS" />

