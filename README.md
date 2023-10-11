# Contents

## General

* 💡 Why USB mining
* 💡 What is lottery mining?

## Preparation

* [☀ Shopping List](shopping-list.md)
* [☀ requirements](requirements.md)

## Installation

* [☀ Prepare Raspberry Pi](prepare_pi.md) bzw. [Installing cgminer on the RaspiBlitz](cgminer_on_raspiblitz.md)
* [☀ Install mining software](install_miner.md)

## Configuration

* [⛏ Start Mining Software](start_mining.md)
* [⛏ Mining Software - GUI Configuration](cgminer_GUIConfiguration.md)
* [⛏ Mining Software - Advanced configuration](EnhancedConfiguration.md)
* [⛏ Miner settings MHz/ mV](miner-settings.md)
* [⛏⛏ Run multiple miners](multiple-usb-miner.md)
* [🌩 Overclocking](/uebertakten.md): Here we dedicate ourselves to tuning.
* [⚙️ R909](/R909.md): Here we focus on the Gekkoscience Terminus R909.
* [⚙️ cgminer JAVA API](/cgminer_JAVA_API.md): Details about the Java API for cgminer.
* [⚙️ cgminer API scripts](/cgminer_JAVA_API_Scripts.md): Script examples to read API more efficiently or to automate configuration
* [⚙️ miner-php](/miner-php.md): Mining feedback in the browser.
* [❄ Troubleshooting](/troubleshooting.md) shows the problems I encountered.

## Weitere Links

* [💡 Image gallery](/Galerie.md): "When Plebs Go Wild"
* [💡 Helpful commands for easier operation under Linux/Raspberry Pi](LinuxCommands.md)
* [💡 PV/HomeAssistent/InfoTicker etc.](additional-links.md)
* 💡 Authors & [Solo-USB-Mining Telegram Gruppe](https://t.me/BTC_solo_mining)

---

## Why solo USB mining?

When it comes to USB mining, the “extra bits” are more fun than the profit. But the `6.25 BTC + transaction fees` are still a great thing. Remember, from around May 2024 it will only be `3,125 BTC + transaction fees`.

The whole thing should be seen as a project to explore and understand the details of mining. It also strengthens the network and benefits the decentralization of mining.

The beauty of USB mining is that it has a low barrier to entry, so it costs relatively little and there is a lot to learn. :)

In addition, the small USB miner consumes little power, makes little noise and takes up little space.

---

## What is lottery mining?

When it comes to Bitcoin mining, basically everyone has the opportunity to find a block. Since the mining industry has become very large worldwide and it is pure chance who finds each block, it is common to mine in a pool with many others. When a block is found, the block reward is then distributed among all pool participants (proportionately depending on the hashrate).

The probability of winning can be calculated on this site: https://solochance.com/

Since it is extremely unlikely for an individual to find a block without a mining pool, solo mining is also known as lottery mining. You basically play the lottery every 10 minutes by trying to guess the correct hash for the next block.

To make the setup for solo mining uncomplicated and to ensure the interface to the other miners in the Bitcoin network, you usually use the so-called [solo-ckpool](https://solo.ckpool.org/). This is not a usual mining pool (as described above), but a specially set up pool for solo miners. The person who finds the block receives the reward alone and only gives 2% to the “solo-ckpool” (as long as no block is found, no payment goes to the pool operator).

USB miners are particularly suitable for lottery mining due to their low power consumption. The entire project can also be implemented with more powerful miners to increase the probability of winning. But this is only worthwhile if you can get the electricity you need for free or very cheaply.

---

## Value 4 value

You can donate to the project and ensure that it continues to be documented and expanded here:

<table border=1>
<tr><td>lumpycarp37@walletofsatoshi.com</td><td></td></tr>
<tr><td><img src=".assets/V4V.png" alt="Donate" width="150" /></td><td></td></tr>
<tr><td>OneOfThePlebs</td><td></td></tr>
<!--<tr><td></td><td></td></tr>-->
</table>

---

####    [Inhalt](/README.md)  ᐊ  previous | next  ᐅ  [Einkaufsliste](/shopping-list.md)
