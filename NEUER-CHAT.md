# NEUER-CHAT – Fenstersteuerung

## Projekt

GitHub:
https://github.com/Markus4771/Fenstersteuerung

## Ziel

Kompakte ESP32-basierte Fenster-/Rollladensteuerung.

Die Hauptelektronik sitzt im Rollladenkasten. Die Sensorik wird abgesetzt. Das System soll bewusst mehr können als ein einfacher Rollladenaktor:

- Rollladen AUF/AB
- Fenster offen/gekippt erkennen
- Präsenz erkennen
- Raumklima erfassen
- Endlagen-/Hilfsmechanik ansteuern
- Sensorplatine per RS485 anbinden

Es ist **kein Fensterantrieb** vorgesehen.

## Festgelegte Hardware

### Hauptplatine

- ESP32-S3
- 2 x Finder 40.52
- 5-V-Spulen
- DPDT
- Hardware-Interlock
- 230-V-Rollladenmotor-Ausgänge
- 2 x ULN2003
- TOP/BOTTOM-Ausgangsgruppen
- 28BYJ-48 + ULN2003 für Endlagen-/Verstellmechanik
- Reed-Eingänge
- RS485
- ca. 150 x 90 mm
- 2-lagig

### Sensorplatine

- ESP32-C3
- RS485
- 4 Adern zur Hauptplatine:
  - 5 V
  - GND
  - A
  - B
- 3,3-V-kompatibler RS485-Transceiver, z. B. MAX3485
- LD2450 fest eingeplant
- SHT40
- SCD41 optional
- MQ-2 optional

### Reedkontakte

Zwei kabelgebundene Reedkontakte:

1. Fenster offen
2. Fenster gekippt

Die Reedkontakte werden per Kabel direkt von der Hauptelektronik herausgeführt.

## Stromversorgung

Festgelegt:

- 230 V AC Eingang
- Mean Well IRM-20-5
- 5 V / 4 A
- 5-V-Bus
- separate 3,3-V-Regelung
- klare Trennung zwischen Netzspannung und SELV

## Aktuelle PCB-Version

Aktuell:

**Hauptplatine_v3.3g_logic_minimal_150x90.kicad_pcb**

Repository-Pfad:

`hardware/Hauptplatine/Hauptplatine_v3.3g_logic_minimal_150x90.kicad_pcb`

Wichtig:

- v3.3g ist noch nicht vollständig geroutet
- keine Fertigungsfreigabe
- v3.3g basiert auf v3.3f; v3.3f basiert wiederum auf dem sauberen v3.3c-Stand
- v3.3d/v3.3e wegen Routing-Konflikten nicht weiterverwenden
- RS485_A/B weiterhin bewusst offen
- TOP_COIL_1–4 sind aus v3.3f übernommen
- BOT_COIL_1–4 wurden in v3.3g neu geroutet
- BOT_COIL_1 liegt bewusst auf B.Cu, BOT_COIL_2–4 auf F.Cu, um Kreuzungen zu vermeiden

## Noch offene Netze

- TOP_COIL_1–4: DRC prüfen
- BOT_COIL_1–4: neu in v3.3g, DRC prüfen
- STEP_TOP_1–4
- STEP_BOT_1–4
- ROLL_UP
- ROLL_DN
- K1_COIL_LOW
- K2_COIL_LOW
- RS485_A
- RS485_B
- REED_OPEN final prüfen
- REED_TILT final routen/prüfen
- +5 V final prüfen
- GND final prüfen

## Routing-Regel aus den bisherigen Versuchen

Nicht mehrere Netzgruppen gleichzeitig automatisch routen.

Empfohlene Reihenfolge:

1. TOP_COIL
2. BOT_COIL
3. STEP_TOP/BOT
4. Relaisansteuerung
5. RS485
6. Reed-Eingänge
7. Versorgung/GND
8. komplette physische Konnektivitätsprüfung
9. finaler DRC

## Wichtige Lektion aus DRC 3.3c

Ein DRC mit "0 unconnected pads" darf **nicht** automatisch als vollständiges Routing interpretiert werden.

Zusätzlich immer direkt in der PCB-Datei prüfen:

- ob jedes verwendete Netz Leiterbahnsegmente besitzt
- ob alle Pads tatsächlich elektrisch verbunden sind
- ob keine Ratsnest-Verbindungen offen sind

## Fertigungsfreigabe

Nur wenn:

- alle verwendeten Pads physisch verbunden sind
- keine Kurzschlüsse vorhanden sind
- keine Leiterbahnkreuzungen vorhanden sind
- keine offenen/dangling Tracks vorhanden sind
- DRC sauber ist
- RS485 korrekt geroutet ist
- Netzspannungsbereich separat sicherheitstechnisch geprüft ist
- Gerberdaten visuell geprüft sind

## 230-V-Bereich

Vor Produktion separat prüfen:

- Luftstrecken
- Kriechstrecken
- Absicherung
- Leiterbahnbreiten
- Abstand zu 5 V / 3,3 V / GND
- Relaiskontaktführung
- Motoranschlüsse
- Schutzmaßnahmen

Der 230-V-Bereich darf nicht allein aufgrund eines sauberen DRC freigegeben werden.
