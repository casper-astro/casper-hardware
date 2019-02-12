# ROACH2 packetised correlator sizing

ROACH-II was primarily specified for the packetised correlator, which
was considered to be the most demanding application. This page explains
the reasoning behind choosing the memory interfaces and capacities, as
well as IO bandwidth for ROACH2.

## Executive Summary

To enable MeerKAT, PAPER and ATA correlator requirements, ROACH2 should
be upgraded over ROACH1 as follows:

- A Virtex-6 SX475 main FPGA.
- Four 36bit QDR parts of 36Mibit each (144Mibit for MeerKAT2 upgrade
  path).
- A 72-bit DRAM DIMM slot, capable of housing at least a 256MiB DIMM
  running at or faster than 250MHz DDR.
- At least four 10GbE ports (more will be needed for beamforming).

Additionally, it would be convenient to have:

- Increased PPC-FPGA datarates (32bit bus?).

## F engine coarse delay compensation

Simple delay compensation implementation requires dual-ported memory.
Likely to put in on-FPGA BRAM.

`Delay = baseline / speed of signal. Speed of signal ~ 300 000km/s.`

For each polarisation, we then need:

| Longest delay | Sample rate | Samples | Mem req'd at 8b/sample |
| ------------- | ----------- | ------- | ---------------------- |
| 1 km          | 1Gsps       | 3K      | 26Kb                   |
| 8 km          | 2Gsps       | 53K     | 417Kb                  |
| 20 km         | 1Gsps       | 66K     | 520Kb                  |
| 20 km         | 2Gsps       | 133K    | 1017Kb                 |
| 60 km         | 1Gsps       | 198K    | 1562Kb                 |
| 60 km         | 2Gsps       | 396K    | 3125Kb                 |
| 60 km         | 4Gsps       | 792K    | 6250Kb                 |

MeerKAT will see each F board processing two polarisations, so the
numbers in the table above need to be doubled.

**Conclusion**: MeerKAT-1 will require at least 2Mb of BRAM for delay
processing. Any of the Virtex-6 devices will be sufficient if ignoring
FFT, PFB and fine delay requirements.

## F engine logic requirements

Not yet thoroughly checked, but as a guideline: FFT scales linearly with
bandwidth and NlogN for Nchans. ROACH1 runs out of logic at 8k chans at
500MHz.

`PAPER's processing requirements are small.`

`MeerKAT will need at least four times the logic to do 16k chans at 1GHz. SX95 * 4 = SX380.`

## F engine corner-turn bandwidth

There is a small matrix transpose that happens inside the F engines in
order to have each packet contain data for a single antenna, single
frequency channel.

For reference, ROACH has two 18bit-wide dual-ported DDR SRAMs (QDR) and
a single 72bit wide DDR DRAM which are each presented as SDR 36 bit and
144 bit interfaces in application space.

On each FPGA clock cycle, we need to be able to read and write:

`bus_width = n_pols * n_parallel_stream * n_bits * complex`

PAPER is processing 100MHz on a 200MHz FPGA.

`PAPER-64:  8 * 0.5 * 4 * 2 = `**`32``   ``bit`**  
`PAPER-128: 8 * 0.5 * 4 * 2 = `**`32``   ``bit`**

ATA is processing two 1GHz dual pol antennas on a 250MHz FPGA.

`ATA-42:    4 *  8  * 4 * 2 = `**`256``   ``bit`**`  `

MeerKAT1 is processing 1GHz on a 250MHz FPGA.

`MeerKAT1:  2 *  4  * 4 * 2 = `**`64``   ``bit`**

MeerKAT2 is processing 4GHz on a 250MHz FPGA.

`MeerKAT2:  2 *  16  * 4 * 2 = `**`256``   ``bit`**

We're double buffering this CT at the moment and need to read from one
memory while writing to the other.

**Conclusion**: two 64 bit memory interfaces will suffice for naive
implementation up to MeerKAT1, however, ATA and MeerKAT2 require wider
bitwidths of 256bit. These are probably best implemented in 4x QDR 36
bit parts which appear as 4x 72bit SDR interfaces.

