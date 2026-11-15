# Fork bichlesi/OpenMQTTGateway

Fork für das Gateway „OMG_heltec_rtl_433_ESP“ (Heltec WiFi LoRa 32 V2,
rtl_433, 868,3 MHz, Bresser-Wetterstation). Basis: Tag `v1.7.0`
(Branch `basis-v1.7.0`), Änderungen per Pull Request gegen diesen Branch.

## Änderungen

* **Stärksten Zugangspunkt wählen** (`main/main.ino`,
  `wifi_prefer_strongest_ap()`): Das ESP32 sucht standardmäßig die Kanäle
  aufwärts ab und nimmt den ersten Zugangspunkt mit passender SSID. Im
  Mesh (Fritzbox + Repeater) blieb das Gateway am 15.11.2026 mit −90 dB am
  KinoRepeater (Kanal 6) hängen statt am Zugangspunkt mit −37 dB auf Kanal
  11; Folge: ständige Abbrüche und Neustarts. Jetzt: Suche über alle
  Kanäle, Verbindung zum stärksten Signal; gilt für `WiFi.begin()` mit
  und ohne Zugangsdaten und für das automatische Wiederverbinden.
* **Version** `v1.7.0-bichlesi.N` (Build-Flag `OMG_VERSION` in
  `env:heltec-rtl_433-fsk`; ohne setzt ein lokaler Build „version_tag“).
  Achtung: Das Update-Angebot in Home Assistant („Firmware update“) holt
  die offizielle Version von 1technophile und würde diese Änderungen
  überschreiben. Nur aus diesem Fork aktualisieren.
* **868,3 MHz** als Standard in `env:heltec-rtl_433-fsk` (das Gateway läuft
  so; gespeicherte RF-Einstellungen haben ohnehin Vorrang).

## Bauen und aufspielen

```sh
pip install platformio
pio run -e heltec-rtl_433-fsk
# OTA (Gateway-Passwort = "gateway password" aus dem Einrichtungsportal):
python ~/.platformio/packages/framework-arduinoespressif32/tools/espota.py \
  -i 192.168.2.97 -p 8266 -a <passwort> -f .pio/build/heltec-rtl_433-fsk/firmware.bin
```

Alternativ über die Web-Oberfläche des Gateways (Firmware-Upload).
Einstellungen (WLAN, MQTT, RF) bleiben beim Update erhalten.
