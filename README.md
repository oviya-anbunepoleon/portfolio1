# Oviya A. — Portfolio Website

A single-page, animation-rich personal portfolio built with plain HTML, Tailwind (CDN), and vanilla JavaScript — no build step, no framework, no bundler. Everything lives in one `.html` file plus a shared `styles.css` reference sheet and an `assets/` folder for images and documents.

---

## 1. Tech Stack

| Layer | Technology |
|---|---|
| Markup | Static HTML5 (single file) |
| Styling | Tailwind CSS (via CDN `cdn.tailwindcss.com`) + custom CSS (CSS variables, keyframes) |
| Fonts | Google Fonts: **Space Grotesk** (display), **Inter** (body/sans), **IBM Plex Mono** (code/mono accents), **Caveat** (handwritten annotations) |
| Interactivity | Vanilla JavaScript (IIFEs, no dependencies) |
| In-browser SQL engine | [sql.js](https://github.com/sql-js/sql.js) v1.8.0 (SQLite compiled to WebAssembly, loaded from cdnjs) |
| Speech | Native browser `SpeechSynthesis` Web API (text-to-speech for the AI avatar) |
| Form backend | [FormSubmit](https://formsubmit.co) (AJAX endpoint, no server code needed) |
| Hosting target | Any static host (Vercel, Netlify, GitHub Pages, etc.) |

No `package.json`, no npm install — open `index.html` directly or deploy the folder as-is.

---

## 2. File Structure

```
/
├── index.html                 → the entire site (markup + inline <style> + inline <script>)
├── styles.css                 → standalone reference stylesheet (design tokens / non-Tailwind CSS, mirrors the inline <style> block)
├── resume.pdf                 → downloadable resume (linked from the hero "Download Resume" button)
└── assets/
    ├── hero-photo.jpg                     → hero section portrait
    ├── ovi2.png.jpeg                      → About section photo 1 (carousel)
    ├── about-photo-2.jpg                  → About section photo 2 (carousel)
    ├── careerforge-1.png / -2.png         → AI CareerForge project screenshots
    ├── focusdesk-1.png / -2.png           → Focus Desk project screenshots
    ├── article-1.png / -2.png             → Article Website project screenshots
    ├── internsforge-offer-letter.pdf      → AI internship appointment letter
    ├── certificate-guvi-uiux.jpg          → HCL GUVI UI/UX workshop certificate
    ├── certificate-hackwell.jpg           → HACKWELL 2026 finalist certificate
    ├── certificate-internshala.jpg        → Content writing internship certificate
    ├── certificate-brandmonk-uiux.jpg     → BrandMonk Academy UI/UX webinar certificate
    └── certificate-brandmonk-video.jpg    → BrandMonk Academy video editing webinar certificate
```

> ⚠️ All asset paths are relative (`assets/...`). If any file above is missing from your deployment, that specific image/PDF will simply fail to load — nothing else on the page breaks.

---

## 3. Page Sections (in order)

1. **Nav** — sticky top bar with anchor links: about · skills · projects · experience · education · sql · contact
2. **Hero** — name, animated blinking-cursor heading, availability badge, location badge, tagline, CTA buttons (View Projects / Contact Me / Download Resume), a rotated portrait photo with a pinned "what's inside" index card, and 4 clickable stat tiles (Projects / Technologies / Years Learning / Certifications)
3. **About** — swipeable 2-photo carousel + bio paragraph + interest pills
4. **Skills** — 5 category cards: Frontend, Backend, Programming, AI/Data, Tools
5. **Projects** — 3 flip-cards (front = summary, back = live screenshot carousel):
   - **AI CareerForge** (Python, FastAPI, HTML/CSS/JS, Tailwind CSS, SQLite)
   - **Focus Desk** (HTML, CSS, JavaScript, Tailwind CSS)
   - **Article Website** (HTML, CSS, JavaScript)
6. **Experience** — AI Internship, InternsForge Skill India Pvt. Ltd., Bengaluru (Remote), Jun–Sep 2026, with a linked offer-letter PDF
7. **Education** — B.Tech, Artificial Intelligence & Data Science, Saranathan College of Engineering (2025–2029), CGPA 8.67
8. **Achievements** — 5 milestone tiles + a 6-slide flip/swipe certificate gallery (see §4.9)
9. **SQL Challenge** — live, runnable SQL problems powered by an in-browser SQLite engine (see §4.10)
10. **Contact** — real working contact form (FormSubmit) + click-to-copy email/phone + LinkedIn/GitHub/location links
11. **Footer** — closing tagline and copyright

---

## 4. Interactive Features (full list)

### 4.1 Theme picker
Fixed bottom-right pill with 5 swatches: **Lavender** (default), **Dark**, **Dark Pink**, **Red** (currently active/default in this file via `data-theme="red"`), **Yellow**. Switching swaps a full CSS-variable palette (`--paper`, `--ink`, `--brand`, `--line`, blob colors, etc.) on `<html data-theme="...">`, animated with a 0.35s transition. Selection is visual-only (not persisted to storage).

### 4.2 Custom cursor
A soft glowing radial "spotlight" (`.cursor-glow`) that lags behind the pointer using linear interpolation (lerp factor 0.16), plus a small solid dot (`.cursor-dot`) that enlarges on mouse-down. Disabled automatically on touch devices and under `prefers-reduced-motion`.

### 4.3 Ambient background
- A scrolling dotted/lined grid (`.bg-grid`) that drifts via CSS keyframes.
- Three large blurred color "blobs" that float independently (different durations/paths per blob) to give organic ambient motion behind content.
- A fixed left-hand "notebook ring-binder margin" strip (desktop ≥1280px only) with a mustard rule line, purely decorative.

### 4.4 Sticky-note section tags
Every section has a small dashed, "washi-taped" `§ section-name` badge (`.sticky-tag`) rotated a few degrees, reused throughout as the page's recurring visual signature.

### 4.5 Scroll effects
- **Scroll progress bar** — thin gradient bar at the very top of the viewport tracking scroll percentage.
- **Reveal-on-scroll** — every section fades/slides up into view via `IntersectionObserver` (`.reveal-init` → `.in-view`), with a matching "pop" animation on the 4 hero stat numbers.
- **Typewriter headings** — the `<h2>` of About, Skills, Projects, Experience, Education, Achievements, and Contact type themselves out character-by-character the first time they scroll into view (skipped under reduced motion).

### 4.6 Magnetic buttons
All primary CTA buttons (`.magnetic-btn`) subtly follow the cursor within their bounds on hover (translate proportional to cursor offset) and snap back on mouse-leave.

### 4.7 Project flip-cards
Each of the 3 project cards is a 3D `perspective`/`rotateY(180deg)` flip card:
- **Front**: title, description, tech-stack chips, GitHub link, "tap to see screenshots" hint.
- **Back**: fuller description + a 2-image mini screenshot carousel with prev/next buttons, dot indicators, and touch-swipe support. Clicking a screenshot opens the full-size lightbox instead of flipping the card back (`data-no-flip`).

### 4.8 About-section photo carousel
A 2-photo swipeable/clickable carousel with dot indicators, prev/next hover zones (desktop), touch-swipe (mobile), a fading "swipe or click to see the other photo" hint that auto-dismisses after ~4.5s or first interaction, and click-to-zoom via the lightbox.

### 4.9 Certificate/document flip gallery (Achievements)
A real 3D `rotateY` "page turn" gallery cycling through 6 documents: prev/next buttons, dot navigation, swipe support, and click-anywhere-to-advance. Handles both images (opens in lightbox) and PDFs (opens in a new tab) automatically based on file extension. Content list:
1. UI/UX Workshop — HCL GUVI (Vortex '26, NIT Tiruchirappalli)
2. HACKWELL 2026 — Final Round Qualifier
3. AI Internship — InternsForge Skill India Pvt. Ltd. (offer letter PDF)
4. Content Writing Internship — InAmigos Foundation (via Internshala)
5. UI/UX Webinar — BrandMonk Academy
6. Video Editing Webinar — BrandMonk Academy

### 4.10 Live SQL Challenge
A fully functional SQL playground running **entirely client-side**:
- Loads `sql.js` (SQLite-to-WASM) asynchronously from cdnjs on page load.
- 3 built-in placeholder problems (easy → medium), each with its own schema, sample-data preview table, a `<textarea>` for the user's query, a **Run Query** button (shows results + `EXPLAIN QUERY PLAN` output), a **Show Answer** toggle revealing a model solution, and prev/next navigation between problems.
- Problems included: window-function ranking (`DENSE_RANK`), conditional aggregation/pivot, and string parsing with `SUBSTR`/`INSTR`.
- Designed to be swapped out for the site owner's own solved problems (schema, statement, and solution are all defined in a single JS array for easy editing).

### 4.11 Stat detail popups
The 4 hero stat tiles (Projects / Technologies / Years Learning / Certifications) are clickable buttons that open a modal listing the underlying data:
- **Projects** → pulls title/description/tags live from the project cards in the DOM.
- **Technologies** → pulls and counts every skill listed in the Skills section (auto-updates the "20+" hero number).
- **Years Learning** → computed live from a `startYear` constant (currently `2024`) against today's date (auto-updates the hero number).
- **Certifications** → pulls every achievement-card description live from the Achievements section.

### 4.12 Click-to-copy contact details
Email and phone number in the Contact section are click-to-copy (`navigator.clipboard`), confirmed by a small toast notification at the bottom of the screen.

### 4.13 Contact form
Real, working form (name / email / message) submitting via `fetch()` to a FormSubmit AJAX endpoint (`https://formsubmit.co/ajax/hariovi10@gmail.com`), with inline status messages ("Sending…" → success/error) and no page reload. Hidden fields configure the email subject, a table-style template, and disable FormSubmit's captcha step.

### 4.14 Lightbox
A full-screen image viewer triggered by any element tagged `data-lightbox-src` (hero/about photos, project screenshots, certificate images). Closes on backdrop click, the ✕ button, or <kbd>Esc</kbd>.

### 4.15 AI avatar greeting widget
A small animated illustrated avatar (hand-drawn SVG character with blinking eyes, bobbing idle animation, and a talking-mouth animation) appears bottom-left ~0.9s after load with:
- A typewriter-animated greeting bubble ("Hey, welcome! I'm Oviya…").
- A **🔊 speak** button that reads the greeting aloud using the browser's native TTS, auto-selecting the best-sounding available female English voice (with a scored fallback list of common voice names across OS/browsers).
- A **✕ dismiss** button that also cancels any in-progress speech.
- Respects `prefers-reduced-motion` (skips typewriter, shows full text instantly).

### 4.16 Konami code easter egg
Entering the classic sequence (**↑ ↑ ↓ ↓ ← → ← → B A**) anywhere on the page triggers a canvas-based confetti burst plus a celebratory toast message.

### 4.17 Accessibility & motion considerations
- `prefers-reduced-motion: reduce` disables/simplifies: cursor effects, background blobs/grid drift, reveal animations, stat pop, typewriter effects, magnetic buttons, and the AI avatar's idle/talk animations.
- Cursor-glow/dot are hidden entirely on touch (`hover:none`) devices.
- Visible focus rings (`:focus-visible`) are defined in the shared stylesheet.
- All interactive icons/decorative elements use `aria-hidden` or `aria-label` where appropriate (theme swatches, lightbox close button, carousel dots/arrows).

---

## 5. Theming / Design Tokens

Colors are defined as CSS custom properties per theme (`:root` + `html[data-theme="..."]` overrides) rather than hard-coded, so the whole palette — paper/background, ink/text, brand accent, mustard accent, borders, surfaces, nav background, and the 3 ambient blob colors — swaps in one shot when the theme picker is used. Fonts are likewise centralized as Tailwind `fontFamily` extensions (`display`, `sans`, `mono`) mapped to the Google Fonts import.

`styles.css` in the repo root mirrors these tokens in a slightly different (green/blue) default palette — it can be used as the base stylesheet for a non-Tailwind or build-tooled version of the layout (About/Skills/Projects/Timeline/Achievements/Contact classes are all pre-defined there: `.hero`, `.stats`, `.about-grid`, `.skill-card`, `.project`, `.tl-item`, `.ach-card`, `.contact-grid`, etc.).

---

## 6. Third-Party Services & Dependencies

| Service/Library | Purpose | Notes |
|---|---|---|
| Tailwind CSS (CDN build) | Utility classes | Loaded via `<script src="https://cdn.tailwindcss.com">` — no PostCSS/build step |
| Google Fonts | Space Grotesk, Inter, IBM Plex Mono, Caveat | Loaded via `<link>`/`@import` |
| sql.js 1.8.0 (cdnjs) | In-browser SQLite engine for the SQL Challenge section | Loaded asynchronously; section shows "loading engine…" / "engine ready" / error status |
| FormSubmit | Contact form email delivery | No backend required; endpoint is `formsubmit.co/ajax/<email>` |
| Browser `SpeechSynthesis` API | AI avatar voice greeting | No external dependency; voice availability/quality varies by OS/browser |

---

## 7. Known Placeholders / Things to Customize Before Going Live

- **SQL Challenge problems** are explicitly labeled as placeholders in the on-page copy — swap the 3 built-in problems for the site owner's own solved problems (schema, statement, solution live together in one JS array near the bottom of `index.html`).
- **"Years Learning" start year** is a single `startYear = 2024` constant — update to reflect the actual year the site owner started coding.
- Contact phone number, email, LinkedIn, and GitHub URLs are hard-coded in the Contact section — update if any change.

---

## 8. Running Locally

No build step required.

```bash
# Option A — just open it
open index.html

# Option B — serve it (recommended, avoids some browser file:// restrictions
# e.g. fetch() for the contact form, sql.js WASM loading)
npx serve .
# or
python3 -m http.server 8000
```

Then visit `http://localhost:<port>`.

## 9. Deployment

Drop the folder (including `assets/` and `resume.pdf`) onto any static host — Vercel, Netlify, GitHub Pages, Cloudflare Pages — with no build command and `index.html` as the entry point.
