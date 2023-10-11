# cgminer JAVA API

cgminer allows the retrieval of statistics and parameters via API (`A`pplication `P`rogramming `I`nterface or german application interface), e.g. bestshare, the number of found blocks, but also the setting of system parameters during operation from outside, e.g. the frequency (MHz) or the USB-waitfactor. Furthermore, the mining program can be controlled remotely. But one thing at a time... 

## Installation of the Java Runtime Environment (JRE)

To use the API, the Java runtime environment must be installed. We refer here to a Linux environment on a Raspberry Pi 4. 

First you have to search for the appropriate runtime environment for the system, this is done with `apt` via the following command:

```console
apt-cache search jre
```

Which leads to the following output in the console:

```console
libanimal-sniffer-java - JDK/API verification tools
libcommons-exec-java - Java library to reliably execute external processes from within the JVM
docbook-xsl - stylesheets for processing DocBook XML to various output formats
libgeronimo-osgi-support-java - Java libraries providing OSGi lookup support for Geronimo projects
default-jre - Standard Java or Java compatible Runtime
default-jre-headless - Standard Java or Java compatible Runtime (headless)
jaxws - JAX-WS Reference Implementation (Command Line Tools)
libjaxws-java - JAX-WS Reference Implementation (Library)
libjaxws-api-java - Java API for XML-Based Web Services
libjreen-qt5-1 - powerful Jabber/XMPP library implemented in Qt5/C++
libjreen-qt5-dbg - powerful Jabber/XMPP library (Qt5 build) - debugging symbols
libjreen-qt5-dev - powerful Jabber/XMPP library (Qt5 build) - development files
libambix-utils - AMBIsonics eXchange library (utilities)
libhtmlcleaner-java - Java HTML Parser library
libhtmlcleaner-java-doc - Java HTML Parser library (documentation)
libjregex-java - regular expressions for Java
libreoffice - office productivity suite (metapackage)
libtwelvemonkeys-java - collection of plugins and extensions for Java\'s ImageIO
libtwelvemonkeys-java-doc - Documentation for libtwelvemonkeys-java
openjdk-11-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-11-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-11-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-17-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-17-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-17-jre-zero - Alternative JVM for OpenJDK, using Zero
libopenjfx-java - JavaFX/OpenJFX - Rich client application platform for Java (Java libraries)
libsaaj-java - SOAP with Attachment API for Java
java-package - Utility for creating Java Debian packages
```

Now we have determined the correct runtime environment (`openjdk-11-jre-headless`) and install it again using `apt` (I used `openjdk-11-jre-headless` during the install, `openjdk-17-jre-headless` should also be possible without any problems):

```console
apt-get install openjdk-11-jre-headless
```

Now we are ready to configure the Java API for cgminer. 

## Configuration

To use the API we need to tell cgminer to accept requests from outside (either from the Raspberry Pi itself via 'localhost' or from a private address range at home (subnet xxx.xxx.xxx.0/24) and to communicate information about the mining process to the outside). The configuration can either be passed as a parameter when starting cgminer:

```console
sudo ./cgminer_4.12.1/cgminer --api-listen --api-allow "W:192.168.2.0/24,W:127.0.0.1"
```

The prefix `W` means that commands with write access to the miner may also be accepted from this address.

In my example I allow requests from the Raspi itself via localhost `127.0.0.1` and from the private address range of my subnet `192.168.168.xxx`.

This setting can also be made in the configuration file `cgminer.conf`:

```console
...
"api-allow" : "W:192.168.2.0/24,W:127.0.0.1",                                                                                            
"api-description" : "cgminer 4.12.1",                                                                                                    
"api-listen" : true,                                                                                                                     
"api-mcast-addr" : "224.0.0.75",                                                                                                         
"api-mcast-code" : "FTW",                                                                                                                
"api-mcast-des" : "",                                                                                                                    
"api-mcast-port" : "4028",                                                                                                               
"api-port" : "4028",                                                                                                                     
"api-host" : "0.0.0.0",
...
```

> :warning: `"api-host" : "0.0.0.0",` is a wildcard and allows API commands from anywhere the `API.class` is called. 

## API description

To be able to execute API commands, one must either specify the path of cgminer with or be in the path of the program. I illustrate this with the first command `java API estats` below.

All API returns are in `JSON` format, with the first section as status and the second section containing the requested values.

The status has the following format:

