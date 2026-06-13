# SkiFlug OÖ – Sprachaufnahmen selbst einsprechen

So funktioniert's:

1. Sprich die Sätze unten ein (Handy-Sprachmemo, Audacity, was du willst).
2. Exportiere als **MP3**.
3. Lege sie in den Ordner `audio/<key>/` mit dem Namen `<key>-01.mp3`, `<key>-02.mp3` …
   Beispiel: die erste „Start"-Aufnahme → `audio/start/start-01.mp3`.
4. Trage in `index.html` bei `AUDIO_MANIFEST` ein, **wie viele** Varianten du je Aktion hast
   (z. B. `start:8`). Mehr Varianten = mehr Abwechslung, das Spiel würfelt pro Aktion.
5. Im Spiel unter Einstellungen **„Eigene Sprachaufnahmen (Ordner audio/)"** anhaken.

Fehlt eine Datei oder ist der Haken aus, nimmt das Spiel automatisch die Gerätestimme.
Du musst nicht alle Stellen aufnehmen – nur die, die du willst. Tipp: kurze, energiegeladene
Ansagen (1–2 Sekunden) wirken am besten. Es dürfen ruhig mehr Varianten sein als hier gelistet.

---

## KOMMENTATOR (Andi-Stil: laut, euphorisch, oberösterreichischer Dialekt)

### `start/` — Anfahrt beginnt (Ordner `audio/start/`)
1. So, auf geht's, owe vom Balken — passts auf!
2. Er kummt, er kummt, jetzt wird's ernst!
3. Und ab in die Spur, hui geht des dahin!
4. Servas, jetzt schau ma amoi wos do geht!
5. Achtung, jetzt löst er aus — owe geht's!
6. Konzentration am Balken … und er fahrt!
7. Jetzt gilt's — auf in die Anfahrt!
8. Stad is's worden im Stadion … und er kummt!

### `takeoffPerfect/` — perfekter Absprung
1. Heast, wos für a Absprung — bombastisch!
2. Sauba owe vom Tisch, des is a Granate!
3. Perfekt, perfekt, perfekt — Weltklasse, sog i eich!
4. Der Absprung woar a Traum, leiwand!
5. Wia ausm Lehrbuch owe vom Tisch — sensationell!
6. Punktgenau am Schanzentisch — a Wahnsinn!
7. Den Absprung muass ma eahm lassen — bärig!
8. Jawoi! So muass des ausschaun!

### `takeoffGood/` — solider Absprung
1. Guata Absprung, des schaut scho fein aus!
2. Ordentlich am Tisch, des passt eh!
3. Najo, solide owe — do geht no wos!
4. Anständiger Absprung, brauchbar!
5. Passt scho, der hot eh guat ausglöst.
6. Soweit in Ordnung, jetzt fliagn!

### `takeoffPoor/` — verschlafener Absprung
1. Hui, do woar er a Spur zu spät, schod ums Eck!
2. Den Absprung hot er vaschlofn — najo!
3. Geh bitte, do is eahm der Tisch davongrennt!
4. Oje, do woar nix mit'm Timing!
5. Z'spät dran, des kost' eahm Meter!
6. Den hot er valegt, den Absprung — schod!

### `flightHigh/` — schöner, weiter Flug
1. Heast, schau wia der dahinsegelt!
2. Der fliagt und fliagt und fliagt — a Wahnsinn!
3. Der steht in da Luft wia angnagelt, sensationell!
4. Des is a Flug wia aus'm Bilderbuch!
5. Der schwebt do owi, traumhaft schön!
6. Der findt an Auftrieb, des is a Freid!
7. Wia a Adler segelt der ins Tal!
8. Der mocht die Schanze richtig lang — irre!

### `flightLow/` — gedrückter, kurzer Flug
1. Oje, den drückt's owe, des wird kurz!
2. Er findt kan Auftrieb — najo, schod!
3. Do fehlt da Druck unter de Ski, schwierig!
4. Den zaht's owi, des schaut net guat aus!
5. Kein Tragen heut — der kummt net weit.
6. Schod, do is ka Luft unter de Ski!

