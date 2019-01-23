# IBOB to BEE2 Clocking

A common application is to run a synchronous system using IBOBs and
BEE2s. In this case, the IBOB receives its clock from an ADC, and in
turn needs to forward that clock to the BEE2.

This page provides hints for creating such a setup. It also provides
some fixes to potential signal integrity problems.

## Sending the Clock

A clock of any power-of-2 division of the IBOB clock is easily generated
using one bit from a counter. A 1-bit counter is recommended for
providing a half-rate clock, which is then easily doubled by using
**usr\_clk2x** on the BEE2.

If a full-rate clock is required, it can be generated using a DDR output
GPIO.

## Signal Integrity Issues

*User Clock* is distributed on the BEE2 using a Maxim MAX9310A clock
driver. The bulk of the signal integrity problems are due to the fact
that the MAX9310A is an LVPECL-to-LVDS driver, but the IBOB can only
provide LVCMOS or LVTTL outputs. Solutions are to rig an
LVCMOS-to-LVPECL driver at the IBOB, AC couple to a proper LVPECL DC
level at the BEE2, or both.

Tests from UCB RAL Digital Lab showing the signal integrity results of
driving the cable, cable with passive rebiasing, adding an active
driver, and adding an active driver with passive rebiasing.

- [Usrclock\_tests\_2007sep20.pdf](Usrclock_tests_2007sep20.pdf)
- [Usrclock\_tests\_2007oct09.pdf](Usrclock_2007oct09.pdf)

An implementation of the LVPECL driver is made available by Caltech/JPL
below. The PCB is designed to be snapped apart to provide both the
active driver circuit and the passive termination/rebiasing circuit in a
single fab. The board is designed using the freeware version of the
[Eagle PCB software](http://www.cadsoftusa.com/).

- [Ibob\_bee2\_clock.zip](Ibob_bee2_clock.zip)
