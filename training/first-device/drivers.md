# Integrate Peripheral Drivers

# 整合外设驱动

## This lesson teaches you to

## 这一部分内容将为你展示

1.  Integrate Driver Libraries

1. 整合驱动库

2.  Register Framework Drivers

2. 注册框架驱动

## Try it out

## 试试看

*   [Button sample app](https://github.com/androidthings/sample-button)


Get up and running quickly with pre-built drivers from the [Peripheral Driver Library](https://developer.android.google.cn/things/sdk/driver-library.html). These drivers abstract the low-level communication details associated with many common hardware peripherals.

通过从 [Peripheral Driver Library](https://developer.android.google.cn/things/sdk/driver-library.html) 获取的已经构建好的驱动，你可以快速的启动并允许 Android Things 应用。这些驱动可以将那些与很多常见硬件外设相关联的底层通信细节抽象化。

In this lesson, you will learn to integrate library drivers into your app and bind peripherals to the Android framework through the `UserDriverManager`.

在这节课里，你将学会如何将驱动库整合进你的应用中，并通过利用 `UserDriverManager` 将外设绑定到 Android Framework 之中。

## 初始化驱动库

* * *

To import a library driver into your app:

为了将驱动库导入到你的应用中，你需要：

1.  Search the [driver index](https://developer.android.google.cn/things/sdk/driver-library.html) for a driver that matches your peripheral.

1. 在 [driver index](https://developer.android.google.cn/things/sdk/driver-library.html) 中搜索与你的外设设备相匹配的驱动。

2.  Add the driver artifact dependency to your app-level `build.gradle` file:

2. 在应用层的 `build.gradle` 文件中添加驱动依赖：

        dependencies {    ...    compile 'com.google.android.things.contrib:driver-button:0.3'}

3.  Add the required permissions for the driver to your app's manifest file:

3. 在应用的 manifest 文件中为驱动添加其所需要的权限：

        <uses-permission android:name="com.google.android.things.permission.MANAGE_INPUT_DRIVERS" />

>   Different drivers may require different permissions. If you aren't sure which permissions are required for a specific driver, locate the driver's README file. See [User-Space Drivers](https://developer.android.google.cn/things/sdk/drivers/index.html) for more information.

> 不同的驱动可能需要不同的权限。如果你不知道某些驱动具体需要什么权限，你可以在驱动的 README 文件查看相关信息。详情请见 [User-Space Drivers](https://developer.android.google.cn/things/sdk/drivers/index.html)。

4.  Initialize the driver class with the appropriate Peripheral I/O resources.

4. 使用正确的外设 I/O 资源来初始化驱动类。

5.  When your app no longer needs the resource, close the connection.

5. 当你的应用不在需要这个资源时，关闭相关链接。

~~~java

import com.google.android.things.contrib.driver.button.Button;
import  com.google.android.things.contrib.driver.button.ButtonInputDriver;
public class ButtonActivity extends Activity {
        private static final String TAG = "ButtonActivity";
        private static final String BUTTON_PIN_NAME = ; // GPIO port wired to the button    
        private ButtonInputDriver mButtonInputDriver;    
        @Override    
        protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);        
                try {            
                        // Step 3\. Initialize button driver with selected GPIO pin       
                        mButtonInputDriver = new ButtonInputDriver(BUTTON_PIN_NAME,Button.LogicState.PRESSED_WHEN_LOW, KeyEvent.KEYCODE_SPACE);        
                } catch (IOException e) {            
                        Log.e(TAG, "Error configuring GPIO pin", e);        
                }    
        }
        @Override    
        protected void onDestroy(){
                super.onDestroy();        
                // Step 5\. Close the driver and unregister        
                if (mButtonInputDriver != null) {
                        try {
                                mButtonInputDriver.close();            
                        } 
                        catch (IOException e) {                
                                Log.e(TAG, "Error closing Button driver", e);            
                        }        
                }    
        }
}
~~~

## 与 Framework 进行绑定

* * *

The driver library includes [user drivers](https://developer.android.google.cn/things/sdk/drivers/index.html) for supported peripheral types. When these drivers are available, they allow your app to interact with the standard Android framework APIs instead of the Peripheral I/O APIs.

驱动库中包含与具体外设类型相匹配的[用户驱动](https://developer.android.google.cn/things/sdk/drivers/index.html)，当这些驱动可用时，它们允许你的应用使用标准的 Android Framework API 代替外设 I/O API 来与外设交互。

The following code registers a `ButtonInputDriver` peripheral as a key input with the framework. The driver will generate key events using the provided _key code_ each time the button is pressed or released.

接下来的代码将一个 `ButtonInputDriver` 外设在 Framework 中注册为一个按键输入驱动。这个驱动通过在按键按下或者释放时产生一个 key_code 来生成按键事件。

~~~java
import com.google.android.things.userdriver.InputDriver;
import com.google.android.things.userdriver.UserDriverManager;
public class ButtonActivity extends Activity {    
        private static final String TAG = "ButtonActivity";
        private ButtonInputDriver mButtonInputDriver;
        @Override
        protected void onStart() {
                super.onStart();
                mButtonInputDriver.register();
        }
        @Override
        protected void onStop() {
                super.onStop();
                mButtonInputDriver.unregister();
        }
        @Override
        public boolean onKeyDown(int keyCode, KeyEvent event){
                if (keyCode == KeyEvent.KEYCODE_SPACE) {
                        // Handle button pressed event        
                        return true;       
                }        
                return super.onKeyDown(keyCode, event);   
        }    
        @Override    
        public boolean onKeyUp(int keyCode, KeyEvent event) {        
                if (keyCode == KeyEvent.KEYCODE_SPACE) {            
                        // Handle button released event            
                        return true;        
                }        
                return super.onKeyUp(keyCode, event);    
        }
}
~~~


