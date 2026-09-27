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

**Hauptplatine_v3.3al_logic_minimal_150x90.kicad_pcb**

Repository-Pfad:

`hardware/Hauptplatine/Hauptplatine_v3.3al_logic_minimal_150x90.kicad_pcb`

Wichtig:

- v3.3h ist noch nicht vollständig geroutet
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


## Update v3.3h

DRC v3.3g:
- 63 DRC-Verstöße
- 54 offene Verbindungen
- keine gemeldeten Kurzschluss- oder Leiterbahnkreuzungs-Kategorien
- TOP/BOT-Coil-Leitungen hatten offene Enden, weil die Startpunkte nicht auf den tatsächlichen U4/U5-Pads lagen

v3.3h:
- alle bisherigen TOP_COIL_1–4- und BOT_COIL_1–4-Tracks entfernt
- acht Coil-Netze neu direkt von U4/U5 zu J6/J7 geroutet
- tatsächliche Padkoordinaten aus dem DRC/Board verwendet
- nächster Schritt: DRC v3.3h prüfen, erst danach STEP_TOP/STEP_BOT routen


## Update v3.3i

Grund:
- DRC v3.3h zeigte erneut Pad-Kollisionen/Kreuzungen durch direkte diagonale Coil-Leitungen über U4/U5.

Änderungen:
- alle TOP_COIL_1–4- und BOT_COIL_1–4-Tracks aus v3.3h entfernt
- neue Routingstrategie: zuerst horizontal rechts aus U4/U5 herausfächern
- TOP_COIL_1/2 und BOT_COIL_1/2 auf F.Cu
- TOP_COIL_3/4 und BOT_COIL_3/4 mit gezielten Vias auf B.Cu
- keine direkte Diagonalführung mehr durch den DIP-Padbereich
- nächster Schritt: DRC v3.3i prüfen


## Update v3.3j

Wichtige Korrektur:
- Die bisherigen U4/U5-Padkoordinaten waren bei den Routingversuchen falsch transformiert.
- U4/U5 sind im PCB um 90 Grad gedreht; lokale Padkoordinaten müssen entsprechend in globale Koordinaten umgerechnet werden.
- Diese falsche Umrechnung war die Hauptursache der Coil-Padkollisionen in v3.3h/v3.3i.

v3.3j:
- alle TOP/BOT-Coil-Tracks aus der Basis entfernt
- nur TOP_COIL_1–4 neu geroutet
- korrekte globale U4-Padkoordinaten verwendet:
  - TOP1 U4.16: 139.42 / 88.12
  - TOP2 U4.15: 136.88 / 88.12
  - TOP3 U4.14: 134.34 / 88.12
  - TOP4 U4.13: 131.80 / 88.12
- korrekte globale J6-Padkoordinaten:
  - TOP1 J6.4: 164.50 / 91.00
  - TOP2 J6.3: 164.50 / 88.50
  - TOP3 J6.2: 164.50 / 86.00
  - TOP4 J6.1: 164.50 / 83.50
- alle vier TOP-Coils liegen in v3.3j auf B.Cu
- BOT_COIL bleibt bewusst offen
- nächster Schritt: DRC v3.3j prüfen


## Update v3.3k

DRC von v3.3j ausgewertet:
- 79 DRC-Verstöße
- 54 offene Verbindungen
- 3 Kurzschlussfehler
- Hauptursache: TOP-Coil-Startkoordinaten lagen fälschlich bei y=88.12 mm und liefen dadurch durch U5.

Korrektur:
- tatsächliche Koordinaten direkt aus dem KiCad-DRC übernommen
- U4-Ausgänge:
  - TOP1 U4.16: 139.42 / 72.88
  - TOP2 U4.15: 141.96 / 72.88
  - TOP3 U4.14: 144.50 / 72.88
  - TOP4 U4.13: 147.04 / 72.88
- J6:
  - TOP1 J6.4: 164.50 / 76.00
  - TOP2 J6.3: 164.50 / 78.50
  - TOP3 J6.2: 164.50 / 81.00
  - TOP4 J6.1: 164.50 / 83.50
- nur TOP_COIL_1–4 neu auf F.Cu geroutet
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3k


## Update v3.3l

