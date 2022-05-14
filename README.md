# MacOS
tips and tricks for MacOS users

# Migrating from Windows to MacOS
## Why?
not sure yet. So far Windows 10/11 looks more advanced and usable then MacOS.
As a security guy I should stick to tools/equipment used by majority of my herd -> let's try Mac

## Installation & Setup tricks
1. Use "Russian - PC" keyboard layout instead of just "Russian"

## Usefull tools
1. [AltTab](https://github.com/lwouis/alt-tab-macos) - adds Windowds "Win + Tab" experience to MacOS
2. [Rectangle](https://github.com/rxhanson/Rectangle) - normal "full screen" windows resizing 
3. [Scroll Reverser](https://github.com/pilotmoon/Scroll-Reverser) - allows you to have different scroll directions for mouse and touchpad. *Logitech Options* can aslo do that if you have a logitech mouse.
4. [Fig](https://github.com/withfig/autocomplete) - adds autocomplete to MacOS's terminal
5. [Karabiner-Elements](https://github.com/pqrs-org/Karabiner-Elements) - keyboard customization in Mac. Most usefull app because it allows you to use familiar shortcuts (like Ctrl+C, Shift+End, Ctrl+Alt+T..) on Mac

## Keyboard
I use Microsoft Windows keyboard. It works fine, but you really need to remap some keys
1. Install Karabiner-Elements
2. Copy ```karabiner*.json``` files from this repo to  ```.config/karabiner/assets/complex_modifications/```
3. Open Karabiner-Elements, goto "Complex modifications" tab. Click ```Add rule``` at the bottom left corner and you should see new rules there. Enable & Enjoy!

## Security
1. enable firewall via settings
2. enable FileVault (full disk encryption) via settings
3. for M1-based macs no need to seput firmware (NVRAM) password https://support.apple.com/en-us/HT204455
4. "Mac doesn't need an antivirus" <- I should say I agree with this statement for now.. I got used to much higher capabilities, performance, stability and AV usability on Windows systems then on MacOS (especially with M1 chips). I tried only Sophos for now and it's a disaster. It basically broke my system and I was forced to clean it manually because removal tool was also broken.

### Modifying system extentions
You can list *system extensions* (it's like *services* in Windows) using ```systemextensionsctl list```. 

If you need to remove some of them you need to disable the [System Integrity Protection](https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection) first. 

On Apple-silicon macs you need to hold the TouchID button during boot to enter the Recovery Mode. Then go to "Options", pick a user and run the Terminal via the top menu bar. Disable SIP with ```csrutil disable``` & reboot. Use ```systemextensionsctl uninstall <teamId> <bundleId>``` to remove the extention. Reenable SIP using ```csrutil enable``` in the Recovery Mode again.

## Tips and tricks
For multiscreen setup: if your dock jumps from one screen to another - go to the main one and gently move your mouse down in the center of the screen. Dock should appear now. 

You can also remove this checkbox but it will brake other stuff: go to ```System Preferences > Mission control``` and remove "Displays have separate spaces"

Clear Dock - ```defaults write com.apple.dock ResetLaunchPad -bool true; killall Dock```

Screenshot location - ```defaults write com.apple.screencapture location ~/Desktop/screenshots/```

```defaults write com.apple.desktopservices DSDontWriteNetworkStores true``` No ".DS_store" files everywhere
### make sure "home" and "end" work in Terminal
1. Open Terminal preferences
2. "Profiles" -> "Keyboard". Add new rules for "End" and "home" button. Map "End" to Ctrl+E and "Home" to Ctrl+A

## Issues
1. Not really a Mac issue, but keep in mind: slow USB-C speed with some monitors when USB-c connection to Mac is used. Monitor's built-in USB hub is usually a low speed one
2. USB keyboard doesn't work on Mac's login screen when system was rebooted

## links
1. https://habr.com/ru/post/588380/
