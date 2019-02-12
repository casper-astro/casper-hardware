# Roach NFS boot setup

This is a guide to set up a Network File Sharing (NFS) filesystem on a
server, so the ROACH can boot off the network using BOOTP. It will take
you through setting up a DHCP/DNS/TFTP server, as well as setting up an
NFS share.

Some differences exist depending on the Linux OS you are using.
Initially, this guide has been written for Ubuntu, here are some of the
changes you should take into account to make it work with Scientific
Linux (and the other RHEL based Linux):

  - **sudo -\> su** (Just enter su, then your password, then the command
    which needed you to be root. To come back to normal user, enter
    exit, or press ctrl+D)
  - **apt-get -\> yum** (yum is the command used by RHEL based Linux to
    install packages)

To run some Casper tutorials, you should have at least **python-2.6**
installed on your computer. If you don't, you can encounter some
problems. Moreover, it is really difficult to update a previous python
version to 2.6 (at least under SL). Python-2.6 comes with Scientific
Linux 6.0 and above. For Ubuntu, it seems to be easier because even with
the 9.04 version the default version is python-2.6, I didn't check for
the older Ubuntu versions. So, before setting up the NFS filesystem and
configuring everything, I advise you to make sure that your python
version is 2.6 or above.

If you're staying connected to a live network, you may need to modify
some parts of this guide. Also, make sure you don't accidentally
override your college / company's DHCP server, as they probably wouldn't
be happy.

## What you'll need:

  - 1x Roach
  - 1x PC with Ubuntu
  - 1x Ethernet cross-over cable (or 2 straight Ethernet cables with a
    switch)

### What would help a lot:

  - 1x Serial cable

## Obligatory step

