+++
title = "General Troubleshooting Guide"
+++

# General Troubleshooting Guide

## Equipment

### DSD TECH SH-U09C5 USB to TTL UART

<img src="/DSD-TECH-SH-U09C5.png" />

On the pictured cable (upside down in this pic), you will only use White, Gray and Purple leads, tape (or glue) them together with White in the center, and the other two on the sides; the other three are unused.
You can either clip the connections of the WGP wires and solder them directly to the board or solder in leads like the ones pictured so you can disconnect from the board. We prefer the connection method.
On the control board, you will see "R UART T" near the three console points. RXD GND TXD connects in the opposite order, meaning the RXD on the USB adaptor will connect to T on the board, GND will connect to UART, and TXD will connect to R.
If yours is anything like ours, you will have to disconnect the colored cable from either the control board or the adaptor BEFORE powering the test bench up or it will think you have pressed a key interrupt and prevent the board from loading the test file. After about 5 seconds you can reconnect it. We have two cables that behave this way, and two that do not.
Using this with the test bench will allow you to get the report on PUTTY like any other COM cable.

## Microscope

<img src="/Amscope-Microscope.png" />

We have a microscope used to inspect small resistors and leads; we have an AmScope with a camera, but you can find cheaper digital ones that work well enough.

<img src="/Digital-Microscope.png" />

## Soldering Tools

- Kester 186 Flux-Pen, CHIPQUIK RMA791 rosin flux container (personal preference)
- Lead-Free Solder Paste (8oz container or more, MAKE SURE TO KEEP REFRIGERATED and monitor expiration date, this stuff will go bad and will not work)
- You have a good heat gun and solder iron, consider J-tip attachments for iron.
- Set of J-tip tweezers; cross locks are nice but not necessary.
- Spool of solder wick, a width of at least 2mm, 3mm is good.
- Set of pliers, at least one with needle nose.
- Small battery-operated fans for cooling boards during the soldering process.

## Diagnosing Issues

Along with this guide, you should refer to the manual for S17 diagnostics, other models are slightly different, but the theory is the same.

### 0 ASIC

Usually caused by a bad ASIC or power failure.
Using the RO test point, check for 1.8V or higher starting from the last chip (48) to the first; if you get a reading of 1.5V or less, there is a problem with the ASIC to the LEFT of the point. I.E., if the RO between 47 and 46 is reading 1.8V, but between 46 and 45 is reading 0.9V, there is a problem with ASIC 46.
If you are getting 1.8V across one domain (4 chips vertical) but not on another domain, inspect the 5-pin LDOs on the backside bottom of the board. The lowest one should put out 1.8V or higher (max of 3.3V) on the bottom left pin, and the other should output 0.8V on the same pin. If these are not working check for 3.3V input on the other side of the LDO. If it has 3.3V in but is not outputting correctly, change the LDO.
If you cannot get >1.8V anywhere on the board, and your LDOs are not getting 3.3V in, inspect the "Boost Circuit" (refer to the manual) for damaged capacitors and chips, and make sure the entire board is receiving at least 18.0V in.
If the Boost Circuit appears fine and there is 18.0V to the board, replace the 24-Pin PIC chip designated U3 on the underside of the board near the power terminals.
If you have gotten this far and it still has not worked, I am sorry; you have a damaged capacitor on the board, and it is one of the small ones. You will have to look for burns, or capacitors that have moved from their locations. If nothing appears, you will have to use a microscope and inspect each capacitor for cracks and fragmentation. These typically occur near the PIC chip where a lot of power flows, or near your test points on the chips.

### "X" ASIC

These are very straightforward. If the test bench reads out "ONLY HAVE 'X' ASIC" more likely than not the bad chip is X+1. For example "Only Have 31 ASIC" would mean that ASIC 32 needs to be replaced.

### Temp Sensor Lost / Bad Clock Counter / REG_TYPE (Pattern NG)

All these conditions relate to a bad pattern. You will have to use the test bench and find out which chip is scoring low and replace it.
The exception here is the Temp Sensor Lost condition, sometimes this can be fixed by cleaning off the temp sensor (often replacing it) and the capacitors near it. More often than not though, an ASIC that is not seated properly will cause a temp sensor loss because the sensors are fed power FROM the ASIC.
Bad Clock Counter you will only see in the kernel log, it will read something like "BAD CLOCK COUNTER CHAIN 0, ASIC 5" this means that ASIC 5 is seated improperly and may be repaired simply by reflowing the solder.
REG_TYPE errors are related to a bad pattern, SOMETIMES the test bench will point out what chip caused this problem, other times the test log will fill up with "REG BUFFER FULL." This indicates that more than one ASIC has a bad pattern test and needs to be replaced. SOMETIMES you can replace the EEPROM storage chip (designated U6 on S17) and it will solve this problem.

