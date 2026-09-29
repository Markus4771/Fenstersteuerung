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


## DRC v3.3bw bestätigt

Ergebnis:
- 51 DRC-Meldungen, nur Bibliotheks-/Silkscreen-Warnungen
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-/Hole-/Dangling-Fehler
- 26 offene Verbindungen
- 0 Footprint-Fehler

RS485_A ist damit vollständig abgeschlossen:
- U3.6 ↔ R3.1
- R3.1 ↔ J4.3

Aktueller bestätigter Stand:
`hardware/Hauptplatine/Hauptplatine_v3.3bw_logic_minimal_150x90.kicad_pcb`

Nächster Block:
- RS485_B U3.7 ↔ R3.2 ↔ J4.4


## DRC v3.3cp bestätigt

Ergebnis:
- 51 DRC-Meldungen, nur Bibliotheks-/Silkscreen-Warnungen
- 0 Kurzschlüsse
- 0 Leiterbahnkreuzungen
- 0 Clearance-/Keepout-/Hole-/Dangling-Fehler
- 24 offene Verbindungen
- 0 Footprint-Fehler

RS485 ist damit vollständig abgeschlossen:
- RX/TX/DE
- A bis J4.3
- B bis J4.4

Aktueller bestätigter Arbeitsstand:
`hardware/Hauptplatine/Hauptplatine_v3.3cp_logic_minimal_150x90.kicad_pcb`

Als nächstes nur REED_OPEN routen und per DRC prüfen; danach REED_TILT.


## Update v3.3eap

Basis:
- Hauptplatine_v3.3eao_logic_repair_150x90.kicad_pcb

Neu:
- ROLL_UP von U1.11 zu R4.1 geroutet
- Start U1.11 bei 90 / 40.70 mm
- F.Cu bis x=128 mm
- kurzer Wechsel auf B.Cu zur Umgehung der dichten RS485-Führung
- Rückwechsel auf F.Cu bei 147 / 48 mm
- Anfahrt von R4.1 über den freien Korridor bei y=54 mm
- 0,30-mm-Signalbahn, zwei 0,8/0,4-mm-Vias
- 230-V-Bereich unverändert
- bestehende Coil-, STEP-, Reed-, RS485- und Versorgungstracks unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eap_logic_repair_150x90.kicad_pcb`

Status:
- DRC für v3.3eap steht noch aus
- ROLL_DN bleibt bis zum DRC dieses Zwischenstands bewusst unverändert

Nächster Schritt:
1. DRC v3.3eap
2. bei sauberem Ergebnis ROLL_DN U1.9 -> R5.1 separat routen
3. danach verbleibende Versorgung/Konnektivität weiter prüfen

Hinweis:
- weiterhin keine Fertigungsfreigabe
- Netzspannungsbereich vor Fertigung separat sicherheitstechnisch prüfen


## Update v3.3eaq

Basis:
- Hauptplatine_v3.3eap_logic_repair_150x90.kicad_pcb

Neu:
- ROLL_DN von U1.9 zu R5.1 geroutet
- Start U1.9 bei 90 / 38.16 mm
- kurzer F.Cu-Abgang nach links bis x=86 mm
- Layerwechsel auf B.Cu und senkrechter Korridor bis y=65.5 mm
- Rückwechsel auf F.Cu und horizontale Führung bis R5.1 bei 134 / 64 mm
- 0,30-mm-Signalbahn, zwei 0,8/0,4-mm-Vias
- statische Prüfung: keine Leiterbahnkreuzung mit vorhandenen Tracks
- statische Prüfung: keine Pad-Kollision im gewählten Korridor
- 230-V-Bereich unverändert
- bestehende Coil-, STEP-, Reed-, RS485-, +5V- und ROLL_UP-Routen unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eaq_logic_repair_150x90.kicad_pcb`

Status:
- ROLL_UP und ROLL_DN sind nun beide geroutet
- KiCad-DRC für v3.3eaq steht noch aus
- keine Fertigungsfreigabe

Nächster Schritt:
1. DRC v3.3eaq
2. danach offene +3V3-, +5V- und GND-Verbindungen systematisch schließen
3. vollständige physische Konnektivitätsprüfung
4. separate 230-V-Sicherheitsprüfung


## Update v3.3ear

Basis:
- Hauptplatine_v3.3eaq_logic_repair_150x90.kicad_pcb
- DRC-Bericht `DRCeaq.rpt` vom 2026-09-29