DRC v3.3k:
- 62 DRC-Verstöße
- 47 offene Verbindungen
- TOP-Coils weiterhin fehlerhaft
- Probleme: TOP_COIL_1/3 kreuzten sich, TOP_COIL_3 kurzschloss TOP_COIL_4, mehrere Clearance- und Soldermask-Konflikte an U4

Ursache:
- Die Pinreihenfolge U4 -> J6 ist geometrisch invertiert. Vier direkte Leitungen auf einer Lage können daher nicht kreuzungsfrei geführt werden.

v3.3l:
- alle TOP/BOT-Coil-Tracks entfernt
- nur TOP_COIL_1–4 neu geroutet
- TOP1 und TOP3 auf B.Cu
- TOP2 und TOP4 auf F.Cu
- äußere und innere Routingkorridore getrennt
- keine Vias erforderlich
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3l


## Update v3.3m

DRC v3.3l:
- 69 DRC-Verstöße
- 50 offene Verbindungen
- 4 Kurzschlüsse
- 2 Clearance-Verstöße
- 2 dangling Tracks
- 0 Footprint-Fehler

Hauptprobleme:
- TOP_COIL_2 kollidiert mit R5/D2/Q2 und +5 V
- TOP_COIL_1 läuft in den J7/BOT-Bereich
- TOP_COIL_3 zu nah an U4-GND
- die äußeren Routingkorridore sind damit ungeeignet

v3.3m:
- alle TOP/BOT-Coil-Routen aus v3.3l entfernt
- nur TOP_COIL_1–4 neu geroutet
- je TOP-Netz zwei Vias
- erster Via direkt oberhalb der U4-Ausgangsreihe
- Hauptführung auf B.Cu außerhalb des U4-/R5-/Q2-/D2-Bereichs
- zweiter Via erst rechts neben U4, kurz vor J6
- Endstücke zu J6 auf F.Cu
- BOT_COIL bleibt bewusst offen
- nächster Schritt: DRC v3.3m


## Update v3.3n

DRC v3.3m:
- 61 DRC-Verstöße
- 47 offene Verbindungen
- nur noch 2 Kurzschlüsse bei TOP-Coils
- 1 Clearance-Verstoß
- 1 dangling Track
- 0 Footprint-Fehler

Konkrete TOP-Probleme:
- TOP_COIL_3 zu nah an Via/Track von TOP_COIL_4
- TOP_COIL_4 lief mit seiner B.Cu-Strecke zu nah an der J6-Padreihe und kurzschloss TOP_COIL_1
- zusätzlich mehrere Soldermask-Bridge-Warnungen an J6
- Via von TOP_COIL_4 lag direkt neben J6.1 und erzeugte Hole-to-hole-Warnungen

v3.3n:
- TOP_COIL_1–4 vollständig neu geroutet
- vertikale Hauptkorridore nach rechts von J6 verlegt
- J6 wird horizontal von rechts angefahren
- TOP3/TOP4-Vias weiter voneinander getrennt
- keine Vias mehr direkt an J6
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3n


## Update v3.3o

DRC v3.3n:
- 63 DRC-Verstöße
- 47 offene Verbindungen
- 5 Tracks-Crossing-Fehler
- 2 Kurzschlüsse
- 1 dangling Track
- 0 Footprint-Fehler

TOP-Probleme:
- TOP_COIL_1 kreuzte TOP_COIL_4
- TOP_COIL_3 kreuzte TOP_COIL_4
- TOP_COIL_2 kreuzte TOP_COIL_3 und TOP_COIL_4
- TOP_COIL_1 kreuzte TOP_COIL_2
- TOP_COIL_1 kollidierte zusätzlich mit +5 V an D2

v3.3o:
- TOP_COIL_1–4 vollständig neu geroutet
- keine Vias
- TOP1/TOP3 auf B.Cu
- TOP2/TOP4 auf F.Cu
- vier kurze Fluchtkorridore oberhalb der U4-Ausgangsreihe
- rechte Vertikalstücke bewusst gestaffelt
- Endstücke zu J6 so angeordnet, dass sie die jeweils andere Vertikalstrecke nicht schneiden
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3o


## Update v3.3p

