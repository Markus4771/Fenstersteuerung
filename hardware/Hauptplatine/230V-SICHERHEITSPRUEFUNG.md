# 230-V-Sicherheitsprüfung Hauptplatine

Stand: v3.3ebs2  
Status: Entwicklungs-/Prototypenprüfung, **keine formale Sicherheits- oder Fertigungsfreigabe**

## Aktueller Prüfstand

Aktive PCB-Datei:

`hardware/Hauptplatine/Hauptplatine_v3.3ebs2_library_clean_fixed_150x90.kicad_pcb`

Verwendete Designregeln:

`hardware/Hauptplatine/Hauptplatine.kicad_dru`

Interne Entwicklungsziele:

- mindestens 1,5 mm zwischen unterschiedlichen Netzspannungs-Potentialen
- mindestens 6 mm zwischen Netzspannung und SELV
- mindestens 2,0 mm Leiterbahnbreite für Netzspannungsnetze

## Statisch verifizierte Punkte v3.3ebs2

### Netzspannungs-Leiterbahnbreite

Aus der PCB-Datei wurden 56 Leiterbahnsegmente der folgenden Netze ausgewertet:

- AC_L
- AC_N
- PE
- L_PSU_FUSED
- L_MOTOR
- UP_FEED
- DN_FEED
- MOTOR_UP
- MOTOR_DN

Ergebnis:

- kleinste gefundene Leiterbahnbreite: **2,0 mm**
- Segmente unter 2,0 mm: **0**

Damit wird das interne Entwicklungsziel für die Leiterbahnbreite in der statisch ausgewerteten PCB-Geometrie eingehalten.

### Trennung Netzspannung / SELV

Die Leiterplatte besitzt eine echte Kupfer-Keepout-Zone auf F.Cu und B.Cu:

- x = 71 .. 77 mm
- Breite = **6 mm**
- Tracks verboten
- Vias verboten
- Copper Pour verboten

Statische Auswertung der 56 Netzspannungs-Leiterbahnsegmente:

- Netzspannungs-Tracks, die die Keepout-Zone durchqueren: **0**

### Komponenten-/Footprint-Stand

Folgende zuvor problematische Footprints wurden im aktuellen Stand korrigiert bzw. auf die projektlokale Bibliothek umgestellt:

- Mean Well IRM-20-5
- Finder 40.52
- Littelfuse 646 Sicherungshalter
- ESP32-S3 DevKitC-1 Carrier
- Phoenix MKDS Klemmen
- MOV S14K275
- weitere Standard-Footprints in `Fenstersteuerung.pretty`

## Bereits umgesetzte Schutzmaßnahmen

- 6-mm-Trennzone zwischen 230-V-/Motorbereich und SELV-/Logikbereich
- 2,0-mm-Netzspannungsleiterbahnen
- getrennte Sicherung für Netzteilzweig und Motorzweig
- MOV auf der Netzseite
- galvanisch getrenntes AC/DC-Netzteil
- Relaiskontakte räumlich von den Spulenanschlüssen getrennt
- PE wird als eigenes Netz von J1 zum Motorausgang J5 geführt

## Noch vor Prototypenbestellung erforderlich

1. PCB in KiCad 10 öffnen.
2. Alle Kupferzonen neu füllen.
3. `Hauptplatine.kicad_dru` im Design Rule Editor laden bzw. prüfen.
4. „Check rule syntax“ ausführen.
5. vollständigen KiCad-DRC ausführen.
6. insbesondere alle MAINS-different-net-clearance-Verstöße prüfen.
7. bestätigen, dass zwischen allen unterschiedlichen 230-V-Potentialen mindestens das interne Ziel von 1,5 mm erreicht wird.
8. tatsächliche Kaufteile gegen Footprints prüfen:
   - Littelfuse 646
   - Finder 40.52.9.005.0000
   - Mean Well IRM-20-5
   - Phoenix-Klemmen
   - MOV
9. Gerber- und Drill-Daten visuell prüfen.
10. Erstinbetriebnahme ohne Netzspannung durchführen.
11. Netzspannung erst nach separater elektrischer Sicherheitskontrolle anlegen.

## Normative Einordnung

Die oben verwendeten Abstände sind konservative interne Entwicklungsziele und stellen **keinen Nachweis einer Normkonformität** dar.

Die tatsächlich erforderlichen Luft- und Kriechstrecken hängen unter anderem ab von:

- anzuwendender Produktnorm
- Überspannungskategorie
- Verschmutzungsgrad
- Leiterplatten-Materialgruppe / CTI
- Einsatzhöhe
- Basisisolation oder verstärkter Isolation
- Gehäuse und Berührschutz
- Absicherung und Fehlerfallbetrachtung

Vor einem produktiven Einsatz an 230 V ist deshalb eine separate fachliche Sicherheitsbewertung erforderlich.

## Ergebnis v3.3ebs2

Der aktuelle PCB-Stand ist gegenüber den früheren Revisionen wesentlich bereinigt:

- keine Netzspannungsleiterbahn unter 2,0 mm
- keine Netzspannungsleiterbahn durchquert die 6-mm-SELV-Keepout-Zone
- Routing ist vollständig
- Footprints wurden auf die projektlokale Bibliothek vereinheitlicht

Die **finale Freigabe hängt jetzt vor allem vom vollständigen KiCad-DRC mit den Hochspannungsregeln sowie der Prüfung der realen Kaufteile ab**.
