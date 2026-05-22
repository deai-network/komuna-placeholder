# Komuna Placeholder Site

This spec was written against the following baseline:

**Base revision:** `31d20ae45b639837ea31deda1798a57415e331aa` on branch `main` (as of 2026-05-22T11:06:25Z)

## Summary

A single-page static placeholder site for Komuna (an AI service, "Komuna KI"), to be displayed until the actual service comes online. The page shows a looping background video with a single line of German text in orange, positioned in the lower third.

## Goals

- Communicate that the service is coming.
- Set tone: playful "servant addressing masters" framing using formal/old-fashioned German.
- Keep implementation trivial — no build step, no framework, no hosting decisions baked in.

## Non-Goals

- Email signup / lead capture.
- Multi-language toggle.
- Analytics or tracking.
- Custom hosting / deployment config (user will decide separately).

## File Layout

```
/
├── index.html          # single page, inline CSS
├── public/
│   └── komuna-bg.mp4   # background video (already in repo)
└── README.md
```

No build tool. Static files only. Site can be opened directly in a browser or served by any static host.

## Page Content

### Browser tab title

```
Komuna KI – Demnächst
```

### Main text (single H1)

```
Sehr geehrte Herrschaften, Ihr neuer Diener geht bald online!
```

- `Herrschaften` = formal "masters/employers" (what a servant says); gender-neutral.
- `Diener` = servant.
- `geht bald online` = idiomatic German for "is going online soon".

### Language

HTML `lang="de"`.

## Visual Design

### Layout

- Video fills the entire viewport as a fixed background (`object-fit: cover`).
- Text is centered horizontally, positioned in the lower third of the viewport (`bottom: 12%`).
- No other UI elements (no logo, no nav, no footer).

### Typography

- Font: **Playfair Display**, weight 600, loaded from Google Fonts with `display=swap`.
- Fallback chain: `'Playfair Display', Georgia, serif`.
- Size: fluid via `clamp(1.5rem, 4.5vw, 3.25rem)`.
- Color: orange `#ff7a1a`.
- `text-shadow: 0 2px 12px rgba(0,0,0,0.6)` for legibility over arbitrary video frames.
- `text-align: center`, `max-width: 60rem`, `line-height: 1.2`.

### Background

- Body background `#000` so the page is black if the video fails to load.
- Video `z-index: -1` so text overlays it.

## Video Behaviour

`<video>` tag attributes:

- `autoplay` — plays without user interaction.
- `muted` — required for autoplay in all modern browsers.
- `loop` — runs continuously.
- `playsinline` — required for iOS to keep the video inline rather than going fullscreen.

Source: `public/komuna-bg.mp4` (already present in the repo).

## HTML Structure (reference)

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Komuna KI – Demnächst</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&display=swap" rel="stylesheet">
  <style>/* see CSS section */</style>
</head>
<body>
  <video autoplay muted loop playsinline class="bg-video">
    <source src="public/komuna-bg.mp4" type="video/mp4">
  </video>
  <main class="overlay">
    <h1>Sehr geehrte Herrschaften, Ihr neuer Diener geht bald online!</h1>
  </main>
</body>
</html>
```

## CSS (reference)

```css
html, body { margin: 0; height: 100%; background: #000; }

.bg-video {
  position: fixed;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: -1;
}

.overlay {
  position: fixed;
  bottom: 12%;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  padding: 0 1.5rem;
}

.overlay h1 {
  font-family: 'Playfair Display', Georgia, serif;
  font-weight: 600;
  color: #ff7a1a;
  font-size: clamp(1.5rem, 4.5vw, 3.25rem);
  text-align: center;
  max-width: 60rem;
  line-height: 1.2;
  text-shadow: 0 2px 12px rgba(0,0,0,0.6);
  margin: 0;
}
```

## Behavioural Notes

- **No JavaScript.** Page is pure HTML + CSS.
- **Video fails to load:** black background, text still visible.
- **Slow font load:** text renders in `Georgia`/serif fallback until Playfair Display arrives, then swaps (`display=swap`).
- **iOS portrait phones:** lower-third placement sits above the home indicator / bottom safe area in typical viewports. Not specifically tuned with `env(safe-area-inset-bottom)` since the placement (`12%` from bottom) already provides margin.

## Verification

No automated tests (single static page).

Manual verification:

1. Open `index.html` in a desktop browser. Video autoplays, loops, fills viewport. Orange text reads cleanly in lower third.
2. Open at narrow viewport (≤ 400px wide). Text size scales down via `clamp()`, remains readable.
3. Open in mobile Safari (iOS). Video plays inline (no fullscreen takeover) and remains muted.
4. Block the video file in DevTools and reload. Page shows black background; text remains readable.
5. Block Google Fonts and reload. Text renders in Georgia/serif fallback without invisible-text gap.

## Out of Scope (Future Decisions)

- Hosting target (GitHub Pages / Vercel / Netlify / custom). User has deferred this.
- Custom domain.
- SEO / OpenGraph tags. Can be added later if needed.
- Favicon. Can be added later if needed.
