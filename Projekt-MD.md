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
- Zu speichernder Fortschritt umfasst voraussichtlich Stufe, Kernattribute, Ausrüstung, Inventar, Aufgabenfortschritt sowie Kampf- und Monsterstatistiken. Die genaue Datenstruktur und Speicherung sind noch offen. Ein automatischer Stufenaufstieg durch das Sammeln eines bestimmten Erfahrungspunktewerts ist nicht vorgesehen.
- Hinweis zur späteren Umsetzung: Nicknamenbindung ist eine bewusste Spielregel, aber kein sicherer Identitätsnachweis, falls Nicknamen im IRC-Netzwerk übernommen werden können. Umgang mit solchen Fällen später klären.

### Vier Kernattribute

Die vier Kernattribute gelten als gemeinsame Grundlage für Spieler und Monster:

- **STR – Stärke:** bestimmt Stärke und Schaden von Nahkampfangriffen.
- **DEF – Verteidigung:** bestimmt, wie viel Schaden abgewehrt werden kann.
- **INT – Intelligenz:** bestimmt den Schaden von Techniken und trägt zur Abwehr von Technikschaden bei. Die Abwehr durch INT wird mit DEF kombiniert; die genaue Berechnung ist offen.
- **SPD – Geschwindigkeit:** bestimmt die Zugreihenfolge im rundenbasierten Kampf, also wer zuerst handelt.

Die Anfangslebenspunkte und weitere Ausgangswerte sind noch nicht festgelegt. Die maximalen Lebenspunkte, die ein Spieler durch Käufe erreichen kann, sind auf 5.000 HP begrenzt. Weitere HP-Käufe dürfen diesen Wert nicht überschreiten.

Die vier Kernattribute STR, DEF, INT und SPD haben keine festen absoluten Maximalwerte. Ihre Entwicklung soll grundsätzlich offen bleiben; eine noch auszuarbeitende Verhältnisregel soll jedoch verhindern, dass ein einzelnes Attribut gegenüber den übrigen völlig überproportional gesteigert wird.

### Kampfabwechslung, Schadensreduzierung und Stil-XP

- Wiederholt ein Spieler dieselbe Fähigkeit oder Technik in aufeinanderfolgenden Runden, soll der dadurch verursachte Schaden einmalig reduziert werden.
- Die Schadensreduzierung ist nicht stapelbar: Sie soll nicht mit jeder weiteren Wiederholung immer stärker werden.
- Die genaue Höhe der Reduzierung und die präzise Regel, wann sie ausgelöst beziehungsweise zurückgesetzt wird, sind noch offen. Als vorläufiger Vorschlag gilt, dass nur dieselbe Aktion in direkt aufeinanderfolgenden Runden die Reduzierung auslöst.
- Diese Kampfregel ist von der Stil-XP getrennt zu berechnen. Stil-XP beeinflusst den Multiplikator für die Auron-Belohnung am Kampfende; die Schadensreduzierung beeinflusst den Schaden während des Kampfs.
- Beide Mechaniken sollen abwechslungsreiches Spielen fördern, ohne dass ihre Formeln oder technischen Details bereits festgelegt sind.

### Datenhaltung und Grundvorlagen

- Spielwerte und Vorlagen sollen nicht unnötig fest im Quellcode stehen, sondern aus strukturierten Datendateien eingelesen werden.
- Als mögliche Grundlage sind INI-Dateien und das Einlesen über die MSL-Funktion `$readini` vorgesehen.
- Eine Vorlage für einen neuen Standardspieler soll später die einstellbaren Ausgangswerte enthalten.
- Grundvorlagen, veränderliche individuelle Spielstände und allgemeine Regelwerte sollen logisch getrennt gehalten werden.
- Die genaue Aufteilung der Dateien und Speicherorte wird erst beim Entwurf des Datenmodells beschlossen.

### Waffen-Datenbank und Charakterausrüstung

