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
For each API, fetch its MDN page for syntax, description, and usage. MDN URL patterns:
- CSS property: `https://developer.mozilla.org/en-US/docs/Web/CSS/[property-name]`
- JS API: `https://developer.mozilla.org/en-US/docs/Web/API/[InterfaceName]`
- JS method: `https://developer.mozilla.org/en-US/docs/Web/API/[InterfaceName]/[method]`

Note any API or method marked **Experimental**, **Non-standard**, or **Deprecated**.

### Step 3 — Fetch Exact Browser Version Support (Raw JSON — Most Reliable)
MDN pages render compat tables dynamically, so fetch the raw browser-compat-data JSON directly instead — this always works and returns exact version numbers:

- **CSS property:** `https://raw.githubusercontent.com/mdn/browser-compat-data/main/css/properties/[property-name].json`
- **JS API/Interface:** `https://raw.githubusercontent.com/mdn/browser-compat-data/main/api/[InterfaceName].json`
- **HTML element:** `https://raw.githubusercontent.com/mdn/browser-compat-data/main/html/elements/[element].json`

Parse the JSON for `chrome`, `firefox`, `safari`, `edge` version numbers and any `flags` entries. A `flags` entry means the feature requires manual browser flag activation.

### Step 4 — Check Browser Platform Status Trackers for Experimental Features
For anything experimental, in-progress, or cutting-edge, use **WebSearch** (not WebFetch) since these sites are JS-rendered and WebFetch cannot extract data from them:

**Chrome / Edge (Chromium)**
- WebSearch: `site:chromestatus.com [api name]` — look for shipping status, origin trial info
- Status levels to note: In Development / Origin Trial / Shipped / Behind Flag / Removed

**Firefox**
- WebSearch: `site:bugzilla.mozilla.org [api name] implementation` — find the tracking bug
- WebSearch: `firefox [api name] nightly support` — for recent additions
- Firefox release notes: `https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases`

**Safari / WebKit**
- WebKit feature status page is **retired** — do not use `webkit.org/status/`
- WebSearch: `site:webkit.org [api name]` — finds blog posts about new Safari features
- WebSearch: `safari technology preview [api name]` — for bleeding-edge Safari features
- WebKit standards positions (for WebKit's stance on a spec): `https://github.com/WebKit/standards-positions`

**Edge (beyond Chromium)**
- WebSearch: `site:developer.microsoft.com microsoft edge [api name] status`

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
| Browser  | Status                            | Notes                     |
|----------|-----------------------------------|---------------------------|
| Chrome   | Shipped vXX / Origin Trial / Flag | ...                       |
| Firefox  | Shipped vXX / Behind Flag / No    | flag name if applicable   |
| Safari   | Shipped vXX / In Preview / No     | ...                       |
| Edge     | Shipped vXX / Flag / No           | ...                       |

### API Details
- Key interfaces/methods: ...
- Constructor/syntax: ...
- Experimental methods (any browser): ...
- Flags required: exact flag names per browser
- Origin trial enrollment needed: yes/no

### Gotchas
- Browser-specific behavior differences: ...
- Known bugs or incomplete implementations: ...
- Order/precedence issues (e.g. shorthand resets): ...
- Recommended approach for cross-browser support: ...
- Polyfill available: yes/no — [link]
```

Then proceed to use this grounded knowledge when writing code. **Never guess an API signature — always fetch it first.**

## Important Rules
- Cover **all browsers equally** — do not focus only on Chrome
- Always include experimental and non-standard APIs — do not skip them
- Use **raw MDN compat JSON** (Step 3) for version numbers — do not try to scrape MDN HTML pages or caniuse.com for compat tables, they are JS-rendered and will not return data
- Use **WebSearch** for browser platform status trackers — do not WebFetch chromestatus.com or platform-status.mozilla.org directly, they are JS-rendered
- If an API is only in one browser, call that out clearly and suggest a cross-browser approach or polyfill
- If Firefox or Safari lag behind Chrome, surface the gap and recommend a CSS-first or progressive enhancement approach where applicable
- Flag anything requiring a browser flag (include exact flag name), origin trial, or with partial support
- If an API changed recently across any browser (new methods, deprecations, bugs), call it out