Connect the ROACH to your PC via an ethernet cable; you can use a
crossover cable and just connect them to each other. Or connect both of
them to the switch with 2 straight cables. Make sure you connect to the
right ethernet port on the ROACH (it's the upside down one we want - the
cable's pins face upward when connected).

## Preliminary setup 1: The serial connection

The most basic connection you can make to the ROACH is the old-fashion
serial connection. First, you'll need a serial cable. If your PC doesn't
have a serial port, you'll also need a USB-\>Serial cable. Make sure you
have a null modem cable, not a straight-through (ROACH is considered
DTE, as is your PC).

Next, you'll need to figure out your serial device. You can check on the
available tty by opening up the terminal and issuing

`ls -al /dev | grep tty*`

Find your serial port entry (it should have the property 'dialout', I'd
try /dev/ttyS0 first). If you're using a USB-\>serial cable, the chances
are ubuntu has the drivers already, and it's loaded up automatically to
/dev/ttyUSB0.

Serial works over a program called **minicom**. To install it, open up a
terminal and type

`sudo apt-get install minicom`

You might want to install lrzsz as well, which is used for y-modem file
transfer:

`sudo apt-get install lrzsz`

Once these are installed, you need to set up minicom:

`sudo minicom -s`

Once it opens, go to "serial port setup" and change

  - "Serial Device" to /dev/ttyUSB0 or as you found above
  - "Bps/Par/Bits" to 115200 8N1, if it isn't already
  - "Hardware Flow Control" to No

Then save your setup as default (dfl) and exit from minicom. Next, open
minicom from the terminal by typing

`minicom`

Finally, connect the ROACH and your computer together with the serial
cable and turn on the ROACH. If you
see:

`U-Boot 2008.10-svn2338 (Oct  5 2009 - 15:27:43)                                 `  
`                                                                             `  
`CPU:   AMCC PowerPC 440EPx Rev. A at 495 MHz (PLB=165, OPB=82, EBC=82 MHz)      `  
`      No Security/Kasumi support                                               `  
`      Bootstrap Option H - Boot ROM Location I2C (Addr 0x52)                   `  
`      32 kB I-Cache 32 kB D-Cache                                              `  
`Board: Roach                                                                    `  
`I2C:   ready                                                                    `  
`DTT:   1 is 29 C                                                                `  
`DRAM:  (spd v1.0) 512 MB                                                        `  
`FLASH: 64 MB                                                                    `  
`USB:   Host(int phy) Device(ext phy)                                            `  
`Net:   ppc_4xx_eth0                                                             `  
`                                                                               `  
`Roach Information                                                               `  
`Serial Number:    020208                                                        `  
`Monitor Revision: 8.1.0                                                         `  
`CPLD Revision:    8.0.1588                                                      `  
`                                                                               `  
`type run netboot to boot via dhcp+tftp+nfs                                      `  
`type run soloboot to run from flash without network                             `  
`type run mmcboot to boot using filesystem on mmc/sdcard                         `  
`type run usbboot to boot using filesystem on usb                                `  
`type run bit to run tests                                                       `  
`                                                                               `  
`Hit any key to stop autoboot:`

then your serial connection is working.

### serial loopback test

If you're having trouble, you can test to see if your cable is working
by loading up minicom in the terminal

`minicom`

Next, touch pins 2 and 3 ([see
here](http://support.attachmate.com/techdocs/1368.html)) together with a
paperclip or similar, to join transmit directly to receive. If you now
type into minicom, you should see letters appear.

Once you've got serial working, leave it open and minimise it, as we'll
be using it later on to check a few things.

## Preliminary step 2: The USB/MMC connection

It's nice to be able to boot off USB/MMC/SD card, so we can do some
debugging. Namely, we're going to use the mmcboot to test if DHCP server
is working. See [Enabling the MMC card
slot](Enabling_the_MMC_card_slot "wikilink") for setup details. Once
it's up and running, boot the ROACH with

`run mmcboot`

then log in as 'root'. Keep this screen open for later on.

## Everything in its right place

Take care when you want to copy a file or a directory. Make sure to
always use the command **cp --preserve=all**. The common command cp do
not preserve the security context (which is active in SL, but maybe not
in Ubuntu). If you move a file (**mv** command), it will preserve the
security context, so you don't have to worry about it. For example, for
the kernel file (uImage), if the security context is not good, it can
block its access from minicom, even if it has the full access rights\!

Before we start installing DHCP/TFTP and NFS, let's set up the
filesystem. First, create the directory srv/ in the root directory, if
it doesn't exist already. Then, create the directory /srv/roach\_boot/,
and the directory /srv/roach\_boot/boot/.

`sudo mkdir /srv`  
`sudo mkdir /srv/roach_boot`  
`sudo mkdir /srv/roach_boot/boot`

### The kernel

Download the kernel from
[here](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/linux/).
Choose the latest kernel, download it and move it to the
/srv/roach\_boot/boot/ directory you just created. If you want to copy
it instead, don't forget to preserve the security context, as written
above.

`mv uImage_you_just_downloaded /srv/roach_boot/boot/uImage`

Check that the boot/ directory and uImage file have the necessary rights
(ls -l). If not, give them the full rights with the following command:

`sudo chmod -R 777 /srv/roach_boot/boot`

We don't use the /tftpboot directory, as specified in this Guide
previously, because the new dnsmasq.conf file is now looking for the
uImage file in /srv/roach\_boot/boot. If you want to put uImage in the
tftpboot directory, you will have to edit the dnsmasq.conf file.

### The filesystem

Next, download the filesystem
[there](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/filesystem/).
Then, extract it inside the /srv/roach\_boot/
directory.

`tar -xzf where-you-downloaded-the-filesystem/filesystem_etch_someversion.tar.gz -C /srv/roach_boot/`

It should create a new directory named usbstick/. Rename this new
directory as etch/.

`mv /srv/roach_boot/usbstick/ /srv/roach_boot/etch/`

Once this is done, we can move on to installing the DHCP and NFS
servers.

## Installing dnsmasq

[DNSMasq](http://www.thekelleys.org.uk/dnsmasq/doc.html) is a
lightweight DNS/DHCP/TFTP server. To install it:

`sudo apt-get install dnsmasq`

After installing, you will need to edit the configuration file:

`sudo gedit /etc/dnsmasq.conf`

A simple configuration is shown below - copy this over to dnsmasq.conf
and save the file (you might want to make a backup
first).

`# Configuration file for dnsmasq`  
`# Edited for ROACH boot server`  
  
`# We don't want dnsmasq to read /etc/resolv.conf or anything else`  
`no-resolv`  
  
`# Assign the ROACH an IP address manually, based on its MAC`  
`dhcp-host=02:00:01:02:02:08,192.168.100.2`  
  
`# Have a DHCP address range for other things`  
`dhcp-range=192.168.100.128,192.168.100.254,12h`  
  
`# Set the location of the ROACH's root filesystem on the NFS server.`  
`dhcp-option=17,192.168.100.1:/srv/roach_boot/etch`  
  
`# Set the boot filename for BOOTP, which is what the ROACH boots over`  
`dhcp-boot=uImage`  
  
`# Enable dnsmasq's built-in TFTP server. Required for BOOTP.`  
`enable-tftp`  
  
`# Set the root directory for files availble via FTP.`  
`tftp-root=/srv/roach_boot/boot`  
  
`# Set the DHCP server to authoritative mode (then keep away from other networks!)`  
`dhcp-authoritative`  
  
`#Specify which ethernet interface you use to connect to the ROACH (eth0, eth1, eth2 ...)`  
`interface=eth1`  
  
`#May be useful if you have several ethernet interfaces`  
`bind-interfaces`

To give a quick explanation: we are setting the ROACH to always be
assigned an IP 192.168.100.2 (you can set a different IP address if you
want), our server is going to have the IP 192.168.100.1, we've enabled
TFTP, set the dhcp-boot image name to uImage and the TFTP root to
/srv/roach\_boot/boot. An alternative and [more complete
example](http://casper.berkeley.edu/wiki/images/f/fd/Dnsmasq.conf.txt)
can be found
[here](http://casper.berkeley.edu/wiki/images/f/fd/Dnsmasq.conf.txt).

Make sure to edit the dhcp-host line according to your ROACH's MAC
address. If you don't know it, maybe its name can help you. The name of
the ROACHes (written on them) can be the end of their MAC address:

For example: roach030132 -\> MAC = 02:00:00:03:01:32

If you decided not to put your uImage file in the boot directory, you
must modify this line:

`# Set the root directory for files availble via FTP.`  
`tftp-root=/srv/roach_boot/boot `

Once you've saved the config file, you need to restart dnsmasq:

`sudo /etc/init.d/dnsmasq restart`

Then, bring up the ethernet interface (choose the correct one (eth0,
eth1, eth2)), and specify the correct IP address:

`sudo ifconfig eth0 192.168.100.1 netmask 255.255.255.0 up`

And check everything looks okay:

`ifconfig`

Next to the ethernet interface you chose, you should see the MAC address
of the ROACH and the IP address you just assigned.

You should also edit the file /etc/hosts and add the line:

`192.168.100.2  roach030132`

Write the ip address you chose for your ROACH instead of 192.168.100.2,
and the name of your ROACH instead of roach030132.

If the command ifconfig is not accepted by the terminal, you need to
enter /sbin/ifconfig (It means that /sbin/ is not defined in your PATH)
or define it in your PATH by writing:

`PATH=$PATH:/sbin/`

### Testing DHCP

To test if DHCP is working, load up minicom, boot the roach off 'run
mmcboot', and see if you get assigned an IP address. If not, log in as
root, and try issuing the commands:

`dhclient -r`  
`dhclient`

which will release the current IP and request a new IP address. Then
type:

`ifconfig eth0 (or eth1 or eth2)`

to see if you are assigned an address.

If the DHCP connection doesn't work maybe it is because your firewall is
blocking some of the requests made by the ROACH. You can run tcpdump
(has to be run as root), to see the DCHP requests being made by the
ROACH. dnsmasq needs to have tcp port 53 and udp port 67 open for
listening. To check if it is the case, you can issue the command:

`netstat -l`

If these two ports are closed in your firewall, you should open them.

## Installing NFS

A good guide to NFS can be found
[here](http://www.quietearth.us/articles/2006/09/28/Ubuntu-How-to-setup-an-nfs-server).
Following this guide very closely, first we install the packages:

`apt-get install nfs-kernel-server nfs-common`

With Scientific Linux 6.1, (and maybe some more recent Ubuntu versions),
you only have to install the packages:

`yum install nfs-utils nfs-utils-lib    `

This will start multiple services, including portmap and mountd, if you
run:

`rpcinfo -p`

you'll see what's running. If you don't manage to issue rpcinfo, issue

`/usr/sbin/rpcinfo`

Or edit the PATH.

Now we can modify /etc/exports to make our file system available to the
ROACH:

`sudo gedit /etc/exports`

And add the
lines:

`# Share 'roach_boot' directory`  
`/srv/roach_boot 192.168.100.0/24(rw,subtree_check,no_root_squash,insecure)`

Where 192.168.100.0 is the subnet we assigned to the internal network
(with all your ROACHes). This share will be available to all machines
with 192.168.100.x IP addresses. Save and exit, then export the
filesystem by issuing:

`exportfs -a`

Now if we run showmount -e, we'll see the filesystem available as an
export:

`# showmount -e`  
`Export list for ROACH-dev:`  
`/srv/roach_boot 192.168.100.0`

If the command showmount -e returns an error, you have to issue the
following command before:

`service nfs start`

We should now have all the necessary components installed, so it's time
to test out to see if we can boot or not.

## Booting from NFS

If the ROACH was already on, turn it off, launch minicom in a terminal

`sudo minicom`

Then, turn the ROACH on. Sometimes, there is an error if we turn the
ROACH ON before launching minicom.

If you don't press any key, you will be running netboot because it is
the default option. If all goes well, you should
see:

`DHCP client bound to address 192.168.100.2                                      `  
`Using ppc_4xx_eth0 device                                                       `  
`TFTP from server 192.168.100.1; our IP address is 192.168.100.2                 `  
`Filename 'uImage'.                                                              `  
`Load address: 0x400000                                                          `  
`Loading: #######################################################`

And the ROACH will boot. If the ROACH isn't getting an IP address
assigned, check that your PC's eth interface is still up, by opening a
seperate terminal and issuing:

`sudo ifconfig eth0 192.168.100.1 netmask 255.255.255.0 up`

Replace eth0 by eth1 or eth2 if you don't use the same ethernet
interface.

If everything goes all right, you will be able to login. If you can't
write your login and password, it means that you forgot to set the
'Hardware Flow Control' option to 'no', in the minicom options
configuration, as specified at the beginning of this Guide.

## Permission and access problems

If minicom issues the following error, it can be because uImage doesn't
have the the necessary rights, or because of a security context
problem.

`TFTP error: 'cannot access /srv/roach_boot/boot/uImage: Permission denied' (2)`

### Rights

First, try to set up the rights to the folder boot/ and to the file
uImage.

`chmod -R 755 /srv/roach_boot/boot`

### Security context

If it doesn't solve the problem, then, it should be a problem of
'security context'. Check the context of the uImage file by issuing

`ls --context `

If you obtain

`unconfined_u:object_r:default_t:s0`

You have to change the type of the context which should not be
'default'. The easiest way can be to move the uImage file you
downloaded, from the Downloads folder to the /srv/roach\_boot/boot
directory, or to copy it with the command **cp --preserve=all**. The
command cp modifies the type of the security context so it can be the
cause of the error.

`mv uImage_file /srv/roach_boot/boot/uImage`  
`or`  
`cp --preserve=all uImage_file /srv/roach_boot/boot/uImage`

The right context type should be **tftpdir\_t**. You can also define the
context type of the file manually with the command chcon:

`chcon -t tftpdir_t /srv/roach_boot/boot/uImage`

Check that the context of uImage is now
**unconfined\_u:object\_r:tftpdir\_t:s0**, by issuing the command:

`ls --context`

# Getting started with CASPER tutorials

Now that your NFS server is configured, you should make sure that you
can successfully use it with the ROACH. For example, you can try to do
the tutorials you can find on [Casper's
website](https://casper.berkeley.edu/wiki/Tutorials).

However, before you are able to run these tutorials, some more packages
should be installed on your computer.

First, make sure that you have at least python-2.6 installed on your
computer. If you don't, you can encounter some problems with some of the
Casper's tutorials.

## Hardware and software configuration

Here is the configuration I had when I did the tutorials. If you don't
encounter the same errors or if the solutions I give do not work for
you, it may be because your configuration is different.

Hardware configuration:

  - ROACH version: ROACH v1
  - ADC card: ADC2x1000-8 (iADC)
  - Connection pc-ROACH: Serial + Ethernet (2 straight cables + a
    switch)

Software configuration:

  - OS: Scientific Linux 6.1
  - Matlab: Matlab 2009b (including Simulink)
  - Xilinx ISE: Version 11.4
  - Kernel version: uImage-fallback-20110303
  - Filesystem version: filesystem\_etch\_2010-03-24\_sd\_shipping

With other ADC cards (e.g. KatADC) you may need to make some changes in
the mdl files.

## Install some complementary packages

### Download the packages

To execute the .bof and .py files (tutorials 1 and 2), you should have
the following packages installed:

  - spead-0.4.0
  - katcp-0.3.4
  - corr-0.6.7
  - construct-2.06
  - numpy-1.3.0
  - h5py-1.3.1

These versions were the latest ones in March 2012, if newer ones are
available, you can use them.

For numpy and h5py, you should be able to install them as follow:

`yum install numpy h5py     (Scientific Linux)`  
`or`  
`sudo apt-get install numpy h5py    (Ubuntu)`

For the other ones, they may not exist in Linux repositories, so you
should download them directly from their website:

  - [Construct](http://pypi.python.org/pypi/construct)
  - [Spead](http://pypi.python.org/pypi/spead)
  - [Katcp](http://pypi.python.org/pypi/katcp)
  - [Corr](http://pypi.python.org/pypi/corr)

Here are also the Internet addresses where you can find the two packages
you should have already been able to install:

  - [H5py](http://pypi.python.org/pypi/h5py)
  - [Numpy](http://pypi.python.org/pypi/numpy)

Download the latest version of these packages.

### Install the packages

To install these packages, do as follow: Go to the directory where you
downloaded them (e.g. Downloads):

`cd your_home_directory/Downloads`

Extract the packages one by one (e.g. spead-0.4.0.tar.gz):

`tar xzf spead-0.4.0.tar.gz`

It should extract the file in a folder, go inside (e.g. spead-0-4-0):

`cd spead-0-4-0`

There, run the file **setup.py** with python, and it will install the
package on your computer:

`python setup.py install`

Proceed like that for all the packages. Now you should be able to
execute the bof files and the python files.

## CASPER tutorial 1

To test if the NFS server is working correctly with the ROACH and to see
if we can execute the .bof files, let's try to complete the tutorial 1.
You can access the latest version of the tutorial
[here](https://github.com/casper-astro/tutorials_devel/tree/master/tut1).
However, I have only tested the 2010 version, which can still be found
[here](https://github.com/casper-astro/tutorials_devel/tree/master/tuts_old/workshop_2010/roach_tut1_intro).

To go a bit faster, directly download the Matlab file (**tut1.mdl**) and
compile it with with Xilinx ISE to create the **tut1.bof** file, as
specified in the
[tutorial 1](https://casper.berkeley.edu/wiki/Introduction_to_Simulink).

Once you have created the tut1.bof file, give it the execution rights:

`chmod +x tut1.bof `

and move it in the following directory, where your ROACH will be able to
access it:

`mv tut1.bof /srv/roach_boot/etch/boffiles`

Then, log in to the ROACH. You can use minicom or ssh. With ssh don't
forget the -X option to be able to execute files:

`ssh -X username@ROACH_ip`

Once you are logged in to the ROACH, go to the directory where you put
your tut1.bof file:

`cd /boffiles`

and execute tut1.bof:

`./tut1.bof`

This should program the FPGA of your ROACH. If the process is *captured*
by your terminal, that means it is working correctly.

### glibc.i686 problem

If you encounter the following
error:

`./gen_prog_files: ./mkbof: /lib/ld-linux.so.2: bad ELF interpreter: No such file or directory`  
``chmod: cannot access `implementation/system.bof': No such file or directory``  
``cp: cannot stat `implementation/system.bof': No such file or directory``  
  
`Error using ==> gen_xps_files at 689`  
`Programation files generation failed, EDK compilation probably also failed.}`

That means that you have to install the following package:

`yum install glibc.i686`

## CASPER tutorial 2

To test if we can execute the python files (which will execute remotely
a bof file) to program the ROACH, let's try to do the tutorial 2.

Download the Matlab and python files (tut2.py and tut2.mdl) from Casper
website:

  - 2012
    [version](https://github.com/casper-astro/tutorials_devel/tree/master/tut2)
  - 2010
    [version](https://github.com/casper-astro/tutorials_devel/tree/master/tuts_old/workshop_2010/roach_tut2_10GbE)

I had some problems using the python files from the 2011 workshop, the
newest files should work fine, but if you encounter problems, I advise
you to use the files from the 2010 workshop. Create the **tut2.bof**
file with Matlab, give this file the execution rights and put it in the
**/srv/roach\_boot/etch/boffiles** directory, as you did for the
tutorial 1. Then, you should edit the tut2.py file in order to specify
the name of your tut2.bof file.

`gedit tut2.py &`

Line 28, you should see the following line: '''boffile =
'tut2\_2010\_Apr\_02\_1057.bof' '''

Replace the bof file name with your own bof file name and save the file.
Give tut2.py the execution rights and move it in the same directory as
the tut2.bof file.

`chmod +x tut2.py`  
`mv tut2.py /srv/roach_boot/etch/boffiles`

Then, you should be able to execute tut2.py file:

`python tut2.py ROACH_ip_address -p`

If you see 2 plots appearing, every thing worked all right\!
Congratulations\! If you don't, check if you are encountering one of the
following problems.

## Possible problems with tutorials 2 and 3

### 10 GbE cable problem

Make sure you connected the 10 GbE ports 0 and 3 with a cable, as
specified in the tutorial 2. You should obtain the following lines when
executing tut2.py:

`---------------------------`  
`Port 0 linkup: True`  
`Port 3 linkup: True`  
`------------------------`  

If you have a **False** instead of **True**, there is a problem with the
cable (There is no cable or the cable is not connected to the right
ports or the cable or/and the ports have a connection problem).

### GTKAgg backend problem

If you cannot execute the python file and encounter the following
error:

`Traceback (most recent call last):`  
`  File "your_file.py", line 145, in `<module>  
`    exit_fail()`  
`  File "your_file.py", line 138, in `<module>  
`    fig.canvas.manager.window.after(100, plot_spectrum)`  
`AttributeError: FigureManagerBase instance has no attribute 'window'`  
``python: Python/pystate.c:595: PyGILState_Ensure: Assertion `autoInterpreterState' failed.``  
`Aborted`

That means that it is a backend problem, as explained in the following
[webpage](http://old.nabble.com/Can-not-get-animated-matplotlib-example-to-work-td26959581.html).
You are using the GTKAgg backend (the default), whereas the script
expects the TkAgg backend.

So, you should edit the matplotlibrc file, as follow. To find the
matplotlibrc file, enter:

`find / -name "matplotlibrc*"`

Mine was located there:

`/usr/lib64/python2.6/site-packages/matplotlib/mpl-data/`

Then, edit it by replacing the line:

`backend      : GTKAgg`  
`by`  
`backend      : TkAgg`

If you don't want to edit the matplotlibrc file, it should also be
possible to run the script like this:

`python tut2.py ROACH_ip_address -p -dTkAgg `

However, I haven't tried.

### tgtap and tcpborphserver2 problems

If you have an error with **tgtap** or if you see the following lines,

`Programming FPGA... ok`  
`---------------------------`  
`Port 0 linkup:  True`  
`Port 3 linkup:  True`  
`---------------------------`  
`Configuring receiver core... FAILURE DETECTED. Log entries:`  
`192.168.100.7: Starting thread Thread-1`  
`192.168.100.7: #version poco-0.1`  
`192.168.100.7: #build-state poco-0.2804`  
`192.168.100.7: ?progdev tut2b.bof`  
  
`192.168.100.7: !progdev ok 546`  
`192.168.100.7: Programming FPGA with tut2b.bof... ok.`  
`192.168.100.7: ?read gbe0_linkup 0 4`  
  
`192.168.100.7: !read ok \0\0\0`  
`192.168.100.7: ?read gbe3_linkup 0 4`  
  
`192.168.100.7: !read ok \0\0\0`  
`None`

You should download the newest versions of **tgtap** and
**tcpborphserver2**, as explained on this
[website](http://www.mail-archive.com/casper@lists.berkeley.edu/msg02180.html).
You can download them from the following
    websites:

  - [tcpborphserver2](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/tcpborphserver/tcpborphserver2-2011-03-10-r3405-fansilent)
  - [tgtap](http://casper.berkeley.edu/svn/trunk/roach/sw/binaries/tgtap/tgtap_2010-03-24)

Once you have downloaded them, you should replace the current ones. The
tcpborphserver2 and tgtap files can be found in the following
directories:

`/srv/roach_boot/etch/usr/local/sbin/`  
`/srv/roach_boot/etch/usr/local/local/sbin/`

First, rename the current files by adding \_old at the end of their name
(do not delete them, maybe you will want to use them again later). Then,
copy your new tcpborphserver2 and tgtap files to these directories
(better use cp --preserve=all to be sure you will not encounter any
security context errors). Finally, switch OFF and ON your ROACH to
restart the minicom initialization, before trying to execute your
tut2.py file again.

### Library path problem

When executing python files (tutorial 2 or 3 for example), the computer
needs some packages you already installed but it also needs to access
these packages' library files. Usually there isn't any problem, but
sometimes it fails and these library files cannot be found. A common
reason is that you have the good package, but not the expected library
version.

As an example, I will present the problem we had recently. We installed
the HDF5 package. Then, when we executed the python file spectrometer.py
(Casper tutorial 3), it failed and we had the following
error:

`python spectrometer.py roach030287`  
`Traceback (most recent call last):`  
`  File "spectrometer.py", line 13, in `<module>  
`    import corr,time,numpy,struct,sys,logging,pylab,matplotlib`  
`  File "/usr/lib/python2.6/site-packages/corr/__init__.py", line 11, in `<module>  
`    import cn_conf, katcp_wrapper, log_handlers, corr_functions, corr_wb, corr_nb, corr_ddc, scroll, katadc, iadc, termcolors, rx, sim, snap`  
`  File "/usr/lib/python2.6/site-packages/corr/rx.py", line 9, in `<module>  
`    import h5py`  
`  File "/usr/lib64/python2.6/site-packages/h5py/__init__.py", line 24, in `<module>  
`    import h5`  
`ImportError: libhdf5.so.6: cannot open shared object file: No such file or directory `

The most important information is the last line: **ImportError:
libhdf5.so.6: cannot open shared object file: No such file or
directory**.

It means that the library file **libhdf5.so.6** cannot be found.
However, as the HDF5 package is installed, this library file should be
present.

We searched the libhdf5.so.6 file on the computer.

`find / -name "libhdf5.so.*"`

We found it in one of the computer's directory, which was not /lib64/.
As the computer is searching the library files in the /lib64/ directory
(because we are using a 64-bit Linux distribution), we created a
symbolic link named libhdf5.so.6 in the /lib64/ directory, linked to the
real libhdf5.so.6 file.

`ln -s /some_path/libhdf5.so.6 /lib64/libhdf5.so.6`

If the library file needed by your program doesn't exist on your
computer, you can also create a symbolic link linked to another version
of the library file (the newest version present on your computer). For
example, if we hadn't found libhdf5.so.6 on the computer, we would have
created the libhdf5.so.6 symbolic link and linked it to the libhdf5.so.7
file (which was also found on the computer).

`ln -s /some_path/libhdf5.so.7 /lib64/libhdf5.so.6`