- Waffen werden wie Techniken nicht fest im Quellcode definiert. Ihre Daten liegen in einer externen Datei, beispielsweise `weapons.db`, im INI-Stil.
- `[WeaponDisplayTitles]` ordnet Kategorien interne Schlüssel und deutsche Anzeigenamen für die spätere Shopliste zu, zum Beispiel `HandToHand=Handwaffen`.
- `[Weapons]` ordnet einen Kategorienamen einer Waffenliste zu, zum Beispiel `HandToHand=Fists`. Weitere Kategorien und Waffen können später ergänzt werden.
- Ein eigener Waffenabschnitt, beispielsweise `[Fists]`, enthält die Felder `Cost`, `Type`, `Info`, `Abilities`, `Hits`, `BasePower` und `Upgrade`.
- `Type` verweist auf die Waffenkategorie. `Info` enthält die Beschreibung. `Abilities` benennt die von der Waffe bereitgestellte Technik; dieser Technikname muss in `techniques.db` existieren. `Hits` und `BasePower` beschreiben die Standardtrefferzahl und Standardschadensstärke der Waffe. `Upgrade` gibt den Ätherkristallpreis für eine Waffenlevel-Erhöhung an.
- Die Fäuste (`Fists`) sind die Standardwaffe jedes neuen Charakters. Sie sind nicht kaufbar und können nicht abgelegt werden. Der Beispielwert `Cost=1` ist ein Datenfeld; die Regel, dass diese Waffe nicht kaufbar ist, muss vom System ausdrücklich berücksichtigt werden.
- Waffen werden mit Ätherkristallen aufgewertet. Waffen- und Rüstungskäufe beziehungsweise -aufwertungen erhöhen das Shop-Level nicht.
- Der Kauf einer Waffe rüstet sie nicht automatisch aus. Der Spieler rüstet eine bereits besessene Waffe später ausdrücklich mit dem Befehl `!equip weapon waffename` aus.
- Ob Waffen ein erforderliches Mindestlevel erhalten, ist noch offen. Falls diese Regel eingeführt wird, soll `weapons.db` dafür einen eigenen Schlüsselwert erhalten, beispielsweise `RequiredLevel`.
- Beim Ausrüsten muss geprüft werden, ob der Charakter die Waffe besitzt und alle festgelegten Voraussetzungen erfüllt.

Beispielstruktur für `weapons.db`:

```ini
[WeaponDisplayTitles]
HandToHand=Handwaffen

[Weapons]
HandToHand=Fists

[Fists]
Cost=1
Type=HandToHand
Info=Es sind deine Fäuste. Sie lassen sich nicht abnehmen.
Abilities=DoublePunch
Hits=2
BasePower=10
Upgrade=2000
```

### Waffen- und Technikdaten im Charakterspielstand

- Der Charakterspielstand speichert unter `[Weapons]` die aktuell ausgerüstete Waffe über `Equipped=Fists` sowie den individuellen Level jeder besessenen Waffe, beispielsweise `Fists=1`.
- Unter `[Techs]` speichert der Spielstand den individuellen Level jeder Technik, beispielsweise `DoublePunch=1`.
- Die Standardtechnik `DoublePunch` ist für jeden neuen Charakter vorgesehen und wird durch die Standardwaffe `Fists` bereitgestellt. Die Waffe verweist in `weapons.db` über `Abilities=DoublePunch` auf die Definition in `techniques.db`.
- Allgemeine Definitionen in den Datenbanken und individuelle Level im Charakterspielstand bleiben getrennt. Die genaue Prüfung, wie Waffenlevel und Techniklevel den Kampf beeinflussen, wird später festgelegt.
- Bei der Kaufübersicht für Techniken dürfen nur Techniken angezeigt werden, die von der aktuell ausgerüsteten Waffe bereitgestellt werden. Dafür wird die ausgerüstete Waffe aus dem Charakterspielstand gelesen und ihre Technikzuordnung aus `weapons.db` ermittelt; die passenden Technikdefinitionen werden in `techniques.db` nachgeschlagen.
- Die genaue Umsetzung der Filterung sowie die Regeln für bereits bekannte oder aufgewertete Techniken werden beim Entwurf des Shop-Moduls festgelegt.

