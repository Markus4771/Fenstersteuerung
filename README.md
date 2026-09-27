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

**v3.3bd**

Datei:

`hardware/Hauptplatine/Hauptplatine_v3.3bd_logic_minimal_150x90.kicad_pcb`

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

### v3.3bd

Neu gegenüber v3.3bc:

- REED_OPEN neu geroutet
- REED_TILT neu geroutet
- J2/J3, R1/R2 und C1/C2 eingebunden
- bestehendes Coil-, STEP- und RS485_RX/TX/DE-Routing unverändert

**DRC für v3.3bd steht noch aus.**

## Bereits abgeschlossen

- TOP_COIL_1–4
- BOT_COIL_1–4
- STEP_TOP_1–4
- STEP_BOT_1–4
- RS485_RX
- RS485_TX
- RS485_DE

## Noch offen

- DRC von v3.3bd
- RS485_A
- RS485_B
- ROLL_UP
- ROLL_DN
- Q1_BASE
- Q2_BASE
- K1_COIL_LOW
- K2_COIL_LOW
- +3V3
- +5V
- GND
- verbleibende lokale Verbindungen an Widerständen, Dioden und Transistoren
- abschließende vollständige Konnektivitätsprüfung
- finaler DRC
- separate 230-V-Sicherheitsprüfung
- Gerber/Drill/BOM

## Routingstrategie

Die Entwicklung erfolgt bewusst blockweise mit DRC-Checkpoint nach jedem Funktionsblock.

Reihenfolge:

1. Coils
2. STEP_TOP
3. STEP_BOT
4. RS485 RX/TX/DE
5. Reed-Eingänge
6. RS485 A/B
7. Relais-/Transistorsteuerung
8. +3V3 / +5V / GND
9. Gesamt-DRC
10. 230-V-Sicherheitsprüfung
11. Fertigungsdaten

## PCB-Zonen

- links: 230-V-Bereich
- Mitte: ESP32-S3 und Finder-Relais
- rechts: 3,3/5-V-Logik, RS485 und ULN2003
- ganz rechts: TOP/BOTTOM-Anschlüsse

Zwischen Netzspannung und SELV ist ein eigener Trenn-/Keepout-Bereich vorgesehen.

## Sicherheitsanforderungen

Vor Fertigung und insbesondere vor Betrieb an 230 V müssen separat geprüft werden:

- Luftstrecken
- Kriechstrecken
- Absicherung
- Leiterbahnbreiten
- Trennung zu SELV
- Relaiskontakt- und Motorpfade
- Schutzmaßnahmen
- netzspannungsgeeignete Bauteile
- mechanische Abstände
- Gerberdaten

Ein sauberer KiCad-DRC ist **keine Freigabe für Netzspannung**.

## Verzeichnisstruktur

- `hardware/Hauptplatine/` – Hauptplatine
- `hardware/Sensorplatine/` – Sensorplatine
- `DRC/` – DRC-Berichte
- `gerber/` – spätere Fertigungsdaten
- `bom/` – spätere Stücklisten
- `NEUER-CHAT.md` – Übergabestand
- `README.md` – technische Übersicht
