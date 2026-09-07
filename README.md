# STICKMAN: BREAKPOINT / 火柴人：临界点

Private repository for the browser-based 2D stickman action game project.

## Current playable release

`BREAKPOINT 0.9.0` is now stored in this repository as a self-contained playable web release.

The root [`index.html`](index.html) is the game launcher. It reconstructs the compressed release payload under [`release/chunks/`](release/chunks/) in the browser.

Run the repository through a local HTTP server, for example:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080/` in a modern browser.

The release payload can also be reconstructed as a standalone HTML file:

```bash
cat release/chunks/part-*.txt | base64 -d | gzip -dc > BREAKPOINT-0.9.0.html
```

Release integrity details live in [`release/README.md`](release/README.md), and GitHub Actions verifies the reconstructed standalone HTML against its expected SHA-256 hash.

## Project direction

A browser-based 2D stickman action game inspired by the pacing and accessibility of classic side-scrolling stickman combat games, expanded with melee combat, firearms, items, story missions, enemy AI, bosses, progression, destructible environments, and reusable content systems.

## Design documentation

The original production and design specifications are preserved under [`docs/design/`](docs/design/README.md).

## Repository status

The repository now contains both the design documentation and the playable `0.9.0` release. Further production work should iterate from this playable baseline rather than treating the project as design-only pre-production.
