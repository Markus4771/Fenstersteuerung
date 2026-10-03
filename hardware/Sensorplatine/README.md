# Sensorplatine

## Status

Aktueller Entwicklungsstand: **v4.0.ac**. Diese Version ist der aktuelle manuell platzierte KiCad-10-Basisstand ohne Leiterbahnen und Vias.

Die Sensorplatine ist als abgesetzte Raumklima- und Anwesenheitssensorplatine für die Fenstersteuerung vorgesehen. Die Reedkontakte für Fenster offen/gekippt bleiben direkt an der Hauptplatine und gehören ausdrücklich **nicht** auf diese Sensorplatine.

## Geplante Messgrößen und Bausteine

| Messgröße | Vorgesehener Baustein | Schnittstelle | Hinweis |
|---|---|---|---|
| Temperatur | BME280 | I²C | gemeinsamer Sensor mit Feuchte und Luftdruck |
| Luftfeuchtigkeit | BME280 | I²C | ±3 % rF typischer Einsatzbereich laut Hersteller |
| Luftdruck | BME280 | I²C | 300…1100 hPa |
| CO₂ | Sensirion SCD41 | I²C | echter CO₂-Sensor, spezifizierter Bereich 400…5000 ppm |
| VOC / Luftqualität | Sensirion SGP40 | I²C | VOC-Index für Lüftungs-/Luftgütelogik |
| Helligkeit | Vishay VEML7700 | I²C | digitaler Umgebungslichtsensor |
| Bewegung / Anwesenheit | modular | GPIO / UART | PIR oder mmWave-Modul; endgültiger Typ noch festzulegen |
| Schallpegel | digitales MEMS-Mikrofon | I²S | z. B. INMP441-Klasse; relative Geräuschpegelmessung, keine normgerechte Schallpegelmessung |

## Architektur

Ziel ist, möglichst viele Sensoren über **I²C** anzubinden. Dadurch benötigen BME280, SCD41, SGP40 und VEML7700 gemeinsam nur SDA und SCL.

Zusätzlich werden benötigt:

- I²S-Leitungen für das digitale MEMS-Mikrofon
- mindestens ein GPIO für einen PIR-Sensor
- optional UART für ein mmWave-Anwesenheitsmodul
- Versorgung 3,3 V und ggf. 5 V
- GND

## Bewegung / Anwesenheit

Die Platine soll so vorbereitet werden, dass entweder:

1. ein einfacher PIR-Bewegungssensor verwendet werden kann, oder
2. ein mmWave-Anwesenheitssensor über einen Steckverbinder angeschlossen werden kann.

mmWave ist besonders interessant, weil damit auch ruhende Personen erkannt werden können. Wegen Baugröße und unterschiedlicher Modulvarianten soll der mmWave-Sensor zunächst nicht fest als SMD-Bauteil auf die Platine gesetzt werden.

## Schallpegel

Die Schallmessung soll mit einem digitalen MEMS-Mikrofon erfolgen.

Ziel:

- relativer Geräuschpegel
- Erkennung von ungewöhnlich hoher Lautstärke
- Verlauf über die Zeit
- mögliche spätere Nutzung für Anwesenheits-/Komfortlogik

Die Platine ist **nicht** als kalibriertes oder normgerechtes Schallpegelmessgerät vorgesehen.

## Mechanik / Layout

Beim späteren PCB-Layout beachten:

- BME280 möglichst weit von Spannungsreglern, ESP32 und anderen Wärmequellen platzieren
- SCD41 mit freiem Luftaustausch montieren
- SGP40 ebenfalls mit gutem Luftaustausch platzieren
- VEML7700 an einer Gehäuseöffnung bzw. lichtdurchlässigen Stelle platzieren
- Mikrofon mit definierter Schallöffnung im Gehäuse
- Sensorbereiche nicht unnötig durch Kupferflächen oder Wärmequellen aufheizen
- Befestigungsbohrungen vorsehen
- Beschriftung der Steckverbinder auf dem Silkscreen

## Noch festzulegen

1. Verbindung zwischen Hauptplatine und Sensorplatine
2. Kabellänge zwischen beiden Platinen
3. Versorgungsspannung(en) der Sensorplatine
4. PIR- und/oder mmWave-Modultyp
5. endgültiger MEMS-Mikrofontyp
6. Gehäuseabmessungen und Position der Luft-/Licht-/Schallöffnungen
7. Schutzbeschaltung für die Verbindung zur Hauptplatine
8. KiCad-Schaltplan und PCB-Version v1.0


