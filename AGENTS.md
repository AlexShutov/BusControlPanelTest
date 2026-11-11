# EspScreen

## Железо
- Плата: ESP32-P4 (с ESP32-C6 как WiFi/BLE co-процессором по SDIO)
- Чип: ESP32-P4 rev v3.2, dual core + LP core, 400 MHz, кварц 40 MHz
- MAC: e8:f6:0a:e8:b1:f8
- USB: встроенный USB-Serial/JTAG, порт macOS `/dev/cu.usbmodem101`
- У P4 нет своего радио — WiFi/BT через C6 (ESP-Hosted / esp_wifi_remote)

## Инструменты
- ESP-IDF / idf.py на системе не установлены
- esptool доступен через `uvx esptool ...` (v5: `uvx esptool --port /dev/cu.usbmodem101 chip-id`)
- Python 3.15 (Homebrew), есть `uv`
- gh CLI установлен (`/opt/homebrew/bin/gh`), авторизован как AlexShutov (HTTPS)

## Git
- Репозиторий: https://github.com/AlexShutov/BusControlPanelTest (public), ветка main
- Git SSH к github.com не работает (порт 22 блокирован) — только HTTPS через gh

## Статус проекта
- Кода ещё нет; есть README.md (стиль как в DashboardWidgets: What's here / Quickstart / Status), AGENTS.md, .gitignore
- Планируемая структура: src/, tools/, tests/, docs/
- Начальный коммит cdf5ec6 запушен в origin/main
