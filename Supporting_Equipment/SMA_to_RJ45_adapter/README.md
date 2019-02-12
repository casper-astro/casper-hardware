# SMA to RJ45 adapter

The SMA to RJ45 adapter board is used, primarily in the lab, to convert
single-ended analog signals into a differential format suitable for the
[ADC16x250-8\_RJ45\_rev\_1](../../Mezzanine_Boards/ADCs/ADC16x250-8/ADC16x250-8_RJ45_rev_1/README.md)
board.

[https://casper.berkeley.edu/wiki/ADC16\_cross\_corr\_tests\#ADC16x250-8\_rj45\_rev\_1ADC16x250 rj45 cross corr
tests](https://casper.berkeley.edu/wiki/ADC16_cross_corr_tests#ADC16x250-8_rj45_rev_1_ADC16x250_rj45_cross_corr_tests)
describes an example of how this board is used in the lab.

## Block Diagram

- [block diagram with sample usage PDF)](block_diagrams/SMA_to_RJ45_2013jun07.pdf)

## SMA to RJ45 adapter test board

- designed and built for use by the LWA project; Documented here for
  convenience only.
- 16 vertical launch SMA socket connectors for single ended test
  signals and 4 RJ45 connectors to drive 4 CAT7 cables.
- A 1:2 balun is used for each of the 16 test inputs. Different baluns
  may be used based on the degree of optimization for operation at a
  particular operating frequency. Or not.
    - [default part: MiniCircuits TCM2-1T 3-300 MHz 1:2 input balun (PDF)](datasheets/TCM2-1T+.pdf)
    - The typical output phases are not specified..
    - The typical insertion loss is 0.4 to 0.7 dB over 3-300 MHz
    - Alternative baluns are compatible with the PCB footprint; exact
      part numbers are TBD.
- All 16 differential pairs are output over a total of 4 RJ45
  connectors to CAT7 cables..
- Tyco Electronics 4way CAT5E RJ45 connector
    - [Tyco Electronics company website](http://www.te.com/) and the
      specific part number URL [Tyco 5558342 shielded RJ45](http://www.te.com/catalog/pn/en/5558342-1)
    - [Tyco RJ45 datasheet (PDF)](datasheets/Tyco_5558342_drawing.pdf)
- CAT7 shielded RJ45 cables. Here's an example 3 ft yellow cable.
    - [Newegg N82E16812119445](http://www.newegg.com/Product/Product.aspx?Item=N82E16812119445)
- All 16 of the baluns are on the top side of the PCB.
- 8 of 16 differential pairs from the baluns to the RJ45s are on the
  top side of the PCB. The remaining 8 are on the bottom. This way the
  interpair isolation is increased as well as easing the routing to
  the RJ45 connector.
- SMA center pin to SMA center pin spacing is about 11/16 inch or
  0.6875 inches.
- The SMA connectors are on the opposite side of the PCB as the
  corresponding etch and balun to minimize the transmission line stub
  at the SMA launch.
- The board is roughly 9.5 inches by 1.9 inches.

## schematics

- [page 1 of the schematics (PDF)](schematics/SMA_to_RJ45_schematics_1.pdf)
- [page 2 of the schematics (PDF)](schematics/SMA_to_RJ45_schematics_2.pdf)

## place and route

- [top view of the place and route (PDF)](place_and_route/SMA_to_RJ45_PandR_top.pdf)
- [bottom view of the place and route (PDF)](place_and_route/SMA_to_RJ45_PandR_bottom.pdf)

## design files

- [GERBERs (tar)](gerbers/SMA_to_RJ45_gerber.tar)
- [BOM spreadsheet (xls)](BOM/SMA_to_RJ45_BOM.xls)

## photo

- please see the block diagram document shown above
- [close up view of one of the 4 sections (JPG)](photos/SMA_to_RJ45_upclose.JPG)
