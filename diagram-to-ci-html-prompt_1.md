# Prompt: Turn a diagram into 1:2 Coffee CI-formatted HTML

Copy everything below into a chat with Claude, replace the bracketed parts, and attach or describe your diagram.

---

Turn the following diagram into a standalone HTML file, styled to match our "1:2 Coffee" Corporate Identity (CI) spec at https://victoradanaith.github.io/onetotwoproposal/ci.html.

**Diagram to convert:** [describe the flow, or paste an existing mermaid spec, or attach a reference image]

**Requirements — CI compliance:**

1. **Colors** (use these exact hex values as CSS custom properties):
   - `--ink: #141414` (primary text/lines)
   - `--paper: #FFFFFF` (background)
   - `--stone: #F5F4F1` (secondary node fill / panel background)
   - `--roast: #8B5E3C` (single accent color — use sparingly, e.g. one emphasized node or label)
   - `--grey: #6B6B6B` (secondary text)
   - `--mist: #E3E1DC` (borders/dividers)
   - Rule: black + white carry the system, roast is for one accent only, never more.

2. **Typography:**
   - English text: **Outfit** (weights 300/400/500/600/700 as needed)
   - Thai text: **Prompt** (weights 300/400/500/600)
   - Utility labels/tags (small caps-style captions): **IBM Plex Mono**, weight 600, ~12–13px, letter-spacing ~0.15em, color `--roast`
   - Load all three via Google Fonts in `<head>`.
   - Type scale: page title ~28px Outfit Bold; section headings ~26px Outfit/Prompt SemiBold; body text ~16–17px, line-height 1.7 (English) / 1.85 (Thai).

3. **Bilingual everywhere:** every label, title, subtitle, node, edge label, and any explanatory text must appear in **both English and Thai**. Thai text must render in Prompt, not fall back silently to a generic sans-serif — apply `font-family:'Prompt',sans-serif` explicitly to Thai spans/elements (don't rely on the page's default font-family covering it, since Outfit has no Thai glyphs).

4. **Diagram engine:** use **Mermaid.js** (via `<script src="https://cdnjs.cloudflare.com/ajax/libs/mermaid/10.9.1/mermaid.min.js"></script>`), rendered live in the browser inside a `<pre class="mermaid">` block — not a static image. Set the mermaid init to theme `'base'` with `themeVariables` pointing at the CI colors (`primaryColor:'#F5F4F1'`, `primaryTextColor:'#141414'`, `primaryBorderColor:'#141414'`, `lineColor:'#141414'`, `background:'#FFFFFF'`) and `fontFamily:'Outfit, sans-serif'`.

5. **Diagram sizing:** force the diagram to fill the page width rather than rendering small — apply:
   ```css
   .mermaid svg{ width:100% !important; max-width:none !important; height:auto !important; min-width:1100px; }
   ```
   and give the container generous padding (~44px) inside a `--stone` background box with a `1px solid --mist` border and ~14px border-radius, `overflow-x:auto` in case content still exceeds the width.

6. **Page structure:**
   - `<header>` with bilingual `<h1>` (English main line, Thai as a smaller sub-line below it) and 1–2 bilingual subtitle `<p>` tags.
   - `<section>` containing the `.diagram-box` with the mermaid diagram.
   - Optional: an explanatory section below with bilingual `.explain-card` blocks (background `--paper`, border `--mist`, rounded corners) if the diagram needs supporting narrative — otherwise skip this.
   - Optional: a `.callout` box (left border accent in `--roast`, background `--stone`) for any implementation notes or caveats — bilingual if used.

7. **File output:** a single self-contained `.html` file (inline `<style>`, Mermaid loaded from CDN) that opens directly in any browser with no build step.

**Node/edge label format inside the mermaid block:**
Write labels as `English<br/><span style='font-family:Prompt,sans-serif'>ไทย</span>` for node text, and the same pattern for edge labels (arrows use `=="label"==>` for emphasized/thick edges or `-->|"label"|` for normal ones). For subgraph titles, use `English / <span style='font-family:Prompt,sans-serif'>ไทย</span>`.

Use `classDef` to define 2–3 node categories with restrained fills (mostly `--stone` `#F5F4F1` with `1.5px --ink #141414` strokes), and reserve a solid `--roast #8B5E3C` fill (white text) for exactly one special node — e.g. a circular "pool"/hub node using `((...))` syntax — to keep the CI "one accent only" rule.

**Reference file for exact styling/structure:** `accounting-flow-combined_1.html` (the "1:2 Coffee — Accounting Cash Flow" diagram) is the confirmed gold-standard example — attach it and say "match this file's CSS, mermaid init settings, and structure exactly." Its layout pattern: two small labeled subgraphs feeding into a circular emphasized hub node, which fans out to several destination nodes — a good template for any "multiple sources → central pool/hub → multiple destinations" flow. For a linear step-by-step flow instead, drop the subgraphs and destination fan-out, but keep the same classDef/color/font conventions.

---

### Notes on using this prompt
- If you don't have a reference file handy, just say "use the 1:2 Coffee CI diagram format" — Claude can look up the CI spec page itself.
- If the diagram is a variant/extension of an existing one (e.g. adding a new step to the vendor loop), attach the existing HTML file and ask Claude to extend it in place rather than starting fresh — keeps class names and structure consistent across all your CI diagrams.
- Always sanity-check Thai rendering after generation — open the file and confirm Thai text looks like Prompt (rounded, geometric) rather than a generic system font (usually flatter/less distinctive), since this is the most common thing that silently breaks.