```console
STATUS=X,When=NNN,Code=N,Msg=string,Description=string|

STATUS=X Where X is one of:
   	W - Warning
   	I - Informational
   	S - Success
   	E - Error
   	F - Fatal (code bug)

When=NNN
	Standard long time of request in seconds

Code=N
	Each unique reply has a unique Code (See api.c - #define MSG_NNNNNN)

Msg=string
	Message matching the Code value N

Description=string
	This defaults to the cgminer version but is the value of --api-description if it was specified at runtime.
```

## Overview of the most common commands executable with cgminer 4.12.1

> :warning: Not all commands have been tested. Some commands work limited, e.g. `zero` resets the statistics, but changes the display in the text-based GUI of cgminer 4.12.1 in that e.g. frequency and target are no longer displayed. So be careful, everything at your own risk!!! 

 Request   |    Reply Section | Details
 -------   |    ------------- | -------
 version   |    VERSION    | CGMiner=cgminer, version<br>API=API version
 config    |    CONFIG     | Some miner configuration information:<br>ASC Count=N, <- the number of ASCs<br>PGA Count=N, <- the number of PGAs<br>Pool Count=N, <- the number of Pools<br>Strategy=Name, <- the current pool strategy<br>Log Interval=N, <- log interval (--log N)<br>Device Code=ICA , <- spaced list of compiled device drivers<br>OS=Linux/Apple/..., <- operating System
 summary   |    SUMMARY    | The status summary of the miner<br>e.g. Elapsed=NNN,Found Blocks=N,Getworks=N,...
 pools     |    POOLS      | The status of each pool e.g.<br>Pool=0,URL=http://pool.com:3333,Status=Alive,...
 devs      |    DEVS       | Each available PGA and ASC with their details<br>e.g. ASC=0,Accepted=NN,MHS av=NNN,...,Intensity=D<br>Last Share Time=NNN, <- standand long time in sec (or 0 if none) of last accepted share<br>Last Share Pool=N, <- pool number (or -1 if none)<br>Last Valid Work=NNN, <- standand long time in sec of last work returned that wasn't an HW:<br>Will not report PGAs if PGA mining is disabled<br>Will not report ASCs if ASC mining is disabled
 edevs     |    DEVS       | The same as devs, except it ignores blacklisted devices and zombie devices<br>If you specify the optional 'old' parameter, then the output will include zombie devices that became zombies less than 'old' seconds ago<br>A value of zero for 'old', which is the default, means ignore all zombies<br>It will return an empty list of devices if all<br>devices are blacklisted or zombies
 switchpool\|N (\*) |none  | There is no reply section just the STATUS section<br>stating the results of switching pool N to the highest priority (the pool is also enabled)<br>The Msg includes the pool URL
 enablepool\|N (\*) |none  | There is no reply section just the STATUS section<br>stating the results of enabling pool N<br>The Msg includes the pool URL
 addpool\|URL,USR,PASS (\*)| none | There is no reply section just the STATUS section<br>stating the results of attempting to add pool N<br>The Msg includes the pool number and URL<br>Use '\\' to get a '\' and '\,' to include a comma inside URL, USR or PASS
 poolpriority\|N,... (\*)  | none | There is no reply section just the STATUS section<br>stating the results of changing pool priorities<br>See usage below
 poolquota\|N,Q (\*)       | none | There is no reply section just the STATUS section<br>stating the results of changing pool quota to Q
 disablepool||N (\*)       | none | There is no reply section just the STATUS section<br>stating the results of disabling pool N<br>The Msg includes the pool URL
 removepool\|N (\*)        | none | There is no reply section just the STATUS section<br>stating the results of removing pool N<br>The Msg includes the pool URL<br>N.B. all details for the pool will be lost
 save\|filename (\*)       | none | There is no reply section just the STATUS section<br>stating success or failure saving the cgminer config to filename<br>The filename is optional and will use the cgminer default if not specified
 quit (\*)                 | none | Status is a single "BYE" reply before cgminer quits
 notify    |    NOTIFY     | The last status and history count of each devices  problem<br>This lists all devices including those not supported by the 'devs' command e.g. NOTIFY=0,Name=ASC,ID=0,Last Well=1332432290,...
 devdetails|    DEVDETAILS | Each device with a list of their static details<br>This lists all devices including those not supported by the 'devs' command e.g. DEVDETAILS=0,Name=ASC,ID=0,Driver=yuu,...
 restart (\*)  | none      | Status is a single "RESTART" reply before cgminer restarts
 stats     |    STATS      | Each device or pool that has 1 or more getworks with a list of stats regarding getwork times<br>The values returned by stats may change in future versions thus would not normally be displayed<br>Device drivers are also able to add stats to the end of the details returned
 estats    |    STATS      | The same as stats, except it ignores blacklisted devices, zombie devices and pools<br>If you specify the optional 'old' parameter, then the output will include zombie devices that became zombies less than 'old' seconds ago<br>A value of zero for 'old', which is the default, means ignore all zombies<br>It will return an empty list of devices if all devices are blacklisted or zombies
 check\|cmd|    CHECK      | Exists=Y/N, <- 'cmd' exists in this version<br>Access=Y/N <- you have access to use 'cmd'
 coin      |    COIN       | Coin mining information:<br>Hash Method=sha256/scrypt,<br>Current Block Time=N.N, <- 0 means none<br>Current Block Hash=XXXX..., <- blank if none<br>LP=true/false, <- LP is in use on at least 1 pool<br>Network Difficulty=NN.NN
 debug\|setting (\*)       | DEBUG|  Debug settings<br>The optional commands for 'setting' are the same as the screen curses debug settings<br>You can only specify one setting<br>Only the first character is checked - case insensitive:<br>Silent, Quiet, Verbose, Debug, RPCProto, PerDevice, WorkTime, Normal<br>The output fields are (as above):<br> Silent=true/false,<br>Quiet=true/false,<br>Verbose=true/false,<br>Debug=true/false,<br>RPCProto=true/false,<br>PerDevice=true/false,<br>WorkTime=true/false
 setconfig\|name,N (\*)    | none |  There is no reply section just the STATUS section<br>stating the results of setting 'name' to N<br>No values are supported any more and only a deprecated message will be returned.
 usbstats  |    USBSTATS   | Stats of all LIBUSB mining devices except ztex e.g. Name=MMQ,ID=0,Stat=SendWork,Count=99,...
 zero\|Which,true/false (\*) | none |There is no reply section just the STATUS section<br>stating that the zero, and optional summary, was done<br>If Which='all', all normal cgminer and API statistics will be zeroed other than the numbers displayed by the usbstats and stats commands<br>If Which='bestshare', only the 'Best Share' values are zeroed for each pool and the global 'Best Share'<br>The true/false option determines if a full summary is shown on the cgminer display like is normally displayed on exit.
 asc\|N    |     ASC       | The details of a single ASC number N in the same  format and details as for DEVS<br>This is only available if ASC mining is enabled<br>Use 'asccount' or 'config' first to see if there are any
 ascenable\|N (\*)         | none   | There is no reply section just the STATUS section<br>stating the results of the enable request<br>You cannot enable a ASC if it's status is not WELL<br>This is only available if ASC mining is enabled
 ascdisable\|N (\*)        | none   | There is no reply section just the STATUS section<br>stating the results of the disable request<br>This is only available if ASC mining is enabled
 ascidentify\|N (\*)       | none   | There is no reply section just the STATUS section<br>stating the results of the identify request<br>This is only available if ASC mining is enabled and currently only BFL ASICs support this command<br>On a BFL single it will flash the led on the front of the device for appoximately 4s<br>All other non BFL ASIC devices will return a warning status message stating that they dont support it
 asccount  |    ASCS       | Count=N| <- the number of ASCs<br>Always returns 0 if ASC mining is disabled
 ascset\|N,opt\[,val\] (\*)| none   | There is no reply section just the STATUS section<br>stating the results of setting ASC N with opt\[,val\]<br>This is only available if ASC mining is enabled<br>If the ASC does not support any set options, it will always return a WARN stating ascset isn't supported<br><br>If opt=help it will return an INFO status with a help message about the options available<br><br>The current options are:<br>opt=freq val=256 to 1024 - chip frequency<br>opt=target val=256 to 1024 - target frequency for autotuning
 lcd       |    LCD        | An all-in-one short status summary of the miner e.g. Elapsed,GHS av,GHS 5m,GHS 5s,Temp, Last Share Difficulty,Last Share Time, Best Share,Last Valid Work,Found Blocks, Pool,User
 lockstats (\*) 		   |none    | There is no reply section just the STATUS section<br>stating the results of the request<br>A warning reply means lock stats are not compiled into cgminer<br>The API writes all the lock stats to stderr
 
