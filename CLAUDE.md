# Valentine Protocol

Interactive Valentine's Day card disguised as a terminal/dev console. Built for humor -- clicking "No" triggers fake error logs, screen shakes, guilt-trip alerts, and makes the "Yes" button dodge around the screen.

## Running

Open `index.html` directly in a browser, or serve via GitHub Pages.

```
open index.html
```

No server, build step, dependencies, or install required.

## Deployment (GitHub Pages)

1. `git init && git add -A && git commit -m "initial"`
2. Create a GitHub repo and push
3. Settings > Pages > Deploy from branch > main > / (root)
4. The `.nojekyll` file skips Jekyll processing
5. URL: `https://<username>.github.io/<repo-name>/`

## Architecture

Single HTML file (`index.html`) with embedded `<style>` and `<script>` blocks. No external dependencies.

### State

- `attempt` -- counter for "No" clicks, controls escalation
- `yesDodging` -- whether the Yes button is in evasive fixed-position mode
- `accepted` -- terminal state, locks both buttons
- `running` -- prevents concurrent chaos runs

### Flow

1. `banner()` -- prints fake boot sequence on load
2. "No" click -> `runChaos()` -- prints randomized error lines, triggers screen shake, shrinks No button, fires guilt-trip alert, starts `startYesDodge()` (Yes button moves to random fixed positions via interval)
3. After 4 "No" clicks, the No button is disabled and relabeled "Nope (banned)"
4. Yes dodge auto-stops after escalating duration (13s-22s depending on attempt count)
5. "Yes" click -> `accept()` -- only works when not dodging; sets terminal accepted state, shows success box

### Visual Features

- Animated gradient background (4-color dark gradient, 20s cycle)
- Floating heart particles (8 CSS-animated hearts, low opacity, `aria-hidden`)
- Yes button pulsing glow animation
- Card entrance slide-up + fade-in animation
- Glass morphism (`backdrop-filter: blur`) on cards
- Screen shake on No clicks

### Mobile Optimization (iPhone 16 Pro Max / iOS Safari)

- `viewport-fit=cover` with `env(safe-area-inset-*)` padding
- Dynamic viewport height (`100dvh`) for iOS Safari URL bar
- Responsive log height via `clamp(160px, 36vh, 340px)`
- 44px minimum touch targets per Apple HIG
- iOS-aware dodge zones (avoids Dynamic Island / home indicator)
- Touch proximity dodge (finger within 60px triggers evasion)

### CSS

Dark terminal aesthetic with animated gradient. Key classes: `.ok` (green), `.warn` (yellow), `.err` (red), `.muted` (dim). The `.log` div is a scrollable fake console.

## Commands

No build, test, or lint commands exist for this project.
