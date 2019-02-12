## Summary Table

|                |                          |                                                                        |                                                                         |      |          |           |                        |
| -------------- | ------------------------ | ---------------------------------------------------------------------- | ----------------------------------------------------------------------- | ---- | -------- | --------- | ---------------------- |
| Make           | Model \#                 | Datasheet                                                              | SPD                                                                     | Size | PPC Slot | FPGA Slot | Status                 |
| Corsair        | CM72DD1024AR-667/S       | [media:CM72DD1024AR.pdf](media:CM72DD1024AR.pdf "wikilink")            | [media:corsair\_ddr2\_spd.txt](media:corsair_ddr2_spd.txt "wikilink")   | 1GB  | Yes      | Yes       | No longer manufactured |
| Corsair        | CM72DD1024R-667/V        | [media:CM72DD1024R.pdf](media:CM72DD1024R.pdf "wikilink")              | SPD                                                                     | 1GB  | Yes      | No        | No longer manufactured |
| Kingston       | KVR800D2D8P5/1G          | [media:KVR400D2S8R3\_512.pdf](media:KVR400D2S8R3_512.pdf "wikilink")   | [media:Kingston\_ddr2\_spd.txt](media:Kingston_ddr2_spd.txt "wikilink") | 1GB  | Yes      | Yes       | No longer manufactured |
| Viking Modular | 1Rx8 PC2-5300R-555-13-F0 | [media:PS5Exxx7218xxx\_D2.pdf](media:PS5Exxx7218xxx_D2.pdf "wikilink") | [media:Viking\_ddr2\_spd.txt](media:Viking_ddr2_spd.txt "wikilink")     | 1GB  | Yes      | Yes       | Long-Term Support      |
|                |                          |                                                                        |                                                                         |      |          |           |                        |

## ROACH DDR2 Memory Modules

The following ECC registered DIMMS are currently supported in both DDR
slots:

  - Corsair CM72DD1024AR-667/S (No longer available)
  - Kingston ValueRam KVR667D2D8P5/1G (No longer available)
  - Viking Modular VR5ER287218FBWS1 (Long term support)

Revision 1 and 2 ROACH boards were shipped with Samsung KR
M378T6553CZ0-CCC modules. These are unregistered and will lead to
crashes if pushed hard. These modules should be replaced with one of the
recommended modules mentioned above. You will need the latest version of
Uboot for the SPD to be decoded and the memory controller configured
correctly. Just replacing the memory on old boards will not work - you
will need to update your firmware.

The boards were also shipped to boot in configuration H which runs the
EPB bus at 82 MHz and memory bus at 166. This should be changed to
configuration C.

The CPU/memory/bus speeds are configured by bootstrap configuration
options (which setup clock sources/PLLs). These can be pulled from an
IIC memory (eg config H) when the processor comes out of reset or
default to sensible factory defaults if certain pins are pulled high/low
(eg config C). You can checkout the AMCC PPC datasheet for more details.
U-boot doesn't configure these timings, merely decodes the register
values and reports the selected configuration speeds. These are passed
on to Linux (ie Uboot configuration does hang around after Linux boot)
so it's important that these speeds are configured correctly if you want
your RTC to work, for example.

ROACH boards ship with one of the IIC proms configured for 500MHz CPU,
166MHz DRAM, 83MHz bus (used when the PPC is booted in config H) which
pushes it right to the limits. Think of this as an "overclock" mode. The
recommended speeds are identical to the factory defaults in config C and
so this is what we recommend everyone runs unless you need to squeeze
extra performance in which case you can try config H (but YMMV).

More info here in the [email
archive](http://www.mail-archive.com/casper@lists.berkeley.edu/msg01370.html).

## Power PC Memory

The Power PC slot on ROACH will generally take most Registered DDR2
DIMMS. To test if a DIMM works there are a number of things you can do:

  - Plug in the DIMM in the power PC slot, if the board boots up that is
    half the battle won.
  - From Uboot run the command 'mtest' this will iterate indefinitely.
    If there are no errors after a few iterations the DIMM is working.
  - To be even more certain that the DIMM works, let the ROACH boot and
    start up the kernel. If it starts with no problem then the DIMM is
    working fine.

## FPGA Memory

Due to the controller constraints on the FPGA, only a few DIMMS are
supported. These are:

  - Corsair CM72DD1024AR-667/S (Mo at Digicom has stock)
  - Kingston ValueRam KVR667D2D8P5/1G (No longer available)

Currently, we have now got "long support" DIMMs from Viking Modular
which have been tested in both the FPGA and PPC slots.

  - Viking Modular VR5ER287218FBWS1 (Long-term Support)

To purchase these modules, please contact Mo Ohady at mo(at)digicom.org

## Testing FPGA DIMMs
