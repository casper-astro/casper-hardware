# CASPER Z-DOK Compatibility

To ensure interoperability between all CASPER boards using a Z-DOK+
connector, the following guidelines should be followed:

## host side connectors

Host-side/plug (processing board) connector should use part \# 6367550-5

  - <http://www.te.com/catalog/products/en?q=6367550-5>
  - The only alternative part number is the 1367550-5
      - It is a very similar part but it matches only 5 of 6 End of Life
        (ELV) requirements.
      - <http://www.te.com/catalog/products/en?q=1367550-5>

## Adapter side connectors

Adapter-side/receptacle (add-on board) connector should use part \#
6367555-3

  - <http://www.te.com/catalog/products/en?q=6367555-3>
  - Alternative parts for the adapter boards are 6367555-1 and 6367555-2
      - These have different pin lengths for the different utility
        contacts. As the adapter boards are not hot-swappable the
        different pin lengths is not an issue.
  - Other additional alternative parts are the 1367555-1, '-2 and -3
    parts.
      - These are very similar parts but they match only 5 of the 6 ELV
        requirements.

## pinouts

  - Odd-numbered (A1, A3, B1,...) Z-DOK+ pins should be wired LVDS+
  - Even-numbered (A2, A4, B2,...) Z-DOK+ pins should be wired LVDS-
  - some boards have, to ease routing, a consistent flip between the
    active high and active low pins. This inversion is compensated for
    in the FPGA gateware.
  - Z-DOK+ pins C19/C20, F19/F20 should be wired to FPGA clock inputs
  - Z-DOK+ utility pins W1 X1 Y1 Z1 should be wired to GND
  - Z-DOK+ utility pins W2 X2 Y2 Z2 should be wired to +5V
  - Z-DOK+ utility pins W3 X3 Y3 Z3 should be wired to +3.3V
  - Z-DOK+ utility pins W4 X4 Y4 Z4 should be wired to +2.5V
  - Z-DOK+ utility pins W5 X5 Y5 Z5 should be wired to +1.8V
  - Z-DOK+ utility pins W6 X6 Y6 Z6 should be wired to GND
  - Z-DOK+ connectors should be spaced 3525 mils apart

## Availability

Tyco, as of 2013feb05 and before, has made it very hard to impossible to
acquire these connectors in low volumes with short leadtimes. Digicom
has placed a large quantity order of both host and adapter versions. If
you need some, they may be able to provide them.
