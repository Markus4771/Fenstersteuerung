# Fenstersteuerung

Entwicklung einer modularen Fenster-/Rollladensteuerung auf Basis eines ESP32 mit abgesetzter Sensorik.

## Aktueller Hardwarestand

Aktuelle Hauptplatinen-Arbeitsversion: **v3.3f**

Status:
- 150 x 90 mm Hauptplatine
- ESP32-Steuerung
- Finder-Relais 40.52 für Rollladensteuerung
- RS485 vorgesehen
- Reedkontakte für Fensterzustände
- zwei ULN2003-Ausgangsgruppen für TOP/BOTTOM
- 230-V- und Kleinspannungsbereich räumlich getrennt geplant
- Routing noch nicht vollständig abgeschlossen
- v3.3f ist **keine Fertigungsversion**

## Verzeichnisstruktur

- `hardware/Hauptplatine/` – KiCad-Dateien der Hauptplatine
- `hardware/Sensorplatine/` – spätere Sensorplatine
- `DRC/` – Design-Rule-Check-Berichte
- `gerber/` – spätere Fertigungsdaten
- `bom/` – spätere Stücklisten
- `NEUER-CHAT.md` – Übergabestand für die Weiterentwicklung

## Nächste Schritte

1. TOP_COIL-Routing in v3.3f per DRC prüfen
2. BOT_COIL_1–4 routen
3. STEP_TOP_1–4 und STEP_BOT_1–4 routen
4. RS485_A/B neu routen
5. REED_OPEN / REED_TILT fertigstellen
6. Relaisansteuerung ROLL_UP / ROLL_DN vervollständigen
7. Versorgung und GND finalisieren
8. vollständigen DRC durchführen
9. 230-V-Sicherheitsabstände separat prüfen
10. erst danach Gerber-/Bohrdaten erzeugen

## Sicherheit

Der 230-V-Bereich ist vor einer Fertigung bzw. Inbetriebnahme gesondert auf Luft- und Kriechstrecken, Absicherung, Leiterbahnabstände und Schutzmaßnahmen zu prüfen.
