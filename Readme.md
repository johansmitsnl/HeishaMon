﻿

## Panasonic H Series Aquarea air-water protocol

CN-CNT TTL UART 9600,8,E,1 
Pin-out (from top to bottom)
1 - +5V (250mV) 
2 - 0-5V TX 
3 - 0-5 RX 
4 - +12V (250mV) 
5 - GND

CN-CNT female connector PAP-05V-S - JST Female Connector Housing - PA, 2mm Pitch, 5 Way, 1 Row 
Pre-made crimp leads 150 mm crimp-free end lead PA 2.0 can be used.
The Panasonic CZ-TAW1 appears to use a JST B05B-XASK-1 male header.

## Protocol info packet:

To get information from a heat pump, "magic" packet should be send to CN-CNT:

71 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 12

## Protocol bit decrytp info:


7 bit 62=2 52=1 Tank [1:Off, 2:On]
39 bit 80 (123-128) = -5C (-128)Zone1: (water shift/water/room/pool) set temperature for heat mode [°C]
40 bit 8f -128 Zone1: (water shift/water/room) set temperature for cool mode [°C]
41 bit 80 (-128) Zone2: (water shift/water/room/pool) set temperature for heat mode [°C]
42 bit 8a -128 Zone2: (water shift/water/room) set temperature for cool  mode [°C]
43 bit b1 = 49 (177-128) DHW set water temperature Tank water set temperature [°C]
76 bit 9e = 30 (158-130) Heating curve outlet water temperature highest set [°C]
77 bit 96 = 22 (150-128) Heating curve outlet water temperature lowest set [°C]
78 bit 71 = -15 (113-128) Heating curve outside temperature lowest set [°C]
79 bit 8f = 15 (143-128) Heating curve outside temperature highest set °C]
112 bit 56/55 56=tank 55=room 3-way valve [1:Room, 2:Tank]
140 bit b0 (-128) Zone1: Actual (water outlet/room/pool) temperature [°C]
141 bit 00 -128 Zone2: Actual (water outlet/room/pool) temperature [°C]
142 bit aa -128 Actual tank temperature [°C]
143 bit 7c -128 Actual outdoor temperature [°C]
144 bit ab -128 Inlet water temperature [°C]
145 bit b0 -128 Outlet water temperature [°C]
146 bit 32 -128 Zone1: Water temperature [°C]
147 bit 32 -128 Zone2: Water temperature [°C]
148 bit 9c -128 Zone1: Water temperature (Target) [°C]
149 bit b6 -128 Zone2: Water temperature (Target) [°C]
150 bit 32 -128 Buffer tank: Water temperature [°C]
151 bit 32 -128 Solar: Water temperature [°C]
152 bit 32 -128 Pool: Water temperature [°C]
153 bit 80 ?
154 bit b7 -128 Outlet water temperature (Target) [°C]
155 bit af -128 Outlet 2 temperature [°C]
156 bit cd -128 Discharge temperature [°C]
157 bit 9a -128 Room thermostat internal sensor temperature [°C]
158 bit ac -128 Indoor piping temperature [°C]
159 bit 79 -128 Outdoor piping temperature [°C]
160 bit 80 -128 Defrost temperature [°C]
161 bit 77 -128 EVA outlet temperature [°C]
162 bit 80 -128 Bypass outlet temperature [°C]
163 bit ff -128 IPM temperature [°C]
164 bit 91 (145-1)/5 High pressure [kgf/cm2]
165 bit 1 -1 Low pressure [kgf/cm2]
166 bit 29 (x-1)/5 Outdoor current [A]
167 bit 59 x-1 Compressor frequency [Hz]
168 bit 00 - ?
169 bit 00- ? releated to pump flow
170 bit 2nd value , = (x -1)/5 x 2 = ,08 Pump flow rate [L/min]
171 bit 1st value Pump flow rate [L/min]
172 bit 1c (x-1) x 100 /2 - Pump speed [r/min]
173 bit 51 x-1 - Pump duty [duty]
174 bit 59 (x-1) x10  - Fan motor speed 1 [r/min]
175 bit 01  x-1  - Fan motor speed 2 [r/min]
176 bit 



To get decimal values you must convert from hexadecimal and do some calulation depending on value. Most of them need just -128(DEC).
As example 43 bit value to get DHW tank water set temperature b1 (HEX) = 177(DEC) - 128 = 49 C 
Panasonic query, answer and commands are using 8-bit Checksum (CRC-8) to verify serial data. Last bit is checksum value.


## Query Examples:

Panasonic query:

71 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 12

Panasonic answer examlpe:

71 c8 01 10 56 55 62 49 00 05 00 00 00 00 00 00 00 00 00 00 19 15 11 55 16 5e 55 05 09 00 00 00
00 00 00 00 00 00 80 8f 80 8a b2 71 71 97 99 00 00 00 00 00 00 00 00 00 00 00 80 85 15 8a 85 85
d0 7b 78 1f 7e 1f 1f 79 79 8d 8d 9e 96 71 8f b7 a3 7b 8f 8e 85 80 8f 8a 94 9e 8a 8a 94 9e 82 90
8b 05 65 78 c1 0b 00 00 00 00 00 00 00 00 55 56 55 21 53 15 5a 05 12 12 19 00 00 00 00 00 00 00
00 e2 ce 0d 71 81 72 ce 0c 92 81 b0 00 aa 7c ab b0 32 32 9c b6 32 32 32 80 b7 af cd 9a ac 79 80
77 80 ff 91 01 29 59 00 00 3b 0b 1c 51 59 01 36 79 01 01 c3 02 00 dd 02 00 05 00 00 01 00 00 06
01 01 01 01 01 0a 14 00 00 00 77

Quiet mode 1

f1 6c 01 10 00 00 00 10 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 82

Quiet mode 2

f1 6c 01 10 00 00 00 18 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 7a

Quiet mode 3

f1 6c 01 10 00 00 00 20 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 72


Quiet off

f1 6c 01 10 00 00 00 08 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 8a

Set -2C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7e 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 14

set -1C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7f 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 13

Set 0C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 12

set -3C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7d 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 15

set -4C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7c 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 16

Set -5C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7b 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 17

set +1C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 81 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 11

set +2C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 82 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 10

set +3C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 83 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 0f

set +4C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 84 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 0e

set +5C

f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 85 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 0d

cool mode

f1 6c 01 10 00 00 03 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 8f

auto mode

f1 6c 01 10 00 00 08 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 8a

heat mode

f1 6c 01 10 00 00 02 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 90

heat off

f1 6c 01 10 02 00 21 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 6f

heat on

f1 6c 01 10 02 00 62 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 2e










