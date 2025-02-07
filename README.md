### Cave phones

### Introduction
Reliable communication underground, especially in caves with the magnitude we are working within, is vital for the safety of the cavers and the organization of the expedition.
Reliable communications are taken for granted in everyday life. The luxury of being able to communicate with anyone at any time usually does not exist in speleology. Phone service, if not absent at the location of the cave, is lost in the first few meters, and the vhf - uhf radios stop working as soon as line of sight is obstructed. The solution to reliable communication is usually a wired phone system. 
The standard in caving for many years was using military surplus field phones (https://fieldphone.blogspot.com/) with a few designs that have appeared over the years of custom wired telephones. In the earlier two expeditions a few modified door intercom devices were used with success, but they had limitations, and the system failed eventually. 
 
### Prototypes
As soon as last year's expedition ended and with the failures in communications, we wondered how we could improve the system. The first ideas conceptualized used a widely available cheap audio amp board, and it seemed easy enough of a project to try and build some prototypes. The first ideas were using the audio amp to amplify the voice of the user and transmit it over the wire while the other devices sat passively with their speakers connected to the line. The amplifier only amplified the microphone, and the system depended on having enough power to drive the speakers over the long distance (which only worked for a few devices and short distances). The second iteration was using an amplifier with two inputs, amplifying the microphone at transmission and amplifying the received signal on reception. This worked great and is probably the simplest design of wired phones someone could make. The final design was based on the phones of Gleb Kucherovskiy (https://docs.google.com/document/d/1EriDpcy0b-GYJtkANHnrFxnLsx1-ZfM0Cw-MAJZAQfc/edit?tab=t.0#heading=h.7yxopwcpe6mo) which is a revision of the original design of Anisov S.P., Krasnoyarsk (http://igorkov.org/downloads/tele-anisov.docx) with design improvements of Igor Kovalenko (http://igorkov.org/tele). The design is brilliant and is probably the best starting point if you want to make wired phones.
 



### Circuit
The design we ended up using was different. It followed a modular design logic, having a few boards that are connected together. A lot of the design principles were used from the previous circuits. The design has four distinct parts. The preamp, the amp, the input circuitry (with the 1:1 transformer) and the mode switching circuitry (with the electromagnetic relay).  

![Image](https://github.com/user-attachments/assets/6114f83f-8e02-4e07-a2af-cca05f8568bf)

> Figure 1 Phone circuit

The preamp solves a crucial problem with the previous designs, which is the audio quality. The audio quality used to depend on the type of speaker (which was used as an input) but now with the preamplifier we have perfect audio independently. The preamp is also connected in a way that is only powered when we transmit, saving battery.
 The amplifier board is using an op amp (XPT8871) with the components required to run it in open loop configuration, so we just need to add the negative feedback resistor to set the gain and an input resistor for the audio preamp output (So we divide the output voltage of the preamp to match the input limits of the amp). This IC is similar to the IC used by Gleb Kucherovski (MC34119), but it is not ideal. We really do not need output power (given that the line losses are basically voltage drop in a few kilometers of wire, and the output voltage of our amplifier is only dependent on the supply voltage). The ideal would be a modern amplifier with the lowest quiescent supply current possible. Amplifiers like the PAM8403 with an output driver are not suitable due to the time they need to calibrate after each PTT press, resulting in a noticeable delay. 
The 1:1 audio transformer is used to isolate the amp input from the transmission line, it manages the differential signal of the line input and gives us the possibility to directly wire the amp negative output to the speaker negative and one line input. With this wiring we can use one switch of the relay to direct the amp positive output to either the line or the speaker, and the other switch of the relay to either the microphone preamp or the secondary of the transformer with the line input signal.
The last pieces are the input circuitry consisting of an input resistor, a capacitor in parallel and diodes for voltage spikes, status LEDs, the call circuit, and the battery protection – charging circuit.
With this design we have basically emulated the Gleb Kucherovskiy phones with two readily available boards and minimal external components. This means that you do not need a hot-air soldering station to assemble the circuit and if anything breaks you can very easily swap a board to fix it. 
  

![Image](https://github.com/user-attachments/assets/29fc9a77-d44b-4e19-9cad-c2bf11962eaf) ![Image](https://github.com/user-attachments/assets/ab61b726-1a31-4722-b10f-eba0ad66fd59)

> Figure 2 The custom PCB that was used in the phones


### Phone body

![Image](https://github.com/user-attachments/assets/1f43b6e9-03db-4c5d-8d29-465b81425365)
> Figure 3 Phone without back plate

The body of the phones was a bit trickier to get right, we had a few requirements that made the construction a bit difficult.
We opted using 18650 cells instead of alkaline batteries, the 18650 cells have some advantages over alkaline or NiMh cells mainly the increased energy density but also the 18650 cell is the most popular battery for caving headlamps and almost everyone is caring some. The idea is if a phone runs out of battery someone could easily use his batteries for the phone, so a requirement was that the user could easily change the battery without compromising the water resistance of the phone.
The solution was using a case with a snap-on back-plate and 3d printed part that the PCB is bolted on and sealed inside the phone, with enough room next to it for the battery holder to fit. The tolerances were tight, and everything had to be positioned precisely to fit.
That leads to another problem, speaker selection. The original design used a thin 8Ω 0.5W speaker that had extremely poor performance, it could not reproduce low frequencies leading to bad legibility. I tried a lot of speakers, and the best performing were larger diameter 4Ω 3W speakers that were a bit too thick to fit inside the phone.
One solution at the time was mounting the speaker externally and maybe printing a cover for it to protect it. But after a lot of searching, I managed to find some replacement speakers, that had a very slim profile and exceptional performance and were used in most of the phones. 
Everything else was similar to the Russian designs, two banana plugs at the top, two push buttons and an on-off switch.
For the waterproofing the switches and banana plugs were sealed with glue, the speaker was mounted with double sided tape and a thin wire film was placed covering the microphone hole. The speaker and microphone holes need to be as small as possible so that the surface tension of the water droplets is high enough to prevent them from entering inside. Also, a rubber strip is holding the backplate firmly in place just in case. 

![Image](https://github.com/user-attachments/assets/f1e8bf1a-7977-420e-a920-395de57be804)
  
> Figure 4 3D printed frame

Due to the replaceable batteries, there was an issue with the possibility of inserting the battery in reverse. Inserting the battery in reverse destroys the xpt8871 amp. There are a few solutions to this problem, a diode in series could work, but drops the battery voltage and we cannot afford that, a fuse and a diode in parallel could be a solution to protect the circuit, but in the event of the reverse polarity someone needs to open the phone to repair it (so you might as well have nothing and replace the amp). The best solution is a MOSFET and a diode configuration, that does not drop the voltage and does not catastrophically fail. The problem with this is that you need a lot of components that are expensive, but this is the best solution. 
Another solution, after some research, was to use some li-po BMS boards. Usually these are soldered on the li-po cell and provide short-circuit and under-voltage protection, if the cell is connected in reverse to the BMS board it kind of acts as a reverse polarity protection by passing only a few mA of current, and the important thing is that it does not fail catastrophically. The battery protection just needs to be reset by connecting and disconnecting the battery a few times. The problem is that it really wants to have the cell permanently connected to the BMS, and not connecting and disconnecting it, so sometimes when the battery is installed, or replaced by a new one, it does not want to cooperate. Given the fact that every solution was bad I did not use any. 
 

![Image](https://github.com/user-attachments/assets/c244e0f6-8ca3-4785-91b1-c1ef1a85688c)

> Figure 5 Connection diagram of the phones
