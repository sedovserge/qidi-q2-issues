# QIDI Q2 Known Issues & Fixes

Collection of discovered issues and fixes for the QIDI Q2 3D printer.

## Issues

- **[Box: Vendor not detected from RFID tag](box/)** — The printer ignores the vendor byte on RFID tags and always shows "QIDI". Includes a patched Klipper module to fix this.

## Reference

- **[RFID Tag Format & Programming Guide](rfid/)** — Complete reference for RFID tags used with QIDI Box: data format, material/color/vendor codes, authentication keys, firmware behavior, config file format.
