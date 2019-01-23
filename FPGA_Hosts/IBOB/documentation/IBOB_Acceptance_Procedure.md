# IBOB Acceptance Procedure
  
- Reference to Peter McMahon's [Parspec](https://github.com/casper-astro/publications/blob/master/Projects/files/parspec.md) user
  manual, "A 400MHz 1024-channel 2-input, 10GbE Spectrometer for the
  Parkes Radio Telescope, V1.0".
- First, it has to eat power, DC 5V, there is only on power input in
  the board. 2A is OK for no iADC.
- Upon the iBOB have in hand, there is a label with "programmed and
  tested" by Digicom. That means there is a firmware in it, even you
  have no idea of what exactly is in the board.
- Don't download anything by Xilinx's software.
- Connect the RS232 from iBOB to your computer. For my instance,
  Windows XP's hyper-terminal was used. Users have to make your own
  adaptor from standard RS232 9 pins connector convert to the one row
  5 pins on iBOB board as illustated in the picture. Be careful about
  the Tx and Rd pins, if it doesn't work, try to reverse it,then try
  again.
- Digicom's factory setting is 115200, 8 bit None parity ,1 stop bit,
  no flow control.
- If everything works well and have good luck too, the terminal should
  have message as the following :
- I disconnected the iADC for safety, turned out errors pop out as it
  showed.

![iBOB080814_small.png](../photos/iBOB080814_small.png)

\-------------------------------------------------------------

`****************************                             `  
`* TinySH lightweight shell *                            `  
`****************************    `  
`Design name : ibob_ata_ddc                          `  
`Compiled on : 29-Jun-2007 03:51:00                                  `  
`DON'T PANIC ;`  
`Type 'help' for help                    `  
`Type '?' for a list of available commands                                         `  
`ERROR: ADC clocks not fed properly                                   `  
`Initializing LWIP Memory Structur                                 `  
`Starting Network Interface...                               `  
`Intializing netif...                     `  
`Done.       `  
`Initializing TCP Stack...                           `  
`Done.       `  
`Initializing UDP...                     `  
`Done.      `  
`Setting up LWIP interace...                            `  
`Adding interface as default...                               `  
`Attempting bootstrap....                         `  
`Telnet Server Running.                       `  
`LWIP initialization complete.             `  
`               `

  - If want to know the IP address of the board, key in *ifconfig*, it
    will reply you the information like this:

`MAC Address: 00-6D-12-1B-80-00                              `  
`IP Address: 169.254.128.0                          `  
`Subnet Mask: 0.0.0.0                    `  
`GW Address: 0.0.0.0                    `  
`Telnet Port: 23               `

  - Once you got the IP of your iBOB, try to connect it by *telnet*(
    This can be easily executed in Linux).:

`[homin@homin ~]$ telnet 169.254.128.0`  
`Trying 169.254.128.0...`  
`Connected to 169.254.128.0 (169.254.128.0).`  
`Escape character is '^]'.`  
`****************************`  
`* TinySH lightweight shell *`  
`****************************`  
`Design name : ibob_ata_ddc`  
`Compiled on : 29-Jun-2007 03:51:00`  
`DON'T PANIC ;-)`  
`Type 'help' for help`  
`Type '?' for a list of available commands`  
`IBOB %`

  - The functions of RS232 and Ethernet are the same i guess, users can
    use its to access the TinyShell.
  - At this point, the RS232 and Ethernet port have worked well.
  - You might just plug the iADC directly as i did. The LEDs on the
    front panel was blinking in a fast way, that is due to the 2A power
    supply isn't big enough. Replaced it by a 10A power supply as
    specified in this Wiki, it backs to work.