DRC v3.3o:
- 54 DRC-Verstöße
- 47 offene Verbindungen
- 0 Kurzschlüsse
- 2 Tracks-Crossing-Fehler
- 1 dangling Track (REED_TILT)
- restliche Meldungen überwiegend Bibliotheks-/Silkscreen-Warnungen

Verbleibende TOP-Kreuzungen:
- TOP_COIL_2 <-> TOP_COIL_4 auf F.Cu
- TOP_COIL_1 <-> TOP_COIL_3 auf B.Cu

v3.3p:
- nur TOP_COIL_1–4 angepasst
- auf B.Cu die rechten Vertikalkorridore von TOP1/TOP3 getauscht
- auf F.Cu die rechten Vertikalkorridore von TOP2/TOP4 getauscht
- Ziel: keine Kreuzungen mehr innerhalb derselben Lage
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3p


## Update v3.3q

DRC v3.3p:
- 54 DRC-Verstöße
- 47 offene Verbindungen
- 0 Kurzschlüsse
- weiterhin 2 Tracks-Crossing-Fehler:
  - TOP_COIL_2 <-> TOP_COIL_4 auf F.Cu
  - TOP_COIL_1 <-> TOP_COIL_3 auf B.Cu
- 1 dangling Track bei REED_TILT
- Rest überwiegend Bibliotheks-/Silkscreen-Warnungen

Schlussfolgerung:
- Die bisherige senkrechte J6-Ausrichtung erzwingt unnötig komplizierte TOP-Routen.
- Weiteres Um-die-Pads-Routen ist nicht sinnvoll.

v3.3q:
- J6 mechanisch gedreht und nach oben versetzt
- neue J6-Position: 164.5 / 72.88, Rotation 180 Grad
- dadurch liegen TOP1..TOP4 in derselben Reihenfolge wie U4.16..U4.13
- TOP_COIL_1–4 komplett neu auf F.Cu geroutet
- vier gestaffelte horizontale Routing-Lanes
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3q


## Update v3.3r

DRC v3.3q:
- 72 DRC-Verstöße
- 47 offene Verbindungen
- 5 Tracks-Crossing-Fehler
- 3 Kurzschlussmeldungen
- 2 Clearance-Fehler
- 2 Hole-to-hole-Warnungen
- 1 dangling Track bei REED_TILT
- 0 Footprint-Fehler

Hauptursache:
- J6 war nach der Drehung zu dicht an U4 positioniert.
- J6.4 / TOP_COIL_1 lag praktisch auf U4.9 / +5V.
- J6.5 / +5V lag praktisch auf U4.10 / no-net.

v3.3r:
- J6 bleibt 180 Grad gedreht
- J6 von 164.5 / 72.88 auf 168.5 / 63.5 verschoben
- damit kein Pad-Overlap mit U4 mehr
- TOP_COIL_1–4 komplett neu auf B.Cu geroutet
- neue J6-Zielpunkte:
  - TOP1: 161.0 / 63.5
  - TOP2: 163.5 / 63.5
  - TOP3: 166.0 / 63.5
  - TOP4: 168.5 / 63.5
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3r


## Update v3.3s

DRC v3.3r:
- 73 DRC-Verstöße
- 47 offene Verbindungen
- 5 Tracks-Crossing-Fehler
- 4 Kurzschlussmeldungen
- 8 Soldermask-Bridge-Meldungen
- 1 dangling Track
- 0 Footprint-Fehler

Schlussfolgerung:
- v3.3r ist schlechter als v3.3o/v3.3p.
- Die J6-Verlegung nach oben wird verworfen.
- Bester stabiler Ausgangspunkt bleibt v3.3o: 0 Kurzschlüsse, nur 2 TOP-Kreuzungen.

v3.3s:
- Basis: v3.3o
- J6 wieder in der v3.3o-Position
- TOP1 und TOP2 bleiben auf ihren ursprünglichen Lagen
- TOP3 und TOP4 erhalten nur lokal je zwei Vias
- Layerwechsel nur an den beiden bisherigen Kreuzungsstellen
- Ziel: die zwei letzten TOP-Kreuzungen beseitigen, ohne neue lange Korridore zu erzeugen
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3s


## Update v3.3t

