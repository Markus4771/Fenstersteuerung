# NEUER-CHAT – Fenstersteuerung

## Repository

`Markus4771/Fenstersteuerung`

## Aktueller Stand

Aktuelle Hauptplatine:

`hardware/Hauptplatine/Hauptplatine_v3.3ebs2_library_clean_usb_clearance_150x90.kicad_pcb`

Status:

- Routing abgeschlossen
- 0 offene Verbindungen
- 0 Kurzschlüsse
- 0 Track-Kreuzungen
- 0 Clearance-Fehler
- 0 Dangling-Tracks
- 0 Silkscreen-Warnungen
- nur noch 30 nicht-elektrische Library-Warnungen

## Wichtige Korrekturen des finalen Standes

- Mean Well IRM-20-5 Footprint und Pinbelegung korrigiert
- Finder 40.52 auf offizielles Pinraster korrigiert
- Littelfuse-646-Sicherungshalter auf 22,7-mm-Raster korrigiert
- AMS1117 mit 22-µF-Ausgangskondensator ergänzt
- ESP32-S3 DevKitC-1 J1-Pinbelegung korrigiert
- 230-V-Abstände verbessert
- GND-Zoneninseln und Silkscreen-Warnungen bereinigt

## Projektstruktur

- `hardware/Hauptplatine/` – aktiver Hauptplatinenstand
- `hardware/Hauptplatine/archive/` – alte PCB-Zwischenstände
- `bom/` – Stückliste
- `gerber/` – spätere Fertigungsdaten
- `docs/ENTWICKLUNGSHISTORIE.md` – ausführliche Entwicklungshistorie
- `DRC/` – DRC-Dokumentation

## Nächste Schritte

1. BOM gegen reale Bauteile prüfen.
2. Gerber und Drill in KiCad erzeugen.
3. Gerber visuell prüfen.
4. 230-V-Sicherheitsprüfung finalisieren.
5. Prototyp bestellen.
6. Erstinbetriebnahme ohne Netzspannung.
7. Danach 5 V / 3,3 V / ESP32 / RS485 / Relais / Stepper prüfen.
8. Netzspannung erst nach separater Sicherheitsprüfung.

## Hinweis

Nicht mit Dateien aus `hardware/Hauptplatine/archive/` weiterarbeiten. Aktive Basis ist ausschließlich **v3.3ebs2**.


## Update v3.3ebs2

- elektrisch sauberer v3.3ebq2-Stand übernommen
- RS485-Klemme bleibt unten, USB-C frei zugänglich
- alle 17 verwendeten Footprints auf projektlokale Bibliothek `Fenstersteuerung.pretty` umgestellt
- `fp-lib-table` liegt im Hauptplatinen-Ordner
- gegenüber v3.3ebq2 keine Änderungen an Routing, Pads, Vias oder Kupfer
- nächster Schritt: KiCad öffnen, Zonen neu füllen und DRC ausführen