### Technik-Datenbank

- Techniken werden nicht fest im Quellcode definiert. Ihre Daten liegen in einer externen Technik-Datenbank, zunächst beispielsweise in der Datei `techniques.db`, die im INI-Stil aufgebaut ist und über allgemeine Lade- und Kampffunktionen eingelesen wird.
- Die Technikzuordnung ist an die Waffe gekoppelt, mit der ein Charakter startet beziehungsweise die er verwendet. Als Beispiel ist in `[Techs]` die Zuordnung `Fists=DoublePunch` vorgesehen.
- Jede Technik hat einen eigenen Abschnitt mit ihrer Kennung als Abschnittsnamen, beispielsweise `[DoublePunch]`. Vorgesehene Felder sind `Type`, `Hits`, `BasePower`, `BaseStats` und `Desc`.
- Für den ersten Prototypen genügt der Techniktyp `SINGLE`. Später können weitere Typen wie `HEAL`, `AOE`, `DEAD` und `BUFF` hinzukommen.
- `Hits` beschreibt die Trefferzahl. Für den Prototypen kann sie ein fester Wert sein; variable Trefferzahlen, etwa ein zufälliger Wert von 1 bis 8, werden später über eine festgelegte Datenstruktur umgesetzt.
- `BasePower` ist die Grundstärke der Technik. `BaseStats` benennt das Attribut, das für die Schadensberechnung herangezogen wird, beispielsweise `str`. Die konkrete Formel und ein ausbalancierter Multiplikator sind noch offen, damit Techniken nicht unverhältnismäßig stark werden.
- `Desc` enthält den deutschen Beschreibungstext, den der Bot bei Anwendung der Technik ausgibt. Vorgesehen sind Platzhalter wie `%enemy`, die der Bot zur Laufzeit durch passende Werte ersetzt.
- Die Datenbank beschreibt die Technik; die allgemeine Kampflogik liest und verarbeitet diese Werte. Beliebige MSL-Ausdrücke aus Datendateien sollen nicht ungeprüft ausgeführt werden. Zulässige Werte, Zufallsbereiche und Platzhalter werden durch das System kontrolliert.

Beispiel für einen ersten Datensatz:

```ini
[Techs]
Fists=DoublePunch

[DoublePunch]
Type=SINGLE
Hits=2
BasePower=50
BaseStats=str
Desc=%enemy wird mit einem Doppelschlag angegriffen!
```

Dieser Datensatz dient als Strukturbeispiel; die Werte sind noch nicht auf Spielbalance geprüft.

### Charakterentwicklung und Monsterstärke

### Zentrale Levelberechnung: `$get.level($1)`

- `$get.level($1)` ist der allgemeine Helfer für Levelabfragen und Levelberechnungen. Er speichert kein eigenes Level, sondern berechnet bei jedem Aufruf das aktuell gültige Level neu und gibt es zurück.
- Die Funktion verwendet die aktuellen Werte aus dem Abschnitt `[Battle]` des Charakterspielstands. Dadurch können temporäre Buffs und Debuffs das berechnete Level während eines Kampfes beeinflussen.
- Vorgesehene Attributwirkungen umfassen beispielsweise `StrUP`, `DefUP`, `IntUP`, `SpdUP` sowie die entsprechenden Debuffs `StrDown`, `DefDown`, `IntDown` und `SpdDown`.
- Andere Spielmechaniken können den zurückgegebenen Levelwert weiterverwenden, beispielsweise Kampfberechnungen, Buffs, Debuffs oder Prüfungen von Voraussetzungen.
- Nach dem Kampf werden die Werte in `[Battle]` wieder mit den dauerhaften Werten aus `[BaseStats]` synchronisiert. Dadurch ergibt die nächste Berechnung wieder das Level, das sich aus den dann gültigen Werten ergibt.
- Die interne Levelkurve, die genaue Verwendung von `floor` und `StatWeight` sowie die Gewichtungsgrenzen sind noch offen und werden separat entwickelt und mit Beispielcharakteren getestet.
- Eine Attributänderung führt nicht zwingend zu einer Leveländerung; das Ergebnis hängt von der später festgelegten Berechnungsformel ab.

