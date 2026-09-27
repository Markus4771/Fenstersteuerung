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

**Hauptplatine_v3.3l_logic_minimal_150x90.kicad_pcb**

Repository-Pfad:

`hardware/Hauptplatine/Hauptplatine_v3.3l_logic_minimal_150x90.kicad_pcb`

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
