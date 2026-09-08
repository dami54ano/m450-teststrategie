# Übung 2 - Black-Box-Tests Autovermietung

Modul 450 | Damiano | 08.09.2026

Als Beispiel nehme ich [mietauto.ch](https://mietauto.ch). Bei einem Black-Box-Test schaue ich auf das Verhalten der Webseite aus Sicht des Benutzers. Den Code im Hintergrund muss ich dafür nicht kennen.

## Die fünf wichtigsten Testfälle

Die Tests sind geplant und wurden nicht ausgeführt. Die möglichen Ursachen sind deshalb nur Vermutungen für den Fall, dass ein Test fehlschlägt. Für die Buchung würde ich eine Testumgebung verwenden, damit keine echte kostenpflichtige Reservation entsteht.

| ID | Beschreibung | Eingabe / Aktion | Erwartetes Resultat | Effektives Resultat | Status | Mögliche Ursache bei einem Fehler |
|---|---|---|---|---|---|---|
| 1 | Fahrzeug suchen | Abhol- und Rückgabeort Zürich wählen, Abholung 12.10.2026 um 10:00, Rückgabe 15.10.2026 um 10:00; Suche starten. | Verfügbare Autos für den gewählten Ort und Zeitraum erscheinen. Falls nichts frei ist, wird das verständlich angezeigt. | Noch nicht getestet. | Offen | Suche oder Verfügbarkeitsabfrage funktioniert nicht richtig. |
| 2 | Fahrzeug auswählen | Ein verfügbares Auto aus der Suche auswählen. | Fahrzeugdaten, Mietbedingungen und Preis passen zum ausgewählten Angebot. Ort und Zeitraum bleiben erhalten. | Noch nicht getestet. | Offen | Falsche Fahrzeugdaten oder Suchdaten werden nicht übernommen. |
| 3 | Buchung durchführen | In einer Testumgebung ein verfügbares Auto wählen, gültige Testkundendaten eingeben und die Buchung abschliessen. | Eine Bestätigung mit Buchungsnummer erscheint. Fahrzeug, Zeitraum und Preis stimmen mit der Auswahl überein. | Noch nicht getestet; keine echte Buchung ausgelöst. | Offen | Fehler beim Speichern oder Bestätigen der Buchung. |
| 4 | Falsche Eingaben abfangen | Ein Pflichtfeld leer lassen und eine ungültige E-Mail-Adresse eingeben, danach weitergehen. | Die betroffenen Felder werden verständlich markiert. Ohne gültige Angaben geht die Buchung nicht weiter. | Noch nicht getestet. | Offen | Eingabeprüfung fehlt oder greift nicht. |
| 5 | Gesamtpreis prüfen | Ein Auto für drei Tage wählen und eine kostenpflichtige Option hinzufügen. Einzelpreise, Gebühren und allfällige Rabatte mit dem Gesamtpreis vergleichen. | Der Gesamtpreis entspricht den angezeigten Preisbestandteilen und Mietbedingungen. Er bleibt beim Wechsel zur Buchungsübersicht gleich. | Noch nicht getestet. | Offen | Mietdauer, Zusatzkosten oder Rabatte werden falsch verrechnet. |

Diese fünf Fälle decken den Weg von der Suche bis zur Buchung ab. Dazu kommen die Eingabeprüfung und der Preis, weil Fehler dort für den Kunden besonders ärgerlich wären.