DRC v3.3s:
- 60 DRC-Verstöße
- 47 offene Verbindungen
- mehrere TOP-Kurzschlüsse
- mehrere TOP-Kreuzungen
- 1 Clearance-Fehler
- 1 dangling Track bei REED_TILT
- 0 Footprint-Fehler

Schlussfolgerung:
- Lokale Via-Hops lösen die TOP-Geometrie nicht robust.
- Stattdessen wird die Pinbelegung von J6 logisch neu geordnet.
- Bei den vier 28BYJ-Coils kann die Reihenfolge der Coil-Ausgänge frei zugeordnet und später in der Software-Schrittfolge angepasst werden.

v3.3t:
- Basis: v3.3o
- J6 bleibt mechanisch unverändert
- J6-Padbelegung geändert:
  - J6.1 = TOP_COIL_1
  - J6.2 = TOP_COIL_2
  - J6.3 = TOP_COIL_3
  - J6.4 = TOP_COIL_4
  - J6.5 = +5V unverändert
- TOP_COIL_1–4 neu als monotone, gestaffelte F.Cu-Routen geführt
- Ziel: keinerlei Kreuzungszwang mehr
- Hinweis: Schematic/Pin-Mapping muss später konsistent nachgezogen werden
- BOT_COIL bleibt weiterhin offen
- nächster Schritt: DRC v3.3t


## Update v3.3u

Ausgangslage:
- v3.3t war deutlich schlechter als v3.3o.
- bester stabiler Stand bleibt v3.3o mit 0 Kurzschlüssen und 2 TOP-Kreuzungen.

v3.3u:
- Basis: v3.3o
- J6-Pinbelegung wieder unverändert wie in v3.3o
- TOP_COIL_1–4 vollständig neu geroutet
- jede Leitung wird zuerst senkrecht aus dem U4-Padfeld herausgeführt
- erst außerhalb des Padfelds erfolgt die seitliche Führung zu J6
- TOP1/TOP3 auf B.Cu
- TOP2/TOP4 auf F.Cu
- keine Vias
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3u


## Update v3.3v

DRC v3.3u:
- 71 DRC-Verstöße
- 47 offene Verbindungen
- 4 Kurzschlüsse
- 1 Tracks-Crossing
- 14 Soldermask-Bridge-Meldungen
- 1 dangling Track bei REED_TILT
- 0 Footprint-Fehler

Hauptursache:
- Die neue vertikale TOP-Fluchtführung kollidierte mit der K2-Treiberzeile R5/Q2/D2.
- Besonders TOP_COIL_2 und TOP_COIL_4 trafen R5, Q2 und D2.

v3.3v:
- Basis: v3.3o
- R5/Q2/D2 von y=67 mm auf y=64 mm verschoben
- dadurch freier Routing-Korridor zwischen K2-Treiberzeile und U4
- TOP_COIL_1–4 vollständig neu geroutet
- TOP1/TOP2 auf B.Cu
- TOP3/TOP4 auf F.Cu
- keine Vias
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3v


## Update v3.3w

DRC v3.3v:
- 54 DRC-Verstöße
- 47 offene Verbindungen
- 0 Kurzschlüsse
- 2 Tracks-Crossing-Fehler
- 1 dangling Track bei REED_TILT
- 0 Footprint-Fehler

Verbleibende TOP-Kreuzungen:
- TOP_COIL_1 <-> TOP_COIL_2 auf B.Cu
- TOP_COIL_3 <-> TOP_COIL_4 auf F.Cu

v3.3w:
- Basis: v3.3v
- Bauteilpositionen unverändert
- nur die rechten Vertikalkorridore der TOP-Paare getauscht
- B.Cu: TOP1 innen, TOP2 außen
- F.Cu: TOP3 innen, TOP4 außen
- Ziel: die letzten zwei TOP-Kreuzungen beseitigen
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3w


## Update v3.3x

DRC v3.3w:
- 54 DRC-Verstöße
- 47 offene Verbindungen
- 0 Kurzschlüsse
- 2 Tracks-Crossing-Fehler
- 1 dangling Track bei REED_TILT
- 29 Bibliothekswarnungen
- 19 Silkscreen-over-Copper-Warnungen
- 2 Silkscreen-Overlap-Warnungen
- 1 Silkscreen-Edge-Warnung