## Schaltplanstand v1.0

Aktiver Entwurf:

- `Sensorplatine_v1.0.sch`
- lokale Symbolbibliothek: `Sensorplatine_v1.0-cache.lib`
- Pinbelegung: `PINBELEGUNG_v1.0.csv`
- Schaltplan-Spezifikation: `SCHALTPLAN_v1.0.md`

Der Schaltplan enthält bereits:

- STM32G031K8T6 (LQFP-32)
- AP2112K-3.3
- MAX3485 / RS485
- BME280
- SCD41
- SGP40
- VEML7700
- HLK-LD2450
- I2S-MEMS-Mikrofon als noch zu finalisierender Typ
- 4-poligen Anschluss zur Hauptplatine
- SWD-Programmieranschluss

### Vor PCB-Layout noch final festzulegen

1. konkreter I2S-MEMS-Mikrofontyp und Footprint
2. RS485-TVS-Typ
3. 120-Ohm-Abschluss als Jumper/Lötbrücke
4. endgültiger 4-poliger Steckverbinder
5. Footprint des HLK-LD2450 und Antennen-Keepout
6. mechanische Positionen innerhalb 50 x 50 mm


## PCB-Placement v1.0

Erster mechanischer 50 x 50 mm Placement-Entwurf:

`Sensorplatine_v1.0_placement_50x50.kicad_pcb`

Aktuelle Platzierung:

- HLK-LD2450 entlang der oberen Platinenkante
- definierter Antennen-Keepout an der Oberkante
- SCD41 links mit eigener Luftzone
- BME280 und SGP40 mittig im Sensorbereich
- VEML7700 am rechten Rand
- STM32G031K8T6 zentral unten
- MAX3485 und SM712 nahe RS485-Seite
- AP2112K-3.3 unten rechts
- T5848 nahe Gehäuserand für Schallöffnung
- JST-XH 4-polig unten links
- SWD-Header unten
- zunächst zwei 2.5-mm-Befestigungsbohrungen unten

Dieser Stand ist absichtlich noch nicht geroutet. Vor dem Routing werden Footprints und mechanische Abstände final geprüft.


## Routing-Stand v1.0

Neue Datei:

`Sensorplatine_v1.0_routing_draft_50x50.kicad_pcb`

Bereits elektrisch geroutet:

- 5-V-Eingang vom JST-XH-Stecker zum AP2112K
- 3,3-V-Versorgung vom AP2112K zum STM32 und MAX3485
- GND-Verbindungen im Kernbereich
- RS485 A/B vom Hauptanschluss über SM712 zum MAX3485
- USART-Verbindungen MAX3485 <-> STM32
- DE/RE-Steuerung MAX3485 <-> STM32

Die Sensor-Footprints sind in diesem Stand noch bewusst als Platzhalter markiert. Vor dem Routing von I2C, I2S und LD2450 werden die exakten Hersteller-Landpatterns übernommen und geprüft.


## Maximaler Routing-Stand – v1.0

In `Sensorplatine_v1.0_routing_draft_50x50.kicad_pcb` sind jetzt zusätzlich geroutet:

- I2C SDA/SCL vom STM32 bis in die zentrale Sensorzone
- UART TX/RX vom STM32 bis zur LD2450-Modulschnittstelle
- 5 V und GND bis zur LD2450-Modulschnittstelle
- I2S WS / CK / SD vom STM32 bis zur Mikrofonzone
- SWDIO / SWCLK / NRST / 3V3 / GND bis zum SWD-Header

Damit sind praktisch alle langen bzw. kritischen Leiterbahnstrecken vorgeroutet.

Noch offen bleiben nur die kurzen lokalen Verbindungen direkt an:

- BME280
- SCD41
- SGP40
- VEML7700
- T5848
- endgültiges LD2450-Landpattern

Grund: Für diese Bauteile werden vor einer fertigungstauglichen Version die exakten Hersteller-Landpatterns übernommen. Die derzeitigen Sensorflächen sind ausdrücklich noch Platzhalter.

Der aktuelle Stand ist daher ein **maximal gerouteter mechanisch-elektrischer Entwurf**, aber noch keine freigegebene Fertigungsdatei. Vor Gerber-Erzeugung müssen die finalen Footprints eingesetzt und ein KiCad-DRC durchgeführt werden.


## Versionierung

Ab jetzt wird die Sensorplatine strikt versionsbasiert weiterentwickelt.

