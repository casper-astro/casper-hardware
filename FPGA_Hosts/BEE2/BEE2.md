# BEE2

![thumb | BEE2 v1.0](photos/bee2.jpg)

The BEE2 (**B**erkeley **E**mulation **E**gine) is the current CASPER
workhorse signal processing platform.

Learn more about the BEE2 by visiting the BEE2 page:
<http://bee2.eecs.berkeley.edu>

Most information about setting up and using the BEE2 platform can be
found on the now-static wiki:
<http://bee2.eecs.berkeley.edu/wiki/BEE2wiki.html>

## Description

The BEE2 is a general-purpose processing module based on five
high-performance Xilinx FPGAs (Virtex-II Pro 2VP70). In addition to the
large amount of processing fabric provided by the FPGAs, the BEE2 also
provides up to 20GB of high-speed, DDR2 DRAM memory. Each of the five
FPGAs has four independent channels to DDR2 DIMMs which provides very
high memory bandwidth. Finally, the FPGAs on the BEE2 are highly
connected with both high-speed, serial and parallel links.

The FPGAs are laid out in a star/ring topology with four User FPGAs in a
ring and one Control FPGA connected to each user. The User FPGAs each
have four independent high speed serial channels (4 bonded MGTs) which
are capable of transfering data at 10Gbps through CX4 connectors (both
copper and fiber). The User FPGA ring consists of parallel connections
of 138 high-speed LVCMOS traces between the FPGAs which can run at a
maximum of 400Mbps. The Control FPGA has two high-speed serial channels,
64 LVCMOS traces to each User FPGA, and connections to common
peripherals such as 10/100 Ethernet, USB 1.1, RS232 serial, DVI, and
GPIOs.

## Specifications

- **FPGA**
  - 5x Xilinx Virtex-II Pro XC2VP70-7FF1704 FPGA

- **Interfaces**
  - 18x CX4 10Gbps high-speed serial connectors
  - 1x 10/100 RJ45 Ethernet interface
  - 1x RS232 interface
  - 1x HDMI DVI video output
  - 1x USB 1.1 USB-A jack

- **Peripherals**
  - 20x DDR2 DIMMs
  - 1x CompactFlash socket

## Versions

- **v1.0 *Deprecated***
  - Deprecated prototype version

- **v1.1**
  - Reversed 5V connector polarity to match IBOB
  - Changed main DC/DC regulator (P27, P28, P29) mounting holes to
    plated
  - Added power circuitry for Fujitsu CX4 optical transceiver
  - Adjusted CX4 connector positioning

- **v1.2**
  - Added RC circuit to aid TI DVI chip (U18) power-on reset

## Usage Manuals, Guides, Memos, etc.

- [IBOB to BEE2 Clocking](documentation/IBOB_to_BEE2_Clocking)