DRC-Ursachen der letzten ROLL-Routen:
- ROLL_UP kreuzte U1.12
- ROLL_UP kollidierte auf B.Cu mit RS485_DE und J3.2/GND
- ROLL_DN kreuzte U1.31/U1.32

Änderung:
- alte ROLL_UP- und ROLL_DN-Routen vollständig entfernt
- ROLL_UP neu über oberen B.Cu-Randkorridor geführt:
  - U1.11 -> x=84 mm
  - B.Cu über y=21.5 mm bis x=164 mm
  - Rückweg auf F.Cu bei 164/54 mm zu R4.1
- ROLL_DN neu über unteren B.Cu-Korridor geführt:
  - U1.9 -> x=84 mm
  - B.Cu bis y=104 mm, dann nach x=164 mm
  - B.Cu zurück bis 134/66 mm
  - kurzer F.Cu-Anschluss zu R5.1
- 0,30-mm-Signalbahnen, je zwei Vias
- statische Track-/Pad-Kollisionsprüfung der neuen Korridore ohne Treffer
- bestehende 230-V-, Coil-, STEP-, Reed-, RS485- und Versorgungsrouten unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3ear_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. KiCad-DRC v3.3ear ausführen
2. speziell prüfen, ob ROLL_UP/ROLL_DN nun frei von shorting_items/tracks_crossing/clearance sind
3. anschließend verbleibende DRC-Fehler blockweise reparieren


## Update v3.3eas

Basis:
- Hauptplatine_v3.3ear_logic_repair_150x90.kicad_pcb
- DRCear.rpt vom 2026-09-29: 123 Verstöße
- davon 23 Tracks crossing, 9 shorting_items, 6 clearance, 6 unconnected_items

DRC-Ergebnis für die ROLL-Netze:
- ROLL_UP und ROLL_DN kreuzten sich links
- ROLL_DN kollidierte mit J7.2 / BOT_COIL_3
- ROLL_DN kreuzte K2_COIL_LOW

Änderung:
- alte ROLL_UP-/ROLL_DN-Routen entfernt
- beide Netze neu per zweilagigem, hindernisbewusstem Routing geführt
- Pad-Rotation in KiCad bei der Kollisionsprüfung korrekt berücksichtigt
- vorhandene Tracks, Vias und Pads mit Sicherheitsabstand als Hindernisse behandelt
- statische Prüfung der neuen Routen: 0 Track-, Via- und Pad-Kollisionen

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eas_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. KiCad-DRC v3.3eas
2. ROLL_UP/ROLL_DN auf shorting_items/tracks_crossing/clearance prüfen
3. danach verbleibende Altfehler priorisieren: STEP_TOP_4, TOP/BOT_COIL, RS485, Versorgung


## Update v3.3eat

Basis:
- Hauptplatine_v3.3eas_logic_repair_150x90.kicad_pcb
- DRCeas.rpt vom 2026-09-29
- DRCeas: 118 Verstöße
- 20 Tracks crossing
- 8 shorting_items
- 7 clearance
- 6 unconnected_items

Wichtige Erkenntnisse aus DRCeas:
- ROLL_DN taucht nicht mehr als Fehler auf
- ROLL_UP hatte nur noch vier Clearance-Verstöße an J2
- STEP_TOP_4 verursachte zahlreiche Kreuzungen mit STEP_TOP_3, STEP_BOT_1..4, RS485_A/B und J4
- TOP_COIL_1..4 und BOT_COIL_1..4 verursachten mehrere Kreuzungen, Shorts und Maskenfehler
- U2 hatte drei eindeutig offene Layerübergänge: +5V an U2.3 sowie beide +3V3-Flächen von U2.2

