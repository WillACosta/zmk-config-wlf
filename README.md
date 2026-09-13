# The Wolf (wlfkbd) ZMK Firmware

Official ZMK firmware and keymap configuration for **The Wolf** (`wlfkbd`), a 40-key split keyboard.

- **Hardware Repository:** [WillACosta/wlfkbd](https://github.com/WillACosta/wlfkbd)
- **Keymap Editor:** Compatible with [Nick Coutsos' Keymap Editor](https://nickcoutsos.github.io/keymap-editor/)

## Current Keymap

![The Wolf Keymap](keymap-drawer/wlf.svg)

## Features

- **Shields:** `wlf_left`, `wlf_right` (Seeed Studio XIAO BLE nRF52840)
- **Matrix:** 40 keys total (20 per half), column-staggered with splayed thumb cluster
- **Per-key Backlight:** PWM driven via `AO3400A` MOSFET
- **Automated Keymap Drawer:** SVG and YAML keymaps auto-generated on push with `#4720ab` theme and Lucide Icons
- **Keymap Editor Ready:** Native `config/wlf.json` layout definition for visual editing

## Flashing Instructions

1. Put the keyboard half into bootloader mode by pressing the reset button twice quickly (or via the `&bootloader` key).
2. A mass storage device (`XIAO-BLE` or `NRF52BOOT`) will appear on your computer.
3. Download the firmware artifact (`wlf_left.uf2` or `wlf_right.uf2`) from GitHub Actions Releases/Artifacts.
4. Drag and drop the corresponding `.uf2` file onto the drive.
5. Repeat for the other half.