- Das Spiel soll ein Levelsystem für Charaktere und Monster erhalten.
- Die Charakterstufe wird anhand einer Berechnung der vier Kernattribute STR, DEF, INT und SPD bestimmt. Einen festen Erfahrungspunktewert, der automatisch eine höhere Stufe auslöst, gibt es nicht.
- Für die vier Kernattribute gibt es keine festen absoluten Maximalwerte. Die Spieler sollen sie langfristig frei erhöhen können, solange die noch festzulegende Verhältnisregel eine sinnvolle Balance zwischen den Attributen wahrt.
- Lebenspunkte sind davon getrennt: Durch Käufe kann ein Charakter höchstens 5.000 HP erreichen. Dieser Wert darf durch weitere HP-Käufe nicht überschritten werden.
- Spielende stärken ihre Charaktere selbst, indem sie ihre verdienten Auron gezielt für Verbesserungen ausgeben. Dadurch können sich die Kernattribute verändern und damit auch die daraus berechnete Charakterstufe. Die genaue Formel für die Stufenberechnung ist noch offen und wird später gemeinsam festgelegt.
- Die Grundstärke eines Monsters soll anhand der vier festgelegten Kernattribute STR, DEF, INT und SPD bestimmt werden.
- Zusätzlich beeinflusst die allgemeine Siegesserie des Spielenden die Stärke künftig erzeugter Monster, unabhängig von deren Art. Die Siegesserie wird im Charakterspielstand unter `[Info]` mit dem Schlüssel `BattleStreak` gespeichert. Bei jedem Sieg wird der Wert um `1` erhöht; bei einer Niederlage oder Flucht aus dem Kampf wird er auf `0` zurückgesetzt. Eine Flucht gilt als indirekte Niederlage, da der Spielende den Kampf nicht bis zum Sieg fortsetzt. Ein Sieg gegen jede beliebige Monsterart zählt zur selben Siegesserie.
- Die konkrete Formel ist noch offen und wird später mit nachvollziehbaren Beispielen entwickelt und getestet.
- Bei der Gestaltung soll verhindert werden, dass eine unbegrenzte Steigerung der Schwierigkeit entsteht. Welche Auswirkungen Siege genau haben, ist noch nicht entschieden.

### Shop-Level und Preisberechnung

