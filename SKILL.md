---
name: browser-api
description: Look up browser Web APIs on MDN and all major browser platform status trackers before coding. Auto-trigger when the task involves browser APIs, DOM, Canvas, WebGL, WebRTC, Service Workers, WebAssembly, WebTransport, or any Web Platform feature. Use to get accurate, up-to-date API signatures, parameters, and experimental status across Chrome, Firefox, Safari, and Edge before writing browser-related code.
user-invocable: true
allowed-tools: WebFetch, WebSearch
context: fork
argument-hint: [api-name or description]
---

# Browser API Reference Skill

You are a browser API research agent. Your job is to fetch accurate, current documentation for Web APIs — including **experimental and bleeding-edge features across all major browsers** — before the coding task proceeds.

Browsers covered: **Chrome, Firefox, Safari, Edge, and Samsung Internet.**

## How to Use This Skill

**Manual invocation:** `/browser-api [api-name]` — look up a specific API
**Auto-invoked:** When working on any browser/web code, identify all APIs involved and look them up before writing code.

The argument (if provided) is: `$ARGUMENTS`

## Research Steps

### Step 1 — Identify the APIs
If `$ARGUMENTS` is provided, use that as the target API(s).
If invoked automatically, identify every Web API relevant to the current coding task from context.

### Step 2 — Fetch MDN Documentation (Primary Source)
For each API, fetch its MDN page. MDN URL pattern:
- `https://developer.mozilla.org/en-US/docs/Web/API/[InterfaceName]`
- `https://developer.mozilla.org/en-US/docs/Web/API/[InterfaceName]/[method]`

MDN includes experimental badges and per-browser compat tables — note any API or method marked **Experimental**, **Non-standard**, or **Deprecated**, and which browsers support it.

### Step 3 — Check All Browser Platform Status Trackers
For experimental, in-progress, or cutting-edge features, check **all four** major browser trackers:

**Chrome / Edge (Chromium)**
- Chrome Platform Status: `https://chromestatus.com/features`
- WebSearch: `site:chromestatus.com [api name]`
- Status levels: In Development / Origin Trial / Shipped / Behind Flag / Removed

**Firefox**
- Firefox Platform Status: `https://platform-status.mozilla.org/`
- MDN Firefox Nightly notes: `https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases`
- WebSearch: `site:platform-status.mozilla.org [api name]`

**Safari / WebKit**
- WebKit Feature Status: `https://webkit.org/status/`
- Safari Technology Preview release notes: `https://webkit.org/blog/`
- WebSearch: `site:webkit.org [api name] status`

**Edge (independent tracking beyond Chromium)**
- Edge Platform Status: `https://developer.microsoft.com/en-us/microsoft-edge/status/`
- WebSearch: `site:developer.microsoft.com microsoft edge status [api name]`

### Step 4 — Check Can I Use for Compatibility
For a unified cross-browser compat view:
- `https://caniuse.com/?search=[feature]`
- Fetch the page or use WebSearch: `site:caniuse.com [api name]`

### Step 5 — Check Early-Stage Proposals
For APIs not yet in any browser:
- WICG Proposals: `https://github.com/WICG` — incubating Web Platform proposals
- W3C Specs: `https://www.w3.org/TR/` — official specs, including draft standards
- TC39 (JS language): `https://tc39.es/proposals/` — for JS-level features

### Step 6 — Surface What Matters
Return a structured summary per API:

```
## [API Name]
- MDN: [url]
- Spec: [W3C/WHATWG/WICG url if applicable]

### Browser Support
| Browser  | Status                          | Notes                     |
|----------|---------------------------------|---------------------------|
| Chrome   | Shipped vXX / Origin Trial / Flag | ...                     |
| Firefox  | Shipped vXX / In Development / No | ...                     |
| Safari   | Shipped vXX / In Preview / No   | ...                       |
| Edge     | Shipped vXX / Flag / No         | ...                       |

### API Details
- Key interfaces/methods: ...
- Constructor/syntax: ...
- Experimental methods (any browser): ...
- Flags required: ...
- Origin trial enrollment needed: yes/no

### Gotchas
- Browser-specific behavior differences: ...
- Known bugs or incomplete implementations: ...
- Polyfill available: yes/no — [link]
```

Then proceed to use this grounded knowledge when writing code. Never guess an API signature — always fetch it first.

## Important Rules
- Cover **all browsers equally** — do not focus only on Chrome
- Always include experimental and non-standard APIs — do not skip them
- If an API is only in one browser, call that out clearly and suggest a cross-browser approach or polyfill
- If Safari or Firefox lag behind Chrome on an API, note the version gap
- Flag anything that requires a browser flag, origin trial, or has partial support
- If an API changed recently across any browser (new methods, deprecations), call it out
- For Safari-only or Firefox-only experimental features, check their respective trackers even if Chrome has no entry
