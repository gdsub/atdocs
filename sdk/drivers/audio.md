# Audio

The Android audio subsystem enables apps to write audio data for playback and record audio from input sources. Using audio drivers, your apps can register new audio input/output devices connected over [I2S](https://developer.android.google.cn/things/sdk/pio/i2s.html) or other [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) interfaces. Apps can access your audio devices using the standard media framework APIs, such as [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html) and [AudioRecord](https://developer.android.google.cn/reference/android/media/AudioRecord.html).

## Implementing the driver

* * *

To define a new audio device, extend the `AudioInputDriver` or `AudioOutputDriver` class and implement the appropriate `read()` or `write()` method. The media framework calls these methods as needed to request recorded audio or supply data for playback.

Your driver should implement the `onStandbyChanged()` method if the audio hardware is able to react to Android going into and out of standby mode. This might include terminating an audio stream in progress or putting the hardware into a low power state.

The following example implements an `AudioOutputDriver` that sends audio from the framework to an `I2sDevice` for playback:

    public class PlaybackDriver extends AudioOutputDriver {    private I2sDevice mOutputDevice;    public PlaybackDriver(I2sDevice output) {        mOutputDevice = output;    }    @Override    public void onStandbyChanged(boolean inStandby) {        ...    }    @Override    public int write(ByteBuffer buffer, int count) {        try {            return mOutputDevice.write(buffer, count);        } catch (IOException e) {            Log.w(TAG, "Unable to write audio buffer", e);            return -1;        }    }}

## Registering a new device

* * *

Bind your audio device to the framework by registering it with the `UserDriverManager`. Supply the following parameters along with the device registration:

1.  **Audio Format** - Audio data parameters. Including sample rate, encoding, and channel count. Provided as an [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html).
2.  **Device Type** - Either [TYPE_BUILTIN_MIC](https://developer.android.google.cn/reference/android/media/AudioDeviceInfo.html#TYPE_BUILTIN_MIC) or [TYPE_BUILTIN_SPEAKER](https://developer.android.google.cn/reference/android/media/AudioDeviceInfo.html#TYPE_BUILTIN_SPEAKER).
3.  **Buffer Size** - Maximum amount of data the system can exchange with your audio device in one transaction. The buffer size must be an even multiple of 16 audio frames, as defined by the provided format.

To detach the device from the framework, unregister the driver.

    public class AudioDriverService extends Service {    private static final AudioFormat AUDIO_FORMAT_STEREO =            new AudioFormat.Builder()            .setChannelMask(AudioFormat.CHANNEL_IN_STEREO) // 2 channels            .setEncoding(AudioFormat.ENCODING_PCM_16BIT)   // 16-bit samples            .setSampleRate(44100)                          // 44.1kHz            .build();    private static final int BUFFER_SIZE = 1024; // 256 PCM16 frames    private PlaybackDriver mPlaybackDriver;    @Override    public void onCreate() {        super.onCreate();        mPlaybackDriver = new PlaybackDriver();        UserDriverManager manager = UserDriverManager.getManager();        // Register the new driver with the framework        manager.registerAudioOutputDriver(mPlaybackDriver,                AUDIO_FORMAT_STEREO,                AudioDeviceInfo.TYPE_BUILTIN_SPEAKER,                BUFFER_SIZE);    }    @Override    public void onDestroy() {        super.onDestroy();        UserDriverManager manager = UserDriverManager.getManager();        // Unregister the driver when finished        manager.unregisterAudioOutputDriver(mPlaybackDriver);    }}

With the driver properly registered, apps can use the new device for recording and playback through the standard Android media classes, including [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html) and [AudioRecord](https://developer.android.google.cn/reference/android/media/AudioRecord.html).

The registration process can take some time before the audio device is available to clients. Apps should register an [AudioDeviceCallback](https://developer.android.google.cn/reference/android/media/AudioDeviceCallback.html) to be notified when it is available before starting recording or playback on that device:

    public class PlaybackActivity extends Activity {    private AudioManager mAudioManager;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        ...        mAudioManager = (AudioManager) getSystemService(Context.AUDIO_SERVICE);        mAudioManager.registerAudioDeviceCallback(mDeviceCallback, null);    }    @Override    protected void onDestroy() {        super.onDestroy();        ...        mAudioManager.unregisterAudioDeviceCallback(mDeviceCallback);    }    // Listen for registration events from the audio driver    private AudioDeviceCallback mDeviceCallback = new AudioDeviceCallback() {        @Override        public void onAudioDevicesAdded(AudioDeviceInfo[] addedDevices) {            for (AudioDeviceInfo deviceInfo : addedDevices) {                if (deviceInfo.getType() == AudioDeviceInfo.TYPE_BUILTIN_SPEAKER) {                    // Audio user driver is now registered                    startAudioPlayback();                }            }        }        @Override        public void onAudioDevicesRemoved(AudioDeviceInfo[] removedDevices) {            for (AudioDeviceInfo deviceInfo : removedDevices) {                if (deviceInfo.getType() == AudioDeviceInfo.TYPE_BUILTIN_SPEAKER) {                    // Audio user driver was unregistered                    stopAudioPlayback();                }            }        }    };}

## Adding the required permission

* * *

Add the required permission for the user driver to your app's manifest file:

        <uses-permission android:name="com.google.android.things.permission.MANAGE_AUDIO_DRIVERS" />

