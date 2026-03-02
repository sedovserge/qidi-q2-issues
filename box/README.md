# QIDI Q2 Box: Vendor not detected from RFID tag

## Problem

The QIDI Q2 printer (and likely other QIDI printers using Box) correctly reads **material** and **color** from RFID tags, but the **vendor is always shown as "QIDI"** (code 1), regardless of what is actually written on the tag.

## Root cause

The proprietary Klipper module `box_rfid.so` located at `/home/mks/klipper/klippy/extras/` does not read byte 2 (vendor) from the RFID tag data. Instead, the vendor value is **hardcoded to 1** (QIDI).

The RFID tag stores data in MIFARE Classic 1K, Sector 1 Block 0 (absolute block 4):

| Byte | Content |
|------|---------|
| 0 | Material code |
| 1 | Color code |
| 2 | Vendor code |
| 3–11 | Unused |
| 12–15 | "QIDI" signature |

The firmware reads `data[0]` (material) and `data[1]` (color) correctly, but never accesses `data[2]`. Instead it does:

```python
self.stepper.box_extras.save_variable("vendor_" + self.stepper.stepper_name, 1)  # hardcoded
```

## Fix

Replace the proprietary `box_rfid.so` with the patched Python module [`box_rfid.py`](box_rfid.py) that reads the vendor byte from the tag.

The key change:

```diff
  filament = data[0]
  color = data[1]
+ vendor = data[2] if len(data) > 2 else 1
  ...
- self.stepper.box_extras.save_variable("vendor_...", 1)
+ self.stepper.box_extras.save_variable("vendor_...", vendor)
```

## Installation

### 1. SSH into the printer

```bash
ssh mks@<PRINTER_IP>
# password: makerbase
```

### 2. Backup the original module

```bash
cp /home/mks/klipper/klippy/extras/box_rfid.so ~/box_rfid.so.bak
```

### 3. Copy the patched file

Copy `box_rfid.py` from this repository to the printer:

```bash
scp box_rfid.py mks@<PRINTER_IP>:/home/mks/klipper/klippy/extras/
```

Or download directly on the printer:

```bash
cd /home/mks/klipper/klippy/extras/
wget <RAW_URL_TO_THIS_FILE>/box_rfid.py
```

### 4. Remove the original `.so`

```bash
sudo rm /home/mks/klipper/klippy/extras/box_rfid.so
```

### 5. Power cycle the printer

A full power cycle (not just a Klipper restart) is required.

## Rollback

If something goes wrong:

```bash
cp ~/box_rfid.so.bak /home/mks/klipper/klippy/extras/box_rfid.so
rm /home/mks/klipper/klippy/extras/box_rfid.py
# power cycle the printer
```

## Notes

- Tested on **QIDI Q2**.
- The Python module is based on the community reverse-engineering from [qidi-community/Plus4-Wiki](https://github.com/qidi-community/Plus4-Wiki/tree/main/content/customisable_qidibox_firmware).
- After a QIDI OTA firmware update, `box_rfid.so` will be restored — the fix will need to be reapplied.

## Vendor codes

From [`officiall_filas_list.cfg`](https://github.com/QIDITECH/QIDI_Q2/blob/main/config/officiall_filas_list.cfg):

| Code | Vendor |
|------|--------|
| 0 | Generic |
| 1 | QIDI |
| 2 | NIT |
| 3 | Filamentarno |
| 4 | Polymaker |
| 5 | PIC |
| 6 | Fiberon |
| 7 | FDPlast |
