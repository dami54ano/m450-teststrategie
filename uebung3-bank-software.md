# Übung 3 - Bank-Software

Modul 450 | Damiano | 08.09.2026

## Durchführung

Die Bank-Software aus `bank-software-mvn.zip` wurde lokal unter Windows mit Java 25.0.2 gestartet. Maven konnte das unveränderte Projekt erfolgreich kompilieren und die benötigten Bibliotheken laden. Das Projekt selbst ist auf Java 20 eingestellt.

Die folgenden Tests wurden mit Unterstützung von Codex über das echte Konsolenmenü ausgeführt. Die Eingaben wurden automatisch an das laufende Java-Programm übergeben; die Ergebnisse stammen aus dessen Ausgaben. Es wurden keine Ersatzmethoden oder simulierten Resultate verwendet. Jeder Test startete das Programm neu, damit wieder dieselben fünf Beispielkonten vorhanden waren.

Wichtige Startwerte: Konto 1 = Rockefeller, 1500.00 USD; Konto 2 = Gates, 2000.00 EUR; Konto 3 = Musk, 23500.00 CHF; Konto 4 = Bezos, 100.50 EUR. Ein neu erstelltes Konto erhält die Nummer 6 und startet mit 0.00. Ein Startguthaben lässt sich im Menü nicht eingeben.

## Black-Box-Testfälle mit echten Ergebnissen

In der Spalte Eingabe bedeutet `→`, dass die Eingaben nacheinander mit Enter bestätigt werden. Jeder Test beginnt im Hauptmenü. `k` zeigt den Kontostand, `w` wechselt zurück zur Kontoauswahl, `q` beendet das Programm. BB12 endet durch den getesteten Absturz.