- Der Charakter erhält Lebenspunkte und andere Werte nicht automatisch allein durch einen Stufenaufstieg; die genaue Regel für Lebenspunkte bleibt noch offen. Die Charakterentwicklung soll insbesondere über Belohnungen und gezielte Käufe erfolgen.
- Auron werden durch gewöhnliche Monsterkämpfe verdient. Ätherkristalle werden ausschließlich durch Bosskämpfe vergeben, beispielsweise in Boss-Instanzen; gewöhnliche Monsterkämpfe gewähren keine Ätherkristalle.
- Die Auron-Belohnung eines gewöhnlichen Monsterkampfs soll von der Effektivität und Abwechslung des Kampfstils abhängen. Während des Kampfs wird dafür eine sogenannte Stil-XP erfasst: Die wiederholte Nutzung derselben Fähigkeit soll sie verringern, während der abwechslungsreiche Einsatz verschiedener Aktionen – etwa Angriffe, Techniken und Gegenstände – sie erhöhen kann.
- Am Kampfende soll aus der Stil-XP ein Multiplikator berechnet werden, der in die Berechnung der Auron-Belohnung des besiegten Monsters einfließt. Bewertungsregeln, Multiplikatorgrenzen und die genaue Belohnungsformel sowie die technische Umsetzung bleiben offen. Das System soll sinnvolle Vielfalt fördern und nicht bloß wahlloses Wechseln von Aktionen.
- Die genauen Mengen und weitere mögliche Belohnungen sind noch offen.
- Im In-Game-Shop sollen unter anderem Kernattribute, Lebenspunkte, Fertigkeiten, Techniken, Items und Tränke gekauft oder verbessert werden können. Klassen und Klassenmeister sind nicht vorgesehen.
- Auron ist die allgemeine Spielwährung für Charakterwerte, Lebenspunkte, Fertigkeiten, Techniken, Tränke und gewöhnliche Gegenstände. Berechtigte Käufe in diesen Bereichen erhöhen das persönliche Shop-Level des Spielers. Ätherkristalle sind die besondere Spielwährung für Waffen und Rüstungen. Deren Käufe erhöhen das Shop-Level nicht; für sie gilt ein getrenntes Preissystem.
- Das Shop-Level ist selbst der Preisaufschlag-Multiplikator und wird in der jeweiligen Spielerdatendatei im Abschnitt `[Info]` gespeichert, beispielsweise als `ShopLevel=0.1`.
- Die Charakterstufe steigt nicht automatisch durch Erfahrungspunkte. Sie ergibt sich aus einer noch festzulegenden Berechnung der vier Kernattribute STR, DEF, INT und SPD. Die Spielenden entwickeln ihren Charakter selbst, indem sie verdiente Auron in Verbesserungen investieren.
- Nach jedem erfolgreichen berechtigten Kauf steigt das Shop-Level um `0.1`. Das maximale Shop-Level beträgt `30`; bei Erreichen des Höchstwerts steigt es nicht weiter. Käufe bleiben weiterhin möglich, aber Shop-Level und Preisaufschlag steigen nicht über diesen Höchstwert hinaus.
- Der aktuelle Preis wird aus dem unveränderten Grundpreis und dem Shop-Level berechnet: `NeuerPreis = Grundpreis * ShopLevel + Grundpreis` (gleichbedeutend mit `Grundpreis * (1 + ShopLevel)`).
- Beispiel bei einem Grundpreis von 50: Shop-Level `0.0` ergibt 50, `0.1` ergibt 55, `0.2` ergibt 60 und `30` ergibt 1550.
- Waren- und Verbesserungsdaten enthalten unveränderte Grundpreise. Beim Aufruf der Shopliste berechnet der Bot anhand des aktuellen Shop-Levels die aktuellen Preise und zeigt sie beispielsweise per Privatnachricht an.
- Die Shopliste ist eine berechnete Ausgabe. Ob berechnete Listen oder Preise zusätzlich gespeichert werden müssen, ist noch offen; Grundpreise dürfen dabei nicht überschrieben werden.
- Für eine spätere Ausbaustufe sind Shop-NPCs vorgesehen, die besondere Gegenstände verkaufen können, beispielsweise eine `Discount-Card` oder `Medium-Discount-Card`. Diese Gegenstände sollen in einer externen `items.db` mit ihren Eigenschaften, Schlüsselwerten und Funktionen definiert werden.
- Bestimmte Rabattgegenstände sollen das persönliche Shop-Level um eine festgelegte Anzahl an Stufen senken können. Dadurch sinkt der Preisaufschlag und der Spieler kann später wieder günstiger einkaufen. Die konkreten Gegenstände, Senkungswerte, Preise und Anwendungsregeln sind noch offen.
- Shop-NPCs und Rabattgegenstände gehören nicht zum Umfang der ersten spielbaren Version; sie sind als spätere Erweiterung vorgemerkt.
- Der Kaufablauf muss vor Änderungen prüfen, ob der Spieler die nötige Währung besitzt. Erst ein gültiger Kauf darf die Verbesserung, den Währungsabzug und die Shop-Level-Aktualisierung auslösen. Die Umsetzung soll verhindern, dass nur ein Teil dieser Änderungen erfolgt.
- Beispielhafte, nicht endgültige Shopausgabe bei Shop-Level 0: `STR+5 (50)`, `DEF+5 (50)`, `INT+50 (Preis noch offen)`. Zahlen und Schreibweise sind nur Beispiele.
- Beispielhafter Befehl: `!Shop buy stats STR`. Der Bot liest den aktuellen STR-Wert aus, berechnet den neuen Wert (zum Beispiel mit `$calc(%stat-str + 5)`), prüft und zieht die Kosten ab und schreibt die aktualisierten Daten zurück. Die genaue Befehlsstruktur und Reihenfolge werden beim Entwurf festgelegt.
- Die Namen und grundlegenden Einsatzbereiche der beiden Währungen sind beschlossen. Anfangswerte, genaue Dateiaufteilung sowie weitere Währungs- und Kaufregeln sind noch offen.

