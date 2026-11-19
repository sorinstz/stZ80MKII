# stZ80 MKII Hardware

**A real Z80, Amstrad CPC 464/6128 compatibility, and an FPGA platform for future cores such as the ZX Spectrum.**

The stZ80 MKII uses a physical Z80 CPU and a reprogrammable FPGA for video, memory banking, bus control, and peripherals. The board design stays fixed so FPGA improvements can run on boards people have already built.

## Hardware:

- **512 KiB SRAM + 512 KiB flash**, with 16 KiB paging and Dk'tronics-style RAM banking.
- **5 V Z80 bus and 3.3 V peripheral bus**, bridged by FPGA-controlled transceivers.
- **RGB332 VGA**, PS/2 keyboard input, and Atari-style DB9 joystick input.
- **YM2149F audio** with an audio jack and an LM386 internal speaker amplifier.
- **CompactFlash storage**, USB serial via FT232RL, and an I²C header for a DS3231 RTC module.
- **5 V USB-C power input** and a 50-pin CPC-style expansion header.


        +-------------------+   +---------------+   +------------------+    
        |   Power Supply    |   |               |   |  LED Indicator   |    
	      |      ( +5V )      |-->|               |-->| (Status output)  |
        +-------------------+   |               |   +------------------+
        +-------------------+   |               |   +------------------+
        |     Keyboard      |-->|               |-->|     Monitor      |
        |      (PS/2)       |   |               |   |      (VGA)       |
        +-------------------+   |               |   +------------------+
        +-------------------+   |               |   +------------------+
        |    Joystick       |-->|               |-->|     Speaker      |
        |     (9pin)        |   |               |   |   (Audio Out)    |
        +-------------------+   |    stZ80 MKII     +------------------+
        +-------------------+   |               |   +------------------+
        |  Power Switch     |-->|               |<->|  CF Card Storage |
        | (On/Off Control)  |   |               |   |   (CompactFlash) |
        +-------------------+   |               |   +------------------+
        +-------------------+   |               |   +------------------+
        |   Reset Button    |-->|               |<->|Expansion Devices |
        |   (User input)    |   |               |   | (Ext Connector)  |
        +-------------------+   |               |   +------------------+
        +-------------------+   |               |   +------------------+
        |  Serial Com       |<->|               |<->| I2C Peripherals  |
        | ( UART to USB )   |   |               |   |    (I2C bus)     |
        +-------------------+   +---------------+   +------------------+         
			    Fig.1 - Context diagram
							 
		
		                                            [miniITX 170mmX170mm]					 
       +----------------------------------------------------------------+          
       |O +-------------------------+   [P][D]+-+......................O|
       |  | EXPANSION CONNECTOR 50p |    leds | |                     . | 
       |  +-------------------------+         |I|                     . |
       | +-------------------+                |D|                     . |
       | |     ROM DIP-32    |                |E|  CF CARD ADAPTER    . |
       | +-------------------+                | |                     . |
       | +-------------------+     [74HCT32]  |4|                     . |                   
       | |     RAM DIP-32    |                |0|                     . |                      
       | +-------------------+     [74HCT14]  | |                     . |
       | +-----------------------+            +-+...................... |
       | |     Z80 CPU DIP-40    | [LVC245#1]   [LVS245#4]   [LVC245#5] |
       | +-----------------------+                                      |
       | +-+                       [LVC245#2]+-------------------------+|
       |=|[R]eset                            |:::::::::::::::::::::::::||
       | +-+                       [LVC245#3]|                         ||
       |+---------++-------------+           |   FPGA BOARD 2x48DIL    ||
       || VOLTAGE ||:    RTC    :|           |                         || 
       || REG 3V3 ||:   DS3231  :|           |:::::::::::::::::::::::::||
       |+---------++-------------+           +-------------------------+|          
       |O             +--------------------+           +-------------+  |
       |              |    SOUND YM2149F   |           | UART-to-USB |  |
       |              +--------------------+           +-------------+  |
       |                                                                |
       +----------------------------------------------------------------+
       | [ON/OFF] [USB-C]  [AUDIO]  [PS/2]   [JOY]   [VGA]            O |                                 
       +-+--------+-----------------------------------------------------+
	                  Fig.2 - Layout & Fig.3 - Rear view

                          Fig.4 - Block diagram

                          Fig.5 - FPGA DEV Brakeout Board
		
	
        +---------+-----+----------++----------+-----+----------+
        | SIGNAL  | pin | fpga pin || fpga pin | pin |  SIGNAL  |
        +---------+-----+----------++----------+-----+----------+
        | +3.3V   |  1  |    -     ||    -     |  2  | +3.3V    |
        | GND     |  3  |    -     ||    -     |  4  | GND      |
        | FA0     |  5  |  IO11    ||  IO10    |  6  | FA1      |
        | FA2     |  7  |  IO7     ||  IO3     |  8  | FA3      |
        | FA4     |  9  |  IO2     ||  IO1     | 10  | FA5      |
        | FA6     | 11  |  IO144   ||  IO143   | 12  | FA7      |
        | FM1     | 13  |  IO142   ||  IO141   | 14  | FRST     |
        | FMWR    | 15  |  IO138   ||  IO137   | 16  | FMRD     |
        | FA9     | 17  |  IO136   ||  IO135   | 18  | FA8      |
        | SDA     | 19  |  IO133   ||  IO132   | 20  | FA10     |
        | PS2C2   | 21  |  IO129   ||  IO128   | 22  | SCL      |
        | PS2C    | 23  |  IO127   ||  IO126   | 24  | PS2D2    |
        | GL_YMCLK| 25  |  IO125   ||  IO124   | 26  | PS2D     |
        | /RAMEN  | 27  |  IO121   ||  IO120   | 28  | GL_CPUCLK|
        | BC1     | 29  |  IO119   ||  IO115   | 30  | /ROMEN   |
        | /ROM_WR | 31  |  IO114   ||  IO113   | 32  | BDIR     |
        | RAM0    | 33  |  IO112   ||  IO111   | 34  | RAM2     |
        | FIOWR   | 35  |  IO110   ||  IO106   | 36  | RAM1     |
        | CFA1    | 37  |  IO105   ||  IO104   | 38  | FIORD    |
        | CFA0    | 39  |  IO103   ||  IO101   | 40  | CFA2     |
        | GL_CF   | 41  |  IO100   ||  IO99    | 42  | RAM4     |
        | RAM3    | 43  |  IO98    ||  IO23    | 44  | RDIN     |
        | FEXP    | 45  |  IO24    ||  IO25    | 46  | NC       |
        | GND     | 47  |    -     ||    -     | 48  | GND      |
        +---------+-----+----------++----------+-----+----------+

        +---------+-----+----------++----------+-----+----------+
        | SIGNAL  | pin | fpga pin || fpga pin | pin | SIGNAL   |
        +---------+-----+----------++----------+-----+----------+
        | H5V     |  1  |    -     ||    -     |  2  | H5V      |
        | GND     |  3  |    -     ||    -     |  4  | GND      |
        | LDOE    |  5  |  IO30    ||  IO28    |  6  | LDIR     |
        | YMD0    |  7  |  IO32    ||  IO31    |  8  | YMD1     |
        | YMD2    |  9  |  IO34    ||  IO33    | 10  | YMD3     |
        | YMD4    | 11  |  IO39    ||  IO38    | 12  | YMD5     |
        | YMD6    | 13  |  IO43    ||  IO42    | 14  | YMD7     |
        | GL_WAIT | 15  |  IO46    ||  IO44    | 16  | GL_INT   |
        | BLU0    | 17  |  IO50    ||  IO49    | 18  | BLU1     |
        | GRN2    | 19  |  IO52    ||  IO51    | 20  | GRN1     |
        | VSYNC   | 21  |  IO54    ||  IO53    | 22  | GRN0     |
        | RED0    | 23  |  IO58    ||  IO55    | 24  | HSYNC    |
        | RED1    | 25  |  IO60    ||  IO59    | 26  | RED2     |
        | DDIR    | 27  |  IO65    ||  IO64    | 28  | FD4      |
        | FD3     | 29  |  IO67    ||  IO66    | 30  | FD5      |
        | FD6     | 31  |  IO69    ||  IO68    | 32  | FD2      |
        | FD7     | 33  |  IO71    ||  IO70    | 34  | FD0      |
        | FD1     | 35  |  IO73    ||  IO72    | 36  | GL_RST   |
        | DOE     | 37  |  IO75    ||  IO74    | 38  | FHLT     |
        | FMREQ   | 39  |  IO77    ||  IO76    | 40  | FA11     |
        | FA12    | 41  |  IO83    ||  IO80    | 42  | FA13     |
        | FA14    | 43  |  IO85    ||  IO84    | 44  | FA15     |
        | FMIORQ  | 45  |  IO87    ||  IO86    | 46  | TDOUT    |
        | GND     | 47  |    -     ||    -     | 48  | GND      |
        +---------+-----+----------++----------+-----+----------+

                        Fig.6 - FPGA Connector

        +----------------+--------++--------+----------------+
        | SIGNAL         | conpin || conpin | SIGNAL         |
        +----------------+--------++--------+----------------+
        | GND            |   1    ||   2    | CPUCLK         |
        | NC             |   3    ||   4    | /EXP           |
        | RAMDIS         |   5    ||   6    | NC             |
        | ROMDIS         |   7    ||   8    | /RAMEN         |
        | /RESET         |   9    ||  10    | /ROMEN         |
        | /WAIT          |  11    ||  12    | NC             |
        | /BUSREQ        |  13    ||  14    | /BUSACK        |
        | /INT           |  15    ||  16    | /NMI           |
        | /WR            |  17    ||  18    | /HALT          |
        | /IORQ          |  19    ||  20    | /RD            |
        | /M1            |  21    ||  22    | /RFSH          |
        | +5V            |  23    ||  24    | /MREQ          |
        | D1             |  25    ||  26    | D0             |
        | D3             |  27    ||  28    | D2             |
        | D5             |  29    ||  30    | D4             |
        | D7             |  31    ||  32    | D6             |
        | A1             |  33    ||  34    | A0             |
        | A3             |  35    ||  36    | A2             |
        | A5             |  37    ||  38    | A4             |
        | A7             |  39    ||  40    | A6             |
        | A9             |  41    ||  42    | A8             |
        | A11            |  43    ||  44    | A10            |
        | A13            |  45    ||  46    | A12            |
        | A15            |  47    ||  48    | A14            |
        | MONO           |  49    ||  50    | GND            |
        +----------------+--------++--------+----------------+

                Fig.7 - Expansion Connector

## CPC compatibility

The CPC core supports **CP/M and SymbOS**, USB serial auto-type, and CompactFlash-backed floppy emulation for **Standard DSK images**, including interleaved Vendor and Data layouts. Extended DSK and cassette are not supported. The ZX Spectrum is a future core target, not a feature of this release.

## Build and license

Use the **Gerber and drill files** for fabrication and the **schematic PDF** for reference. Editable PCB and schematic files are not included. Match your FPGA build to your board revision.

You may build and sell unchanged boards with attribution: **“stZ80 MKII hardware designed by Sorin Strizu.”** FPGA improvements are welcome as long as they work without hardware changes. Please submit hardware fixes for review rather than publishing a modified board.

See license.md for the full terms.
