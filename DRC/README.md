# DRC-Berichte

Hier werden die KiCad-Design-Rule-Check-Berichte der jeweiligen Hauptplatinen- und Sensorplatinen-Versionen abgelegt.

Ein sauberer DRC ersetzt nicht die zusätzliche Prüfung der tatsächlichen Netzkonnektivität und der 230-V-Sicherheitsabstände.

## Hauptplatine v3.3ebs2

Aktive PCB-Datei:

`hardware/Hauptplatine/Hauptplatine_v3.3ebs2_library_clean_fixed_150x90.kicad_pcb`

Ergebnis der zuletzt vom Benutzer in KiCad ausgeführten DRC-Läufe:

- 0 unverbundene Pads
- 0 Footprint-Fehler
- keine elektrischen Clearance-Fehler
- keine gemeldeten Verstöße gegen die projektspezifischen 230-V-Regeln
- 30 Warnungen vom Typ `lib_footprint_mismatch`

### Bewertung der 30 Library-Mismatch-Warnungen

Die projektlokale Bibliothek `Fenstersteuerung.pretty` wird von KiCad korrekt gefunden.

Ein exemplarischer Direktvergleich des Sicherungshalters
`Fenstersteuerung:Littelfuse_646_5x20`
mit dem auf der PCB eingebetteten Footprint zeigt:

- gleicher Padabstand: 22,7 mm
- gleiche Padgröße: 2,4 x 2,4 mm
- gleicher Bohrdurchmesser: 1,4 mm
- gleicher Gehäuseumriss
- keine Abweichung der für die Fertigung relevanten Grundgeometrie festgestellt

Die verbleibende Warnung ist daher nach aktuellem Prüfstand als Bibliotheks-/Metadatenabweichung zu behandeln und nicht als elektrischer DRC-Fehler.

Vor Fertigungsfreigabe bleiben dennoch erforderlich:

1. Gerber- und Drill-Daten erzeugen.
2. Gerber visuell gegen die PCB prüfen.
3. reale Kaufteile gegen die Footprints prüfen.
4. 230-V-Sicherheitsprüfung separat abschließen.
