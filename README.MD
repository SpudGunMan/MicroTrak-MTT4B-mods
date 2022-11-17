# Micro-Trak MTT4/BT/FA-mods

MTT4BT-FA, This MicroTrak tracker line combines the TinyTrak4 with an internal bluetooth module. With a supplied GPS and built in ~14-watt frequency agile VFO / 2-meter (144 MHz - 148 MHz)transmitter. its similar looking components to the [Micro-Trak RTG FA](https://github.com/SpudGunMan/Micro-TrakRTGFA-mods), turned to '11' and the PIC, is a old friend the DIP ATMEGA128.

![hardware](img/mtt4b.jpg)

## Cables
make sure to buy the following cables, in addition to a GPS. (not included but, kinda needed)
- USB-25mm, and a TT-USB

Comes with a PP-RTG which a GPS or null modem PC will connect to. 
- PP-RTG cable which will connect 12v and GPS.
- See the [RTG](https://github.com/SpudGunMan/Micro-TrakRTGFA-mods#gps) for notes of GPS

## QuickStart
TT4 Quick Start [Guide](tt4/TinyTrak4_Quick_Start_Guide_v0.7.pdf) has the best where to start. This document builds on the little brother [Micro-Trak RTG](https://github.com/SpudGunMan/Micro-TrakRTGFA-mods) be sure to review it for foundational not covered here, covers the basics of sensors and Telemetry feeds to aprs.fi. As well as quick starts for setting those parts up. This document will focus more on the TNC aspects and added hardware i/o

if you have the TT-25 and/or TT-USB cables and jump down to [Connecting](#connecting-via-serial) otherwise skip a section down to [Ports](#ports) to figure out how to connect.

### LED Indicators
On application of 6-12v there is a green and red stand-off LED's which will alternate twice, the red LED is a CPU running watchdog and TX/PTT indicator along with audible relay click, the Yellow is RX traffic. The green is GPS status: blink acquire, locked solid. Additionally with the cover off is D8 and D9, showing HC-05 BT Module LED.

## Documentation
This device's manuals are a bit thick, as they build from years of the TT4 enhancement add-ons.

 - The [Manual](doc/MTT4B_Manual_v1.0.pdf) and hardware guide and a list of TNC commands, a [Mini Addendum](doc/MTT4B-Mini_Addendum.pdf) if you have the stamp sized version of the MTT4B. 

 - The [addendum](doc/MTT4BT_Manual_Addendum.pdf) has hardware configs and Bluetooth settings for the big version, which also references this BT chip, [HC05BT](doc/HC05BT.pdf). Also as well, the firmware [guide](doc/TinyTrak4_Firmware_Manual_v1.1.pdf) for flashing. And the tt4 firmware [manual](tt4/TinyTrak4_Alpha_Firmware_Manual_v0.72.pdf) with more details on software as well!

 - not done yet! next up is the extra stuff, the operation of (but otherwise different hardware) the [display](tt4/tt4_display_v1.3.pdf) as well as wire [guide](tt4/tt4_display_wiring.pdf) applied here as well.

 - after you consumed all that, don't forget the tt4 hardware [build](tt4/TinyTrak4_Built_Hardware_Manual_v8.pdf) and & MTT4B [Schematics](doc/MTT4BTv1.7Schematic.pdf) for further details on the parts not covered in the others... whew!

# Ports

## PORT B, the DB-9 
The power/serial cable, is generally used to connect a GPS. It is wired "backwards" in respect to a normal RS-232 connector, so you don't have to use a Null Modem adapter to connect a standard GPS receiver. (like all devices)

- Typical settings, have PORT-B 4800 Baud NMEA GPS. 
- All the ports on a TT4/MTT4 start up at 19200 and TEXT, and then switch to whatever else you may have set
- The TT-USB cable will supply the 5 Volt power needed to light up the ATMEGA on the MTT4BT.
  - This keeps you from having to connect it to external power and subsequently having to connect the unit to an antenna or dummy load. 
  - With the current generations of MTT4BT, you can configure the MTT4BT through this port using a serial terminal program. 
  - The GUI TT4 config won't work per factory, but did on mine shipped un-configured.

## PORT A, the 2.5mm Sub-Mini headphone
Be sure to pick up a, not included, Byonics USB-25mm cable, which is also a kenwood/serial/usb 2.5mm compatible cable and likely a lot of others like radio shack etc..

- You have to apply power to the MTT4BT to use this port
  - This can be done via the TT-USB cable connected to PORT-B
  - PP-RTG cable via PORT-B 12v, Don't blow up your transceiver, use a RF load dummy; whenever using direct 12 Volt power
- Official documentation states flash firmware with 2.5mm port
  - additionally needed for to access the BT module serial port.

## PORT A, Bluetooth

The MTT4BT makes PORT-A available either via (bluetooth OR 2.5mm stereo jack). 2.5mm jumpers on the MTT4BT PCB board select how PORTA is configured.

- Default jumpers set, PORT-A wired to the ATMEGA, and is identical to the original, non-­bluetooth version of the MTT4B
- Must change jumpers to change PORTA behavior (ATMEGA-serial or BT-serial)

The following image shows settings to enable the BT module. Open the device and change jumpers...

![enable](img/enable-bt.jpg)
I CHANGED MINE AND LOST A CHIP HOLD:STOP

image shows settings to enable BT module as PORT-A detailed in the [addendum](doc/MTT4BT_Manual_Addendum.pdf), 

also see BT module [config](https://github.com/SpudGunMan/MicroTrak-MTT4B-mods#enable-porta-configuration-to-bt-module) and BT [Connecting](#connecting-bluetooth) details below.

## HEADER for PS2, LCD

Byonics has details you can make your own LCD/Keyboard display [adapter](tt4/tt4_display_wiring.pdf). This adapter lets you send messages using a display and PS/2 keyboard, as well as changing most of the commonly used commands without having to connect to a PC. 
 - Recommend a "real" ps/2 keyboard. 
 - uses serial data for LCD not I2C, make sure to have removable I2C daughterboard.

 ![jp9](img/jp9.jpg)

MTT4B JP9 Header, comes populated just need `IDC 10pin 2x5 Socket 2.54mm, with Ribbon Cable`
 - [digikey](https://www.digikey.com/en/products/detail/cw-industries/C3AAS-1036G/1123111)

The key parameters for the display are:
|Command|Description|
| --- | --- |
NODISP FALSE|NODISP true means there is no display
TXTDISP TRUE|unknown

## Sensor VCC, SIG, GND
The MTT4B has two built in telemetry channels, one that reports the supply voltage to the unit, another, which reports the temperature inside the unit’s enclosure. Five additional telemetry channels are available for use ( PA3 through PA7 on J3) These inputs are capable of reading 0-5 Volt sensors and reporting the values over the APRS network.

![jp3](img/jp3.jpg)

JP3 Sensor Header, comes unpopulated, so you will need to add 2.5mm header pin's, included is 2.2k protection on each line.

VCC +5
- J3 Pin 2

Ground
- J3 Pin 1

***NOTE: JP9 Header, Byonics suggests avoiding that header for sensor use, used for LCD/PS2***
 - watch out JP9 and JP3 have reversed pin 1 and 2 5v/GND

# Connecting via serial
Using TerraTerm/Putty/screen you can connect to the TT-25 or the DB9 The TT-25/TT-USB Cable well supported in windows and linux. this document will focus on raspberry pi4 which is quickly adapted to any ubuntu(mint). The reason is development is more approachable in linux vs win 10,11 for most of our work here.

To setup AX25 and other packet apps check out my tool [bapi](https://github.com/SpudGunMan/bapi)

## TNC Options Menu / Text Mode
Install tools for connection
- windows [terra-term](https://osdn.net/projects/ttssh2/releases/) or [putty](https://www.putty.org)
  - Will be a COM port, cool for windows [serial](https://www.uwe-sieber.de/ComPortMan_e.html)
- debian linux `$ sudo apt-get install screen`
  - optionaly `$ sudo apt-get install putty` if you need more of a GUI serial emulator

linux kernel TT-25 cable will show up as 
`/dev/serial/by-id/usb-Prolific_Technology_Inc._USB-Serial_Controller-if00-port0`

using screen: `$ screen /dev/ttyUSB0 19200`
- to [exit](https://www.gnu.org/software/screen/manual/screen.html#Detach) screen Ctrl-a d (or) Ctrl-a Ctrl-d
- to re-attach `$ screen -r`

to exit KISS for example..
- apply power
- press ESC three times and you have a common TNC: prompt `AMODE TEXT`

## TNC KISS mode
apply power
press ESC three times and you have a common TNC: prompt you can 
- flip to KISS mode with `AMODE KISS`
```
:AMODE KISS
AMODE was TEXT
AMODE is KISS
:QUIT
```

### APRS Software
- [XAStir](https://xastir.org/index.php/Main_Page)
  - ![xastir](img/xastir.jpg)
### Packet Radio Terminal
- [QtTermTCP](https://www.cantab.net/users/john.wiseman/Documents/QtTermTCP.html)
- any more good ones?? [linpac](https://groups.io/g/KM4ACK-Pi/topic/linpac_for_direwolf_tnc/78591198?p=) needs ax.25 on the to do list

kiss2TCP stuff havent tested, any ideas or additions anyone?
- [TNCAttach](https://github.com/markqvist/tncattach) kissattach without ax stack.
  - `$ sudo tncattach /dev/ttyUSB0 19200 -e --mtu 575 --noipv6 --ipv4 127.0.0.2/8`
- [ShareTNC](https://github.com/trasukg/share-tnc)
  - `$ share-tnc /dev/ttyUSB0 8000 --baud=19200`

# Connecting Bluetooth
Needed packages on Raspian/Debian on a raspberry pi (default in new installs) `$ sudo apt-get install bluetooth bluez blueman`

On the linux box, navigate to new bluetooth interface in menu bar. Turn MTT4B on with BT module jumpers [enabled](https://github.com/SpudGunMan/MicroTrak-MTT4B-mods#port-a-bluetooth) and make sure your HC-05 module is blinking red you should see a pair connection named MTT4BT on raspberry pi. Password is 1234
- Add new device, pair connection named MTT4BT on raspberry pi. 
- Password is 1234

If you see device, HC-05 the module may need programming see [more](https://github.com/SpudGunMan/MicroTrak-MTT4B-mods#enable-porta-configuration-to-bt-module) details below.

Should see a serial port which you can connect with TNC applications, depending on the mode of PORTA (KISS or TEXT) you can now access your TNC.

Example, Share TNC KISS via TCP, https://github.com/trasukg/share-tnc

# Software
Need to improve add a full table:

The TNC commands available in the MTT4B are largely identical to the [commands](doc/MTT4B_Manual_v1.0.pdf) in the standard tt4 firmware [manual](tt4/TinyTrak4_Alpha_Firmware_Manual_v0.72.pdf) which has more detailed commands. One major difference is that the transmit and receive frequency are set in software, and by default are both on 144.390 MHZ. You can have a split frequency or a simplex frequency without problems. To set the frequency from the command line of the TNC (text mode)
 - TXFREQ 144.390
 - RXFREQ 144.390

After you have made any changes, you can either type “quit”, recycle power, or push the reset button on the PC board inside the unit. Your changes will now be in effect.

Device runs at 19200baud

The TEST44 is a bench-test ID entered during testing. If you did not request that Byon program your MTT4BT prior to shipping, it has that test callsign or the default NOCALL entered as callsigns entered in the main bank. 

 - The binary config [software](bin/TinyTrak4Config/TinyTrak4AlphaConfigv0.68.exe) is inside the Firmware v0.68.zip
 - Additionally you can just program like a classic TNC via text
   - config save files are just variable dump

## Firmware
Flashing is done via binary mode of a terminal emulator
 - Boot-up into `b` boot loader
    - press `s` to start binary transfer
    - blink blink .........* finished old school stuff.

## Enable PORTA Configuration to BT-Module
To change any settings on the BT [module](doc/HC05BT.pdf) (using the 2.5mm cable) change the jumpers to the following, you will likley need to source 1 "pc"-jumper for this task as not all shipped have 3 jumpers installed.


![btserial](img/bt-serial.jpg)
CAUTION I DID THIS AND LOST A CHIP STAND:BY

Set up a terminal-emulator at 19200 on your PC, With MTT4BT power off, remove the J7 jumper shunt, power the MTT4BT, then place the J7 jumper shunt on the J7 header pins. 

In the terminal-emulator console enter `​AT` and press enter, and the module should respond with `O​K` ​If it does not, check that the terminal is sending CR/LF when enter is pressed. It may be possible that the baud rate is 38400 or 9600, or 4800 so also try those if you don't connect.

You can copy and paste the following, full block to "reset" your module to Byonics default config. Paste into your terminal-emulator. if you really mess up `AT+ORGL` will reset to 38400 baud and then past the following..

```
AT+NAME=MTT4BT
AT+PSWD=1234
AT+ADDR?
AT+UART:19200,1,0
```

additionally you can send from linux CLI
```
echo -e "AT+UART:19200,1,0\r" | picocom -b 38400 -qrx 1000 /dev/ttyUSB0
```

now, return to normal mode, remove J7 jumper shunt reset to the shown [BT-PORTA](https://github.com/SpudGunMan/MicroTrak-MTT4B-mods#port-a-bluetooth) mode after configuring. And [connect](#connecting-bluetooth)!

### Partial list of AT command table
| AT Command | Description |
| --- | --- |
|`AT+ORGL`|Slave mode, pin code :1234, device name: H-C-2010-06-01 ,Baud 38400
|`AT+UART?`|Read the current serial configuration
|`AT+UART:19200,1,0`|Set the serial configuration to 19200 baud, 1 stop bit, no parity
|`AT+NAME?`|Read the current device name
|`AT+PSWD?`|Read the current connection PIN/password
|`AT+RESET`|Reset
|`AT+VERSION?`|Version information
|`AT+ADDR?`|Bluetooth address: NAP: UAP : LAP
|`AT+ROLE?`|Check module mode, set with `AT+ROLE=` 0-slave 1-master 2-slaveloop
|`AT+CLASS?`|Check device class set with `AT+CLASS=`
|`AT+INQM?`|Check Query access patterns
|`AT+CMODE`|Check connect mode set with `AT+CMODE=` 0-connect fixed 1-connect any
|`AT+BIND?`|Check fixed address set with `AT+BIND=` 00:00:00:00:00:00
|`AT+IPSCAN?`|Check scan parameter
|`AT+SNIFF?`|Check SNIFF parameter
|`AT+RMAAD`|Delete All Authenticated Device
|`AT+ADCN?`|Get Authenticated Device Count
|`AT+MRAD?`|Most Recently Used Authenticated Device
|`AT+STATE?`|Get the module working state

remember to return to normal mode, reset to the shown [BT-PORTA](https://github.com/SpudGunMan/MicroTrak-MTT4B-mods#port-a-bluetooth) mode after configuring.

# Hardware mods
the following is all untested.

I noticed on my new in box unit on inspection that the PCB had thermal discoloring from the RF power driver. I added a thermal tape and a passive cooling fins. this is untested and currently should be of note this might have reverse and negative effects!

![cooling](img/cooling.jpg)

this is untested and currently should be of note this might have reverse and negative effects! (SWR and therm)


### Hi Power mod
a higher power RF chip can be used[ra30h1317m](https://www.mitsubishielectric-mesh.com/products/pdf/ra30h1317m.pdf)

There is a tiny set of contacts near the power trimmer pot that can be solder-bridged to provide the higher gate Voltage the the higher power amplifiers require.


# References
 - K7MHI linux software installer https://github.com/SpudGunMan/bapi
 - http://aprs.org/satt4.html
 - https://opensource.com/article/18/3/how-configure-aprs-igate-raspberry-pi
 - https://github.com/trasukg/share-tnc
 - [Micro-TrakRTGFA](https://github.com/SpudGunMan/Micro-TrakRTGFA-mods)
 - http://www.aprs.org/labsats/labs08/EA467-Telemetry08c.pdf

# Recognition
Thanks to Rob KB8RCO and Allen AF6OF