## 3.1 Umfang des ersten spielbaren Prototyps

Der erste Prototyp soll einen kleinen, aber bereits spielbaren Mehrspieler-Kern enthalten. Weitere Systeme wie der Shop werden später ergänzt.

- **Charakterregistrierung:** Spielende können einen Charakter registrieren. Der Spielstand enthält unter anderem den Charakternamen, die Charakterbeschreibung, Statuswerte und die erforderlichen Anfangswerte.
- **Standardausrüstung:** Jeder neue Charakter erhält die Standardwaffe `Fists` (Fäuste) sowie die Technik `DoublePunch` mit einem festgelegten Anfangslevel. Die genauen Startwerte und das Anfangslevel werden bei der Ausarbeitung des Datenmodells bestätigt.
- **Monster-Auswahl:** Für den Prototypen sind zunächst höchstens fünf einfache Monsterarten vorgesehen. Bei einer Begegnung wird zufällig eine dieser Arten ausgewählt.
- **Rundenbasierter Kampf:** Der Prototyp umfasst einfache Angriffe, die Standardtechnik, die Berechnung von Schaden und Zugreihenfolge sowie die Verarbeitung von Sieg und Niederlage.
- **Monsterentwicklung durch Siegesserien:** Für jeden Spielenden wird eine allgemeine Siegesserie unabhängig von der Monsterart geführt. Sie wird in der Charakterdatei unter `[Info]` als `BattleStreak` gespeichert und beginnt bei `0`. Jeder Sieg erhöht den Wert um `1`, egal welche Monsterart besiegt wurde. Bei einer Niederlage oder Flucht wird der Wert auf `0` zurückgesetzt; eine Flucht gilt als indirekte Niederlage. Die Siegesserie beeinflusst die Stärke künftig erzeugter Monster; die genaue Formel, Gewichtung und Grenzen werden später festgelegt und getestet.
- **Belohnung:** Ein Sieg gewährt zunächst Auron. Die genaue Höhe der Belohnung ist noch festzulegen. Ätherkristalle werden im Einklang mit der bestehenden Regel ausschließlich durch Bosskämpfe vergeben; gewöhnliche Monsterkämpfe gewähren keine Ätherkristalle.
- **Externe Meldungen und Beschreibungen:** Kampfmeldungen, Systemmeldungen, Fehlermeldungen und Monsterbeschreibungen sollen in den jeweils zuständigen Datendateien liegen und nicht fest in den MSL-Quellcode geschrieben werden.
- **Erweiterbarkeit:** Der Shop und weitere geplante Systeme gehören nicht zum ersten Prototypen. Die Modul- und Datenstruktur soll ihre spätere Ergänzung ermöglichen, ohne den vorhandenen Kern unnötig umzubauen.

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
- [x] Charakterstufe wird aus den vier Kernattributen berechnet; kein automatischer Stufenaufstieg durch einen festen EP-Wert. Charaktere werden durch selbst finanzierte Verbesserungen mit verdienten Auron gestärkt.
- [x] Vier Kernattribute STR, DEF, INT und SPD festgelegt.
- [x] Allgemeine Level-Funktion `$get.level($1)` festgelegt: Sie berechnet das aktuelle Level aus `[Battle]`; die konkrete Formel bleibt offen.
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
- Die Charakterstufe ergibt sich aus einer Berechnung der vier Kernattribute; ein fester EP-Wert löst keinen automatischen Stufenaufstieg aus. Spielende stärken ihre Charaktere selbst durch Käufe mit verdienten Auron. Die genaue Stufenformel bleibt offen.
- Die zentrale Funktion `$get.level($1)` berechnet bei jedem Aufruf das aktuelle Level anhand der aktuellen `[Battle]`-Werte und gibt es zurück, ohne das Level separat zu speichern. Buffs und Debuffs können dadurch das berechnete Level vorübergehend beeinflussen. Die genaue Levelkurve und `StatWeight`-Formel bleiben offen.
- Monsterstärke soll sich aus diesen vier Kernattributen und der allgemeinen Siegesserie des Spielenden zusammensetzen. Die Siegesserie wird unabhängig von der Monsterart unter `[Info]` im Schlüssel `BattleStreak` gespeichert: jeder Sieg erhöht sie um `1`, eine Niederlage oder Flucht setzt sie auf `0`; eine Flucht gilt als indirekte Niederlage. Die genaue Formel bleibt offen.
- Spielwerte sollen über strukturierte Datendateien verwaltet werden; INI-Dateien und `$readini` sind als Ansatz vorgesehen.
- Techniken werden extern in einer Technik-Datenbank im INI-Stil definiert und über Waffenzuordnungen verfügbar gemacht; für den ersten Prototypen ist der Typ `SINGLE` vorgesehen. Technikdaten wie Trefferzahl, Grundstärke, Attribut und Beschreibung werden nicht fest im Quellcode hinterlegt.
- Waffen werden extern in `weapons.db` definiert; Kategorien, Anzeigenamen, Waffenwerte, verfügbare Techniken und Ätherkristall-Aufwertungskosten stehen in der Datei statt im Quellcode.
- Die Standardwaffe jedes neuen Charakters sind nicht kaufbare und nicht ablegbare Fäuste (`Fists`) mit der Standardtechnik `DoublePunch`. Ausgerüstete Waffe sowie Waffen- und Techniklevel werden individuell in der Charakterdatei unter `[Weapons]` und `[Techs]` gespeichert.
- Der Kauf einer Waffe rüstet sie nicht automatisch aus; zum Ausrüsten ist der Befehl `!equip weapon waffename` vorgesehen. Ein mögliches Mindestlevel für Waffen ist noch offen.
- In der kaufbaren Technikliste dürfen nur Techniken erscheinen, die zur aktuell ausgerüsteten Waffe gehören. Die Auswahl wird anhand des Charakterspielstands und der Zuordnung in `weapons.db` gefiltert.
- Die Anfangslebenspunkte sind noch nicht festgelegt; die maximale durch Käufe erreichbare Lebenspunktzahl beträgt 5.000 HP.
- Die vier Kernattribute haben keine festen absoluten Obergrenzen. Eine noch zu entwickelnde Verhältnisregel soll einseitige Verteilungen verhindern, ohne die freie Schwerpunktwahl vollständig aufzuheben.
- Die technische Form getrennter Kämpfe und möglicher Gruppenbereiche wird später entschieden.

