# Prepare Raspberry Pi

> If the Raspberry Pi is already set up, this step can be skipped.

> SSH should be enabled.

---

## Option 1: Use Raspberry Pi OS [Original].

The Raspberry Pi requires an operating system (OS) which is placed on a micro SD card, which is then inserted into the Raspberry Pi. When preparing the SD card, some practical basic settings can be made for later.

1. [Download Raspberry Pi Imager](https://www.raspberrypi.com/software/)

2. Connect micro SD card to the computer

3. Start Raspberry Pi Imager

4. `Select OS` -> `Raspberry Pi OS (other)` -> `Raspberry Pi OS Lite (32-bit)`

5. Select SD card

6. Click on the gear (bottom right).

     Activate and fill in `Hostname` (the Raspberry Pi will later appear in the network under the hostname, this saves having to look for the IP address later)

     `Enable SSH`

     `Set username and password` (you can use these details to log into the Raspberry Pi system later)

     Wifi can be set up, but connecting via LAN is more recommended!

     Press 'Save'

    <!--![Imager](https://user-images.githubusercontent.com/108631209/177061261-761e8192-d44e-4b84-abd1-bf8082eaf8d2.png)-->
    <figure><img src="https://user-images.githubusercontent.com/108631209/177061261-761e8192-d44e-4b84-abd1-bf8082eaf8d2.png" alt="" width="400" /><!--<figcaption></figcaption>--></figure>

7. Select SD card and press “Write”.

8. After the writing process is complete, the SD card can be removed from the computer and inserted into the Raspberry Pi

9. Now connect the power and internet supply to the Raspberry Pi and wait about 2 minutes

---

## Option 2: Use Raspiblitz

If you wish to learn or support other Bitcoin technologies, the Raspiblitz image is suitable for installation on the Raspberry Pi. Raspiblitz is a Raspberry Pi Os image specially adapted for the operation of a Bitcoin full node, a Lightning Node and various other tools from the Bitcoin ecosystem.

### Installing Raspiblitz on a Raspberry Pi (4GB or 8GB recommended)

The following hardware and software were used:

* Raspberry Pi 4 4GB
* 1TB Samsung T5 SSD
* Customized raspberry pi 4 aloy housing for better heat dissipation.
* Raspiblitz 1.8.0 ([https://github.com/rootzoll/raspiblitz](https://github.com/rootzoll/raspiblitz))

There are already detailed instructions for the installation on Github ([https://github.com/rootzoll/raspiblitz](https://github.com/rootzoll/raspiblitz)). In case of problems, the Raspiblitz Telegram group can also help: [https://t.me/raspiblitz\_DE.](https://t.me/raspiblitz\_DE)

> :bulb: That's it. Now the Raspberry Pi is ready. All steps based on Raspiblitz are explained below. The steps for Raspberry Pi OS should be similar.

---

####  [Requirements](/requirements.md)  ᐊ  previous | next  ᐅ  [Installation of cgminer on the RaspiBlitz](cgminer_on_raspiblitz.md)
