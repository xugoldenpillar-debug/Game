# BREAKPOINT 0.9.0 release payload

The playable release is a self-contained HTML application stored as a gzip-compressed,
base64-encoded payload split across `release/chunks/part-00.txt` through `part-06.txt`.

The repository root `index.html` reconstructs and launches the game in a modern browser.

## Integrity

- Standalone HTML SHA-256: `22f52b6378fcd89ee0392fd31b1b8b8f3a110e20e3ad3477f55a65abb4affb14`
- gzip SHA-256: `e05b6e1863fa4f9f000c1e3718bc45acceb994e80dce77b71b6f1b72f3217fc0`

## Rebuild standalone release

```bash
cat release/chunks/part-*.txt | base64 -d | gzip -dc > BREAKPOINT-0.9.0.html
sha256sum BREAKPOINT-0.9.0.html
```

The expected standalone SHA-256 is shown above. Serve the repository through HTTP when using the root launcher because it fetches the chunk files at runtime.
