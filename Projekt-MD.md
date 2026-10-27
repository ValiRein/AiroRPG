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

### Dauerhafte Spielaccounts und Nicknamenbindung

- Jeder Spielende erhält einen dauerhaft gespeicherten Spielaccount mit fortbestehendem Charakter und Fortschritt.
- Der Spielaccount ist ausdrücklich an den aktuellen IRC-Nicknamen gebunden.
- Wird der Nickname gewechselt, erkennt das Spielsystem den neuen Nicknamen als anderen, nicht zugeordneten Account. Auf den bisherigen Spielaccount kann unter diesem neuen Nicknamen nicht zugegriffen werden.
- Bei Befehlen, für die ein Spielaccount erforderlich ist, wird in diesem Fall eine verständliche Fehlermeldung auf Deutsch ausgegeben.
- Zu speichernder Fortschritt umfasst voraussichtlich Stufe, Erfahrung, Kernattribute, Ausrüstung, Inventar, Aufgabenfortschritt sowie Kampf- und Monsterstatistiken. Die genaue Datenstruktur und Speicherung sind noch offen.
- Hinweis zur späteren Umsetzung: Nicknamenbindung ist eine bewusste Spielregel, aber kein sicherer Identitätsnachweis, falls Nicknamen im IRC-Netzwerk übernommen werden können. Umgang mit solchen Fällen später klären.

### Vier Kernattribute

Die vier Kernattribute gelten als gemeinsame Grundlage für Spieler und Monster:

- **STR – Stärke:** bestimmt Stärke und Schaden von Nahkampfangriffen.
- **DEF – Verteidigung:** bestimmt, wie viel Schaden abgewehrt werden kann.
- **INT – Intelligenz:** bestimmt den Schaden von Techniken und trägt zur Abwehr von Technikschaden bei. Die Abwehr durch INT wird mit DEF kombiniert; die genaue Berechnung ist offen.
- **SPD – Geschwindigkeit:** bestimmt die Zugreihenfolge im rundenbasierten Kampf, also wer zuerst handelt.

Die Anfangslebenspunkte und weitere Ausgangswerte sind noch nicht festgelegt.

### Datenhaltung und Grundvorlagen

- Spielwerte und Vorlagen sollen nicht unnötig fest im Quellcode stehen, sondern aus strukturierten Datendateien eingelesen werden.
- Als mögliche Grundlage sind INI-Dateien und das Einlesen über die MSL-Funktion `$readini` vorgesehen.
- Eine Vorlage für einen neuen Standardspieler soll später die einstellbaren Ausgangswerte enthalten.
- Grundvorlagen, veränderliche individuelle Spielstände und allgemeine Regelwerte sollen logisch getrennt gehalten werden.
- Die genaue Aufteilung der Dateien und Speicherorte wird erst beim Entwurf des Datenmodells beschlossen.

### Charakterentwicklung und Monsterstärke

- Das Spiel soll ein Levelsystem für Charaktere und Monster erhalten.
- Die Grundstärke eines Monsters soll anhand der vier festgelegten Kernattribute STR, DEF, INT und SPD bestimmt werden.
- Zusätzlich soll die Siegesserie des Spielenden gegen das betreffende Monster beziehungsweise die Monsterart in seinen Kampf-Instanzen die Begegnung beeinflussen.
- Die konkrete Formel ist noch offen und wird später mit nachvollziehbaren Beispielen entwickelt und getestet.
- Bei der Gestaltung soll verhindert werden, dass eine unbegrenzte Steigerung der Schwierigkeit entsteht. Welche Auswirkungen Siege genau haben, ist noch nicht entschieden.

### Shop-Level und Preisberechnung

