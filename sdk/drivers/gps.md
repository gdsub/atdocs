# GPS


The GPS user driver allows your app to publish updates to the device's physical location through the Android [location services](https://developer.android.google.cn/training/location/index.html).

GPS 设备驱动能够让你的应用通过 Android [位置服务](https://developer.android.google.cn/training/location/index.html) 更新设备的地理位置信息。

GPS modules are receive-only devices that triangulate signals from remote satellites in order to determine an accurate physical location. Once the GPS module collects enough satellite data to calculate an accurate position, it has a valid location (a **fix** ) that it can report.

GPS 模块是个单向接收的设备，它利用三角测量的方式从远程卫星获取信号，从而确定设备的实际地理位置。一旦 GPS 模块收集到足够用来计算准确位置的卫星数据后，它就会生成一个能够上报的有效位置信息 (一个 **定位**)。

GPS modules typically connect to the host system via [UART](https://developer.android.google.cn/things/sdk/pio/uart.html), but may use other forms of [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html). For example, they may contain additional [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html) pins to control power management or report when the module has gained or lost a fix.

GPS 模块通常使用 [UART](https://developer.android.google.cn/things/sdk/pio/uart.html) 连接到设备系统, 也有可能使用其它类型的 [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html)。例如，它们可能额外地包含 [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html) 针脚来进行电源管理，或者上报设备何时获得或失去定位。

<aside class="note">**Note:** <span>The framework only supports a single source for GPS location data. You cannot register multiple GPS drivers.</span></aside>

<aside class="note">**注意:** <span>Framework 仅支持单一的 GPS 位置信息数据源。你不能注册多个 GPS 驱动。</span></aside>

## Creating the driver

## 创建驱动

* * *

The driver implementation is responsible for communicating with the connected GPS hardware, monitoring for valid location changes, and reporting those changes to the framework. To create a driver:

这个驱动的实现是用来与 GPS 硬件通信，监听位置信息变化，并将这些变更上报到 framework 中。按照如下方法来创建驱动：

1.  Create a new `GpsDriver` instance:
2.  Register the driver with the `UserDriverManager`:

        import com.google.android.things.userdriver.GpsDriver;import com.google.android.things.userdriver.UserDriverManager;...public class GpsDriverService extends Service {    private GpsDriver mDriver;    @Override    public void onCreate() {        super.onCreate();        // Create a new driver implementation        mDriver = new GpsDriver();        // Register with the framework        UserDriverManager manager = UserDriverManager.getManager();        manager.registerGpsDriver(mDriver);    }}

3.  Unregister the driver when location events are not longer required.

        public class GpsDriverService extends Service {    ...    @Override    protected void onDestroy() {        super.onDestroy();        UserDriverManager manager = UserDriverManager.getManager();        manager.unregisterGpsDriver(mDriver);    }}
        
* * *

1.  创建一个新的 `GpsDriver` 实例：
2.  使用 `UserDriverManager` 来注册这个驱动:
``` java
import com.google.android.things.userdriver.GpsDriver;
import com.google.android.things.userdriver.UserDriverManager;
...

public class GpsDriverService extends Service {

    private GpsDriver mDriver;

    @Override
    public void onCreate() {
        super.onCreate();

        // Create a new driver implementation
        mDriver = new GpsDriver();

        // Register with the framework
        UserDriverManager manager = UserDriverManager.getManager();
        manager.registerGpsDriver(mDriver);
    }

}
```
3.  当不需要位置信息时，取消注册驱动。

``` java
public class GpsDriverService extends Service {
    ...

    @Override
    protected void onDestroy() {
        super.onDestroy();

        UserDriverManager manager = UserDriverManager.getManager();
        manager.unregisterGpsDriver(mDriver);
    }
}


```

## Reporting location

## 上报位置

* * *

Report each new location fix to the Android framework with the `reportLocation()` method. This method accepts a [Location](https://developer.android.google.cn/reference/android/location/Location.html) object, which contains the updated contents to report. Set the provider for each reported `Location` to `LocationManager.GPS_PROVIDER`.

使用 `reportLocation()` 方法上报每次定位更新到 Android framework 中。这个方法接受一个 [Location](https://developer.android.google.cn/reference/android/location/Location.html) 对象，该对象中包含需要上报的最新内容。将每个上报 `Location` 的 provider 类型设置为 `LocationManager.GPS_PROVIDER`。

``` java
import android.location.Location;
    ...

    public class GpsDriverService extends Service {

        private GpsDriver mDriver;
        ...

        private Location parseLocationFromString(String gpsData) {
            Location result = new Location(LocationManager.GPS_PROVIDER);

            // ...parse raw GPS information...

            return result;
        }

        public void handleLocationUpdate(String rawGpsData) {
            // Convert raw data into a location object
            Location location = parseLocationFromString(rawGpsData);

            // Send the location update to the framework
            mDriver.reportLocation(location);
        }
    }
```
    
<aside class="note">**Note:** <span>Drivers should send every update discovered from the GPS hardware. The framework filters updates delivered to apps based on their request criteria.</span></aside>

<aside class="note">**注意:** <span>驱动应当在每次从 GPS 硬件发现数据更新时将数据发出。Framework 中的过滤器会按照各个应用所需的更新频率标准去发送数据更新。</span></aside>

The following table describes the `Location` attributes that a GPS driver can report to the framework. Attributes marked _required_ must be included or the framework will reject the location update:

下面的表格描述了 GPS 驱动能够上报给 framework 的 `Location` 数据的所有属性。 标记有 _必需_ 的属性必须被包含在 `Location` 中，否则 framework 会丢弃该次上报的数据更新：

<table>

<tbody>

<tr>

<th>Required Attributes</th>

<th>Optional Attributes</th>

</tr>

<tr>

<td>[Accuracy](https://developer.android.google.cn/reference/android/location/Location.html#getAccuracy())  
[Timestamp](https://developer.android.google.cn/reference/android/location/Location.html#getTime())  
[Latitude](https://developer.android.google.cn/reference/android/location/Location.html#getLatitude())  
[Longitude](https://developer.android.google.cn/reference/android/location/Location.html#getLongitude())  
</td>

<td>[Altitude](https://developer.android.google.cn/reference/android/location/Location.html#getAltitude())  
[Bearing](https://developer.android.google.cn/reference/android/location/Location.html#getBearing())  
[Speed](https://developer.android.google.cn/reference/android/location/Location.html#getSpeed())  
</td>

</tr>

</tbody>

</table>

<table>

<tbody>

<tr>

<th>必需属性</th>

<th>可选属性</th>

</tr>

<tr>

<td>[准确率](https://developer.android.google.cn/reference/android/location/Location.html#getAccuracy())  
[时间戳](https://developer.android.google.cn/reference/android/location/Location.html#getTime())  
[纬度](https://developer.android.google.cn/reference/android/location/Location.html#getLatitude())  
[经度](https://developer.android.google.cn/reference/android/location/Location.html#getLongitude())  
</td>

<td>[高度](https://developer.android.google.cn/reference/android/location/Location.html#getAltitude())  
[方位](https://developer.android.google.cn/reference/android/location/Location.html#getBearing())  
[速度](https://developer.android.google.cn/reference/android/location/Location.html#getSpeed())  
</td>

</tr>

</tbody>

</table>

## Converting GPS data to a location object

## 将 GPS 数据转换成位置对象

* * *

GPS hardware typically reports location information as ASCII strings in the [NMEA standard format](https://en.wikipedia.org/wiki/NMEA_0183). Each line of data is a comma-separated list of data values known as a _sentence_. While each GPS module may choose to report different portions of the NMEA protocol, most devices send one or more of the following sentences:

GPS 硬件通常遵循 [NMEA standard format](https://en.wikipedia.org/wiki/NMEA_0183) 标准，使用 ASCII 码字符串来上报位置信息。每一行数据是被逗号分隔开的数据值列表，被称为 **语句**。 尽管每个 GPS 模块可能选择 NMEA 协议的不同部分去上报数据，但大部分的设备都会使用如下一个或多个句式:

*   **GPGGA** (Fix Information): Includes position fix, altitude, timestamp, and satellite metadata.
*   **GPGLL** (Geographic Latitude/Longitude): Includes position fix and timestamp.
*   **GPRMC** (Recommended Minimum Navigation): Includes position fix, speed, timestamp, and navigation metadata.


*   **GPGGA** (定位信息): 包含定位位置，高度，时间戳，以及卫星的元数据。
*   **GPGLL** (地理纬度/经度): 包含定位位置和时间戳。
*   **GPRMC** (推荐最小导航信息): 包含定位位置，速度，时间戳，以及导航的元数据。

As an example, the `GPRMC` sentence takes the following format:

举例来说, `GPRMC` 句式的格式如下：

    $GPRMC,<Time>,<Status>,<Latitude>,<Longitude>,<Speed>,<Angle>,<Date>,<Variation>,<Integrity>,<Checksum>

Using real data values, here is a sample `GPRMC` with the latitude, longitude, and speed portions highlighted in bold:

使用真实的数据值举例，例如这里有包含高度，经度，纬度以及被粗体高亮的速度等信息的 `GPRMC` 句式：

    $GPRMC,172934.975,V,3554.931,N,07402.499,W,16.4,3.35,300816,,E*41

The following code example parses the `GPRMC` string format into a [Location](https://developer.android.google.cn/reference/android/location/Location.html) instance:

下面的代码示例将 `GPRMC` 字符串格式解析成一个 [Location](https://developer.android.google.cn/reference/android/location/Location.html) 实例：

``` java
// Convert latitude from DMS to decimal format
private float parseLatitude(String latString, String hemisphere) {
    float lat = Float.parseFloat(latString.substring(2))/60.0f;
    lat += Float.parseFloat(latString.substring(0, 2));
    if (hemisphere.contains("S")) {
        lat *= -1;
    }
    return lat;
}

// Convert longitude from DMS to decimal format
private float parseLongitude(String longString, String hemisphere) {
    float lat = Float.parseFloat(longString.substring(3))/60.0f;
    lat += Float.parseFloat(longString.substring(0, 3));
    if (hemisphere.contains("W")) {
        lat *= -1;
    }
    return lat;
}

// Return a location from an NMEA GPRMC string
public Location parseLocationFromString(String rawGpsData) {
    // Tokenize the string input
    String[] nmea = rawGpsData.split(",");

    Location result = new Location(LocationManager.GPS_PROVIDER);
    // Create timestamp from the date + time tokens
    SimpleDateFormat format = new SimpleDateFormat("ddMMyyhhmmss.ss");
    format.setTimeZone(TimeZone.getTimeZone("UTC"));
    try {
        Date date = format.parse(nmea[9] + nmea[1]);
        result.setTime(date.getTime());
    } catch (ParseException e) {
        return null;
    }

    // Parse the fix information tokens
    result.setLatitude(parseLatitude(nmea[3], nmea[4]));
    result.setLongitude(parseLongitude(nmea[5], nmea[6]));
    result.setSpeed(Float.parseFloat(nmea[7]));

    return result;
}
```
## Adding the required permission

## 添加需要的权限

* * *

Add the required permission for the user driver to your app's manifest file:

在你应用的 manifest 文件中为使用用户驱动添加必要的权限声明：

        <uses-permission android:name="com.google.android.things.permission.MANAGE_GPS_DRIVERS" />