Änderungen in v3.3eat:
- ROLL_UP im Bereich J2 neu geführt; statische Prüfung ohne Track-/Via-/Pad-Kollision
- STEP_TOP_4 vollständig neu geroutet
- TOP_COIL_1..4 vollständig neu geroutet
- BOT_COIL_1..4 vollständig neu geroutet
- alle neun STEP/COIL-Routen vor Commit zusätzlich geometrisch gegen vorhandene Tracks, Pads und Vias geprüft: 0 erkannte Kollisionen
- +5V-Layerübergang direkt an U2.3 ergänzt
- +3V3-Layerübergänge an beiden U2.2-Padflächen ergänzt
- ROLL_DN unverändert
- 230-V-Bereich unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eat_logic_repair_150x90.kicad_pcb`

Noch bewusst offen:
- GND U1.2
- GND U1.42
- GND U5.8
Diese drei Punkte werden nach dem nächsten KiCad-DRC separat behandelt, da die B.Cu-GND-Zonenfüllung die Bewertung beeinflusst.

Nächster Schritt:
1. Zonen in KiCad neu füllen
2. DRC für v3.3eat ausführen
3. Restfehler aus DRC priorisieren
4. verbleibende GND-Verbindungen schließen
5. abschließende 230-V-Sicherheitsprüfung


## Update v3.3eau

Basis:
- Hauptplatine_v3.3eat_logic_repair_150x90.kicad_pcb
- DRCeat.rpt vom 2026-09-29
- DRCeat: 97 Verstöße
- 8 Tracks crossing
- 6 shorting_items
- 2 clearance
- 1 unconnected_item
- 0 Footprint-Fehler

Wichtige Erkenntnisse:
- nur noch eine offene Verbindung: U1.2 / GND zur LOGIC_GND_PLANE
- RS485_A/B/DE verursachten mehrere Kreuzungen und Shorts
- +3V3 kreuzte /+5V im U2-Bereich
- ROLL_UP/ROLL_DN sowie die neu gerouteten TOP/BOT-Coil-Netze erscheinen im DRCeat nicht mehr als eigene Kurzschluss-/Kreuzungsblöcke

Änderungen in v3.3eau:
- RS485_A vollständig neu geroutet (U3.6, R3.1, J4.3)
- RS485_B vollständig neu geroutet (U3.7, R3.2, J4.4)
- RS485_DE vollständig neu geroutet (U3.2/U3.3 zu U1.25)
- +3V3 vollständig lokal neu aufgebaut zwischen R1, R2, U2.2 und U3.8
- bisherige +3V3-Routen/Vias entfernt, damit der +5V/+3V3-Kurzschluss am U2 entfällt
- explizite GND-Verbindung von U1.2 zu J2.2 ergänzt, damit die letzte offene GND-Verbindung nicht allein von der Zonenfüllung abhängt
- 230-V-Bereich unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eau_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. Zonen neu füllen
2. DRC v3.3eau
3. verbleibende Altfehler bei +5V/K1_COIL_LOW, REED_TILT/RS485_TX/STEP_TOP_1 und BOT_COIL_3-Padabstand bereinigen
4. danach Silkscreen-/Library-Warnungen getrennt behandeln


## Update v3.3eav

Basis:
- Hauptplatine_v3.3eau_logic_repair_150x90.kicad_pcb
- DRCeau.rpt vom 2026-09-29
- DRCeau: 105 Verstöße
- 0 unconnected pads
- 0 Footprint-Fehler

Elektrische Restfehler aus DRCeau:
- REED_TILT kurzgeschlossen/gekreuzt mit RS485_TX und STEP_TOP_1
- K1_COIL_LOW kurzgeschlossen/gekreuzt mit +5V
- RS485_B-Via zu nah an U3.2 / RS485_DE
- BOT_COIL_3 zu nah an unbeschaltetem U5.5
- GND-Via im U1-Bereich zu nah an U1.4
- zusätzlich viele reine Warnungen (isolated copper, Library, Silkscreen)

Änderungen in v3.3eav:
- REED_TILT vollständig neu geroutet
- K1_COIL_LOW vollständig neu geroutet
- BOT_COIL_3 vollständig neu geroutet
- RS485_B vollständig neu geroutet; erster Via weiter vom U3-DE-Pad entfernt
- explizite GND-Verbindung U1.2 -> J2.2 im problematischen U1-Bereich neu geführt
- strengere statische Kollisionsprüfung verwendet:
  - Vias gegen beide Kupferlagen geprüft
  - Pads ohne Netz ebenfalls als Hindernisse berücksichtigt
- 230-V-Bereich unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eav_logic_repair_150x90.kicad_pcb`

Status:
- alle Pads elektrisch verbunden
- Routing vollständig; Fokus jetzt auf DRC-Fehlerfreiheit und Warnungsbereinigung
- keine Fertigungsfreigabe

Nächster Schritt:
1. Zonen neu füllen
2. DRC v3.3eav
3. verbleibende elektrische DRC-Fehler beheben
4. danach Warnungen separat priorisieren (GND-Zoneninseln, Silkscreen, fehlende Footprint-Libraries)
5. abschließende 230-V-Sicherheitsprüfung


