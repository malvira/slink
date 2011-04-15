LED:
----

Cree:
XPGWHT-01-0000-00EE5

Digikey: XPGWHT-01-0000-00EE5CT-ND

+ eagle device done.

qty 1:   $5.43
qty 100: $4.80
gty 1000: $2.79 (TR)

Handles 50A 250us pulses --- breaks above 50A.
approx 7.6V for 1 LED

LED Configuration:
------------------

strings of 4 LEDs

+ 7.6V * 4 = approx 30V per string

4 strings in parallel in a checker board pattern

Allows for 100%, 75%, 50%, 25% LED population.

ESR adds 50mV per mOhm.

at 1% duty ratio about 2.5 mW average per mOhm.

need to measure LED characteristics to determine bypassing
requirements

+ 30mV is about 1A change in the LED.
+ i = C dV/dt
+ for 250us pulse, 40A = C * 0.1V / 250 uS
  C = 100mF (per LED).

338-2158-1-ND is 1.5mF 35V surface mount $2.56. One or two of these
per board.

+ eagle device done.

565-2915-ND is 6.8mF 63V $4.1, bank of these on the power supply.

FET/Drive circuit:
------------------

Bypassing for each string or board

Pulldown FET for each string or board

+ Test rig use FDP5800 $2.15 each 6mOhm @ 80A.
+ 785-1221-1-ND AOD4184A, $.79 ea 7mOhm @ 20A logic level drive
    - eagle device done
+ lay out one per string for now with rework option to test all
  strings on one.

Gate driver for each FET with gate resistor to control on/off
transients.

+ use TC4427, TC4426. Can only take 18V supply. Need a separate
    12V supply or so for the gate drivers.
    - eagle device done
+ might not need it for the AOD4184A, have a board option to
    bypass the gate driver.
+ laying it out now gives us the option to change the FETs.
+ Need reverse polarity protection on this supply.

Sense resistor for current test point.

+ MCS1632R010FER 10mOhm 1206 1W
  MCS1632R010FERCT-ND $0.35 ea
    - eagle device done
+ needs to be ground referenced. One per FET.

Trim resistance to balance each string or board.

+ 300mV trim will adjust about 10A
+ 1mOhm trim would do 30ish mV at 40A.
+ at 20A, 40mV on LED adjusts down 1A to 19A. (supply is 90mV).
    - that's a 1.5mOhm trim.
+ a 50mil trace that's 100mil long should be 1mOhm. 
    - we could do binary weights.
    - 24 mil is 2mOhm
    - 12 mil is 4mOhm
    - 6 mil is  8mOhm
+ use solder braid to short out segments for trim.

Overall current magnitude set by board rail voltage.

Fuses for each string or board.

+ 507-1186-1-ND 1A 1206 package $0.62 ea, slow blow
+ there are a bunch in 1206 --- might have to try different
  current ratings
+ 800% blows in min. 2ms max 50ms. That would be 40A. Must be per
string.
+ others to look into: 1206SFH100F/24CT-ND has 20A fuse (3 mOhm).
+ Do one per FET for now...
+ Might need something more like 507-1524-1-ND
+ or might need to fuse the main power supply or put a breaker on it.
+ Maybe fuse the bus wires with automotive fuses?
+ SMD1206 eagle package added along with FUSE device.

I think per string is a good idea. Otherwise we are talking 200A
pulses and ESR becomes really important. Although Slink will probably
be in the 25% populated region.

+ decided to ditch the PCB fusing and just use an in-line option:
    - http://www.allelectronics.com/make-a-store/item/FHP-25/WEATHERPROOF-ATC-BLADE-FUSE-HOLDER//1.html
    - http://www.allelectronics.com/make-a-store/item/FHP-2/IN-LINE-AGC-3AG-FUSE-HOLDER//1.html

Question for Jeff:

    we are going to do a layout that can have 25%, 50%, or 100% of the
    leds. How many per in^2 do you want and is that the 100%
    configuration? The boards are going to be 3" x 8".

    I remember us talking about 4 per in^2 but I don't remember if that
    was for Slink or if that was the 100% population.

Some options are:

24 in^2 total

+ 1 string  4 LEDs/board, 1 LED/ 6 in^2
+ 2 strings 8 LEDs/board, 1 LED/ 3 in^2
+ 4 strings 16 LEDs/board, 1 LED/ 1.5 in^2
+ 8 strings 32 LEDs/board, 1 LED / 0.75 in^2 

Answer is 4 per sqin is 100%, slink might only need 25%.

96 LEDs total (for 8x3 board). 

24 strings per board. Try for 8 strings per FET. At 25% pop. that will
be 2 strings per FET. 3 FETs per board.

6 FETs per board should work well. That would be 4 strings per FET.

rev 1 will be used to see how far we can push each FET.

PCB: 
---

8" x 3" modules. 

Number of LEDs per sq. in. in 100% config: TBD.

Number of parallel strings is TBD. (min is 4, max could be 8 or 12, or
if only 100% and 50% then 2,4,6,8... etc).

Power bus copper on top and bottom, copper braid to connect all the
boards side-by-side.

4-40 clearance holes for mounting.

TDB connectors for logic level on/off signals.

Bus Bar Pads:
------------

Half inch on top and bottom layer will be about 1mOhm per 3 inches.

Tasks
=====

+ determine LEDs per in^2 and population options (50%-100% or 25%,50%,100%)
    - done
+ create slink.brd with formfactor part
    - preliminary formfactor done
+ add leds to slink.sch and slink.brd 
    - done
+ place LEDS
    - done
+ determine and place mounting holes
    - preliminary mounting holes done
+ hook up leds in .sch
    - done
+ route LEDS
+ create FET device, hookup and place
    - done
+ bypass caps
    - done
+ determine connectors, hookup and place

Routing Order:
--------------

LED strings to FET
FET path to GND
Gate drivers to FETs
Power to gate drivers
logic level signals to gate drivers
Route back to a connector:
+ 12v0
+ GND
+ 1-6 logic level drives (you could connect them all, or vertical
groups, or keep them individual)
