# Avalanche photo diode (APD)

Avalanche photo diode (APD) power supply unit (PSU)

## APD Description

A small circuit board that uses a 90 VDC input to generate a user
controllable 60 to 80 VDC regulated output with setting and regulation
to 0.03 V with 2 uAmp typical load. To protect the APD load, under any
condition, the maximum current must be limited to 20 uAmp.

APD Design constraints

  - physical: none. Expected size is a few inches by a few inches (XY)
    and no particular limits on the Z axis either.
      - Include some sort of mounting scheme for standoffs or something
        for use on the lab bench.
      - larger leaded, old fashioned, low density components are
        preferred where possible to easy lab testing and customization.
        Avoid small surface mount parts where possible.
      - Circuit board layers: 2 (for minimal cost and simple assembly
        and rework).
  - input power connector: none.
      - input PSU will be something like the [CircuitSpecialist.com CS12001X](http://www.circuitspecialists.com/benchtop-power-supply-csi12001x.html)
        for users in the US with 120VAC @ 60 Hz.
      - International users, with different VAC such as 240 VAC @ 50 Hz,
        will need to find their own external PSU.
  - output power connector: none.
  - Control
      - Strictly manual.
      - No computer control, eg I2C or GPIB, of any type required.
      - No on off switch. Output disabled by turning off the external
        PSU or disconnecting the input wires.
  - Monitor
      - Strictly manual.
      - Must include test points for external test equipment, such as
        multimeter or oscilloscope, to probe the board.
      - input voltage and current monitoring: none. these will be
        checked manually.
      - output voltage and current monitoring: none; these will be
        checked manually.
  - storage temperature range: none.
  - operating temperature range: none. But expect 0 to 45 degC is a
    useful starting point.
  - altitude: none. But expect 0 to 10,000 ft is a useful starting
    point.
  - electrical efficiency: none.
  - vibration: none.
  - radiation: none.

## APD inputs

  - 2 wires 18 to 30 AWG
      - wire 1: VDC nominally 5 VDC or more above the required max
        output VDC.
          - Maximum VDC input : 100 VDC
          - Maximum VAC input : 0 VAC
      - wire 2: VDC return aka ground

## APD outputs

  - 2 wires 18 to 30 AWG
      - wire 1: manually set VDC for the APD load. Nominally 60 to 80
        VDC ?
          - Maximum output current : 20 uAmp.
          - Please note: this board has lots of capacitors. It can take
            35 seconds or more from when the input power is removed
            until the output is completely 0 Volts and 0 Amps.
      - wire 2: VDC return aka ground
      - the input and output grounds are identical. There is only this 1
        ground. There are no separate chassis, signal, safety, ...
        grounds.
      - The 4 mounting holes are directly connected to the 1 ground.

## APD Design Files

  - **Block Diagram**
      - TBD

<!-- end list -->

  - **APD Schematics**
      - [design files (tar)](schematics/APD_PSU_Altium_2013sep10.tar)
      - [schematics (pdf)](schematics/APD_PSU_schematics_2013sep10.pdf)

<!-- end list -->

  - **APD Layout design files**
      - [actual Altium design files (tar)](schematics/APD_PSU_Altium_2013sep10.tar)
      - this tar file exactly the same as the schematic tar file above.

<!-- end list -->

  - **APD Layout Gerber files**
      - [Gerber files for fabrication (TAR)](gerbers/APD_PSU_Gerbers_2013sep10.tar)

<!-- end list -->

  - **APD Bill of Material (BOM)**
      - [Bill of Materials (pdf)](BOM/APD_psu_rev_1_2013oct08b.pdf)

<!-- end list -->

  - **APD datasheets**
      - [TI datasheets (tar)](datasheets/APD_PSU_TI_datasheets_2013dec21.tar)
      - [non-TI datasheets (tar)](datasheets/APD_PSU_datasheets_2013dec21.tar)
      - there are 2 tar files for the datasheets because of wiki upload
        file size restrictions.

## APD photographs

Blank Circuit Board

  - [blank PCB (pdf)](photos/APD_blank_PCB_2013dec21.pdf)

Assembled Circuit Board

  - [populated PCB (pdf)](photos/APD_PCB_2013dec21.pdf)

Reworked Circuit Board

  - what rework ?

## APD Test Results

  - [raw low level lab tests (txt)](test_results/APD_raw_lab_notes.txt)
  - [first tests with both 200umAPD with APD PSU (pdf)](test_results/APD_first_lab_tests_2013dec19.pdf)

## APD Chassis / Enclosure

## APD Contributors

  - Dan Werthimer
  - Rick Raffanti
  - Calvin Cheng
  - Matt Dexter

## APD Schedule

Here's a somewhat randomly selected dates of possible significance.

  - 2013feb26 Initial schematic design review. Thanks Rick.
  - 2013marxx Start of search for student to finish the schematics and
    place and route the circuit board
  - 2013aug30 Schematic capture and PCB place and route assigned to RAL
    staff.
  - 2013sep10 final review of schematics and PCB
  - 2013sep12 PCB fabrication order placed. 50 boards in total.
  - 2013sep13 Bill of materials (BOM) complete; components ordering
  - 2013oct08 S/N 001 assembly (stuff and solder) started (not all parts
    have arrived yet)
  - 2013oct18 S/N 001 lab testing - all the basic operations except max
    output current.
  - 2013oct25 S/N 001 lab testing - verify max output current isn't too
    large.
  - 2013novxx S/N 002 and S/N 003 are fully assembled and ready to be
    tested. S/N 004..050 are still blank PCBs
  - 2013dec04 find analog over temperature switch as a fail safe for
    load.
  - 2013dec05 S/N 001 sent to U Toronto for first use with an APD
  - 2013dec19 The combination of APD S/N 001 and the 200uAPD has been
    successfully tested in the lab.
  - 2013dec21 Wiki page created
  - 2014jan23 S/N 002 and S/N 003: have passed low level initial testing
    and are ready for the next level of testing/use.