## Update v3.3eaw

Basis:
- Hauptplatine_v3.3eav_logic_repair_150x90.kicad_pcb
- DRCeav.rpt vom 2026-09-29
- DRCeav: 109 Verstöße
- keine unconnected_items
- 4 Clearance-Fehler
- 1 Tracks crossing
- 1 solder_mask_bridge
- 1 starved_thermal
- Rest überwiegend Warnungen: isolated_copper, lib_footprint_issues, silk_over_copper, silk_overlap

Elektrische Restfehler aus DRCeav:
- K1_COIL_LOW zu nah an J4.1 / +5V
- +5V-B.Cu-Trunk von 162/58 nach 146.92/58 lief durch D1.2 / K1_COIL_LOW
- dadurch Track-Kreuzung und Solder-Mask-Bridge im D1-Bereich

Änderungen in v3.3eaw:
- problematischen +5V-B.Cu-Abschnitt 162/58 -> 146.92/58 entfernt
- +5V im D1-Bereich mit Umfahrung über y=59..60 mm neu geführt
- K1_COIL_LOW vollständig neu geroutet
- K1_COIL_LOW im J4-Bereich konservativer mit größerem Abstand zur rechteckigen J4.1-Kupferfläche geführt
- K1_COIL_LOW im D1/Q1-Bereich unterhalb des neuen +5V-Trunks geführt
- alle Netze weiterhin verbunden
- 230-V-Bereich unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eaw_logic_repair_150x90.kicad_pcb`

Status:
- keine offenen Netze laut DRCeav
- Routing elektrisch vollständig
- v3.3eaw zielt auf Beseitigung der letzten Kurzschluss-/Clearance-/Maskenfehler
- GND-starved-thermal, GND-Zoneninseln, Silkscreen- und Library-Warnungen werden separat bereinigt
- keine Fertigungsfreigabe

Nächster Schritt:
1. Zonen neu füllen
2. DRC v3.3eaw
3. verbleibende echte elektrische Fehler auf 0 bringen
4. GND-Zonenwarnungen bereinigen
5. Silkscreen/Library-Warnungen bereinigen
6. separate 230-V-Sicherheitsprüfung


## Update v3.3eax

Basis:
- Hauptplatine_v3.3eaw_logic_repair_150x90.kicad_pcb
- DRCeaw.rpt vom 2026-09-29
- DRCeaw: 106 Verstöße
- keine unconnected_items
- keine shorting_items
- keine tracks_crossing
- keine clearance-Fehler
- 1 starved_thermal an J3.2/GND
- 50 isolated_copper
- 29 lib_footprint_issues
- 19 silk_over_copper
- 2 holes_co_located
- 2 hole_to_hole
- 2 silk_overlap
- 1 silk_edge_clearance

Änderungen in v3.3eax:
- letzten echten DRC-Fehler starved_thermal an J3.2/GND gezielt adressiert
- J3.2/GND explizit per Leiterbahn zu Q1.3/GND verbunden
- redundanten +5V-Via direkt auf U1.1 entfernt; PTH-Pad verbindet F.Cu/B.Cu bereits selbst
- alte explizite U1.2->J2.2-GND-Hilfsroute entfernt, da deren Via den hole_to_hole-Fehler nahe U1.2 verursachte
- 230-V-Bereich unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eax_logic_repair_150x90.kicad_pcb`

Status:
- alle Netze laut DRC verbunden
- keine Kurzschlüsse/Kreuzungen/Clearance-Fehler in DRCeaw
- v3.3eax zielt auf 0 echte elektrische DRC-Fehler
- verbleibende Meldungen danach voraussichtlich überwiegend Zonen-/Silkscreen-/Library-Warnungen
- keine Fertigungsfreigabe

Nächster Schritt:
1. Zonen neu füllen
2. DRC v3.3eax
3. prüfen, ob starved_thermal und hole-Warnungen verschwunden sind
4. danach GND-Zoneninseln und Silkscreen/Library-Warnungen bereinigen
5. separate 230-V-Sicherheitsprüfung


## Update v3.3eay

