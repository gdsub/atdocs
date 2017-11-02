# I2S


The [Inter-IC Sound](https://en.wikipedia.org/wiki/I%C2%B2S) (IIS or I<sup>2</sup>S) bus connects digital audio devices and transfers [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) audio data between them. It is commonly used to connect microphones and audio amplifiers to a host control system.

I<sup>2</sup>S is a **_synchronous_** serial interface, which means it relies on shared clock signals to synchronize data transfer between devices. An I<sup>2</sup>S master device drives communication and timing through the primary **bit clock (BCLK)** signal, similar to [I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html) or [SPI](https://developer.android.google.cn/things/sdk/pio/spi.html). I<sup>2</sup>S also includes a separate **left-right clock (LRCLK)** signal, which the master uses to select the proper audio channel for data (left or right).

![""](https://developer.android.google.cn/things/images/i2s-connections.png)

<aside class="note">**Note:** <span>LRCLK is also often called the **Frame Select (FS)** or **Word Select (WS)** signal.</span></aside>

I<sup>2</sup>S devices contain at least one **Serial Data (SD)** signal. This is common with peripherals, which generally only produce or consume audio data. Master devices are more likely to have dedicated lines for receive (**SDIN**) and transmit (**SDOUT**) to support **_full-duplex_** communication. This allows you to connect to multiple peripherals to send and receive audio data simultaneously.

<aside class="note">**Note:** <span>Multiple peripherals sharing the same **BCLK** and **LRCLK** lines must use the same audio encoding and sample rate parameters.</span></aside>

## Managing the device connection

* * *

In order to open a connection to a particular I<sup>2</sup>S device, you need to know the unique name of the bus. During the initial stages of development, or when porting an app to new hardware, it's helpful to discover all the available device names from `PeripheralManagerService` using `getI2sDeviceList()`:

    PeripheralManagerService manager = new PeripheralManagerService();List<String> deviceList = manager.getI2sDeviceList();if (deviceList.isEmpty()) {    Log.i(TAG, "No I2S bus available on this device.");} else {    Log.i(TAG, "List of available devices: " + deviceList);}

Once you know the target device name, use `PeripheralManagerService` to connect to that device and supply the audio parameters of the PCM data. You can provide these as discrete parameters, or as an [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html). When you are done communicating with the peripheral device, close the connection to free up resources. Additionally, you cannot open a new connection to the device until the existing connection is closed. To close the connection, use the device's `close()` method.

    public class HomeActivity extends Activity {    // I2S Device Name    private static final String I2S_DEVICE_NAME = ...;    private static final AudioFormat AUDIO_FORMAT_STEREO =            new AudioFormat.Builder()            .setChannelMask(AudioFormat.CHANNEL_IN_STEREO)            .setEncoding(AudioFormat.ENCODING_PCM_16BIT)            .setSampleRate(44100)            .build();    private I2sDevice mDevice;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Attempt to access the I2C device        try {            PeripheralManagerService manager = new PeripheralManagerService();            mDevice = manager.openI2sDevice(I2S_DEVICE_NAME, AUDIO_FORMAT_STEREO);        } catch (IOException e) {            Log.w(TAG, "Unable to access I2S device", e);        }    }    @Override    protected void onDestroy() {        super.onDestroy();        if (mDevice != null) {            try {                mDevice.close();                mDevice = null;            } catch (IOException e) {                Log.w(TAG, "Unable to close I2S device", e);            }        }    }}

## Choosing an audio format

* * *

When creating an I<sup>2</sup>S connection, the supplied audio parameters define the transfer characteristics of the bus, such as the clock frequency. Consider the following example format for 2-channel audio data, 16-bit samples, captured at 44.1kHz:

    private static final AudioFormat AUDIO_FORMAT_STEREO =        new AudioFormat.Builder()        .setChannelMask(AudioFormat.CHANNEL_IN_STEREO) // 2 channels        .setEncoding(AudioFormat.ENCODING_PCM_16BIT)   // 16-bit samples        .setSampleRate(44100)                          // 44.1kHz        .build();

In addition to describing the expected audio data, this tells the I<sup>2</sup>S driver the required frequency of the **BCLK** signal. The frequency is derived from the following formula:

**Frequency = SampleRate x Encoding x ChannelCount**

Per the formula, the above example format requires an I<sup>2</sup>S bit clock frequency of **_1.41MHz_**. Similar to the requirements on [UART](https://developer.android.google.cn/things/sdk/pio/uart.html) baud rate, the timing of this signal must be very accurate to avoid transmission errors caused by [jitter](https://en.wikipedia.org/wiki/Jitter). Ensure that your hardware platform can accurately generate the required bit clock frequency for your chosen format parameters.

To learn more about audio frames, encoding, and sampling, see the [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html) reference.

## Transferring audio samples

* * *

The data transfer methods of `I2sDevice` mirror the format of the [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html) and [AudioRecord](https://developer.android.google.cn/reference/android/media/AudioRecord.html) APIs. This allows your app to integrate with custom audio peripherals the same way it would with the built-in microphone or speaker. Each method accepts data as a `byte[]`, `short[]`, or `ByteBuffer`. Choose the version that matches the [encoding](https://developer.android.google.cn/reference/android/media/AudioFormat.html#encoding) used for your audio data.

The following example reads data from an I<sup>2</sup>S input device (such as a microphone) and echoes that data out through the default audio route:

    public int echoAudio(String i2sName, AudioFormat format) throws IOException {    // Set up audio input source    PeripheralManagerService manager = new PeripheralManagerService();    I2sDevice source = manager.openI2sDevice(i2sName, format);    // Set up the audio playback sink    int bufferSize = AudioTrack.getMinBufferSize(            format.getSampleRate(),            format.getChannelMask(),            format.getEncoding());    AudioTrack sink = new AudioTrack.Builder()            .setAudioFormat(format)            .setTransferMode(AudioTrack.MODE_STATIC)            .setBufferSizeInBytes(bufferSize)            .build();    // Transfer data from input to output    ByteBuffer buffer = ByteBuffer.allocate(bufferSize);    int read = source.read(buffer, bufferSize);    sink.write(buffer, read);    return read;}