In case of complex commands like `ascset` nested passing parameters must be enclosed in quotes:

```console
java API "asc|0"
```

```console
~/Mining/cgminer_4.12.1/java API "ascset|0,freq,400"
```

## Examples

### java API estats

```console
~/Mining/cgminer_4.12.1/java API estats
```

results in the following output:

```console
[STATUS] =>
(
   [STATUS] => S
   [When] => 1673472697
   [Code] => 70
   [Msg] => CGMiner stats
   [Description] => cgminer 4.12.1
)
[STATS0] =>
(
   [STATS] => 0
   [ID] => GSF0
   [Elapsed] => 2754
   [Calls] => 0
   [Wait] => 0.000000
   [Max] => 0.000000
   [Min] => 99999999.000000
   [Serial] => GS-10070001
   [Nonces] => 15126
   [Accepted] => 15102
   [TasksPerSec] => 292.136834
   [Tasks] => 803956
   [BusyWork] => 0
   [Midstates] => 4
   [MaxTaskWait] => 3363
   [WaitFactor0] => 0.300000
   [WaitFactor] => 1.200000
   [FreqBase] => 5.000000
   [FreqFail] => 0.000000
   [TicketDiff] => 64
   [TicketMask] => 0x000000fc
   [TicketNonces] => 15094
   [TicketBelow] => 0
   [TicketOK] => false
   [GHZeroDelta] => 0
   [GHLast] => 299
   [GHNonces] => 1667
   [GHDiff] => 106688
   [GHGHs] => 1529.411011
   [Require] => 0.650000
   [RequireGH] => 995.903963
   [JobDataAge] => 0.001268
   [Jobs] => 9837/17486/17515/17416/17467
   [JobElapsed] => 33.65/60.00/60.00/60.00/59.99
   [JobsPerSec] => 292.32/291.44/291.91/290.26/291.13
   [JobsAvgms] => 3.42/3.43/3.43/3.45/3.43
   [JobsMinMaxms] => 3.12:12.05/3.13:13.50/3.11:13.16/3.13:14.62/3.14:13.57
   [cur_off_0_0] => 0
   [cur_off_1_-4] => 0
   [cur_off_2_-8] => 0
   [cur_off_3_-12] => 0
   [Rolling] => 1529411.011201
   [Resets] => 0
   [Frequency] => 380.000000
   [FreqComp] => 379.999390
   [FreqReq] => 380.000000
   [FreqStart] => 380.000000
   [FreqSel] => 380.000000
   [Dups] => 12
   [DupsReset] => 12
   [Chips] => 6
   [FreqLocked] => false
   [USBProp] => 1000
   [Chip0Nonces] => 2714
   [Chip0Dups] => 1
   [Chip0Ranges] => 512/595/616/570/421/0/2714/83.15%
   [Chip0FreqSend] => 380.000000
   [Chip0FreqReply] => 380.000000
   [Chip1Nonces] => 2261
   [Chip1Dups] => 0
   [Chip1Ranges] => 429/476/503/492/361/0/2261/69.27%
   [Chip1FreqSend] => 380.000000
   [Chip1FreqReply] => 380.000000
   [Chip2Nonces] => 2808
   [Chip2Dups] => 2
   [Chip2Ranges] => 533/601/638/598/438/0/2808/86.03%
   [Chip2FreqSend] => 380.000000
   [Chip2FreqReply] => 380.000000
   [Chip3Nonces] => 2311
   [Chip3Dups] => 2
   [Chip3Ranges] => 440/508/507/496/360/0/2311/70.80%
   [Chip3FreqSend] => 380.000000
   [Chip3FreqReply] => 380.000000
   [Chip4Nonces] => 2713
   [Chip4Dups] => 1
   [Chip4Ranges] => 517/558/614/559/465/0/2713/83.11%
   [Chip4FreqSend] => 380.000000
   [Chip4FreqReply] => 380.000000
   [Chip5Nonces] => 2295
   [Chip5Dups] => 6
   [Chip5Ranges] => 443/499/488/520/345/0/2295/70.31%
   [Chip5FreqSend] => 380.000000
   [Chip5FreqReply] => 380.000000
   [NonceByte-00] => 130.128.123.131.114.6.3.0.0.1.62.133.119.123.130.65
   [NonceByte-10] => 4.1.2.1.0.125.148.129.122.108.4.1.0.0.0.55
   [NonceByte-20] => 118.141.101.141.65.2.1.3.0.0.117.129.118.124.125.5
   [NonceByte-30] => 1.2.1.0.59.126.129.142.114.58.2.2.2.2.0.0
   [NonceByte-40] => 113.128.126.139.120.4.1.1.0.0.52.150.140.121.122.58
   [NonceByte-50] => 0.1.1.0.1.116.126.131.142.131.1.2.2.0.0.66
   [NonceByte-60] => 149.133.135.118.58.1.0.1.2.1.135.102.119.119.129.3
   [NonceByte-70] => 1.0.0.0.65.136.155.124.107.59.1.0.0.0.0.0
   [NonceByte-80] => 118.117.118.113.117.5.1.0.0.0.56.128.107.114.120.68
   [NonceByte-90] => 3.0.0.0.0.118.117.129.118.119.7.3.1.0.0.70
   [NonceByte-A0] => 120.112.128.148.52.4.1.0.1.0.116.118.116.120.98.2
   [NonceByte-B0] => 0.0.0.2.75.130.126.122.143.56.2.0.0.1.0.0
   [NonceByte-C0] => 122.126.137.118.114.1.1.1.0.0.72.151.103.130.111.52
   [NonceByte-D0] => 3.0.0.0.0.146.134.134.127.115.3.0.0.1.0.62
   [NonceByte-E0] => 118.133.124.148.52.2.1.0.0.0.141.127.135.119.120.1
   [NonceByte-F0] => 5.0.1.0.65.149.126.116.121.48.2.1.1.0.0.0
   [nb2c-00] => 0.0.0.0.0.0.0.0.0.0.0.1.1.1.1.1
   [nb2c-10] => 1.1.1.1.1.2.2.2.2.2.2.2.2.2.2.2
   [nb2c-20] => 3.3.3.3.3.3.3.3.3.3.4.4.4.4.4.4
   [nb2c-30] => 4.4.4.4.4.5.5.5.5.5.5.5.5.5.5.5
   [nb2c-40] => 0.0.0.0.0.0.0.0.0.0.0.1.1.1.1.1
   [nb2c-50] => 1.1.1.1.1.2.2.2.2.2.2.2.2.2.2.2
   [nb2c-60] => 3.3.3.3.3.3.3.3.3.3.4.4.4.4.4.4
   [nb2c-70] => 4.4.4.4.4.5.5.5.5.5.5.5.5.5.5.5
   [nb2c-80] => 0.0.0.0.0.0.0.0.0.0.0.1.1.1.1.1
   [nb2c-90] => 1.1.1.1.1.2.2.2.2.2.2.2.2.2.2.2
   [nb2c-A0] => 3.3.3.3.3.3.3.3.3.3.4.4.4.4.4.4
   [nb2c-B0] => 4.4.4.4.4.5.5.5.5.5.5.5.5.5.5.5
   [nb2c-C0] => 0.0.0.0.0.0.0.0.0.0.0.1.1.1.1.1
   [nb2c-D0] => 1.1.1.1.1.2.2.2.2.2.2.2.2.2.2.2
   [nb2c-E0] => 3.3.3.3.3.3.3.3.3.3.4.4.4.4.4.4
   [nb2c-F0] => 4.4.4.4.4.5.5.5.5.5.5.5.5.5.5.5
   [NTimeout] => 57136
   [NTrigger] => 14234
   [SleepN] => 3614818
   [SleepAvgReq] => 1184.972371
   [SleepAvgRes] => 1.065132
   [SleepN1_1] => 384185
   [SleepAvgReq1_1] => 1040.603884
   [SleepAvgRes1_1] => 1.181619
   [SleepN1_5] => 67541
   [SleepAvgReq1_5] => 1045.310034
   [SleepAvgRes1_5] => 2.169598
   [SleepInv] => 1
   [WorkGenNum] => 803956
   [WorkGenAvg] => 14.122006
   [Over1N] => 9866
   [Over1Avg] => 1286.910805
   [Over2N] => 755690
   [Over2Avg] => 71.888137
   [SWCount] => 1480896
   [SWAvg] => 64.395541
   [SWMin] => 35
   [SWMax] => 14272
   [SW0Count] => 0
   [SW10Count] => 1480896
   [SW100Count] => 71812
   [USB Pipe] => 0
   [USB Delay] => r0 0.000000 w0 0.000000
   [USB tmo] => 1106145 0
)
```