### PIC Lost / Heart Beat Fail

These are ALWAYS caused by a PIC chip failure. If the board is receiving proper 18.0V and it does not work, Replace the 24-pin PIC chip U3. If that does not fix it, inspect the capacitors near the chip.

## Replacing ASIC Chips

### Bin Numbers

This is the most important factor to consider. The ASIC chips are compatible between the T17, S17, and the plus models (NOT ENTIRELY SURE ABOUT T17E BECAUSE WE DON’T HAVE ANY) so you can use them from old boards within the same 17 family. However, labeled on the boards is a sticker that designates BIN number, this can be Bin1, 2, 3, or 4. You MUST use Bin 2 chips on a Bin 2 board, there is NO compatibility between them. Secondly, but less important, if recycling chips, try to use boards that pass at a similar pattern level; this is designated by the sticker "L1, L2, L3 … L8"
When purchasing ASIC chips online, be careful and identify the BIN number you are purchasing and mark the bag they come in as soon as you get them. There is NO WAY to identify bin number by looking at the chip. (I am sorry about the roll you guys have; it may be useless unless you can find that info.)

### Removal

Remove the heatsinks from the top and bottom of the chip (480 degrees, 50% fan) do not apply pressure to the heatsinks or you will damage the chip, they should come right off.
Apply flux pen to the leads of the ASIC, and some to the bottom (you can see air holes underneath)
Heat the chip (460 degrees, 25% - 30% fan) directly above (1-2 inches) until you see the solder flow
While heating, take J-tip tweezers on the NON-contact edges of the ASIC and pick it straight up. Again, not too much pressure, when it is heated enough it will come right up. Otherwise, you will damage the chip or PCB. Pick the chip STRAIGHT UP as best you can, or you risk bridging the contacts with solder or knocking a capacitor out of place.
If the chip does not come off after 3 minutes of heat, it may be fused to the PCB; this happens when a chip gets too hot or receives too much current. By this point, you may see the chip starting to crack or parts of it coming off. Unfortunately, there is no way to save the PCB once this happens, the internal circuitry is damaged. The good news about this is that most of the other components on the board are reusable so you will have gained a good parts board.

### Placing

First, inspect the socket and chip for any solder bridges or areas with low solder. Clear any bridges and apply the new paste to areas that need it, heat the new solder until it tins. Check the main pad areas of the socket and chip for high spots and clear them using the solder iron and wick, or the chip will not sit flat. Apply a generous amount of flux to the chip and socket, and double-check your orientation.
I like to hold the chip in place with the tweezer first, apply a short burst of heat until the solder begins to flow, I call this tacking. Remove heat and tweezers and the chip should sit where you were holding it. Apply more flux if needed and apply heat directly above the chip (460 degrees, 25% fan) until the chip sits down, you will see the solder flow onto the contacts around the chip. You may have to apply GENTLE downward pressure to the chip to seat it properly. If you see any contacts that look bare, apply a small amount of pressure to that area during heating.
Once placed, allow the board to cool using a battery-operated fan or sit it on the tester with the fans on.
Check your test points (attached documentation) for their proper readings, if they are good you can run the board on the tester, BUT ONLY ALLOW IT TO DO ASIC COUNT. You can toggle which mode the test runs in the config.ini file on the testing SD card. Alternatively, you can run it until the “nonce” value on the little screen changes from 0. (If you have the console cable hooked up you can see where it will read how many ASICs it sees). TURN OFF THE BENCH ONCE IT BEGINS THE PATTERN TEST. If you have not reapplied the heatsinks the board WILL overheat and give you a bad pattern result or burnout.
If your board is now reading all its ASICs (or a different chip is bad now) you can reattach the heatsinks to the top and bottom of the chip. Use 480-degree heat, 30% fan above the heatsink and apply NO PRESSURE, the heatsink will seat itself when it is hot enough.
Next, run the pattern test; if everything went well your chip should return a good pattern result. Otherwise, you may need to apply more heat to the chip, reseat it, or replace it again.
If it passes the pattern test with no issues, the board is ready to put into a miner and run.

_Good Luck!_
