# Projekt „SkiFlug" (Arbeitstitel) – Skisprung-Spiel als PWA

**Konzept- und Analysedokument · Version 0.1 · Vorbereitung für die Implementierung**

---

## 1. Vision

Ein 2D-Skisprungspiel im Geiste von Deluxe Ski Jump 2 (DSJ2), das vollständig als Progressive Web App im Browser läuft – mobile-first, offline-fähig, installierbar auf dem Homescreen. Steuerung und Grafikgefühl orientieren sich an der nativen DSJ2-iPhone-App. Darüber hinaus erweitern wir das Konzept um zwei große neue Säulen, die DSJ2 nicht bietet:

1. **Karrieremodus** – einen Springer über mehrere Saisonen entwickeln und begleiten
2. **Schanzen-Editor** – eigene Schanzen entwerfen, testen und teilen

---

## 2. Analyse der Vorlage: DSJ2

### 2.1 Was DSJ2 ausmacht

DSJ2 (Mediamond / Jussi Koskela, frühe 2000er, iOS-Version seit 2022) ist eine minimalistische, aber physikalisch tiefgehende 2D-Skisprungsimulation in Seitenansicht. Der Reiz liegt nicht in der Grafik, sondern im „Fluggefühl": Die Fluglage des Springers wird kontinuierlich und feinfühlig gesteuert, kleine Fehler kosten Meter, perfekte Sprünge fühlen sich verdient an. Hoher Wiederspielwert durch „easy to learn, hard to master".

### 2.2 Spielablauf eines Sprungs (Phasenmodell)

| Phase | Spieler-Input | Was passiert |
|---|---|---|
| 1. Start | Tap | Springer verlässt den Balken, beschleunigt in der Anlaufspur |
| 2. Anfahrt | keiner | Geschwindigkeit ergibt sich aus Anlauflänge/Gate, Reibung, Hocke |
| 3. Absprung | Tap (Timing!) | Kraftimpuls am Schanzentisch; zu früh/zu spät kostet Höhe & Weite |
| 4. Flug | kontinuierlich (Touch-Drag oder Gyro/Tilt) | Anstellwinkel von Körper & Ski steuern Auftrieb vs. Luftwiderstand; Wind wirkt permanent |
| 5. Landung | Tap (Timing) | Telemark bei gutem Timing → Haltungsnoten; zu spät/falsche Lage → Sturz |
| 6. Auslauf & Wertung | keiner | Weitenmessung, Richternoten, Anzeige |

### 2.3 Steuerung der iOS-App (unser Referenzmodell)

Die iOS-Version bietet zwei umschaltbare Steuerungsarten mit einstellbarer Empfindlichkeit:

- **Touch-Modus:** Daumen auf dem Bildschirm, vertikales Ziehen neigt den Springer vor/zurück (kontinuierlich, kein Tasten-Digital-Input). Taps für Start, Absprung und Landung.
- **Gyro/Tilt-Modus:** Neigen des Geräts steuert die Fluglage; gilt in der Community als präziser auf großen Schanzen.
- **Empfindlichkeit (Sensitivity):** einstellbar in Stufen – essenziell, da Anfänger niedrige, Profis hohe Werte bevorzugen.

→ **Wir übernehmen alle drei Elemente:** Touch-Drag als Standard, Gyro als Option (DeviceOrientation API, auf iOS mit Permission-Prompt), Sensitivity-Slider in den Einstellungen.

### 2.4 Grafik & Sound

- Seitenansicht, flächige Retro-2D-Grafik (Original: 320×200 px), weich scrollende Kamera, die dem Springer folgt
- Weitenmarkierungen im Aufsprunghang, Windanzeige (Pfeil + m/s), Geschwindigkeitsanzeige am Tisch
- Replay-Funktion mit Frame-Navigation
- Lo-Fi-Soundeffekte (Anfahrtsrauschen, Wind, Aufsprung, Publikum)

### 2.5 Inhalte & Modi in DSJ2

- 32 Schanzen von K50 bis K250 (Skiflug)
- Einzelspringen (Training), Weltcup und Team-Cup, Hot-Seat bis 16 Spieler, CPU-Gegner mit editierbaren Namen und Schwierigkeitsstufen
- Wind als zentrales Strategieelement (Aufwind unter den Skiern = weite Sprünge)

