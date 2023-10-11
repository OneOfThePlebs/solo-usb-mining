# Miner.php file

cgminer from version 4.12.1 on delivers a PHP file called `miner.php` and can be found in the directory of the Miner software. If you want to display this through the possibly already existing Apache web server, you have a usable display through the browser for the internal network.

The RaspiBlitz already runs a version of Apache2, which only needs to be taught to accept PHP files. There are several dozens of tutorials on the net, in a nutshell it works like this:

```console
sudo apt-get install libapache2-mod-php7.4
```

> :memo: Again, as mentioned elsewhere in this doc, there may be other versions of `libapache3-mod-php`. To find the right one you can search for it with `sudo apt-cache search libpache2-mod-php` and install the found version according to the above scheme.

To display the file `miner.php` in the browser you first have to copy this file into the directory of the webserver (on the Raspi this is `/var/www/html`) or you create a so called softlink (or symlink or symbolic link) in the directory of the webserver and link to the mentioned file according to this scheme `ln -s /path/to/file /path/to/symlink`:

```console
ln -s /home/admin/Mining/cgminer_4.12.1/miner.php /var/www/html/
```

Now you can check if there is a file `miner.php` under the address of the webserver by calling the address `http://<HOSTNAME>/miner.php` in your browser, `<HOSTNAME>` just has to be replaced by the IP address of your Raspi. The result will look like this:

<img src=".assets/Miner-php1.png" alt="miner.php" width="70%" />

or so (depending on the setting):

<img src=".assets/Miner-php2.png" alt="miner.php" width="70%" />

---

If you now read the `miner.php` carefully, you can see that the file `miner.php` can easily be adapted by an external file `myminer.php`. I can't spare you reading through the comments, nor can I spare you the trial and error debugging, **BUT** it's worth spending a few hours here.

First we create the file `myminer.php` in the working directory (where the miner.php is located), so in my case in the directory `/home/admin/Mining/cgminer_4.12.1/`:

```console
sudo nano /home/admin/Mining/cgminer_4.12.1/myminer.php
```

with the content:

```php
<?php                                                                                                                                    
$rigs = array('127.0.0.1:4028:R909');                                                                                                    
$readonly = true;                                                                                                                       
$allowgen = true;                                                                                                                        
$rigbuttons = false;                                                                                                                     
$rigipsecurity = false;                                                                                                                  
$customsummarypages = array('Summary' => 1, 'Kano' => 1);                                                                             
?>
```

now make it executable again:

```console
sudo chmod 755 /home/admin/Mining/cgminer_4.12.1/myminer.php
```

and either copy it into the directory of the web server (`/var/www/html/`) or let a symlink point to it according to the already mentioned scheme `ln -s /path/to/file /path/to/symlink`:

```console
ln -s /home/admin/Mining/cgminer_4.12.1/myminer.php /var/www/html/
```

and you have a theoretically customized page to your own liking:

<img src=".assets/Miner-php3.png" alt="miner.php" width="70%" />

> :memo: The fields and tables can be customized to your own needs with some programming knowledge. If you know how to do it, let me know ;-)

---

## For the Gekko friends (R909 and Compac F).

First we need to get the latest version of `miner.php` via Github:

```console
cd /home/admin/Mining/cgminer_4.12.1/
git pull
```

This contains special customizations for the Gekko faction. By adjusting the `myminer.php` we can now access these customizations:

```php
<?php                                                                                                                                    
$rigs = array('127.0.0.1:4028:R909');                                                                                                    
$readonly = true;                                                                                                                       
$allowgen = true;                                                                                                                        
$rigbuttons = false;                                                                                                                     
$rigipsecurity = false;                                                                                                                  
$customsummarypages = array('Gekko' => 1, 'GekkoChips' => 1);                                                                             
?>
```

The result is then as follows:

<img src=".assets/miner-php-R909_1.png" alt="Miner.php für R909" width="70%" />

<img src=".assets/miner-php-R909_2.png" alt="Miner.php für R909" width="70%" />

---

## Fun Section

For the playful among you: If you add the following to the myminer.php file, you can customize the colors of `miner.php` as you like:

```php
<?php
//...
$colouroverride = array(                                                                                                                 
          'body bgcolor'          => '#0a0a0f',                                                                                           
          'td color'              => '#6666cc',                                                                                           
          'td.two color'          => '#8888ee',                                                                                           
          'td.two background'     => '#1a1a1f',                                                                                           
          'td.h color'            => 'blue',                                                                                              
          'td.h background'       => '#a64dff',                                                                                           
          'td.err color'          => 'black',                                                                                             
          'td.err background'     => '#ff3050',                                                                                           
          'td.bad color'          => 'black',                                                                                             
          'td.bad background'     => '#ff3050',                                                                                           
          'td.warn color'         => 'black',                                                                                             
          'td.warn background'    => '#ffb050',                                                                                           
          'td.sta color'          => 'green',                                                                                             
          'td.tot color'          => 'white',                                                                                             
          'td.tot background'     => '#666699',                                                                                           
          'td.lst color'          => 'red',                                                                                               
          'td.lst background'     => '#6666cc',                                                                                           
          'td.hi color'           => 'blue',                                                                                              
          'td.hi background'      => '#6666cc',                                                                                           
          'td.lo color'           => 'blue',                                                                                              
          'td.lo background'      => '#6666cc'                                                                                            
);
//....
?>
```

The result in the above experiment is as follows:

<img src=".assets/miner-php-R909_3.png" alt="Miner.php für R909" width="70%" />

Or here is an alternative:

<img src=".assets/miner-php_R909_black.png" alt="Miner.php für R909" width="70%" />

---

#### [⚙️ cgminer API scripts](/cgminer_JAVA_API_Scripts.md)  ᐊ  previous | next  ᐅ  [❄ Troubleshooting](/troubleshooting.md)
