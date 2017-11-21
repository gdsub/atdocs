# Input


# 输入

Input user drivers provide an interface for apps to inject events into Android's [input pipeline](http://source.android.google.cn/devices/input/overview.html). With this API, apps can emulate a Human Interface Device (HID) or connect external hardware to the input system using [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html).

输入用户驱动为应用提供向 Android 的[输入管道](http://source.android.google.cn/devices/input/overview.html) 注入事件的功能。有了这些 API 后, 应用可以模拟人体接口设备 (HID) 或使用 [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) 连接外部硬件到系统的输入中。

## Key Events

## 按键事件

* * *

Key events indicate a momentary press and release of an input switch. They are generally used for generic button input (e.g. volume keys, media playback keys) and the keys of a keyboard. Android represents each event as a [KeyEvent](https://developer.android.google.cn/reference/android/view/KeyEvent.html) instance.

按键事件即一个输入开关被短暂按下并释放的过程。这种事件一般代表常见的按钮输入（例如，音量键盘，媒体重播键）以及键盘的按键输入。Android 将每个事件表示为 [KeyEvent](https://developer.android.google.cn/reference/android/view/KeyEvent.html) 实例。

1.  Create a new driver instance using the `InputDriver.Builder` and the source type `SOURCE_CLASS_BUTTON`.
2.  Register the driver with the `UserDriverManager`.

        import com.google.android.things.userdriver.InputDriver;import com.google.android.things.userdriver.UserDriverManager;...public class ButtonDriverService extends Service {    // Driver parameters    private static final String DRIVER_NAME = "EscapeButton";    private static final int DRIVER_VERSION = 1;    // Key code for driver to emulate    private static final int KEY_CODE = KeyEvent.KEYCODE_ESCAPE;    private InputDriver mDriver;    @Override    public void onCreate() {        super.onCreate();        // Create a new driver instance        mDriver = InputDriver.builder(InputDevice.SOURCE_CLASS_BUTTON)                .setName(DRIVER_NAME)                .setVersion(DRIVER_VERSION)                .setKeys(new int[] {KEY_CODE})                .build();        // Register with the framework        UserDriverManager manager = UserDriverManager.getManager();        manager.registerInputDriver(mDriver);    }    @Override    public IBinder onBind(Intent intent) {        return null;    }}

3.  When a hardware event occurs, construct a new `KeyEvent` for each state chnage with the current key code and input action.

4.  Inject the events into the driver with the `emit()` method.

        public class ButtonDriverService extends Service {    ...    // A state change has occurred    private void triggerEvent(boolean pressed) {        int action = pressed ? KeyEvent.ACTION_DOWN : KeyEvent.ACTION_UP;        KeyEvent[] events = new KeyEvent[] {new KeyEvent(action, KEY_CODE)};        if (!mDriver.emit(events)) {            Log.w(TAG, "Unable to emit key event");        }    }}

5.  Unregister the driver when key events are not longer required.

        public class ButtonDriverService extends Service {    ...    @Override    public void onDestroy() {        super.onDestroy();        UserDriverManager manager = UserDriverManager.getManager();        manager.unregisterInputDriver(mDriver);    }}
        
* * *

1.  使用 `InputDriver.Builder` 创建一个新的驱动实例，并将源类型声明为 `SOURCE_CLASS_BUTTON`。
2.  使用 `UserDriverManager` 注册该驱动实例。
``` java
import com.google.android.things.userdriver.InputDriver;
import com.google.android.things.userdriver.UserDriverManager;
...

public class ButtonDriverService extends Service {

    // Driver parameters
    private static final String DRIVER_NAME = "EscapeButton";
    private static final int DRIVER_VERSION = 1;
    // Key code for driver to emulate
    private static final int KEY_CODE = KeyEvent.KEYCODE_ESCAPE;

    private InputDriver mDriver;

    @Override
    public void onCreate() {
        super.onCreate();

        // Create a new driver instance
        mDriver = InputDriver.builder(InputDevice.SOURCE_CLASS_BUTTON)
                .setName(DRIVER_NAME)
                .setVersion(DRIVER_VERSION)
                .setKeys(new int[] {KEY_CODE})
                .build();

        // Register with the framework
        UserDriverManager manager = UserDriverManager.getManager();
        manager.registerInputDriver(mDriver);
    }

    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }
}
```
3.  当硬件事件发生时，使用当前的键值和输入动作为每个状态改变构建一个新的 `KeyEvent`。

4.  使用 `emit()` 方法将这个事件注入到驱动中。
``` java
public class ButtonDriverService extends Service {
    ...

    // A state change has occurred
    private void triggerEvent(boolean pressed) {
        int action = pressed ? KeyEvent.ACTION_DOWN : KeyEvent.ACTION_UP;
        KeyEvent[] events = new KeyEvent[] {new KeyEvent(action, KEY_CODE)};

        if (!mDriver.emit(events)) {
            Log.w(TAG, "Unable to emit key event");
        }
    }
}
```
5.  当按键事件不再需要时，取消注册该驱动。
``` java
public class ButtonDriverService extends Service {
    ...

    @Override
    public void onDestroy() {
        super.onDestroy();

        UserDriverManager manager = UserDriverManager.getManager();
        manager.unregisterInputDriver(mDriver);
    }
}
```

## Motion Events

## 运动事件

* * *

Input drivers can also emit motion events to connect a pointing device to the framework, such as a touchpad or mouse. These devices report an absolute position value as an x/y coordinate. Each event include an optional pressed state to indicate if the event represents a "tap" or "click" event at that location.

输入驱动还能产生运动事件用来配合连接一个指向性设备到 framework 中，例如触控板或鼠标。这些设备以 x/y 坐标系的形式上报它们的绝对位置。每个事件还包含一个可选的按压状态，用以表明在那个位置时设备是否处于“按压”或“点击”状态。

<aside class="note">**Note:** <span>You must report all coordinates as positive integer values.</span></aside>

<aside class="note">**注意:** <span>你上报的坐标必须是正整数值。</span></aside>

1.  Create a new driver instance using the `InputDriver.Builder` and the source type `SOURCE_TOUCHPAD`.
2.  Register the driver with the `UserDriverManager`.

        import com.google.android.things.userdriver.InputDriver;...public class TouchpadDriverService extends Service {    // Driver parameters    private static final String DRIVER_NAME = "Touchpad";    private static final int DRIVER_VERSION = 1;    private InputDriver mDriver;    @Override    public void onCreate() {        super.onCreate();        mDriver = InputDriver.builder(InputDevice.SOURCE_TOUCHPAD)                .setName(DRIVER_NAME)                .setVersion(DRIVER_VERSION)                .setAbsMax(MotionEvent.AXIS_X, 255)                .setAbsMax(MotionEvent.AXIS_Y, 255)                .build();        UserDriverManager manager = UserDriverManager.getManager();        manager.registerInputDriver(mDriver);    }    @Override    public IBinder onBind(Intent intent) {        return null;    }}

3.  When a hardware event occurs, inject the new coordinates into the driver with the `emit()` method.

        public class TouchpadDriverService extends Service {    ...    // A state change has occurred    private void triggerEvent(int x, int y, boolean pressed) {        if (!mDriver.emit(x, y, pressed)) {            Log.w(TAG, "Unable to emit motion event");        }    }}

4.  Unregister the driver when pointer events are not longer required.

        public class TouchpadDriverService extends Service {    ...    @Override    public void onDestroy() {        super.onDestroy();        UserDriverManager manager = UserDriverManager.getManager();        manager.unregisterInputDriver(mDriver);    }}
        
* * *

1.  使用 `InputDriver.Builder` 创建一个新的驱动实例，并将源类型声明为 `SOURCE_TOUCHPAD`。
2.  使用 `UserDriverManager` 注册该驱动实例。
``` java
import com.google.android.things.userdriver.InputDriver;
...

public class TouchpadDriverService extends Service {

    // Driver parameters
    private static final String DRIVER_NAME = "Touchpad";
    private static final int DRIVER_VERSION = 1;

    private InputDriver mDriver;

    @Override
    public void onCreate() {
        super.onCreate();

        mDriver = InputDriver.builder(InputDevice.SOURCE_TOUCHPAD)
                .setName(DRIVER_NAME)
                .setVersion(DRIVER_VERSION)
                .setAbsMax(MotionEvent.AXIS_X, 255)
                .setAbsMax(MotionEvent.AXIS_Y, 255)
                .build();

        UserDriverManager manager = UserDriverManager.getManager();
        manager.registerInputDriver(mDriver);
    }

    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }
}
```
3.  当硬件事件发生时，使用 `emit()` 方法将新坐标注入到驱动中。
``` java
public class TouchpadDriverService extends Service {
    ...

    // A state change has occurred
    private void triggerEvent(int x, int y, boolean pressed) {

        if (!mDriver.emit(x, y, pressed)) {
            Log.w(TAG, "Unable to emit motion event");
        }
    }
}
```
4.  当不在需要指针事件时，取消注册该驱动。
``` java
public class TouchpadDriverService extends Service {
    ...

    @Override
    public void onDestroy() {
        super.onDestroy();

        UserDriverManager manager = UserDriverManager.getManager();
        manager.unregisterInputDriver(mDriver);
    }
}
```

## Handling Input Events

## 处理输入事件

* * *

Android delivers input events to the foreground activity through various callback methods. Your app receives key events through the `onKeyDown()` and `onKeyUp()` methods, and all other input events through the `onGenericMotionEvent()` method.

Android 通过多种回调方法传递输入事件给前台活动。你的应用通过使用 `onKeyDown()` 和 `onKeyUp()` 方法接收按键事件,以及通过 `onGenericMotionEvent()` 方法接收所有其它输入事件。
``` java
public class HomeActivity extends Activity {

    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        // Handle key pressed and repeated events

        return true;
    }

    @Override
    public boolean onKeyUp(int keyCode, KeyEvent event) {
        // Handle key released events

        return true;
    }

    @Override
    public boolean onGenericMotionEvent(MotionEvent event) {
        // Handle motion input events

        return true;
    }
}
```

See [Handling Controller Actions](https://developer.android.google.cn/training/game-controllers/controller-input.html) for more details on how Android handles input events from external source devices.

查阅[处理控制器的行为](https://developer.android.google.cn/training/game-controllers/controller-input.html) 来获取更多有关 Android 如何处理从外部设备源中获取输入事件的详细信息。

## Adding the required permission

## 添加必需权限

* * *

Add the required permission for the user driver to your app's manifest file:

在你应用的清单文件中添加用户驱动所需的权限：

        <uses-permission android:name="com.google.android.things.permission.MANAGE_INPUT_DRIVERS" />