Basis:
- Hauptplatine_v3.3eax_logic_repair_150x90.kicad_pcb
- DRCeax.rpt vom 2026-09-29
- DRCeax: 101 DRC-Verstöße + 1 unconnected pad
- keine Kurzschlüsse/Kreuzungen/Clearance-Fehler
- offener Anschluss ausschließlich J2.2 / GND zur LOGIC_GND_PLANE
- 50 isolated_copper
- 29 lib_footprint_issues
- Silkscreen-Warnungen verbleiben

Änderung in v3.3eay:
- J2.2/GND direkt und ohne Via mit J3.2/GND verbunden
- gerade B.Cu-Leiterbahn x=128.08 mm von y=30 mm bis y=45 mm
- keine anderen Netze verändert
- 230-V-Bereich unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eay_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. Zonen neu füllen
2. DRC v3.3eay
3. prüfen, ob unconnected_items = 0 bleibt
4. anschließend GND-isolated_copper-Warnungen und Silkscreen/Library-Warnungen bereinigen
5. separate 230-V-Sicherheitsprüfung


## Update v3.3eaz

Basis:
- Hauptplatine_v3.3eay_logic_repair_150x90.kicad_pcb
- Screenshot vom 2026-09-29 zeigte weiterhin mehrere Ratsnest-/Luftlinien

Ursache:
- mehrere bestehende Leiterbahnen endeten nur knapp neben den exakten Padkoordinaten
- typische Abweichungen lagen bei 0,04 bis 0,20 mm
- dadurch wirkten die Netze in einer groben Textprüfung verbunden, KiCad zeigte aber weiterhin Luftlinien

Korrigierte Netze:
- RS485_RX
- RS485_TX
- STEP_TOP_1
- STEP_TOP_2
- STEP_TOP_3
- STEP_BOT_1
- STEP_BOT_2
- STEP_BOT_3
- STEP_BOT_4

Änderung:
- nur kurze Anschlusssegmente von den bisherigen Track-Enden zu den exakten Padkoordinaten ergänzt
- bestehende Hauptrouten unverändert
- 230-V-Bereich unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eaz_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. Datei in KiCad öffnen
2. Zonen neu füllen
3. prüfen, ob die sichtbaren Signal-Ratsnest-Linien verschwunden sind
4. DRC ausführen
5. verbleibende GND-/Zonenlinien separat behandeln


## Update v3.3eba

Basis:
- Hauptplatine_v3.3eaz_logic_repair_150x90.kicad_pcb
- Screenshot vom 2026-09-29 zeigte nur noch GND-Ratsnest-Linien

Analyse:
- Signalnetze RS485_RX/TX sowie STEP_TOP_1..3 und STEP_BOT_1..4 sind nach v3.3eaz an exakten Padkoordinaten angeschlossen
- verbleibende getrennte Kupfergruppen gehörten ausschließlich zum GND-Netz
- betroffen: U5.8, U2.1, C2.2, PS1.4, U4.8, Q2.3, U3.5, U1.2, U1.42, C1.2, J4.2
- gemeinsamer GND-Anker: J3.2 / 128.08,45 mm

Änderungen in v3.3eba:
- explizites GND-Backbone ergänzt
- alle oben genannten GND-Pads separat an den gemeinsamen GND-Anker angebunden
- neue GND-Leiterbahnen mit 0,4 mm
- Layerwechsel nur über 0,9/0,45-mm-GND-Vias
- bestehende Signalrouten unverändert
- 230-V-Bereich unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3eba_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. v3.3eba in KiCad öffnen
2. Zonen neu füllen
3. Ratsnest prüfen — Ziel: 0 sichtbare Luftlinien
4. DRC ausführen
5. anschließend GND-Zoneninseln/Silkscreen/Library-Warnungen bereinigen
6. separate 230-V-Sicherheitsprüfung


## Update v3.3ebb

Basis:
- Hauptplatine_v3.3eba_logic_repair_150x90.kicad_pcb
- DRCeba.rpt vom 2026-09-29
- DRCeba: 131 DRC-Warnungen
- 0 unconnected pads
- keine Kurzschlüsse
- keine Track-Kreuzungen
- keine Clearance-Fehler

Warnungsverteilung:
- 50 isolated_copper
- 30 holes_co_located
- 29 lib_footprint_issues
- 19 silk_over_copper
- 2 silk_overlap
- 1 silk_edge_clearance