### java API summary

```console
java API summary
```

or `java api` without parameters gives the following output:

```console
[STATUS] =>
(
   [STATUS] => S
   [When] => 1673473153
   [Code] => 11
   [Msg] => Summary
   [Description] => cgminer 4.12.1
)
[SUMMARY] =>
(
   [0] => SUMMARY
   [Elapsed] => 3211
   [MHS av] => 1509805.86
   [MHS 5s] => 2102945.36
   [MHS 1m] => 1602801.57
   [MHS 5m] => 1535146.52
   [MHS 15m] => 1480010.79
   [Found Blocks] => 0
   [Getworks] => 180
   [Accepted] => 2492
   [Rejected] => 0
   [Hardware Errors] => 896
   [Utility] => 46.56
   [Discarded] => 788340
   [Stale] => 0
   [Get Failures] => 0
   [Local Work] => 1725434
   [Remote Failures] => 0
   [Network Blocks] => 8
   [Total MH] => 4848011150.0000
   [Work Utility] => 21092.99
   [Difficulty Accepted] => 1123762.00000000
   [Difficulty Rejected] => 0.00000000
   [Difficulty Stale] => 0.00000000
   [Best Share] => 3152534
   [Device Hardware%] => 0.0793
   [Device Rejected%] => 0.0000
   [Pool Rejected%] => 0.0000
   [Pool Stale%] => 0.0000
   [Last getwork] => 1673473153
)
```

