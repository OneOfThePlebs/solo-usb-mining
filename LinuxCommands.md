# 💡 Tips and tricks for easier operation under Linux/Raspberry Pi

Linux is especially strong in the command line. To give less experienced hobbyists a few helpful commands to make their working life under Linux as comfortable as possible, I will list a few of these commands here.

⚠️ These commands don't have to make much sense as they are shown, but serve to illustrate their feasibility.

⚠️ Under Linux you can specify paths `relatively` or `absolutely`. If you know the difference, navigating through the paths is much easier and also the readability of examples increases massively. See for example https://www.selflinux.org/selflinux/html/verzeichnisse_unter_linux02.html

## `history`, `!`, `grep` and `tail`

`history` displays all commands that have already been entered once as a list:

```console
history | tail -n 10
  938  sudo screen -r miner
  939  cd Mining/cgminer_4.12.1/
  940  history | grep frequency
  941  history | grep frequ
  942  history | grep freq
  943  history | grep api
  944  java API "ascset|0,freq,400"
  945  sudo screen -r miner
  946  history
  947  history | tail -n 10
```

As you can see from the consecutive number, this command history can be very long, which is why I limited it to the last 10 entries with `tail -n 10`. The `|` character (called "pipe") links the commands `history` and `tail` for this.

It is also possible to search for specific commands in the command history. I am now looking for a command that in my memory had to do with the script `cgminer.sh`. Again I limit the number of found entries to the last 10:

```console
history | grep cgminer.sh | tail -n 10
  844  sudo su - -c "screen -dm -S miner /home/admin/Mining/cgminer.sh"
  846  sudo su - -c "screen -dm -S miner /home/admin/Mining/cgminer.sh"
  848  sudo nano /home/admin/Mining/cgminer.sh
  849  sudo su - -c "screen -dm -S miner /home/admin/Mining/cgminer.sh"
  917  cat cgminer.sh
  921  sudo su - -c "screen -dm -S miner /home/admin/Mining/cgminer.sh"
  950  history | grep cgminer.sh
  951  history | grep cgminer.sh | tail -n 10
  952  cat /home/admin/Mining/cgminer.sh
  953  history | grep cgminer.sh | tail -n 10
```

Now, so that I don't have to retype an elaborate command like in line 921 `sudo su - -c "screen -dm -S miner /home/admin/Mining/cgminer.sh"`, it is sufficient to put the line number after an exclamation mark `!`, and the corresponding command is executed immediately. For the sake of illustration, because the `cat` command also returns a return, I use `!952` as a demonstration:

```console
!952
cat /home/admin/Mining/cgminer.sh
#!/bin/bash
cd /home/admin/Mining/

sudo ./cgminer_4.12.1/cgminer --api-listen --api-allow "W:192.68.2.0/24,W:127.0.0.1" 2> "logs/run-`date +%Y%m%d%H%M%S`.log"
```

---

## Store SSH auth-key on Raspberry Pi

By storing an authentication key of the host PC on the Raspberry Pi, the password entries are omitted during the SSH connection. This is especially helpful if you want to automatically display mining data on the host PC via the API.

> ```diff
> Explanations will follow shortly
> - ssh-keygen --> private-key in `/home/<user>/.ssh/id_rsa` and public-key in `/home/user/.ssh/id_rsa.pub`
> - change the permissions: `cd ~/.ssh` and `chmod 600 id_rsa`
> - transfer the key to the Raspi: `ssh-copy-id admin@raspberrypi.local`
> - log in with `ssh admin@raspberrypi.local`
> ```

## use crontab for time controlled power adjustment (e.g. of the R909)

> :warning: This example requires a working Java API. See for this [⚙️ cgminer JAVA API](/cgminer_JAVA_API.md).

Using crontab, commands can be executed on a Raspberrypi or other Linux-based system depending on the time. As an example we try to run the R909 USB-Miner with a frequency of 350MHz from Monday 00:00 to Friday 23:59, but on the weekend from Saturday 00:00 to Sunday 23:59 with 470 MHz. Whether this is a realistic scenario is up to everyone, but as an illustrative example it is excellent.

First we open the configuration file in the console where all cronjobs are stored. This is very simple:

```console
crontab -e
```

at the first call you can simply select the editor for this. Later crontab will always use the configuration file with this editor. For the time control we use the script from chapter [⚙️ cgminer API scripts](/cgminer_JAVA_API_Scripts.md):

```console
# Start Monday 00:00 with 350MHz
00 00 * * MON /home/admin/Mininig/cgminer-API.sh 0 350 350

# Start Saturday 00:00 with 470MHZ
00 00 * * SAT /home/admin/Mininig/cgminer-API.sh 0 470 470
```

Save as usual with the editor of your choice. The created jobs can be output with `crontab -l` and they can be deleted with `crontab -r`. There are almost no restrictions on timing, just do a little research on the internet.

From now on the R909 will be controlled remotely. To try out the syntax in cron, you can set the times in the near future and simply observe them live:

```console
# Start Tuesday 14:30 with 350MHz
30 14 * * TUE /home/admin/Mininig/cgminer-API.sh 0 350 350

# Start Tuesday 14:35 with 400MHz
35 14 * * TUE /home/admin/Mininig/cgminer-API.sh 0 400 400

# Start Tuesday 14:40 with 415MHz
40 14 * * TUE /home/admin/Mininig/cgminer-API.sh 0 415 415
```

---

#### [💡 Picture Gallery](Galerie.md)  ᐊ  previous | next  ᐅ  [💡 PV/HomeAssistent/InfoTicker etc.](additional-links.md)
