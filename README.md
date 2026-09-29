# Fenstersteuerung

Modulare ESP32-S3-basierte Fenster-/Rollladensteuerung mit 230-V-Rollladenansteuerung, Reedkontakten, RS485 und zwei ULN2003-Ausgangsgruppen.

## Aktueller Hauptplatinenstand

**v3.3ebs2**

Datei:

`hardware/Hauptplatine/Hauptplatine_v3.3ebs2_library_clean_usb_clearance_150x90.kicad_pcb`

### DRC-Status

Der aktuelle KiCad-DRC ist elektrisch sauber:

- 0 offene Verbindungen
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-Fehler
- 0 Dangling-Tracks
- 0 Silkscreen-Warnungen
- 0 Footprint-Fehler

Es verbleiben nur 30 Bibliothekswarnungen durch angepasste bzw. lokal fehlende Footprint-Bibliotheken.

## Hauptfunktionen

- ESP32-S3 DevKitC-1
- 2 × Finder 40.52, 5-V-Spule, DPDT
- Hardware-Interlock für Rollladen AUF/AB
- Mean Well IRM-20-5
- AMS1117-3.3 mit 22-µF-Ausgangskondensator
- MAX3485 RS485
- 2 × ULN2003A
- 2 × 28BYJ-48-Ausgangsgruppe
- Reedkontakte für Offen/Gekippt
- 230-V-Motoranschluss
- getrennte 230-V- und SELV-Bereiche

## Wichtige Dateien

- PCB: `hardware/Hauptplatine/Hauptplatine_v3.3ebs2_library_clean_usb_clearance_150x90.kicad_pcb`
- DRC-Regeln: `hardware/Hauptplatine/Hauptplatine.kicad_dru`
- 230-V-Prüfung: `hardware/Hauptplatine/230V-SICHERHEITSPRUEFUNG.md`
- BOM: `bom/Hauptplatine_v3.3ebs2_BOM.csv`
- Entwicklungsarchiv: `hardware/Hauptplatine/archive/`
- Historie: `docs/ENTWICKLUNGSHISTORIE.md`
- Übergabe für neuen Chat: `NEUER-CHAT.md`

## Noch vor der Prototypenbestellung

1. BOM gegen tatsächlich bestellte Bauteile prüfen.
2. Gerber- und Drill-Dateien aus KiCad erzeugen.
3. Gerber im Viewer kontrollieren.
4. 230-V-Sicherheitsprüfung final abschließen.
5. Erste Inbetriebnahme zunächst ohne Netzspannung durchführen.

## Sicherheit

Der saubere KiCad-DRC ist keine formale Freigabe für 230-V-Betrieb. Luft-/Kriechstrecken, PE-Führung, Absicherung, Bauteilzulassungen, Gehäuse und Endanwendung müssen separat geprüft werden.


## Update v3.3ebs2

- elektrisch sauberer v3.3ebq2-Stand übernommen
- RS485-Klemme bleibt unten, USB-C frei zugänglich
- alle 17 verwendeten Footprints auf projektlokale Bibliothek `Fenstersteuerung.pretty` umgestellt
- `fp-lib-table` liegt im Hauptplatinen-Ordner
- gegenüber v3.3ebq2 keine Änderungen an Routing, Pads, Vias oder Kupfer
- nächster Schritt: KiCad öffnen, Zonen neu füllen und DRC ausführen
