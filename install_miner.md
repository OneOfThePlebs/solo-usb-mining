# Install mining software

> :bulb: The Raspberry Pi's power and internet must be connected.
>
> :bulb: Connect USB miner or switch on USB hub.
>
> ⚠️ The path information does not have to apply to your system. Before installing, it is advisable to briefly look at 'relative' and 'absolute' paths/path information in Linux (see also [💡 Helpful commands for easier operation under Linux/Raspberry Pi](LinuxCommands.md)).

In order to be able to access the Raspberry Pi from your own computer via SSH, the computer must be on the same network. If this is the case, open the terminal. To do this, simply search for the “Terminal” program on your PC. It then starts in the terminal:

Access the Raspberry Pi via SSH:

```console
ssh solominer@miner.local
```

solominer is the user name, miner is the host name (alternatively use the IP address of the Raspberry Pi as the host name)
Enter yes and press ENTER -> Enter password and press ENTER
(the terminal does not provide any feedback when entering the password)

Perform updates:

```console
sudo apt-get update 
sudo apt-get upgrade -y
```

Install packages:

```console
sudo apt-get install -y build-essential git autoconf automake libtool pkg-config libcurl4-openssl-dev libudev-dev libusb-1.0-0-dev libncurses5-dev
```

Now the CGMiner branch can be cloned from GIT:

```console
mkdir -p mining; cd mining 
git clone https://github.com/kanoi/cgminer.git
cd cgminer
```

After successful cloning from GIT, the CGMiner can now be compiled:

```console
./autogen.sh
CFLAGS="-O2 -Wall -march=native -fcommon" ./configure --enable-gekko
sudo make
```

(At this point, restart the Raspberry Pi with `sudo reboot` or, for a Raspiblitz installation, `sudo restart` if the next step does not work)

Now the progress can be tested as follows:

```console
sudo ./cgminer -n
```

The name of the miner should then appear in the answer. Here's an example:

```console
…
Manufacteur: 'GekkoScience'
Product: 'NewPac Bitcoin Miner'
…
```

> :bulb: Now everything is installed and mining can begin!

---

#### [Installation of cgminer on the RaspiBlitz](cgminer_on_raspiblitz.md)  ᐊ  previous | next  ᐅ  [Start Mining Software](start_mining.md)
