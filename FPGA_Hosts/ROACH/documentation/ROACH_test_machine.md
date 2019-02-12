This section will list all the hardware and software needed to configure
and build a ROACH test machine.

## Computer Hardware

  - Minimum requirement: Intel Pentium D CPU 3.00GHz with 2 GB of RAM
  - Harddisk space of at least 30 GBytes to install the required
    software.
  - 2 x 1-Gig Ethernet ports, 3 if an outside link is required
  - Serial port (DB-9 male or provided by USB to serial port adapter)
  - 3 x USB ports in addition to keyboard and mouse ports

## Support Hardware

  - Xilinx JTAG Platform Cable (USB)
      - P/N HW-USB-II-G or UW-USB-II-G $225 or $125 respectively at
        <http://em.avnet.com>
  - Actel FlashPro3 (USB)
      - P/N Actel FlashPro3 $138 at
<http://www.mouser.com/>

`  The Actel FlashPro3 is marked end of life and may be hard to find.`  
`  The current product is the FlashPro 4; however this unit has not been exercised`  
`  as of 2010mar22.`

  - Macraigor usb2Demon or usbWiggler (USB)
      - P/N Macraigor usbWiggler for AMCC 4xx (U2W-AMCC) PowerPC
        processor $250 at <http://www.macraigor.com/>
  - USB flash drive formatted with partition 1 as FAT32 to store the
    roach\_bsp.bin test files (exercise via uboot's run bit)
      - roach\_bsp.bin may be found at
        <http://casper.berkeley.edu/svn/trunk/roach/production/test_software/roach_testing/gen_files/fpga>
        copy latest version to top level directory of this USB flash
        drive and rename to roach\_bsp.bin.
  - USB flash drive (512MB or larger) formatted with partition 1 as ext2
    to store the Linux Root file system.
      - The 2010mar24 version is [filesystem.tar.gz 150MB (gzipped
        tar)](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/filesystem/filesystem_etch_2010-03-24_sd_shipping.tar.gz).
      - Boot to this USB flash drive via uboot's run usbboot.
      - after uboot's soloboot or mmcboot one may mount this USB flash
        drive.
      - See [Getting Started with
        ROACH](http://casper.berkeley.edu/wiki/Getting_Started_with_ROACH)
        for more details.

## Cables and connectors

  - Standard serial cable (default is DB-9 socket at both ends)
  - 2 x Ethernet cables (or 3 if outside link is required)
  - 2 x 10GigE CX4 cables (for loopback tests)
  - 2 x CASPER Z-DOK Loopback tester boards

## Primary Software

  - Windows XP Professional 32-bit version
      - this is mandatory due to device drivers for at least some of the
        support hardware listed above.
  - Cygwin standard install from <http://www.cygwin.com/> and the
    following
  - as installed on the RAL test machine (some may not be required):
  - running cygcheck -s on the RAL machine returns with :
    [RAL\_cygcheck.txt
    (txt)](http://casper.berkeley.edu/wiki/images/3/32/RAL_cygcheck.txt)
      - Base package
      - autoconf
      - automake
      - bash
      - bzip2
      - dbus
      - diffutils
      - docbook
      - grep
      - inetutils
      - netcat
      - openssh
      - perl
      - ping
      - propcs and psmisc (for pkill and the like)
      - python
      - rxvt VT102
      - screen
      - shutdown
      - subversion
      - tar
      - TCL/TK
      - termcap
      - time
      - transfig
      - unzip
      - vim
      - W32API
      - xaw3d
      - xdelta-devel
      - xdpinfo
      - zip

<!-- end list -->

  -   - for development and extra work environments
          - binutils
          - gcc C, gcc g++ (C compiler and utilities)
          - X11 see
            <http://x.cygwin.com/docs/ug/setup-cygwin-x-installing.html>

<!-- end list -->

  - SolarWinds TFTP
        Server
      - <http://www.solarwinds.com/products/freetools/free_tftp_server.aspx>
      - Version 9.2.0.111 requires about 10 MBytes of disk space. Other
        versions may be acceptable too.
  - Actel FlashPro Version: 8.5.0.34, Release
        v8.5
      - <http://www.actel.com/download/software/libero/libero85rl.aspx#download>

`  Actel Libero release v8.5 is an out of date version and requires about 1.1 GByte of disk space.`  
`  If a copy is not available from Actel directly a copy can be made from an existing setup.`

  - Xilinx Impact Version: 10.1.03 (nt), Application Version: K.39
      - actually only the WebPACK 10.1 package is required and can be
        found at
      - <http://www.xilinx.com/webpack/classics/wpclassic/>
      - WebPACK 10.1 is a 2.3 GByte download and requires 4.6 GByte of
        disk space.
  - Lantronix DeviceInstaller 4.2.0.1 for XPort 1001001-03R Embedded
    Ethernet Device
        Server
      - <http://www.lantronix.com/device-networking/utilities-tools/device-installer.html>
      - DeviceInstaller requires 6 MByte of disk space.
  - OCD Commander Version: 2.5.6 for Macraigor Systems usbWiggler
      - <http://www.macraigor.com/ocd_cmd.htm>
      - OCD Commander requires about 13 MByte of disk space.
  - Virtual Serial Ports Emulator
      - this may not be required. The RAL ROACH test machine uses this
        to access the
      - USB\<-\>serial port adapter. With this a hyperterminal window
        may be used to snoop
      - on the serial link for low level debugging.
      - <http://www.eterlogic.com/Products.VSPE.html>

## Secondary Software

  - TightVNC Server to allow remote experts to access the local test
    machine for black-belt debugging.
      - Stable revision 1.3.10 from
        <http://www.tightvnc.com/download.php>
  - Wireshark network analyzer to monitor network traffic across the 3
    NICs
      - stable version 1.2.6 <http://www.wireshark.org/download.html>
  - Winscp scp and sftp client
      - revision 4.2.7 from <http://winscp.net/eng/index.php>
  - Powerarchiver 2001 or equivalent to process bzip2, gzip, rar, zip,
    and related archives.
      - <http://www.powerarchiver.com/>

## Contact

For any ROACH issues, check the [CASPER mailing list
archive](http://www.mail-archive.com/casper@lists.berkeley.edu/) or
submit questions to the CASPER [Mailing List](Mailing_List "wikilink").
