# browser-api — Claude Code Skill

A Claude Code skill that fetches live browser API documentation before writing code — covering **all major browsers** including experimental and bleeding-edge features.

## Why Use This

Without this skill, Claude writes browser API code from training data — which means:
- **Stale signatures** — APIs evolve; training data doesn't
- **Chrome bias** — most examples online target Chrome, so Firefox/Safari differences get missed
- **Missed experimental APIs** — newer or behind-flag features get skipped entirely
- **Wrong browser support assumptions** — code that works in one browser silently breaks in another

With this skill, Claude fetches live docs before touching a single line of code. You get:
- **Accurate, current API signatures** pulled directly from MDN
- **Full cross-browser picture** — what's shipped, what's flagged, what's missing per browser
- **Experimental feature awareness** — origin trials, nightly builds, and behind-flag APIs included
- **Early-stage proposal visibility** — WICG and W3C draft specs surfaced before they're mainstream
- **Compatibility warnings upfront** — polyfill suggestions and browser gap callouts before you ship broken code

## What It Does

Before writing any browser/web API code, this skill:

1. Fetches the MDN documentation for every relevant API
2. Checks all four major browser platform status trackers for experimental features
3. Cross-references Can I Use for compatibility data
4. Surfaces early-stage WICG proposals and W3C draft specs
5. Produces a structured per-browser support table so you always know what's stable, experimental, flagged, or missing

No more guessed API signatures, stale training data, or Chrome-only assumptions.

## Browsers Covered

| Browser | Status Tracker |
|---------|---------------|
| Chrome | [chromestatus.com](https://chromestatus.com) |
| Firefox | [platform-status.mozilla.org](https://platform-status.mozilla.org) |
| Safari / WebKit | [webkit.org/status](https://webkit.org/status/) |
| Edge | [developer.microsoft.com/en-us/microsoft-edge/status](https://developer.microsoft.com/en-us/microsoft-edge/status/) |

## Installation

Copy the `SKILL.md` file into your Claude Code skills directory:

```bash
# Personal (applies to all projects)
mkdir -p ~/.claude/skills/browser-api
cp SKILL.md ~/.claude/skills/browser-api/SKILL.md
```

Or clone directly:

```bash
git clone https://github.com/thisidnotfound/browser-api-skill ~/.claude/skills/browser-api
```

## Usage

### Manual
```
/browser-api IntersectionObserver
/browser-api WebTransport
/browser-api OffscreenCanvas
/browser-api View Transitions API
```

### Automatic
The skill auto-triggers when Claude detects browser API work in your coding task — no need to invoke it manually.

## Output Format

For each API, the skill returns:

```
## [API Name]
- MDN: [url]
- Spec: [W3C/WHATWG/WICG url]

### Browser Support
| Browser  | Status              | Notes        |
|----------|---------------------|--------------|
| Chrome   | Shipped v123        | ...          |
| Firefox  | In Development      | ...          |
| Safari   | Shipped v17.4       | ...          |
| Edge     | Shipped v123        | ...          |

### API Details
- Key interfaces/methods
- Constructor/syntax
- Experimental methods (per browser)
- Flags required

### Gotchas
- Browser-specific behavior differences
- Known bugs or incomplete implementations
- Polyfill available: yes/no
```

## Sources

- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API)
- [Chrome Platform Status](https://chromestatus.com)
- [Firefox Platform Status](https://platform-status.mozilla.org)
- [WebKit Feature Status](https://webkit.org/status/)
- [Microsoft Edge Status](https://developer.microsoft.com/en-us/microsoft-edge/status/)
- [Can I Use](https://caniuse.com)
- [WICG Proposals](https://github.com/WICG)
- [W3C Specifications](https://www.w3.org/TR/)

## Requirements

- [Claude Code](https://claude.ai/code) CLI

## License

MIT