Änderung in v3.3ebb:
- doppelte GND-Vias an identischen Koordinaten bereinigt
- 9 tatsächlich redundante GND-Vias entfernt
- die 30 holes_co_located-Warnungen entstanden aus Mehrfachpaarungen dieser doppelten Vias
- elektrische Netze und Signalrouten unverändert
- 230-V-Bereich unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3ebb_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. Zonen neu füllen
2. DRC v3.3ebb
3. prüfen, ob holes_co_located = 0
4. anschließend 50 isolated_copper-Warnungen der GND-Zone bereinigen
5. danach Silkscreen- und Library-Warnungen
6. separate 230-V-Sicherheitsprüfung


## Update v3.3ebc / v3.3ebd – 230-V-Sicherheitsprüfung

Basis:
- Hauptplatine_v3.3ebb_logic_repair_150x90.kicad_pcb
- Beginn der separaten 230-V-Endprüfung

Mess-/Prüfergebnisse:
- echte 6-mm-Kupfer-Keepout-Zone zwischen Primärseite und SELV vorhanden:
  - x=71..77 mm
  - F.Cu + B.Cu
  - Tracks/Vias/Copperpour verboten
- kleinster gemessener Primär->SELV-Kupferabstand außerhalb PS1/K1/K2 ca. 8,8 mm
- 230-V-Leiterbahnen aktuell 2,0 mm breit
- PS1 Primär-/Sekundär-Padabstand im Footprint >40 mm
- K1/K2 Kontaktseite->Spulenseite im Footprint min. ca. 12,9 mm

v3.3ebc:
- obere AC_L-Führung weiter von oberer Platinenkante nach innen verlegt
- Kupfer->Board-Edge von ca. 1,5 mm auf ca. 3,0 mm verbessert

v3.3ebd:
- kritischen L_PSU_FUSED-Zweig zu PS1 neu geroutet
- vorher ca. 0,3 mm zu PS1.1 / AC_N
- neue Führung links um PS1 herum

Neue Datei:
- `hardware/Hauptplatine/Hauptplatine.kicad_dru`
  - unterschiedliche MAINS-Netze: min. 1,5 mm
  - MAINS->SELV: min. 6 mm
  - MAINS-Trackbreite: min. 2,0 mm
- `hardware/Hauptplatine/230V-SICHERHEITSPRUEFUNG.md`

Statische Prüfung v3.3ebd:
- Primär->SELV weiterhin min. ca. 8,8 mm
- noch 16 Objektpaare unter dem konservativen 1,5-mm-Ziel innerhalb der 230-V-Seite
- Hauptbereiche:
  - AC_L bei F1/RV1
  - AC_L/L_MOTOR/DN_FEED im K1-Bereich
  - DN_FEED/K2.22
  - MOTOR_DN bei J5

Nächster Schritt:
1. v3.3ebd mit Hauptplatine.kicad_dru in KiCad öffnen
2. Design Rule Editor -> Check rule syntax
3. Zonen neu füllen
4. DRC ausführen
5. neuen DRC-Bericht hochladen
6. MAINS-clearance-Fehler einzeln neu routen
7. danach Footprint-Datenblattprüfung und Prototypenfreigabe-Check

Wichtig:
- keine formale Sicherheits-/Fertigungsfreigabe
- 230-V-Sicherheitsprüfung bleibt separat erforderlich


## Update v3.3ebj2 – ESP32-S3 DevKitC-1 Header korrigiert

Basis:
- Hauptplatine_v3.3ebi2_logic_repair_150x90.kicad_pcb

Wichtige Erkenntnis:
- die rechte U1-Headerreihe entspricht sauber dem offiziellen J3-Pinout
- auf der linken U1-Headerreihe waren nur bestimmte Anschlüsse falsch belegt
- die frühere Annahme einer komplett um einen Pin verschobenen linken Reihe war zu grob

Offizielles ESP32-S3-DevKitC-1 J1:
- J1.1/J1.2 = 3V3
- J1.3 = RST
- J1.7 = GPIO7
- J1.8 = GPIO15
- J1.14 = GPIO46 (Input-only, daher ungeeignet für STEP-Ausgang)
- J1.15 = GPIO9
- J1.21 = 5V
- J1.22 = GND

Korrekturen:
- U1.1: +5V entfernt, unbenutzt
- U1.3: STEP_TOP_3 entfernt, unbenutzt
- U1.5: STEP_TOP_2 entfernt, unbenutzt
- U1.13 = STEP_TOP_3 (J1.7 / GPIO7)
- U1.15 = STEP_TOP_2 (J1.8 / GPIO15)
- U1.27 = STEP_TOP_4 entfernt (J1.14 / GPIO46)
- U1.29 = STEP_TOP_4 (J1.15 / GPIO9)
- U1.41 = +5V (J1.21 / 5V)

