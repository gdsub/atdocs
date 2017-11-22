


*   [Hardware](https://developer.android.google.cn/things/hardware/index.html)

*   [硬件](https://developer.android.google.cn/things/hardware/index.html)

# Hardware 101

# 硬件 101 



## In this document

## 内容

1.  [Breadboards](#breadboards)

      [面包板](#breadboards)
    
2.  [Power Supply](#power_supply)
      
       [电源](#power_supply)
     
3.  [Analog and digital I/O](#analog_and_digital_io)

    [模拟和数字 I/O](#analog_and_digital_io)
4.  [Pull-ups and pull-downs](#pull-ups_and_pull-downs)

    [上拉 和 下拉 ](#pull-ups_and_pull-downs)
5.  [Signal debounce](#signal_debounce)

    [信号抗扰](#signal_debounce)
6.  [Protecting I/O pins](#protecting_io_pins)

    [保护 I/O pin脚](#protecting_io_pins)

This page provides an overview of the basic electronics concepts you should be familiar with to safely connect peripherals to devices and begin writing apps that bring them to life.

本页面综述了所需要熟悉的基本电子概念，以便安全地连接外设设备并开始写实际的应用程序。

You should have a basic understanding of the relationships between voltage, current, resistance, and power before moving on, so you might find [DC Circuit Theory](http://www.electronics-tutorials.ws/dccircuits/dcp_1.html) and [Ohms Law and Power](http://www.electronics-tutorials.ws/dccircuits/dcp_2.html) helpful if you need more detail on these topics first.


在进一步了解之前，需要对电压，电流，电阻和电源之间的关系有一个基本的理解，所以如果需要这些方面的更多细节，请参阅 [直流电路理论](http://www.electronics-tutorials.ws/dccircuits/dcp_1.html) 和 [欧姆定律和电源](http://www.electronics-tutorials.ws/dccircuits/dcp_2.html) 会有帮助。


## Breadboards

## 面包板

* * *

[Breadboards](https://en.wikipedia.org/wiki/Breadboard) are a common tool for quickly prototyping circuits without needing to solder components together. This allows you to make wiring changes during development until the design is stable. Breadboards can also be useful in testing, by allowing you to connect instrumentation easily and probe various connections in the circuit.

[面包板](https://en.wikipedia.org/wiki/Breadboard) 是常用的用来快速做电路原型的工具，这样可以不用把原件焊接到一起。用面包板可以让你开发当中做线路改变直到电路设计稳定下来。

![""](https://developer.android.google.cn/things/images/breadboard-connections.png)

The holes of a breadboard are internally connected together in rows and columns to allow multiple components to share the same connection point. The outer rows are connected perpendicular to the rest of the board and form a single bus in each row across the top and bottom. These rows are typically used to connect power and ground, or other common signals needed across the entire circuit.

面包板的孔内部行列连到一起这样多个元件可以共用一个连接点。 最外边的行直连到整个板并在每一行形成一个单一的贯通总线。 这些行通常用来连接电源和地或者其他需要通过整个电路的信号。

## Power supply

##  电源

* * *

Embedded devices contain active circuitry, which means they need an external power supply to function. Power is the input voltage delivered to the components on the board from an external source such as a wall adapter, battery, or USB port. The following signals are provided to the board by the power supply:

嵌入式设备包含活跃的电路, 这意味着它们需要电源来工作。电是从外部源如插座，电池或者USB口传过来到板上元件的输入电压。下面的信号由电源提供支持:
<dl>

<dt>V<sub>IN</sub></dt>

<dd>Voltage of the external source connected to the board. Many boards support a range of input voltages, and use an internal _voltage regulator_ to provide stable power to the rest of the components.</dd>

<dd>连到板子的外部电压。很多板子支持一个范围内的输入电压, 并使用一个内部的 _稳压源_ 给其他的元件提供稳定的电压。</dd>

<dt>V<sub>CC</sub> or V<sub>DD</sub></dt>

<dt>V<sub>CC</sub> 或者 V<sub>DD</sub></dt>


<dd>Internal regulated voltage powering the components on the board. Common power supply voltages are +5V, +3.3V, and +1.8V.</dd>

<dd>内部稳压给板子上的元件供电。 通常的供电电压有 +5V, +3.3V, and +1.8V.</dd>

<dt>Ground (GND)</dt>

<dt>地 (GND)</dt>


<dd>Reference point for 0 volts on the board. All other voltages are measured with respect to ground. Voltages measured below ground are considered negative.</dd>

<dd>板子上的0伏参考点。所有其他电压参考地来测量。低于地的电压视为负电压。</dd>
</dl>

To shut down a board, it is safe to disconnect the power supply. No special shut down procedures are necessary.

为了安全地关掉板子，断开电源就可以。不需要其他的特殊的步骤。
## Analog and digital I/O

## 模拟和数字 I/O

* * *

Peripherals connect to your device using various input and output pins exposed on the board. Input pins allow your app to read and interpret the current electrical state. Output pins allow your app to control the electrical state of the pins. Peripherals and onboard I/O are either analog or digital in nature.

外设使用板子上裸露的不同的输入和输出PIN脚与板子相连。 输入PIN脚允许应用读取和解释当前的状态。 输出脚允许应用来控制PIN脚状态。 外设和板上 I/O 本质上都是或者模拟或者数字的.
### Analog

### 模拟

Analog devices produce voltage that's proportional to the physical conditions they measure. A good example is a temperature sensor, which may produce an output between 0-5V corresponding to a temperature between 0-100°C.

模拟设备按照它们测量的物理条件来输出对应的电压。温度传感器是一个好例子，能按照相应的 0-100°C 产生一个在 0-5V 之间的输出电压


Analog inputs translate a discrete voltage level into a proportional integer value using an [analog-to-digital converter](https://en.wikipedia.org/wiki/Analog-to-digital_converter) (ADC). The range of integers used to express the voltage level is based on the _resolution_ of the ADC, expressed in bits. A 10-bit ADC, for example, can express the input voltage as a value between 0-1023 (for example, 2<sup>10</sup> discrete steps).

模拟输入使用[模数转换器](https://en.wikipedia.org/wiki/Analog-to-digital_converter) (ADC) 把离散的电压值转换成一个成比例的整数值。用来表示电压的整数范围基于按位表示的 ADC 的分辨率。例如，一个10位 ADC可以表示在 0-1023之间的值(例如, 2<sup>10</sup> 离散值).

### Digital

### 数字

Digital logic represents a voltage signal as a binary value:

数字逻辑用二进制值来代表一个电压信号

*   **High**: When the voltage is at or near V<sub>CC</sub>. Typically represented as a logical "1".

*   **High**: 当电压在或接近 V<sub>CC</sub>。 通常代表逻辑 "1"。

*   **Low**: When the voltage is at or near ground. Typically represented as a logical "0".

*   **Low**: 当电压在或接近地。 通常代表逻辑"0"。

It's rare for a digital signal to be exactly 0V or V<sub>CC</sub>. Most digital logic devices interpret a range of voltages near the extremes as a valid logic level. The following table indicates common input voltage ranges for each logic state.

对一个数字信号来讲是很少会正好是  0V 或者 V<sub>CC</sub>。大多数逻辑设备把一定范围的接近的值作为有效的逻辑值。下面的表格表明每个逻辑值的通常的输入电压范围。

<table>

<tbody>

<tr>

<th>Supply Voltage (V<sub>CC</sub>)</th>

<th>Logic Low (0)</th>

<th>Logic High (1)</th>

</tr>

<tr>

<td>5V ([TTL](https://en.wikipedia.org/wiki/Transistor%E2%80%93transistor_logic))</td>

<td>< 0.8V</td>

<td>> 2.0V</td>

</tr>

<tr>

<td>3.3V ([CMOS](https://en.wikipedia.org/wiki/CMOS))</td>

<td>< 0.8V</td>

<td>> 2.0V</td>

</tr>

<tr>

<td>1.8V ([CMOS](https://en.wikipedia.org/wiki/CMOS))</td>

<td>< 0.6V</td>

<td>> 1.2V</td>

</tr>

</tbody>

</table>

<table>

<tbody>

<tr>

<th> 供电电压 (V<sub>CC</sub>)</th>

<th>Logic Low (0)</th>

<th>Logic High (1)</th>

</tr>

<tr>

<td>5V ([TTL](https://en.wikipedia.org/wiki/Transistor%E2%80%93transistor_logic))</td>

<td>< 0.8V</td>

<td>> 2.0V</td>

</tr>

<tr>

<td>3.3V ([CMOS](https://en.wikipedia.org/wiki/CMOS))</td>

<td>< 0.8V</td>

<td>> 2.0V</td>

</tr>

<tr>

<td>1.8V ([CMOS](https://en.wikipedia.org/wiki/CMOS))</td>

<td>< 0.6V</td>

<td>> 1.2V</td>

</tr>

</tbody>

</table>
Peripherals typically use digital I/O in a few common ways:

外设通常用几种普遍方式在使用数字 I/O 

*   **Stable state**: Single on/off state mapped to a stable high or low value.

*   **稳定状态**: 单一的开/关状态 变到一个稳定的高或低。
    ![""](https://developer.android.google.cn/things/images/digital-1.png)

*   **Pulse train**: Series of digital signal pulses with variable frequency and width transmitted continuously over time.

*   **脉冲序列**: 一串带可变频率和带宽的持续数字脉冲。
    ![""](https://developer.android.google.cn/things/images/digital-2.png)

*   **Serial communication**: Series of digital 1s and 0s representing individual bits of a binary number.

*   **串行通信**: 一系列的0和1代表二进制数的独立位。
    ![""](https://developer.android.google.cn/things/images/digital-3.png)

For more information on analog and digital I/O, see [Sensors and Transducers](http://www.electronics-tutorials.ws/io/io_1.html) and [Binary Numbers](http://www.electronics-tutorials.ws/binary/bin_1.html).

要获得模拟和数字 I/O 的更多信息, 可以看[各种传感器](http://www.electronics-tutorials.ws/io/io_1.html)和[二进制数](http://www.electronics-tutorials.ws/binary/bin_1.html).
## Pull-ups and pull-downs

## 上拉 和 下拉

* * *

![](https://developer.android.google.cn/things/images/hardware-inputs.png)

In many digital interface circuits, resistors are connected between the I/O signal pins and V<sub>CC</sub> or ground. These are known as pull-up and pull-down resistors, respectively. They guarantee that each signal has a stable default state that the rest of the system can rely on, without significantly affecting the input or output signal directly.

在许多数字接口电路里，电阻连在 I/O 信号脚 和 V<sub>CC</sub> 或者地之间。 这就是所谓的上拉或下拉电阻。它们可以保证每个信号可以有个系统里其他部分可以依赖的稳定的缺省状态，同时不会明显的影响输入输出信号。

A digital input that is not actively connected to any signal is a _floating input_. Floating inputs are susceptible to [electromagnetic interference](https://en.wikipedia.org/wiki/Electromagnetic_interference), which affects the value reported to your app and causes unpredictable readings. Pull-up or pull-down resistors ensure that the line is driven to a stable value, even when nothing else is connected.

没有连到任何信号的数字输入是漂浮输入。漂浮输入容易遭受 [电磁干扰](https://en.wikipedia.org/wiki/Electromagnetic_interference), 电磁干扰会影响给应用的值使得应用读取的值不可预测。即使什么也不连，上拉和下拉电阻也可以确保电路拉到一个稳定值。

As an example, think of a button or switch. A switch is a pair of contacts that connects an input pin to a high or low voltage when closed, but leaves the input floating when open. In addition, many digital transducers use [open collector](https://en.wikipedia.org/wiki/Open_collector) (or open drain) outputs to report a state change. These outputs act like a simple switch and require and external source to drive the input when the switch is open.

作为一个例子, 考虑一个按键或开关. 开关是一对继电器，当关的时候连接一个输入PIN脚到一个高电压或低电压上，当开的时候输入处于漂浮态。 另外, 许多 数字传感器使用[开集](https://en.wikipedia.org/wiki/Open_collector) (或开漏) 输出来报告状态改变。 这些输入就如同一个简单开关，当开关打开时需要外部资源去 驱动输入。

### Resistor strength

### 电阻强度

The resistor values you choose affect the system in different ways. Low value resistors are considered "strong" because more current flows. Strong pull-ups (or pull-downs) draw more power overall, but they can reset the signal to an idle level more quickly than a "weak" resistor with a higher value.

选择的电阻值会以不同的方式影响系统。 因为有更大的电流，低阻值电阻被看作“强”。总的来说强上拉（或下拉）吸引更多电，但它们比一个“弱”高阻值的电阻能更快地复位信号到没有

<aside class="note">**Note:** <span>Pull-up and pull-down resistor values are typically between 1kΩ and 10kΩ.</span></aside>

<aside class="note">**Note:** <span>上拉和下拉电阻阻值一般在 1kΩ 和 10kΩ 之间</span></aside>

As an example, the I<sup>2</sup>C serial bus uses pull-up resistors to keep the clock and data lines stable when the bus is idle. Each device added to the bus loads down these lines, making it harder for the pull-up to keep the line at the appropriate level. As the number of devices on the bus increases, the strength of the pull-ups must also increase to handle the added load.

作为一个例子, I<sup>2</sup>C 串行总线在总线空闲时使用上拉电阻来保持时钟和数据线稳定。 每一个加到总线的设备给这些线很重负但并使得上拉难以保持该线在合适电平。随着在总线上面的设备增加，上拉的力量必须加强以应对增加的负载。

See [Pull-up Resistors](http://www.electronics-tutorials.ws/logic/pull-up-resistor.html) for more details on applications and calculating the proper values.

看 [上拉电阻](http://www.electronics-tutorials.ws/logic/pull-up-resistor.html) 可以看到更多应用的细节并计算合适的值。
## Signal debounce

## 信号去抖

* * *

![](https://developer.android.google.cn/things/images/hardware-debounce.png)

Many electrical input devices, such as switches and relays, have a mechanical component. As the mechanical motion of the device settles, the electrical signal can temporarily oscillate — or "bounce" — between multiple values. In many cases, this will be seen by your app as multiple input events in a very short time.

许多电子输入设备, 如开关和 继电器, 有机械元件. 随着该设备机械移动停下来, 电信号会在多个值之间临时震荡 — or "跳跃"。在很多情况中，应用会认为在很短时间里发生多个输入事件。

To correct this problem, you must _debounce_ the signal using hardware or software. Software debounce involves setting a time delay between the initial input event and when the input is expected to stabilize (usually not more than a few hundred milliseconds).

为了修正这个问题, 必须用硬件或软件来_去抖_ 。 软件去抖涉及在初始的输入事件和输入稳定之间设定一个延迟时间(通常不超过几百微秒)。

To debounce your input with hardware, add a simple RC circuit (so-named because it contains a resistor and capacitor) between the input pin and the device. When the input device changes state, the capacitor will charge and discharge at a rate proportional to the size of the input resistor, effectively slowing down the transitions seen by the input pin.

为了用硬件去抖, 在输入PIN脚和设备之间增加一个简单的 RC电路 (这样叫是因为它包含一个电阻和电容)。 当输入设备改变状态, 电容会以正比于输入阻抗的速率充电和放电, effectively可以有效的减缓输入PIN脚的状态改变。

See [Input Interfacing Circuits](http://www.electronics-tutorials.ws/io/input-interfacing-circuits.html) for more information on calculating debounce and other techniques for connecting input signals to your device.

可以看 [输入接口电路](http://www.electronics-tutorials.ws/io/input-interfacing-circuits.html) 来了解更多在计算去抖和其他连输入信号到设备上的技术的相关信息。

## Protecting I/O pins

## I/O引脚 保护

* * *

![](https://developer.android.google.cn/things/images/hardware-outputs.png)

Each output pin has a limited capability to source (when high) or sink (when low) current from the circuitry connected to it. Peripherals that draw more current than the pin can handle — even temporarily — can damage the output. To protect the pin, insert a current-limiting resistor in series with the load.

每一个输出PIN脚有一个有限的能力从相连电路去拉电流（为高）或灌电流(为低)。 外设能吸引比PIN脚能承受的更多的电流 — 即使是短暂的 — 也能损坏输出。为了保护PIN脚，插入一个限流带负载的串阻。

<aside class="note">**Note:** <span>Series resistor values are typically between 100Ω and 300Ω.</span></aside>

<aside class="note">**Note:** <span>串阻值通常在 100Ω 和 300Ω 之间。</span></aside>

To control higher power transducers, such as motors, buffer the load from the output pin using a transistor or similar electronically controlled switch and power the transducer directly from the power supply.

为了控制高能变换器, 例如马达, 用一个晶体管做负载缓冲或简单的电子控制开关并从直接对变换器供电。

<aside class="note">**Note:** <span>The source/sink capacity of I/O pins will vary by device. Check your hardware documentation to better understand what your board can support.</span></aside>

<aside class="note">**Note:** <span> I/O PIN脚的拉/灌能力因设备而不同。 看你的硬件文档来更好的理解什么是你的板子能支持的。</span></aside>

All I/O pins are designed to safely operate with in the voltage range between 0V and V<sub>CC</sub>. Connecting any pin to a voltage higher than the power supply for that component will likely damage it. Always verify that the voltage levels generated by your sensors and transducers match their connected I/O pins. To connect devices of variable supplies together, use a [logic level converter](https://www.sparkfun.com/products/12009) circuit.

所有的 I/O PIN脚设计成能在电压范围在 0V 和 V<sub>CC</sub>之间安全工作。连任何PIN到一个高于元件供电电压很可能会损害到该PIN脚。总是验证你的传感器和变能器产生的电压能匹配跟它们连的 I/O PIN脚。为了连不同供电的设备到一起, 用[电平转换器](https://www.sparkfun.com/products/12009) 电路。

See [Output Interfacing Circuits](http://www.electronics-tutorials.ws/io/output-interfacing-circuits.html) for more examples of circuits you can use to safely interface with digital and analog I/O.

看 [输出接口电路](http://www.electronics-tutorials.ws/io/output-interfacing-circuits.html) 来找到更多能用来安全的同数字和模拟 I/O 接口的电路。