Verbleibende TOP-Kreuzungen:
- TOP_COIL_1 <-> TOP_COIL_2 bei ca. x=168.2 / y=69.0
- TOP_COIL_3 <-> TOP_COIL_4 bei ca. x=166.6 / y=71.0

v3.3x:
- Basis: v3.3w
- TOP1 und TOP3 bleiben auf ihren bisherigen Lagen
- TOP2 erhält einen kurzen F.Cu-Bridge-Abschnitt nur über den TOP1-Kreuzungspunkt
- TOP4 erhält einen kurzen B.Cu-Bridge-Abschnitt nur über den TOP3-Kreuzungspunkt
- vier kleine 0.8/0.4-mm-Vias nur für diese beiden Brücken
- keine Änderungen an Bauteilpositionen
- BOT_COIL weiterhin offen
- nächster Schritt: DRC v3.3x


## Update v3.3y

DRC v3.3x:
- 52 DRC-Verstöße
- 47 offene Verbindungen
- 0 Kurzschlüsse
- 0 Tracks-Crossing-Fehler
- 1 dangling Track bei REED_TILT
- 0 Footprint-Fehler
- TOP_COIL_1–4 damit erstmals sauber geroutet

v3.3y:
- Basis: v3.3x
- TOP-Routing unverändert übernommen
- BOT_COIL_1–4 neu geroutet
- erfolgreiche TOP-Strategie gespiegelt:
  - BOT1/BOT2 primär B.Cu
  - BOT3/BOT4 primär F.Cu
  - BOT2 mit kurzer F.Cu-Brücke
  - BOT4 mit kurzer B.Cu-Brücke
  - insgesamt vier kleine 0.8/0.4-mm-Vias für die beiden lokalen Brücken
- keine Bauteilpositionen verändert
- nächster Schritt: DRC v3.3y


## Update v3.3aa

DRC v3.3z:
- 54 DRC-Verstöße
- 43 offene Verbindungen
- 0 Kurzschlüsse
- 2 Tracks-Crossing-Fehler
- 1 dangling Track bei REED_TILT
- 0 Footprint-Fehler

Verbleibende BOT-Kreuzungen:
- BOT_COIL_1 <-> BOT_COIL_2 auf B.Cu
- BOT_COIL_3 <-> BOT_COIL_4 auf F.Cu

v3.3aa:
- Basis: v3.3z
- TOP-Routing unverändert
- BOT1 und BOT3 bleiben auf ihren bisherigen Lagen
- BOT2 bekommt einen kurzen F.Cu-Brückenabschnitt
- BOT4 bekommt einen kurzen B.Cu-Brückenabschnitt
- insgesamt vier kleine 0.8/0.4-mm-Vias für die beiden lokalen Brücken
- keine Bauteile verschoben
- nächster Schritt: DRC v3.3aa


## Update v3.3ab

DRC v3.3aa:
- 54 DRC-Verstöße
- 43 offene Verbindungen
- 0 Kurzschlüsse
- weiterhin 2 Tracks-Crossing-Fehler
- 1 dangling Track bei REED_TILT
- 0 Footprint-Fehler

Verbleibende BOT-Kreuzungen:
- BOT_COIL_1 <-> BOT_COIL_2
- BOT_COIL_3 <-> BOT_COIL_4

Ursache:
- Die Layer-Brücken in v3.3aa lagen zu spät.
- BOT2 kreuzte BOT1 bereits auf dem senkrechten Abgang direkt unter U5.
- BOT4 kreuzte BOT3 ebenfalls bereits direkt unter U5.

v3.3ab:
- Basis: v3.3aa
- TOP-Routing unverändert
- BOT1 und BOT3 unverändert
- BOT2 wechselt direkt am U5-Ausgang auf F.Cu und bei y=90.4 zurück auf B.Cu
- BOT4 wechselt direkt am U5-Ausgang auf B.Cu und bei y=92.8 zurück auf F.Cu
- keine Bauteile verschoben
- nächster Schritt: DRC v3.3ab


## Update v3.3ak

