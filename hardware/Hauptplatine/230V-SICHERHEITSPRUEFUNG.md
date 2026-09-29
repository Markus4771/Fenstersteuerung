# 230-V-Sicherheitsprüfung Hauptplatine

Stand: v3.3ebd  
Status: Entwicklungs-/Prototypenprüfung, **keine Sicherheits- oder Fertigungsfreigabe**

## Aktueller positiver Stand

- Elektrisches Standard-DRC zuletzt ohne offene Netze, Kurzschlüsse, Track-Kreuzungen oder normale Clearance-Fehler.
- 230-V/SELV-Trennzone ist als echter Keepout auf F.Cu und B.Cu umgesetzt:
  - x = 71..77 mm
  - Breite = 6 mm
  - Tracks/Vias/Copperpour verboten
- Gemessener kleinster Kupferabstand Primärseite -> SELV außerhalb der isolierenden Bauteile:
  - ca. 8,8 mm
- Netzspannungs-Leiterbahnen:
  - 2,0 mm Breite
- PS1 (Mean Well IRM-20-5) Primär-/Sekundär-Padabstände im Footprint:
  - deutlich > 40 mm
- K1/K2 Kontaktseite -> Spulenseite im Footprint:
  - kleinster gemessener Kupferabstand ca. 12,9 mm

## Änderungen v3.3ebc / v3.3ebd

### v3.3ebc
- obere AC_L-Leiterbahn weiter von der oberen Platinenkante nach innen verschoben
- Kupferabstand zur Platinenkante von ca. 1,5 mm auf ca. 3,0 mm erhöht

### v3.3ebd
- L_PSU_FUSED-Zweig zu PS1 neu geführt
- vorheriger Abstand L_PSU_FUSED zu PS1.1 / AC_N: ca. 0,3 mm
- neue Führung links um PS1 herum; kritische Annäherung beseitigt

## Neue KiCad-Hochspannungsregeln

Datei:
`hardware/Hauptplatine/Hauptplatine.kicad_dru`

Regeln:
1. unterschiedliche 230-V-Netze: mindestens 1,5 mm
2. 230 V -> SELV: mindestens 6 mm
3. 230-V-Leiterbahnbreite: mindestens 2,0 mm

Die Syntax muss vor Prototypenfreigabe im KiCad-10 Design Rule Editor mit "Check rule syntax" bestätigt werden.

## Noch erwartete Primär-Primär-Abstandsverstöße (< 1,5 mm)

Die statische Geometrieprüfung von v3.3ebd findet derzeit 16 Objektpaare unter 1,5 mm. Mehrere gehören zur selben geometrischen Problemstelle.

Wichtigste Bereiche:

- oberer AC_L-Zweig bei F1 / RV1:
  - ca. 0,8..1,0 mm
- AC_L im Relaisbereich um K1:
  - ca. 0,9..1,1 mm
- L_MOTOR zu K1.14 / MOTOR_UP:
  - ca. 0,98 mm
- AC_L zu DN_FEED:
  - ca. 1,1..1,14 mm
- DN_FEED zu K2.22 / UP_FEED:
  - ca. 1,12 mm
- MOTOR_DN nahe J5.2 / PE und J5.3 / MOTOR_UP:
  - ca. 1,3 mm

Diese Stellen werden erst nach KiCad-DRC mit den neuen Regeln endgültig bewertet und anschließend einzeln neu geroutet.

## Normative Einordnung

Die exakten Anforderungen hängen vom Endgerät ab, insbesondere von:
- Überspannungskategorie
- Verschmutzungsgrad
- Leiterplatten-Materialgruppe / CTI
- Einsatzhöhe
- erforderlicher Basisisolation oder verstärkter Isolation
- einschlägiger Produktnorm

Als konservative Entwicklungsziele werden aktuell verwendet:
- 1,5 mm zwischen verschiedenen Netzspannungs-Potentialen
- 6 mm zwischen Netzspannung und SELV

Diese Werte sind interne Designziele und keine Aussage über eine formale Normkonformität.

## Vor Prototypenbestellung noch erforderlich

1. KiCad-DRC mit `Hauptplatine.kicad_dru`
2. alle MAINS-clearance-Fehler bewerten und beseitigen
3. F1/T2A-Footprints gegen die tatsächlich bestellten Sicherungshalter prüfen
4. Finder-40.52-Footprint und Pinbelegung gegen Datenblatt / reales Relais prüfen
5. Mean-Well-IRM-20-5-Footprint gegen Datenblatt prüfen
6. PE-Führung und Klemmenzuordnung kontrollieren
7. GND-Zonen-/Silkscreen-/Library-Warnungen bereinigen
8. Fertigungsdaten visuell prüfen
9. erste Inbetriebnahme ohne Netzspannung
10. Netzspannungsprüfung erst nach separater Sicherheitskontrolle