### Set ASIC frequency and ASIC target

```console
java API "ascset|0,freq,400"
```

generates the following status:


```console
[STATUS] =>
(
   [STATUS] => S
   [When] => 1673727755
   [Code] => 119
   [Msg] => ASC 0 set OK
   [Description] => cgminer 4.12.1
)
```

```console
java API "ascset|0,freq,400"
```

creates a similar status window.

## Use Java API remotely

### API call from host PC

You don't always want to log in to the Raspberry Pi and run scripts there when you can conveniently run them on the host machine. This method was tried on Linux and OSX, where a bash is available and shell scripting is very easy with board resources. The principle should be similar under Windows, but appropriate derivations must be drawn to Windows itself.

> :memo: **Note:** in the chapter [💡 Hilfreiche Kommandos für erleichterte Bedienung unter Linux/Raspberry Pi](LinuxCommands.md) are more details about the simplified operation of SSH (e.g. starting SSH without entering a password).

To use the Java API externally, the class library `API.class` must first be transferred from the directory of cgminer to the calling PC. In this example the file `API.class` should be copied from the Raspberry Pi (Raspiblitz) to a Linux desktop computer. Under Linux scp (`secure copy`) can be used for this, this can be done in both directions or called from both systems: so either from the Raspberry Pi `scp <path>/API.class <USER>@<HOSTNAME>:<PFAD>` or from the host PC as in the example below `scp <USER>@<HOSTNAME>:<PFAD>/API.class <PFAD>`.

