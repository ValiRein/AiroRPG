# AiroRPG – Projekt-MD

> Zweck: dauerhafte Projektübersicht und Übergabe-Kontext für neue ChatGPT-Unterhaltungen. Nach wichtigen Entscheidungen wird dieses Dokument aktualisiert.

## 1. Projektziel

Ein modular aufgebauter, textbasierter RPG-Spielbot für ein bereits bestehendes IRC-Netzwerk. Anope und UnrealIRCd sind bereits eingerichtet; deren Installation und Netzwerkkonfiguration sind nicht Teil dieses Projekts.

Das Spiel soll über IRC-Kommandos und atmosphärische Bot-Nachrichten spielbar sein. Spielwelt, Tonalität und Umfang werden schrittweise gemeinsam festgelegt.

## 2. Technologie

- Sprache: mIRC Scripting Language (MSL, auch MRC genannt).
- Umgebung: mIRC-Scripting; genaue Laufzeit-/Hostdetails werden bei Bedarf geklärt.
- Serverumgebung: bestehendes IRC-Netz mit Anope und UnrealIRCd.
- Versionsverwaltung: GitHub-Repository ValiRein/AiroRPG.

## 3. Geplante Spielbereiche

Bisher genannte Ideen, noch nicht abschließend spezifiziert:
- Rollenspiel und Charaktere
- Quests
- Dungeons und Monster
- Crafting
- Waffenwahl und Ausrüstung
- Integrierter Shop
- Events
- Eigenes rundenbasiertes Kampfsystem
- Atmosphärische Ausgaben im IRC

Eine mögliche Final-Fantasy-Inspiration wurde erwähnt; Stil und konkrete Ausgestaltung sind noch offen.

## 4. Architekturprinzipien

- Keine monolithische Ein-Datei-Lösung.
- Funktionen und Systeme modular trennen.
- Für jedes Modul klare Aufgaben sowie definierte Ein- und Ausgaben festlegen.
- Datenfluss und Abhängigkeiten dokumentieren.
- Änderungen in kleinen, testbaren Schritten entwickeln.
- Bestehendes Verhalten nicht ohne Prüfung unnötig überschreiben.

## 5. Zusammenarbeit

- Planung und Umsetzung erfolgen schrittweise und in kleinen Portionen.
- Pro Gesprächsrunde wird möglichst nur eine gezielte offene Frage gestellt.
- Der Assistent soll aktiv Ideen einbringen, Architektur und Code entwerfen und bei komplexer Mathematik, Schadensformeln und Algorithmen unterstützen.
- Der Projektverantwortliche prüft, testet und gibt Rückmeldung.
- Annahmen, Entscheidungen, offene Fragen und Teststatus werden in diesem Dokument festgehalten.
- GitHub-Zugriff und Änderungen werden transparent gemeldet; keine erfolgreiche Änderung behaupten, bevor das Werkzeug sie bestätigt.

## 6. Aktueller Stand

- [x] Programmiersprache als MSL/MRC festgelegt.
- [x] GitHub-Repository gefunden: ValiRein/AiroRPG.
- [x] Lesetest erfolgreich: README.md auf dem Standardbranch main gelesen.
- [x] Die verbundene GitHub-Integration meldet für das Repository Push-Rechte.
- [ ] Tatsächlicher Schreibtest bzw. erster Commit bestätigt, dass Schreiben funktioniert.
- [ ] Spielkonzept und erste spielbare Minimalversion festlegen.
- [ ] Modulstruktur und Datenhaltung entwerfen.

## 7. Nächste Schritte

1. Projektgrundlagen und gewünschte Spielerfahrung gemeinsam präzisieren.
2. Eine kleine, lauffähige Minimalversion festlegen.
3. Modulstruktur, Datenmodell und IRC-Kommandos entwerfen.
4. In kleinen Schritten implementieren und testen.

## 8. Entscheidungsprotokoll

Noch keine weiteren Entscheidungen. Offene Themen werden erst nach gemeinsamer Besprechung als beschlossen eingetragen.
