# Agilent N9310A Remote Control

The Agilent N9310A RF Signal Generator supports remote control via USB
using SCPI commands. This is useful for instrument characterization, as
it enables automated sweeping of input signals over a wide parameter
space.

## Configuration

In Linux, install the USBTMC kernel driver.

1.  Download the source code [here](code/Usbtmc.tar).
2.  To compile: untar; run `make`.
3.  To install: Run the `usbtmc_load` script as root.

The script will create devices `/dev/usbtmc[0-9]`.

## Usage

The `usbtmc0` device is a meta-device that keeps track of all available
devices. To see what devices are available, `cat /dev/usbtmc0`.

For all other `usbtmc` devices, the general mode of interaction is to
`echo` SCPI command strings to the device. If you send a command that
should return a response (ie, commands that end with a question mark),
you must then `cat` the device to read the response.

For a list of available commands, check the SCPI Command List in the
"Remote Control" section of the [Agilent N9310A Quick Start Guide](documentation/N9310AQuickStartGuide.pdf‎).

## Sample Code

The following example bash script enables simple interaction with an
N9310A device.

We'll assume that your device is connected and registered as
`/dev/usbtmc1`.

``` bash
#!/bin/bash

DEVICE=/dev/usbtmc1

case "$1" in
    freq)
        if [[ -n $2 ]]
        then
            echo FREQ:CW $2 MHz > $DEVICE
        else
            echo FREQ:CW? > $DEVICE
            cat $DEVICE
            echo
        fi
    ;;
    ampl)
        if [[ -n $2 ]]
        then
            echo AMPL:CW $2 dBm > $DEVICE
        else
            echo AMPL:CW? > $DEVICE
            cat $DEVICE
            echo
        fi
    ;;
    rf)
        case "$2" in
            on)
                echo RFO:STAT ON > $DEVICE
            ;;
            off)
                echo RFO:STAT OFF > $DEVICE
            ;;
            *)
                echo RFO:STAT? > $DEVICE
                state=`cat $DEVICE`
                if ((state))
                then
                    echo "RF is on."
                else
                    echo "RF is off."
                fi
            ;;
        esac
    ;;
    *)
        echo "Usage:"
        echo "  $0 freq [frequency in MHz]"
        echo "  $0 ampl [amplitude in dBm]"
        echo "  $0 rf [on/off]"
        exit 1
    ;;
esac

exit 0
```

## References

  - [What is USBTMC and How Can I Communicate with My USB Instrument?](http://digital.ni.com/public.nsf/allkb/044FA220F32774ED86256DB3005850CA)
  - [Linux Test Automation](http://www.agilent.com/find/linux)
  - [USBTMC Kernel Driver Documentation](http://www.home.agilent.com/upload/cmc_upload/All/usbtmc.html)
