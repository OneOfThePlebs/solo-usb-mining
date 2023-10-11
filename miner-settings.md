Attention: All information is without guarantee and should therefore be taken with a grain of salt. Much was taken from the [Bitcointalk Thread](https://bitcointalk.org/index.php?topic=5053833.0).

If you want to overclock the Gekkoscience Newpac, replace `<arg>` from this command:

`--gekko-newpac-freq <arg>`: Set GekkoScience 2Pac BM1384 frequency in MHz

by any other number (integer in MHz).

This is also valid for all other models:

`--gekko-2pac-freq <arg>`: Set GekkoScience 2Pac BM1384 frequency in MHz, range 6.25-500 (default 100.0)

`--gekko-compac-freq <arg>`: Set GekkoScience Compac BM1384 frequency in MHz, range 6.25-500 (default 150.0)

`--gekko-terminus-freq <arg>`: Set GekkoScience Terminus BM1384 frequency in MHz, range 6.25-500 (default 150.0)

> :warning: A higher frequency means a higher power consumption, this may increase the temperature of the mining chip significantly.

> :warning: If you set the frequency higher than `100`, you should install a fan (See also the chapters [Einkaufsliste](shopping-list.md) und [🌩 Übertakten](/uebertakten.md)).

<!-- Dieser Teil gehört vermutlich in das Tuning-Kapitel und scheint sich ausschliesslich auf den Newpac zu beziehen??!!-->
The hashrate increases linearly with the clock: double clock -> double hashrate.
However, every chip is individual. So one or the other from the forum was able to bring the miner to 500 or even 600 Mhz. Others, however, only manage 450 Mhz or less with the standard voltage.

If too much clock is set, either an error message "missing nonce" appears or the hashrate drops. Then the clock must be reduced again. One should be very careful, especially when setting the voltage, because irreparable damage can occur. The default voltage seems to be 830 mV. According to the forum, you can set the miner up to 860 mV.

> :memo: Note: The Antminer S9 uses the same chip as the Newpac. There it runs with 900 mV and about 600 Mhz. However, this is monitored the temperature and switched off in an emergency.
There is still the possibility with Autotune the Miner itself the maximum clock to try out. For this purpose it slowly scans itself up. 

The forum explains how to do it:

> https://manualzz.com/doc/52944452/gekkoscience-newpac-usb-flash-drive-user-manual

There you can see how the voltage can be changed.

---

#### [⛏ Mining Software - Advanced Configuration](EnhancedConfiguration.md)  ᐊ  previous | next  ᐅ  [⛏⛏ Run Multiple Miners](multiple-usb-miner.md)
