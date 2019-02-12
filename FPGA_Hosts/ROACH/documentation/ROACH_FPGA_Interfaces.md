## Z-DOK+

**2x 40 differential pair connectors** for ADCs, DACs, etc.

## CX-4

**4x 10Gbps network connector** for asynchronous high-speed
communication.

These are the four rectangular connectors on a bulky metal riser on the
back of the board. They are numbered from 0-3, starting on the
bottom-right and proceeding counter-clockwise.

  - For communication via a switch or with a computer, use the
    [10GbE](10GbE "wikilink") block.
  - For board-to-board communication, use either the
    [10GbE](10GbE "wikilink") or [XAUI](XAUI "wikilink") block.

## QSH

**1x 40 differential pair connector** for direct board-to-board
communication.

## GPIO

**16x general purpose input/output** for low-speed control/status bits.

These are split between two 20 pin headers: J9 (aka GPIO A) and J2 (aka
GPIO B). Each header has 8 GPIO pins, a +3.3V pin, and 11 grounds.

  - The data pins are labeled 0-7 on the board.
  - The +3.3V pin is above data pin 0.
  - The ground pin is below data pin 7.
  - All pins on the other half are grounded.

Each header has one direction selector that controls the direction of
all 8 pins. For more information, see the [GPIO](GPIO "wikilink") block
documentation.

For cabling convenience, two of the GPIO pins are also wired to SMA
connectors. Pins 6 and 7 of GPIO A are wired to J11 and J10,
respectively.

*The GPIO inputs take 0-3.3V, ranging from DC to \~10MHz.*

## SMA

**2x aux\_clk SMAs** for auxiliary clock inputs.

These are the two SMA headers next to the Z-DOK+ connectors: J12 and
J13. These correspond to aux0\_clk and aux1\_clk, respectively. These
are buffered through a high-speed comparator, making them ideal clock
inputs.

The comparators are rated for up to 500MHz. The inputs are clamped to
0-5V and are DC coupled. If AC coupling is needed, remove R111 (and the
input signal will then roll off below \~10MHz).

*The aux\_clk inputs take 0-5V, ranging from DC to 500MHz.*

They are accessible as FPGA clock inputs from the XSG Core Config block.

**2x GPIO SMAs** for low-speed digital I/O.

These are the two SMA headers next to the CX-4 connectors: J11 and J10.
These are wired to GPIO A data pins 6 and 7, respectively. Unlike the
other GPIO pins, these are 50 ohm terminated.

*The GPIO SMA inputs take 0-3.3V, ranging from DC to \~10MHz.*

For more information, see the [
GPIO](ROACH_FPGA_Interfaces#GPIO "wikilink") section above.
