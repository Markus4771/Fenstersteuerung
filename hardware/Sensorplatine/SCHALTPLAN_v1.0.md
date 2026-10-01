# Sensorplatine v1.0 – Schaltplan-Spezifikation

## Ziel

50 × 50 mm Sensorplatine für die Fenstersteuerung mit eigenem STM32G031K8T6 und RS485-Anbindung an die Hauptplatine.

Die Reedkontakte bleiben an der Hauptplatine.

## Funktionsblöcke

### 1. Versorgung

Eingang von der Hauptplatine:

- +5V
- GND
- RS485_A
- RS485_B

Versorgung:

- +5V direkt für HLK-LD2450
- +5V auf 3V3 über Regler
- 3V3 für STM32G031K8T6, BME280, SCD41, SGP40, VEML7700, MAX3485 und digitales MEMS-Mikrofon

Vorgesehener Regler:

- AP2112K-3.3, SOT-23-5, 600 mA

Abblockung:

- 10 µF am 5-V-Eingang
- 10 µF am 3V3-Ausgang des Reglers
- 100 nF direkt an jedem IC
- zusätzlich 4.7 µF nahe SCD41 / SGP40

### 2. STM32

U1: STM32G031K8T6

Gehäuse:

- LQFP-32
- 7 × 7 mm
- gut handlötbar

Versorgung:

- VDD/VDDA -> 3V3
- VSS/VSSA -> GND
- 100 nF + 1 µF nahe U1

Reset:

- PF2/NRST
- 10 kΩ Pull-up nach 3V3
- Test-/Programmierzugang über SWD

SWD:

- 3V3
- GND
- SWDIO
- SWCLK
- NRST

### 3. I2C-Sensorbus

Bus:

- PB8 = I2C1_SCL
- PB9 = I2C1_SDA

Pull-ups:

- 4.7 kΩ nach 3V3 auf SDA und SCL

Teilnehmer:

#### U2 – BME280

Messgrößen:

- Temperatur
- Luftfeuchtigkeit
- Luftdruck

Anbindung:

- I2C
- VDD = 3V3
- VDDIO = 3V3
- CSB = 3V3
- SDO = GND für Standard-I2C-Adresse
- 100 nF nahe Sensor

#### U3 – SCD41

Messgröße:

- CO2

Anbindung:

- I2C
- Versorgung 3V3
- lokale Abblockung 100 nF + 4.7 µF

Mechanik:

- freie Luftzufuhr vorsehen
- nicht direkt an Wärmequellen platzieren

#### U4 – SGP40

Messgröße:

- VOC / Luftgüte

Anbindung:

- I2C
- VDD und VDDH gemeinsam an 3V3
- VSS und Center-Pad an GND
- 100 nF + lokale Pufferung

#### U5 – VEML7700

Messgröße:

- Umgebungshelligkeit

Anbindung:

- I2C
- VDD = 3V3
- GND = GND
- 100 nF nahe Sensor

Mechanik:

- am Platinenrand / Gehäusefenster platzieren

### 4. mmWave-Anwesenheit

U6/M1: HLK-LD2450

Versorgung:

- 5V direkt
- GND

UART:

- PA2 = USART2_TX -> RX des LD2450
- PA3 = USART2_RX <- TX des LD2450

Mechanik:

- Modul direkt auf Sensorplatine
- Antennenbereich an Platinenkante
- Keepout vor Antenne
- keine Kupferfläche oder große Bauteile vor der Antenne

### 5. RS485 zur Hauptplatine

U7: MAX3485, SOIC-8

Versorgung:

- VCC = 3V3
- GND = GND
- 100 nF direkt an VCC

UART:

- PB6 = USART1_TX -> DI
- PB7 = USART1_RX <- RO
- PB1 = RS485_DE

Enable:

- DE und /RE gemeinsam an PB1
- 10 kΩ Pull-down auf PB1, damit der Treiber beim Reset deaktiviert bleibt

Bus:

- A -> RS485_A
- B -> RS485_B

Abschluss:

- 120 Ω zwischen A und B
- per 2-poligem Jumper oder Lötbrücke zuschaltbar

Schutz:

- TVS für RS485 A/B vorsehen
- optional Serienwiderstände / Common-Mode-Choke als Bestückungsoption

### 6. Digitales Mikrofon

U8: digitales MEMS-Mikrofon, endgültiger Typ noch festzulegen.

Ziel:

- relativer Schallpegel
- keine normgerechte dB-Messung

I2S:

- PA4 = WS/LRCLK
- PA5 = CK/BCLK
- PA7 = SD

Versorgung:

- 3V3
- GND
- 100 nF nahe Mikrofon

Mechanik:

- Mikrofon an Platinenrand
- definierte Schallöffnung im Gehäuse

### 7. Hauptanschluss

J1: 4-poliger Anschluss Hauptplatine <-> Sensorplatine

1. +5V
2. GND
3. RS485_A
4. RS485_B

Steckertyp wird erst mit Gehäuse und Kabelwahl final festgelegt.

## STM32-Pinplanung

| Pinname | phys. Pin | Funktion |
|---|---:|---|
| PB9 | 1 | I2C SDA |
| PB8 | 28 | I2C SCL |
| PA2 | 9 | LD2450 TX |
| PA3 | 10 | LD2450 RX |
| PA4 | 11 | I2S WS |
| PA5 | 12 | I2S CK |
| PA7 | 14 | I2S SD |
| PB1 | 16 | RS485 DE + /RE |
| PB6 | 32 | RS485 TX |
| PB7 | 27 | RS485 RX |
| PA13 | 24 | SWDIO |
| PA14 | 25 | SWCLK |
| PF2/NRST | 6 | Reset |

Die physikalischen Pin-Nummern wurden gegen das aktuelle ST-Datenblatt für STM32G031KxT im LQFP-32 geprüft. Die Alternate-Functions für USART1, USART2 und I2S1 sind ebenfalls abgeglichen.

## Layoutvorgaben 50 × 50 mm

- LD2450 entlang einer Außenkante
- SCD41 und BME280 möglichst weit weg von Regler, STM32 und LD2450-Wärmequellen
- VEML7700 an einer lichtzugänglichen Außenkante
- MEMS-Mikrofon an einer Gehäuseöffnung
- RS485- und 5-V-Anschluss an einer gemeinsamen Kabelseite
- 4 Befestigungsbohrungen, soweit der verfügbare Raum dies zulässt
- GND-Fläche, aber Antennen-Keepout des LD2450 beachten
- Testpunkte für 5V, 3V3, GND, SDA, SCL, RS485_A, RS485_B, TX/RX

## Noch offen

1. endgültiger MEMS-Mikrofontyp
2. endgültiger 4-poliger Steckverbinder
3. genaue TVS-Diode für RS485
4. endgültiger Footprint des HLK-LD2450
5. Gehäuse und Öffnungen
6. Überführung dieser Spezifikation in KiCad-Schaltplan und PCB
