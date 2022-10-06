# I want to show pictures on an epaper device via MS Windows.

## We need to flash the cc2531 and the epaper device

### Target tools:
 * one cc2531 usb dongle

 * one SOLUM ST-GR29000 2,9" 128x296 pixel bwr (black-white-red)

### For flashing:
 * one or two (two is more convinient) esp8266 boards. I choosed the Wemos D1 Mini,
   because I have a ton of them.

 * some 2 mm 2 row female depont headers.
   It works with single row headers and some bending. It works with direct soldering but is
   not nice at all.

 * some wires, soldering tools, first aid box   

* the universal wisdom tip
   Get yourself a number of "Maxibriefkartons" 350 x 250 x 50 mm or 
   "Klappdeckelkarton" 200 x 150 x 100 mm. They are cheap and great for storing 
   your projects vertically.

I assume that you have any Arduino IDE (1.8.x or 2.x) and the esp8266 
board installed.

Hint: All devices are 3.3 volt devices. So we need 3.3 v stuff to work with.

### Step 1: flash the cc2531

- get the TIMAC-Firmware for cc2531

  It can be downloaded directly from https://www.klosko.net/tools/ePaper/MACcoP-cc2531.hex
  If that location fails:
  It is part of the Texas Instruments TIMAC - IEEE802.15.4 Medium Access Control (MAC) Software
  Stack (https://www.ti.com/tool/TIMAC). You need an account at TI and sign some export 
  forms....

- convert the .hex to a .bin file

  Objcopy is needed for that. I found one in my arduino avr folder.
```
  arduino\avr 4.9.2\avr\bin\objcopy.exe" --gap-fill 0xFF --pad-to 0x040000 -I ihex MACcoP-cc2531.hex -O binary MACcoP-cc2531.bin
```

- get the CCLoader

  It is at https://github.com/RedBearLab/CCLoader.

- Open Arduino\CCLoader\CCLoader.ino with the Arduino IDE and change at line 88:
```
int DD = 14;
int DC = 4;
int RESET = 5;
int LED = 2;
```
 (I configured a generic esp8266 board, 160mhz, 4mb). Compile, upload, no thrills.

 - build the connector

```
   +-------+           Connect cc2531 esp2866
   +       +                   DD     GPIO 14/D4
   +       +                   DC     GPIO 4/D2
   +oOOO   +                   Reset  GPIO 5/D1
   +oOOO sw+                   Gnd    Gnd
   +oOOO   +                   Vts    3V3
   +o      +                   3V3    3V3
   +o      +
   +o   12 +    1 GND     Vts 2
   +o   34 +    3 DC       DD 4
   +o   56 +    5 CS     SCLK 6  
   + sw 78 +    7 Reset  MOSI 8 
   +    90 +    9 3V3   MISO 10
   +       +
   + +---+ +
   +-!   !-+
     !USB!
     +---+
```     
 
 - upload the .bin file with ccloader

```
  CCLoader_x86_64.exe 3 MACcoP-cc2531.bin 0
```
  
  You might need to change the com port number (here: 3). You should see some messages 
  and the flashed blocks counting up.