DRC v3.3ak:
- 52 DRC-Verstöße
- 43 offene Verbindungen
- 0 Kurzschlüsse
- 0 Tracks-Crossing-Fehler
- 0 Clearance-Fehler
- 0 Hole-Clearance-/Hole-to-Hole-Fehler
- 1 dangling Track bei REED_TILT
- übrige DRC-Meldungen sind Bibliotheks-/Silkscreen-Warnungen

Wichtiger Meilenstein:
- TOP_COIL_1–4 sauber geroutet
- BOT_COIL_1–4 sauber geroutet
- U4/J6 und U5/J7 folgen jetzt demselben Routingprinzip
- Coil-Routing damit abgeschlossen

Offene Netze laut DRC betreffen jetzt vor allem:
- GND
- +5V
- +3V3
- STEP_TOP_1–4
- STEP_BOT_1–4
- REED_OPEN
- REED_TILT
- RS485_A / RS485_B / RS485_DE / RS485_RX / RS485_TX
- K1_COIL_LOW / K2_COIL_LOW
- Q1_BASE / Q2_BASE
- ROLL_UP / ROLL_DN

Nächster Schritt:
- offene Logik- und Versorgungssignale blockweise routen
- zuerst STEP_TOP/STEP_BOT, danach RS485/Reed, danach Versorgung/GND


## Update v3.3al

Basis:
- v3.3ak mit sauberem TOP- und BOT-Coil-Routing
- DRC v3.3ak: 0 Kurzschlüsse, 0 Leiterbahnkreuzungen, 0 Clearance-/Hole-Fehler, 43 offene Verbindungen

v3.3al:
- STEP_TOP_1–4 vollständig geroutet
- U1 -> U4
- STEP_TOP_1/2 vollständig auf F.Cu
- STEP_TOP_3/4 überwiegend F.Cu
- kurze B.Cu-Brücken nur zum Überqueren der vorhandenen REED_TILT-Führung
- 0.3-mm-Signalbahnen
- Coil-Routing unverändert
- U5/J7 unverändert

Nächster DRC-Checkpoint:
- STEP_TOP_1–4 prüfen
- danach STEP_BOT_1–4, RS485/Reed und Relaissteuerung weiter routen


## Aktueller Stand v3.3bd

### Bestätigte Meilensteine

v3.3ak:
- TOP_COIL_1–4 sauber
- BOT_COIL_1–4 sauber
- 0 Kurzschlüsse
- 0 Tracks-Crossing
- 0 Clearance-/Hole-Fehler

v3.3as:
- STEP_TOP_1–4 sauber
- 51 DRC-Meldungen, nur Warnungen
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-Fehler
- 40 offene Verbindungen

v3.3az:
- STEP_BOT_1–4 sauber
- TOP/BOT-Coils weiterhin sauber
- STEP_TOP weiterhin sauber
- 51 DRC-Meldungen, nur Warnungen
- 0 echte Routingfehler
- 36 offene Verbindungen

v3.3bc:
- RS485_RX sauber
- RS485_TX sauber
- RS485_DE sauber
- 51 DRC-Meldungen, nur Warnungen
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-Fehler
- 32 offene Verbindungen

