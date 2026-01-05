# iPhone
## iPhone MDM unlock
https://github.com/tazMeah/MDMPatcher-Universal

# MacOS
tips and tricks for MacOS users

## Installation & Setup tricks
1. Use "Russian - PC" keyboard layout instead of just "Russian"

## Usefull tools
- [Tabby](https://tabby.sh/) as a terminal mostly because of convinient SSH manager. 
- [Maccy](https://maccy.app/) as a clipboard manager. Press Option+V to get a list of last copied items (Analog of Win+V on Windows)
- [Scroll Reverser](https://github.com/pilotmoon/Scroll-Reverser) - allows you to have different scroll directions for mouse and touchpad. *Logitech Options* can aslo do that if you have a logitech mouse. Currently I use [Mos.app](https://mos.caldis.me/). Not sure which one is better... 
- [Karabiner-Elements](https://github.com/pqrs-org/Karabiner-Elements) - keyboard customization in Mac. Most usefull app because it allows you to use familiar shortcuts (like Ctrl+C, Shift+End, Ctrl+Alt+T..) on Mac. Update: I now got used to Mac's hotkeys. 

## Remap keys "Windows-style"

### Karabiner path
1. Install Karabiner-Elements
2. Copy ```karabiner*.json``` files from this repo to  ```.config/karabiner/assets/complex_modifications/```
3. Open Karabiner-Elements, goto "Complex modifications" tab. Click ```Add rule``` at the bottom left corner and you should see new rules there. Enable & Enjoy!
### Remap keys path
1. System Settings > Keyboard > Keyboard Shortcuts > Modifier Keys. Swap Control with Command keys.
2. create this file ```~/Library/KeyBindings/DefaultKeyBinding.Dict``` with this content:
```
{
    "\UF729"   = "moveToBeginningOfLine:";                       /* Home         */
    "~\UF702"  = "moveToBeginningOfLine:";                       /* Option + Left Arrow */
    "$\UF729"  = "moveToBeginningOfLineAndModifySelection:";     /* Shift + Home */

    "\UF72B"   = "moveToEndOfLine:";                             /* End          */
    "~\UF703"  = "moveToEndOfLine:";                             /* Option + Right Arrow */

    "$\UF72B"  = "moveToEndOfLineAndModifySelection:";           /* Shift + End  */
    "$~\UF703" = "moveToEndOfLineAndModifySelection:";           /* Shift + Option + Left Arrow  */

    "\UF72C"   = "pageUp:";                                      /* PageUp       */
    "\UF72D"   = "pageDown:";                                    /* PageDown     */

    "@\UF702"  = "moveWordBackward:";                            /* Cmd  + LeftArrow */
    "@\UF703"  = "moveWordForward:";                             /* Cmd  + RightArrow */

    "@$\UF702" = "moveWordBackwardAndModifySelection:";          /* Shift + Cmd  + Leftarrow */
    "@$\UF703" = "moveWordForwardAndModifySelection:";           /* Shift + Cmd  + Rightarrow */
}
```
.. or make [your own](https://gist.github.com/trusktr/1e5e516df4e8032cbc3d#file-defaultkeybinding-dict)


## Security
Apple's security platform guide [2]

1. enable firewall via settings
2. enable FileVault (full disk encryption) via settings

### Modifying system extentions
You can list *system extensions* using ```systemextensionsctl list```. 

If you need to remove some of them you need to disable the [System Integrity Protection](https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection) first. 

On Apple-silicon macs you need to hold the TouchID button during boot to enter the Recovery Mode. Then go to "Options", pick a user and run the Terminal via the top menu bar. Disable SIP with ```csrutil disable``` & reboot. Use ```systemextensionsctl uninstall <teamId> <bundleId>``` to remove the extention. Reenable SIP using ```csrutil enable``` in the Recovery Mode again.

## SSH
```bash
cat Downloads/router.pem > ~/.ssh/id_rsa
sudo chmod 0700 .ssh/
sudo chmod 0600 .ssh/id_rsa
```
+ export/import iTerm2 profiles.

## Tips and tricks
For multiscreen setup: if your dock jumps from one screen to another - go to the main one and gently move your mouse down in the center of the screen. Dock should appear now. 

You can also remove this checkbox but it will brake other stuff: go to ```System Preferences > Mission control``` and remove "Displays have separate spaces"

```bash
# Clear dock. Helps fight bugs such as non-existing icons
defaults write com.apple.dock ResetLaunchPad -bool true; killall Dock
# Change screenshot location
defaults write com.apple.screencapture location ~/Desktop/screenshots/
# stop writing ".DS_Store" files to all network folders
defaults write com.apple.desktopservices DSDontWriteNetworkStores true
# show all files
defaults write com.apple.Finder AppleShowAllFiles true
```
### Run "damaged" apps
aka remove quarantine flags
```bash
sudo xattr -rds com.apple.quarantine /Applications/Coherence\ Pro.app
```

### Make sure "home" and "end" work in Terminal
1. Open Terminal preferences
2. "Profiles" -> "Keyboard". Add new rules for "End" and "home" button. Map "End" to Ctrl+E and "Home" to Ctrl+A or follow [this article](https://medium.com/internetstack/making-the-apple-magic-keyboard-home-end-buttons-behave-like-windows-with-modifier-keys-7c070c09ab93) )

## Issues
1. Not really a Mac issue, but keep in mind: slow USB-C speed with some monitors when USB-c connection to Mac is used. Monitor's built-in USB hub is usually a low speed one. That means you can't have just one USB-C to connect Mac to all support devices.
2. USB keyboard doesn't work on Mac's login screen when system was rebooted

# Links
1. https://habr.com/ru/post/588380/
2. https://help.apple.com/pdf/security/en_US/apple-platform-security-guide.pdf
