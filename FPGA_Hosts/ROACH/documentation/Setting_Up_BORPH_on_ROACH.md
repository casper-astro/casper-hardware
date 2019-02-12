All production ROACH boards should be pre-programmed with a working
installation of Debian GNU/Linux. If you have such a board, then you
probably want to skip ahead to [Getting Started with
ROACH](Getting_Started_with_ROACH "wikilink"). This page is intended for
anyone trying to bring up a board with nothing but U-Boot installed.

Official instructions for compiling a BORPH kernel for ROACH can be
found on Hayden So's [website](http://www.eee.hku.hk/~hso/borph.html).
This guide is an elaboration on those instructions.

## Installing BORPH on ROACH and ROACH2

These instructions assume that you are running Debian GNU/Linux.

### Install a PowerPC cross-compiler.

The folks at [emdebian.org](http://www.emdebian.org) have kindly handled
the messy work of compiling a cross-compiler so that you don't have to.
The following section just summarizes their
[instructions](http://www.emdebian.org/tools/crosstools.html).

Add the following entry to `/etc/apt/sources.list`:

``` text
deb http://www.emdebian.org/debian/ lenny main
```

Do an `apt-get update`, and then `apt-get install` the following
packages:

``` text
libc6-powerpc-cross
libc6-dev-powerpc-cross
binutils-powerpc-linux-gnu
gcc-4.3-powerpc-linux-gnu-base
g++-4.3-powerpc-linux-gnu
```

If all went well, you should have a working PowerPC cross-compiler.

``` bash
$ powerpc-linux-gnu-gcc -v
Using built-in specs.
Target: powerpc-linux-gnu
Configured with: (... blah blah blah ...)
gcc version 4.3.2 (Debian 4.3.2-1.1)
```

Congratulations.

### Cross-compile a BORPH kernel.

Create a working directory for kernel compilation. We'll assume it's
called `roach-borph-linux`.

Install the package for generating U-Boot kernel images.

``` bash
$ sudo apt-get install uboot-mkimage
```

Check out the latest BORPH Linux kernel source.

``` bash
$ svn co http://casper.berkeley.edu/svn/trunk/roach/sw/linux
```

Finally, cross-compile a kernel.

``` bash
$ cd linux
$ export CROSS_COMPILE=powerpc-linux-gnu-
$ make roach_defconfig
$ make uImage
```

### Build a minimal root filesystem.

*To be documented.*

In the meantime, you can just download our latest image.

``` bash
$ wget http://casper.berkeley.edu/software/roach/romfs
```

### Transfer images to the ROACH.

Connect to the ROACH with a serial cable. We used a USB-to-RS232
adapter, so our serial device is `/dev/ttyUSB0`. We'll assume you are in
the directory with your kernel image and your filesystem image, and that
you have `picocom` and `lrzsz`
installed.

``` bash
$ picocom --baud 115200 --send-cmd="sb -vv" --receive-cmd="rb -vvv" /dev/ttyUSB0
```

To send files with picocom, you hit C-a C-s, and it'll prompt you for
the path of the file to send.

``` text
=> run newkernel
<send uImage when prompted>
=> run newroot
<send romfs when prompted>
```

Your session should look something like this:

``` text
picocom v1.4

port is        : /dev/ttyUSB0
flowcontrol    : none
baudrate is    : 115200
parity is      : none
databits are   : 8
escape is      : C-a
noinit is      : no
noreset is     : no
nolock is      : no
send_cmd is    : sb -vv
receive_cmd is : rb -vvv

Terminal ready

=> run newkernel
## Ready for binary (ymodem) download to 0x00200000 at 115200 bps...
C
*** file: uImage
sb -vv uImage
Sending: uImage
Bytes Sent:1498368   BPS:7671
Sending:
Ymodem sectors/kbytes sent:   0/ 0k
Transfer complete

*** exit status: 0
(SOH)/0(STX)/0(CAN) packets, 3 retries
## Total Size      = 0x0016dcbe = 1498302 Bytes

.............. done
Erased 14 sectors
Copy to Flash... done
=> loady 200000;era 0xfc200000 0xfc5fffff;cp.b 0x200000 0xfc200000 0x400000
## Ready for binary (ymodem) download to 0x00200000 at 115200 bps...
C
*** file: romfs
sb -vv romfs
Sending: romfs
Bytes Sent:3908608   BPS:7871
Sending:
Ymodem sectors/kbytes sent:   0/ 0k
Transfer complete

*** exit status: 0
SOH)/0(STX)/0(CAN) packets, 5 retries
## Total Size      = 0x003ba400 = 3908608 Bytes

................................ done
Erased 32 sectors
Copy to Flash... done
=> 
```

You can now boot into your extremely minimalistic Debian system, which
consists only of Busybox on a read-only romfs root filesystem.

``` text
=> boot
```

### Install a full-featured root filesystem.

A pre-built root filesystem is now available. Unless you have good
reason, you should probably use
that.

``` bash
http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/filesystem/filesystem_etch_nfs_DATE.tar.gz
```

## External links

  - BORPH: Berkeley Operating system for ReProgrammable Hardware:
    <http://www.eee.hku.hk/~hso/borph.html>
  - Embedded Debian Cross Toolchains:
    <http://www.emdebian.org/tools/crosstools.html>
