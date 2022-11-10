These are the Alpha firmware version 0.68 files for the Byonics TinyTrak4.

The file tt4_alpha_v068_644.TT4 is for ATMEGA644P based TinyTrak4s with
bootloader version 1.2b, for kits or builts sold before July 2010.

The file tt4_alpha_v068_1284.TT4 is for ATMEGA1284P based TinyTrak4s with
bootloader version 1.2c, for builts sold after June 2010.

To load the firmware, use the TinyTrtak4 Alpha Config program, or
follow the instructions in the TinyTrak4 Firmware Manual

Please send comments to support @ byonics . com

Changes in v0.68
----------------------
Fixed Wind Speed Errors
Added DECSTAT to show incoming audio level and decoded TXD length for received packets.
Fixed MIC-E long decoding bug
Added support for AvMap weather reporting
Added DIGIMY setting to enable digipeating of MYCALL
Default AMODE & BMODE to GPS
Stopped digipeating unused WIDE2-0 (bugfix for KPC-3)
Send CR LF in GKRELAY
Added PASSTHRU command to help setting TT4BT settings

Changes in v0.67
----------------------
Fixed bug with GPS valid detection
Firmware manual and Config software do not require changes from v0.66

Changes in v0.66
----------------------
Added delay before considering gps valid
Made SOFTRST reset to bootloader
Made RXA, AMODE, etc take when entered by keyboard
Added MTXD and MPPER for PTT IN Support
Added LRNTPS to make TPS auto learning
Added GPSCHK to validate Checksum
Added INTCLK to force internal clock timing
Added GKRELAY to send GPS data to KISS port
Made C0 FF C0 work for SOFTRST in KISS


Changes in v0.65
----------------------
Added support for the ATMEGA1284P (1284) TinyTrak4