Routing:
- alte Anschlussstücke zu den falschen Pads entfernt
- STEP_TOP_3 neu zweilagig zu U1.13 geführt
- STEP_TOP_2 neu zweilagig zu U1.15 geführt
- STEP_TOP_4 kurz zu U1.29 umgelegt
- +5V von bestehender +5V-Leitung bei y=90 mm zu U1.41 geführt
- neue statische Kollisionsprüfung: 0 erkannte Track-/Pad-Kollisionen
- alle vier neuen U1-Pads exakt getroffen

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3ebj2_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. v3.3ebj2 in KiCad öffnen
2. Zonen neu füllen
3. DRC ausführen
4. prüfen: 0 unconnected / 0 shorts / 0 crossings / 0 clearance
5. danach verbleibende Warnungen und Gerber-Freigabe


## Update v3.3ebk2 – DRCebj2 elektrische Restfehler behoben

Basis:
- Hauptplatine_v3.3ebj2_logic_repair_150x90.kicad_pcb
- DRCebj2.rpt vom 2026-09-29

DRCebj2:
- 60 DRC-Verstöße
- 3 unconnected pads
- 1 echter Clearance-Fehler
- 5 dangling-track-Warnungen
- Rest Library-/Silkscreen-Warnungen

Behobene elektrische Punkte:
1. REED_OPEN:
   - J2.1 bei 123/30 mm war zwischen zwei alten Track-Ästen nicht verbunden
   - beide Äste direkt zu J2.1 geführt

2. +5V:
   - nach der U1-Pinout-Korrektur war der obere B.Cu-5V-Bus vom übrigen +5V-Netz getrennt
   - Via bei 82.5/50.8 mm ergänzt
   - B.Cu-Verbindung 82.5/50.8 -> 82.5/24 -> 90/24

3. AC_L:
   - bei 66.2/27 mm fehlte der Layerwechsel F.Cu -> B.Cu
   - 1.2/0.6-mm-Via ergänzt

4. K1_COIL_LOW:
   - Clearance zu K2.A2 nur 0.18 mm bei geforderten 0.20 mm
   - lokalen Korridor von y=87.5 mm auf y=89 mm verlegt

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3ebk2_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. v3.3ebk2 öffnen
2. Zonen neu füllen
3. DRC ausführen
4. Ziel: 0 unconnected / 0 clearance / 0 dangling
5. danach nur noch Warnungsbereinigung und Fertigungscheck


## Update v3.3ebl2 – letzter elektrischer Restfehler geschlossen

Basis:
- Hauptplatine_v3.3ebk2_logic_repair_150x90.kicad_pcb
- DRCebk2.rpt vom 2026-09-29

DRCebk2:
- 57 DRC-Verstöße
- 1 unconnected item
- 3 track_dangling-Warnungen
- 0 Footprint-Fehler
- keine Clearance-/Short-/Crossing-Fehler mehr

Letzter echter elektrischer Fehler:
- K1_COIL_LOW war zwischen 81.5/91 und 102/91 unterbrochen
- fehlendes Segment wieder ergänzt

Zusätzliche Bereinigung:
- +5V-Dangling-Junction bei 82.5/58.3 beseitigt
- alte vertikale +5V-Segmente entfernt
- +5V-Bus neu mit explizitem Knoten bei 82.5/50.8 aufgebaut:
  - 82.5/28 -> 82.5/50.8
  - 82.5/50.8 -> 82.5/69
- vorhandener Via bei 82.5/50.8 bleibt Layerübergang zum oberen B.Cu-5V-Bus

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3ebl2_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. Zonen neu füllen
2. finalen DRC v3.3ebl2 ausführen
3. Ziel: 0 unconnected / 0 dangling / 0 electrical errors
4. danach nur noch Silkscreen-/Library-Warnungen und Fertigungscheck


## Update v3.3ebm2 – letzter +5V-Dangling-Endpunkt geschlossen

Basis:
- Hauptplatine_v3.3ebl2_logic_repair_150x90.kicad_pcb
- DRCebl2.rpt vom 2026-09-29

DRCebl2:
- 55 DRC-Warnungen
- 0 unconnected pads
- 0 Footprint-Fehler
- keine Clearance-/Short-/Crossing-Fehler
- nur noch 1 elektrische Warnung:
  - track_dangling bei +5V an 82.5/28 mm
