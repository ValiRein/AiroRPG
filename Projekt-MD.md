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
- `Hits` beschreibt die Trefferzahl und kann entweder einen festen Zahlenwert oder einen unterstützten Zufallsausdruck enthalten, beispielsweise `Hits=2` oder `Hits=$rand(1,8)`. Separate Schlüssel wie `HitsMin` und `HitsMax` sind nicht erforderlich. Das System wertet den Eintrag für die jeweilige Attacke einmal aus. Liegt der berechnete Wert über `8`, wird er intern auf `8` begrenzt; andernfalls wird der berechnete Wert verwendet.
- `BasePower` ist die Grundstärke der Technik. `BaseStats` benennt das Attribut, das für die Schadensberechnung herangezogen wird, beispielsweise `str`. Die konkrete Formel und ein ausbalancierter Multiplikator sind noch offen, damit Techniken nicht unverhältnismäßig stark werden.
- `Desc` enthält den deutschen Beschreibungstext, den der Bot bei Anwendung der Technik ausgibt. Vorgesehen sind Platzhalter wie `%enemy`, die der Bot zur Laufzeit durch passende Werte ersetzt.
- Die Datenbank beschreibt die Technik; die allgemeine Kampflogik liest und verarbeitet diese Werte. Zahlenwerte und ausdrücklich unterstützte Ausdrücke wie `$rand(1,8)` werden kontrolliert ausgewertet. Beliebige MSL-Ausdrücke aus Datendateien werden nicht ungeprüft ausgeführt.

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

### Regionen, Schlachtfelder und Monster-Vorschauen

