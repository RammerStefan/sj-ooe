# CLAUDE.md – SkiFlug OÖ

Skisprung-Spiel im Geiste von DSJ2 (Deluxe Ski Jump 2) als **PWA, mobile-first, Browser-only**.
Alle Schanzen tragen Namen oberösterreichischer Orte. Geplante Alleinstellungsmerkmale:
**Karrieremodus** (mehrere Saisonen) und **Schanzen-Editor**. Vollständige Vision, Roadmap
(M1–M7), Datenmodelle und Schanzen-Roster: siehe `KONZEPT.md`.

## Aktueller Stand: Milestone 4 – Karrieremodus

Multi-File-PWA: `index.html` + `manifest.webmanifest` + `sw.js` + Icons.
**Zum echten PWA-Test alle Dateien zusammen hosten** (Netlify Drop / GitHub Pages, HTTPS).
In der Artifact-Vorschau läuft das Spiel; SW/Install/IndexedDB sind dort u. U. deaktiviert (Fallbacks greifen).

Neu in M4 – **Karrieremodus** (Menü → „Karriere"):
- **Springer erstellen:** Name, OÖ-Heimatort, Anzug-/Helmfarbe (färbt den gerenderten Springer via `setSuit`), 5 Attribute per Punktepool verteilen.
- **Attribute beeinflussen die echte Physik** (`career.mod` aus `playerMods()`): Sprungkraft→Absprungimpuls, Flugstil→Lift, Stabilität→Wind-Resistenz, Telemark→Landefenster, Nerven→Streuung am Absprung. Du springst selbst – die KI wird simuliert.
- **Weltcup-Kalender** (`makeCalendar`): ~10 Bewerbe inkl. **OÖ-Tournee** (4 Schanzen) und **Skiflug-Finale Obertraun**. WC-Punkte (100-80-60…) für Top 30.
- **KI-Welt:** ~48 generierte Springer (`genWorld`), Ergebnisse aus Attributen + Schanze + Seed (`aiJump`, via `careersim.js` kalibriert: Mittelfeld erreichbar, Sieg verlangt Top-Sprünge).
- **Training** zwischen Bewerben (Attributpunkte; Übertraining kostet Form).
- **Saisons:** Saisonende mit Gesamtweltcup-Siegerehrung, dann Alterskurve (`ageCurvePeak`), KI-Rücktritte + Nachwuchs (`endSeasonProgression`), neue Saison.
- **Persistenz:** Karriere wird in IndexedDB gespeichert (`saveCareer`/Store), beim Öffnen fortgesetzt.
- **Karriere-Statistik:** Siege, Podeste, Starts, WC-Titel, weitester Sprung, Karrierepunkte (`career.stats`) – im Hub, Saison-Bilanz und über Saisons hinweg.
- **Form-System:** Tagesform schwankt, Training kostet Form, Erholungstag (`restDay`) gibt sie zurück; Form erholt sich zwischen Bewerben/Saisons. Training nur einmal pro Bewerb (`_trainedAt`).
- **Hub** zeigt Karrierebilanz + letztes Ergebnis; Saisonende mit Saison-Bilanz und Titelzählung.
- Panels scrollen auf Touch (`touch-action: pan-y`); alle Eingaben/Buttons sind touch-bedienbar (Fix für nicht erreichbare Buttons/Namensfeld).

Stand M1–M3 bleibt: Physik, 8 OÖ-Schanzen, FIS-Wertung/5 Richter, Touch/Gyro, Replays,
prozeduraler Sound, Kommentator, PWA-Hülle, Persistenz/Export-Import, Mehrspieler lokal & WLAN.

Noch NICHT enthalten (M6/M7): Schanzen-Editor, Karriere-Ökonomie (Sponsoren/Ausrüstung),
Online-Leaderboards/Signaling-Server, Continental-Cup-Unterbau.


## Code-Aufbau in `index.html`

Alles in einem `<script>`, aber bereits nach Zielmodulen gegliedert (Suchanker im Code):

| Abschnitt | Zukünftiges Modul | Inhalt |
|---|---|---|
| `HILL_DEFS` + `buildHill()` | `/editor`, `/modes` | Parametrische Schanzen-Definition → Geometrie, Helfer (`groundAt`, `inrunPos`, Masten, Wald). **Das ist der Keim des Schanzen-Editors.** |
| `PHYS` + `physStep()` | `/physics` | Pure Physik, fixed timestep 120 Hz, deterministisch (mulberry32-RNG, Seed pro Sprung) |
| `game` + Lebenszyklus | `/core` | State-Machine: `menu → inrun → flight → slide → result` |
| `drawXxx()`-Funktionen | `/render` | Canvas-2D-Rendering in voller Auflösung |
| Pointer-/Key-Handler | `/input` | Touch-Drag (relativ), Taps, Rotation-Mapping |
| DOM-Boards | UI | Menü + Ergebnistafel als HTML-Overlays |
| `scoreJump()` + `meterValue()` | `/scoring` | FIS-nahe Weiten- & Haltungspunkte, 5 Richter |
| `Gyro` | `/input` | DeviceOrientation + iOS-Permission, Kalibrierung |
| `replays`/`startReplay()`/`rec` | `/modes` | deterministische Aufzeichnung & Wiedergabe |
| `comp`/`startCompetition`/`compAdvance` | `/modes` | Bewerb lokal + Netz, Standings/Podium |
| `Net` (WebRTC) | `/net` | serverloses WLAN/Online, manuelle Signalisierung |
| `Store`/`snapshot`/`export/import` | `/persist` | IndexedDB + JSON-Export, Fallback |
| `manifest.webmanifest`, `sw.js`, Icons | `/pwa` | Installierbarkeit + Offline-Cache |
| `OOE` | Flair | OÖ-Namens-/Ortsgenerator |
| `career`/`playerMods`/`aiJump` | `/career` | Attribute→Physik, KI-Sim, Kalender, Saisons |
| `setSuit`/`shade` | `/render` | Springer-Farben (Anzug/Helm) aus dem Profil |

### Koordinaten-Konventionen (WICHTIG – häufigste Fehlerquelle!)
- **Weltkoordinaten:** Meter, y nach **oben**, Ursprung = Schanzentisch-Kante. Springer fliegt in +x.
- **Canvas:** y nach **unten**. Umrechnung nur über `wx()/wy()`. Beim Zeichnen von Figuren in
  lokalen Frames gilt: „oben" = negatives y. (Ein Vorzeichenfehler hier hat den Springer
  bereits einmal kopfüber fliegen lassen.)
