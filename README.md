# Fenstersteuerung

Modulare Fenster-/Rollladensteuerung auf Basis eines ESP32-S3 mit abgesetzter Sensorik.

## Projektziel

Die Hauptelektronik sitzt im Rollladenkasten. Die Steuerung übernimmt:

- Rollladensteuerung über 2 Finder-Relais
- kabelgebundene Fensterkontakte für Offen/Gekippt
- RS485-Anbindung einer separaten Sensorplatine
- zwei ULN2003-Ausgangsgruppen für TOP/BOTTOM
- 28BYJ-48 / Hilfsmechanik für die geplante Verstellung
- spätere Einbindung in Home Assistant

Es ist kein Fensterantrieb vorgesehen.

## Hauptplatine

- ESP32-S3
- 2-Lagen-KiCad-PCB
- ca. 150 x 90 mm
- getrennte 230-V- und SELV-Bereiche
- 2 x Finder 40.52, 5-V-Spule, DPDT
- Hardware-Interlock für Rollladenrichtung
- 2 x ULN2003A für TOP/BOTTOM
- RS485
- Reed-Eingänge
- 230-V-Eingang
- Mean Well IRM-20-5, 5 V / 4 A
- separate 3,3-V-Regelung

## Sensorplatine

Geplant:

- ESP32-C3
- MAX3485 bzw. 3,3-V-kompatibler RS485-Transceiver
- LD2450
- SHT40
- optional SCD41
- optional MQ-2
- 4-adrige Verbindung zur Hauptplatine:
  - +5 V
  - GND
  - RS485 A
  - RS485 B

## Aktueller KiCad-Stand

Aktuelle Arbeitsversion:

**v3.3bw**

Datei:

`hardware/Hauptplatine/Hauptplatine_v3.3bw_logic_minimal_150x90.kicad_pcb`

### Bestätigt sauber per DRC

#### v3.3ak
- TOP_COIL_1–4 sauber
- BOT_COIL_1–4 sauber
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-Fehler

#### v3.3as
- STEP_TOP_1–4 sauber
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-Fehler

#### v3.3az
- STEP_BOT_1–4 sauber
- TOP/BOT-Coils weiterhin sauber
- STEP_TOP weiterhin sauber
- 0 echte Routingfehler
- 36 offene Verbindungen

#### v3.3bc
- RS485_RX sauber
- RS485_TX sauber
- RS485_DE sauber
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-Fehler
- 32 offene Verbindungen

### Aktueller Routingstand

#### v3.3bp – letzter vollständig bestätigter sauberer Stand

DRC v3.3bp:

- 51 DRC-Meldungen, ausschließlich Bibliotheks-/Silkscreen-Warnungen
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-Fehler
- 0 Hole-/Dangling-Fehler
- 28 offene Verbindungen
- 0 Footprint-Fehler

Zusätzlich zu den früheren Meilensteinen sind dort sauber abgeschlossen:

- Q1_BASE
- Q2_BASE
- lokale K1_COIL_LOW-Verbindung Q1 ↔ D1
- lokale K2_COIL_LOW-Verbindung Q2 ↔ D2

#### v3.3bt – aktueller Teststand

Datei:

`hardware/Hauptplatine/Hauptplatine_v3.3bt_logic_minimal_150x90.kicad_pcb`

Neu gegenüber v3.3bp:

- lokaler Teil von RS485_A zwischen U3.6 und R3.1
- Layerwechsel auf B.Cu zum Unterqueren der bestehenden GND-Leitung
- J4.3 / langer RS485_A-Hauptweg bleibt bewusst noch offen

Vorstufe v3.3bs hatte:

- 52 DRC-Meldungen
- 27 offene Verbindungen
- 0 Kurzschlüsse
- 0 Clearance-Fehler
- genau 1 Leiterbahnkreuzung: RS485_A gegen GND

Diese Kreuzung wurde in v3.3bt mit einem kurzen B.Cu-Abschnitt korrigiert.

**DRC v3.3bt bestätigt sauber:** 51 Warnmeldungen, 0 echte Routingfehler, 27 offene Verbindungen, 0 Footprint-Fehler.



### DRC v3.3bw bestätigt

- 51 DRC-Meldungen, ausschließlich Bibliotheks-/Silkscreen-Warnungen
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-Fehler
- 0 Hole-/Dangling-Fehler
- 26 offene Verbindungen
- 0 Footprint-Fehler

Damit ist RS485_A vollständig abgeschlossen:
- U3.6 ↔ R3.1
- R3.1 ↔ J4.3
