# ruehlig.de

Private Spielesammlung unter [ruehlig.de](https://ruehlig.de) — sechs Browser-Klassiker in einer einzigen statischen HTML-Datei, gehostet auf GitHub Pages.

## Was drin ist

| Spiel | Steuerung |
| --- | --- |
| Space Invaders | Pfeile links/rechts, Leertaste schießt |
| Pong | Pfeile hoch/runter, erste Seite auf 7 Punkte |
| Snake | Pfeiltasten |
| Blocks | Pfeile links/rechts, hoch dreht, runter beschleunigt |
| Breakout | Pfeile links/rechts, drei Bälle |
| Minesweeper | Linksklick gräbt, Rechtsklick setzt Fahne |

`ESC` schließt ein laufendes Spiel, `Leertaste` startet nach Game Over eine neue Runde.

## Aufbau

- `index.html` — die komplette Seite: Layout, Texte, alle sechs Spiel-Engines, Impressum und Datenschutz. Keine externen Requests (keine Fonts, kein CDN, kein Tracking), kein Build-Schritt.
- Alle Spiele laufen auf einem gemeinsamen 640×400-Canvas mit einer zentralen `requestAnimationFrame`-Schleife; pro Spiel gibt es eine Update-/Zeichenmethode.

## Lokal ansehen

Datei im Browser öffnen — oder:

```bash
python3 -m http.server 8000
```

## Deployment

Push auf `main` → GitHub Pages (Settings → Pages, Branch `main`, Ordner `/`). Custom Domain `ruehlig.de`, „Enforce HTTPS" aktiv.

## Rechtliches

Impressum und Datenschutzerklärung sind als Overlays im Footer verlinkt. Verantwortlich: Kai Rühlig, Anton-Fils-Str. 6, 67227 Frankenthal — Kontakt: website@ruehlig.de

## Lizenz

Code zur freien Verwendung; Inhalte und Impressumsdaten nicht.
