# ☀ Multiple USB-Miner

Running multiple miners on one USB hub overwhelms ALL hubs I have tried, except for the special hardware from Gekkoscience mentioned below. This is probably due to the fact that USB is usually not specified for currents greater than 1A and therefore all hubs behave according to the standard. Of course this does not help us with a power hungry device like the Gekkoscience Compac F.

Standard	| Voltage	| max. Current	| max. Power
--------- | --------- | ----------------- | -------------
USB 1.0/1.1	| 5 V	| 0,1 A	| 0,5 W
USB 2.0	| 5 V	| 0,5 A	| 2,5 W
USB 3.0/3.1 (Gen1)	| 5 V	| 0,9 A	| 4,5 W
USB 3.1 (Gen2)	| 5 V	| 3 A	| 15 W
USB-BC 	| 5 V	| 1,5 A	| 7,5 W
USB-PD	| 5 V, 12 V, 20 V	| 5 A	| 100 W

With a standard hub I could run 2 USB miners, but at a meager 270 MHz (or 150GH/s (I don't remember exactly)).

Therefore, the urgent recommendation for a similar setup for multiple USB miners:
> * Gekkoscience USB-Hub
> * External ATX power supply (I use a BeQuiet Straightpower with 480W)
> * USB3.1 extensions (again, only the USB3 extensions worked, the one USB2 extension I used could not get the miner to work. The reason for this is unclear to me). 

The Gekkoscience USB hub requires an external power supply, which can be purchased in the original, but the hub also offers a connector for a PCIe-6 connector as found on any ATX power supply.

<img src=".assets/GekkoHub.jpg" alt="Gekkoscience USB-Hub" width="400" />

The power supply only works if the mainborad connector is bridged, otherwise the power supply does not start.

<img src=".assets/ATXNetzteil.jpg" alt="ATX Netzteil" width="400" />

In my case it is sufficient to bridge `Pin 16` (`PS_ON`) and `18` (`Mass`):

<img src=".assets/ATXStecker.jpg" alt="Stecker des ATX-Netzteils" width="400" />

Standard pin assignment of a 24-pin ATX connector:

<img src=".assets/ATXPinBelegung.png" alt="ATX Pinbelegung" width="200" />

---

As an alternative to the jumper above, you can also order a switch for the 20- or 24-pin connection:

<img src=".assets/ATX Schalter.png" alt="ATX Schalter" width="400" />

---

#### [⛏ Miner Einstellungen MHz/ mV](miner-settings.md)  ᐊ  previous | next  ᐅ  [🌩 Übertakten](/uebertakten.md)
