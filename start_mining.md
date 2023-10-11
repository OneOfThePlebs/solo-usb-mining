# Mining Software starten

> :memo: **IMPORTANT:** You have to be in the folder of cgminer for the start command to work. If you are not in the folder, e.g. because the Raspberry Pi was restarted, you can get into the folder with the following command:

```console
cd mining/cgminer
```

To start cgminer and thus mining, the following command must be executed:

for NewPac:

```console
sudo ./cgminer --compact --real-quiet -o stratum+tcp://solo.ckpool.org:3333 -u <BTCADRESSE> -p x --suggest-diff 32 --gekko-newpac-freq 100 --gekko-newpac-boost
```

for CompacF:

```console
sudo ./cgminer --compact --real-quiet -o stratum+tcp://solo.ckpool.org:3333 -u <BTCADRESSE> -p x --gekko-compacf-freq 500 --gekko-start-freq 450 --gekko-mine2 --gekko-tune2 60
```

> `<BTCADRESSE>` must be exchanged with own Bitcoin address

> The number behind `--gekko-newpac-freq` or `--gekko-compacf-freq` can be increased to make the USB miner run with a higher clock rate (this increases the hashrate but at the same time also the temperature the miner reaches).

The commands `compact` and `real-quiet` should be omitted at the beginning to see if the miner runs well. You can see in the case of the Compac F-miner for example on which frequency it settles. Depending on the USB hub, different frequencies are not reached.

> :warning: **Again the hint**: Never operate the Compac F without cooling!

Short excerpt of the explanation of the commands:

```
General:

  --compact                   Use compact display without per device statistics
  
  --real-quiet                Disable all output

Newpac:

  --suggest-diff <arg>        Suggest miner difficulty for pool to user (default: none)
 
  --compac-freq <arg>         Set GekkoScience Compac frequency in MHz, range 100-500 (default: 150.0)
  
  --gekko-newpac-boost

Compac F:

  --gekko-compacf-freq <arg>  Set GekkoScience CompacF BM1397 frequency in MHz, range 100-800 (default 200.0)
  
  --gekko-start-freq <arg>    Ramp start frequency MHz 25-500
  
  --gekko-mine2
  
  --gekko-tune2 60
```

> :warning: **WARNING:** When the terminal is closed, the mining process is also terminated!
> To keep mining running in the background, you can use `nohup` or `screen`, for more information see chapter [⛏ Mining Software - Enhanced Configuration](EnhancedConfiguration.md).

<!--
```console
nohup <COMMAND> &
```

Um zu überprüfen, ob der Mining Prozess läuft, kann folgender Befehl ausgeführt werden:

```console
cat nohup.out
```

Es wird ein Standbild von dem Prozess gezeigt.
Der USB-Miner blinkt mit einer weißen LED, wenn das Mining aktiv ist. Dadurch kann unkompliziert und visuell überprüft werden, ob das Mining läuft.

Die aktiven Prozesse des Raspberry Pis können mit diesem Befehl angezeigt werden:

```console
top
```

Um die Prozess Übersicht zu beenden einfach die `Q`-Taste drücken.

Damit der cgminer beendet werden kann muss folgender Befehl ausgeführt werden:

```console
sudo kill 1234
```

Anstelle von `1234` muss die Prozess-Nummer vom cgminer eingefügt werden (Diese steht links in der Prozess Übersicht).

Wenn man den Befehl für den cgminer im Hintergrund mehrmals gestartet hat, läuft der cgminer mehrfach im Hintergrund. Es ist dann zu empfehlen die Prozesse zu beenden damit nur einer aktiv ist.
-->
---

## Retrieve statistics:

### For ckpool

https://solo.ckpool.org/
page and under `Statistics` enter its BTC address that was used in the command. All relevant data are then stored there.

### For Kano-Pool

Call worker address (without login, ideal for solo-mining-pools): https://kano.is/index.php?k=work&username=<USERNAME>&api=<API-KEY> where `<USERNAME>` corresponds to the logged in user at Knao and `<API-KEY>` must be replaced by the key stored in the user interface. Alternatively log in to kano.is (for solo miners).
  
### Mine with us and compare data easily here http://solomining.info:8080.
  
> :bulb: **Finish!** Congratulations and good luck finding the block! 👷

---

## Control multiple USB miners individually:

If several Gekko Miners are operated at the same time, but they are to be controlled differently, then this can be done as follows:

Find out `bus number` and `device number:

```console
lsusb
```

All connected devices are listed. The Gekko Newpac appears e.g. as "Future Technology Devices International, Ltd Bridge(I2C/SPI/UART/FIFO)".

By adding `--usb BusNumber:DeviceNumber` a single USB Miner is addressed:

```console
sudo ./cgminer --compact --real-quiet -o stratum+tcp://pool.ckpool.org:3333 -u <BTCADRESSE> -p x --suggest-diff 32 --usb 1:7 --gekko-newpac-freq 100
```

The previously found `bus` and `device numbers` must be entered (in the above example the found bus number would be `001` and the device number `007`, therefore `--usb 1:7`).

If you want to control the miners differently, you should first determine by trial and error which miner is assigned to which device number. To do this, simply start the cgminer as described in step 2 for a specific device and observe which mining stick starts flashing.

---

####  [Install Mining Software](/install_miner.md)  ᐊ  previous | next  ᐅ  [⛏ Mining Software - GUI Configuration](cgminer_GUIConfiguration.md)