- Die Spielwelt besteht aus mehreren Regionen, beispielsweise Dämmerwald, Giftiger Sumpf oder Eisschlucht. Die verfügbaren Regionen werden in einer Regionsdatenbank unter `[Regions]` mit dem Schlüssel `List` geführt.
- Jede Region beschreibt ihre Atmosphäre und enthält die zugehörigen Schlachtfelder, mögliche Rohstoffe sowie eine **regionweite Monsterliste**. Diese Liste beschreibt alle Monsterarten, die irgendwo in der gesamten Region vorkommen können, unabhängig davon, an welchem Schlachtfeld sie sich aufhalten. Sie dient unter anderem als Grundlage für allgemeine Regionsinformationen, beispielsweise auf einem schwarzen Brett in einer Stadt: Dort können die Beschreibung der Region, auffindbare Rohstoffe und die in der Region bekannten Gefahren beziehungsweise Monsterarten angezeigt werden.
- Jedes Schlachtfeld besitzt eine eigene, separate Monsterliste, die ausschließlich festlegt, welche Monsterarten an genau diesem Ort vorkommen. Diese Liste muss immer eine Teilmenge der Monsterliste der übergeordneten Region sein. Ein Monster darf also nur einem Schlachtfeld zugeordnet werden, wenn es auch in der regionalen Monsterliste vorhanden ist. Eine Region kann beispielsweise Schattenwolf, Riesenspinne, Waldgoblin und Verwaister Zombie enthalten, während der Waldweg nur Schattenwolf und Riesenspinne und eine dunkle Höhle nur Riesenspinne und Waldgoblin enthält. Diese Namen sind Beispiele, keine endgültige Inhaltsfestlegung. Schlachtfelder können außerdem eigene Rohstofflisten besitzen; die endgültige Datenstruktur wird beim Entwurf des Datenmodells festgelegt.
- In der Charakterdatei werden unter `[Info]` die aktuelle Region und das aktuelle Schlachtfeld gespeichert, vorgesehen sind die Schlüssel `CurrentRegion` und `CurrentBattlefield`.
- **Feste Reisewege:** Regionen sind nicht frei und unmittelbar von überall aus erreichbar. Jede Region darf nur über ausdrücklich festgelegte Verbindungen betreten werden. `!reise <Regionsname>` bleibt der allgemeine Reisebefehl, führt aber nur dann zu einem Regionswechsel, wenn zwischen der aktuellen Region und dem Ziel eine gültige direkte Verbindung besteht. Ein gültiges Regionsziel ohne direkte Verbindung führt zu einer verständlichen Fehlermeldung; der Standort und vorhandene Monster-Vorschauen bleiben unverändert. Die Verbindungen werden als Weltkartendaten gepflegt und nicht fest in die Befehlslogik eingebaut. **Neue Reisewege werden erst sichtbar, nachdem sie entdeckt wurden.** Die Entdeckung neuer Reisewege kombiniert eigenständige Erkundung, Hinweise durch Reisende oder Aufgaben sowie besondere Ereignisse, etwa Kämpfe oder erreichte Ziele. Es gibt zwei Arten von Hinweisen: **Allgemeine Hinweise** erhöhen die Chance, bei einer Erkundung irgendeinen neuen Reiseweg zu entdecken; welcher Weg es ist, bleibt dabei eine Überraschung. **Konkrete Hinweise** geben Anhaltspunkte für einen bestimmten Reiseweg und ermöglichen es, gezielt nach diesem Weg zu suchen. Beide Hinweisarten können parallel vorkommen. Die konkreten Auslöser, Wahrscheinlichkeiten und Belohnungen werden später ausgearbeitet. Entdeckte Reisewege werden persönlich pro Charakter gespeichert: Jeder Charakter muss einen Reiseweg selbst entdecken; die Entdeckung durch einen anderen Charakter schaltet ihn nicht automatisch frei.\n- **Erkundung in der ersten Version:** Die Erkundung wird über einen eigenen, noch zu benennenden Befehl gestartet und liefert sofort ein Ergebnis. Mögliche Ergebnisse können Hinweise auf Reisewege, Ressourcenfunde, Monsterbegegnungen oder die Entdeckung eines neuen Reisewegs sein. Die konkreten Ereignisse, Wahrscheinlichkeiten und Regeln werden später entworfen. Das System soll so aufgebaut werden, dass es später ohne grundlegenden Umbau um längere Expeditionen erweitert werden kann.\n- **Spätere Erweiterung:** Längere Expeditionen sollen aus mehreren möglichen Ereignissen bestehen. Beim Abschluss einer Expedition kann ein neuer Reiseweg freigeschaltet werden. Diese Funktion gehört nicht zum ersten Prototypen.\n- **Weltkarte und Reiseübersicht:** Die In-Game-Karte wird über `!map` aufgerufen und bietet eine globale Übersicht der Regionen und ihrer Schlachtfelder. Für jede Region zeigt sie ausschließlich die regionweite Monsterliste, also alle Monsterarten, die irgendwo in dieser Region vorkommen können. Die Karte listet ausdrücklich **keine Monsterarten pro einzelnem Schlachtfeld** auf; diese Zuordnung wird getrennt in den Schlachtfelddaten verwaltet und bei der Schlachtfeld-Erkundung beziehungsweise Vorschau berücksichtigt. Die Karte zeigt außerdem keine individuellen Monster-Vorschauwerte und keine konkreten Kampfwerte wie Ziellevel, HP, STR, DEF, INT oder SPD. Sie ist eine allgemeine Orientierungshilfe für die Welt, nicht die Vorschau eines einzelnen Spielenden.
- **Regionswechsel:** Für den Wechsel in eine andere Region wird der allgemeine Reisebefehl `!reise <Regionsname>` verwendet. Es wird kein zusätzlicher oder abweichender Regionsbefehl eingeführt, nur weil der Spieler zuvor die Karte aufgerufen hat. Der Spieler muss für einen Regionswechsel nicht erst in eine Stadt zurückkehren. Existiert die angegebene Region nicht in der Regionsdatenbank, wird eine Fehlermeldung ausgegeben und kein Vorschauwert verändert. Ist das Ziel dieselbe Region wie `CurrentRegion`, bleibt der Spieler dort; vorhandene Vorschauen bleiben erhalten und werden nicht neu berechnet. Bei einem Wechsel in eine andere gültige Region werden die alten regionalen Vorschauen entfernt, `CurrentRegion` aktualisiert, `CurrentBattlefield` geleert oder auf einen gültigen Ort der neuen Region gesetzt und neue Monster-Vorschauen anhand der aktuellen Siegesserie erzeugt.
- **Schlachtfeldwechsel:** Der Befehl `!schlachtfeld <Name>` wechselt zu einem Schlachtfeld innerhalb der aktuellen Region. Das System prüft, ob der angegebene Ort tatsächlich in der Schlachtfeldliste dieser Region enthalten ist. Ist das Ziel dort nicht vorhanden, findet kein Wechsel statt und der aktuelle Standort sowie seine Vorschauen bleiben unverändert. Die Fehlermeldung lautet: `Dieses Schlachtfeld befindet sich nicht in deiner aktuellen Region. Wirf einen Blick auf deine Karte, um den richtigen Weg zu finden.` Ein erfolgreicher Schlachtfeldwechsel aktualisiert `CurrentBattlefield` und erzeugt Vorschauen ausschließlich für die Monster des neuen Schlachtfelds. `BattleStreak` bleibt dabei unverändert.
- Reisebefehle werden in drei Fällen geprüft: (A) Existiert das Ziel nicht in der Regionsliste der Datenbank, wird eine Fehlermeldung ausgegeben und es werden keinerlei Vorschauwerte verändert. (B) Ist das Ziel dieselbe Region wie `CurrentRegion`, bleibt der Spieler dort; vorhandene Vorschauen bleiben erhalten und werden durch den Reisebefehl nicht neu berechnet. (C) Ist das Ziel eine gültige andere Region, findet ein Regionswechsel statt: Die alten regionalen Vorschauen werden entfernt, `CurrentRegion` wird aktualisiert, `CurrentBattlefield` wird geleert oder auf einen gültigen Ort der neuen Region gesetzt, und die Monster-Vorschauen der neuen Region werden neu erzeugt.
- Die Monster-Vorschau wird getrennt von der aktiven Kampfinstanz verwaltet und bezieht sich auf den aktuellen Standort: Es werden Vorschauen ausschließlich für die Monster erzeugt, die in der Monsterliste des aktuellen Schlachtfelds stehen. Die allgemeine Monsterliste der Region allein berechtigt nicht dazu, ein Monster an jedem Schlachtfeld anzuzeigen oder anzugreifen. Für jedes Monster des aktuellen Schlachtfelds werden Vorschauwerte gespeichert: Ziellevel, HP, STR, DEF, INT, SPD und verfügbare Techniken. Die Vorschau kann für grobe Informationen und eine genauere Untersuchung verwendet werden. Sie ist noch kein aktiver Kampf und erzeugt noch keine kampfspezifische Monsterdatei.
- Der Spieler entscheidet freiwillig, ob er eines der am aktuellen Schlachtfeld angezeigten Monster angreift, beispielsweise mit `!kampf Schattenwolf`. Vor dem Start prüft das System, ob das angegebene Monster in der Monsterliste des aktuellen Schlachtfelds enthalten ist. Ist es dort nicht vorhanden, wird eine verständliche Fehlermeldung ausgegeben und kein Kampf gestartet, auch wenn das Monster grundsätzlich in der regionalen Monsterliste vorkommt. Erst bei einem gültigen Ziel kopiert das System die unveränderte Originaldefinition des gewählten Monsters aus `/monsters/` in die persönliche Kampfinstanz und übernimmt die bereits gespeicherten Vorschauwerte. Andere statische Daten der Vorlage, etwa Techniklevel und weitere Monsterangaben, bleiben erhalten, sofern sie nicht ausdrücklich durch Kampfregeln geändert werden. Es wird für denselben Vorschaugegner nicht erneut unabhängig gewürfelt, sodass die angezeigten Werte und die Werte zu Kampfbeginn übereinstimmen.
- Die regionale Vorschau bleibt bestehen, solange der Spieler in derselben Region bleibt. Das Erkunden allein löst keine Neuberechnung bereits vorhandener Vorschauen aus. Nach einem Sieg wird die Siegesserie erhöht und die Vorschauen der aktuellen Region werden nach den neuen Regeln aktualisiert. Nach einer Niederlage wird `BattleStreak=0` gesetzt und die Vorschauen der aktuellen Region werden anhand dieser Siegesserie neu berechnet. Bei `BattleStreak=0` beträgt die Zielstufe neu erzeugter Monster-Vorschauen zunächst Level 1, nicht Level 0. Flucht und technische Kampfabbrüche setzen die Siegesserie gemäß den bereits beschlossenen Regeln ebenfalls auf 0; ihre Vorschauaktualisierung muss dieselbe konsistente Regel verwenden.
- Die allgemeine Siegesserie wird in der Charakterdatei unter `[Info]` im Schlüssel `BattleStreak` gespeichert. Sie beeinflusst die Zielstufe neu erzeugter Vorschauen, unabhängig vom eigenen Charakterlevel. Ein Regionswechsel erzeugt Vorschauen anhand der zum Wechselzeitpunkt gespeicherten Siegesserie. Ein erneuter Reisebefehl in dieselbe Region erzeugt keine neuen Vorschauen.
- Beim regulären Kampfabschluss wird nur die aktive Kampfinstanz einschließlich ihrer kampfspezifischen Kopien und temporären Kampfdaten nach erfolgreicher Speicherung und Prüfung bereinigt. Die Regionsverwaltung und ihre Vorschauen werden nicht zusammen mit der aktiven Kampfinstanz gelöscht. Nach Sieg oder Niederlage werden die Vorschauen gemäß den obigen Regeln aktualisiert. Dadurch kann der Spieler in derselben Region weitere Kämpfe beginnen, auch nachdem die vorherige Kampfinstanz gelöscht wurde.
- Vorschauwerte und aktive Kampfwerte müssen getrennt gespeichert und als unterschiedliche Zustände behandelt werden. Bei einem Neustart oder technischen Abbruch darf eine Vorschau nicht fälschlich als laufender Kampf wiederaufgenommen werden. Die konkrete Aufteilung auf Dateien und Verzeichnisse wird beim Entwurf des Datenmodells festgelegt.

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
- Die allgemeine Siegesserie des Spielenden beeinflusst die Zielstärke neu erzeugter Monster, unabhängig von deren Art; das eigene Spielerlevel bestimmt diese Zielstärke nicht. Bei `BattleStreak=0` beginnt die Begegnung mit Zielstufe `1`; nach einem Sieg soll die nächste Zielstufe zunächst beispielhaft um eine Stufe steigen. Die Monsterattribute HP, STR, DEF, INT und SPD werden anhand einer noch zu entwickelnden Skalierungsformel erzeugt. `$get.level($1)` soll auch auf Monster angewendet werden; Zielstufe und aus den Attributen berechnetes Level müssen sauber abgeglichen werden, ohne unkontrollierte Rückkopplung. Die genaue Formel bleibt offen. Die Siegesserie wird im Charakterspielstand unter `[Info]` mit `BattleStreak` gespeichert. Jeder Sieg erhöht sie um `1`; Niederlage, Flucht und Verbindungsabbruch während eines Kampfes setzen sie auf `0`. Eine Flucht gilt als indirekte Niederlage.
- Die konkrete Formel ist noch offen und wird später mit nachvollziehbaren Beispielen entwickelt und getestet.
- Bei der Gestaltung soll verhindert werden, dass eine unbegrenzte Steigerung der Schwierigkeit entsteht. Welche Auswirkungen Siege genau haben, ist noch nicht entschieden.

