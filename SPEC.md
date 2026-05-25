# SPEC: AC Coupling Topic Page

## Site Architecture

Two page types:
- **Topic pages** — standalone one-off explanations (e.g. "AC Coupling"), linkable from anywhere
- **Narrative pages** — longer guided content that links to topic pages for deeper dives

Directory structure:
```
/
├── index.html              # landing / table of contents
├── topics/
│   └── ac-coupling.html    # ← this task
├── narratives/             # future
├── assets/
│   ├── style.css           # shared styles
│   └── circuit-snippet.js  # embed script (vendored)
└── SPEC.md
```

Plain HTML + CSS. No build step, no framework. Pages share one stylesheet.

## AC Coupling Page

Content outline:
1. **What** — capacitor blocks DC, passes AC
2. **Why** — remove DC offset between stages, protect inputs
3. **How** — RC high-pass filter behavior, cutoff frequency f = 1/(2πRC)
4. **Interactive circuit** — `<circuit-snippet>` embed of RC high-pass with adjustable R and C

## Circuit Embed

Using `<circuit-snippet>` web component from `circuit-snippet.js`:

```html
<circuit-snippet width="700" height="400" layout="horizontal" theme="dark">
  <script type="text/xml">
    <!-- Falstad XML: AC source → capacitor → resistor → ground, with output probe -->
  </script>
  <script type="application/json">
    {
      "controls": [
        { "component": "c:0", "param": "capacitance", "label": "C", "min": 1e-9, "max": 1e-5, "scale": "log", "unit": "F" },
        { "component": "r:0", "param": "resistance", "label": "R", "min": 100, "max": 100000, "scale": "log", "unit": "Ω" }
      ]
    }
  </script>
</circuit-snippet>
```

Source: vendor `circuit-snippet.js` into `assets/` (built IIFE bundle from the repo).

## Open Questions

- Vendor circuit-snippet.js now, or load from CDN/relative path to a local clone?
- Dark theme default for embeds — match site theme or independent?
- Any preferred visual style for the site (minimal, textbook-ish, dark mode)?
