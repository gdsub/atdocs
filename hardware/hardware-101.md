

*   [Hardware](https://developer.android.google.cn/things/hardware/index.html)

# Hardware 101



## In this document

1.  [Breadboards](#breadboards)
2.  [Power Supply](#power_supply)
3.  [Analog and digital I/O](#analog_and_digital_io)
4.  [Pull-ups and pull-downs](#pull-ups_and_pull-downs)
5.  [Signal debounce](#signal_debounce)
6.  [Protecting I/O pins](#protecting_io_pins)


This page provides an overview of the basic electronics concepts you should be familiar with to safely connect peripherals to devices and begin writing apps that bring them to life.

You should have a basic understanding of the relationships between voltage, current, resistance, and power before moving on, so you might find [DC Circuit Theory](http://www.electronics-tutorials.ws/dccircuits/dcp_1.html) and [Ohms Law and Power](http://www.electronics-tutorials.ws/dccircuits/dcp_2.html) helpful if you need more detail on these topics first.

## Breadboards

* * *

[Breadboards](https://en.wikipedia.org/wiki/Breadboard) are a common tool for quickly prototyping circuits without needing to solder components together. This allows you to make wiring changes during development until the design is stable. Breadboards can also be useful in testing, by allowing you to connect instrumentation easily and probe various connections in the circuit.

![""](https://developer.android.google.cn/things/images/breadboard-connections.png)

The holes of a breadboard are internally connected together in rows and columns to allow multiple components to share the same connection point. The outer rows are connected perpendicular to the rest of the board and form a single bus in each row across the top and bottom. These rows are typically used to connect power and ground, or other common signals needed across the entire circuit.

## Power supply

* * *

Embedded devices contain active circuitry, which means they need an external power supply to function. Power is the input voltage delivered to the components on the board from an external source such as a wall adapter, battery, or USB port. The following signals are provided to the board by the power supply:

<dl>

<dt>V<sub>IN</sub></dt>

<dd>Voltage of the external source connected to the board. Many boards support a range of input voltages, and use an internal _voltage regulator_ to provide stable power to the rest of the components.</dd>

<dt>V<sub>CC</sub> or V<sub>DD</sub></dt>

<dd>Internal regulated voltage powering the components on the board. Common power supply voltages are +5V, +3.3V, and +1.8V.</dd>

<dt>Ground (GND)</dt>

<dd>Reference point for 0 volts on the board. All other voltages are measured with respect to ground. Voltages measured below ground are considered negative.</dd>

</dl>

To shut down a board, it is safe to disconnect the power supply. No special shut down procedures are necessary.

## Analog and digital I/O

* * *

Peripherals connect to your device using various input and output pins exposed on the board. Input pins allow your app to read and interpret the current electrical state. Output pins allow your app to control the electrical state of the pins. Peripherals and onboard I/O are either analog or digital in nature.

### Analog

Analog devices produce voltage that's proportional to the physical conditions they measure. A good example is a temperature sensor, which may produce an output between 0-5V corresponding to a temperature between 0-100°C.

Analog inputs translate a discrete voltage level into a proportional integer value using an [analog-to-digital converter](https://en.wikipedia.org/wiki/Analog-to-digital_converter) (ADC). The range of integers used to express the voltage level is based on the _resolution_ of the ADC, expressed in bits. A 10-bit ADC, for example, can express the input voltage as a value between 0-1023 (for example, 2<sup>10</sup> discrete steps).

### Digital

Digital logic represents a voltage signal as a binary value:

*   **High**: When the voltage is at or near V<sub>CC</sub>. Typically represented as a logical "1".
*   **Low**: When the voltage is at or near ground. Typically represented as a logical "0".

It's rare for a digital signal to be exactly 0V or V<sub>CC</sub>. Most digital logic devices interpret a range of voltages near the extremes as a valid logic level. The following table indicates common input voltage ranges for each logic state.

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

Peripherals typically use digital I/O in a few common ways:

*   **Stable state**: Single on/off state mapped to a stable high or low value.

    ![""](https://developer.android.google.cn/things/images/digital-1.png)

*   **Pulse train**: Series of digital signal pulses with variable frequency and width transmitted continuously over time.

    ![""](https://developer.android.google.cn/things/images/digital-2.png)

*   **Serial communication**: Series of digital 1s and 0s representing individual bits of a binary number.

    ![""](https://developer.android.google.cn/things/images/digital-3.png)

For more information on analog and digital I/O, see [Sensors and Transducers](http://www.electronics-tutorials.ws/io/io_1.html) and [Binary Numbers](http://www.electronics-tutorials.ws/binary/bin_1.html).

## Pull-ups and pull-downs

* * *

![](https://developer.android.google.cn/things/images/hardware-inputs.png)

In many digital interface circuits, resistors are connected between the I/O signal pins and V<sub>CC</sub> or ground. These are known as pull-up and pull-down resistors, respectively. They guarantee that each signal has a stable default state that the rest of the system can rely on, without significantly affecting the input or output signal directly.

A digital input that is not actively connected to any signal is a _floating input_. Floating inputs are susceptible to [electromagnetic interference](https://en.wikipedia.org/wiki/Electromagnetic_interference), which affects the value reported to your app and causes unpredictable readings. Pull-up or pull-down resistors ensure that the line is driven to a stable value, even when nothing else is connected.

As an example, think of a button or switch. A switch is a pair of contacts that connects an input pin to a high or low voltage when closed, but leaves the input floating when open. In addition, many digital transducers use [open collector](https://en.wikipedia.org/wiki/Open_collector) (or open drain) outputs to report a state change. These outputs act like a simple switch and require and external source to drive the input when the switch is open.

### Resistor strength

The resistor values you choose affect the system in different ways. Low value resistors are considered "strong" because more current flows. Strong pull-ups (or pull-downs) draw more power overall, but they can reset the signal to an idle level more quickly than a "weak" resistor with a higher value.

<aside class="note">**Note:** <span>Pull-up and pull-down resistor values are typically between 1kΩ and 10kΩ.</span></aside>

As an example, the I<sup>2</sup>C serial bus uses pull-up resistors to keep the clock and data lines stable when the bus is idle. Each device added to the bus loads down these lines, making it harder for the pull-up to keep the line at the appropriate level. As the number of devices on the bus increases, the strength of the pull-ups must also increase to handle the added load.

See [Pull-up Resistors](http://www.electronics-tutorials.ws/logic/pull-up-resistor.html) for more details on applications and calculating the proper values.

## Signal debounce

* * *

![](https://developer.android.google.cn/things/images/hardware-debounce.png)

Many electrical input devices, such as switches and relays, have a mechanical component. As the mechanical motion of the device settles, the electrical signal can temporarily oscillate — or "bounce" — between multiple values. In many cases, this will be seen by your app as multiple input events in a very short time.

To correct this problem, you must _debounce_ the signal using hardware or software. Software debounce involves setting a time delay between the initial input event and when the input is expected to stabilize (usually not more than a few hundred milliseconds).

To debounce your input with hardware, add a simple RC circuit (so-named because it contains a resistor and capacitor) between the input pin and the device. When the input device changes state, the capacitor will charge and discharge at a rate proportional to the size of the input resistor, effectively slowing down the transitions seen by the input pin.

See [Input Interfacing Circuits](http://www.electronics-tutorials.ws/io/input-interfacing-circuits.html) for more information on calculating debounce and other techniques for connecting input signals to your device.

## Protecting I/O pins

* * *

![](https://developer.android.google.cn/things/images/hardware-outputs.png)

Each output pin has a limited capability to source (when high) or sink (when low) current from the circuitry connected to it. Peripherals that draw more current than the pin can handle — even temporarily — can damage the output. To protect the pin, insert a current-limiting resistor in series with the load.

<aside class="note">**Note:** <span>Series resistor values are typically between 100Ω and 300Ω.</span></aside>

To control higher power transducers, such as motors, buffer the load from the output pin using a transistor or similar electronically controlled switch and power the transducer directly from the power supply.

<aside class="note">**Note:** <span>The source/sink capacity of I/O pins will vary by device. Check your hardware documentation to better understand what your board can support.</span></aside>

All I/O pins are designed to safely operate with in the voltage range between 0V and V<sub>CC</sub>. Connecting any pin to a voltage higher than the power supply for that component will likely damage it. Always verify that the voltage levels generated by your sensors and transducers match their connected I/O pins. To connect devices of variable supplies together, use a [logic level converter](https://www.sparkfun.com/products/12009) circuit.

See [Output Interfacing Circuits](http://www.electronics-tutorials.ws/io/output-interfacing-circuits.html) for more examples of circuits you can use to safely interface with digital and analog I/O.