### 2.6 Was DSJ2 fehlt (unsere Chance)

- Keine Karriere, keine Springer-Entwicklung, keine Saison-übergreifende Progression
- Kein Schanzen-Editor für Endnutzer
- Kein Online-Modus (häufigster Community-Wunsch)
- Keine moderne Plattform-Unabhängigkeit → genau das löst unsere PWA

---

## 3. Rechtliche Abgrenzung (wichtig!)

Spielmechaniken sind nicht schützbar – Assets, Name, Code und konkrete Schanzendaten von DSJ2 schon. Daher:

- Eigener Name, eigenes Logo, komplett eigene Grafiken und Sounds
- Keine 1:1-Kopie der DSJ2-Schanzenprofile; wir bauen Schanzen nach realen FIS-Normparametern (K-Punkt, HS, Tischneigung etc.), die frei verwendbar sind – die eigene Identität entsteht über das Oberösterreich-Setting (siehe Abschnitt 12)
- Keine echten Athletennamen ohne Lizenz; Namensgenerator + editierbare Namen wie bei DSJ2

---

## 4. Technologie-Stack

| Bereich | Entscheidung | Begründung |
|---|---|---|
| Sprache | TypeScript | Typsicherheit für Physik & Datenmodelle |
| Build | Vite + vite-plugin-pwa (Workbox) | schneller Dev-Loop, Service-Worker-Generierung |
| Rendering | HTML5 Canvas 2D (eigene Engine) oder PixiJS (WebGL) | Retro-2D braucht keine schwere Engine; PixiJS falls Partikel/Skalierung nötig. Empfehlung: mit Canvas 2D starten, Render-Layer abstrahieren |
| Physik | eigene 2D-Physik, fixed timestep (z. B. 120 Hz), deterministisch | volle Kontrolle über das „Fluggefühl", Replays & Ghosts werden trivial (Input-Aufzeichnung statt Video) |
| UI (Menüs) | leichtgewichtig: Preact oder Svelte – Spiel selbst rein Canvas | klare Trennung Game-Loop / Menü-UI |
| Persistenz | IndexedDB (Karriere-Saves, eigene Schanzen, Replays) + `navigator.storage.persist()` | localStorage nur für Settings |
| Audio | Web Audio API | latenzarm, Lautstärke-Mix |
| Optional später | kleines Backend (Leaderboards, Schanzen-Sharing, Cloud-Save) | für MVP nicht nötig – alles läuft offline |

### PWA-/iOS-Besonderheiten, die wir von Anfang an einplanen

- **Installierbarkeit:** Web App Manifest, `display: standalone`/`fullscreen`, Icons, Splash
- **Offline:** komplettes Spiel precachen (Assets sind klein), App startet ohne Netz
- **Orientation:** Landscape ist Pflicht fürs Spielgefühl. `screen.orientation.lock()` funktioniert auf iOS Safari **nicht** → eleganter „Bitte Gerät drehen"-Overlay als Fallback
- **Vibration API:** auf iOS nicht verfügbar → Haptik nur auf Android, niemals als Gameplay-Voraussetzung
- **Gyro:** `DeviceOrientationEvent.requestPermission()` auf iOS erforderlich (HTTPS + User-Geste)
- **Audio-Unlock:** Web Audio erst nach erster Touch-Interaktion starten
- **Speicher-Eviction:** iOS kann Website-Daten löschen → Export/Import der Saves als Datei (JSON) einbauen
- **Touch:** `touch-action: none` auf dem Canvas, kein Pull-to-Refresh, kein Doppeltipp-Zoom, Safe-Area-Insets (Notch)

---

## 5. Spielmechanik-Spezifikation (Kern)

### 5.1 Physikmodell (vereinfacht, aber „ehrlich")

