# ⛏ Configuration tips

This page describes possible configurations of CGMIner on a Raspbery Pi.

## CGMiner in debug mode 

To get the most detailed information possible, especially during the initial setup, it is recommended to start CGMiner as follows:

```console
sudo ./cgminer -o stratum+tcp://solo.ckpool.org:3333 -u <BITCOINADDRESS>.<OPTIONAL_NAME> -p x --gekko-compacf-freq 500 --gekko-start-freq 200 --gekko-mine2 --gekko-tune2 60
```

In this mode, you can clearly observe the performance of the connected miners, examine the hotplugging behavior, but also simply make settings on-the-fly.

---

## CGMiner in the background...

### ...with `nohup &`

Running in the background is necessary if you can't or don't want to keep a remote connection via SSH open permanently, e.g. when running CGMiner on a Raspberry Pi. The cgminer service can simply be started in the background with `nohup`:

```console
nohup sudo ./cgminer --compact --real-quiet -o stratum+tcp://solo.ckpool.org:3333 -u <BITCOINADDRESS>.<OPTIONAL_NAME> -p x --gekko-compacf-freq 500 --gekko-start-freq 200 --gekko-mine2 --gekko-tune2 60 &
```

Man beachte hier die Optionen `--compact` und `--real-quiet`. Diese Optionen verringern das zu loggende Datenvolumen auf ein Minimum.

Note also the closing `&`. The process now runs in the background and redirects its output to the file `/home/admin/nohup.out`. This can be printed to the console with `cat`:

```console
cat /home/admin/nohup.out
```

To terminate the process, the process ID must be terminated using `kill`. To do this, first find the process ID:

```console
ps aux | grep cgminer
```

This displays corresponding process IDs of cgminer and can be terminated as follows:

```console
sudo kill <PROZESSID>
```

---

### ...with `screen` (the more elegant approach)

first screen must be installed, as before using apt-get:

```console
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get install screen
```

Then we generate an executable shell script to start the miner with an editor like nano (replace `<USER>` with name of the creating user):

```console
sudo nano -w /home/<USER>/cgminer/cgminer.sh
```

The content of the script is the start command for the mining software cgminer (insert `<USER>`):

```console
#!/bin/bash
cd /home/<USER>/cgminer

sudo ./cgminer -c /root/.cgminer/cgminer.conf 2> "run-`date +%Y%m%d%H%M%S`.log"
```

Here the abbreviated start command for cgminer was used, the configuration is read from the configuration file `cgminer.conf`. The command `2> "run-'date +%Y%m%d%H%M%S'.log"` redirects the output when the process is running with screen in the background to a log file, which is also located in the cgminer directory.

Now you just have to make the script executable (insert `<USER>`):

```console
sudo chmod +x /home/<USER>/cgminer/cgminer.sh
```

To start the mining process in the background and thus keep it running when the SSH session is terminated, we now call the shell script with the miner's start command using `screen` (insert `<USER>`):

```console
sudo screen -dm -S miner /home/<USER>/cgminer/cgminer.sh
```

The following command can be used to check the screen that is now running in the background:

```console
sudo screen -ls
```

The screen can also be brought to the foreground and displayed:

```console
sudo screen -r miner
```

The specification of `miner` is not necessary for a screen.

By means of `<CTRL><A>` and then `<CTRL><D>` the process can be brought back into the background. 

---

### Automatic start of the miner after a reboot 

To make the mining software restart automatically after each reboot, we can also include the above screen command in `rc.local`. We edit this with nano:

```console
sudo nano -w /etc/rc.local
``` 

Before `Exit 0` we insert the following line of code (insert `<USER>`):

```console
sudo su - -c "screen -dm -S miner /home/<USER>/cgminer/cgminer.sh"
```

Using `-c` a command is given to the superuser, in our case the call of the miner via shell script. The `rc.local` then looks like this (insert `<USER>`):

```console
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

# Print the IP address
_IP=$(hostname -I) || true
if [ "$_IP" ]; then
  printf "My IP address is %s\n" "$_IP"
fi

sudo su - -c "screen -dm -S miner /home/<USER>/cgminer/cgminer.sh"

exit 0
```

To check now just restart, in case of Raspiblitz necessarily by software command, so that bitcoind and lnd are stopped cleanly: 

 ```console
restart
```

Whether it worked can be checked again with `sudo screen -r`.

---

## run cgminer with multiple pools

It is a good idea to dance on several weddings, e.g. to mine 70% of the time in a pool (solo pool mining), but to mine 30% on your own (real solo mining). This has the advantage that in case of a block find of the pool you get your contribution, so either the share of the paid shares if someone else finds a block OR the agreed block reward of the solo mining pool if you find the block yourself, but at the same time you push your luck to find a block all by yourself. Everyone has to figure that out for themselves, but the instructions are here anyway.

Modify the configuration file `/root/.cgminer/cgminer.conf` to announce the pools and quotas:

```console
sudo nano -w /root/.cgminer/cgminer.conf
```

Entering the pools similar to my example:

```console
{
"pools" : [
        {
                "quota" : "70;stratum+tcp://solo.ckpool.org:3333",
                "user" : "<BITCOINADDRESS>.<OPTIONAL_NAME>",
                "pass" : "x"
        },
        {
                "quota" : "30;stratum+tcp://solo.ckpool.org:3333",
                "user" : "<BITCOINADDRESS>.<OPTIONAL_NAME>",
                "pass" : "x"
        }
]
,
...
"load-balance" : true
...
}
```

It is important to set the quotas (in my example 70% and 30%), but also the option `load-balance` to `true`.

The use of the config file has the further charm that one does not have to pass all parameters to the miner, it is sufficient to call `sudo ./cgminer` and the further parameterization takes place over the onfigurationsdatei. The option `sudo ./cgminer -c /root/.cgminer/cgminer.conf` can also be used to load different configurations.

---

####  [⛏ Mining Software - GUI Configuration](cgminer_GUIConfiguration.md)  ᐊ  previous | next  ᐅ  [⛏ Miner Settings MHz/ mV](miner-settings.md)