Regeln:

- vorhandene Hardwareversionen werden nicht mehr überschrieben
- jede elektrische oder mechanische Änderung erzeugt eine neue Version
- reine Dokumentationskorrekturen können innerhalb derselben Version dokumentiert werden
- DRC-Berichte werden passend zur jeweiligen Version abgelegt
- Gerber-Dateien erhalten immer dieselbe Versionsnummer wie das zugehörige PCB

Aktueller Stand:

- **v1.0** = erster 50 x 50 mm Sensorplatinen-Entwurf mit STM32G031, RS485, LD2450 und vorgerouteten Sensorbussen
- nächste Entwicklungsstufe: **v1.1**

Geplante Dateinamen:

- `Sensorplatine_v1.0_routing_draft_50x50.kicad_pcb` – eingefrorener aktueller Stand
- `Sensorplatine_v1.1_50x50.kicad_pcb` – nächste bearbeitete PCB-Version
- `DRC_v1.0.rpt`, `DRC_v1.1.rpt` usw.
- spätere Gerber: `Sensorplatine_v1.1_Gerber.zip` usw.


## Version v1.2

Datei:

`Sensorplatine_v1.2_50x50.kicad_pcb`

Änderungen gegenüber v1.1:

- 5-V-Versorgung neu geroutet
- GND-Backbone neu aufgebaut
- 3,3-V-Verteilung neu geroutet
- RS485 A/B getrennt geführt
- MAX3485 <-> STM32 neu geroutet
- I2C bis zur Sensorzone geroutet
- LD2450 UART sowie 5 V/GND bis zur Modulschnittstelle geroutet
- I2S bis zur Mikrofonzone geroutet
- SWD neu geroutet
- SWD-Header weiter in die Platinenkontur verschoben

Hinweis:
v1.2 basiert auf der in GitHub gespeicherten v1.1. Lokal in KiCad verschobene Bauteile aus einer nicht hochgeladenen `.kicad_pcb` konnten noch nicht übernommen werden.


## Version 4.0

### v4.0.aa – manuelle Neuplatzierung

Datei:

`Sensorplatine_v4.0.aa_placement_only_50x50.kicad_pcb`

- neue manuelle Bauteilplatzierung
- KiCad-10-Dateiformat
- keine Leiterbahnen
- keine Vias
- dient als saubere mechanische Ausgangsbasis

### v4.0.ab – Routingversuch

Datei:

`Sensorplatine_v4.0.ab_50x50.kicad_pcb`

DRC:

`DRC_v4.0.ab.rpt`

Dieser Stand war ein maximaler 2-Lagen-Routingversuch. Der DRC zeigte zu viele geometrische Konflikte und Kurzschlüsse. **v4.0.ab ist deshalb ausdrücklich nicht für Fertigung oder weitere Entwicklung freigegeben.**

### v4.0.ac – aktueller Basisstand

Datei:

`Sensorplatine_v4.0.ac_50x50.kicad_pcb`

- basiert auf der aktuellen manuellen Bauteilplatzierung
- KiCad 10
- aktuell 0 Leiterbahnsegmente
- aktuell 0 Vias
- neuer Ausgangspunkt für das nächste kontrollierte Routing
- Bauteilpositionen sollen vor dem nächsten Routing nicht automatisch verändert werden

**Aktueller Arbeitsstand: v4.0.ae**


### v4.0.ad – konservatives Kernrouting

Datei:

`Sensorplatine_v4.0.ad_50x50.kicad_pcb`

- basiert auf v4.0.ac
- manuelle Bauteilplatzierung unverändert
- +5 V und GND im Kernbereich geroutet
- RS485_A und RS485_B geroutet
- D1 und U7 im RS485-Pfad berücksichtigt
- +3V3 von U9 zu U7 geroutet
- enge Sensor-, I2C-, I2S- und SWD-Bereiche noch bewusst offen
- vor Weiterentwicklung ist ein neuer KiCad-DRC erforderlich

**Aktueller Arbeitsstand: v4.0.ad**


### v4.0.ae – RS485-only routing

Datei:

`Sensorplatine_v4.0.ae_50x50.kicad_pcb`

- basiert auf der reinen v4.0.aa-Manuellplatzierung
- nur RS485_A und RS485_B geroutet
- D1 und U7 angebunden
- Versorgung, I2C, I2S und SWD bleiben bewusst offen
- Ziel: zuerst einen DRC-sauberen RS485-Kern herstellen
