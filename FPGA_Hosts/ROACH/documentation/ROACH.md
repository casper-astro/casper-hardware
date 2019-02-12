![ROACH v1.00](Roach_v1_00.jpg "ROACH v1.00")

![The official ROACH logo.](ROACHLogo.png "The official ROACH logo.")

ROACH (**R**econfigurable **O**pen **A**rchitecture **C**omputing
**H**ardware) is a standalone FPGA processing board.

## History

ROACH is a Virtex5-based upgrade to current CASPER hardware. It merges
aspects from the [IBOB](IBOB "wikilink") and [BEE2](BEE2 "wikilink")
platforms into a single board. The CX4/XAUI/10GbE interfaces of both are
kept, while combining the Z-DOK daughter board interface of the IBOB
with the high bandwidth/capacity DRAM memory and standalone usage model
of the BEE2. ROACH is a single-FPGA board, dispensing with the on-board
inter-FPGA links in favor of 10GbE interfaces for all cross-FPGA
communications.

## Description

![The ROACH block diagram.](Roach.png "The ROACH block diagram.")

The centrepiece of ROACH is a Xilinx Virtex 5 FPGA (either LX110T for
logic-intensive applications, or SX95T for DSP-slice-intensive
applications). A separate PowerPC runs Linux and is used to control the
board (program the FPGA and allow interfacing between the FPGA "software
registers/BRAMs/FIFOs" and external devices using Ethernet).

Two quad data rate (QDR) SRAMs provide high-speed, medium-capacity
memory (specifically for doing corner-turns), and one DDR2 DIMM provides
slower-speed, high-capacity buffer memory for the FPGA. The PowerPC has
an independent DDR2 DIMM in order to boot Linux/BORPH.

The two Z-DOK connectors allow ADC, DAC and other interface cards to be
attached to the FPGA, in the same manner as the [IBOB](IBOB "wikilink")
allowed (with backwards compatibility for the ADC boards used with the
IBOB).

Four CX4 connectors provide a total of 40Gbits/sec bandwidth for
connecting ROACH boards together, or connecting them to other
XAUI/10GbE-capable devices (such as [BEE2](BEE2 "wikilink") boards,
computers with 10GbE NICs and 10GbE switches).

For more detailed descriptions, see: [ROACH
Architecture](ROACH_Architecture "wikilink")

## Specifications

  - **FPGA**
      - 1x [Xilinx Virtex-5 XC5VLX110T-1FF1136 **or** Virtex-5
        XC5VSX95T-1FF1136](http://www.xilinx.com/support/documentation/data_sheets/ds100.pdf)
        FPGA

<!-- end list -->

  - [ **FPGA Interfaces**](ROACH_FPGA_Interfaces "wikilink")
      - 2x Z-DOK+ 40 differential pair connectors
      - 4x CX4 10Gbps high-speed serial connectors
      - 1x QSH 40 differential pair connector
      - 16x GPIO
      - 4x SMA IO (2x clock-capable)

<!-- end list -->

  - **FPGA Peripherals**
      - 2x 2M x 18-bit QDRII+ SRAMs
      - 1x DDR2 DRAM DIMM

<!-- end list -->

  - **CPU**
      - 1x [AMCC
        PowerPC 440EPx](https://www.amcc.com/MyAMCC/retrieveDocument/PowerPC/440EPx/PPC440EPx_DS2023.pdf)
        Embedded Processor

<!-- end list -->

  - **CPU Interfaces**
      - 1x RS232 DB9 serial port
      - 1x 10/100/1000Mbit RJ45 Ethernet
      - 1x USB2.0
      - 1x MMC/SD card socket

<!-- end list -->

  - [ **Monitor and
    Management**](Roach_monitor_and_management_subsystem "wikilink")
      - Temperatures of Xilinx Virtex5, PowerPC and Actel Fusion.
      - Voltages of 12V, 5V, 3.3V, 2.5V, 1.8V, 1.5V, 1V and 1.2V aux
        rails.
      - Automated shutdown in the event of over temperature, over or
        under voltage with logging of last shutdown event.
      - Remote power on/off.
      - Separate 100Mbps Ethernet port for independent board control and
        health monitoring.

<!-- end list -->

  - **Block Diagram**
      - [v1.02 Block Diagram
        (PDF)](http://casper.berkeley.edu/hardware/roach_v1_02_blkdgrm.pdf)

<!-- end list -->

  - **Schematics**
      - [v1.01 Schematics
        (PDF)](http://casper.berkeley.edu/hardware/roach_v1_01_sch.pdf)
      - [v1.02 Schematics
        (PDF)](http://casper.berkeley.edu/hardware/roach_v1_02_sch.pdf)
      - [v1.03 Schematics
        (PDF)](https://casper.berkeley.edu/wiki/images/6/6e/Roach103.pdf)

<!-- end list -->

  - **Gerbers**
      - [v1.0.3 Gerbers, see readme for
        details](http://casper.berkeley.edu/wiki/images/7/78/Roach103-CAM.zip)

A free PADS PCB viewer is available from
<http://www.mentor.com/products/pcb-system-design/design-flows/pads/pads-pcb-viewer>

## Development

The details about development progression have been moved to its own
dedicated page: [ROACH Development](ROACH_Development "wikilink").

![ROACH on lab bench with two iADC's](roach_with_iadcs_on_bench.jpg
"ROACH on lab bench with two iADC's") ![ROACH in 1.3U ATX
case](Roach_in_1.3U_atx_case.jpg "ROACH in 1.3U ATX case") ![Production
ROACH v1.02 in 1.3U ATX case](Roach_v1_02_proto.jpg
"Production ROACH v1.02 in 1.3U ATX case")

## How to get it

Production is managed by Mo at [Digicom](http://www.digicom.org). Send
production-related inquiries to `mo at digicom dot org`. Production is
grouped into batches, so lead times are variable.

## Usage Manuals, Guides, Memos, etc.

  - [Getting Started with ROACH](Getting_Started_with_ROACH "wikilink").
  - [ Latest firmware and software versions](LatestVersions "wikilink").
  - [ Initial bringup, configuration, and test
    process](ROACH_Bringup "wikilink").
  - [ROACH test machine](ROACH_test_machine "wikilink").
  - [ UBOOT and kernel update
    procedure](ROACH_kernel_uboot_update "wikilink").
  - [ Setting up BORPH on ROACH](Setting_Up_BORPH_on_ROACH "wikilink").
  - [ ROACH configuration memo (KAT-7 DBE internal memo
    008)](Media:NRF-KAT7-5.0-MEM-008_ROACH_config.pdf‎ "wikilink").
  - [ ROACH onboard monitor and management
    subsystem](Roach_monitor_and_management_subsystem "wikilink").
  - [ KATCP for remote control over the network](KATCP "wikilink").
  - [ZDOK Pin Numbering](ZDOK_Pin_Numbering "wikilink").
  - [ROACH NFS guide](ROACH_NFS_guide "wikilink")
  - [ ROACH Enclosure/Chassis](Enclosures "wikilink").
  - [ Latest Versions](LatestVersions "wikilink").
  - [ ROACH DDR2 Modules](ROACH_DDR2_Memory_Modules "wikilink").
  - [ How to debrick your roach using an open source alternative to OCD
    commander](ROACH_Debricking "wikilink").
  - [ ROACH USB issue and workaround](ROACH_USB_issue "wikilink")
  - [ RFI Tests](RFI_Tests_of_ROACH_1 "wikilink")
  - Sync inputs
      - The Roach1 has (at least) two possible sync inputs:
          - Via the vertical mount SMA receptacle connectors J12. The
            signal input to this connector is terminated into 50 ohms
            and then turned into the LVDS differential pair
            "GPIO\_CLK0\_P" and "GPIO\_CLK0\_N" via an Analog Devices
            ADCMP605BCPE comparator and enters the FPGA at pins G15/G16.
          - Via the vertical mount SMA receptacle connectors J13. The
            signal input to this connector is terminated into 50 ohms
            and then turned into the LVDS differential pair
            "GPIO\_CLK1\_P" and "GPIO\_CLK1\_N" via an Analog Devices
            ADCMP605BCPE comparator and enters the FPGA at pins H14/H15.
      - These pins are available in the standard CASPER tools libraries.

## Contact

For any ROACH issues, submit questions to the CASPER [Mailing
List](Mailing_List "wikilink").