### Shop-Level und Preisberechnung

- Der Charakter erhält Lebenspunkte und andere Werte nicht automatisch allein durch einen Stufenaufstieg; die genaue Regel für Lebenspunkte bleibt noch offen. Die Charakterentwicklung soll insbesondere über Belohnungen und gezielte Käufe erfolgen.
- Auron werden durch gewöhnliche Monsterkämpfe verdient. Ätherkristalle werden ausschließlich durch Bosskämpfe vergeben, beispielsweise in Boss-Instanzen; gewöhnliche Monsterkämpfe gewähren keine Ätherkristalle.
- Die Auron-Belohnung eines gewöhnlichen Monsterkampfs soll von der Effektivität und Abwechslung des Kampfstils abhängen. Während des Kampfs wird dafür eine sogenannte Stil-XP erfasst: Die wiederholte Nutzung derselben Fähigkeit soll sie verringern, während der abwechslungsreiche Einsatz verschiedener Aktionen – etwa Angriffe, Techniken und Gegenstände – sie erhöhen kann.
- Bei einer normalen Niederlage soll der Spielende eine anteilige Auron-Belohnung für seine Mühen erhalten; sie ist geringer als die volle Belohnung für einen Sieg. Die genaue Berechnung und das Zusammenspiel mit Stil-XP bleiben offen. Bei einer bewussten Flucht mit dem Befehl `!fliehen` während des laufenden Spielbetriebs erhält der Spielende keine Auron-Belohnung. `BattleStreak` wird auf `0` gesetzt, die Kampfwerte werden aus `[BaseStats]` wiederhergestellt und die Kampfinstanz wird erst nach erfolgreicher Speicherung und Prüfung bereinigt. Als kurze, einheitliche IRC-Meldung wird ausgegeben: „Du ziehst dich aus dem Kampf zurück und verlierst die Gelegenheit auf eine Belohnung.“ Die technische Abwicklung folgt dem abgesicherten Kampfabschluss: Der Bot kennzeichnet die Flucht dauerhaft als in Bearbeitung, speichert `BattleStreak=0` und stellt `[Battle]` aus `[BaseStats]` wieder her. Danach prüft er die gespeicherten Werte. Erst wenn Speicherung und Prüfung erfolgreich waren, wird die Kampfinstanz bereinigt. Bei einem Fehler bleiben die für einen erneuten Versuch benötigten Kampfdaten erhalten. Ein erneuter Versuch darf weder eine Auron-Belohnung vergeben noch den Vorgang doppelt abschließen. Technische Abbrüche durch Verbindungsverlust, Bot-Absturz oder geplanten Bot-Neustart folgen der gesonderten Regel für technische Abbrüche und gewähren ebenfalls keine Belohnung.
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
- **Maximal eine aktive Kampfinstanz pro Spieler:** Diese Regel gilt projektweit. Ein Spieler kann nicht gleichzeitig in mehreren Kampfinstanzen kämpfen. Solange für ihn eine Kampfinstanz aktiv oder noch nicht sicher abgeschlossen und bereinigt ist, darf kein weiterer Kampf für ihn initialisiert werden. Das schützt vor Konflikten zwischen Spielerattributen, temporären Kampfwerten und Belohnungen.
- **Kampf-ID als Sperrmerkmal für spielerbezogene Befehle:** Die Spielerdatei speichert unter `[Info]` die zugehörige Kampfinstanz-ID, beispielsweise `BattleID=Kampf_00042`. Da jeder Spieler höchstens einer eigenen Kampfinstanz zugeordnet sein darf, prüft ein gesperrter Befehl wie der Shop zunächst nur dieses Feld: Ist `BattleID` nicht leer, wird der Befehl verweigert. Ist `BattleID` leer, darf der Befehl nach den übrigen regulären Prüfungen fortfahren. Für diese spielerbezogene Sperre ist kein zusätzlicher `IsOpen`-Schlüssel in `battle.ini` erforderlich. Eine allgemeine Kennzeichnung des offenen/geschlossenen Zustands könnte später für andere Kampfarchitekturen, etwa eine gemeinsame Kampfinstanz, erneut geprüft werden; sie ist nicht Teil der aktuellen Lösung.
- **Kampf-ID und abgebrochene Instanzen:** Bei einem regulären Kampfende darf `BattleID` erst geleert werden, nachdem Belohnungen, Siegesserie und zurückgesetzte Spielerwerte dauerhaft gespeichert und überprüft wurden. Bei einem Verbindungsabbruch eines Spielers sowie bei einem Bot-Absturz oder geplanten Bot-Neustart gelten alle laufenden Kampfinstanzen dagegen als verloren. Sie werden niemals fortgesetzt und gewähren keine Belohnung. `BattleStreak` wird auf `0` gesetzt und `[Battle]` wird aus `[BaseStats]` wiederhergestellt. Nach erfolgreicher Speicherung und Überprüfung werden `BattleID`, `battle.ini`, Monsterkopie und alle weiteren temporären Daten der Instanz gelöscht. Beim Start muss der Bot übrig gebliebene Instanzen des vorherigen Laufs als abgebrochen erkennen und ausschließlich diese verlustfreie Bereinigung ausführen; eine nachträgliche Belohnungsauszahlung ist ausgeschlossen. Schlägt die Bereinigung fehl, muss sie ohne Belohnung wiederholbar bleiben.
- **Mehrere Monster innerhalb einer Kampfinstanz:** Die Beschränkung auf eine Kampfinstanz bedeutet nicht, dass ein Kampf dauerhaft auf ein einziges Monster begrenzt ist. Später können Fähigkeiten weitere Monster in dieselbe Kampfinstanz rufen, sodass ein Spieler innerhalb eines Kampfes gegen mehrere Monster kämpfen kann. Das ist eine zukünftige Erweiterung und gehört nicht zum ersten Prototypen.
- **Begegnung und Kampfstart:** Als zukünftiger Ablauf ist vorgesehen, dass Spieler Monster in einer Region zunächst entdecken und Informationen zu ihnen abrufen können. Der eigentliche Kampf wird anschließend ausdrücklich durch einen Befehl initialisiert. Die genaue Befehlsstruktur und das Begegnungssystem werden später geplant.
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
- Die Erkundung liefert in der ersten Version sofort ein Ergebnis und wird über einen eigenen Befehl gestartet; dessen Name und genaue Mechanik sind noch offen. Das System wird für spätere längere Expeditionen mit mehreren Ereignissen vorbereitet, bei denen beim Abschluss ein Reiseweg freigeschaltet werden kann.\n- Die Welt verwendet feste Reisewege: Ein Regionswechsel ist nur über eine ausdrücklich definierte direkte Verbindung zwischen aktueller Region und Zielregion möglich. `!reise <Regionsname>` bleibt der allgemeine Reisebefehl; bei fehlender Verbindung wird nicht gereist und der aktuelle Standort samt Vorschauen bleibt unverändert. **Neue Reisewege werden erst nach ihrer Entdeckung auf der Karte sichtbar.** Die Entdeckung neuer Reisewege kombiniert eigenständige Erkundung, Hinweise durch Reisende oder Aufgaben sowie besondere Ereignisse, etwa Kämpfe oder erreichte Ziele. Die konkreten Auslöser und Belohnungen bleiben offen. Entdeckte Reisewege werden persönlich pro Charakter gespeichert: Jeder Charakter muss einen Reiseweg selbst entdecken; die Entdeckung durch einen anderen Charakter schaltet ihn nicht automatisch frei.\n- Das Projekt und alle von uns verfassten Texte werden auf Deutsch geführt.
- Das Spiel ist von Anfang an ein Mehrspieler-Rollenspiel.
- Jeder Spielaccount bleibt dauerhaft bestehen und ist an den IRC-Nicknamen gebunden; ein Nicknamenwechsel führt nicht zur automatischen Wiedererkennung des bisherigen Accounts.
- Die Kernattribute STR, DEF, INT und SPD gelten für Spieler und Monster.
- Die Charakterstufe ergibt sich aus einer Berechnung der vier Kernattribute; ein fester EP-Wert löst keinen automatischen Stufenaufstieg aus. Spielende stärken ihre Charaktere selbst durch Käufe mit verdienten Auron. Die genaue Stufenformel bleibt offen.
- Die zentrale Funktion `$get.level($1)` berechnet bei jedem Aufruf das aktuelle Level anhand der aktuellen `[Battle]`-Werte und gibt es zurück, ohne das Level separat zu speichern. Buffs und Debuffs können dadurch das berechnete Level vorübergehend beeinflussen. Die genaue Levelkurve und `StatWeight`-Formel bleiben offen.
- Die grundsätzliche Zielstärke beziehungsweise das Ziellevel eines neu erzeugten Monsters richtet sich nach der allgemeinen Siegesserie des Spielenden, nicht nach dessen eigenem Level. Bei `BattleStreak=0` beginnt die Begegnung mit einem Monster der Zielstufe `1`; nach einem Sieg steigt die Siegesserie auf `1`, und das nächste Monster soll beispielhaft die Zielstufe `2` erhalten. Höhere Siegesserien können somit auch dann zu stärkeren Monstern führen, wenn der Spielende seine eigenen Attribute nicht verbessert hat. Die konkreten Werte für HP, STR, DEF, INT und SPD werden über eine noch zu entwickelnde und zu testende Skalierungsformel erzeugt. `$get.level($1)` soll auch für Monster verwendet werden, um deren tatsächliches Level anhand ihrer Attribute zu berechnen beziehungsweise zu prüfen; Zielstufe und berechnetes Level dürfen dabei keine unkontrollierte Rückkopplung der Werte verursachen. Die genauen Formeln und Grenzen bleiben offen. Die Siegesserie wird unabhängig von der Monsterart unter `[Info]` im Schlüssel `BattleStreak` gespeichert: Jeder Sieg erhöht sie um `1`; eine normale Niederlage oder bewusste Flucht setzt sie auf `0`. Eine Flucht bei laufendem System folgt den normalen Fluchtregeln und gilt als indirekte Niederlage. Verlässt der Spielende während eines Kampfes den IRC-Server oder bricht die Verbindung ab, gilt der Kampf ohne Belohnung als abgebrochen. Dasselbe gilt bei Bot-Absturz oder geplantem Bot-Neustart. In allen technischen Abbruchfällen wird `BattleStreak` auf `0` gesetzt, der Kampf wird nach erneuter Verbindung beziehungsweise Bot-Start niemals fortgesetzt und die Instanz wird ohne Belohnung bereinigt.
- Spielwerte sollen über strukturierte Datendateien verwaltet werden; INI-Dateien und `$readini` sind als Ansatz vorgesehen.
- Techniken werden extern in einer Technik-Datenbank im INI-Stil definiert und über Waffenzuordnungen verfügbar gemacht; für den ersten Prototypen ist der Typ `SINGLE` vorgesehen. Technikdaten wie Trefferzahl, Grundstärke, Attribut und Beschreibung werden nicht fest im Quellcode hinterlegt.
- Waffen werden extern in `weapons.db` definiert; Kategorien, Anzeigenamen, Waffenwerte, verfügbare Techniken und Ätherkristall-Aufwertungskosten stehen in der Datei statt im Quellcode.
- Monsterdefinitionen werden als Dateien im Verzeichnis `/monsters/` gespeichert und sind ähnlich wie Charakterdateien aufgebaut. `/monsters/to_make/new_mon.char` ist nur die allgemeine Vorlage zur Erstellung neuer Monsterdefinitionen; sie wird nicht direkt als laufendes Monster verwendet. Eine daraus erstellte Datei, zum Beispiel `/monsters/Schattenwolf.char`, enthält die Grundwerte des jeweiligen Monsters. Bei der Erzeugung eines Kampfmonsters wird diese Definition in den Unterordner der zugehörigen Kampfinstanz kopiert; nur die Kopie wird verändert. Das Original bleibt unberührt.
- Monsterdateien verwenden `[Info]` mit `Flag=monster`, damit das System sie von Spielerdaten unterscheiden kann. `[BaseStats]` enthält die Grundwerte und `[Battle]` die veränderlichen Kampfwerte. Der Schlüssel `Status` in `[Battle]` hat drei festgelegte Zustände: `Alive` bedeutet, dass die Monster-KI ihre Züge ausführt; `Inactive` bedeutet, dass das Monster passiv/neutral ist, seine KI-Züge überspringt und erst nach erlittenem Schaden zu `Alive` wechselt; `Dead` bedeutet, dass das Monster besiegt ist und nach Abschluss aller Kampfabschlussaktionen bei der Bereinigung entfernt werden kann. `[Descriptions]` enthält Erscheinungs- und Todesmeldungen; zusätzlich ist `DescInactive` als optionaler Schlüssel für eine monsterspezifische Meldung beim Überspringen eines Zuges vorgesehen. Fehlt `DescInactive`, soll eine allgemeine Systemmeldung verwendet werden. `[Stuff]` enthält unter anderem die Auron-Belohnung, `[monster]` die Monsterart beziehungsweise Rasse und `[Weapons]` sowie `[Techniques]` die Monsterwaffe und dazu passende Techniken. Monsterwaffen und Techniken müssen in den jeweiligen externen Datenbanken definiert sein. Die Bereinigung darf eine Monsterdatei oder Kampfinstanz nicht entfernen, bevor Belohnungen, Kampfmeldungen und sonstige Abschlussaktionen abgeschlossen sind.
- **Belohnung im Prototypen:** Der Schlüssel `Auron` im Abschnitt `[Stuff]` der Monsterdatei enthält entweder einen festen Zahlenwert oder einen unterstützten Zufallsausdruck, zum Beispiel `Auron=10` oder `Auron=$rand(1,15)`. Separate Schlüssel wie `AuronMin` und `AuronMax` sind nicht erforderlich. Beim Erzeugen der Kampfinstanz wird dieser Eintrag einmal ausgewertet und der konkrete Zahlenwert in `battle.ini` unter `[Rewards]` mit `Auron=<ermittelter Wert>` gespeichert. Die Monsterdatei bleibt damit die Quelle der Belohnungsvorgabe; `battle.ini` hält den für diesen Kampf festgelegten Wert dauerhaft fest. Beim Kampfabschluss und bei Wiederholungsversuchen wird ausschließlich der gespeicherte Wert aus `[Rewards]` verwendet, nicht erneut der Ausdruck aus `[Stuff]` ausgewertet. Dadurch bleibt die Belohnung auch nach einem Bot-Absturz gleich. Zusätzlich muss die Auszahlung gegen doppelte Gutschrift abgesichert sein: Das dauerhafte Speichern des Belohnungswerts verhindert ein erneutes Auswürfeln, ersetzt aber nicht die separate Absicherung gegen eine doppelte Auszahlung. Bei einer Niederlage bleibt eine geringere Teilbelohnung vorgesehen; deren Berechnung ist noch offen. Im Prototypen erhöhen Stil-XP, Monsterlevel oder weitere Faktoren die Belohnung noch nicht zusätzlich. Solche Einflüsse sind für spätere Ausbaustufen vorgesehen.
- Jeder Kampfabschluss muss gegen doppelte Verarbeitung abgesichert werden. Dass ein Monster `Status=Dead` hat oder die Abschlussroutine erneut ausgelöst wird, darf niemals allein eine weitere Belohnung auslösen. Die Kampfinstanz erhält in `battle.ini` einen dauerhaft gespeicherten Abschlussstatus, beispielsweise `Open`, `Processing`, `Completed` und `Cleanup`; die genaue Zustandsmaschine wird bei der Implementierung festgelegt. Vor einer Gutschrift muss geprüft werden, ob die Belohnung dieses eindeutigen Kampfes bereits verbucht wurde. Der Schutz darf sich nicht allein auf eine Variable im Arbeitsspeicher verlassen. Unterbrechungen und Neustarts müssen beim späteren Entwurf berücksichtigt werden, damit ein erneuter Abschlussversuch keine doppelten Auronen gutschreibt. Die sichere Wiederaufnahme des Ablaufs und die Reihenfolge der Speicherung sind Teil des Entwurfs der Kampfabschlussfunktion.
- Eine Kampfinstanz darf erst gelöscht werden, wenn der Kampfabschluss vollständig durchgeführt und die dauerhaften Spielerdaten erfolgreich gespeichert und überprüft wurden. Die Reihenfolge für den aktuellen Prototypen lautet: (1) Sieg oder Niederlage feststellen und Kampfabschluss vorbereiten, (2) Auron-Belohnung berechnen und gutschreiben, (3) `BattleStreak` abhängig vom Ergebnis erhöhen oder zurücksetzen und speichern, (4) die Spielerwerte in `[Battle]` aus den dauerhaften Werten in `[BaseStats]` wiederherstellen, (5) die gespeicherten Auronen, die Siegesserie und die zurückgesetzten Kampfwerte auf Richtigkeit prüfen und erst danach (6) das Verzeichnis der Kampfinstanz samt kampfspezifischen Dateien löschen. Schlägt eine Speicherung oder Prüfung fehl, bleibt die Kampfinstanz erhalten, damit die Bereinigung erneut versucht werden kann, ohne den Kampfzustand voreilig zu verlieren. Das Monster kann bereits `Status=Dead` haben; dieser Status allein berechtigt nicht zum Löschen der Kampfinstanz.
- Die Grundwerte der Monsterdefinition entsprechen der Basis für ein Monster der Zielstufe `1`. Bei `BattleStreak=0` muss das erste erzeugte Monster Zielstufe `1` haben; die Erzeugung darf nicht allein durch die anfänglichen Attribute versehentlich ein Monster der Stufe `5`, `10` oder `20` ergeben. Die genaue Attributskalierung für höhere Zielstufen bleibt offen und wird mit `$get.level($1)` getestet.
- Die Standardwaffe jedes neuen Charakters sind nicht kaufbare und nicht ablegbare Fäuste (`Fists`) mit der Standardtechnik `DoublePunch`. Ausgerüstete Waffe sowie Waffen- und Techniklevel werden individuell in der Charakterdatei unter `[Weapons]` und `[Techs]` gespeichert.
- Der Kauf einer Waffe rüstet sie nicht automatisch aus; zum Ausrüsten ist der Befehl `!equip weapon waffename` vorgesehen. Ein mögliches Mindestlevel für Waffen ist noch offen.
- In der kaufbaren Technikliste dürfen nur Techniken erscheinen, die zur aktuell ausgerüsteten Waffe gehören. Die Auswahl wird anhand des Charakterspielstands und der Zuordnung in `weapons.db` gefiltert.
- Die Anfangslebenspunkte sind noch nicht festgelegt; die maximale durch Käufe erreichbare Lebenspunktzahl beträgt 5.000 HP.
- Die vier Kernattribute haben keine festen absoluten Obergrenzen. Eine noch zu entwickelnde Verhältnisregel soll einseitige Verteilungen verhindern, ohne die freie Schwerpunktwahl vollständig aufzuheben.
- Als vorläufige Architekturentscheidung erhält jeder Spielende eine eigene Kampfinstanz. Gruppen mit gemeinsamer Kampfinstanz können später ergänzt werden. Ob die Einzelinstanzen unter gleichzeitiger Last flüssig laufen, wird im Prototyp getestet; bei Leistungsproblemen wird die Architektur anhand der Testergebnisse neu bewertet.
- **Timeout-Regel für die eigene Kampfinstanz:** Für jeden Spielerzug gilt ein Zeitlimit von `180` Sekunden. Reagiert der Spielende innerhalb dieser Zeit nicht, wird sein Zug übersprungen und die Monster-KI führt ihren Zug wie gewohnt aus. Danach beginnt für den nächsten Spielerzug erneut ein Zeitlimit von `180` Sekunden. Wiederholte Timeouts entfernen den Spielenden nicht aus der Kampfinstanz und brechen den Kampf nicht automatisch ab. Der Kampf läuft weiter, sodass das Monster bei jedem Timeout erneut handeln kann, bis der Spielende wieder reagiert oder der Kampf regulär mit Sieg beziehungsweise Niederlage endet. Eine Niederlage durch Inaktivität folgt den normalen Niederlagenregeln, einschließlich der vorgesehenen anteiligen Auron-Belohnung; ein Timeout allein gewährt oder verweigert keine Belohnung.
- Jede Kampfinstanz erhält eine eindeutige interne ID und einen eigenen Unterordner unter `/battles/`. Dort liegen die veränderlichen kampfspezifischen Daten, darunter eine Kopie des erzeugten Monsters und eine Datei für den Kampfzustand. Dauerhafte Spielerdaten verbleiben unter `/characters/`; Monsterdefinitionen unter `/monsters/` bleiben unverändert. Die Zahl gleichzeitig laufender Einzelkämpfe und mögliche Verzögerungen werden im Prototyp getestet. Erst bei nachgewiesenen Leistungsproblemen wird die Architektur neu bewertet; Gruppenkämpfe können später als gemeinsame Kampfinstanz ergänzt werden.

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
- Die allgemeine Siegesserie jedes Spielenden soll bereits im Prototyp erfasst werden. Sie wird in der Charakterdatei unter `[Info]` als `BattleStreak` gespeichert, startet bei `0`, steigt bei jedem Sieg unabhängig von der Monsterart um `1` und wird bei einer normalen Niederlage oder bewussten Flucht auf `0` zurückgesetzt. Bei einer Flucht im laufenden System gelten die normalen Fluchtregeln. Ein Verbindungsabbruch, Bot-Absturz oder geplanter Bot-Neustart bricht den Kampf ohne Belohnung ab und setzt `BattleStreak` ebenfalls auf `0`. Sie bestimmt die Zielstärke neu erzeugter Monster: bei Siegesserie `0` ist die erste Zielstufe `1`; nach jedem Sieg soll die nächste Zielstufe zunächst beispielhaft um eine Stufe steigen. Die konkrete Skalierung von HP, STR, DEF, INT und SPD sowie die Abgleichung mit `$get.level($1)` bleiben offen und werden getestet. Spielende sollen auch bei einer Niederlage eine anteilige Auron-Belohnung für ihre Mühen erhalten; sie fällt geringer als die volle Belohnung für einen Sieg aus. Die genaue Berechnung bleibt offen.