### `overK/` — über den K-Punkt
1. Über'n K-Punkt — und es geht weiter!
2. Heast is des a Weite, der lasst olle stehn!
3. Wahnsinn, der fliagt am K-Punkt vorbei wia nix!
4. K-Punkt passiert — und no lang net fertig!
5. Über die kritische Marke — bärig weit!
6. Der haut den K-Punkt um Längen weg!
7. Weiter, weiter, über'n K-Punkt aussi!
8. Des is gscheit weit — über'n Kritischen!

### `overHS/` — über die Hill Size (Highlight!)
1. Wahnsinn, über die Hill Size — i wir narrisch!
2. A Monsterweite, sowos hob i selten gsehn!
3. Heast, des is irre — der hört goar nimma auf!
4. Sensationell, des sprengt die Schanze!
5. Über'n Hill-Size-Punkt — des is gschichtsträchtig!
6. Der fliagt ausm Stadion aussi — unglaublich!
7. So weit hot do no kana gflogn — Wahnsinn!
8. I fass es net — über die Hill Size aussi!

### `telemark/` — Telemark-Landung
1. Und der Telemark sitzt — bilderbuchmäßig, bravo!
2. Sauba gstellt, perfekt glandet, traumhaft!
3. Mustergültig owe, do gibt's die Topnoten!
4. Telemark wia gmalt — des gfreut die Richter!
5. Sauber in den Schnee gstellt — perfekt!
6. Bravo, der Telemark passt hundertprozentig!

### `landOk/` — Landung ohne Telemark
1. Glandet is glandet, weiter geht's!
2. Auf beide Ski owe — des zählt amoi!
3. Najo, ka Telemark, aber gstandn is er!
4. Beidbeinig owe — kost' a paar Punkterln.
5. Gstandn hot er, mehr aber a net.
6. Ohne Telemark, aber sicher unten.

### `crash/` — Sturz
1. Oje oje, do hot's eahm zaumghaut!
2. Sturz! Des tuat weh beim bloßen Zuaschaun!
3. Geh, do is er glegn — aufstehn, weiter geht's!
4. Uiuiui, des is goar nia guat ausgangen!
5. Owe ghaut hot's eahm — hoffentlich nix passiert!
6. Des woar nix, do liegt er im Schnee!
7. Sturz im Auslauf — au weh!
8. Najo, des hod si net ausgangen.

### `record/` — Schanzenrekord / Bestweite
1. Schanzenrekord — i wir narrisch do herobn!
2. Bestweite, sensationell, des is ja leiwand!
3. Neue Bestmarke — heast, des feiern ma mit am Most!
4. Rekord — ganz Oberösterreich steht Kopf!
5. So weit woar do no kana — Schanzenrekord!
6. Die Bestweite is gfalln — a historischer Moment!
7. Rekord, Rekord, Rekord — i halt's net aus!
8. Des is die neue Hausmarke — gigantisch!

### `win/` — Bewerbssieg (Karriere)
1. Der Sieg! Owe vom Balken zum Stockerl ganz obn!
2. Gwonnen, gwonnen, gwonnen — wos für a Leistung!
3. Der hot's gholt, der Bewerbssieg geht zu uns!
4. Ganz obn am Stockerl — wia vadient!
5. Sieg in Oberösterreich — heast, des feiern ma!
6. Der Tagessieg is fix — sensationell!

### `podium/` — Podestplatz (Karriere)
1. Aufs Stockerl! A Podestplatz, des passt scho fein!
2. Unter die ersten Drei — bravo, starke Leistung!
3. Podest, des is a Grund zum Feiern!
4. Ganz vorn dabei — aufs Stockerl gsprungen!
5. A Medaille is fix — sauber gmacht!

### `caught/` — beim Anzug-Trick erwischt (Karriere)
1. Oje, do hot da Materialkontrolleur wos gfunden — disqualifiziert!
2. Erwischt! Der Anzug woar z'groß, des gibt a Sperre!
3. Heast, des is jetzt ärgerlich — Disqualifikation und Sperre dazua!
4. Beim Schwindeln erwischt — des kost' an Bewerb!
5. Der Anzug woar z'weit — Disqualifikation, schod!
6. Ertappt! Jetzt is er gsperrt — selber schuld.

---

## CO-KOMMENTATOR (Bertl-Stil: ruhig, sachlich, erklärend) — FUN FACTS

