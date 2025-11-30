<!-- Copilot / AI agent instructions for the `math` repo -->
# Repo Snapshot

- This repository is a very small static site: the main UI and logic live in `index.html` (inline CSS + JS). Images are kept in the `images/` folder.
- There are no build tools, package manifests, or tests. Changes are applied directly to the HTML file and viewed in a browser.

# High-level architecture (what to know)

- Single-page static app: `index.html` contains structure, styles and behaviour in one file.
  - Visuals: CSS inside `<style>` in the document head.
  - Interaction: plain DOM manipulation in an inline `<script>` at the bottom of the file.
- Data model is DOM-first: cell values are read from `.cell` elements (no separate JS state object). Example: `const values = [...cells].map(c => Number(c.innerText || 0));`
- Puzzle data is encoded in arrays inside the file: `magicSquares` holds example solutions, `hints` holds hint strings.

# Important repo-specific conventions and gotchas

- Image paths in `index.html` currently use an absolute Windows path (`D:\Kavya\Puzzles\Test\images\logo.png`). Prefer relative paths (`images/logo.png`) when editing or creating new assets.
- The grid is hard-coded to 3x3. Grid geometry, numbers and win-check logic are all implemented directly in `index.html` (look for `.grid`, `.cell`, `magicSquares`, and `lines` arrays).
- Drag-and-drop uses HTML5 `draggable` + `dataTransfer`. Look at the `dragstart`, `dragover`, and `drop` listeners for the pattern.
- UI actions are attached with `onclick` attributes on buttons (`check()`, `resetGrid()`, `newPuzzle()`, `toggleHint()`). These functions are defined in the inline script.

# Typical developer workflows (how to run / debug)

- Open `index.html` in a browser for quick checks. For a stable local server (recommended), run (PowerShell):

```powershell
python -m http.server 8000
# then open http://localhost:8000/index.html
```

- Use browser DevTools to inspect DOM, check console for JS errors, and monitor drag/drop events. Typical issues to check:
  - Missing images caused by absolute paths.
  - Uncaught exceptions if DOM selectors return null (editing element class names will break behaviour).

# Patterns & examples to reference when coding

- Selecting and mapping cells: `const cells = document.querySelectorAll('.cell');`
- Reading cell values for validation: `const values = [...cells].map(c => Number(c.innerText || 0));`
- Win-check lines are encoded as an array of index triples: `const lines = [[0,1,2],[3,4,5],...];` — update both layout and these indices together when changing grid size.
- Generate new puzzle example: `const puzzle = magicSquares[Math.floor(Math.random()*magicSquares.length)];`

# What to change and where (concrete examples)

- To change visuals: edit the CSS block near top of `index.html` (inside `<style>`).
- To add new puzzles or hints: add entries to `magicSquares` or `hints` in the script near the bottom of `index.html`.
- To fix the logo image path: replace the absolute path in the `<img>` `src` with `images/logo.png`.

# PR / edit guidance for AI agents

- Keep edits minimal and localized. This repo expects quick, small changes directly to `index.html` rather than introducing complex build steps.
- If you move code into separate files (e.g., `app.js` or `styles.css`), update `index.html` accordingly and test in a browser. Prefer relative paths.
- Don't assume availability of external tooling; prefer solutions that a reviewer can validate by opening the file in a browser.

# Questions for maintainers (ask these if you need to proceed)

- Should the image path be converted to a relative path (`images/logo.png`) and committed?
- Is expanding the app to support other puzzle sizes desirable (would require redesign of `lines` and CSS grid)?

---
If anything here is unclear or you want more detailed agent rules (commit messages, branch naming, tests), tell me which area to expand.
