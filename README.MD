Micro-TrakRTGFA [moved](https://github.com/SpudGunMan/Micro-TrakRTGFA-mods)

now scratch for Micro-TrakMTT4B-mods

This device's manuals are much harder to focus on as it builds from years of the TT4 enhancment addons.

First the config software is inside the TinyTrak4_Alpha_Firmware_v0.68.zip

Flashing is done via binary mode of a terminal emulator but likely near a final rev.
 - Bootup into `b` boot loader
    - press `s` to start binary transfer
    - blink blink .........* finished old school stuff.










# Sensors
https://groups.io/g/TinyTrak/topic/77958326#494

JP5 has Vcc (5V) coming through R7 (2.2k ohms), so current limited.

So by jumper-ing R7 and removing R12, you could have 5V on one side of JP5 and ground on the other.
That provides a 10-pin (2x5) connector for 4 sensors with grounds with signal and ground.
If you don't use more than 3 sensors, you can still use JP1 for bank switching.
Again, much easier to do on the KIT version.

It appears the MTT4B has a straight 7-pin header with Vcc, Ground, and the 5 analog input pins.
It does not appear to have the resistor on Vcc.
That would be even easier for analog inputs!


 PA0 - UNAVAILABLE used for received audio
  PA1 - UNAVAILABLE used to monitor Battery Voltage
  PA2 - UNAVAILABLE used to monitor LM335Z temperature sensor
  PA3 - connect your analog sensor between 0 - 5V at JP1
  PA4 - connect your analog sensor between 0 - 5V at JP2
  PA5 - connect your analog sensor between 0 - 5V at JP3
  PA6 - connect your analog sensor between 0 - 5V at JP4
  PA7 - Available at JP5, but I don't recommend as this is connected to Vcc
Any and/or all can be reported in the status or beacon using ^V ^T (or^t), and ^# to indicate which to report.  The voltage readings for Battery and Temp and be sent with ^6 (Bat) and ^7 (Temp)

Only 5 values can be reported in the TELEMETRY message.   The catch to sending the data in Telemetry packets is that the pins reported change depend on the settings in of TELEVOLT and TELETEMP:
   TELEVOLT=FALSE TELETEMP=FALSE     T#201,JP5,JP4,JP3,JP2,JP1,{digital pins}
   TELEVOLT=TRUE TELETEMP=TRUE        T#201,JP6,JP7,JP5,JP4,JP3,{digital pins}
   TELEVOLT=TRUE TELETEMP=FALSE       T#201,JP6,JP5,JP4,JP3,JP2,{digital pins}
   TELEVOLT=FALSE TELETEMP=TRUE       T#201,JP7,JP5,JP4,JP3,JP2,{digital pins}

TPERIOD [ seconds ]
Sets the time in seconds between transmissions of a telemetry report. A setting of 0 will disable telemetry reports. The analog voltages sent in order are JP5, JP4, JP3, JP2,JP1. If TELVOLT and/or TELTEMP is true, those will be sent first, followed by JP5, and the rest, up to a total of 5.
 
>>> So with both true you will transmit: ^V ^T ^5 ^4 ^3 d0 d1 d2 d3 d4 d5 d6 d7

TPROTOCOL [ MIC-E | APRS | COMPRESSED ]
Sets the tracker position report format. Valid settings are MIC-E for the standard binary encoded short reports, APRS for the human readable text format, or COMPRESSED for the highly compressed and high resolution format

Plus, of course, the ones mentioned below. 
BTWm I do not believe you need to set TELEHIRES FALSE.  When true it the range is 0 - 999 which fits the updated standard (it always allowed 3 digits, so why not allow up to 9 in each digit).

Two other settings need a little extra effort:
1. TVOLTTWK - Measure the voltage at the TT4 connector and adjust this value to read properly.
2. TTEMPTWK - Adjust this to room temperature with the cover off and in open air to get it closer.

You can put teh temperature and voltage (or any input) into the beacon and status message as well - just to keep a closer eye on it.

As for getting the values in the Status or comments:
 BTEXT  /TinyTrak4 Alpha Bat=^V Temp=^T 
 
  and/or
  BTEXT  /TinyTrak4 Alpha  
  STATUSRATE 2        {>0 to add TSTAT text to BTEXT  note this will send Bat and Temp every other BTEXT}
  TSTAT Bat=^V Temp=^T

NOTE: it is always a good idea to puit a trailing space at the end of these.  That way when other info is added, it doesn't run together.  Better to have 2 spaces, than text running together (IMO)

JP5 has a 2,7k resistor connected to the power +5V , so depends on your sensor .I think JP4 to JP2 will be better .
Important question is this : Which kind of signal does your barometric sensor generate ? 0 to 5V ?


MTT4BT, you can tap +5 from pin 2 of J3.  Pin 1, the square pad, is ground. Note that although it looks like this pin is connected through a limiting resistor, it is not, it is connected directly to the switched 5 Volt regulators output. The resistor connecting to it is the bias for D3, the LM335 Temperature sensor.

Note that +5 is also available on JP9. ( The square hole is positive, violating all laws of god and man) But I suggest avoiding that pin, since you may want to install a jack for use with a Byonics ( Or homemade) LCD/Keyboard display adapter. This adapter lets you send messages using a display and PS/2 keyboard, as well as changing most of the commonly used commands without having to connect to a PC. A warning note is that USB keyboards with a PS/2 adapter may or may not work ( Mostly not) You need a "real" ps/2 keyboard. 

To do this directly with a MTT4B on J3 (pin1 Vcc to pin7 R15), I would probably go with something like this:
   Pin 2 ---------------------------------------------------------------------------------- Vcc
                                  |                                  |                                  |     
                                  |                                 Rt                                | 
                               CdS                      6------|                                Mic---------- additional Mic circuit ------ 5    
                      7 -------|                              TMP37                            |
                                 Rl                                 |                                 Rs     
   Pin 1 ---------------------------------------------------------------------------------- Gnd
Note:  Pin 7 (I believe) is JP5 of the TT4 and would be reported after TeleVolt, and TeleTemp. 
           Or the start of the telemetry data if those are OFF. 
In my project:
          Resistors Rl (light), Rt (Temp), and Rs (sound) were kind of current limiting protection (separate for each).
          The sensors were mounted on a piece of perf board with a cable going to the Mico board.
          That was so they could be 'aimed' to not be at a window, or noise source.
FYI, I actually used the tutorial from Adafruit as my guide for the CdS sensor:  https://learn.adafruit.com/photocells/using-a-photocell


PORT B, the DB-9 connector on the power/serial cable, is generally used to connect a GPS.It is wired "backwards" in respect to a normal RS-232 connector, so you don't have to use a Null Modem adapter to connect a standard GPS receiver. In the default settings, it is set up as a 4800 Baud NMEA GPS. (All the ports on a TT4/MTT4 start up at 19200 and TEXT, and then switch to whatever else you may have set) 

The TT-USB cable will supply the 5 Volt power needed to light up the low power parts of an MTT4BT. This keeps you from having to connect it to external power and subsequently having to connect the unit to an antenna or dummy load. With the current generations of MTT4BT, you can configure the MTT4BT through this port using a serial terminal program ( The GUI TT4 config won't work) 

To connect through PORT A using a Byonics USB-25mm cable, you have to apply power to the MTT4BT. This can be done via the USB cable connected to PORT B, or external power. ( Don't blow up your transceiver please, use a dummy load  whenever you are using direct 12 Volt power) 

The TEST44 is a bench-test ID entered during testing. If you did not request that Byon program your MTT4BT prior to shipping, it has that test callsign or the default NOCALL entered as callsigns entered in the main bank. 