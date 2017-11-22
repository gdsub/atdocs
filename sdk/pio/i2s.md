# I2S

# I2S

The [Inter-IC Sound](https://en.wikipedia.org/wiki/I%C2%B2S) (IIS or I<sup>2</sup>S) bus connects digital audio devices and transfers [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) audio data between them. It is commonly used to connect microphones and audio amplifiers to a host control system.

[Inter-IC Sound](https://en.wikipedia.org/wiki/I%C2%B2S) (IIS or I<sup>2</sup>S) 总线连接数字音频设备与 [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) 并且在两者之间交互音频数据。通常用于主机控制系统与麦克风或者扬声器之间的连接。

I<sup>2</sup>S is a **_synchronous_** serial interface, which means it relies on shared clock signals to synchronize data transfer between devices. An I<sup>2</sup>S master device drives communication and timing through the primary **bit clock (BCLK)** signal, similar to [I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html) or [SPI](https://developer.android.google.cn/things/sdk/pio/spi.html). I<sup>2</sup>S also includes a separate **left-right clock (LRCLK)** signal, which the master uses to select the proper audio channel for data (left or right).

I<sup>2</sup>S 是 **_同步_** 串行接口，意味着它要通过共享的时钟信号在设备之间同步传输数据。与 [I2C](https://developer.android.google.cn/things/sdk/pio/i2c.html) 或者是 [SPI](https://developer.android.google.cn/things/sdk/pio/spi.html) 类似， I<sup>2</sup>S 主设备驱动通过 **bit clock (BCLK) 位时钟** 信号校准时钟。 I<sup>2</sup>S 也包括了一个独立的 **left-right clock (LRCLK) 左右时钟** 信号，控制器用这个时钟来选择数据声道(左声道或者右声道)。

![""](https://developer.android.google.cn/things/images/i2s-connections.png)


**Note:**  LRCLK is also often called the **Frame Select (FS)** or **Word Select (WS)** signal.

**Note:** LRCLK 也通常称作 **Frame Select (FS)帧选择** 或者是 **Word Select (WS)字选择** 信号。 

I<sup>2</sup>S devices contain at least one **Serial Data (SD)** signal. This is common with peripherals, which generally only produce or consume audio data. Master devices are more likely to have dedicated lines for receive (**SDIN**) and transmit (**SDOUT**) to support **_full-duplex_** communication. This allows you to connect to multiple peripherals to send and receive audio data simultaneously.

 I<sup>2</sup>S 设备至少包括一个 **Serial Data (SD)串行数据** 信号。这是一个通用的外设信号，一般用来输入或者是输出音频数据。主设备一般可以选择 接收 (**SDIN**) 或者是发送 (**SDOUT**) 通道来支持 **全双工** 通信。 这种通信方式可以让你连接多个外设，并且同时接收或者发送数据。

<aside class="note">**Note:** <span>Multiple peripherals sharing the same **BCLK** and **LRCLK** lines must use the same audio encoding and sample rate parameters.</span></aside>

<aside class="note">**Note:** <span>多种外设如果需要共享 **BCLK** 和 **LRCLK** 信号，就必须用相同的音频编码和采样频率参数 </span></aside>

## Managing the device connection

## 管理设备连接

* * *

In order to open a connection to a particular I<sup>2</sup>S device, you need to know the unique name of the bus. During the initial stages of development, or when porting an app to new hardware, it's helpful to discover all the available device names from `PeripheralManagerService` using `getI2sDeviceList()`:

如果要打开一个特定的 I<sup>2</sup>S 设备， 就需要知道总线的唯一名称。在开发的最初阶段，或者当移植一款应用到新的硬件的时候。最有效的办法是使用 `getI2sDeviceList()` 方法从 `PeripheralManagerService` 中来发现所有的设备：

~~~java
    PeripheralManagerService manager = new PeripheralManagerService();
	List<String> deviceList = manager.getI2sDeviceList();
	if (deviceList.isEmpty()) {    
		Log.i(TAG, "No I2S bus available on this device.");
	} else {    
		Log.i(TAG, "List of available devices: " + deviceList);
	}
~~~

Once you know the target device name, use `PeripheralManagerService` to connect to that device and supply the audio parameters of the PCM data. You can provide these as discrete parameters, or as an [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html). When you are done communicating with the peripheral device, close the connection to free up resources. Additionally, you cannot open a new connection to the device until the existing connection is closed. To close the connection, use the device's `close()` method.

一旦你知道了目标设备的名称，就可以用 `PeripheralManagerService` 连接设备并且设定 PCM 的数据的音频参数。 可以依据 [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html) 或一个 [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html) 来提供音频离散参数。 当你已经完成外设的数据传输时，记行关闭连接来释放资源。除此之外，除非现有连接已经关闭，否则请不要打开一个新的设备连接。要想关闭连接，可以使用设备中的 `close()` 方法。

~~~java
    public class HomeActivity extends Activity {    
		// I2S Device Name    
		private static final String I2S_DEVICE_NAME = ...;    
		private static final AudioFormat AUDIO_FORMAT_STEREO =   new AudioFormat.Builder()            
				.setChannelMask(AudioFormat.CHANNEL_IN_STEREO)            
				.setEncoding(AudioFormat.ENCODING_PCM_16BIT)            
				.setSampleRate(44100)            
				.build();    
		private I2sDevice mDevice;    

		@Override protected void onCreate(Bundle savedInstanceState) {        
				super.onCreate(savedInstanceState);        
				// Attempt to access the I2C device        
				try {            
						PeripheralManagerService manager = new PeripheralManagerService();            
						mDevice = manager.openI2sDevice(I2S_DEVICE_NAME, AUDIO_FORMAT_STEREO);        
				} catch (IOException e) {            
						Log.w(TAG, "Unable to access I2S device", e);        
				}    
		}    

		@Override    protected void onDestroy() {        
				super.onDestroy();        
				if (mDevice != null) {            
					try {                
						mDevice.close();                
						mDevice = null;            
					} catch (IOException e) {                
						Log.w(TAG, "Unable to close I2S device", e);            
					}        
				}    
		}
	}
~~~

## Choosing an audio format

## 选择音频格式

* * *

When creating an I<sup>2</sup>S connection, the supplied audio parameters define the transfer characteristics of the bus, such as the clock frequency. Consider the following example format for 2-channel audio data, 16-bit samples, captured at 44.1kHz:

当我们创建 I<sup>2</sup>S 连接时，我们提供的音频参数决定了总线的传输特性，比如时钟频率。下面的例子展示了双通道的音频数据， 16位的采样，以及 44.1kHz 的采样率格式的编码过程：

~~~java
    private static final AudioFormat AUDIO_FORMAT_STEREO =   new AudioFormat.Builder()        
				.setChannelMask(AudioFormat.CHANNEL_IN_STEREO) // 2 channels        
				.setEncoding(AudioFormat.ENCODING_PCM_16BIT)   // 16-bit samples        
				.setSampleRate(44100)                          // 44.1kHz        
				.build();
~~~

In addition to describing the expected audio data, this tells the I<sup>2</sup>S driver the required frequency of the **BCLK** signal. The frequency is derived from the following formula:

要获取到我们所希望的音频数据，I<sup>2</sup>S 驱动就需要知道 **BCLK** 信号的频率。 这个频率可以由以下公式得到：

**Frequency = SampleRate x Encoding x ChannelCount**

**频率 = 采样率 x 采样长度 x 声道数**

Per the formula, the above example format requires an I<sup>2</sup>S bit clock frequency of **_1.41MHz_**. Similar to the requirements on [UART](https://developer.android.google.cn/things/sdk/pio/uart.html) baud rate, the timing of this signal must be very accurate to avoid transmission errors caused by [jitter](https://en.wikipedia.org/wiki/Jitter). Ensure that your hardware platform can accurately generate the required bit clock frequency for your chosen format parameters.

上面的例子需要 I<sup>2</sup>S 的位时钟频率为 **_1.41MHz_**。 和串口 [UART](https://developer.android.google.cn/things/sdk/pio/uart.html) 的波特率类似。 信号时钟必须十分精确，完全避免由 [抖动](https://en.wikipedia.org/wiki/Jitter) 产生的传输错误。 需要确认硬件是否能够精确产生你选择的参数所需要的位时钟。

To learn more about audio frames, encoding, and sampling, see the [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html) reference.

需要获更多的关于音频帧， 编码， 以及采样相关的内容，请参考 [AudioFormat](https://developer.android.google.cn/reference/android/media/AudioFormat.html)。

## Transferring audio samples


## 传输音频样本

* * *

The data transfer methods of `I2sDevice` mirror the format of the [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html) and [AudioRecord](https://developer.android.google.cn/reference/android/media/AudioRecord.html) APIs. This allows your app to integrate with custom audio peripherals the same way it would with the built-in microphone or speaker. Each method accepts data as a `byte[]`, `short[]`, or `ByteBuffer`. Choose the version that matches the [encoding](https://developer.android.google.cn/reference/android/media/AudioFormat.html#encoding) used for your audio data.

`I2sDevice` 中的数据传输方法跟 [AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html) 和[AudioRecord](https://developer.android.google.cn/reference/android/media/AudioRecord.html) 文档描述一致。 它能使你的应用把自定义的音频外设和内置的麦克风或者是扬声器整合在一起。音频数据会以 `byte[]`, `short[]`, 或者 `ByteBuffer`的形式来存放。在 [编码](https://developer.android.google.cn/reference/android/media/AudioFormat.html#encoding) 中选择正确的音频数据存放方式。

The following example reads data from an I<sup>2</sup>S input device (such as a microphone) and echoes that data out through the default audio route:

下面的例子完成了通过 I<sup>2</sup>S 输入设备(比如麦克风)读取音频数据，然后再通过默认的音频输出设备来播放音频的过程。

~~~java
    public int echoAudio(String i2sName, AudioFormat format) throws IOException {    
			// Set up audio input source    
			PeripheralManagerService manager = new PeripheralManagerService();    
			I2sDevice source = manager.openI2sDevice(i2sName, format);    

			// Set up the audio playback sink    
			int bufferSize = AudioTrack.getMinBufferSize(format.getSampleRate(), 
					format.getChannelMask(),  format.getEncoding());    
			AudioTrack sink = new AudioTrack.Builder()   
				.setAudioFormat(format)
				.setTransferMode(AudioTrack.MODE_STATIC)
				.setBufferSizeInBytes(bufferSize)
				.build();
			// Transfer data from input to output    
			ByteBuffer buffer = ByteBuffer.allocate(bufferSize);
			int read = source.read(buffer, bufferSize); 
			sink.write(buffer, read); 
			return read;
	}
~~~
