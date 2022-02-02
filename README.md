# MacOS
tips and tricks for MacOS users

# Migrating from Windows to MacOS
## Why?
not sure yet. So far Windows 10/11 looks more advanced and usable then MacOS. But time will tell...

## Installation & Setup
1. Use "Russian - PC" keyboard layout instead on just "Russian"

## Usefull tools
1. [AltTab](https://github.com/lwouis/alt-tab-macos) - adds Windowds "Win + Tab" experience to MacOS
2. [Rectangle](https://github.com/rxhanson/Rectangle) - normal "full screen" windows resizing 
3. [Scroll Reverser](https://github.com/pilotmoon/Scroll-Reverser) - allows you to have different scroll directions for mouse and touchpad. *Logitech Options* can aslo do that if you have a logitech mouse.
4. [Fig](https://github.com/withfig/autocomplete) - adds autocomplete to MacOS's terminal
5. [Karabiner-Elements](https://github.com/pqrs-org/Karabiner-Elements) - keyboard customization in Mac. 

## Keyboard
I sue Microsoft Windows keyboard. It works fine, but you really need to remap some keys
1. Install Karabiner-Elements
2. Copy ```*.json``` files from this repo to  ```.config/karabiner/assets/complex_modifications/```
3. Open Karabiner-Elements, goto "Complex modifications" tab. Click ```Add rule``` at the bottom left corner and you should see new rules there. Enable & Enjoy!

## Security
1. enable firewall via settings
2. enable FileVault (full disk encryption) via settings
3. for M1-based macs no need to seput firmware (NVRAM) password https://support.apple.com/en-us/HT204455

## Issues
1. Unstable wifi6. By HP 840 g2 win11 wifi reception was better. Solution - switch to wire (external Ethernet-USB adapter is needed)
2. Slow USB-C speed with some displays/hubs. We carefull, many hubs/displays who offer USB-c connection to MBP have USB2.0 hubs. Solution - external separate USB-C to USB-A hub 

## links
1. https://habr.com/ru/post/588380/