- **Anfahrt:** v aus Hangabtrieb − Gleitreibung − Luftwiderstand; Startgate verschiebt den Startpunkt (Gate-Faktor wie im echten Sport)
- **Absprung:** Timing-Fenster (~±100 ms um den Tischkante-Idealpunkt), Impuls senkrecht zur Tischebene, skaliert mit Timing-Qualität und (im Karrieremodus) Sprungkraft-Attribut
- **Flug:** Kräfte = Gravitation + Lift + Drag. Lift/Drag sind Funktionen des Anstellwinkels (Körperneigung relativ zur Flugbahn). Zu aufrecht → Bremsfallschirm; zu flach nach vorn → Druckverlust und Absturzgefahr. Genau diese Balance IST das Spiel.
- **Wind:** Basiswind + Böen (Perlin-Noise), wirkt auf Lift/Drag; Anzeige als Pfeil + m/s; optional Windkompensation in der Wertung (modern) oder ohne (klassisch DSJ2-Feeling) – als Regel-Setting
- **Landung:** Telemark-Fenster; Sturzlogik aus Aufprallwinkel + Geschwindigkeit + Fluglage
- **Determinismus:** fixed timestep, seedbarer RNG → Replays = gespeicherte Inputsequenz + Seed (wenige KB)

### 5.2 Wertung

- Weite: Abstand zum K-Punkt → Punkte nach Meterwert (K-Punkt-abhängig, wie FIS)
- Haltung: 5 simulierte Richter, höchste und niedrigste Note gestrichen; Abzüge für unruhigen Flug, fehlenden/wackligen Telemark, Sturz
- Anzeige: Weite, Noten, Gesamtpunkte, Zwischenstand des Wettbewerbs

---

## 6. Steuerung (Spec)

