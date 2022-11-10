# Micro-TrakMTT4B-mods
 
 MTT4BT, This MicroTrak tracker line combines the TinyTrak4 with an internal bluetooth module. With a supplied GPS and built in ~14-watt frequency agile VFO / 2-meter (144 MHz - 148 MHz)transmitter. its similar looking components to the [Micro-TrakRTGFA](https://github.com/SpudGunMan/Micro-TrakRTGFA-mods) for the RF deck, turned to '11'


## Documentation
This device's manuals are much harder to focus on as it builds from years of the TT4 enhancment addons.
Kicking it off!

 - The [Manual](doc/MTT4B_Manual_v1.0.pdf) which has Schematic's and hardware guide and a list of software commands, a [Mini Addendum](doc/MTT4B-Mini_Addendum.pdf) if you have the stamp sized version of the MTT4B. 

 - The [addendum](doc/MTT4BT_Manual_Addendum.pdf) has hardware configs and Bluetooth settings for the big version, which also references this BT chip, [HC05BT](doc/HC05BT.pdf). Also as well, the firmware [guide](doc/TinyTrak4_Firmware_Manual_v1.1.pdf) for flashing. And the tt4 firmware [manual](tt4/TinyTrak4_Alpha_Firmware_Manual_v0.72.pdf) with more details on software as well!

 - not done yet! next up is the extra stuff, the operation of the [display](tt4/tt4_display_v1.3.pdf) as well as wire [guide](tt4/tt4_display_wiring.pdf) for the tt4 but applied here as well.

 - after you consumed all that font forget the tt4 hardware [build](tt4/TinyTrak4_Built_Hardware_Manual_v8.pdf) for further details on the parts not covered in the others... whew!

-  Now that was a mouthfull and to say this github is a replacement for the tt4 Quick Start [Guide](tt4/TinyTrak4_Quick_Start_Guide_v0.7.pdf) I didnt mention yet is a understatment...

Truth be told the reason for this github is just sorting out the endless documentation I didnt want to do every time I try and change a setting...



## Software
 - The config [software](bin/TinyTrak4Config/TinyTrak4 Alpha Config v0.68.exe) is inside the TinyTrak4_Alpha_Firmware***_v0.68.zip***
 - Additionally you can just program like a classic TNC via text
   - config save files are just variable dump

### Firmware
Flashing is done via binary mode of a terminal emulator
 - Bootup into `b` boot loader
    - press `s` to start binary transfer
    - blink blink .........* finished old school stuff.











# Also See
Micro-TrakRTGFA [here](https://github.com/SpudGunMan/Micro-TrakRTGFA-mods)

# Recognition
Thanks to Rob KB8RCO and Allen AF6OF
