This is a practical guide to debricking ROACHes by debugging the AMCC
PPC440EPx using only free (as in freedom) software.

## History

It is quite easy to clobber uboot on a ROACH, particularly during an
update. Up to now the suggested recourse was buying an Macraigor Wiggler
programmer (or equivalent) and using the OCD Commander tool. Both the
programmer and the tool are proprietary and don't work well under Linux.
However, you were left with no alternative as the PPC JTAG COP debug
interface is kept very secret.

There is now an alternative...

## The Solution

We have done some work on reverse engineering the JTAG COP interface and
this has borne fruit. At the moment I have a python script which
converts Macraigor .mac scripts into urjtag jtag scripts. Urjtag is a
GPL'ed, general purpose JTAG interface tool which can use a number of
jtag programmers. Debian and Ubuntu users will be able to "apt-get
install urjtag".

## The Programmer

In theory any number of programmers could be used. However, I have only
tested the solution using an Amontec JTAGKey Tiny programmer. This is an
FTDI-chip based programmer (a chip very similar to that on ROACH 2). You
can view the Amontec site for more details:
<http://www.amontec.com/jtagkey-tiny.shtml>

## The converter script

The converter script is at the link below. Its not by any means final
(or pretty), but it seems to work:
<http://casper.berkeley.edu/svn/trunk/roach/production/test_software/ocdc_macro_convert/ocdc_macro_convert.py>

I have also checked in the urjtag script which loads rinit (Marc Welz's
mini-bootloader for roach):
<http://casper.berkeley.edu/svn/trunk/roach/production/test_software/ocdc_macro_convert/urjtag_scripts/load_rinit.urj>

which is a converted version of:
<http://casper.berkeley.edu/svn/trunk/roach/production/test_software/roach_testing/macraigor_macros/loadram_rinit_auto.mac>

To generate a urjtag script: run ./ocdc\_macro\_convert.py mymacro.mac
\> myurjtag.urj

## Connecting the programmer

You connect it as expected except the Amontec CPU RESET pin needs to be
attached to the halt pin. You may need to have a look at the ROACH 1
schematics to get the pin locations.

## Loading the rinit boot loader

from the command line run "urjtag load\_rinit.urj"

## Loading uboot

Once rinit is loaded you need to send the uboot image via xmodem on the
serial port.

## Contact

If you are interested in using/extending this tool it may be beneficial
to contact the author. David George: dgeorgester (@t) gmail {d0t}
\[c0m\].