- **Rotationen:** `ctx.rotate(+a)` = im Uhrzeigersinn. Hangneigung kippen: `rotate(+slope*D2R)`;
  Fluglage B (0° = horizontal, positiv = aufrecht): Körper-Frame = `rotate(-B*D2R)`.
- **Querformat-Trick:** Bei Hochformat wird `#app` per CSS um 90° rotiert (`ROT=true`),
  W/H getauscht; Touch-Deltas dann `dv = -dx` statt `dy`. iOS erlaubt kein `orientation.lock()`.

## Physik & Kalibrierung

Flugmodell: Auftrieb/Widerstand ∝ v² der **Luft**-Relativgeschwindigkeit; Anstellwinkel
`aoa = B − gamma` steuert `clShape(aoa)` (Optimum ~30°, Stall >60°, Sturzgefahr bei aoa < −9°
länger als 0,5 s). Wind: positiv = Aufwind, wirkt horizontal entgegen + vertikale Komponente
(`windY = 0.65·head`). Absprungqualität skaliert Impuls UND Kantentempo (`v *= 0.955+0.045q`).

Kalibrierte Konstanten (NICHT blind ändern – siehe Workflow unten):

```
PHYS: muInrun=0.03  kAirInrun=0.0013  vJump=2.9
      KLmax=0.0052  KD0=0.0047  KD2=3.0e-6
Hinzenbach: Anlauf 78 m → 84 km/h · liftMul=1.0
            perfekt ≈ 87 m, verpatzt ≈ 66 m, +2 m/s Aufwind ≈ 93 m
Obertraun:  Anlauf 115 m → 105 km/h · liftMul=1.42
            perfekt ≈ 223 m (8 s Flug), +2.6 Aufwind ≈ 250 m (über HS!)
```

