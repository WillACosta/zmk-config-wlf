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
- **Status LEDs:** 4× SK6812 Mini-E addressable RGB LEDs per half, power-gated via `AO3407A` P-channel MOSFET (`STS_LED_EN` / `P0.10` active-low) and driven via SPIM3 MOSI (`STS_LED_DATA` / `P0.09`)
- **Automated Keymap Drawer:** SVG and YAML keymaps auto-generated on push with `#4720ab` theme and Lucide Icons
- **Keymap Editor Ready:** Native `config/wlf.json` layout definition for visual editing

## Included in this first firmware revision

- Wireless BLE split: the left half is the central and the right half is the peripheral.
- Battery reporting for both LiPo-powered halves through ZMK's XIAO BLE board battery-voltage-divider support.
- The 40-key, 6-column × 4-row electrical matrix, including the missing outer-pinky bottom key and three thumb keys per half.
- A four-layer QWERTY keymap: Base, Navigation, Symbols, Numbers, plus a Settings layer reached from the Nav/Symbol thumb key.
- Bluetooth profile selection/clear, a bootloader key on each outside key of Settings, NKRO, sleep, and short matrix debounce.
- Single-zone white backlight support: D10/P1.14 PWM drives the AO3400A low-side MOSFET; use Settings `BL_TOG`, `BL_INC`, and `BL_DEC`.
- Status RGB LED support: 4 addressable SK6812 Mini-E LEDs per half on `P0.09` (NFC1 / SPIM3 MOSI), power-gated by `P0.10` (NFC2) via an AO3407A P-channel MOSFET (`EXT_POWER`, active-low) to prevent parasitic battery drain. Controlled via Settings layer `RGB_TOG`, `RGB_BRI`, `RGB_BRD`, `RGB_EFF`, and `EP_TOG`.

## Hardware mapping

The schematics define the same matrix mapping on both halves:

| Signal | XIAO pin |
| --- | --- |
| ROW0–ROW3 | D0–D3 |
| COL0–COL5 | D4–D9 |
| White backlight PWM | D10 / P1.14 |
| Status LED data (`STS_LED_DATA`) | NFC1 / P0.09 (SPIM3 MOSI) |
| Status LED power enable (`STS_LED_EN`) | NFC2 / P0.10 (AO3407A Gate, Active-Low) |

The diode orientation is configured as `col2row`. The right-hand transform has a six-column offset and reverses its columns so every layer follows the physical order from the left outer edge to the right outer edge.

## Build

The recommended build route is GitHub Actions. Push this repository to GitHub; the repository-level **Build The Wolf firmware** workflow builds the three entries in [`build.yaml`](build.yaml): `wlf_left`, `wlf_right`, and `wlf_settings_reset`. Download the `wlf-firmware` workflow artifact and unzip it after the build completes.

For a local build, run the following from the repository root to initialize the configuration as a ZMK workspace, then build the two shields with the `xiao_ble` board. The required ZMK manifest is [`config/west.yml`](config/west.yml); its ZMK revision is `main`.

```sh
west init -l firmware/config
west update
west zephyr-export
west build -s zmk/app -d build/wlf-left -b xiao_ble -- -DSHIELD=wlf_left -DZMK_CONFIG="$PWD/firmware/config" -DZMK_EXTRA_MODULES="$PWD"
west build -s zmk/app -d build/wlf-right -b xiao_ble -- -DSHIELD=wlf_right -DZMK_CONFIG="$PWD/firmware/config" -DZMK_EXTRA_MODULES="$PWD"
```

## Flashing Instructions

1. Put the keyboard half into bootloader mode by pressing the reset button twice quickly (or via the `&bootloader` key).
2. A mass storage device (`XIAO-BLE` or `NRF52BOOT`) will appear on your computer.
3. Download the firmware artifact (`wlf_left.uf2` or `wlf_right.uf2`) from GitHub Actions Releases/Artifacts.
4. Drag and drop the corresponding `.uf2` file onto the drive.
5. Repeat for the other half.
