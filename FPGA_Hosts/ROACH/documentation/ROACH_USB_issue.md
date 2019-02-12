On some ROACH boards the USB is not correctly picked up in uboot. This
presents a workaround for the problem.

## History

ROACH has a USB 2.0 host connector. This connector is directly attached
to the PowerPC. On power-up uboot is loaded on the PowerPC and then
Linux is booted from uboot. Uboot only loads USB 1.1, not high-speed.
For an unknown reason uboot sometimes does not correctly pick up drives
attached to the USB connector.

## The Solution

Linux picks up the USB correctly and when rebooting uboot also loads the
driver correctly. Note that the problem returns if the board is power
cycled. If Linux is to be loaded from a USB drive follow this procedure:

  - Power-up ROACH
  - At the uboot prompt type: run soloboot
  - Wait for the pre-loaded Linux to boot
  - Login using root
  - At the Linux prompt type: reboot -f
  - Uboot will load and the USB drive should be correctly loaded.