**Workflow für Physik-Änderungen:** Flug niemals „nach Gefühl" im Browser tunen.
Die Formeln sind 1:1 in einem headless Node-Skript reproduzierbar (Profil aufbauen,
Anlauf integrieren, Flug mit Ideal-Piloten `B = gamma + 30` simulieren) – Zielwerte
pro Schanze prüfen, erst dann Konstanten in die HTML übernehmen. Vorlage: einfach den
Physik-Block aus `index.html` kopieren und eine Parametersuche drüberlegen.

**Neue Schanze hinzufügen** = neuer Eintrag in `HILL_DEFS` (Profil-Winkelfunktion über
Bogenlänge s, Anlaufparameter, K/HS/P, `liftMul`, `view`-Zoom, Meterlinien-Bereich) +
Segment-Button im Menü. Danach Kalibrierlauf für Anlauflänge und ggf. `liftMul`.
Roster mit allen 14 OÖ-Schanzen: `KONZEPT.md`, Abschnitt 12.

## Bekannte Punkte / offene Enden

- `vPerp`-Landeschwellen (Telemark < 8.2, Sturz > 9.5 …) sind global; auf der K220 ggf. je Schanze skalieren.
- Kein `localStorage`/IndexedDB im Prototyp: In der Claude-Artifact-Vorschau ist Browser-Storage blockiert; Session-Bestweiten liegen nur im RAM. Beim Selbst-Hosten darf Persistenz rein (Konzept sieht IndexedDB + Export/Import vor).
- Gyro-Steuerung (DeviceOrientation, iOS-Permission-Prompt!) ist im Konzept, fehlt noch.
- Toast/Chips nutzen feste Abstände statt `env(safe-area-inset)` im rotierten Modus (kosmetisch).
- Flug auf der K220: Über-HS-Flüge landen in der Übergangszone und werden hart – gewollt.

## Nächste Schritte (Vorschlag = Roadmap M3/M4)

1. M3: Migration nach Vite + TypeScript gemäß Modul-Tabelle, Service Worker + Manifest (echte PWA), IndexedDB für Bests/Replays/Settings + Export/Import
2. M4: Wettkampfformat (2 Durchgänge, Top 30), CPU-Gegner (aus Attributen + Schanzenprofil + Seed gezogen), Hot-Seat
3. Replays als Ghost gegen den aktuellen Sprung einblenden (Daten liegen schon vor)
4. Danach M5 Karriere / M6 Schanzen-Editor (buildHill/makeProfile sind die Basis)


## Konventionen

- UI-Texte Deutsch; Schanzennamen = oberösterreichische Orte (rechtlich unkritisch, eigene Identität)
- Keine DSJ2-Assets/Namen übernehmen – nur die Spielmechanik-Idee
- Kommentator „Andi Silberberger" ist eine **frei erfundene Figur** – bewusst KEIN realer Kommentator nachgebaut (Persönlichkeitsrechte). Echte/natürliche Sprachausgabe ist NICHT enthalten: die Web Speech API klingt geräteabhängig synthetisch. Für echtes Kommentatoren-Feeling später eingesprochene/lizenzierte Audio-Clips einbinden (als Asset-Dateien). Die prozeduralen SFX (`Snd`) tragen aktuell die Audio-Atmosphäre.
- Alle Tunables zentral in `PHYS` bzw. `HILL_DEFS`, keine Magic Numbers im Gameplay-Code
- Determinismus erhalten: jede Zufallsquelle über den seedbaren RNG, Physik nur im 120-Hz-Fixed-Step
- Vor jedem Commit der HTML: `node --check` auf den extrahierten Script-Block
