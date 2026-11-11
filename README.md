# EspScreen

ESP32-P4 dashboard/screen firmware, built around a P4 + C6 sibling pair:

- **ESP32-P4** — dual-core + LP core, 400 MHz: drives the display and the app UI.
- **ESP32-C6** — WiFi/BLE co-processor over SDIO (the P4 has no radio of its own), via ESP-Hosted / `esp_wifi_remote`.

## Hardware

| Item | Value |
| --- | --- |
| Board | ESP32-P4 + ESP32-C6 (co-processor) |
| Chip | ESP32-P4 rev v3.2, dual core + LP core, 400 MHz |
| Crystal | 40 MHz |
| MAC | `e8:f6:0a:e8:b1:f8` |
| USB | Built-in USB-Serial/JTAG, macOS port `/dev/cu.usbmodem101` |

## What's here

Project scaffolding is not created yet; the folder currently holds only project memory:

- **`AGENTS.md`** — agent memory: hardware facts, tooling notes, project status.
- **`README.md`** — this file.

Planned layout (mirrors the repo style used in DashboardWidgets):

- **`src/`** — application code (UI, display driver, connectivity).
- **`tools/`** — flash/monitor helpers and host-side preview scripts.
- **`tests/`** — host-side unit tests for pure logic.
- **`docs/`** — rendered previews, screenshots, schematics.

## Quickstart

Tooling on this machine: ESP-IDF / `idf.py` are not installed; `esptool` runs ad hoc via `uv`.

```sh
uvx esptool --port /dev/cu.usbmodem101 chip-id   # identify the chip
uvx esptool --port /dev/cu.usbmodem101 flash-id  # flash layout
```

ESP-IDF setup (when the project code lands):

```sh
brew install esp-idf              # or the official esp-idf installer
idf.py set-target esp32p4
idf.py build flash monitor
```

## Status

Folder initialized; firmware not yet started.

## License

TBD.
