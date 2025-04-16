# PCB800182_LCD_Adapter
Reverse engineering of PCB800182

## Background
I was about to receive a VM820C evaluation board from Bridgetek which which is a credit-card board for exploration of the EVE5 chip BT820.
The VM820C has a 50pin FFC connector for a bunch of 10.1" LCDs that Bridgetek could locally source, such as the MTF101UG18A-V, for which I was not able to find documentaion.
So I was looking for alternatives and ended buying a 7" 1024x600 with single channel LVDS from Buydisplay.com - ER-TFT070-3 - and it has a 40pin FFC.
Next I started to build my own adapter board and soon realized that I could not route the differential pairs of LVDS lines straight from the 50pin FFC to the 40pin FFC, I would need to cross the lines and I did not like that.
Additionally there are a couple of signals I had no idea on how to provide, like VGL and VGH.
So I looked for alternatives and found the PCB800182 which prommises to connect a single lane LVDS on a 20pin header to a 40pin LCD.
Some sellers on EBay provide the pinout for the 40pin FFC and it is a match for the LCD I bought.
The VM820C also has a 30pin LVDS header, so all I had to do was to switch 2x10 header on the provided cable to a 2x15 header with the desired pinout.
## Looking for more information
While everything was still out for delivery, I searched for more information regarding the PCB800182 - and I really did not find much. These things are beeing sold from various sources and annoyingly so in different revisions. There does not seem to be a common manufacturer for these however, it looks like these are assembled by a bunch of different factories in China.
I found one .pdf in chinese from RUI CHENG XIANG, document RCX131128, released november 28th of 2013.
I had to translate it from chinese to englisch. In this document there was a picture of the very same board I was about the receive, an image for the mechanical dimensions that showed positions of parts that did not match the image, descriptions of configuration resistors that turned out to be not on the board and not description of the resistors that actually are on the board.
Be aware that there are similar boards with that name, I have seen for example a version that that does not have the footprint for the 6pin backlight-power header.

## Reverse Engineering
So when the board arrived I started to analyze it and PCB800182_LCD_Adapter.PDF in this repository is the result of this.
First I analyzed the backlight driver circuit and found it was using a PT4103.
Turned out, the output current for the backlight driver was setup to 150mA and this would not only have grilled my LCD, it might had damaged the VM820C as well since the current is derived from the 3.3V supply and the source current needed is roughly tripple the current that is running thru the LEDs.
So I initially removed the provided 0.68R resistor and soldered two 10R resistors on top of each other in order to set the current to 20mA.

Next I checked the signals on the FFC40:
VCOM is set to 1V by a voltage resistor.
Reset is pulled to 3.3V by a 10k resistor.
STBYB is left open.
DIMO is left open.
SELB is hardwired to GND and therefore the adapter only allows 8 bit per color.
AVDD has a capacitor to GND and also is the input for the voltage pump VGH is supplied from and which is clocked by the switch output of the PT4103.
L/R und U/D have the only actual configuration resistors.
VGL is supplied from a second voltage pump clocked by the switch output of PT4103.
CABCEN0 and CABCEN1 are left open - and I wish they weren't.

What I left out on my schematic are most of the designators, the reason is that the silkscreen printed on the PCB I received is atrocious, most of it is merely a collection of dots and not real characters.


## The end?
I reached my goal, the LCD is up and running, I trust the thing enough, I can setup the backlight current as required.
If you however find anything wrong, please tell me.