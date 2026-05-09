# 5x5 Go

A self-contained 5x5 Go game playable against a 1-ply lookahead bot. Single HTML file, vanilla JS, SVG board, no build step.

Live: [go-5x5.danieljohnmorris.com](https://go-5x5.danieljohnmorris.com)

## Run locally

```bash
python3 -m http.server 8765
```

Open http://localhost:8765.

## Rules

- 5x5 board, intersections (25 points)
- Captures: groups with no liberties are removed
- Suicide forbidden (unless it captures opponent first)
- Simple positional ko: cannot immediately recapture into a 1-stone ko
- Two consecutive passes end the game
- Chinese (area) scoring with 2.5 komi for White

## AI

Each turn the bot enumerates legal moves, simulates each, and picks the one that maximises its own area score under the same scoring function. Tiny centre/corner bonus to break ties; small random jitter to avoid loops. Passes if no move improves on the current evaluation and it is already winning. Around 30 lines.

## Files

- `index.html` - everything: board, rules engine, AI, render
- `Dockerfile` - nginx static container for deployment

## Test hooks

For unit testing, the page exposes `window.__go`:

```js
window.__go.tryMove(board, x, y, colour, koPoint) // pure rules engine
window.__go.score(board)                          // area + komi
window.__go.legalMoves(board, colour, koPoint)
window.__go.setBoard(board, toMove, koPoint)      // force position
```
