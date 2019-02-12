# ROACH

## Gateware

  - ROACH Monitor Actel - 8.3.1698
      - [UFC
        File](http://casper.berkeley.edu/svn/trunk/roach/gw/binaries/roach_monitor/roach_monitor_8_3_1698.ufc)
      - [STP
        File](http://casper.berkeley.edu/svn/trunk/roach/gw/binaries/roach_monitor/roach_monitor_8_3_1698.stp)
      - Note that these files are for iStar or other generic ATX
        enclosures. Those using the [ROACH
        motel](https://casper.berkeley.edu/wiki/ROACH_Motel) require
        different Fusion firmware for the LED/front switch panel to
        operate correctly.

<!-- end list -->

  - Xilinx CPLD
      - [JED file
        - 8.0.1588](http://casper.berkeley.edu/svn/trunk/roach/gw/binaries/roach_cpld/roach_cpld_8_0_1588.jed)

## Firmware

  - UBOOT
      - [July 2010](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/uboot/uboot-2010-07-15-r3231-dram)

<!-- end list -->

  - Busybox minimal
        filesystem
      - [2010-07-19](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/filesystem/mini-root-2010-07-19-size.gz)

## Software

  - Linux
        Kernel

<!-- end list -->

  -   - [2011-08-12](https://casper.berkeley.edu/svn/trunk/roach/sw/binaries/linux/uImage-20110812-mmcomitfix)
          - Includes mmc and monitoring drivers that were not present in
            the previous compile

<!-- end list -->

  - tcpborphserver (KATCP server on
        ROACH):

<!-- end list -->

  -   - [2011-03-03](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/tcpborphserver/tcpborphserver2-2011-03-03-r3398-fallback)
          - Allows for detection of FPGA programming complete. Requires
            kernel 20110303.

<!-- end list -->

  - tgtap (10GbE userspace
        driver)
      - [2010-03-24](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/tgtap/tgtap_2010-03-24)

<!-- end list -->

  - Linux Root
        filesystem:
      - [2010-03-24\_sd\_shipping](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/filesystem/filesystem_etch_2010-03-24_sd_shipping.tar.gz)
          - Note: This filesystem currently contains an outdated
            tcpborphserver.
          - Note: This filesystem defaults to mounting root filesystem
            read/write. Network-boot users: watch out for multiple
            boards trying to read/write to the same filesystem, thereby
            overwriting each others' files; mod fstab or the kernel boot
            (init) string in uboot as appropriate.

## Power on reset issues

The Actel Fusion device (which manages ROACH's power supplies and system
monitoring) does not have an internal POR. An external circuit
constructed around a capacitor and a resistor provide a time delay to
perform this function. Early ROACH boards (up to and including serial
numbers 03aabb) were shipped with a 0.01uF capacitor C33. This should be
changed to 0.22uF to increase the POR pulse signal and ensure that this
circuit behaves as it should. If your board behaves strangely when you
first plug it into the wall (maybe it turns itself on or won't turn on
when you press the power button) but the behaviour returns to normal
after pushing the chassis reset button, then you should change the cap.
