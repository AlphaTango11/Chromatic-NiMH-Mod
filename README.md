# Chromatic Ni-MH Charging Mod

![PCB Overview](/images/10mm-Ni-MH-Overview.jpg)

My first attempt to design, assemble, and integrate a real PCB from scratch as someone with very little (no) EE background.

### Purpose

Enables USB-C charging of Ni-MH batteries for the ModRetro Chromatic in a safe, cheap, and easy mod that only requires a bit of soldering.

The first board uses a simple, constant-current BQ25172DSGR configured to charge 3 Ni-MH AA cells at 83 mA for a maximum of 22 hours.

A standard Ni-MH Eneloop AA typically has a capacity of ~1900 mAh. This charge configuration would result in ~1850 mAh.


### Limitations

The charger is ideally used with nearly empty cells. If the cells are just below the recharge threshold of ~1.3 V (approximately 90% SoC), then they will reach "full" within a few hours but continue receiving current.

I am unsure of Eneloop's ability to self-catalyze like older Ni-MH cells. 83 mA is around C/23, which should be very safe even in the worst case, but I do not know of the risks or effects on cell life.

There is an overvoltage cutoff (1.7 V per cell), but the cells never reach that with 83 mA of current. In my charge tests, the cells will peak at around 1.5 V.
    
Alkaline batteries are not detected by this charger - it may try to charge them when plugged in. 
**Do not use the USB-C port with alkaline batteries installed.**

The LED is not really visible from inside the case. I can make an even smaller board without it, or I can add solderable leads to enable running it closer to a visible location.


## Assembly Steps

1. Remove Chromatic motherboard ([iFixit guide steps 1-5](https://www.ifixit.com/Guide/MODRETRO+Chromatic+Disassembly/180018))

2. Choose installation location (recommended: vacant micro SD slot or on top of MCU)
    - Plan wire routing and pay attention to button membrane locations/silkscreens.
    - I tried both the micro SD slot and on top of the MCU, but the LED was not visible. The top of the MCU may let the LED shine through the IR blaster area with more modification.
        - Do not try to put it on top of the ESP32, it won't fit back in the case.
    - I put kapton tape on the LCD to prevent any risk of shorting.

3. Connect wires:
   - 5V from TP16
   - GND from any ground point - I used the ground of a nearby capacitor
   - Battery output to TP7/Pin 3 of battery connector

4. Verify LED operation with batteries and USB-C cable connected
    - If the batteries are less than 1.3V, you should get a steady green LED.
    - If you don't see an LED, it may be because your batteries are too full. Verify all cells are <1.3 V.
    - *If the batteries are connected* and the LED is FLASHING, there is some other fault.
        - The LED will flash with no batteries connected.

5. Secure board as needed (dot of superglue, tape, etc.)

6. Reassemble device

## Pictures

![Schematic](/images/10mm-Schematic.png)
![PCB Design](/images/10mm-PCB.png)
![Render](/images/10mm-Corner.png)
![Before Soldering](/images/10mm-BeforeSolder.jpg)
![After Soldering](/images/10mm-AfterSolder.jpg)

## Components
    
I honestly don't know much about choosing components, but these are what I used. You may be able to do better.
    

10mm x 10mm V1.0

[Digikey Link for All Components (3 boards worth)](https://www.digikey.com/short/9fqp0m98)

[OSH Park Board Link](https://oshpark.com/shared_projects/6lf2HPNr) 

<a href="https://oshpark.com/shared_projects/6lf2HPNr"><img src="https://oshpark.com/assets/badge-5b7ec47045b78aef6eb9d83b3bac6b1920de805e9a0c227658eac6e19a045b9c.png" alt="Order from OSH Park"></img></a>

| Quantity | Component | Description | URL |
| ------------- | ------------- | ------------- | ------------- |
| 1 | BQ25172DSGR | 0.8-A, ONE- TO SIX-CELL NIMH STA | [Link](https://www.digikey.com/en/products/detail/texas-instruments/BQ25172DSGR/16585660) |
| 2 | CL05A106MP6NUN8 | CAP CER 10UF 10V X5R 0402 - INPUT/OUTPUT | [Link](https://www.digikey.com/en/products/detail/samsung-electro-mechanics/CL05A106MP6NUN8/10478957) |
| 3 | ERJ-2RKF3601X | RES SMD 3.6K OHM 1% 1/10W 0402 - STAT, ISET, TMR | [Link](https://www.digikey.com/en/products/detail/panasonic-electronic-components/ERJ-2RKF3601X/1746202) |
| 1 | ERJ-2RKF2402X | RES SMD 24K OHM 1% 1/10W 0402 - VSET | [Link](https://www.digikey.com/en/products/detail/panasonic-electronic-components/ERJ-2RKF2402X/1746163) |
| 1 | ERJ-2RKF1002X | RES SMD 10K OHM 1% 1/10W 0402 - TS | [Link](https://www.digikey.com/en/products/detail/panasonic-electronic-components/ERJ-2RKF1002X/192073) |
| 1 | APHHS1005CGCK | LED GREEN CLEAR CHIP SMD | [Link](https://www.digikey.com/en/products/detail/kingbright/APHHS1005CGCK/1747499) |



<br>

I highly recommend reading the [datasheet](https://www.ti.com/lit/ds/symlink/bq25172.pdf) for the BQ25172DSGR to get a better understanding of its operation.
