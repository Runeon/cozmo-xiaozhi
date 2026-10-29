# Cozmo-Xiaozhi

Xiaozhi-Firmware für den **FreeNove FNK0104** (ESP32-S3, ST77922 QSPI 320×480) mit Cozmo-Gesicht.

Basierend auf [78/xiaozhi-esp32](https://github.com/78/xiaozhi-esp32) und [RealDeco/xiaozhi-esphome](https://github.com/RealDeco/xiaozhi-esphome).

## Features

- 🎤 Xiaozhi AI Sprachassistent (WebSocket zu Xiaozhi-Server)
- 🔐 Aktivierungscode (Pairing) beim Erststart
- 🤖 Cozmo-Gesicht (Augen + Mund, animiert durch Interaktionen)
- 🕐 Uhr + Datum im oberen Bildschirmbereich
- 💬 Textfenster/Antworten unterhalb des Gesichtes
- 📱 Web Flasher (Chrome/Edge via WebSerial)
- 🔌 MCP-Integration (Home Assistant via xiaozhi-mcp-ha)

## Hardware

| Komponente | Details |
|---|---|
| Board | FreeNove FNK0104 (ESP32-S3, 16 MB Flash, 8 MB PSRAM) |
| Display | ST77922, QSPI, 320×480, Touch (GT911) |
| Audio | ES8311 Codec, I2S |
| Backlight | GPIO41 (PWM) |

## Flashen

### Web Flasher
Öffne die [Web Flasher Seite](https://<github-pages-url>/) in Chrome/Edge.

### Lokal (esptool)
```bash
python scripts/flash_local.py COM3
```

Siehe [FLASHING.md](docs/FLASHING.md) für Details.

## Custom Build

Siehe [BUILD.md](docs/BUILD.md) für die ESP-IDF Build-Anleitung.

## Pairing

1. Nach dem Flashen: WiFi-Setup (Hotspot `Xiaozhi-XXXX`).
2. Gerät zeigt Aktivierungscode.
3. Code in [Xiaozhi Console](https://xiaozhi.me) eingeben.
4. Gerät gebunden → bereit.

## Lizenz

MIT (basierend auf xiaozhi-esp32, MIT License)