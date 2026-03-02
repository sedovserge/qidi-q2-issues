# QIDI Box RFID Tag Format & Programming Guide

Complete reference for RFID tags used with QIDI Box (Q2 and other QIDI printers).

## Tag Type

**MIFARE Classic 1K** — standard 13.56 MHz RFID tag with 16 sectors, 4 blocks each (16 bytes per block).

## Data Location

- **Sector 1, Block 0** (absolute block 4)
- Authentication: Key A

### Authentication Keys

| Key | Usage |
|-----|-------|
| `FFFFFFFFFFFF` | Default key, works with blank/generic tags |
| `D3F7D3F7D3F7` | QIDI factory key, used on original QIDI-branded tags |

When writing/reading, try `D3F7D3F7D3F7` first, then fall back to `FFFFFFFFFFFF`.

## Block 4 Data Format (16 bytes)

| Byte | Content | Range | Notes |
|------|---------|-------|-------|
| 0 | Material code | 1–56+ | See [Material codes](#material-codes) |
| 1 | Color code | 1–24 | See [Color codes](#color-codes) |
| 2 | Vendor code | 0–7+ | See [Vendor codes](#vendor-codes) |
| 3–11 | Unused | — | Zeros |
| 12–15 | Optional | — | Some apps write "QIDI" signature here for validation |

All values are stored as raw byte values (unsigned integers).

## Material Codes

From [`officiall_filas_list.cfg`](https://github.com/QIDITECH/QIDI_Q2/blob/main/config/officiall_filas_list.cfg):

| Code | Material | Min Temp | Max Temp | Box Max Temp | Type |
|------|----------|----------|----------|-------------|------|
| 1 | PLA Rapido | 190 | 240 | 0 | PLA |
| 2 | PLA Matte | 190 | 240 | 0 | PLA |
| 3 | PLA Metal | 190 | 240 | 0 | PLA |
| 4 | PLA Silk | 190 | 240 | 0 | PLA |
| 5 | PLA-CF | 210 | 250 | 0 | PLA-CF |
| 6 | PLA-Wood | 190 | 240 | 45 | PLA |
| 7 | PLA Basic | 190 | 240 | 0 | PLA |
| 8 | PLA Matte Basic | 190 | 230 | 0 | PLA |
| 9 | *(empty)* | — | — | — | — |
| 10 | Support For PLA | 210 | 240 | 0 | PLA-S |
| 11 | ABS Rapido | 240 | 280 | 45 | ABS |
| 12 | ABS-GF | 240 | 280 | 45 | ABS-GF |
| 13 | ABS-Metal | 240 | 280 | 45 | ABS |
| 14 | ABS-Odorless | 240 | 280 | 45 | ABS |
| 15–17 | *(empty)* | — | — | — | — |
| 18 | ASA | 240 | 280 | 45 | ASA |
| 19 | ASA-Aero | 240 | 280 | 45 | ASA-AERO |
| 20 | ASA-CF | 240 | 280 | 45 | ASA-CF |
| 21–22 | *(empty)* | — | — | — | — |
| 23 | PC | 240 | 280 | 0 | PC |
| 24 | UltraPA | 260 | 300 | 55 | UltraPA |
| 25 | PA-CF | 260 | 300 | 65 | PA-CF |
| 26 | UltraPA-CF25 | 300 | 320 | 65 | UltraPA-CF25 |
| 27 | PA12-CF | 260 | 300 | 65 | PA12-CF |
| 28–29 | *(empty)* | — | — | — | — |
| 30 | PAHT-CF | 300 | 320 | 65 | PAHT-CF |
| 31 | PAHT-GF | 300 | 320 | 65 | PAHT-GF |
| 32 | Support For PAHT | 260 | 280 | 65 | PAHT-S |
| 33 | Support For PET/PA | 260 | 280 | 65 | PA-S |
| 34 | PC/ABS-FR | 260 | 280 | 50 | PC-ABS-FR |
| 35 | TPEE | 230 | 260 | 0 | TPEE |
| 36 | PEBA | 230 | 260 | 0 | PEBA |
| 37 | PET-CF | 280 | 320 | 65 | PET-CF |
| 38 | PET-GF | 280 | 320 | 50 | PET-GF |
| 39 | PETG Basic | 240 | 280 | 45 | PETG |
| 40 | PETG-Tough | 240 | 275 | 45 | PETG |
| 41 | PETG Rapido | 220 | 270 | 45 | PETG |
| 42 | PETG-CF | 240 | 270 | 45 | PETG-CF |
| 43 | PETG-GF | 240 | 270 | 45 | PETG-GF |
| 44 | PPS-CF | 300 | 350 | 65 | PPS-CF |
| 45 | PETG Translucent | 240 | 280 | 45 | PETG |
| 46 | PPS-GF | 300 | 350 | 65 | PPS-GF |
| 47 | PVA | 210 | 250 | 50 | PVA |
| 48 | TPU-AERO 64D | 200 | 250 | 0 | TPU-AERO 64D |
| 49 | TPU-Aero | 200 | 250 | 0 | TPU-AERO |
| 50 | TPU 95A-HF | 200 | 250 | 0 | TPU |
| 51 | PETG | 240 | 280 | 45 | PETG |
| 52 | PA6-CF | 260 | 350 | 55 | PA6-CF |
| 53 | ABS-GF6 | 240 | 280 | 45 | ABS-GF6 |
| 54 | PETG-CF17 | 270 | 320 | 0 | PETG-CF17 |
| 55 | ABS-GF4 | 240 | 280 | 55 | ABS-GF4 |
| 56 | PA6-CF20 | 280 | 300 | 55 | PA6-CF20 |

## Color Codes

| Code | Color | Hex |
|------|-------|-----|
| 1 | White | `#FAFAFA` |
| 2 | Black | `#060606` |
| 3 | Silver | `#D9E3ED` |
| 4 | Green (Bright) | `#5CF30F` |
| 5 | Mint | `#63E492` |
| 6 | Blue | `#2850FF` |
| 7 | Pink | `#FE98FE` |
| 8 | Yellow | `#DFD628` |
| 9 | Green (Dark) | `#228332` |
| 10 | Light Blue | `#99DEFF` |
| 11 | Navy | `#1714B0` |
| 12 | Lavender | `#CEC0FE` |
| 13 | Lime | `#CADE4B` |
| 14 | Royal Blue | `#1353AB` |
| 15 | Sky Blue | `#5EA9FD` |
| 16 | Purple | `#A878FF` |
| 17 | Coral | `#FE717A` |
| 18 | Red | `#FF362D` |
| 19 | Ivory | `#E2DFCD` |
| 20 | Grey | `#898F9B` |
| 21 | Brown | `#6E3812` |
| 22 | Khaki | `#CAC59F` |
| 23 | Orange | `#F28636` |
| 24 | Gold | `#B87F2B` |

## Vendor Codes

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

## How the Printer Reads Tags

The printer uses an **FM17550** RFID reader chip connected via SPI to the Klipper MCU. The Klipper module `box_rfid.so` (compiled, proprietary) in `/home/mks/klipper/klippy/extras/` handles communication.

Reading process:
1. Tag enters the field
2. FM17550 authenticates with Key A and reads block 4 (16 bytes)
3. Data is sent to Klipper via SPI response
4. The module reads the data twice and compares to ensure consistency
5. Validates: `filament` must be 1–99, `color` must be 1–24
6. Saves values to Klipper variables: `filament_<stepper>`, `color_<stepper>`, `vendor_<stepper>`

### Known firmware bug

The stock `box_rfid.so` **does not read byte 2** (vendor) — it is hardcoded to `1` (QIDI). See [`../box/`](../box/) for the fix.

## Config File Format

The source of truth for codes is `officiall_filas_list.cfg` from the [QIDI Q2 firmware repo](https://github.com/QIDITECH/QIDI_Q2/blob/main/config/officiall_filas_list.cfg).

The printer also supports importing this config to update the filament database. Format:

```ini
[fila{N}]                           # N = material code (byte 0)
filament                       = PLA Rapido
min_temp                       = 190
max_temp                       = 240
box_min_temp                   = 0
box_max_temp                   = 0
type                           = PLA

[colordict]
1                              = #FAFAFA    # code = hex color
2                              = #060606

[vendor_list]
0                              = Generic    # code = name
1                              = QIDI
```

## Existing Apps for Writing Tags

| App | Platform | Link |
|-----|----------|------|
| QIDI RFID Tag Writer | Android | [isnlab/qidi-rfid](https://github.com/isnlab/qidi-rfid) |
| qidi-box-rfid-manager | Android (React Native) | [n0cloud/qidi-box-rfid-manager](https://github.com/n0cloud/qidi-box-rfid-manager) |
| Qidi RFID App | Desktop (Rust/Tauri) | [LexyGuru/Qidi_RFID_App](https://github.com/LexyGuru/Qidi_RFID_App) |
| qidi-filament-nfc-flipper | Flipper Zero | [alexk42/qidi-filament-nfc-flipper](https://github.com/alexk42/qidi-filament-nfc-flipper) |

## Example Config

The file [`officiall_filas_list.cfg`](officiall_filas_list.cfg) is a copy of the firmware config from the [QIDI Q2 repo](https://github.com/QIDITECH/QIDI_Q2/blob/main/config/officiall_filas_list.cfg). It contains all material, color, and vendor definitions that the printer uses.

## References

- [QIDI Q2 Firmware (GitHub)](https://github.com/QIDITECH/QIDI_Q2)
- [QIDI Wiki — RFID](https://wiki.qidi3d.com/en/QIDIBOX/RFID)
- [SimplyPrint — QIDI Material Standard](https://help.simplyprint.io/en/article/the-qidi-material-standard-nfcrfid-for-the-qidi-box-ns0c8d/)
- [Community Python replacement for box_rfid.so](https://github.com/qidi-community/Plus4-Wiki/tree/main/content/customisable_qidibox_firmware)
