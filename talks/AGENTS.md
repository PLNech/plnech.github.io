# AGENTS.md - Talks Repository

This repository contains Marp-based presentations compiled to static HTML.

## Repository Structure

```
talks/
├── index.html                    # Redirects to talk.html
├── AGENTS.md                     # This file - talks knowledge base
├── trust_me_bro/                 # "1M Tokens Trust Me Bro" (2025)
│   ├── talk.html                 # Compiled Marp presentation
│   ├── index.html                # Redirects to talk.html
│   └── img/                      # Images, SVGs, screenshots
├── agentic_evals/                # Agentic Evals talk
│   ├── talk.html
│   ├── index.html
│   └── img/
├── cold_brew_hot_takes/          # "Cold Brew, Hot Takes" (2026)
│   ├── talk.md                   # Marp source markdown
│   ├── talk.html                 # Compiled output
│   ├── index.html                # Redirects to talk.html
│   └── img/
└── .crush/                       # Image optimization (gitignored)
```

## Presentation Format

- **Source**: Marp markdown (`talk.md`) in each talk directory
- **Output**: Single self-contained HTML file (`talk.html`)
- **Slide Deck**: Bespoke.js engine via Marp CLI
- **Size**: 16:9 (1280x720px)
- **Presenter mode**: Press `p` during presentation
- **Fullscreen**: Press `f` or click fullscreen button

## Build Commands

```bash
# CRITICAL: Always use --html flag (without it, raw HTML tags get stripped)
marp --html talk.md -o talk.html

# Watch mode for development
marp --html -w talk.md

# Preview in browser
marp --html -p talk.md
```

### Known Pitfalls

- **Forgetting `--html` flag** is the #1 recurring issue. Without it, Marp strips all raw HTML from slides (inline styles, custom divs, etc). Commit `81fcfd5` was literally "rebuild with --html flag".
- **Slide overflow**: Dense content (tables, long lists) can overflow slide boundaries. Test visually.
- **Inline styles stripped**: Related to `--html` — diagram inline styles disappear without it.
- **Image paths**: Use relative paths (`img/foo.jpg`), not absolute. Marp resolves from the .md file location.

## Marp Features & Patterns

### Built-in Classes
- `lead` — centered title layout
- `invert` — inverted color scheme

### Custom CSS Patterns (reusable)

**Column layouts** (grid-based):
```css
.columns { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 1.5em; }
.columns-40-60 { display: grid; grid-template-columns: 40fr 60fr; gap: 1.5em; }
```

**Utility classes**:
```css
.dim { opacity: 0.25; }                          /* De-emphasize elements */
.small { font-size: 0.7em; color: #888; }        /* Fine print */
.highlight { background: #5B8DEF; color: white; padding: 0.2em 0.5em; border-radius: 4px; }
```

**Background image with overlay** (title slides):
```css
section.bg-dim::before {
  content: ''; position: absolute; inset: 0;
  background: rgba(28, 20, 16, 0.75);  /* Adjust alpha for readability */
}
```

### Fragmented Lists (progressive reveal)
```markdown
* First point
* <!-- fragment --> Second point (appears on click)
* <!-- fragment --> Third point
```
Compiles to `data-marpit-fragment="N"` attributes.

### Speaker Notes
```markdown
<!--
Your speaker notes here. Visible in presenter mode (p key).
Multi-line supported.
-->
```

### Background Images
```markdown
![bg](img/photo.jpg)                    <!-- Full background -->
![bg right:40%](img/photo.jpg)          <!-- Right 40% of slide -->
![bg left:30% opacity:0.7](img/x.jpg)  <!-- Left with opacity -->
![bg blur:3px](img/photo.jpg)           <!-- Blurred background -->
```

## Theme Conventions

### Dark theme (trust_me_bro, agentic_evals)
- Background: `#1a1a2e`, text: `#e0e0e0`, accent: `#5B8DEF`
- Font: Inter / Helvetica Neue

### Warm dark theme (cold_brew_hot_takes)
- Background: `#1c1410`, text: `#f5e6d3` (cream), accent: `#c8956c`
- Fonts: Cormorant Garamond (headers), Work Sans (body), JetBrains Mono (code)
- Google Fonts imported in frontmatter style block

## Deployment

- **Hosting**: GitHub Pages at `me.nech.pl`
- **Protected talks**: Client-side SHA-256 password gate + `sessionStorage`, with `noindex` meta tags
- **Redirect chain**: Each `index.html` redirects to `talk.html`
- **Companion pages**: Mobile-friendly HTML pages with speaker notes/cues for presenter use (separate from Marp output)

## Image Handling

- Keep images in `img/` subdirectory per talk
- Download at `w=1920&q=80` from Unsplash for good quality without bloat
- `.crush/` directory available for optimization (SQLite-based, gitignored)
- Verify downloaded images visually before building — Unsplash URLs can return redirects or wrong content