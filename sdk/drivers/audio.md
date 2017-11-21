# Audio

# 音频

The Android audio subsystem enables apps to write audio data for playback and record audio from input sources. Using audio drivers, your apps can register new audio input/output devices connected over [I2S](https://developer.android.google.cn/things/sdk/pio/i2s.html) or other [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) interfaces. Apps can access your audio devices using the standard media framework APIs, such as [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html) and [AudioRecord](https://developer.android.google.cn/reference/android/media/AudioRecord.html).

Android 音频子系统可以让应用能够写入音频数据用来播放，或者从输入源中录制音频。使用音频音频驱动，你的应用可以注册已经连接到 [I2S](https://developer.android.google.cn/things/sdk/pio/i2s.html) 或其它 [Peripheral I/O](https://developer.android.google.cn/things/sdk/pio/index.html) 接口上的新音频输入/输出设备。其它应用可以通过标准的媒体框架 API，例如 [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html) 和 [AudioRecord](https://developer.android.google.cn/reference/android/media/AudioRecord.html) 来接入你的音频设备。

## Implementing the driver

## 实现驱动

* * *

To define a new audio device, extend the `AudioInputDriver` or `AudioOutputDriver` class and implement the appropriate `read()` or `write()` method. The media framework calls these methods as needed to request recorded audio or supply data for playback.

为了定义一个新的音频设备，继承  `AudioInputDriver` 或 `AudioOutputDriver` 类并实现相应的  `read()` 或 `write()` 方法。媒体框架在需要录制音频或提供音频播放时会调用这些方法。

Your driver should implement the `onStandbyChanged()` method if the audio hardware is able to react to Android going into and out of standby mode. This might include terminating an audio stream in progress or putting the hardware into a low power state.

如果你的音频硬件可以响应 Android 的进入/离开待机模式，你的驱动应当实现 `onStandbyChanged()` 方法。这可能会涉及终止正在使用的音频流，或将硬件置于低功耗状态。

The following example implements an `AudioOutputDriver` that sends audio from the framework to an `I2sDevice` for playback:

下面的样例实现了一个 `AudioOutputDriver`，它将从 framework 发送音频数据到一个  `I2s设备` 去播放：
``` java
public class PlaybackDriver extends AudioOutputDriver {

    private I2sDevice mOutputDevice;

    public PlaybackDriver(I2sDevice output) {
        mOutputDevice = output;
    }

    @Override
    public void onStandbyChanged(boolean inStandby) {
        ...
    }

    @Override
    public int write(ByteBuffer buffer, int count) {
        try {
            return mOutputDevice.write(buffer, count);
        } catch (IOException e) {
            Log.w(TAG, "Unable to write audio buffer", e);
            return -1;
        }
    }
}
```

## Registering a new device

## 注册一个新设备

* * *

Bind your audio device to the framework by registering it with the `UserDriverManager`. Supply the following parameters along with the device registration:

通过使用 `UserDriverManager` 注册，从而将你的音频设备绑定到 framework 中。在设备注册过程中，提供以下参数：

1.  **Audio Format** - Audio data parameters. Including sample rate, encoding, and channel count. Provided as an [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html).
2.  **Device Type** - Either [TYPE_BUILTIN_MIC](https://developer.android.google.cn/reference/android/media/AudioDeviceInfo.html#TYPE_BUILTIN_MIC) or [TYPE_BUILTIN_SPEAKER](https://developer.android.google.cn/reference/android/media/AudioDeviceInfo.html#TYPE_BUILTIN_SPEAKER).
3.  **Buffer Size** - Maximum amount of data the system can exchange with your audio device in one transaction. The buffer size must be an even multiple of 16 audio frames, as defined by the provided format.

* * *
1.  **音频格式** - 音频数据参数。包括采样率，编码方式，以及通道数，以 [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html) 的方式提供。
2.  **设备类型** - [TYPE_BUILTIN_MIC](https://developer.android.google.cn/reference/android/media/AudioDeviceInfo.html#TYPE_BUILTIN_MIC) 或者 [TYPE_BUILTIN_SPEAKER](https://developer.android.google.cn/reference/android/media/AudioDeviceInfo.html#TYPE_BUILTIN_SPEAKER).
3.  **缓存大小** - 在一次事务中，系统与你的音频设备可交换数据的最大值。缓存大小必须是 16 个音频帧的偶数倍，由提供的格式定义。

To detach the device from the framework, unregister the driver.

从 framework 中解绑设备，需要注销驱动。
``` java
public class AudioDriverService extends Service {

    private static final AudioFormat AUDIO_FORMAT_STEREO =
            new AudioFormat.Builder()
            .setChannelMask(AudioFormat.CHANNEL_IN_STEREO) // 2 channels
            .setEncoding(AudioFormat.ENCODING_PCM_16BIT)   // 16-bit samples
            .setSampleRate(44100)                          // 44.1kHz
            .build();
    private static final int BUFFER_SIZE = 1024; // 256 PCM16 frames

    private PlaybackDriver mPlaybackDriver;

    @Override
    public void onCreate() {
        super.onCreate();

        mPlaybackDriver = new PlaybackDriver();
        UserDriverManager manager = UserDriverManager.getManager();

        // Register the new driver with the framework
        manager.registerAudioOutputDriver(mPlaybackDriver,
                AUDIO_FORMAT_STEREO,
                AudioDeviceInfo.TYPE_BUILTIN_SPEAKER,
                BUFFER_SIZE);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();

        UserDriverManager manager = UserDriverManager.getManager();
        // Unregister the driver when finished
        manager.unregisterAudioOutputDriver(mPlaybackDriver);
    }
}
```

With the driver properly registered, apps can use the new device for recording and playback through the standard Android media classes, including [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html) and [AudioRecord](https://developer.android.google.cn/reference/android/media/AudioRecord.html).

当驱动正确地被注册后，其它应用可以通过标准的 Android 媒体类操控你的新设备录音或播放音频。这些媒体类包括 [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html) 和 [AudioRecord](https://developer.android.google.cn/reference/android/media/AudioRecord.html)。

The registration process can take some time before the audio device is available to clients. Apps should register an [AudioDeviceCallback](https://developer.android.google.cn/reference/android/media/AudioDeviceCallback.html) to be notified when it is available before starting recording or playback on that device:

在音频设备可用之前，注册过程可能会需要一些时间。在录制和播放音频之前，应用应当注册一个 [AudioDeviceCallback](https://developer.android.google.cn/reference/android/media/AudioDeviceCallback.html) 回调，这样，当音频设备可用时，它将会告知应用：
``` java
public class PlaybackActivity extends Activity {

    private AudioManager mAudioManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ...
        mAudioManager = (AudioManager) getSystemService(Context.AUDIO_SERVICE);
        mAudioManager.registerAudioDeviceCallback(mDeviceCallback, null);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        ...
        mAudioManager.unregisterAudioDeviceCallback(mDeviceCallback);
    }

    // Listen for registration events from the audio driver
    private AudioDeviceCallback mDeviceCallback = new AudioDeviceCallback() {
        @Override
        public void onAudioDevicesAdded(AudioDeviceInfo[] addedDevices) {
            for (AudioDeviceInfo deviceInfo : addedDevices) {
                if (deviceInfo.getType() == AudioDeviceInfo.TYPE_BUILTIN_SPEAKER) {
                    // Audio user driver is now registered
                    startAudioPlayback();
                }
            }
        }

        @Override
        public void onAudioDevicesRemoved(AudioDeviceInfo[] removedDevices) {
            for (AudioDeviceInfo deviceInfo : removedDevices) {
                if (deviceInfo.getType() == AudioDeviceInfo.TYPE_BUILTIN_SPEAKER) {
                    // Audio user driver was unregistered
                    stopAudioPlayback();
                }
            }
        }
    };
}
```

## Adding the required permission

## 添加必需权限

* * *

Add the required permission for the user driver to your app's manifest file:

在你应用的清单文件中添加用户驱动所需的权限：

``` java
<uses-permission android:name="com.google.android.things.permission.MANAGE_AUDIO_DRIVERS" />
```

