**KATCP** stands for Karoo Array Telescope Control Protocol. It is a
syntax specification for controlling devices over a TCP or RS232 link.
It is the preferred remote command and control interface for ROACH, and
all ROACH boards ship with a daemonized KATCP server.

This page serves as a **Getting started** guide for users. Software
developers trying to implement a KATCP client or server should consult [
Memo \#32](media:NRF-KAT7-6.0-IFCE-002-Rev4.pdf‎ "wikilink") for
detailed information about the KATCP protocol.

## Connecting

Open a telnet connection to your ROACH board on port 7147. Multiple
connections are supported, but you will need to manage this yourself
(ie, if two connections try to read/write to a register at the same
time, the behaviour is undefined).

## Command syntax

All commands are prefixed with a question mark (*?*) and suffixed with a
carriage-return or line-feed. To get a list of available commands, type

`?help`

and press **enter**.

Every command will produce a response.

All responses are prefixed with an exclamation mark (*\!*) followed by
the command name and a response of the form *ok*, *invalid* or *fail*.
For multi-line responses, each line will start with a hash (*\#*), and
the final line will consist an exclamation mark (*\!*) followed by the
number of lines you should have received.

These characters (*?*, *\#*, *\!*) are all reserved characters. If you
want to use one of them, you must escape it with a preceding backslash
(*\\*). See section *2.1: Message grammar* of [ the KATCP
memo](media:NRF-KAT7-6.0-IFCE-002-Rev4.pdf‎ "wikilink") for details.

Arguments are separated by spaces. While you *can* put escaped spaces or
tabs in an argument, we recommend that you avoid doing so because it
makes parsing the results more difficult. Try to use underscores
instead.

## Clients and servers

**Generic KATCP Client/Server Libraries:**

  - [Python library](http://pypi.python.org/pypi/katcp) - Karoo Array
    Telescope Communication Protocol library Python reference
    implementation
  - [C
    Library](http://casper.berkeley.edu/svn/trunk/roach/sw/tcpborphserver/katcp/)
  - [Ruby library](http://rb-katcp.rubyforge.org/) - Ruby/KATCP is a
    Ruby extension for communicating via KATCP
  - No Java library available yet.

**ROACH-specific Clients:**

  - [Python client](http://pypi.python.org/pypi/corr) - Correlator
    Control package
  - [C
    Client](http://casper.berkeley.edu/svn/trunk/roach/sw/tcpborphserver/)
  - [Ruby client](http://rb-katcp.rubyforge.org/) - Ruby/KATCP includes
    a core KATCP client and a tcpborphserver2 (i.e. ROACH) client
  - [ Matlab Client](Matlab_KATCP "wikilink")
  - No Java client available yet.

There is a ROACH-specific
[wrapper](https://github.com/ska-sa/corr/blob/master/src/katcp_wrapper.py)
in the Python client package.

## ROACH-specific commands

In addition to the core KATCP commands (eg, *help*, *restart*,
*sensor-list*, etc.), there are some new commands specifically for
interfacing with FPGAs.

What follows is a list of the basic ones, along with their usage. This
list will probably not be kept up-to-date, so for the latest info,
you'll want to call `help` on any particular function from within a
Python shell.

### wordwrite

Writes data words to a named register.

### write

Writes arbitrary data lengths to a named register. Only useful once the
FPGA has been programmed.

`?write `<register name>` `<offset>` `<data>

Where:

  - *register name* is a string value as listed in *listdev*.
  - *offset* is an integer specifying the byte offset from where you'd
    like to start writing.
  - *data* is the raw binary data stream to write to that location (big
    endian).

### wordread

Reads data words from a named register.

### indexwrite

Writes arbitrary data lengths to a numbered register. This was important
on clients with low-power CPUs (BEE2) where the overhead to match
strings for register names caused significant bottlenecks. Registers
could thus be indexed using an integer lookup. ROACH's PPC440 is
significantly faster and does not suffer from this limitation. It is
here for legacy purposes, but has not been tested with ROACH.

### indexread

Reads arbitrary data lengths from a numbered register. This was
important on slower clients (BEE2) where the overhead to match strings
for register names caused significant bottlenecks. Registers could thus
be indexed using an integer lookup. ROACH's PPC440 is significantly
faster and does not suffer from this limitation. It is here for legacy
purposes, but has not been tested with ROACH.

### read

Reads arbitrary data lengths from a named register. Only useful once the
FPGA has been programmed. Data is returned in big-endian format.

`?read `<register name>` `<offset>` `<size>` `

Where:

  - *register name* is a string value as listed in *listdev*.
  - *size* is an integer in bytes (keep multiple of four) of how much
    data you'd like to retrieve from this register.
  - *offset* is an integer specifying the byte offset from where you'd
    like to start reading.

### listdev

Displays available device registers. Only useful if the FPGA is
programmed. Newer tcpborphserver2 versions accept an optional *size*
argument which causes device sizes to be given as well.

`?listdev [size]`

### listcmd

Displays available Linux shell commands. Takes no arguments. This is
useful for remotely executing a custom script. There will be a command
available in a future release for executing these commands (*execcmd*).

`?listcmd`

### listbof

Displays available BORPH bitstreams in /boffiles. Takes no arguments.
When running *progdev*, you need to supply the string argument exactly
as printed here.

`?listbof`

### status

Displays image status information. Takes no arguments.

If the FPGA is programmed, you'll receive a string with the path to the
hardware registers, eg **\!status ok /proc/335/hw/ioreg**.

If the FPGA is not programmed, you'll receive a status error.

`?status`

### progdev

Programs the FPGA with a BORPH bitstream. See the **listbof** command
for available bitstreams.

`?progdev <my_borph_file.bof>`

### sensor-list

Lists sensors. The intention is to add fan speeds, voltage rails and
temperatures here, however, this is not yet available. It will conform
to generic KATCP sensor requirements when released.

### tap-start

Program a 10GbE device and start the TAP driver.

`?tap-start `<device_name>` `<mac>` `<ip>` `<port>

Where:

  - *device\_name* is a string: name of the device. *(eg: gbe0)*
  - *mac* is an integer: MAC address, 48 bits. *(eg: 00:12:34:56:78:9a)*
  - *ip* is an integer: IP address, 32 bits. *(eg: 10.0.0.2)*
  - *port* is an integer: port of fabric interface, 16 bits. *(eg:
    10000)*

*Note: The first two hex digits of the MAC address must be zeroes for it
to be valid.*

*Note: Old versions only support one instance of tgtap. Please update
your tcpborphserver if you need multiple instances.*