- Der Charakter erhält Lebenspunkte und andere Werte nicht automatisch allein durch einen Stufenaufstieg; die genaue Regel für Lebenspunkte bleibt noch offen. Die Charakterentwicklung soll insbesondere über Belohnungen und gezielte Käufe erfolgen.
- Monster können später Belohnungen wie Auron, Ätherkristalle oder besondere Gegenstände geben. Die genauen Beutearten und Belohnungsregeln sind noch offen.
- Im In-Game-Shop sollen unter anderem Kernattribute, Lebenspunkte, Fertigkeiten, Techniken, Items und Tränke gekauft oder verbessert werden können. Klassen und Klassenmeister sind nicht vorgesehen.
- Auron ist die allgemeine Spielwährung für Charakterwerte, Lebenspunkte, Fertigkeiten, Techniken, Tränke und gewöhnliche Gegenstände. Berechtigte Käufe in diesen Bereichen erhöhen das persönliche Shop-Level des Spielers. Ätherkristalle sind die besondere Spielwährung für Waffen und Rüstungen. Deren Käufe erhöhen das Shop-Level nicht; für sie gilt ein getrenntes Preissystem.
- Das Shop-Level ist selbst der Preisaufschlag-Multiplikator und wird in der jeweiligen Spielerdatendatei im Abschnitt `[Info]` gespeichert, beispielsweise als `ShopLevel=0.1`.
- Nach jedem erfolgreichen berechtigten Kauf steigt das Shop-Level um `0.1`. Das maximale Shop-Level beträgt `30`; bei Erreichen des Höchstwerts steigt es nicht weiter. Käufe bleiben weiterhin möglich, aber Shop-Level und Preisaufschlag steigen nicht über diesen Höchstwert hinaus.
- Der aktuelle Preis wird aus dem unveränderten Grundpreis und dem Shop-Level berechnet: `NeuerPreis = Grundpreis * ShopLevel + Grundpreis` (gleichbedeutend mit `Grundpreis * (1 + ShopLevel)`).
- Beispiel bei einem Grundpreis von 50: Shop-Level `0.0` ergibt 50, `0.1` ergibt 55, `0.2` ergibt 60 und `30` ergibt 1550.
- Waren- und Verbesserungsdaten enthalten unveränderte Grundpreise. Beim Aufruf der Shopliste berechnet der Bot anhand des aktuellen Shop-Levels die aktuellen Preise und zeigt sie beispielsweise per Privatnachricht an.
- Die Shopliste ist eine berechnete Ausgabe. Ob berechnete Listen oder Preise zusätzlich gespeichert werden müssen, ist noch offen; Grundpreise dürfen dabei nicht überschrieben werden.
- Der Kaufablauf muss vor Änderungen prüfen, ob der Spieler die nötige Währung besitzt. Erst ein gültiger Kauf darf die Verbesserung, den Währungsabzug und die Shop-Level-Aktualisierung auslösen. Die Umsetzung soll verhindern, dass nur ein Teil dieser Änderungen erfolgt.
- Beispielhafte, nicht endgültige Shopausgabe bei Shop-Level 0: `STR+5 (50)`, `DEF+5 (50)`, `INT+50 (Preis noch offen)`. Zahlen und Schreibweise sind nur Beispiele.
- Beispielhafter Befehl: `!Shop buy stats STR`. Der Bot liest den aktuellen STR-Wert aus, berechnet den neuen Wert (zum Beispiel mit `$calc(%stat-str + 5)`), prüft und zieht die Kosten ab und schreibt die aktualisierten Daten zurück. Die genaue Befehlsstruktur und Reihenfolge werden beim Entwurf festgelegt.
- Die Namen und grundlegenden Einsatzbereiche der beiden Währungen sind beschlossen. Anfangswerte, genaue Dateiaufteilung sowie weitere Währungs- und Kaufregeln sind noch offen.

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
- [ ] Kaufregeln und Kaufablauf weiter spezifizieren; die Währungsnamen und ihre grundlegenden Einsatzbereiche sind festgelegt.
- [x] Dauerhafte Spielaccounts mit strikter Nicknamenbindung als Spielregel festgelegt.
- [x] Levelsystem und Monsteranpassung anhand von vier Kernattributen und Siegesserien als Konzept festgehalten.
- [x] Vier Kernattribute STR, DEF, INT und SPD festgelegt.
- [x] Grundwerte sollen aus strukturierten Datendateien statt ausschließlich aus fest eingebauten Quellcodewerten gelesen werden.

## 8. Nächste Schritte

1. Das Mehrspieler-Konzept weiter eingrenzen.
2. Eine kleine, spielbare erste Ausbaustufe festlegen.
3. Modulaufteilung, Datenmodell und IRC-Befehle planen.
4. Funktionen einzeln entwickeln und testen.

## 9. Entscheidungsprotokoll

### Beschlossene Entscheidungen
- Das Projekt und alle von uns verfassten Texte werden auf Deutsch geführt.
- Das Spiel ist von Anfang an ein Mehrspieler-Rollenspiel.
- Jeder Spielaccount bleibt dauerhaft bestehen und ist an den IRC-Nicknamen gebunden; ein Nicknamenwechsel führt nicht zur automatischen Wiedererkennung des bisherigen Accounts.
- Die Kernattribute STR, DEF, INT und SPD gelten für Spieler und Monster.
- Monsterstärke soll sich aus diesen vier Kernattributen und der Siegesserie gegen die jeweilige Monsterart zusammensetzen; die Formel bleibt offen.
- Spielwerte sollen über strukturierte Datendateien verwaltet werden; INI-Dateien und `$readini` sind als Ansatz vorgesehen.
- Die Anfangslebenspunkte sind noch nicht festgelegt.
- Die technische Form getrennter Kämpfe und möglicher Gruppenbereiche wird später entschieden.

### Beschlossene Entscheidungen
- Die Charakterentwicklung soll über Belohnungen und Käufe erfolgen; automatische Lebenspunktsteigerungen durch Stufenaufstieg sind nicht festgelegt.
- Berechtigte Käufe von Charakterwerten, Fertigkeiten, Techniken, Items und Tränken erhöhen das persönliche Shop-Level um `0.1`, bis zum Höchstwert `30`; dadurch steigen spätere Preise. Auch auf Höchstlevel bleiben Käufe möglich, ohne dass Shop-Level oder Preisaufschlag weiter steigen.
- Das Shop-Level ist selbst der Preisaufschlag-Multiplikator. Die Formel lautet `NeuerPreis = Grundpreis * ShopLevel + Grundpreis`.
- Auron ist die allgemeine Spielwährung für Charakterwerte, Lebenspunkte, Fertigkeiten, Techniken, Tränke und gewöhnliche Gegenstände. Ätherkristalle sind die besondere Spielwährung für Waffen und Rüstungen. Käufe mit Ätherkristallen erhöhen das Shop-Level nicht.
- Das Shop-Level wird in der jeweiligen Spielerdatendatei unter `[Info]` gespeichert.
- Grundpreise bleiben in den Warendaten unverändert; die angezeigten Preise werden anhand des Shop-Levels berechnet.

### Noch offen
- Ob Kämpfe in privaten Kanälen, getrennten Spielinstanzen oder auf andere Weise stattfinden.
- Wie Spielende Gruppen bilden und weitere Personen einladen.
- Spielwelt, Tonalität und genaue Spielregeln.
- Weitere Kaufregeln, Anfangswerte und Einzelheiten zur Verwendung und Vergabe der Währungen.
- Ob berechnete Shoplisten gespeichert oder nur bei Aufruf erzeugt werden.