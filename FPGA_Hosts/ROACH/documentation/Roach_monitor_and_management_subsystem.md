ROACH has an onboard, fully independent management subsystem that can be
accessed through the Xport.

This system monitors voltages and temperatures and will shut down the
board if these are out of specification. A log is kept of the shutdown
cause, its value and time.

# Clients

There is a simple Python-based script available
[here](http://casper.berkeley.edu/svn/trunk/roach/sw/roach_monitor/roach_monitor.py)
for querying the Actel Fusion over the Xport.

# Default values

|               |                     |                |               |
| ------------- | ------------------- | -------------- | ------------- |
| Channel index | Channel Description | Shutdown under | Shutdown over |
| 7             | 12V                 | 10.0V          | 14V           |
| 10            | 5V                  | 4.4V           | 5.6V          |
| 13            | 3V3                 | 3.0V           | 3.6V          |
| 16            | 1V                  | 0.9V           | 1.05V         |
| 22            | 1V5                 | 1.4V           | 1.55V         |
| 25            | 1V8                 | 1.7V           | 1.9V          |
| 28            | 2V5                 | 2.45V          | 2.54V         |
| 0             | 1V5aux              | 1.4V           | 1.6V          |
| 3             | FPGA temperature    | N/A            | 95degC        |
| 9             | PPC temperature     | N/A            | 70degC        |
| 31            | Actel temperature   | N/A            | 70degC        |
|               |                     |                |               |

When an automated shutdown occurs, the index of the faulty channel as
well as it's value can be read out over the X-port. The X-port's default
IP address is **192.168.4.20** when shipped from Digicom.