### Beschlossene Entscheidungen
- Die Charakterentwicklung erfolgt durch selbst gewählte Verbesserungen, die mit verdienten Auron gekauft werden. Die Charakterstufe wird aus den vier Kernattributen berechnet und steigt daher nicht automatisch durch einen festen Erfahrungspunktewert. Die genaue Formel bleibt offen.
- Die Kernattribute STR, DEF, INT und SPD besitzen keine festen absoluten Maximalwerte; eine Verhältnisregel soll verhindern, dass einzelne Attribute die übrigen vollständig verdrängen. Die maximale durch Käufe erreichbare Lebenspunktzahl ist auf 5.000 HP begrenzt.
- Automatische Lebenspunktsteigerungen durch Stufenaufstieg sind nicht vorgesehen; Lebenspunkte werden bei Bedarf gezielt verbessert und mit Auron gekauft.
- Berechtigte Käufe von Charakterwerten, Fertigkeiten, Techniken, Items und Tränken erhöhen das persönliche Shop-Level um `0.1`, bis zum Höchstwert `30`; dadurch steigen spätere Preise. Auch auf Höchstlevel bleiben Käufe möglich, ohne dass Shop-Level oder Preisaufschlag weiter steigen.
- Das Shop-Level ist selbst der Preisaufschlag-Multiplikator. Die Formel lautet `NeuerPreis = Grundpreis * ShopLevel + Grundpreis`.
- Auron ist die allgemeine Spielwährung für Charakterwerte, Lebenspunkte, Fertigkeiten, Techniken, Tränke und gewöhnliche Gegenstände. Ätherkristalle sind die besondere Spielwährung für Waffen und Rüstungen. Käufe mit Ätherkristallen erhöhen das Shop-Level nicht.
- Auron werden durch gewöhnliche Monsterkämpfe verdient; Ätherkristalle gibt es ausschließlich als Belohnung für Bosskämpfe, etwa in Boss-Instanzen. Normale Monsterkämpfe gewähren keine Ätherkristalle.
- Die Auron-Belohnung gewöhnlicher Monsterkämpfe soll durch Stil-XP und einen daraus berechneten Multiplikator beeinflusst werden. Abwechslungsreiche, effektive Aktionen sollen die Stil-XP fördern; wiederholte Nutzung derselben Fähigkeit soll sie senken. Die genaue Formel und Umsetzung bleiben offen.
- Die wiederholte Nutzung derselben Fähigkeit oder Technik in aufeinanderfolgenden Runden soll eine einmalige, nicht stapelbare Schadensreduzierung auslösen. Die genaue Höhe und Auslöse-/Rücksetzungsregel bleiben offen. Diese Schadensregel wird getrennt von der Stil-XP und dem Belohnungsmultiplikator behandelt.
- Das Shop-Level wird in der jeweiligen Spielerdatendatei unter `[Info]` gespeichert.
- Grundpreise bleiben in den Warendaten unverändert; die angezeigten Preise werden anhand des Shop-Levels berechnet.

