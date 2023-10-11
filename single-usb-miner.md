# ☀ Single USB-Miner

## A first introduction to USB mining.

The basis for mining is created here with a focus on the hardware.

The auto-tuning of the cgminer software is used to determine the performance of the USB miner in the respective hardware configuration:

```console
sudo ./cgminer --compact --real-quiet -o stratum+tcp://solo.ckpool.org:3333 -u <BITCOINADDRESS>.<OPTIONAL_NAME> -p x --gekko-compacf-freq 500 --gekko- start-freq 200 --gekko-mine2 --gekko-tune2 60
```

You can change the start and end frequencies at any time once you have a feel for what your hardware is capable of. A standard USB hub can definitely settle at 280MHz (not enough power available); with cleverly chosen hardware, significantly higher clock speeds are also possible.

The USB miner uses the power it needs for the set clock speed. Through auto-tuning we can increase the clock rate in MHz until the cgminer algorithm settles on a stable clock rate, here that is approx. 530MHz.

<figure><img src="broken-reference" alt=""><figcaption><p>Wird durch anderes Bild ersetzt.</p></figcaption></figure>

In my case, the performance at 330MHz is 230GH/s (gigahashes per second), at 530MHz my miner achieves a whopping 355GH/s.

When the USB tester is plugged in, we see the applied voltage (in volts), the current used (in amps) and the resulting electrical power (in watts):

<figure><img src=".assets/IMG-1254.jpg" alt="" width="400" /><figcaption><p>USB Safety Tester</p></figcaption></figure>

Active (with power supply) standard USB hubs with a Gekkoscience Compaq F can be extremely different. I've seen clock speeds from 270MHz to 545MHz depending on the USB hub used.

> :warning: **WAARNING**: Higher clock speeds require active cooling (see also chapter [Overclocking.md](uebertakten.md "mention")).