| ID | Beschreibung | Eingabe / Aktion | Erwartetes Resultat | Effektives Resultat | Status | Mögliche Ursache |
|---|---|---|---|---|---|---|
| BB01 | Konto auswählen und Programm beenden | `1 → q` | Konto 1 mit Namen und 1500.00 USD erscheint. Beenden funktioniert. | Rockefeller, Konto 1 und 1500.00 USD angezeigt; normal beendet. | Bestanden | - |
| BB02 | Unbekannte Kontonummer | `999 → q` | Meldung, dass das Konto nicht existiert; Programm läuft weiter. | «Ein Konto mit dieser Nummer ist nicht vorhanden!»; danach normal beendet. | Bestanden | - |
| BB03 | Geld einzahlen | `1 → e → 100 → k → q` | Guthaben steigt von 1500.00 auf 1600.00 USD. | 1600.00 USD angezeigt, auch bei der erneuten Abfrage mit `k`. | Bestanden | - |
| BB04 | Geld abheben | `1 → a → 100 → k → q` | Guthaben sinkt auf 1400.00 USD. | 1400.00 USD angezeigt. | Bestanden | - |
| BB05 | Mehr als das Guthaben abheben | `1 → a → 1501 → 100 → k → q` | 1501 wird abgelehnt, Guthaben bleibt zunächst 1500.00 USD. Die anschliessende gültige Abhebung von 100 ergibt 1400.00 USD. | Meldung «Kontostand zu niedrig (momentan 1500.0 USD)»; nach der Korrektur 1400.00 USD. | Bestanden | - |
| BB06 | Zwischen zwei EUR-Konten überweisen | `2 → ü → 4 → 100 → w → 4 → k → q` | Konto 2 hat danach 1900.00 EUR, Konto 4 hat 200.50 EUR. | Beide Kontostände stimmen mit der Erwartung überein. | Bestanden | - |
| BB07 | Konto erstellen | `e → Testkonto → CHF → 6 → k → q` | Neues Konto 6 mit Namen Testkonto, Währung CHF und Guthaben 0.00. | Konto wurde erstellt und konnte ausgewählt werden; 0.00 CHF. | Bestanden | - |
| BB08 | Neu erstelltes Testkonto löschen | `e → Testkonto → CHF → 6 → l → j → 6 → a → q` | Nach Bestätigung ist Konto 6 nicht mehr auswählbar und fehlt in der Liste. | Löschung bestätigt; Auswahl von 6 ergibt «nicht vorhanden»; Liste enthält nur Konten 1 bis 5. | Bestanden | - |
| BB09 | Überweisung auf eigenes Konto | `1 → ü → 1 → k → q` | Überweisung wird abgelehnt; 1500.00 USD bleiben erhalten. | Aufforderung, ein anderes Konto zu wählen; Guthaben bleibt 1500.00 USD. | Bestanden | - |
| BB10 | Negativen Betrag einzahlen | `1 → e → -100 → k → q` | Betrag wird abgelehnt; Guthaben bleibt 1500.00 USD. | Guthaben sinkt auf 1400.00 USD, ohne Fehlermeldung. | Fehler | `deposit()` addiert auch negative Beträge. |
| BB11 | Negativen Betrag abheben | `1 → a → -100 → k → q` | Betrag wird abgelehnt; Guthaben bleibt 1500.00 USD. | Guthaben steigt auf 1600.00 USD, ohne Fehlermeldung. | Fehler | `withdraw()` zieht einen negativen Betrag ab. |
| BB12 | Leere Aktion eingeben | `1 → [nur Enter]` | Verständliche Meldung; das Programm läuft weiter. | Absturz mit `StringIndexOutOfBoundsException` in `Counter.editAccount()`; Exit-Code 1. | Fehler | `substring(0,1)` wird auf einen leeren Text angewendet. |
| BB13 | Genau das ganze Guthaben abheben | `1 → a → 1500 → k → q` | Abhebung ist möglich; Restguthaben 0.00 USD. | 0.00 USD angezeigt. | Bestanden | - |
| BB14 | Buchstaben statt Geldbetrag | `1 → e → abc → 100 → k → q` | Fehlermeldung für `abc`; danach ist eine gültige Eingabe möglich. | «Ungültige Eingabe, bitte nochmals!»; nach 100 Einzahlung 1600.00 USD. | Bestanden | - |
| BB15 | EUR nach CHF überweisen | `2 → ü → 3 → 100 → w → 3 → k → q` | Umrechnung mit einem gültigen Kurs oder Ablehnung ohne Kontostandsänderung, wenn kein Kurs verfügbar ist. | «Es wurde keine Umrechnung vorgenommen.» Trotzdem werden 100 EUR abgezogen und 100 CHF gutgeschrieben: 1900.00 EUR und 23600.00 CHF. | Fehler | Für EUR → CHF fehlt ein Umrechnungszweig; der Betrag wird unverändert zurückgegeben. |

**Ergebnis: 15 Tests ausgeführt, davon 11 bestanden und 4 mit Fehlern.** Bei BB15 wurde kein genauer Wechselkurs vorausgesetzt. Der Fehler ist, dass trotz ausdrücklich fehlender Umrechnung gebucht wird.

Der externe Wechselkursabruf über `w` im Hauptmenü wurde nicht getestet. Dafür wird ein externer Dienst mit API-Schlüssel verwendet. Die getesteten Überweisungen laufen über die lokale Umrechnung. Nullbeträge, sehr grosse Beträge und weitere Währungspaare wären zusätzliche Testfälle.

Die vollständigen Eingaben, Konsolenausgaben und Exit-Codes stehen in [bank-testprotokoll.txt](bank-testprotokoll.txt). Diese Markdown-Datei ergänzt den bisherigen PDF-Stand, in dem die Tests noch offen waren.

## Mögliche White-Box-Tests

Diese Tabelle beschreibt Vorschläge anhand des Codes. Es wurden dafür keine separaten JUnit-Tests ausgeführt.