## F engine corner-turn space

For reference, ROACH1 has 2x 36Mib QDRs and 1x 1GiB DRAM.

Calculation is as follows: req'd\_mem = double\_buffer \* pkt\_len \*
n\_chans \* n\_parallel\_streams \* n\_bits \* complex.

We assume 4 bit quantisation (4b real + 4b imaginary).

`PAPER-64:    2 * 128 *  2048 * 8 * 4 * 2 = `**`32``   ``Mibit`**  
`PAPER-128:   2 * 256 *  2048 * 8 * 4 * 2 = `**`128Mibit`**  
`MeerKAT1:    2 * 256 * 16384 * 2 * 4 * 2 = `**`128Mibit`**  
`MeerKAT3:    2 * 256 * 65536 * 2 * 4 * 2 = `**`512Mibit`**

**Conclusion**: ROACH2 should have at least 128Mbit of QDR memory to
support the matrix transpose operation for MeerKAT phase-1. Should we
want to use ROACH2 for future MeerKAT phases, we will need at least 512
Mib (4x 144Mib parts) for the F engine.

## X engine VACC data rates

The X engine cores output data in windows. You need to be able to output
all baselines, all stokes complex values within one window to avoid
overflows. Each window is length n\_ants\*pkt\_size. The Xengine must
produce n\_baselines\*n\_stokes\*complex during this time.

The vector accumulator needs to be able to read and write once per
incomming value. On QDR, this is a single-clock operation (since it's
dual ported), but on DRAM, this requires two clocks.

The choice of pkt size has implications for minimum integration period.

For reference, ROACH has 2x 36bit QDR interfaces and 1x 144bit DRAM
interface.

If we assume the use of QDR for the VACC, which allows simultaneous
reads and writes, the following table
results...

| n\_ants | pkt\_size | clk\_avil | n\_bls | demux           | clk\_req'd |
| ------- | --------- | --------- | ------ | --------------- | ---------- |
| 32      | 128       | **4096**  | 528    | 8 (32bit VACC)  | 4224       |
| 32      | 128       | **4096**  | 528    | 4 (64bit VACC)  | 2112       |
| 64      | 128       | **8192**  | 2080   | 8 (32bit VACC)  | 16640      |
| 64      | 256       | **16384** | 2080   | 8 (34bit VACC)  | 16640      |
| 64      | 128       | **8192**  | 2080   | 4 (64bit VACC)  | 8320       |
| 64      | 256       | **16384** | 2080   | 4 (68bit VACC)  | 8320       |
| 64      | 128       | **8192**  | 2080   | 2 (128bit VACC) | 4160       |
| 128     | 128       | **16384** | 8256   | 2 (128bit VACC) | 16512      |
| 128     | 256       | **32768** | 8256   | 2 (136bit VACC) | 16512      |
| 128     | 512       | **65536** | 8256   | 2 (144bit VACC) | 16512      |
|         |           |           |        |                 |            |

**Conclusion**: We will need a single 128-bit interface for larger
numbers of antennas (128). For smaller numbers of antennas (\<=64), we
would like multiple 64-bit interfaces. QDR is more convenient for these
VACCs as implementation is easier. Multiple QDR parts would give
additional flexibility in terms of how the memory is arranged (single
large databus vs multiple smaller busses). Thus, there should be at
least four 32-bit interfaces, which can be configured for use as four
stand-alone VACCs, or combined in parallel to use as two 64-bit VACCs or
as a single 128-bit interface. Four 36-bit QDR parts are appropriate,
and match Feng corner-turn bandwidth requirements (see above section).

## X engine VACC capacities

Each X engine processes a subset of frequency channels. The number of X
engines required scale with the bandwidth you're processing. If your
FPGAs are running at the same speed as the bandwidth you're processing,
then you need one X engine for every F engine. However, if you're
processing a wideband design (eg 1GHz with FPGAs running 250MHz) then
you need X:Y times more X engines than F engines where X is the
bandwidth you're processing and Y is the FPGA clock rate of your X
engine.

