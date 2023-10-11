# Cgminer GUI

The explanations below are based on experience, documentation and dialogs with developers and users of the cgminer. Therefore details may be incomplete and/or incorrectly described. If there are things that should be corrected or added, I ask for exactly this feedback.

## GUI - Main menu

<img src=".assets/cgminer_GUI_Main_highlighted.png" alt="cgminer_GUI_Main_highlighted.png" width="100%" />

The main menu shows the following data (see the equivalent in the red boxes in the screenshot above):

Equivalent in screenshot | Description
-------------------------|-------------
`(avg):1.511TH/s` | The averaged hash performance since the start of the mining software or since the last reset of the statistics.
`A:167502850` | Number of shares accepted (`a`ccepted)
`R:116442` | Number of shares rejected (`r`ejected)
`HW:80256` | Number of Hardware-Errors
`1: GSF 10070001: BM1397:06+` | Miner on slot `1` with serial number `10070001` mi6 `6` ASICS of chipset `BM1397`.
`450.00MHz T:450 P:450` | ASIC-Frequency and Target-Frequency
`(2:2)` | Speed with which shares are routed to the ASICs in ms
`100% WU: 83%` | Efficiency indicator `WU` is the number of work units, i.e. shares per minute (accepted vs. rejected).

## GUI - Sub menues

From the main menu, pressing the key for the respective letter takes you to further submenus for extended display of data or for further configuration.

Keyboard-Shortcut | Description
------------------|-------------
`U` | (`U`)SB management 
`P` | (`P`)ool management: Pools can be added or deleted here, and quotas and priorities can be managed.
`S` | (`S`)ettings: Reset the mining software and write a config file.
`D` | (`D`)isplay: Expand and reduce the information displayed.
`Q` | (`Q`)uit cgminer, close process

I have marked the (for me) most relevant shortcuts in color in the screenshots.

### (`U`)SB management

<img src=".assets/cgminer_GUI_USB_highlighted.png" alt="cgminer_GUI_USB_highlighted.png" width="100%" />

### (`P`)ool management

<img src=".assets/cgminer_GUI_Pool_highlighted.png" alt="cgminer_GUI_Pool_highlighted.png" width="100%" />

Very useful menu together with the (`S`)ettings menu to create multiple pools, set quotas and priorities and then write them directly to the config file. The quotas can be chosen arbitrarily, for example a quota from pool1 to pool2 of `3:1` is equal to `75:25` (if you prefer to express it in %).

The priority indicates which pool should be mined first in the event of an error.

### (`S`)ettings

<img src=".assets/cgminer_GUI_Settings_highlighted.png" alt="cgminer_GUI_Settings_highlighted.png" width="100%" />

(`W`)rite config file: Create/overwrite the configuration file

(`C`)gminer restart: Reset the mining software and reset the statistics. The reset also resets the numbering of the miners, this must be taken into account for some API calls.

### (`D`)isplay

<img src=".assets/cgminer_GUI_Display_highlighted.png" alt="cgminer_GUI_Display_highlighted.png" width="100%" />

(`Z`)ero statistics: Resetting the statistics is useful when reconfiguring the miner so that, for example, all averaged values are recalculated.

---

####  [⛏ Start Mining Software](start_mining.md)  ᐊ  previous | next  ᐅ  [Mining Software - Advanced Configuration](EnhancedConfiguration.md)
