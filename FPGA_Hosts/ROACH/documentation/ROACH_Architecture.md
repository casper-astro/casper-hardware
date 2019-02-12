The ROACH architecture can be broken down into 3 major and relatively
independent subsystems. A ROACH board consists primarily of the FPGA
subsystem, the PowerPC subsystem, and the monitor/management subsystem.

![Roach.png](Roach.png "Roach.png")

## Monitor/Mangement Subsystem

The MonMan subsystem of the ROACH board provides remote power control
and system status monitoring. An Actel AFS600 mixed-signal FPGA (*U60*)
implements the core functionality. The Actel FPGA handles the ATX power
supply's soft power toggling and monitors the voltage and current draw
of key power supplies on the board using on-chip ADCs. It can shut down
power to the board in the event of a power supply being out of range.
This subsystem is powered from the ATX auxiliary 5V power rail, and thus
remains on even after a board powerdown.

Remote access to the Actel FPGA is enabled by the Lantronix XP1001001
XPORT (*J28*). The XPORT encapsulates serial I/O with the Actel in an
Ethernet format, which allows switched-network communication with the
FPGA.

## PowerPC Subsystem

The PowerPC subsystem is intended to be the primary command/control
mechanism for the ROACH board, along with moderate-bandwidth data I/O
with the Xilinx FPGA. Its primary component is the AMCC PowerPC 440EPx
embedded processor (*U1*). The PowerPC's functions are supported by DDR2
DRAM (*J8*), a Spansion 16x32M flash memory (*U42*), and a National
DP83865 10/100/1000 Ethernet PHY (*U58*). An Xilinx XC2C256 CPLD (*U54*)
acts as an interface to additional I/O capability for the PowerPC,
including a MMC/SD memory card socket (*J17*).

## FPGA Subsystem

ROACH is built around the primary FPGA (*U15*) for its signal processing
capabilities. Either a Xilinx Virtex 5 XC5VLX110T or XC5VSX95T can be
populated. The FPGA is connected to a number of peripherals and I/O
interfaces; high-bandwidth data I/O is accessible via 2 Tyco Z-DOK+
40-pair host connectors (*P5*, *P7*) and 4 CX4 ports for XAUI/10GbE
(*P3*). Memory is available in both DDR2 DRAM (*J15*) and QDRII+ SRAM
(*U17*, *U63*).
