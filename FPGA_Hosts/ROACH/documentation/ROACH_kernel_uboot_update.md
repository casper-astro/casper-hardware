This page contains the instructions to update UBOOT and the Linux kernel
on ROACH.

# Image Locations

[UBOOT
image](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/uboot/)

[Linux kernel
image](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/linux/)

# Requirements

  - ROACH loaded with a previous version of UBOOT
  - Terminal Software
      - Linux – Minicom
      - Windows – HyperTerminal

# ROACH Connections

  - Power – J18 (ATX power connector)
  - Chassis connections – J7
      - Power switch – 1 & 3
      - Chassis LED0 – 5(+) & 9(-)
      - Chassis LED1 – 11(+) & 13(-)
  - Serial – P2 (Serial port for communication with the PowerPC)

# Upgrade Procedure

These insructions are for HyperTerminal in Windows, Minicom Y-modem
transfer procedure will be different but the UBOOT instructions remain
the same.

1.  Start HyperTerminal in Windows
      - Serial settings:
          - BAUD = 115200
          - 8 Bits
          - no parity
          - stop bit
          - no flow control
2.  Transfer UBOOT binary (HyperTerminal Instructions)
    1.  Press hardware reset button and / or power-up ROACH
    2.  Stop autoboot (press any key when prompted)
    3.  Type “run newuboot”
    4.  Click “Transfer” → “Send File” on the command bar of terminal
        window.
    5.  Select “ymodem” protocol
    6.  Browse to UBOOT bin file
    7.  Click “Send”
    8.  Wait for transfer to finish
3.  Reset UBOOT environment and boot settings
    1.  Type "run clearenv"
    2.  Type “reset”
    3.  Stop autoboot
    4.  Type "run init\_eeprom"
    5.  Set required MAC address: type “setenv ethaddr
        xx:xx:xx:xx:xx:xx” (MAC address suggestion:
        02:00:00:serial\_number)
    6.  Type “saveenv”
    7.  Type "reset"
    8.  Stop autoboot
4.  Transfer Kernel Image
    1.  Type “run newkernel”
    2.  Click “Transfer” → “Send File”
    3.  Select “ymodem” protocol
    4.  Browse to kernel image
    5.  Click “Send”
    6.  Wait for transfer to finish

# Notes

The update can be done faster by loading the images via network or USB.
See the UBOOT macros that are run by typing "printenv" when UBOOT is
running in a terminal to determine commands to program these images to
the correct locations.
