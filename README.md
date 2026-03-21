# Tipp·Trainer

A single-file typing speed trainer for German and English. No build step, no dependencies to install — open the HTML file in any modern browser and start typing.

---

## Features

**Training**
- Live WPM and accuracy tracking updated every second
- Four time modes: 15 s, 30 s, 60 s, 120 s
- Three difficulty levels: easy, medium, hard
- Four text categories: Nature, Technology, Culture, Science
- Automatic text advance — a new passage loads the moment you finish one
- Shake animation on wrong keystrokes
- Streak counter for consecutive sessions with ≥ 90 % accuracy

**Statistics**
- Live WPM sparkline in the timer card
- Session-end results overlay with a full WPM-over-time curve
- Personal best display and history of the last 50 sessions
- All data persisted in `localStorage` — survives page refresh

**Analysis**
- QWERTZ keyboard heatmap — keys you mistype most glow red
- Top-8 problem key ranking with error counts
- Per-session accuracy bar chart

**UI**
- Dark mode and light mode with a single click — preference saved across sessions
- German / English toggle — all labels, texts, and ratings switch instantly
- Fully responsive layout

---

## Usage

Download `tipp-trainer-v4.html` and open it directly in your browser. No server required.

```
open tipp-trainer-v4.html        # macOS
xdg-open tipp-trainer-v4.html    # Linux
start tipp-trainer-v4.html       # Windows
```

### Keyboard shortcuts

| Key     | Action                        |
|---------|-------------------------------|
| `Enter` | Focus the typing area / start |
| `Tab`   | Load a new text immediately   |
| `Esc`   | Reset the current session     |

---

## Project structure

The entire application lives in a single HTML file with no external dependencies except two CDN resources loaded at runtime:

| Resource | Purpose |
|----------|---------|
| [DM Mono + Syne](https://fonts.google.com) | Typography |
| [Chart.js 4.4](https://www.chartjs.org) | WPM sparkline, history chart, accuracy bars |

```
tipp-trainer-v4.html
├── <style>          CSS custom properties, dark/light theme tokens, layout
├── <body>           Three pages: Train · Leaderboard · Analysis
└── <script>
    ├── I18N         Translation strings (de / en)
    ├── TEXTS        Text database (de / en × 4 categories × 3 difficulties)
    ├── chartColors  Theme-aware colour resolver for Chart.js canvas
    ├── State (S)    Single mutable state object
    ├── Input loop   keydown + input handlers, auto-advance logic
    ├── Timer        setInterval tick, WPM sampling every 3 s
    ├── Session end  Record persistence, streak, result overlay
    └── Pages        Leaderboard charts, heatmap SVG, error list
```

---

## Customisation

### Adding texts

Open the file and find the `TEXTS` constant. Each language has four categories (`nature`, `tech`, `culture`, `science`), each with three difficulty arrays (`easy`, `medium`, `hard`). Add any string to the appropriate array:

```js
TEXTS.de.culture.medium.push(
  "Ihr neuer Satz kommt hier rein und wird automatisch in die Zufallsrotation aufgenommen."
);
```

### Adding a language

1. Add a new key to `I18N` with all required translation keys (copy the `en` block as a template).
2. Add a matching key to `TEXTS` with text arrays for each category and difficulty.
3. Extend `toggleLang()` to cycle through the additional language.

### Changing the accent colour

All colours are CSS custom properties on `[data-theme="dark"]` and `[data-theme="light"]`. Change `--accent` and `--accent-dim` in both blocks, then update the matching hex values inside `chartColors()` so the Chart.js canvases stay in sync.

---

## Browser support

Any browser with ES2020 support and Canvas API. Tested on Chrome 120+, Firefox 122+, Safari 17+.

`localStorage` is required for saving results and the error heatmap. In private / incognito mode data is not persisted between sessions.

---

## License

MIT — do whatever you like with it.
