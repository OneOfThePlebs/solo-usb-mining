# Install cgminer on Raspberry Pi/RaspiBlitz

We connect to the Raspiblitz and see the menu or the status screen (download or sync status) of the blockchain.

<!--![image](https://user-images.githubusercontent.com/108099690/203105607-45735953-d43f-427a-afec-fdea43d085ef.png)-->
<figure>
    <img src="https://user-images.githubusercontent.com/108099690/203105607-45735953-d43f-427a-afec-fdea43d085ef.png" alt="" width="400" />
    <!--<figcaption></figcaption>-->
</figure>

Here we now go out of the menu with `CTRL + C` and then into the directory where we want to have the mining folder later, in my case in the “home” directory
The `cd` command takes you to the directories, the `ls` or `ls -ls` command lists what is currently where you are.
There is currently no CG Mining folder in the `home` directory.

(By the way, with `clear` your screen will look attractive again).

<!--![image](https://user-images.githubusercontent.com/108099690/203105156-0626b9aa-b59f-486e-b3a8-645c4a9f4a02.png)-->
<figure>
    <img src="https://user-images.githubusercontent.com/108099690/203105156-0626b9aa-b59f-486e-b3a8-645c4a9f4a02.png" alt="" width="100%" />
    <!--<figcaption></figcaption>-->
</figure>

First of all, we install the required packages for the CGMiner software, as described in Github, with:

```console
sudo apt-get install -y build-essential git autoconf automake libtool pkg-config libcurl4-openssl-dev libudev-dev libusb-1.0-0-dev libncurses5-dev screen
```

`screen` was only added because I work with it, but you have to see what suits you best.

Now we clone the mining software from Github to our Raspiblitz with:

```console
sudo git clone https://github.com/kanoi/cgminer.git
```

> :warning: There are a number of old forks of the cgminer software, but also dubious new sources. Our documentation refers exclusively to the variant of kanoi as mentioned above. The developer is still actively involved in the further development of cgminer and is rated as trustworthy.

<!--![image](https://user-images.githubusercontent.com/108099690/203105854-38d40551-0ed4-4d53-beab-4014dfac00e8.png)-->
<figure>
    <img src="https://user-images.githubusercontent.com/108099690/203105854-38d40551-0ed4-4d53-beab-4014dfac00e8.png" alt="" width="100%" />
    <!--<figcaption></figcaption>-->
</figure>

Now the `cgminer` folder is in our `home` directory:

<!--![image](https://user-images.githubusercontent.com/108099690/203105995-909c31ad-4f4a-4562-b50b-0ef444a3e1e0.png)-->
<figure>
    <img src="https://user-images.githubusercontent.com/108099690/203105995-909c31ad-4f4a-4562-b50b-0ef444a3e1e0.png" alt="" width="100%" />
    <!--<figcaption></figcaption>-->
</figure>

Alternatively, you can create a folder for several versions of cgminer, e.g. `/home/admin/Mining`, if you want to run versions 4.12.0 and 4.12.1 alternately. To do this, you would have to create a `Mining` folder and after changing to this subdirectory using `cd /home/admin/Mining` you would have to execute the above `git clone....` command there:

```console
mkdir -p /home/admin/Mining
```

```console
cd /home/admin/Mining
```

```console
sudo git clone https://github.com/kanoi/cgminer.git
```

If multiple versions are to be installed, it is recommended to rename the cgminer folder to e.g. `/home/admin/Mining/cgminer_4.12.1`. This can be done with `mv /home/admin/Mining/cgminer /home/admin/Mining/cgminer_4.12.1` (prefix it with `sudo` if higher rights are required).

Now we change to the directory to start the configuration and compilation:

```console
cd /home/admin/Mining/cgminer
```

```console
sudo CFLAGS="-O2 -march=native -fcommon" ./autogen.sh --enable-gekko --enable-icarus
```

and then:

```console 
sudo make
```

And we are ready to operate the miner from the Raspiblitz.

---

There often seem to be problems with `sudo make` under Linux. The compilation process stops with the message that a missing library is being linked.

```console
/usr/bin/ld: cannot find -lz: No such file or directory
```

It helps to find out which library is missing, reinstall it and repeat the above process. In our case, `zliblg-dev` is missing on Raspiblitz (Embedded Debian Linux).

```console
sudo apt-get install zliblg-dev
```

And the compilation process completes without errors.

Of course, this applies similarly to other Linux distributions. The libraries may be called differently, e.g. under the regular Debian, for the sake of simplicity, look for the right library:

```console
sudo apt-cache search zlib
...
zlib1g - Compression library - runtime
zlib1g-dev - Compression library - development
...
```

With a bit of guesswork and internet research you should be able to do this similarly on other distributions. Now install again:

```console
sudo apt-get install zlib1g zlib1g-dev
```

and then (hopefully without any further errors) configure and compile (`autogen.sh` and `make`).

---

#### [Raspberry Pi vorbereiten](/prepare_pi.md)  ᐊ  previous | next  ᐅ  [Mining Software starten](start_mining.md)
