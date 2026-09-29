# Hauptplatine – KiCad-Projektordner

## Wichtig beim Öffnen

Nicht nur die einzelne `.kicad_pcb`-Datei herunterladen.

Für die projektlokale Footprint-Bibliothek müssen diese Dateien/Ordner zusammen bleiben:

- `Hauptplatine_v3.3ebs2_library_clean_fixed_150x90.kicad_pcb`
- `fp-lib-table`
- `Fenstersteuerung.pretty/`
- `Hauptplatine.kicad_dru`

Am einfachsten:

1. komplettes Repository herunterladen/klonen
2. in `hardware/Hauptplatine/` wechseln
3. `Hauptplatine_v3.3ebs2_library_clean_fixed_150x90.kicad_pcb` aus diesem Ordner öffnen

Die `fp-lib-table` verweist auf:

`${KIPRJMOD}/Fenstersteuerung.pretty`

Wenn nur die PCB-Datei separat heruntergeladen wird, kann KiCad die lokale Bibliothek nicht finden und meldet für alle Footprints:

`Die aktuelle Konfiguration enthält die Footprint-Bibliothek "Fenstersteuerung" nicht`

## Aktueller DRC-Stand

DRCebs.rpt vom 2026-09-29:

- 0 unconnected pads
- 0 Footprint errors
- keine elektrischen DRC-Fehler
- 30 reine Library-Warnungen wegen nicht geladener Projektbibliothek

Nach Öffnen aus dem vollständigen Projektordner bitte Zonen neu füllen und DRC erneut ausführen.
