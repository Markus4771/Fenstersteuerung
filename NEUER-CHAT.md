# NEUER-CHAT – Fenstersteuerung

## Projektziel

Kompakte ESP32-basierte Fenster-/Rollladensteuerung mit Hauptplatine im Rollladenkasten und abgesetzter Sensorplatine.

## Hardware

- ESP32
- Finder 40.52 Relais
- Reedkontakte werden per Kabel aus der Hauptelektronik herausgeführt
- RS485 vorgesehen
- ULN2003-Ausgänge TOP/BOTTOM
- Sensorik räumlich von der Hauptelektronik getrennt
- Hauptplatine ca. 150 x 90 mm

## Aktueller Stand

Aktuelle Arbeitsversion: **Hauptplatine_v3.3f_logic_minimal_150x90.kicad_pcb**

Wichtig:
- v3.3f ist noch nicht vollständig geroutet.
- Sie ist keine Fertigungsfreigabe.
- Frühere automatische Routing-Versuche v3.3d/v3.3e erzeugten DRC-Konflikte und werden nicht als Basis verwendet.
- v3.3f wurde wieder aus dem saubereren v3.3c-Stand aufgebaut.
- RS485 wurde in v3.3f bewusst offen gelassen.
- TOP_COIL_1–4 wurden neu geroutet und müssen als nächster Schritt per DRC geprüft werden.

## Noch offene Netze / Aufgaben

- BOT_COIL_1–4
- STEP_TOP_1–4
- STEP_BOT_1–4
- ROLL_UP
- ROLL_DN
- K1_COIL_LOW
- K2_COIL_LOW
- RS485_A
- RS485_B
- REED_TILT bzw. Reed-Bereich final prüfen
- +5 V und GND final prüfen
- komplette Konnektivität unabhängig vom DRC kontrollieren

## Vorgehensweise

Nicht mehrere Netzgruppen gleichzeitig automatisch routen.

Empfohlene Reihenfolge:
1. TOP_COIL prüfen
2. BOT_COIL routen und DRC
3. STEP_TOP/BOT routen und DRC
4. Relaisansteuerung
5. RS485
6. Reed-Eingänge
7. Versorgung/GND
8. finaler DRC
9. 230-V-Sicherheitsprüfung
10. Gerber/Drill/BOM

## Fertigungsfreigabe

Erst wenn:
- alle verwendeten Pads physisch verbunden sind,
- keine Kurzschlüsse/Kreuzungen vorhanden sind,
- DRC sauber ist,
- Netzspannungsbereich separat sicherheitstechnisch geprüft ist,
- Gerberdaten visuell kontrolliert wurden.
