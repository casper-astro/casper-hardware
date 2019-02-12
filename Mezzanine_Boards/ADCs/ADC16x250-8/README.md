# ADC16x250-8

# Board Varieties

In total there are 5 different varieties of the ADC16x250-8
board as shown below. The main difference is in the type of analog
input: differential or single ended.

Of the 5 varieties only 2 are current and available from CASPER's
standard supplier Digicom. The 3 other board varieties are
deprecated/obsolete and are NOT available from Digicom.

## Current Varieties

### Differential Analog Inputs (RJ-45)

- [ADC16x250-8 RJ45 rev 1](ADC16x250-8_RJ45_rev_1/README.md) Q2: 2013 - present
    - 16, 8 or 4 differential inputs over a total of 4 RJ45 CAT7 cables
    - run time programmable operating modes :
        - 16 inputs by 250 MSPS by 8 bits (default)
        - 8 inputs by 500 MSPS by 8 bits
        - 4 inputs by 1000 MSPS by 8 bits

### Single Ended Analog Inputs (SMB)

- [ADC16x250-8 coax rev 2](ADC16x250-8_coax_rev_2/README.md) Q2: 2013 - present
    - 16, 8 or 4 single-ended inputs over a total of 16,8 or 4 50 ohm coax cables.
    - Connector is SMB due to 1U chassis faceplate space limitations.
    - run time programmable operating modes :
        - 16 inputs by 250 MSPS by 8 bits (default)
        - 8 inputs by 500 MSPS by 8 bits
        - 4 inputs by 1000 MSPS by 8 bits

## Deprecated/Obsolete Varieties : NOT FOR NEW DESIGNS

### Differential Analog Inputs (VPORT) : NOT FOR NEW DESIGNS

- [ADC16x250-8 differential rev 1](ADC16x250-8_differential_rev_1/README.md) NOT FOR NEW DESIGNS
    - 16,8 or 4 differential inputs over 1 Samtec VPORT (VPSTP/VRDPC) cable
    - run time programmable operating modes :
        - 16 inputs by 250 MSPS by 8 bits (default)
        - 8 inputs by 500 MSPS by 8 bits
        - 4 inputs by 1000 MSPS by 8 bits

<!-- end list -->

- [ADC16x250-8 differential rev 2](ADC16x250-8_differential_rev_2/README.md) NOT FOR NEW DESIGNS: never built.
    - 16,8 or 4 differential inputs over 1 Samtec VPORT (VPSTP/VRDPC) cable
    - run time programmable operating modes :
        - 16 inputs by 250 MSPS by 8 bits (default)
        - 8 inputs by 500 MSPS by 8 bits
        - 4 inputs by 1000 MSPS by 8 bits

### Single Ended Analog Inputs (SMB) : NOT FOR NEW DESIGNS

- [ADC16x250-8 coax rev 1](ADC16x250-8_coax_rev_1/README.md) NOT FOR NEW DESIGNS
    - 16, 8 or 4 single-ended inputs over a total of 16,8 or 4 50 ohm coax cables.
    - Connector is SMB due to 1U chassis faceplate space limitations.
    - run time programmable operating modes :
        - 16 inputs by 250 MSPS by 8 bits (default)
        - 8 inputs by 500 MSPS by 8 bits
        - 4 inputs by 1000 MSPS by 8 bits


# ADC16 Sample Rate vs Virtex-6 MMCM Limitations

The ADC16 data is sent to the ROACH2's Virtex 6 FPGA over high speed
serial links. The timing of these high speed serial data streams can be
quite tight at high frequencies. The FPGA provides de-serializer blocks
to handle this, but they rely on a high quality clock to properly
capture the data bits. The ADC16 provides a clock to the FPGA that is
based on the sample clock, which is typically a high quality clock. The
ADC line clock (i.e. output clock) drives a Multi-Mode Clock Manager
(MMCM) which derives several other clock signals of related frequencies
and phases needed by the ADC receive logic and much of the user design
as well. The MMCM has two modes of operation: "low bandwidth mode" and
"high bandwidth mode". The exact meaning of these terms is not well
defined in the Xilinx documentation.

Configuring the ADC16's MMCM such that it runs in "low bandwidth mode"
leads to unreliable data capture from the ADC despite extensive probing
showing that the input clocks were of very high quality. Configuring the
MMCM to run in "high bandwidth mode" resulted in vastly superior
performance in terms of data capture reliability.

The MMCM configuration involves several parameters that interact with
each other and the input clock frequency and the MMCM's limits. In order
to be in high bandwidth mode, the frequency at the MMCM's Phase
Frequency Detector (PFD) must be between 135 and 450 MHz for the -1
speed grade FPGAs on the typical ROACH2. Its Voltage Controlled
Oscillator (VCO) frequency is limited in the -1 FPGAs to the range 600
to 1200 MHz and the minimum value for the MMCM's CLKFBOUT\_MULT\_F
attribute is 5. Thus, for -1 speed grade Virtex-5 FPGAs, the maximum
frequency at the PFD is really limited to 240 MHz (1200/5). To further
complicate things, the DIVCLK\_DIVIDE attribute cannot be set to 3 or 4
if the input clock frequency is greater than 315 MHz.

For the standard "-1" speed grade FPGAs used on the ROACH2, the MMCM
cannot be configured to run in "high bandwidth mode" for all ADC sample
clock frequencies, so some sample clock frequency ranges are not usable.

The combined effects of all these constraints for the -1 speed grade
FPGAs are summarized in the following table (all frequencies are in
MHz). Note that the frequency of the ADC line clock is 0.5x, 1x, or 2x
the sample clock frequency depending on the ADC16 input mode chosen by
the
user.

| Line Clock | DIVCLK\_DIVIDE | PFD Freq | CLKFBOUT\_MULT\_F | VCO Freq | MMCM BW |
| :--------: | :------------: | -------: | :---------------: | :------: | :-----: |
|   60-100   |       1        |   60-100 |       10.0        | 600-1000 |   LOW   |
|  100-135   |       1        |  100-135 |        8.0        | 800-1080 |   LOW   |
|  135-240   |       1        |  135-240 |        5.0        | 675-1200 |  HIGH   |
|  240-270   |       2        |  120-135 |        8.0        | 960-1080 |   LOW   |
|  270-480   |       2        |  135-240 |        5.0        | 675-1200 |  HIGH   |
|  480-500   |       5        |   96-100 |       10.0        | 960-1000 |   LOW   |

The ADC sample clock frequencies corresponding to line clock frequencies
are shown here for various numbers of inputs (all frequencies in
MHz):

| Line Clock | Msps (16 in) | Msps (8 in) | Msps (4 in) | Recommended | Supported |
| :--------: | :----------: | :---------: | :---------: | :---------: | :-------: |
|   60-100   |    30-50     |   60-100    |   120-200   |     NO      |    NO     |
|  100-135   |   50-67.5    |   100-135   |   200-270   |     NO      |    NO     |
|  135-240   |   67.5-120   |   135-240   |   270-480   |     YES     |  NOT YET  |
|  240-270   |   120-135    |   240-270   |   480-540   |     NO      |    NO     |
|  270-480   | **135-240**  | **270-480** | **540-960** |     YES     |    YES    |
|  480-500   |   240-250    |   480-500   |  960-1000   |     NO      |    NO     |