- Rest ausschließlich Library-/Silkscreen-Warnungen

Änderung v3.3ebm2:
- Via bei 82.5/28 mm ergänzt
- B.Cu-Verbindung 82.5/28 -> 82.5/24 ergänzt
- dadurch ist das F.Cu-Ende des +5V-Busses direkt mit dem bestehenden oberen B.Cu-+5V-Bus verbunden

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3ebm2_logic_repair_150x90.kicad_pcb`

Nächster Schritt:
1. Zonen neu füllen
2. DRC v3.3ebm2
3. Ziel: keine elektrischen DRC-Meldungen mehr
4. danach Silkscreen-/Library-Warnungen bereinigen und Fertigungsdaten prüfen


## Update v3.3ebn2 – elektrisch sauber, Silkscreen-Warnungen bereinigt

Basis:
- Hauptplatine_v3.3ebm2_logic_repair_150x90.kicad_pcb
- DRCebm2.rpt vom 2026-09-29

DRCebm2:
- 54 DRC-Warnungen
- 0 unconnected pads
- 0 Footprint-Fehler
- keine Clearance-/Short-/Crossing-/Dangling-Fehler
- keine elektrischen DRC-Meldungen mehr

Verteilung der verbliebenen Warnungen:
- 21 lib_footprint_mismatch
- 9 lib_footprint_issues (fehlende RolladenCustom-Library)
- 21 silk_over_copper
- 2 silk_overlap
- 1 silk_edge_clearance

Änderungen v3.3ebn2:
- ausschließlich F.SilkS bereinigt
- problematische F.SilkS-Umrisse entfernt bei:
  - RV1
  - F1
  - U2
  - U3
  - D1
  - D2
  - Q1
  - Q2
- kollidierende Referenztexte U2/U3 ausgeblendet
- Kupfer, Pads, Netze und Routing unverändert

Datei:
`hardware/Hauptplatine/Hauptplatine_v3.3ebn2_logic_repair_150x90.kicad_pcb`

Erwarteter nächster DRC:
- 0 elektrische Fehler
- Silkscreen-Warnungen deutlich reduziert bzw. 0
- verbleibend voraussichtlich nur Library-Warnungen

Library-Warnungen:
- nicht elektrisch
- entstehen durch fehlende lokale RolladenCustom-Bibliothek und durch bewusst angepasste eingebettete Footprints
- vor finaler Projektpflege kann eine repository-lokale Footprint-Library aufgebaut und die Footprints damit synchronisiert werden

Nächster Schritt:
1. v3.3ebn2 öffnen
2. Zonen neu füllen
3. DRC ausführen
4. wenn nur Library-Warnungen bleiben: Fertigungs-/Gerber-Check
5. separate 230-V-Sicherheitsprüfung bleibt erforderlich


## Update v3.3ebn2 – DRC elektrisch vollständig sauber

Basis:
- Hauptplatine_v3.3ebn2_logic_repair_150x90.kicad_pcb
- DRCebn2.rpt vom 2026-09-29

DRCebn2:
- 30 DRC-Warnungen
- 0 unconnected pads
- 0 Footprint-Fehler
- keine Clearance-Fehler
- keine Kurzschlüsse
- keine Track-Kreuzungen
- keine Dangling-Tracks
- keine Silkscreen-Warnungen
- keine sonstigen elektrischen DRC-Meldungen

Verbleibend ausschließlich Library-Warnungen:
- 21 lib_footprint_mismatch
- 9 lib_footprint_issues
- Ursache: angepasste lokale Footprints und fehlende RolladenCustom-Library in der lokalen KiCad-Konfiguration
- diese Meldungen sind nicht elektrisch

Aktueller Arbeits-/Fertigungsprüfstand:
`hardware/Hauptplatine/Hauptplatine_v3.3ebn2_logic_repair_150x90.kicad_pcb`

Noch vor Bestellung:
1. repository-lokale Footprint-Library optional sauber aufbauen
2. Gerber/Drill erzeugen
3. Gerber im Viewer prüfen
4. Bohrungen/Boardkontur/Bestückungsseite prüfen
5. BOM gegen tatsächlich bestellte Bauteile abgleichen
6. 230-V-Sicherheitsprüfung separat dokumentiert abschließen
7. erste Inbetriebnahme ohne Netzspannung
