# Contributing to Spellwreck

## Architecture

Spellwreck is a single-file web app. Everything lives in `index.html` — HTML, CSS, and JavaScript inlined together. There are no build tools, no dependencies, and no server.

## Running Locally

Open `index.html` in a browser. That's it.

## Code Structure

The file is organized in this order:

1. **CSS** — all styles in a single `<style>` block
2. **HTML** — setup screen, scoreboard panel, game screen, fullscreen overlay, summary screen
3. **JavaScript** — in a single `<script>` block at the end

### JavaScript Sections

- **Sound Effects** — Web Audio API oscillator functions
- **State** — game variables (players, scores, streaks, timers, etc.)
- **LocalStorage** — persistence for players and seen words
- **Screen Management** — show/hide screens, gear menu, score panel
- **Setup** — player add/remove, render player list
- **Word Selection** — filtering by set, deduplication, pick random word
- **Timer** — countdown logic, start/stop/pause/toggle
- **Game Flow** — startGame, nextWord, revealWord, handleCorrect/Wrong/Skip
- **Scoreboard** — render scores with crown/clown/streak emojis
- **End Game Summary** — podium, streaks, missed words
- **Fullscreen Mode** — enter/exit/sync
- **Keyboard Shortcuts** — arrow keys, number keys, spacebar, escape
- **Event Listeners** — all DOM event bindings
- **Init** — loadState, renderPlayerList

## Adding a Word Set

1. Add the words to `WORD_BANK` in the JavaScript section. Each entry is `{w:"Word",d:"setid"}`. Group them under a comment:

```js
// My New Show
{w:"Character",d:"myshow"},{w:"Location",d:"myshow"},
```

2. Add a checkbox to the setup HTML inside the appropriate subtitle group (e.g. TV & Movies):

```html
<label><input type="checkbox" id="set-myshow"> My Show</label>
```

3. Add the checkbox read in `startGame()`:

```js
if (document.getElementById("set-myshow").checked) selectedDifficulties.push("myshow");
```

That's it. Duplicates across sets are handled automatically at runtime — each word only appears once per game.

### Word Set Guidelines

- No duplicates within a single set
- Cross-set duplicates are fine (deduplicated at runtime)
- Use Canadian spelling for English words (colour, favourite, licence)
- Proper nouns keep their original casing (the display uppercases everything via CSS)
- Aim for words that are tricky to spell, not obscure vocabulary

## Styling Guidelines

- Dark theme: background `#1a1a2e`, text `#eee`, accent `#f5c842`, error `#e74c3c`, success `#2ecc71`
- All interactive elements need `touch-action: manipulation` and minimum 44px tap targets for mobile
- Use `env(safe-area-inset-*)` for fixed-position elements near screen edges
- Use `100dvh` with `100vh` fallback for full-height layouts
- Test on desktop, iPhone, and iPad Safari

## Testing

Manual testing only. Open the file and verify:

- Add/remove players
- Play through several rounds with different word sets
- Correct/Wrong/Skip all work, scores update
- Wrong words reappear in later rounds
- Timer starts, pauses, expires correctly
- Fullscreen mode works with controls and keyboard
- Streaks, crown/clown emojis display correctly
- End game summary shows accurate data
- State persists on page reload
- No scrollbar during gameplay
- Responsive on mobile
