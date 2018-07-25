# casper-hardware
A repository of information and source files for toolflow-supported hardware

# FPGA Hardware Platforms

## Hardware Compatibility Matrix

![FPGA Hardware Matrix](https://github.com/casper-astro/casper-hardware/raw/master/FPGA_Hosts/hw-matrix.png)

## SKARAB hardware configurations available

## Toolflow supported hardware configuration:

The following configuration is supported by the toolflow:

![Toolflow Supported Architecture](https://github.com/casper-astro/casper-hardware/raw/master/FPGA_Hosts/skarab-toolflow-supported-architecture.png)

There is no yellow block for the ADC Mezzanine Card currently.The 40GbE yellow block only supports a single Tx/Rx link (port 0) and can only be fitted to Mez3 – firmware limitation due to SKA-SA BSP being integrated with 40GbE yellow block. The HMC Mezzanine cards can be placed on Mez0, Mez1, Mez2, but not Mez3.

## Non-Toolflow supported hardware configurations:

The following configuration is not supported by the toolflow:

![Non-Toolflow Supported Architecture](https://github.com/casper-astro/casper-hardware/raw/master/FPGA_Hosts/skarab-toolflow-non-supported-architecture.png)

This configuration and those listed below are not supported by the toolflow and SKA-SA BSP at this stage. 

The hardware can support the following configurations of Mezzanine Cards (not shown above):

1) 40GbE Mezzanine Cards (4 ports) x 4 
2) HMC Mezzanine Cards, 4GB, 2 link x 4
3) ADC Mezzanine Cards, 3GSPS, 14 bits, JESD204B, 4 channels x 4 (TBC due to external I/O needed from SKARAB Motherboard – it can definitely handle 1 board)

## Custom Boards

[IBOB](https://github.com/casper-astro/casper-hardware/wiki/IBOB)

[BEE2](https://github.com/casper-astro/casper-hardware/wiki/BEE2)

[ROACH](https://github.com/casper-astro/casper-hardware/wiki/ROACH)

[ROACH2](https://github.com/casper-astro/casper-hardware/wiki/ROACH2)

[SNAP](https://github.com/casper-astro/casper-hardware/wiki/SNAP)

[SNAP2](https://github.com/casper-astro/casper-hardware/wiki/SNAP2)

[SKARAB](https://github.com/casper-astro/casper-hardware/wiki/SKARAB)


## Commercial Boards

# ZDOK ADCs

# ZDOK DACs

# FMC ADCs
