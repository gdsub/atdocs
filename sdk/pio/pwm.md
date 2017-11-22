# PWM

# PWM


[Pulse Width Modulation](https://en.wikipedia.org/wiki/Pulse-width_modulation) (PWM) is a common method used to apply a proportional control signal to an external device using a digital output pin. For example, servo motors use the pulse width of an incoming PWM signal to determine their rotation angle. LCD displays adjust their brightness based on a PWM signal's average value.

[脉冲宽度调制](https://en.wikipedia.org/wiki/Pulse-width_modulation) (PWM, Pulse Width Modulation) 是一种针对数字输出引脚使用比例控制信号的通用方法。 例如，伺服电机使用输入 PWM 信号的脉冲宽度来确定它们的旋转角度，LCD显示屏根据 PWM 信号的平均值来调整亮度。

PWM is a digital (i.e. square wave) signal that oscillates according to a given **_frequency_** and **_duty cycle_**.

PWM根据给定的 **频率** 和 **占空比** 产生振荡，得到一个数字(例如方波)信号。

* The frequency (expressed in Hz) describes how often the output pulse repeats.
* The period is the time each cycle takes and is the inverse of frequency.
* The duty cycle (expressed as a percentage) describes the width of the pulse within that frequency window.

* 频率 (以 Hz 表示) 用来描述输出脉冲重复的次数。
* 周期表示每个循环所花费的时间，是频率的倒数。
* 占空比表示该频率窗口内的脉冲宽度。

For example, a PWM signal set to 50% duty is active for half of each cycle:

例如，一个 PWM 信号设置了 50% 的占空比，表示每一个循环内，有一半时间，是处于活动状态。

![](https://developer.android.google.cn/things/images/pwm-signal.png)

You can adjust the duty cycle to increase or decrease the average "on" time of the signal. The following diagram shows pulse trains at 0%, 25%, and 100% duty:

您可以调整占空比来增加或者减少信号处于活动状态的的平均时间。下图表示脉冲阵列为 0%, 25%, 100% 时候的占空情况。

![](https://developer.android.google.cn/things/images/pwm-duty.png)

<aside class="note">**Note:** <span>Most PWM hardware has to toggle at least once per cycle, so even duty values of 0% and 100% will have a small transition at the beginning of each cycle.</span></aside>

<aside class="note"> **注意:** <span>由于大多数 PWM 硬件必须每个周期至少切换一次，所以即使是占空比的值为 0% 或者是 100% ，仍然会在每个周期开始时，有一个小的过渡。</span></aside>

## Managing the connection

## 管理连接

* * *

In order to open a connection to a PWM port, you need to know the unique port name. During the initial stages of development, or when porting an app to new hardware, it's helpful to discover all the available port names from `PeripheralManagerService` using `getPwmList()`:

为了打开PWM端口的连接，你需要知道唯一的端口名称。在开发的最初阶段，或者当移植一款应用到新的硬件的时候，最有效的办法是使用 `PeripheralManagerService` 下的 `getPwmList()` 方法来查看所有可用的端口名称。

~~~java
    PeripheralManagerService manager = new PeripheralManagerService();
	List<String> portList = manager.getPwmList();
	if (portList.isEmpty()) {    
		Log.i(TAG, "No PWM port available on this device.");
	} 
	else {    
		Log.i(TAG, "List of available ports: " + portList);
	}
~~~

Once you know the target name, use `PeripheralManagerService` to connect to that port. When you are done communicating with the PWM port, close the connection to free up resources. Additionally, you cannot open a new connection to the port until the existing connection is closed. To close the connection, use the port's `close()` method.

当你知道的目标设备的名称，就可以用 `PeripheralManagerService` 方法进行连接。当你已经完成与 PWM 端口的数据传输时，记得关闭连接来释放资源。在已有连接关闭之前，请不要打开一个新的端口连接。用设备的 `close()` 方法可以关闭连接。

~~~java
    public class HomeActivity extends Activity {    
		// PWM Name    
		private static final String PWM_NAME = ...;    
		private Pwm mPwm;    

		@Override    protected void onCreate(Bundle savedInstanceState) {        
			super.onCreate(savedInstanceState);        
			// Attempt to access the PWM port        
			try {            
				mPwm = mPeripheralManager.openPwm(PWM_NAME);        
			} 
			catch (IOException e) {            
				Log.w(TAG, "Unable to access PWM", e);        
			}    
		}    

		@Override    protected void onDestroy() {        
			super.onDestroy();        
			if (mPwm != null) {            
				try {                
					mPwm.close();                
					mPwm = null;            
				} 
				catch (IOException e) {                
					Log.w(TAG, "Unable to close PWM", e);            
				}        
			}    
		}
	}
~~~

## Configuring and controlling the PWM signal

## 配置并控制PWM信号

* * *

After making a connection, configure the timing parameters for the PWM signal. You must set these parameters before activating the signal the first time. To activate the PWM signal, call `setEnabled(true)`. If you need to temporarily de-activate the signal, you can call `setEnabled(false)`.

建立连接之后，就可以配置PWM信号的时间参数。一定要记得，在第一次激活信号之前配置这些参数。使用 `setEnabled(true)` 方法激活PWM信号， 使用 `setEnabled(false)` 休眠PWM信号。 

The following example configures the PWM to cycle at 120Hz (period of 8.33ms) with a duty of 25% (on-time of 2.08ms every cycle):

下面的例子配置了PWM的频率为 120Hz (周期为 8.33ms) ，占空比为 25% (每个周期的活动时间大约为2.08ms)。

~~~java
    public void initializePwm(Pwm pwm) throws IOException {    
			pwm.setPwmFrequencyHz(120);    
			pwm.setPwmDutyCycle(25);    
			// Enable the PWM signal    
			pwm.setEnabled(true);
}