| Aktion | Touch-Modus | Gyro-Modus |
|---|---|---|
| Start vom Balken | Tap irgendwo | Tap |
| Absprung | Tap (Timing) | Tap |
| Fluglage | vertikales Ziehen (relativer Drag, Daumen bleibt liegen) | Gerät neigen |
| Landung/Telemark | Tap | Tap |
| Empfindlichkeit | Slider 1–10 in Settings | Slider 1–10 + Kalibrierung („Nullpunkt setzen") |

Grundsätze: keine UI-Buttons während des Flugs, gesamte Bildschirmfläche ist Eingabefläche, beidhändig im Querformat nutzbar, optionales Tutorial-Overlay beim ersten Sprung.

---

## 7. Karrieremodus (neues Feature, Spec)

### 7.1 Grundgerüst

- **Springer erstellen:** Name, Nation, Aussehen (Anzug-/Helmfarben), Startattribute per Punkteverteilung
- **Attribute:** Sprungkraft, Flugstil (Lift-Effizienz), Stabilität (Böen-Resistenz), Telemark, Nerven (Leistungs-Streuung unter Druck), Form (dynamisch, Tagesform-Kurve)
- **Saisonstruktur:** Sommer-GP (optional) → Weltcup-Kalender (~20 Springen auf verschiedenen Schanzen) → Highlights: „OÖ-Tournee" über vier Schanzen (Abschnitt 12), Skiflug-Finale in Obertraun, WM alle 2 Jahre, Olympia alle 4 Jahre
- **Zwischen den Events:** Trainingsplanung (Punkte auf Attribute verteilen), Erholung vs. Risiko (Verletzungs-/Formrisiko bei Übertraining)
- **Progression über Saisons:** Alterskurve (Aufstieg ~17–24, Peak ~25–30, Abbau danach), Continental Cup als Unterbau (bei schlechten Leistungen Abstieg, Quotenplätze)
- **KI-Welt:** ~60–100 generierte Springer mit eigenen Attributen, Entwicklung, Rücktritten und Nachwuchs → die Welt lebt über Jahre

### 7.2 Wirtschaft & Meta (Ausbaustufe 2)

- Sponsoren (Verträge an Weltranglistenposition geknüpft), Ausrüstung kaufen/verbessern (Ski, Anzug → kleine Physik-Boni innerhalb fairer Grenzen)
- Statistik-Hub: Saisonhistorie, persönliche Bestweiten je Schanze, Podien, Gesamtweltcup-Trophäen

### 7.3 Simulation der KI-Sprünge

KI-Ergebnisse werden nicht physikalisch voll simuliert, sondern aus Attributen + Schanzenprofil + Zufall (seedbar) gezogen – schnell genug, um einen kompletten Wettkampftag in Sekunden zu rechnen, mit „Live-Ticker"-Anzeige zwischen den eigenen Sprüngen.

---

## 8. Schanzen-Editor (neues Feature, Spec)

### 8.1 Ansatz: parametrisch statt Freihand

Schanzen werden über reale Normparameter definiert – das garantiert springbare, glaubwürdige Profile:

- Anlauf: Länge, Neigung, Radius des Übergangsbogens, Anzahl Startgates
- Tisch: Länge, Neigung (typ. 10–11°), Tischhöhe
- Aufsprunghang: K-Punkt, Hill Size (HS), Landehang-Neigung, Übergangskurve in den Auslauf (intern als Spline/Klothoide generiert)
- Umgebung: Tageszeit/Himmel, Schneetextur, Publikum, Flutlicht, Name & Ort

Live-Vorschau des Profils + sofortiger **Testsprung direkt aus dem Editor**. Validator warnt bei unspringbaren Kombinationen (z. B. Tisch zu flach für K-Punkt).

### 8.2 Speichern & Teilen

- Schanze = kompaktes JSON-Objekt → IndexedDB
- Export/Import als Datei oder **Share-Code** (Base64-komprimiertes JSON, via Web Share API / Link) – funktioniert ohne Backend!
- Eigene Schanzen sind in Einzelspringen, eigenen Events und (Ausbaustufe) im Karrierekalender nutzbar

---

## 9. Datenmodelle (Skizze)

```ts
interface HillProfile {
  id: string; name: string; country: string;
  kPoint: number; hillSize: number;
  inrun: { length: number; angle: number; gates: number };
  table: { length: number; angle: number; height: number };
  landing: { slopeAngle: number; profilePoints: Vec2[] }; // generiert
  env: { time: 'day'|'night'; weatherBias: number };
  author?: string; version: number;
}

interface Jumper {
  id: string; name: string; nation: string;
  attrs: { power: number; flight: number; stability: number;
           telemark: number; nerves: number };
  age: number; form: number;            // dynamisch
  colors: { suit: string; helmet: string };
}

interface CareerSave {
  version: number; createdAt: number;
  player: Jumper; world: Jumper[];
  season: number; calendar: EventDef[]; results: ResultRow[];
  economy?: { money: number; sponsors: Sponsor[]; gear: Gear };
  rngSeed: string;
}

interface Replay { hillId: string; seed: string; inputs: InputFrame[]; }
```

---

## 10. Architektur (Module)

```
/src
  /core      → game loop (fixed timestep), state machine der Sprungphasen
  /physics   → Anfahrt, Flug (Lift/Drag), Wind, Landung – pure functions, testbar
  /render    → Canvas-Layer: Parallax-Hintergrund, Schanze, Springer-Sprites, HUD
  /input     → Touch-Drag, Gyro, Sensitivity; abstrahiert zu "LeanInput"
  /scoring   → Weite, Richter, Wettkampfstand
  /modes     → Einzelspringen, Weltcup, Karriere, Editor-Testsprung
  /career    → Saison-Sim, KI-Springer, Kalender, Ökonomie
  /editor    → Parametrik, Profilgenerator, Validator, Share-Codes
  /persist   → IndexedDB-Wrapper, Save-Migrationen, Export/Import
  /pwa       → Service Worker, Manifest, Install-Prompt, Update-Flow
```

Leitprinzip: Physik und Karriere-Sim sind UI-frei und deterministisch → unit-testbar und replay-fähig.

---

## 11. Roadmap

| Milestone | Inhalt | Ergebnis |
|---|---|---|
| **M1 – Spielgefühl-Prototyp** | 1 Schanze (hardcoded), komplette Sprungphysik, Touch-Steuerung, Weite | „Fühlt es sich wie DSJ2 an?" – wichtigster Meilenstein, hier wird iteriert |
| **M2 – Spielbares Spiel** | Wind, Wertung/Richter, 5–8 Schanzen aus dem OÖ-Roster (Abschnitt 12), Gyro-Option, Sound, Replays | Einzelspringen komplett |
| **M3 – PWA-Shell** | Offline-Cache, Install, Saves in IndexedDB, Settings, Landscape-Handling | installierbare App |
| **M4 – Weltcup & Hot-Seat** | CPU-Gegner, Wettkampfformat (2 Durchgänge, Top 30), lokaler Mehrspieler | DSJ2-Featureparität |
| **M5 – Karrieremodus** | Springer, Attribute, Saison-Sim, KI-Welt, mehrere Saisons | USP Nr. 1 |
| **M6 – Schanzen-Editor** | Parametrik, Testsprung, Validator, Share-Codes | USP Nr. 2 |
| **M7 – Polish & Ausbau** | Karriere-Ökonomie, Statistiken, ggf. Online-Leaderboards (Backend) | Release-Kandidat |

---

## 12. Schanzen-Roster: Oberösterreich-Setting

Alle Schanzen tragen Namen oberösterreichischer Orte – das gibt dem Spiel eine eigene, unverwechselbare Identität (und grenzt uns zusätzlich von DSJ2 ab). Die Schanzengröße folgt dabei der Geografie: kleine Schanzen im flachen Inn- und Mühlviertel, Normal- und Großschanzen Richtung Voralpen, Skiflugschanzen im Salzkammergut und in der Pyhrn-Priel-Region.

| # | Schanze | K-Punkt | HS | Charakter / Besonderheit |
|---|---|---|---|---|
| 1 | Schärding | K50 | HS55 | Einsteigerschanze am Inn, Tutorial |
| 2 | Freistadt | K65 | HS72 | Mühlviertler Hügelland |
| 3 | Ried im Innkreis | K70 | HS77 | Innviertel |
| 4 | Rohrbach-Berg | K75 | HS83 | Böhmerwald-Kulisse |
| 5 | Hinzenbach | K85 | HS90 | Hommage an die reale Skisprung-Arena in OÖ (bei Eferding); Flutlicht-Nachtspringen |
| 6 | Wels | K90 | HS98 | Messestadt-Schanze |
| 7 | Linz-Pöstlingberg | K95 | HS104 | Stadtschanze bei Nacht, Donaublick |
| 8 | Steyr | K105 | HS115 | Altstadt-Panorama |
| 9 | Gmunden | K110 | HS120 | Grünberg, Blick auf den Traunsee |
| 10 | Bad Ischl | K120 | HS131 | Katrin, Salzkammergut |
| 11 | Windischgarsten | K125 | HS138 | Pyhrn-Priel, klassische Großschanze |
| 12 | Hinterstoder | K130 | HS142 | Höss, Weltcup-Atmosphäre |
| 13 | Grünau im Almtal | K185 | HS205 | Kasberg, erste Skiflugschanze |
| 14 | Obertraun | K225 | HS253 | Dachstein/Krippenstein, Endgame-Monsterflugschanze |

Daraus ergeben sich direkte Anknüpfungen an andere Features:

- **„OÖ-Tournee"** als Karriere-Highlight (unser Vier-Schanzen-Tournee-Äquivalent): Linz-Pöstlingberg → Bad Ischl → Windischgarsten → Hinterstoder
- **Skiflug-Saisonfinale** in Obertraun
- **Editor:** Beim Erstellen eigener Schanzen schlägt ein Namensgenerator weitere OÖ-Orte vor (Reserve-Pool: Vöcklabruck, Kirchdorf an der Krems, Perg, Grieskirchen, Mondsee, St. Wolfgang, Gosau, Ebensee …)
- Geografische Ortsnamen sind frei verwendbar – rechtlich unproblematisch

---

## 13. Offene Entscheidungen (vor M1 klären)

1. Canvas 2D vs. PixiJS (Empfehlung: Canvas 2D, Render-Schicht austauschbar halten)
2. Grafikstil: bewusst retro-pixelig (320×200-Hommage) oder cleaner Flat-Vektor-Look?
3. Windkompensation in der Wertung: klassisch (ohne) als Default, modern als Option?
4. Hochformat-Support als Notlösung oder strikt Landscape-only?
5. Name des Spiels & Branding
