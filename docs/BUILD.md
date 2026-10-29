# Custom Build Guide (ESP-IDF)

## Voraussetzungen

- ESP-IDF v6.1 (release/v6.1 branch)
- Python 3.8+
- Git

## ESP-IDF installieren

```bash
# Windows
# Download from: https://dl.espressif.com/dl/esp-idf/
# Oder via git:
git clone -b release/v6.1 --recursive https://github.com/espressif/esp-idf.git C:\esp-idf
cd C:\esp-idf
install.bat esp32s3
```

## xiaozhi-esp32 klonen

```bash
git clone https://github.com/78/xiaozhi-esp32.git
cd xiaozhi-esp32
```

## Custom Board hinzufügen

1. Erstelle `main/boards/cozmo-freenove/` mit:
   - `config.h` (Pins aus ESPHome-YAML)
   - `config.json`
   - `cozmo-freenove.cc` (Board-Klasse)

2. Registriere in `CMakeLists.txt` und `Kconfig.projbuild`.

3. Pin-Mapping (aus `bug-cozmo_fn.yaml`):
   - QSPI: SCK=12, D0=11, D1=13, D2=14, D3=9, CS=10
   - Backlight: 41
   - I2C: SDA=38, SCL=39
   - I2S: MCLK=17, BCLK=18, LRCLK=21, DOUT=15, DIN=16
   - ES8311 PA: 1
   - Touch (GT911): I2C 0x5D/0x14, INT=47
   - Boot Button: 0

## Build

```bash
# ESP-IDF Environment laden
. C:\esp-idf\export.bat

cd xiaozhi-esp32
idf.py set-target esp32s3
idf.py menuconfig
# → Board Type: cozmo-freenove
# → Flash Size: 16MB
# → Partition Table: custom (mit assets-Partition)
idf.py build
```

## Binaries extrahieren

```bash
# Nach erfolgreichem Build:
cp build/bootloader/bootloader.bin ../cozmo-xiaozhi/bin/
cp build/partition_table/partition-table.bin ../cozmo-xiaozhi/bin/
cp build/ota_data_initial.bin ../cozmo-xiaozhi/bin/
cp build/srmodels/srmodels.bin ../cozmo-xiaozhi/bin/
cp build/xiaozhi.bin ../cozmo-xiaozhi/bin/
```

## ST77922-QSPI Treiber

Xiaozhi hat keinen nativen ST77922-QSPI-Treiber. Optionen:

1. **SPI-Fallback**: ILI9341-Treiber als Vorlage, Init-Sequenz aus ESPHome-YAML einfügen.
   - Langsamer (20 MHz vs. 80 MHz), aber funktional.

2. **Custom QSPI-Driver**: ESP-LCD-QSPI-Panel für ST77922 schreiben.
   - Schneller, aber aufwändiger.

Die Init-Sequenz steht in `bug-cozmo_fn.yaml` (display → init_sequence).

## Cozmo-Gesicht als Emotion-Display

1. Erstelle GIF-Assets für jede Emotion (20 Emotionen).
2. Tools: `78/xiaozhi-assets-generator`
3. Partition: `assets` (mmap)
4. Emotion-Mapping: siehe Plan-Dokument.

## Nächste Schritte

1. ESP-IDF installieren
2. Custom Board implementieren
3. Build + Test
4. Binaries ins Repo kopieren
5. Web Flasher aktualisieren (GitHub Pages redeploy)