Jeweils ein Fact je Schanze, mehrere Varianten. Ordner z. B. `audio/fact_steyr/`,
Datei `fact_steyr-01.mp3`. (Die Hinweise „Weltcup in Oberösterreich" etc. sagt der
Kommentator separat — du kannst den Fun Fact alleine einsprechen.)

### `fact_schaerding/`
1. Schärding am Inn ist berühmt für seine bunte Barock-Häuserzeile, die Silberzeile.
2. Die Altstadt von Schärding gehörte einst den bayerischen Herzögen — man sieht's an den Fassaden.
3. Schärding ist als Kurort mit der Bäderzeile bekannt.

### `fact_freistadt/`
1. Freistadt im Mühlviertel hat eine fast vollständig erhaltene mittelalterliche Stadtmauer.
2. Das Freistädter Bier wird in einer Bürgerbrauerei gebraut, die der Stadt selbst gehört.
3. Freistadt war im Mittelalter ein wichtiger Handelsplatz an der Salzstraße nach Böhmen.

### `fact_hinzenbach/`
1. In Hinzenbach bei Eferding steht eine der bekanntesten Nachwuchs-Schanzen Österreichs.
2. Eferding zählt zu den ältesten Städten Österreichs und ist fürs Gemüse bekannt.
3. Die Schanze in Hinzenbach ist regelmäßig Schauplatz von Continental-Cup-Bewerben.

### `fact_poestlingberg/`
1. Der Pöstlingberg ist Linz' Hausberg — hinauf fährt eine der steilsten Bergbahnen Europas.
2. Oben thront die Wallfahrtsbasilika, das Wahrzeichen von Linz.
3. Im Berg versteckt sich die Grottenbahn mit ihrem Drachen.

### `fact_steyr/`
1. Steyr gilt als eine der schönsten Eisenstädte und war Zentrum der Eisenverarbeitung.
2. In Steyr liegt Christkindl — von dort verschickt das berühmte Weihnachtspostamt Briefe in alle Welt.
3. Der Stadtplatz von Steyr zählt zu den prächtigsten Plätzen Österreichs.

### `fact_badischl/`
1. Bad Ischl war die kaiserliche Sommerresidenz — die Kaiservilla zog den ganzen Hof ins Salzkammergut.
2. Die berühmte Konditorei Zauner machte Bad Ischl zur süßen Hauptstadt der Monarchie.
3. 2024 war Bad Ischl mit dem Salzkammergut Kulturhauptstadt Europas.

### `fact_windischgarsten/`
1. Windischgarsten liegt mitten im Nationalpark Kalkalpen, einem riesigen Buchen-Urwald.
2. Die Region rund um den Pyhrnpass ist ein altes Pass- und Handelsgebiet.
3. Vom Hochbuchberg hat man einen Traumblick aufs Tote Gebirge.

### `fact_obertraun/`
1. Obertraun liegt am Hallstätter See — der Dachstein mit seiner Eishöhle ragt direkt darüber auf.
2. Die Region Hallstatt-Dachstein ist UNESCO-Welterbe und uralte Salzabbau-Gegend.
3. Auf den Dachstein führt die Krippenstein-Seilbahn hinauf zum berühmten Aussichtssteg.

---

## Übersicht aller Ordner/Keys

| Key | Wann | Varianten in dieser Liste |
|---|---|---|
| start | Anfahrt beginnt | 8 |
| takeoffPerfect | perfekter Absprung | 8 |
| takeoffGood | guter Absprung | 6 |
| takeoffPoor | schlechter Absprung | 6 |
| flightHigh | schöner Flug | 8 |
| flightLow | gedrückter Flug | 6 |
| overK | über K-Punkt | 8 |
| overHS | über Hill Size | 8 |
| telemark | Telemark-Landung | 6 |
| landOk | Landung o. Telemark | 6 |
| crash | Sturz | 8 |
| record | Schanzenrekord | 8 |
| win | Bewerbssieg | 6 |
| podium | Podestplatz | 5 |
| caught | beim Trick erwischt | 6 |
| fact_schaerding … fact_obertraun | Fun Facts je Schanze | je 3 |

Wenn du mehr (oder weniger) Varianten aufnimmst, passe einfach die Zahl in
`AUDIO_MANIFEST` in `index.html` an. Sobald die 10 weiteren Schanzen drin sind,
kommen die passenden `fact_…`-Keys dazu — die liefere ich dir dann nach.