### Umfang des ersten spielbaren Prototyps

- Der erste Prototyp umfasst Charakterregistrierung mit Charakterbeschreibung und Statuswerten, Standardausrüstung (Fäuste und `DoublePunch`), bis zu fünf zufällig ausgewählte einfache Monsterarten, ein grundlegendes rundenbasiertes Kampfsystem, Sieg und Niederlage, Auron-Belohnungen sowie externe Dateien für Kampf- und Systemmeldungen.
- Die allgemeine Siegesserie jedes Spielenden soll bereits im Prototyp erfasst werden. Sie wird in der Charakterdatei unter `[Info]` als `BattleStreak` gespeichert, startet bei `0`, steigt bei jedem Sieg unabhängig von der Monsterart um `1` und wird bei einer Niederlage auf `0` zurückgesetzt. Sie beeinflusst die Stärke künftig erzeugter Monster; die genaue Formel und Grenzen bleiben offen.
- Der Shop wird erst in einer späteren Ausbaustufe ergänzt.

### Noch offen
- Ob Kämpfe in privaten Kanälen, getrennten Spielinstanzen oder auf andere Weise stattfinden.
- Wie Spielende Gruppen bilden und weitere Personen einladen.
- Spielwelt, Tonalität und genaue Spielregeln.
- Weitere Kaufregeln, Anfangswerte und Einzelheiten zur Verwendung und Vergabe der Währungen.
- Ob berechnete Shoplisten gespeichert oder nur bei Aufruf erzeugt werden.
- Die genaue Verhältnisregel für STR, DEF, INT und SPD sowie die mathematische Formel zur Berechnung der Charakterstufe.
- Anfangs-HP und HP-Kaufregeln im Detail; die absolute HP-Obergrenze von 5.000 ist festgelegt.