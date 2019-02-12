Due to the system complexity, a cold bringup of ROACH requires several
steps to configure the necessary subsystems. This page will serve as a
general overview of the process, while detailed descriptions and
instructions will be provided in other documentation.

<font color=red> **Please note that this procedure is performed at the
assembly house (Digicom) before shipment. All boards are shipped fully
configured and tested. You do not need to repeat this process yourself
upon receipt of a ROACH board. This page serves as a reference so you
can see what was done to the board you received.** </font>

The general flow of bringing up a newly-assembled ROACH board consists
of the following steps:

  - Visual inspection of the board for fabrication and assembly problems
  - Ensure no shorts across all power supply rails
  - Configure XPORT device to allow remote communications access
  - Configure Actel FPGA to enable power monitor/management
  - Configure CPLD to support PowerPC operations
  - Configure PowerPC with u-boot bootloader
  - Write BORPH operating system into flash memory
  - Load Xilinx FPGA with test bitstream
  - Run FPGA tests

This process can currently be done using an automated script provided by
KAT/SKA-SA. The script ties together the individual steps. The script
runs on a [ROACH test machine](ROACH_test_machine "wikilink").

## Visual Inspection

Visual inspection of each board should be performed to ensure all
components are correctly populated and with the right orientation.
Polarized passives and ICs should have orientation markers on the
silkscreen. Previously-identified problem areas should be given extra
attention and be inspected under magnification, if necessary.

## Power Distribution Check

Before powering on a board, any power/ground shorts should be identified
to help prevent damage to the board. ROACH has a fairly complex power
system, subdivided and soft-switched.

### ATX power system

ROACH is powered using a industry-standard ATX power supply, which
provides soft on/off capabilities. Details about the ATX power supply
standard can be found at
<http://en.wikipedia.org/wiki/Atx#Power_supply>. ATX power is brought in
on connector **J18**, which is also a convenient place to probe the ATX
power nets. J18 follows the ATX standard pinout, as shown from a
top-down view with J18 on the right-hand side of the board:

![ROACH\_ATX\_Power.png](ROACH_ATX_Power.png "ROACH_ATX_Power.png")

### On-board power system

On-board power is regulated primarily from the ATX5V and ATX12V. ATX12V
is used to generate 2.5V (*U53*), 1.8V (*U27*), and 1.5V(*U51*). ATX5V
is used to generate 1.0V (*U50*), while ATX5VAUX is used to generate
3.3VAUX (*U59*), which is in turn used to generate 1.5VAUX using an
on-chip regulator on the Actel FPGA (*U60*). All of the major voltage
rails have voltage and current sense resistors connected to the Actel,
which also double as probe points.

## XPORT Configuration

The first step in board configuration is the XPORT (*J28*), which allows
the test computer to remotely access the ROACH board during the process.
The XPORT should be connected to the proper Ethernet port on the test
computer using a Cat5 cable. When the XPORT is in its factory-default
unconfigured state, it will seek an IP address using DHCP. It does this
at increasing intervals, starting at 15 seconds after powerup. Because
the test script has a timeout in establishing communications with the
XPORT for the first time, it is easiest to power cycle the board at this
step, forcing the XPORT to reboot and seek its address at the proper
time.

Once the XPORT receives a known IP address using DHCP, a configuration
file is copied to the XPORT device via its telnet interface. This setup
file sets the network interface of the XPORT and gives it a static IP
address of 192.168.4.20. After this configuration, the XPORT is
rebooted. When the XPORT comes back up with it static IP, the telnet
interface is used for the remaining configuration parameters. Predefined
strings are output over **netcat** to emulate a user interacting with
the XPORT's telnet console. This method is used to configure the XPORT's
3 GPIOs and to set the XPORT's CPU into High-Performance mode.

When the XPORT configuration is completed, the test computer will be
able to communicate with the Actel monitor. An XPORT daemon (process
name **xportd**) is kept running in the background for the test scripts
to use to communicate with the XPORT.

## Actel Monitor/Management Configuration

The Actel FPGA (*U60*) is programmed using an Actel FlashPro3 USB
programmer and the FlashPro software. The Actel FPGA has a dedicated
JTAG connector (*J27*) for this purpose. Programming consists of 2
parts. The first step programs the FPGA fabric with a STAPL
(**roach\_monitor.stp**) file. In the second step, a UFC file with the
board's serial number is generated, which is written into the
non-volatile FlashROM memory on the FPGA.

Once configured, the Actel FPGA will be able to control the main power
supplies of the board. It has been observed that the board requires a
*hard* power cycle(that is, the board needs to stop receiving **all**
power, including the ATX5VAUX, so either unplug the ATX power connector
at J18, switch off the ATX power supply, or unplug the ATX power supply)
before the interaction between the XPORT, the Actel FPGA, and the ATX
power supply will behave correctly.

## CPLD Configuration