| Methode | Was ich testen würde | Erwartung / abgedeckter Ablauf |
|---|---|---|
| `Account.deposit(double amount)` | Bei 100 CHF zuerst 50 einzahlen; zusätzlich negative Beträge prüfen. | Gültiger Fall: 150 CHF. Negative Beträge sollten nach einer Verbesserung abgelehnt werden. |
| `Account.withdraw(double amount)` | Bei 100 CHF separat 50, 100 und 101 abheben. | Die beiden Pfade werden geprüft: `true` mit 50 bzw. 0 Rest; `false` mit unveränderten 100 bei zu hohem Betrag. |
| `Account.getBalance()` | Nach einer Einzahlung und einer Abhebung den Wert lesen. | Der Rückgabewert entspricht dem berechneten Guthaben. |
| `Bank.createAccount(...)` und `getNumberOfAccounts()` | Anzahl vor und nach dem Erstellen vergleichen; Name, Währung und Startwert prüfen. | Ein zusätzliches Konto mit den übergebenen Daten. |
| `Bank.getAccount(int nr)` | Konto am Anfang und Ende der Liste, unbekannte Nummer und leere Bank prüfen. | Richtiges Konto bei einem Treffer; sonst `null`. Damit werden Treffer und Durchlaufen ohne Treffer geprüft. |
| `Bank.deleteAccount(Account a)` | Ein Konto erstellen, löschen und nochmals suchen. | Anzahl sinkt um eins; gelöschte Nummer liefert `null`. |
| `Counter.transferAmount(...)` | Genug und zu wenig Guthaben sowie gleiche und unterschiedliche Währungen prüfen. | Abbuchung und Gutschrift passen zusammen; bei zu wenig Guthaben wird erneut gefragt. |
| `Counter.convertCurrency(...)` | Je 100 für USD → CHF, USD → EUR und CHF → USD; zusätzlich EUR → CHF. | Aktueller Code: 111, 91 bzw. 90; beim nicht unterstützten Paar wird 100 zurückgegeben. Der letzte Fall zeigt eine Lücke. |

`convertCurrency()` ist privat. Ich würde sie über die Überweisung mitprüfen oder die Umrechnung in eine eigene Klasse auslagern. Dann wären gezielte Unit-Tests einfacher. Die genannten Kurse sind feste Werte aus dem Übungscode und keine aktuellen Marktkurse.

## Was ich am Code verbessern würde

- **Beträge prüfen:** Negative Beträge, `NaN` und unendliche Werte abfangen. BB10 und BB11 zeigen, warum das nötig ist. Ob 0 erlaubt sein soll, sollte klar festgelegt werden.
- **Leere Eingaben abfangen:** Vor `substring(0,1)` prüfen, ob überhaupt etwas eingegeben wurde. Das würde den Absturz aus BB12 verhindern.
- **Währungswechsel vollständig behandeln:** Für jedes unterstützte Paar einen gültigen Kurs verwenden. Ohne Kurs darf keine Überweisung gebucht werden. Abbuchung und Gutschrift sollten gemeinsam erfolgreich sein oder beide ausbleiben.
- **Geldbeträge sauber speichern:** Statt `double` würde ich `BigDecimal` mit einer klaren Rundungsregel verwenden.
- **API-Schlüssel aus dem Code nehmen:** Den Schlüssel über eine Umgebungsvariable laden. Bei der API-Antwort auch HTTP-Status und Fehlermeldungen prüfen und die Antwort danach schliessen.
- **Counter aufteilen:** Eingaben, Ausgaben und Banklogik trennen. Kleine Methoden wären leichter verständlich und testbar.
- **Automatisierte Tests ergänzen:** Vor allem für Guthaben, Kontoverwaltung und die gefundenen Fehler wären JUnit-Tests sinnvoll.

Die normalen Abläufe funktionieren in den getesteten Fällen. Bei falschen Eingaben und beim Währungswechsel gibt es aber klare Fehler, die vor einem echten Einsatz behoben werden müssten.

## Tests nochmals ausführen

Im entpackten Maven-Projekt lassen sich die Abhängigkeiten und der Start unter PowerShell so vorbereiten:

```powershell
mvn compile dependency:build-classpath "-Dmdep.outputFile=classpath.txt"
$bankClasspath = "target/classes;" + (Get-Content classpath.txt -Raw).Trim()
java -Dfile.encoding=UTF-8 -cp $bankClasspath ch.tbz.bank.software.Main
```

Danach die Eingaben aus einer Tabellenzeile nacheinander eintippen. Für den nächsten Test das Programm neu starten. Bei der Durchführung wurden Java-Ein- und Ausgaben auf UTF-8 gesetzt, damit auch die Menüaktion `ü` korrekt ankommt.
