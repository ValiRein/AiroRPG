# AiroRPG – Projektdokumentation

> Zweck: Diese Datei hält den verbindlichen Projektstand fest und dient dazu, die Arbeit in einer neuen ChatGPT-Unterhaltung ohne Verlust wichtiger Entscheidungen fortzusetzen.

## 1. Projektziel

Entwicklung eines modularen, textbasierten Mehrspieler-Rollenspiels als IRC-Spielbot. Anope und UnrealIRCd sind bereits eingerichtet; deren Installation und Netzwerkkonfiguration sind nicht Teil dieses Projekts.

Das Spiel wird über IRC-Befehle und stimmungsvolle Bot-Nachrichten bedient. Spielwelt, Atmosphäre und genaue Spielmechaniken werden schrittweise gemeinsam festgelegt.

## 2. Sprache und technische Umgebung

- Skriptsprache: mIRC Scripting Language (MSL, auch MRC genannt).
- Netzwerk: bestehendes IRC-Netz mit Anope und UnrealIRCd.
- Versionsverwaltung: GitHub, Repository ValiRein/AiroRPG.
- Sprache des Projekts: Deutsch.
- Sämtliche Dokumentationen, Kommentare im Quelltext, Beschreibungen, Hinweise für Spielende, Meldungen, Bezeichnungen in der Projektverwaltung und Versionsmeldungen werden auf Deutsch verfasst. Englische Fachbegriffe werden vermieden, soweit sie nicht technisch vorgegeben sind, etwa durch die Skriptsprache oder externe Schnittstellen.

## 3. Mehrspieler-Konzept

- Das Spiel ist von Anfang an als Mehrspieler-Spiel geplant und nicht als Einzelspieler-Spiel.
- Mehrere Personen sollen gleichzeitig spielen können.
- Kämpfe und andere Spielsituationen sollen voneinander getrennt sein können; die genaue technische Umsetzung ist noch offen.
- Denkbare spätere Möglichkeiten sind persönliche Kampfbereiche, eigene Kanäle oder Gruppen, zu denen weitere Personen eingeladen werden können. Noch keine dieser Varianten ist beschlossen.
- Die Architektur muss von Beginn an mehrere gleichzeitig aktive Spielende und voneinander unabhängige Spielinstanzen berücksichtigen.

## 4. Geplante Spielbereiche

Bisher genannte Ideen; Umfang und Regeln sind noch nicht abschließend festgelegt:
- Charaktere und Rollenspiel
- Aufgaben und Quests
- Verliese und Monster
- Herstellung von Gegenständen
- Waffenwahl und Ausrüstung
- Integrierter Laden
- Ereignisse
- Eigenes rundenbasiertes Kampfsystem
- Stimmungsvolle Ausgaben im IRC

Eine mögliche Inspiration durch Final Fantasy wurde erwähnt. Stil und konkrete Ausgestaltung bleiben offen.

## 5. Architekturgrundsätze

- Keine unübersichtliche Ein-Datei-Lösung.
- Systeme und Funktionen in klar abgegrenzte Module aufteilen.
- Für jedes Modul Aufgaben, Eingaben, Ausgaben und Abhängigkeiten dokumentieren.
- Den Datenfluss zwischen Modulen nachvollziehbar halten.
- Mehrere Spielende und getrennte Spielinstanzen von Anfang an berücksichtigen.
- In kleinen, überprüfbaren Schritten entwickeln und testen.
- Bestehendes Verhalten vor Änderungen prüfen und nicht unnötig überschreiben.

## 6. Zusammenarbeit

- Wir planen und entwickeln langsam und schrittweise; pro Gesprächsrunde möglichst nur eine gezielte Frage.
- Der Assistent bringt eigene Ideen ein, entwirft die Architektur und schreibt Code.
- Besonders komplexe Berechnungen, Schadensformeln und mathematische Verfahren werden vom Assistenten ausgearbeitet und verständlich erklärt.
- Der Projektverantwortliche prüft, testet und gibt Rückmeldung.
- Entscheidungen, offene Fragen, Änderungen und Testergebnisse werden in dieser Datei festgehalten.
- GitHub-Zugriff und Änderungen werden transparent gemeldet. Eine Änderung gilt erst nach bestätigter Ausführung als erfolgt.

## 7. Aktueller Stand

- [x] MSL/MRC als Skriptsprache festgelegt.
- [x] GitHub-Lesezugriff erfolgreich getestet.
- [x] GitHub-Schreibzugriff erfolgreich getestet: Diese Datei wurde angelegt, committet und danach wieder eingelesen.
- [x] Deutsch als verbindliche Projektsprache festgelegt.
- [x] Mehrspieler-Spiel als grundlegende Projektentscheidung festgelegt.
- [ ] Konkrete Regeln für Einzelkämpfe, Gruppen und getrennte Spielinstanzen festlegen.
- [ ] Erste kleine, spielbare Grundversion planen.
- [ ] Modulaufteilung und Datenspeicherung entwerfen.

## 8. Nächste Schritte

1. Das Mehrspieler-Konzept weiter eingrenzen.
2. Eine kleine, spielbare erste Ausbaustufe festlegen.
3. Modulaufteilung, Datenmodell und IRC-Befehle planen.
4. Funktionen einzeln entwickeln und testen.

## 9. Entscheidungsprotokoll

### Beschlossene Entscheidungen
- Das Projekt und alle von uns verfassten Texte werden auf Deutsch geführt.
- Das Spiel ist von Anfang an ein Mehrspieler-Rollenspiel.
- Die technische Form getrennter Kämpfe und möglicher Gruppenbereiche wird später entschieden.

### Noch offen
- Ob Kämpfe in privaten Kanälen, getrennten Spielinstanzen oder auf andere Weise stattfinden.
- Wie Spielende Gruppen bilden und weitere Personen einladen.
- Spielwelt, Tonalität und genaue Spielregeln.