For PAPER, 100MHz is processed on FPGAs running at 200MHz. Thus there
are twice as many F engines than X engines.

MeerKAT-1 will likely process 1GHz of bandwidth on FGPAs at 250MHz (four
times as many X engines as F engines).

MeerKAT-2 will likely process 4GHz of bandwidth on FPGAs at 250MHz (16
times as many X engines as F engines).

Each baseline has 4 stokes, 32 bit complex numbers (=256bits per
baseline).

Data must be double-buffered, so that previous accumulation can be read
out slowly while capturing next
accumulation.

| n\_ants | n\_chan | n\_xeng | n\_bls | mem\_per\_xeng | n\_xeng\_per\_roach2 | total\_double\_buff\_mem\_per\_roach2 |
| ------- | ------- | ------- | ------ | -------------- | -------------------- | ------------------------------------- |
| 32      | 2048    | 64      | 2080   | 4.125Mb        | 4                    | **33Mb**                              |
| 64      | 2048    | 32      | 2080   | 32.5Mb         | 2                    | **130Mb**                             |
| 64      | 4096    | 256     | 2080   | 8.13Mb         | 2                    | **33Mb**                              |
| 128     | 2048    | 64      | 8256   | 64.5Mb         | 1                    | **130Mb**                             |
| 128     | 16384   | 512     | 8256   | 64.5Mb         | 1                    | **130Mb**                             |
| 128     | 65536   | 512     | 8256   | 258Mb          | 1                    | **512Mb**                             |
|         |         |         |        |                |                      |                                       |

**Conclusion:** ROACH2 should have at least 130Mb of QDR memory for
MeerKAT1. MeerKAT-2/3/4 can switch to DRAM VACC at the expense of
minimum dump times (see section above on VACC bandwidth).

## Minimum integration period

This is affected linearly by number of freq channels and packet size.

Ignoring VACC output datarates, yields the following possible
example:

`   Processing 2GHz bandwidth, 16384 chans, minimum integration as follows:`  
`       1/2GHz:           0.5ns`  
`       16384 chans:   *16384`  
`       pkt size:      *  256`  
`                     =========`  
`       min dump time: ~2.048ms  `

However, this ignores the fact that with the current design, the VACC
uses spare FPGA cycles to retrieve the previous accumulation while
storing the current one. If the data is forwarded through the VACC, how
quickly you can retrieve the previous accumulation (even if only
integrating a single spectrum) thus depends on your board clock rate and
the ratio of number of valid clocks to idle clocks. Realistically, this
will be another factor of \~4, increasing the minimum dump time in
afore-mentioned example to \~8ms. Alternatively, you can bypass the VACC
entirely and output at a fixed \~2ms period.

## Switch and 10GbE requirements

Calculating maximum analogue bandwidth transportable over a 10GbE
link...

`Digital link: 156.25MHz*4*20, but 8/10 encoding -> 10Gbps.`

Channel utilisation:

|                            | Bits | %pkt |
| -------------------------- | ---- | ---- |
| Layer1 overhead            | 160  | 6%   |
| Ethernet header and footer | 160  | 6%   |
| IPv4 header                | 160  | 6%   |
| UDP header                 | 64   | 2%   |
| Application header         | 64   | 2%   |
| Data payload               | 2048 | 77%  |
|                            |      |      |

`77/100 * 10Gbps = 7.7Gbps max application usable per 10GbE link. `

Assuming 4 bit complex data: 7.7/8b = 962.5MHz total. With 2 pols
(single antenna/F engine per board) gives **max 481MHz bandwidth per
link**.

- This does NOT account for any out of band signalling (currently used
  for heartbeat signals and legacy data synchronisation).
- This does NOT conform to SPEAD packet formats. It is the maximum
  efficiency we can get away with.
- Not recommended to run links near 100%.

**Conclusion**: MeerKAT1 will thus require at least **four ports per
board** (X engines need two to switch and two to F engines to carry 1GHz
bandwidth).
