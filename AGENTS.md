# EspScreen

## Железо (подтверждено листингом AliExpress 1005009673625472)
- Плата: **Guition JC4880P443C_I_W_Y** (продаётся также как JC4880P433C), 4.3", корпус с камерой
- Ядро: модуль **JC-ESP32P4-M3** (SOC ESP32P4NRW32, 32MB PSRAM, 16MB flash, WiFi: ESP32-C6)
- Чип: ESP32-P4 rev v3.2 (новая ревизия — другие платы из issue у людей rev v1.3), dual core + LP core, 400 MHz, кварц 40 MHz
- MAC: e8:f6:0a:e8:b1:f8
- USB: встроенный USB-Serial/JTAG, порт macOS `/dev/cu.usbmodem101`; прошивать через **USB2** разъём, питание от 600 mA
- Экран: 480×800 (портрет), MIPI-DSI, драйвер **ST7701** (в ESPHome модель `JC4880P443`)
- Тач: **GT911** (I2C)
- Камера: 2MP **OV02C10** (MIPI CSI 2-lane)
- У P4 нет своего радио — WiFi 6 (2.4GHz) / BLE5 через C6 по SDIO (ESP-Hosted / esp_wifi_remote)
- Интерфейсы: микрофон, динамик MX1.25 2P, батарея MX1.25 2P, UART0 MX1.25 4P, UART MX1.25 4P, SH1.0 4P I2C, 2×13 pin header (GPIO28/32/34/35/49/50/51/52, GPIO29/30/31/33, C6_U0RXD/TXD, C6_IO9, C6_CHIP_PU, I2C_SDA/SCL)
- Размеры: 66.80×114.40×13.8 mm, активная область 56.16×93.60 mm
- Рабочие чужие конфиги: github.com/jtenniswood/espcontrol `devices/guition-esp32-p4-jc4880p443`, xiaozhi-esp32 `boards/guition-jc4880p443`

## Документация платы: где и как скачать
- Источник: GUITION-DISK (облако производителя) `https://pan.jczn1688.com/1#/`
  - Нужный файл: папка **HMI display** → **`JC4880P443C_I_W.zip`** (455 MB, public_id `JdMnFwhrmKpc4Axfil2L9Q`)
  - В браузере: `https://pan.jczn1688.com/directlink/1/HMI%20display/JC4880P443C_I_W.zip`
- Программно (файл разрезан на 10 шард на CDN):
  1. Манифест с URL шард и sha256: `curl https://pan.jczn1688.com/api/files/JdMnFwhrmKpc4Axfil2L9Q/manifest`
  2. Скачать все шарды `https://shard.jczn1688.com/<sha256>`, проверить sha256
  3. Склеить по seq: `cat shard_0 ... shard_9 > JC4880P443C_I_W.zip` (итог 477 630 572 байт)
- Локально распаковано в `docs/JC4880P443C_I_W/JC4880P443C_I_W/` (исключено из git через .gitignore — 1.4 GB):
  - `1-Demo/` — примеры: ESP-IDF 5.5.4 и 6.0.2 (P4 3.x, проект `video_lcd_display` + xiaozhi-esp32 с бордой `guition-jc4880p443`), arduino_examples (lvgl_demo_v8, mp3_player и др.), видео на TF-карту (<32GB, FAT32, папка MJPEG)
  - `2-Specification/` — спецификация EN
  - `3-Structure_Diagram/` — чертежи (pdf/dwg/jpg)
  - `4-Driver_IC_Data_Sheet/` — datasheet ESP32-P4 CN, OV02C10
  - `5-Schematic/` — полная схема (JC4880P443_V1.0.pdf + png по блокам: PWR, LCD&CSI, ESP32-P4, USB&IO, 485, CODEC&TFCARD, OTHER)
  - `6-User_Manual/` — Getting started
  - `7-Character&Picture_Molding_Tool/` — Windows-инструменты (CH340 драйвер, sscom32, WinHex, сетевой дебаг, JPGCompact)
  - `8-Burn operation/` — готовые bin (P4 V2.1/V2.2/V2.3 для P4_V3X, C6-slave v2.3.2, xiaozhi 2.0.4), flash_download_tool 3.9.9; прошивать через USB2, ток ≥600 mA

## Инструменты
- ESP-IDF v6.0.3 установлен в `~/esp/esp-idf` (клон v6.0.3, --recursive); активация: `. ~/esp/esp-idf/export.sh`
- Тулчейны в `~/.espressif` (macos-arm64): xtensa-esp-elf (S3), riscv32-esp-elf (P4/C6), gdb, openocd, esp-rom-elfs; цели esp32p4 и esp32s3 установлены через `./install.sh esp32p4,esp32s3`
- IDF 6.0.x нужна этой плате (чип rev v3.2 = «P4_3X», у старых плат rev 1.x хватило бы 5.5.4); v6.0.3 поддерживает и esp32p4, и esp32s3
- Prerequisites через brew: cmake, ninja, dfu-util, ccache (+ python3 3.15 Homebrew)
- esptool доступен через `uvx esptool ...` (v5: `uvx esptool --port /dev/cu.usbmodem101 chip-id`)
- Python 3.15 (Homebrew), есть `uv`
- gh CLI установлен (`/opt/homebrew/bin/gh`), авторизован как AlexShutov (HTTPS)
- PlatformIO установлен (~/.platformio, CLI ~/.platformio/penv/bin/pio) с espressif32 6.8.0 — но он НЕ поддерживает ESP32-P4 (нет Arduino core 3.x); для P4 использовать ESP-IDF или pioarduino

## Git
- Репозиторий: https://github.com/AlexShutov/BusControlPanelTest (public), ветка main
- Git SSH к github.com не работает (порт 22 блокирован) — только HTTPS через gh
- Vendor-доки из docs/ в git не попадают (правило `docs/JC4880P443C_I_W*` в .gitignore)

## Статус проекта
- Кода ещё нет; есть README.md (стиль как в DashboardWidgets: What's here / Quickstart / Status), AGENTS.md, .gitignore
- Планируемая структура: src/, tools/, tests/, docs/ (docs/ занята vendor-доками платы)
- Начальный коммит cdf5ec6 запушен в origin/main
