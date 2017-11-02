# PWM


[Pulse Width Modulation](https://en.wikipedia.org/wiki/Pulse-width_modulation) (PWM) is a common method used to apply a proportional control signal to an external device using a digital output pin. For example, servo motors use the pulse width of an incoming PWM signal to determine their rotation angle. LCD displays adjust their brightness based on a PWM signal's average value.

PWM is a digital (i.e. square wave) signal that oscillates according to a given **_frequency_** and **_duty cycle_**.

*   The frequency (expressed in Hz) describes how often the output pulse repeats.
*   The period is the time each cycle takes and is the inverse of frequency.
*   The duty cycle (expressed as a percentage) describes the width of the pulse within that frequency window.

For example, a PWM signal set to 50% duty is active for half of each cycle:

![](https://developer.android.google.cn/things/images/pwm-signal.png)

You can adjust the duty cycle to increase or decrease the average "on" time of the signal. The following diagram shows pulse trains at 0%, 25%, and 100% duty:

![](https://developer.android.google.cn/things/images/pwm-duty.png)

<aside class="note">**Note:** <span>Most PWM hardware has to toggle at least once per cycle, so even duty values of 0% and 100% will have a small transition at the beginning of each cycle.</span></aside>

## Managing the connection

* * *

In order to open a connection to a PWM port, you need to know the unique port name. During the initial stages of development, or when porting an app to new hardware, it's helpful to discover all the available port names from `PeripheralManagerService` using `getPwmList()`:

    PeripheralManagerService manager = new PeripheralManagerService();List<String> portList = manager.getPwmList();if (portList.isEmpty()) {    Log.i(TAG, "No PWM port available on this device.");} else {    Log.i(TAG, "List of available ports: " + portList);}

Once you know the target name, use `PeripheralManagerService` to connect to that port. When you are done communicating with the PWM port, close the connection to free up resources. Additionally, you cannot open a new connection to the port until the existing connection is closed. To close the connection, use the port's `close()` method.

    public class HomeActivity extends Activity {    // PWM Name    private static final String PWM_NAME = ...;    private Pwm mPwm;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Attempt to access the PWM port        try {            mPwm = mPeripheralManager.openPwm(PWM_NAME);        } catch (IOException e) {            Log.w(TAG, "Unable to access PWM", e);        }    }    @Override    protected void onDestroy() {        super.onDestroy();        if (mPwm != null) {            try {                mPwm.close();                mPwm = null;            } catch (IOException e) {                Log.w(TAG, "Unable to close PWM", e);            }        }    }}

## Configuring and controlling the PWM signal

* * *

After making a connection, configure the timing parameters for the PWM signal. You must set these parameters before activating the signal the first time. To activate the PWM signal, call `setEnabled(true)`. If you need to temporarily de-activate the signal, you can call `setEnabled(false)`.

The following example configures the PWM to cycle at 120Hz (period of 8.33ms) with a duty of 25% (on-time of 2.08ms every cycle):

    public void initializePwm(Pwm pwm) throws IOException {    pwm.setPwmFrequencyHz(120);    pwm.setPwmDutyCycle(25);    // Enable the PWM signal    pwm.setEnabled(true);}

