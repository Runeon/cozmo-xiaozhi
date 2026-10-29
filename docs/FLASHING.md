# Flashing Guide

## Web Flasher (empfohlen)

1. Öffne die [Web Flasher Seite](https://<your-github-pages-url>/) in Chrome oder Edge.
2. Verbinde den ESP32-S3 per USB.
3. Klicke auf "Connect" und wähle den COM-Port.
4. Klicke auf "Install" und warte, bis der Flash-Vorgang abgeschlossen ist.
5. Das Gerät startet automatisch neu.

**Voraussetzungen:**
- Chrome oder Edge (WebSerial API)
- HTTPS (GitHub Pages) oder localhost
- ESP32-S3 mit 16 MB Flash

## Lokales Flashen (esptool)

```bash
cd cozmo-xiaozhi
python scripts/flash_local.py COM3
```

Oder manuell:
```bash
python -m esptool --chip esp32s3 --port COM3 --baud 2000000 \
  --before default-reset --after hard-reset write-flash -z \
  --flash-mode dio --flash-freq 40m --flash-size 16MB \
  0x0 bin/bootloader.bin \
  0x8000 bin/partition-table.bin \
  0xd000 bin/ota_data_initial.bin \
  0x10000 bin/srmodels.bin \
  0x100000 bin/xiaozhi.bin
```

## Flash-Layout

| Offset | Datei | Beschreibung |
|---|---|---|
| 0x0 | bootloader.bin | ESP32-S3 Bootloader |
| 0x8000 | partition-table.bin | Partitionstabelle |
| 0xd000 | ota_data_initial.bin | OTA-Daten (initial) |
| 0x10000 | srmodels.bin | Spracherkennungsmodelle |
| 0x100000 | xiaozhi.bin | Hauptfirmware |

## Erststart (Pairing)

1. Nach dem Flashen startet das Gerät im WiFi-Setup-Modus (Hotspot `Xiaozhi-XXXX`).
2. Verbinde dich mit dem Hotspot und öffne `192.168.4.1`.
3. Gib deine WiFi-Zugangsdaten ein.
4. Das Gerät verbindet sich mit dem Xiaozhi-Server und zeigt einen **Aktivierungscode**.
5. Gib den Code in der [Xiaozhi Console](https://xiaozhi.me) ein.
6. Das Gerät ist jetzt gebunden und bereit.