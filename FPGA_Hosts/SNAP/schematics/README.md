`SNAP_schematic_commented.pdf` contains comments from Jason Ray and Randy McCollough of NRAO in Green Bank after reviewing the design on 13th January 2014. Comments were incorporated into revision v1.0 of the schematics.

`SNAP_schematic_v1_0.pdf` are v1.0 of the schematics.

`SNAP_schematic_v2_1.pdf` are v2.1 of the schematics. v2.0 fixed the ZDOK connector and power connector footprints, as well as minor bugfixes and improvements. See sheet 7 of the schematics for details.
v2.1 fixed FPGA temp/voltage monitoring. See sheet 7 of the schematics for details.

*WARNING*  The ASCII art diagram of showing the ZDok+ connector pin locations on page 27 is not correct for a bird's eye view of the top of the board. It has signal pins reversed; A1 and A20 should be swapped. The ASCII art drawing on page 7 is correct.

*WARNING* The comments on page 9 stating that the VCCO of the IO banks for the ZDok+ signal pins will probably be 3.3V and on bank 16 are incorrect. The diagram and changes for Rev B comment on page 7 are correct: the banks are 12, 13 and 14 and the voltage is 2.5V. The comment on page 12 that VADJ_FPGA probably set to 2.5V is also correct.

`SNAP_schematic_orcad_vX_Y` are zipped schematic vX.Y source files generated using Orcad.

`SNAP_schematic_v2_1_netlist.txt` is an ascii-formatted netlist of v2.1 of the schematics. It is in Mentor PADS format.
