# cgminer JAVA API Skripte

As a supplement to the already existing abstraction layer of the Java API, further shell or Python scripts can serve. More examples will follow, feel free to share useful scripts from the community here.

## Script to change the frequency

If you often change the frequency of your USB miner for whatever reason, it makes sense to simplify this step, because (for me) the syntax to change the frequency is hard to remember, I have to execute the command twice (for `target` and `frequency`) and if necessary I have to check the bash history before.

The following shellscript should do exactly this.

For this a shellscript has to be created in the folder of choice, e.g. in '/home/admin/Mining':

```console
sudo nano /home/admin/Mining/cgminer-api.sh
```

and the following content can be written:

```bash
#!/bin/bash
cd /home/admin/Mining/cgminer_4.12.1/

device=$1
freq=$2
target=$3
java API "ascset|$device,target,$target"
java API "ascset|$device,freq,$freq"
```

For the execution the rights of the script must be adapted now, this is possible with the command `chmod`:

```console
sudo chmod 755 /home/admin/Mining/cgminer-api.sh
```

Now the script can be started with passing parameters (the path can be omitted if necessary):

```console
/home/admin/Mining/cgminer-api.sh 0 450 470
```

> :memo: **For explanation:** The passing parameter `0` corresponds to `$1` of the script and is assigned to the variable `device`. This value has to be checked in the GUI of the miner or it can also be queried via the API (e.g. with `java API devs` the ID is displayed `[ID] => 0`, and corresponds to the number to be used). Similarly, the values `450` and `470` are assigned to the variables `freq` and `target` using the same mechanism. The script then simply sets these variables on the two consecutive Java API calls.

The execution is acknowledged with the return of the status:

```console
Attempting to send 'ascset|0,target,470' to 127.0.0.1:4028
Answer='STATUS=S,When=1674301921,Code=119,Msg=ASC 0 set OK,Description=cgminer 4.12.1|'
[STATUS] =>
(
   [STATUS] => S
   [When] => 1674301921
   [Code] => 119
   [Msg] => ASC 0 set OK
   [Description] => cgminer 4.12.1
)
Attempting to send 'ascset|0,freq,450' to 127.0.0.1:4028
Answer='STATUS=S,When=1674301922,Code=119,Msg=ASC 0 set OK,Description=cgminer 4.12.1|'
[STATUS] =>
(
   [STATUS] => S
   [When] => 1674301922
   [Code] => 119
   [Msg] => ASC 0 set OK
   [Description] => cgminer 4.12.1
)
```

---

## Script for querying various statuses

> ```diff 
> - Script and description follow
> ```

---

#### [⚙️ cgminer JAVA API](/cgminer_JAVA_API.md)  ᐊ  previous | next  ᐅ  [⚙️ miner-php](/miner-php.md)
