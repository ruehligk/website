# CLAUDE.md

Kontext für Claude Code in diesem Repo.

## Projekt

Private Spieleseite unter ruehlig.de. Eine statische Seite auf GitHub Pages, ohne Build-Schritt, ohne Framework-Setup, ohne npm. `index.html` ist das Produkt.
Fine-grained access token mit Read-Access für Content, Pages und Metadata: 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


## Harte Regeln

- **Keine externen Requests.** Keine Google Fonts, keine CDNs, kein Analytics, keine Bilder von fremden Servern. Die Datenschutzerklärung sagt zu, dass nichts das Gerät verlässt. Das muss wahr bleiben.
- **Kein Build.** Kein npm, kein Bundler, kein Framework-Nachziehen. Alles läuft direkt im Browser.
- **Keine Nintendo-Assets.** Der Look ist 8-Bit-inspiriert, aber komplett eigen. Keine Sprites, Schriften, Sounds oder Figuren aus kommerziellen Spielen kopieren.
- **Impressumsdaten nicht anfassen**, außer ich sage es ausdrücklich.

## Design

- Palette: Papierweiß `#f6efe2`, Karten `#fffaf0`, Tinte `#1c1a16`, Akzente Rot `#c8412b`, Gelb `#e8a01f`, Blau `#2f7fd1`, Grün `#3a9d4a`, Grau `#8a8175`.
- Kanten sind hart: 3px schwarze Rahmen, 8px Offset-Schatten (`box-shadow: 8px 8px 0 #1c1a16`), keine Rundungen, keine Gradienten, keine weichen Schatten.
- Überschriften und alle Labels in Monospace, Großbuchstaben, weite Laufweite. Fließtext in Helvetica.
- Canvas-Grafik: nur Rechtecke, Farben aus der Palette, Hintergrund `#12100f`.
- Texte deutsch und verspielt; **Spieltitel bleiben englisch** (Space Invaders, Pong, Snake, Blocks, Breakout, Minesweeper).

## Spiel-Architektur

Alle Spiele teilen einen Canvas (640×400 logische Pixel) und eine dauerhaft laufende `requestAnimationFrame`-Schleife.

- `start(g)` setzt `this.active = g`, leert `this.ctx`/`this.over`, ruft `init_<g>()` auf, das den Spielzustand in `this.d` ablegt.
- `loop()` plant sich immer neu ein, holt den Canvas-Context lazy, löscht den Canvas auf `#12100f` und ruft die Methode des aktiven Spiels auf.
- Jedes Spiel hat genau eine Methode (`pong()`, `snake()`, `invaders()`, `tetris()`, `breakout()`, `mines()`), die Eingaben verarbeitet **und** zeichnet.
- Eingaben über `this.keys` (Keydown/Keyup auf `window`). Für schrittweise/diskrete Aktionen (z. B. Blocks: Drehen, Links/Rechts) direkt im globalen `keydown`-Listener behandeln statt über `this.keys` zu pollen, sonst wiederholt sich die Aktion jeden Frame, solange die Taste gedrückt bleibt.
- Mausspiele (Minesweeper) über einen `mousedown`-Listener auf dem Canvas: Client-Koordinaten anhand von `canvas.getBoundingClientRect()` auf die logischen 640×400 zurückskalieren (der Canvas wird per CSS auf Container-Breite gestreckt), `e.button` unterscheidet Links-/Rechtsklick.
- Game Over: `this.over = 'TEXT'` setzen. Der gemeinsame Banner samt Neustart per Leertaste kommt automatisch.
- Spielfelder, die nicht den ganzen Canvas füllen (z. B. Blocks-Grid), brauchen einen sichtbaren Rahmen/Hintergrund-Panel in Palettenfarbe, sonst verschwimmt das Spielfeld mit dem schwarzen Canvas-Hintergrund.

### Neues Spiel hinzufügen

1. `init_<name>()`-Methode schreiben, die den Anfangszustand in `this.d` ablegt.
2. Update-/Zeichenmethode schreiben, im `loop()`-Dispatch registrieren (Methodenname == Game-Key).
3. Karte im Spiele-Grid ergänzen (gleiche Struktur wie bestehende: Farbfeld mit Scanlines, Nummer, Titel, Beschreibung, START-Button).
4. Titel und Steuerungshinweis in die `titles`/`hints`-Tabellen eintragen.

## Nicht tun

- Karten, Spiele oder Sektionen „aufhübschen", die nicht Teil der Aufgabe sind.
- Zusätzliche Sektionen, Statistiken, Highscore-Server oder Social-Links erfinden.
- Emoji einsetzen.
- Die Seite in mehrere Dateien aufteilen, ohne dass ich es verlange.
