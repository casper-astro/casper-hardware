# ROACH 2 Enclosure

## Introduction

The ROACH 2 Enclosure is a 1U enclosure, with mounting holes for an ATX
size PCB, the Sunpower SPX-62000A1 PSU, dual ADCs and cutouts for the
ROACH 2 rear panel. Provision is made for custom front faces, including
holes for LEDs and ADCs. The Enclosure is designed to be compatible with
a 19" computer rack when fitted with Intel's AXXBASICRAIL kit or
iStarUSA's TC RAIL 20 (Not 1U Compatible).

[Intel AXXBASICRAIL](http://ark.intel.com/products/51276/Basic-Rail-Kit-AXXBASICRAIL)

[iStarUSA TC RAIL 20(Not 1U Compatible](http://www.istarusa.com/istarusa/products.php?series=Accessories&sub=Rails%20&%20Supporters&model=TC-RAIL-20)

![roach\_2\_chassis\_iso.jpg](photos/roach_2_chassis_iso.jpg "roach_2_chassis_iso.jpg")

The box is "RFI aware" (meaning that it was designed with RFI
considerations), but it is not RFI tight. It offers \~60dB of shielding?
(requires verification), with most of the leakage coming from the power
cable and peripheral leads (eg. ethernet jack). Seams are welded shut
and removable panels have overlapping edges. The SMA connectors to the
ADC's (8 of them) have been moved to the rear of the chassis to help
improve RFI.

![roach\_2\_chassis\_rear.jpg](photos/roach_2_chassis_rear.jpg "roach_2_chassis_rear.jpg")

Provision is made for a Mini-Circuits type two-way splitter to be
mounted internally (for feeding clocks to the ADCs) and there is
sufficient space for additional RF components, should these be necessary
in your application (see pictures below).

You might want to consider adding a heatsink to your PPC to aid cooling.

## Production Prototype Manufacturing Files

The production files and manufacturing packs can be found inside the
github repo:
<https://github.com/ska-sa/roach2_chassis_design/tree/master/roach_2_manufacture>
The ROACH2 PCB can be found here:
<https://github.com/ska-sa/roach2_hardware.git> With the LED frontpanel
PCB here: <https://github.com/ska-sa/roach2_ledstatus_frontpanel_pcb/>

The BOM file lists all the designed in parts and extras used in the
ROACH 2 Chassis by the South African team, including ATX power supplies,
fans etc.

## Changes for the ADC16x250-8 differential rev 1 and ADC16x250-8 differential rev 2 boards

Changes are required to the this standard Roach2 1U enclosure, in order
to make it compatible with the ADC16x250-8 differential rev 1 and
differential rev 2 cards. The primary issue is the lack of access for
the analog inputs, via the Samtec Vport cable, to pass pass through the
faceplate and enclosure face and connect to the ADC16x250-8 differential
rev 1 or rev 2 card. A secondary issue is the need, as discussed above,
to remove the front panel LED cable from Roach2 rev2 P15 to make way for
the control and configuration cable for the ADC16x250-8 differential rev
1 at P11 "ZDok1".

- [1U chassis and faceplate changes (PDF)](documentation/Roach2_chassis_opening.pdf)
- This PDF has two sketches. All dimensions are in 0.001". Tolerances
  aren't critical.
    - Sketch 1 at the top of the page and shows the \#6-32 thru holes
      that must be drilled into the bottom of the chassis to hold the
      Samtec cable strain reliefs. Not shown is the counter sink step
      for the exterior bottom side so the screw heads don't protrude.
    - Sketch 2 is at the bottom of the page and shows the openings in
      the face of the chassis and the separate face plate. The changes
      to make on the front face of the 1U chassis are :
        - Englarge the hole for the reset button for the Samtec cable
          for the ADC16x250-8 differential rev 1 or rev 2 at ZDok1
        - create a new opening for the ADC16x250-8 differential rev 1
          or rev 2 at ZDok0
        - Drill a new hole for the relocated reset button.
        - the opening for the power button is unchanged.
    - The changes to the 1U faceplate are
        - Turn the hole for the reset button into a notch so it can
          slide over the ZDok1 Samtec cable.
        - Drill a new hole for the relocated reset button.
        - Make a new notch so it can slide over the ZDok0 Samtec
          cable.

<!-- end list -->

- [Samtec Vport cable clamp (PDF)](documentation/Vport_cable_clamp.pdf)
- This PDF has 1 sketch showing the two parts of the cable clamp made
  from 1 block of Delrin Plastic. All dimensions are in 0.001".
    - The kerf of the blade making the cut isn't critical.
    - When one screws the two parts together around the (trimmed)
      Samtec cable stop once the cable is firmly held. Don't over tighten things as that will probably destroy the threads in the plastic as well as needless compress the cable.

- It certainly OK to tweak the basic idea to better match your
  application such as to use metric hardware if you'd like.

<!-- end list -->

- **1U enclosure pictures after rework**
- [small collection of pictures of the modified enclosure (\~4MByte
  PDF)](documentation/Roach2_enclosure_for_ADC16x250-8.pdf)

## Changes for the ADC16x250-8 RJ45 rev 1 boards

- [small collection of pictures of the modified enclosure with its 2
  ADC16x250-8 RJ45 boards. (\~5MB PDF)](documentation/ADC16x250_rj45_chassis_photo.pdf)
    - TBD include details of the CAT 7 analog input cabling.

Basic idea is to have 1 large rectangles cut out of the front of the 1U
enclosure and 1U faceplate. Through this 1 openings will pass 8 CAT7
cables that mount to the RJ45 connectors on the [ADC16x250-8 RJ45 rev 1](../../../Mezzanine_Boards/ADCs/ADC16x250-8/ADC16x250-8_RJ45_rev_1/README.md) boards. There are no bulkhead
mount RJ45 connectors. There are no CAT 7 cable strain reliefs. Here are
some mechanical sketches.

- [front panel (PDF)](documentation/ADC16x250_8_rj45_front_panel.pdf)
- [chassis (PDF)](documentation/ADC16x250_8_rj45_chassis_mod.pdf)

## Changes for the ADC16x250-8 coax boards

There are a number of changes to the front face of the enclosure as well
as an entirely new front panel to provide mounting for the 16 coaxial
analog inputs per ADC16x250-8 coax board. 32 in total for a typical
application which has two such cards per enclosure.

Mechanical design change files

- [coax design files (TAR.GZ)](documentation/ADC16x250_coax_chassis.tar.gz)
- The machining isn't all that hard. It's even possible to do the
  machining by hand using an electric drill and metal file. It wasn't
  fun but not the end of the world either.
- The Roach2, PSU and other components must be removed from the
  enclosure prior to machining the enclosure due to all the metal
  filings produced by the drilling and filing.
- After the machining is complete the enclosure must be carefully
  cleaned.
    - The intersection between the chassis and the cover plates for
      the Mezzanine Board (SFP+ and CX4, ...) are very good at
      capturing metal filings.
- No particular precision is required on the 2 big rectangular
  openings. Same idea for the 2 button holes; there are wires from
  button to LED board so quite a bit of slop is allowed.
- Some care is required on locating the holes for the 5 LEDs. the LEDs
  are mounted to a PCB and they can't move around all that much. Of
  course, if LEDs aren't all that important to you then there may be
  no need to make holes to see them.
- the following coax cable files also have some dimensions for the
  balun spacings across the board (top and bottom side).
    - [rev 2 coax cable mapping aid (PDF)](documentation/ADC16x250-8_coax_rev_2_cables.pdf)
    - [rev 1 coax cable mapping aid (PDF)](documentation/ADC16_coax_cables.pdf)

Electrical design change files

- schematics
    - [schematics (PDF)](documentation/ADC16x250_8_coax_compatible_LED_PCB.pdf)
    - note: there are only 3 GPIO LEDs available, bits 0, 1 and 2, due
      to limited space.
        - GPIO bit 3 is available, as a digital logic signal, on the
          LED PCB to those willing to use a soldering iron.

<!-- end list -->

- Altium design files (schematics and place and route)
    - [actual Altium design files (TAR)](documentation/ADC16x250-8_coax_compatible_altium.tar)

<!-- end list -->

- Gerber files for bare board fabrication
    - The first fabrication run was with Sunstone quickturn
      <http://www.sunstone.com/QuoteQT.aspx> using the panelized tar
      file
        - [gerber files for a 2x3 panel fabrication of boards (TAR)](documentation/ADC16x250_coax_compatible_panelized_gerber.tar)
        - PO 2 panels of \~ 2.8 x 2.8 inches for a total of 12 boards.
        - please see the gerber\_readme.txt file in the above tar
          file.
    - [Gerber files for 1 board fabrication (TAR)](documentation/ADC16x250_coax_compatible_gerber.tar)

<!-- end list -->

- Bill of Material (BOM) for assembly
    - please note this BOM includes information on components actually
      soldered to the LED PCB as well as the cabling from the LED PCB
      to the Roach PCB.
    - [Bill of Materials (PDF)](documentation/ADC16x250_8_coax_compatible_BOM.pdf)
    - No special assembly nor rework instructions. The first boards
      were built at the RAL E Lab.
    - These documents include no information on the \~ 3" long wires
      and on/off and reset buttons that are mounted to the front of
      the chassis. Those components are used without modification.

Mechanical and Electrical Design photos

- [tbd small collection of pictures of the modified enclosure still
  waiting for its 2 ADC16x250-8 coax rev 2 boards. (\~5MB PDF)](documentation/ADC16x250_coax_rev2_chassis_photo.pdf)

## changes to internal cables for the ADC16x250 boards

This section isn't strictly a mechanical change to the chassis. Rather
how the chassis is typically populated.

The typical use of the ADC16x250-8 sampler boards, any flavor, has

- 2 ADC16x250-8 boards of the same type in 1 chassis.

- 1 20 inch long RG316 50 ohm coax cable with one end a right angle
  SMA plug with clock for the ADC16x250-8 rj45 at ZDok+ slot 0 and the
  second end a straight bulkhead jack in the lower LEFT hand corner
  position of chassis' rear panel grid of SMA connectors.

- 1 20 inch long RG316 50 ohm coax cable with one end a right angle
  SMA plug with clock for the ADC16x250-8 rj45 at ZDok+ slot 1 and the
  second end a straight bulkhead jack in the lower RIGHT hand corner
  position of chassis' rear panel grid of SMA connectors.

- 1 20 inch long RG316 50 ohm coax cable with one end a right angle
  SMA plug with Syncout from the Roach2 rev2 AUX\_SYNC OUT aka J11
  output and the second end a straight bulkhead jack in the UPPER left
  hand corner position of chassis' rear panel grid of SMA connectors.

- 1 20 inch long RG316 50 ohm coax cable with one end a right angle
  SMA plug with Syncin for the Roach2 rev2 AUX\_SYNC IN aka J10 input
  and the second end a stright bulkhead jack in the UPPER right hand
  corner position of chassis' rear panel grid of SMA connectors.

- [SMA locations at back of chassis (JPG)](photos/ADC16x250_8_clock_sync_chassis_back.jpg)
  
    - clk0 is the input clock for the ADC16x250-8 board at ZDok+ slot 0
    - clk1 is the input clock for the ADC16x250-8 board at ZDok+ slot 1
    - SYNC OUT is the output sync from the Roach2 rev2
    - SYNC IN is the input sync to the Roach2 rev2

- [cable network analyzer measurements (PDF)](documentation/Roach2_clock_sync_internal_cable.pdf)
  
    - these cables are about 0.6 dB loss at the 1 GHz maximum clock frequency of the ADC16x250-8 boards. These losses are to be used when designing the overall clock distribution sub-system.
