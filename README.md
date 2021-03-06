# Project: Touch-Piano-Recording-Reply
Hello! this project will be extremely excited for electronics makers because we will be designing our own touch-capacitive Piano with Arduino Nano. We will include a recording and replay feature on our Piano. So far, we've made a few piano projects using Arduino, but this one is quite different because we are going to use capacitive touch keys for our piano keys. So along with learning how to build a fun to play piano we will also explore how to design capacitive touch keys on PCB, as you can we have tried to make our keys looks as close an actual piano key. The PCB looks and works like a piano, thanks to its fabricator PCBWay, we will also explore how we designed and fabricated this board but before that, let's explore capacitive touch sensors and how it works.

![Arduino-based-Piano](https://user-images.githubusercontent.com/34489444/133959384-0069761b-2b17-4b9f-99ea-2e8b53c0417f.gif)

### How a Capacitive Touch Sensor Works?

We know that in order to form a capacitor with some capacitance value, we need two parallel conductive plates separated by a dielectric material. But how can we tell if the capacitance has changed simply by touching the conductive plate with our fingers? Our answer is based on our fundamental understanding of the capacitor. As we know, changing the area of the conductive plate or the distance between two parallel conductive plates can change the capacitance value. Between the conductive plate and the finger, we have air as a dielectric medium. As a result, when we touched the plate with our fingers, there was an uncertain increase in capacitance because our finger was acting as a conductive object and the distance between the two conductive objects was reduced. We know the basic formula of the capacitance of a parallel plate capacitor is,
```diff
C = ϵA/d
```
Where "A" is representing the area of the conducting plates, "d" is representing the distance between the two conductive plates and the "ϵ" is representing the permittivity of the AIR. As a result, increasing the area and decreasing the distance between the two parallel conductive plates increases the capacitance value. In our case, touching the conductive plate reduces the distance while increasing the capacitance value. Can we detect this changing capacitance by connecting a conductive material to a resistor and a microcontroller's GPIO pin? The answer is, we can't. Yes, connecting a voltage source to it will cause a small change in the analog voltage, but this is not a very robust solution.

### How to Detect the Capacitance Change in a Capacitive Touch Sensor?

So, how could we tell if the capacitance value had changed? There is, however, a better way to go about it. Let's take a look at the block diagram below. Consider this to be a basic circuit consisting of a Microcontroller (in this case, an Arduino Nano), a 1 Mega Ohm resistor, and a Conductive Plate. The Arduino Nano's two digital lines are connected to a resistor loop with a 1 megaohm resistor. This resistor is also connected to the conductive plate at one point. Although this plate is acting as a single point of the capacitor, it can still introduce capacitance that changes when we touch it. This, however, cannot be detected simply by detecting a change in voltage. The change in capacitance on this line is not as easy as sensing the toggle of the on or off the value on the GPIO pin. The change in capacitance on this line is not as simple as sensing the toggle of the GPIO pin's value on or off.

### How the Capacitive Sensor Library Works?

This is where the Arduino libraries come in handy. Thank you very much to Paul Bagder and Paul Stoffregen, the authors of the “CapacitiveSensor” library. When we touch that conductive plate, we can detect a change in capacitance using this library. One of the digital pins is used as a send pin (as OUTPUT), while the other is used as a receive pin (as INPUT) in this library. The duration between when the send pin goes high and when the receive pin goes high is the sole way to detect a change in capacitance. When you set the send pin to high (or 5 volts), the resistor-capacitor pair produces a delay between when the send pin becomes high and when the receive pin reads the high value from the send pin. The CapacitiveSensor library provides a function that sets the send pin to HIGH, then waits and counts until the receive pin is read as HIGH. This function returns a time value that can be used to detect capacitance changes. When the time value increases or decreases, it indicates that the capacitance value has changed. The receive pin will take longer to reach high when there is more capacitance, and it will take less time to go high when there is less capacitance. As a result, we can determine what the normal state is and then check for changes each time they send pin toggles.

We are going to use the “CapacitveSensor” library to detect the change in the capacitance. But before going into the programing part let’s create the circuit and PCB for our project. Here we are using the EasyEDA platform to create the Schematic and the PCB for our project. To detect the change in capacitance, we'll use the “CapacitveSensor” library. Let's start with the circuit and PCB for our project before we go into the programming. The Schematic and PCB for our project were created using the EasyEDA platform. On the EasyEDA platform, we've created a large number of PCB projects. Those projects can be used to obtain a concept of how to design a PCB on EasyEDA.

### Circuit Diagram for the PCB Piano Using Arduino Nano

The eight 1Mega Ohm Resistors are connected to the Arduino Nano's digital pin number 2 in the following circuit diagram. The digital pins 3 to 10 were further connected to the other connecting points of each resistor. On the diagram below, we have a slide switch labelled "RECODINGSWITCH". The Arduino Nano's digital pin 12 is connected to the slide switch's "EN" pin. The slide switch's "Vs" pin is connected to the Arduino Nano's "5V" pin. The sliding switch's "GND" pin is connected to the Arduino Nano's "Ground Pin". The BUZZER's Positive pin is connected to the Arduino Nano's "A4" pin. And the negative of the BUZZER is connected to the Ground pin of the Arduino Nano.

![Arduino-based-Piano-Circuit](https://user-images.githubusercontent.com/34489444/133959824-07047020-e0bf-4110-add5-e6fff2a73da2.png)

We've connected eight 10uF capacitors to each of the resistors. And the negative pin of each capacitor is connected to the Arduino Nano's ground pin. Then we have a Power section that provides a proper 6.6V to the Arduino Nano's "Vin" pin. The 18650 battery cell is linked to the 18650 battery charger module, and the charger module's output is linked to the DC to DC Voltage booster. The voltage booster's positive output pin (BOUT+) is connected to the Arduino Nano's "Vin" pin, and the voltage booster's negative output pin (BOUT-) is connected to the Arduino Nano's ground pin.

Note: If necessary, we can add capacitors. Small capacitors (20pF - 400pF) are highly recommended for stabilizing detected data. However, ensure that the capacitors are grounded, as this reduces the parallel to the body resistance. However, in my case, I did not use the capacitors because it works fine for me without them. I mentioned the capacitors in the schematic above so you could easily add them during the practical implementation. The following capacitors' values must be between 20pF and 400pF, as specified in the “CapacitveSensor” library's documentation.

### The Overview of the PCB

The PCB view of the above schematic is representing in the following picture below. You can download the Gerber File of the project from our GitHub Repo. Or, you can visit the project on EasyEDA platform for more details. The yellow color is for the Top-Silk Layer. Whereas the Green color is representing the bottom silk Layer. The Red Color is for the Top- Layer and the Blue color is representing the Bottom-Layer.


![Touch-Capacitive-Piano-PCB](https://user-images.githubusercontent.com/34489444/133959889-608b085b-15af-4c37-9a52-c65bb3647393.jpg)

### The Top-Layer of the PCB:

Now, let’s see every layer of the PCB one by one. The top-layer is shown in the image below. As you can see the Top-Layer is Red in color. I have designed each conductive plate in such a way that it looks like a piano. Every key of the piano is connected to each of the 1 megaohm resistors respectively.

![Arduino-based-Piano-PCB (1)](https://user-images.githubusercontent.com/34489444/133960257-5970f8c6-1f93-4081-a969-ce5169e23b4b.jpg)


I used the Rectangular shape that can be found in the PCB Tool section on the EasyEDA that is circled in red color in the image below. Make sure that the width of the keys is large enough so that you can touch each of the keys with your finger. In my case, I managed to draw every key having 10mm or greater than 10mm width.

![Easy-EDA-Tools (1)](https://user-images.githubusercontent.com/34489444/133960313-664b523f-35ec-4d84-b08a-a6a5566d3c30.jpg)

 
### The Bottom-Layer of the PCB:

At the bottom layer we have a complete copper layer that is used to connect all the grounds. You can use the “Solid Region” option in the “PCB Tools” on EasyEDA. This is called the “Copper Pouring” method. This step will convert the bottom layer as a common grounded layer. We have some other copper connections at this layer.

![PCB-Bottom-Layer (1)](https://user-images.githubusercontent.com/34489444/133960358-8ade89b5-8553-479b-b86d-6d0b34e6d310.jpg)


### The Top-Silk-Layer of the PCB:

The following image below is representing the Top-Silk-Layer of the PCB. We can design our PCBs by adding some silk layers or Non-copper layers. I marked the slide switch as “RECORD” and “PLAY”. So that we can understand which mode we are using. We have the footprint of the BUZZER at the top silk layer. The “XL6009E1” is the footprint of the DC to DC voltage booster module surrounded by the square area. We can add text and images by using the respective PCB Tools which are available on the EasyEDA.

![Arduino-Piano-PCB](https://user-images.githubusercontent.com/34489444/133960408-f7703877-3090-4f4a-87c7-8e8e69df9ce3.jpg)


### The Bottom-Silk-Layer of the PCB:

At the bottom silk layer of the PCB we havethe footprint of the Arduino Nano, eight 1Mega ohm resistors, eight capacitors, one 18650 batter holder or single cell and the charging module.

![PCB-Design_0](https://user-images.githubusercontent.com/34489444/133960440-f90601b9-0082-4895-9ea0-a2648b19c875.jpg)


