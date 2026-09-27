# Fenstersteuerung

Modulare Fenster-/Rollladensteuerung auf Basis eines ESP32 mit abgesetzter Sensorik.

## Projektziel

Ziel ist ein kleines, günstiges Eigenmodul, das deutlich mehr kann als ein reiner Rollladenaktor.

Die Hauptelektronik sitzt im Rollladenkasten. Die Sensorik ist räumlich abgesetzt. Die Steuerung übernimmt neben der Rollladenfunktion auch Fensterzustand, Präsenz, Raumklima und die Verstellung/Ansteuerung der vorgesehenen Endlagen-/Hilfsmechanik.

Es ist **kein Fensterantrieb** vorgesehen.

## Festgelegte Architektur

### Hauptplatine

- ESP32-S3 als Hauptcontroller
- 2-lagige KiCad-Platine
- aktueller Platinenstand: ca. 150 x 90 mm
- räumliche Trennung von 230-V- und Kleinspannungs-/SELV-Bereich
- 2 x Finder 40.52 Relais
- 5-V-Spule
- DPDT
- Hardware-Interlock für die Rollladenrichtung
- 230-V-Ausgänge für den Rollladenmotor
- 2 x ULN2003-Ausgangsgruppen für TOP/BOTTOM
- 28BYJ-48 / ULN2003 für die geplante Endlagen-/Verstellmechanik
- RS485 zur Sensorplatine
- kabelgebundene Reed-Eingänge

### Sensorplatine

- ESP32-C3
- abgesetzt von der Hauptelektronik
- Verbindung zur Hauptplatine über RS485
- 4-adrige Verbindung:
  - +5 V
  - GND
  - RS485 A
  - RS485 B
- 3,3-V-kompatibler RS485-Transceiver vorgesehen, z. B. MAX3485
- LD2450 als Präsenzsensor fest eingeplant
- SHT40 für Temperatur/Luftfeuchtigkeit
- SCD41 optional für CO2
- MQ-2 optional

### Fensterkontakte

Zwei Reedkontakte werden per Kabel direkt von der Hauptelektronik herausgeführt:

- Fenster offen
- Fenster gekippt

Die Reedkontakte sind **nicht** Teil einer Funklösung.

## Stromversorgung

Festgelegt:

- Eingang: 230 V AC
- Netzteil: Mean Well IRM-20-5
- Ausgang: 5 V / 4 A
- 5-V-Bus für die Kleinspannungsversorgung
- separate 3,3-V-Regelung für die Logik
- Netzspannungs- und SELV-Bereich müssen sauber getrennt bleiben

## Kommunikation

### RS485

Die Sensorplatine wird per RS485 angebunden.

Leitungen:

- +5 V
- GND
- A
- B

Die RS485-Leitungen müssen im PCB sauber getrennt von den Motor-/Coil-Leitungen geführt werden.

## Aktueller KiCad-Stand

Aktuelle Hauptplatinen-Arbeitsversion:

**v3.3f**

Datei:

`hardware/Hauptplatine/Hauptplatine_v3.3f_logic_minimal_150x90.kicad_pcb`

Status:

- v3.3f basiert auf dem saubereren v3.3c-Stand
- v3.3d und v3.3e hatten durch automatisches Routing neue DRC-Konflikte erzeugt und werden nicht als Basis verwendet
- alte partielle RS485_A/B-Tracks wurden in v3.3f entfernt
- TOP_COIL_1–4 wurden neu geroutet
- vollständiges Routing ist noch nicht abgeschlossen
- v3.3f ist **keine Fertigungsversion**

## Noch offene Routing-Arbeiten

- TOP_COIL_1–4 per DRC prüfen
- BOT_COIL_1–4 routen
- STEP_TOP_1–4 routen
- STEP_BOT_1–4 routen
- ROLL_UP routen
- ROLL_DN routen
- K1_COIL_LOW vervollständigen
- K2_COIL_LOW vervollständigen
- RS485_A neu routen
- RS485_B neu routen
- REED_OPEN final prüfen
- REED_TILT final routen/prüfen
- +5-V-Verteilung finalisieren
- GND-Verbindungen finalisieren
- physische Konnektivität unabhängig vom DRC prüfen

## PCB-Zonen

Die derzeitige Aufteilung der Hauptplatine:

- links: 230-V-Bereich
- Mitte: ESP32-S3 und Finder-Relais
- rechts: 3,3/5-V-Logik, RS485 und ULN2003
- ganz rechts: TOP/BOTTOM-Anschlüsse

## Sicherheitsanforderungen

Vor Fertigung bzw. Inbetriebnahme muss der 230-V-Bereich gesondert geprüft werden:

- Luftstrecken
- Kriechstrecken
- Absicherung
- Leiterbahnabstände
- Trennung zu SELV
- Schutzmaßnahmen
- Relaiskontakt-/Motorpfade

Ein sauberer KiCad-DRC allein ist **keine** Freigabe für Netzspannung.

## Verzeichnisstruktur

- `hardware/Hauptplatine/` – KiCad-Dateien der Hauptplatine
- `hardware/Sensorplatine/` – Sensorplatine
- `DRC/` – Design-Rule-Check-Berichte
- `gerber/` – spätere Fertigungsdaten
- `bom/` – spätere Stücklisten
- `NEUER-CHAT.md` – Übergabestand
- `README.md` – technische Gesamtübersicht

## Empfohlene nächste Schritte

1. DRC für v3.3f auswerten
2. TOP_COIL sauber abschließen
3. BOT_COIL routen
4. STEP_TOP/BOT routen
5. Relaisansteuerung vervollständigen
6. RS485 routen
7. Reed-Eingänge finalisieren
8. 5 V und GND finalisieren
9. vollständige Konnektivitätsprüfung
10. finaler DRC
11. separate 230-V-Sicherheitsprüfung
12. Gerber/Drill/BOM erzeugen
13. Gerber visuell kontrollieren
14. erst dann Fertigung