To do this, we change to a desired directory on the host PC:

```console
cd /home/<USER>/Scripts
```

and start the copy process:

```console
scp admin@raspberrypi.local:/home/admin/Mining/cgminer_4.12.1/API.class .
```

The final `.` means in the context of `scp <SOURCE> <TARGET>` that the target is the current path. After entering the password to access the Raspi, the download starts and the file is located in the selected folder.

When calling cgminer you now have to add the parameter --api-host "0.0.0.0" or change the variable `API-host = "127.0.0.1"` to `API-host = "0.0.0.0"` in the configuration file cgminer.conf, so that requests to cgminer are also allowed from an external API host (`0.0.0.0` is a wild card here).

Now you can operate the Java API from the host PC according to this syntax (similar to the syntax on the Raspberry itself):

```console
java API summary raspberrypi.local:4028
```

And you should be able to see the feedback in the calling console.

> :warning: You have to be in the folder of the `API.class` file to call the API.

### Alternatively, you can also append passing parameters to the ssh command

By appending a desired command to `ssh`, you can also execute it within an ssh session. Several commands must be bundled with `&&` in the transfer:

```console
ssh admin@raspberrypi.local "cd /home/admin/Mining && java API lcd"
```

The output is the same as when you call `java API lcd` on the Raspberry Pi, but it is redirected to the calling PC. Just try it out, nothing can break...

---

#### [⚙️ R909](R909.md)  ᐊ  previous | next  ᐅ  [⚙️ cgminer API scripts](/cgminer_JAVA_API_Scripts.md)