### v3.3bd

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3bd_logic_minimal_150x90.kicad_pcb`

Neu:
- REED_OPEN komplett neu geroutet
- REED_TILT komplett neu geroutet
- J2/J3, R1/R2 und C1/C2 eingebunden
- Coil-, STEP- und RS485_RX/TX/DE-Routing aus den bestätigten sauberen Ständen unverändert

Status:
- DRC v3.3bd steht noch aus
- RS485_A/B bewusst noch offen

### Danach weiter

1. DRC v3.3bd auswerten
2. RS485_A/B separat routen
3. ROLL_UP / ROLL_DN
4. Q1_BASE / Q2_BASE
5. K1_COIL_LOW / K2_COIL_LOW
6. +3V3
7. +5V
8. GND
9. komplette physische Konnektivitätsprüfung
10. finaler DRC
11. separate 230-V-Sicherheitsprüfung
12. Gerber/Drill/BOM

### Wichtig

Der aktuelle Entwicklungsstand ist **keine Fertigungsfreigabe**. Vor Fertigung muss der 230-V-Bereich separat auf Luft-/Kriechstrecken, Absicherung, Leiterbahnbreiten, Schutzmaßnahmen, Relais-/Motorpfade und Bauteileignung geprüft werden.


## Update v3.3bp bis v3.3bt

### Letzter vollständig bestätigter sauberer Stand: v3.3bp

DRC v3.3bp:

- 51 DRC-Meldungen, ausschließlich Bibliotheks-/Silkscreen-Warnungen
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-Fehler
- 0 Hole-/Dangling-Fehler
- 28 offene Verbindungen
- 0 Footprint-Fehler

Zusätzlich abgeschlossen:

- Q1_BASE
- Q2_BASE
- lokale K1_COIL_LOW-Verbindung Q1.1 ↔ D1.2
- lokale K2_COIL_LOW-Verbindung Q2.1 ↔ D2.2

### RS485_A

v3.3bq:
- kompletter A-Versuch bis J4
- offene Verbindungen auf 26 reduziert
- jedoch 7 Leiterbahnkreuzungen, 1 Kurzschluss und 1 Clearance-Fehler
- langer Weg deshalb verworfen

v3.3br:
- nur U3.6 ↔ R3.1 lokal
- 27 offene Verbindungen
- 2 Kurzschlussmeldungen gegen RS485_TX

v3.3bs:
- lokaler A-Weg rechts um U3 herum und unter RS485_TX
- 52 DRC-Meldungen
- 27 offene Verbindungen
- 0 Kurzschlüsse
- 0 Clearance-Fehler
- nur noch 1 Leiterbahnkreuzung gegen GND

v3.3bt:
- aktueller Arbeitsstand
- Basis: v3.3bs
- kurzer Layerwechsel auf B.Cu, um die GND-Leitung zu unterqueren
- danach zurück auf F.Cu zu R3.1
- J4.3 bleibt bewusst offen
- DRC v3.3bt steht noch aus

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3bt_logic_minimal_150x90.kicad_pcb`

### Aktuell bestätigte saubere Funktionsblöcke

- TOP_COIL_1–4
- BOT_COIL_1–4
- STEP_TOP_1–4
- STEP_BOT_1–4
- RS485_RX
- RS485_TX
- RS485_DE
- Q1_BASE
- Q2_BASE
- lokale K1_COIL_LOW-Verbindung
- lokale K2_COIL_LOW-Verbindung

### Noch offen

- DRC v3.3bt
- RS485_A Restweg zu J4.3
- RS485_B
- REED_OPEN
- REED_TILT
- ROLL_UP
- ROLL_DN
- K1_COIL_LOW Restweg zum Relais
- K2_COIL_LOW Restweg zum Relais
- +3V3
- +5V
- GND
- vollständige physische Konnektivitätsprüfung
- finaler DRC
- separate 230-V-Sicherheitsprüfung
- Gerber/Drill/BOM

### Arbeitsregel

Weiterhin nur ein kleines Netz bzw. eine klar abgegrenzte Teilverbindung pro DRC-Schritt ändern. Saubere Coil-, STEP-, RS485_RX/TX/DE- und Transistor-Basisnetze nicht erneut verändern, solange der DRC dies nicht zwingend erfordert.

Der aktuelle Stand ist keine Fertigungsfreigabe.


## DRC v3.3bt bestätigt

Ergebnis:
- 51 DRC-Meldungen, ausschließlich Bibliotheks-/Silkscreen-Warnungen
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-Fehler
- 0 Hole-/Dangling-Fehler
- 27 offene Verbindungen
- 0 Footprint-Fehler

Damit ist der lokale Abschnitt von RS485_A zwischen U3.6 und R3.1 sauber abgeschlossen.

Weiter offen:
- RS485_A Restweg R3.1 ↔ J4.3
- RS485_B U3.7 ↔ R3.2 ↔ J4.4
- REED_OPEN
- REED_TILT
- ROLL_UP
- ROLL_DN
- K1_COIL_LOW Restweg zum Relais
- K2_COIL_LOW Restweg zum Relais
- +3V3
- +5V
- GND

Aktueller bestätigter Arbeitsstand:
`hardware/Hauptplatine/Hauptplatine_v3.3bt_logic_minimal_150x90.kicad_pcb`
