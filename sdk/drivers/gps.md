# GPS


The GPS user driver allows your app to publish updates to the device's physical location through the Android [location services](https://developer.android.google.cn/training/location/index.html).

GPS modules are receive-only devices that triangulate signals from remote satellites in order to determine an accurate physical location. Once the GPS module collects enough satellite data to calculate an accurate position, it has a valid location (a **fix** ) that it can report.

GPS modules typically connect to the host system via [UART](https://developer.android.google.cn/things/sdk/pio/uart.html), but may use other forms of [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html). For example, they may contain additional [GPIO](https://developer.android.google.cn/things/sdk/pio/gpio.html) pins to control power management or report when the module has gained or lost a fix.

<aside class="note">**Note:** <span>The framework only supports a single source for GPS location data. You cannot register multiple GPS drivers.</span></aside>

## Creating the driver

* * *

The driver implementation is responsible for communicating with the connected GPS hardware, monitoring for valid location changes, and reporting those changes to the framework. To create a driver:

1.  Create a new `GpsDriver` instance:
2.  Register the driver with the `UserDriverManager`:

        import com.google.android.things.userdriver.GpsDriver;import com.google.android.things.userdriver.UserDriverManager;...public class GpsDriverService extends Service {    private GpsDriver mDriver;    @Override    public void onCreate() {        super.onCreate();        // Create a new driver implementation        mDriver = new GpsDriver();        // Register with the framework        UserDriverManager manager = UserDriverManager.getManager();        manager.registerGpsDriver(mDriver);    }}

3.  Unregister the driver when location events are not longer required.

        public class GpsDriverService extends Service {    ...    @Override    protected void onDestroy() {        super.onDestroy();        UserDriverManager manager = UserDriverManager.getManager();        manager.unregisterGpsDriver(mDriver);    }}

## Reporting location

* * *

Report each new location fix to the Android framework with the `reportLocation()` method. This method accepts a [Location](https://developer.android.google.cn/reference/android/location/Location.html) object, which contains the updated contents to report. Set the provider for each reported `Location` to `LocationManager.GPS_PROVIDER`.

        import android.location.Location;    ...    public class GpsDriverService extends Service {        private GpsDriver mDriver;        ...        private Location parseLocationFromString(String gpsData) {            Location result = new Location(LocationManager.GPS_PROVIDER);            // ...parse raw GPS information...            return result;        }        public void handleLocationUpdate(String rawGpsData) {            // Convert raw data into a location object            Location location = parseLocationFromString(rawGpsData);            // Send the location update to the framework            mDriver.reportLocation(location);        }    }

<aside class="note">**Note:** <span>Drivers should send every update discovered from the GPS hardware. The framework filters updates delivered to apps based on their request criteria.</span></aside>

The following table describes the `Location` attributes that a GPS driver can report to the framework. Attributes marked _required_ must be included or the framework will reject the location update:

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

## Converting GPS data to a location object

* * *

GPS hardware typically reports location information as ASCII strings in the [NMEA standard format](https://en.wikipedia.org/wiki/NMEA_0183). Each line of data is a comma-separated list of data values known as a _sentence_. While each GPS module may choose to report different portions of the NMEA protocol, most devices send one or more of the following sentences:

*   **GPGGA** (Fix Information): Includes position fix, altitude, timestamp, and satellite metadata.
*   **GPGLL** (Geographic Latitude/Longitude): Includes position fix and timestamp.
*   **GPRMC** (Recommended Minimum Navigation): Includes position fix, speed, timestamp, and navigation metadata.

As an example, the `GPRMC` sentence takes the following format:

    $GPRMC,<Time>,<Status>,<Latitude>,<Longitude>,<Speed>,<Angle>,<Date>,<Variation>,<Integrity>,<Checksum>

Using real data values, here is a sample `GPRMC` with the latitude, longitude, and speed portions highlighted in bold:

    $GPRMC,172934.975,V,3554.931,N,07402.499,W,16.4,3.35,300816,,E*41

The following code example parses the `GPRMC` string format into a [Location](https://developer.android.google.cn/reference/android/location/Location.html) instance:

    // Convert latitude from DMS to decimal formatprivate float parseLatitude(String latString, String hemisphere) {    float lat = Float.parseFloat(latString.substring(2))/60.0f;    lat += Float.parseFloat(latString.substring(0, 2));    if (hemisphere.contains("S")) {        lat *= -1;    }    return lat;}// Convert longitude from DMS to decimal formatprivate float parseLongitude(String longString, String hemisphere) {    float lat = Float.parseFloat(longString.substring(3))/60.0f;    lat += Float.parseFloat(longString.substring(0, 3));    if (hemisphere.contains("W")) {        lat *= -1;    }    return lat;}// Return a location from an NMEA GPRMC stringpublic Location parseLocationFromString(String rawGpsData) {    // Tokenize the string input    String[] nmea = rawGpsData.split(",");    Location result = new Location(LocationManager.GPS_PROVIDER);    // Create timestamp from the date + time tokens    SimpleDateFormat format = new SimpleDateFormat("ddMMyyhhmmss.ss");    format.setTimeZone(TimeZone.getTimeZone("UTC"));    try {        Date date = format.parse(nmea[9] + nmea[1]);        result.setTime(date.getTime());    } catch (ParseException e) {        return null;    }    // Parse the fix information tokens    result.setLatitude(parseLatitude(nmea[3], nmea[4]));    result.setLongitude(parseLongitude(nmea[5], nmea[6]));    result.setSpeed(Float.parseFloat(nmea[7]));    return result;}

## Adding the required permission

* * *

Add the required permission for the user driver to your app's manifest file:

        <uses-permission android:name="com.google.android.things.permission.MANAGE_GPS_DRIVERS" />

