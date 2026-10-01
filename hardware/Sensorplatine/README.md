# Sensorplatine

## Status

Konzeptstand **v0.1** – noch keine fertige Hardwareversion.

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