A Xilinx CPLD (*U54*) provides secondary I/O for the PowerPC, including
interfaces for boot settings. Once the Actel is powered and the ATX
power supply can be switched on, the CPLD is next in the configuration
sequence. The CPLD is configured using the Xilinx Parallel IV
programming cable and the iMPACT software. On early revisions of the
ROACH board (v1.00, v1.01), the CPLD has a dedicated JTAG connector
(*J22*). Starting with board ROACH v1.02, both Xilinx chips (*U15* and
*U54*) are cascaded on a single JTAG scan chain from connector *J24*.

The CPLD is configured using a JEDEC configuration bitstream
(**roach\_cpld.jed**).

## PowerPC U-Boot Configuration

To get the PowerPC running, the processor must first be initialized so a
bootloader can be loaded into its nonvolatile memory. This is done using
a Macraigor usbWiggler USB JTAG debugger and OCD Commander software,
which connects to the PowerPC's JTAG port *P1*. OCD Commander will load
the **program.mac** macro to force the PowerPC to configure its serial
port to 115200-8N1 and initialize its 16kB of on-chip memory.

The macro will put the PowerPC's program counter in the right place
(0x71000000) and prepares it to receive data. The U-Boot binary
**uboot.bin** is transferred to the PowerPC using the **serial\_ptys**
process to implement the **XMODEM** protocol over a serial port
connection from the test computer to *P2*. The data is written to the
on-board Spansion flash memory (*U42*).

With U-Boot loaded into flash memory, the PowerPC will be able to boot
at powerup and present a simple user interface that is accessible
through the serial port *P2*.

## PowerPC BORPH Configuration

Once U-Boot is loaded and running, the PowerPC is also capable of
loading and running a basic BORPH Linux. U-Boot is prepared for booting
Linux by first setting network and memory parameters. The
**serial\_ptys** process is again used to establish a serial port
connection to the PowerPC from the test computer. This emulates user
interaction with the U-Boot shell. The test script uses this connection
to set a MAC address for the PowerPC's 10/100/1000 Ethernet interface
(*U58*, *J25*) as an environment variable in U-Boot. It also sets an
environment variable containing memory mapping information for booting
Linux. These environment variables are stored with U-Boot in flash
memory.

With a MAC address available from U-Boot, the PowerPC should be able to
bring up its Ethernet interface. A functional network interface on the
ROACH board will allow a DHCP server to start on the test computer,
which then assigns a dynamic IP address to the PowerPC. At this point, a
TFTP server needs to be running on the test computer's 192.168.3.2
network connection. Using the serial port, the test computer will cause
the PowerPC to request a TFTP transfer of the kernel image **uImage**.
Upon successful transfer, the test script will have the PowerPC to erase
a sector of flash memory and copy the kernel image to flash. It then
requests a TFTP transfer of the filesystem image **romfs**, which is
also copied to flash after transfer.

If all the environment variables are correctly set and the Linux images
properly TFTP'd and written to flash, the ROACH board will be issued a
software reset, causing the PowerPC to reboot and boot into BORPH Linux.

## FPGA Configuration and Testing

At this point, the configuration of the Mon/Man and PowerPC subsystems
is complete, and the testing of the FPGA components can commence. The
board is reset again and BORPH loading is interrupted so that the
PowerPC boots into U-Boot. The PowerPC's network interface is also
started and tries to request an IP address from the test computer's DHCP
server.

At this point a USB memory key with FPGA test bitstream
**roach\_bsp.bin** must be inserted into USB port *P6*. The serial
interface is used to run a test sequence preloaded in the U-Boot image.
The test sequence first tests components on the PowerPC subsystem:

  - Serial port connection (*P2*)
  - Proper configuration and communication with the CPLD (*U54*)
  - PowerPC DDR2 DRAM access (*J8*)
  - Flash memory access (*U42*)
  - USB communication (*P6*) by trying to read **roach\_bsp.bin**
  - SelectMAP communication between PowerPC and FPGA
  - MMC socket (*J17*) via I/O on the CPLD (*U54*)
  - PowerPC Ethernet interface (*U58* and *J25*) by trying to request an
    IP address on DHCP and initiate a TFTP transfer of blank file
    **tftpfile.img**

Most of the above tests are redundant; if the board configuration has
completed successfully up to this point, then only the USB, SelectMAP,
and MMC interfaces are untested.

The SelectMAP interface communication between the PowerPC and FPGA is
tested by attempting to program the test bitstream from the USB key onto
the FPGA (*U15*). This interface also allows the PowerPC to interact
with the FPGA, which initiates tests on the FPGA and reads back the
results.

The test sequence then uses the FPGA bitstream to test the following
components attached to the FPGA:

  - FPGA DDR2 DRAM (*J15*)
  - QDRII+ SRAM 0 (*U63*)
  - QDRII+ SRAM 1 (*U17*)
  - ADC 0 (Z-DOK+ interface *P5*; connectivity check via loopback
    tester)
  - ADC 1 (Z-DOK+ interface *P7*; connectivity check via loopback
    tester)
  - 10GbE 0 (*P3*)
  - 10GbE 1 (*P3*)
  - 10GbE 2 (*P3*)
  - 10GbE 3 (*P3*)
