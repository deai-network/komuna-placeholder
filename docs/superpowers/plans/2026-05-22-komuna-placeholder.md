# Komuna Placeholder Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page static placeholder site for Komuna KI: a looping fullscreen background video with one line of orange German text in the lower third.

**Architecture:** One static HTML file (`index.html`) at the repo root with inline CSS in a `<style>` block. The existing `public/komuna-bg.mp4` is the background video. No build tool, no JavaScript, no framework.

**Tech Stack:** HTML5, CSS3, Google Fonts (Playfair Display).

**Spec:** `docs/superpowers/specs/2026-05-22-komuna-placeholder-design.md`

---

## File Structure

- `index.html` — new file at the repo root. Contains everything: HTML head with meta tags + Google Fonts link + inline `<style>`, body with `<video>` element and `<main>` text overlay.
- `public/komuna-bg.mp4` — already exists, untouched.

That is the only file the plan creates. There is no separate CSS or JS file.

---

### Task 1: Create `index.html` with unstyled HTML structure

**Why split into two tasks:** Building the DOM first (without CSS) confirms the video element, source path, and text content are correct before layering on styling. If the video path is wrong, you find out before debugging CSS.

**Files:**
- Create: `/home/csillag/deai/komuna-placeholder/index.html`

- [ ] **Step 1: Create the file with the full HTML skeleton (no styling yet)**

Write the following to `/home/csillag/deai/komuna-placeholder/index.html`:

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Komuna KI – Demnächst</title>
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

Notes:
- `lang="de"` because content is German.
- `playsinline` is required on iOS to prevent the video from going fullscreen.
- `muted` is required for `autoplay` to work in modern browsers.
- The dash in the title is an en-dash (`–`, U+2013), not a hyphen.

- [ ] **Step 2: Verify the file by opening it in a browser**

Run:

```bash
cd /home/csillag/deai/komuna-placeholder
python3 -m http.server 8000
```

Then open `http://localhost:8000/` in a browser (Chrome or Firefox).

Expected:
- The page loads.
- The video `komuna-bg.mp4` autoplays, is muted, and loops.
- The German text "Sehr geehrte Herrschaften, Ihr neuer Diener geht bald online!" appears as a default-styled `<h1>` somewhere on the page (black text on white, large default font — no styling yet).
- Browser tab title reads "Komuna KI – Demnächst".

If the video does not play: check the path `public/komuna-bg.mp4` — the file should be at `/home/csillag/deai/komuna-placeholder/public/komuna-bg.mp4`. Confirm with `ls public/`.

Stop the server with Ctrl+C.

- [ ] **Step 3: Commit**

```bash
cd /home/csillag/deai/komuna-placeholder
git add index.html
git commit -m "Add unstyled HTML skeleton for placeholder page"
```

---

### Task 2: Add inline CSS for layout, typography, and colour

**Files:**
- Modify: `/home/csillag/deai/komuna-placeholder/index.html` (add Google Fonts `<link>` tags inside `<head>` and a `<style>` block also inside `<head>`)

- [ ] **Step 1: Add the Google Fonts preconnect + stylesheet link in `<head>`**

Inside `<head>`, immediately after the `<title>` line, add:

```html
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&display=swap" rel="stylesheet">
```

The `<head>` should now end with `</head>` after these three lines.

- [ ] **Step 2: Add the inline `<style>` block in `<head>`**

Inside `<head>`, immediately after the Google Fonts `<link>` lines and before `</head>`, add:

```html
  <style>
    html, body {
      margin: 0;
      height: 100%;
      background: #000;
    }

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
      text-shadow: 0 2px 12px rgba(0, 0, 0, 0.6);
      margin: 0;
    }
  </style>
```

The complete `<head>` section should now look like this (full reference, to avoid ambiguity):

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Komuna KI – Demnächst</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&display=swap" rel="stylesheet">
  <style>
    html, body {
      margin: 0;
      height: 100%;
      background: #000;
    }

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
      text-shadow: 0 2px 12px rgba(0, 0, 0, 0.6);
      margin: 0;
    }
  </style>
</head>
```

The `<body>` section is unchanged from Task 1.

- [ ] **Step 3: Verify the styled page in a desktop browser**

Run:

```bash
cd /home/csillag/deai/komuna-placeholder
python3 -m http.server 8000
```

Open `http://localhost:8000/` in a desktop browser.

Expected:
- Video fills the entire browser window. Aspect ratio preserved (no stretching) — overflow is cropped by `object-fit: cover`.
- One line of orange (`#ff7a1a`) text reads "Sehr geehrte Herrschaften, Ihr neuer Diener geht bald online!" centered horizontally, with its bottom edge roughly 12% above the bottom of the viewport.
- Font is Playfair Display (serif with high contrast between thick and thin strokes — distinguishable from Times). If you see something that looks like Times/Georgia, the Google Fonts request did not load — open DevTools → Network and look for the `fonts.googleapis.com` and `fonts.gstatic.com` requests.
- Text has a subtle dark shadow that keeps it legible regardless of the underlying video frame.

Stop the server with Ctrl+C.

- [ ] **Step 4: Commit**

```bash
cd /home/csillag/deai/komuna-placeholder
git add index.html
git commit -m "Style placeholder page with Playfair Display, orange text, lower-third layout"
```

---

### Task 3: Manual verification of responsive sizing and fallback behaviour

This task does not modify any files. It runs the verification checklist from the spec and records observations. If any check fails, fix the cause in `index.html` and re-verify; otherwise just confirm and commit nothing.

**Files:** none modified (verification only).

- [ ] **Step 1: Verify responsive text sizing at narrow viewport**

Run:

```bash
cd /home/csillag/deai/komuna-placeholder
python3 -m http.server 8000
```

Open `http://localhost:8000/` in a desktop browser. Open DevTools and use the device toolbar (Chrome: Cmd/Ctrl+Shift+M) to switch to a narrow viewport (e.g., iPhone SE width, 375px).

Expected:
- Text is smaller than at desktop size (because `clamp(1.5rem, 4.5vw, 3.25rem)` scales down).
- Text remains a single sensible block (it may wrap onto 2-3 lines, that is fine).
- Text remains readable (not truncated, not overflowing horizontally, not behind the address bar).

- [ ] **Step 2: Verify iOS-style inline playback (using DevTools mobile emulation)**

Still in the device toolbar, refresh the page. The video should:
- Autoplay.
- Stay inline (not go fullscreen).
- Remain muted.

(Full iOS device verification can be done separately if a real device is available; the `playsinline` attribute is the relevant guarantee.)

- [ ] **Step 3: Verify behaviour when the video fails to load**

In DevTools → Network tab, find the `komuna-bg.mp4` request, right-click → "Block request URL". Reload the page.

Expected:
- The viewport is solid black (from `body { background: #000 }`).
- The German text remains visible and readable in orange.

Then un-block the request.

- [ ] **Step 4: Verify fallback font when Google Fonts is blocked**

In DevTools → Network tab, add a request blocking rule for `fonts.googleapis.com` and `fonts.gstatic.com`. Reload.

Expected:
- The text appears in Georgia/serif (the CSS fallback chain).
- Text appears immediately — no period where the text is invisible (this is what `display=swap` guarantees).

Then un-block.

Stop the server with Ctrl+C.

- [ ] **Step 5: No commit needed**

Task 3 is verification only. If all steps pass, proceed. If any step revealed a problem, fix it in `index.html` and add a follow-up commit describing the fix.

---

## Self-Review Notes (for the writer of this plan)

Spec coverage check:
- File layout (spec §"File Layout") → Task 1 + Task 2 produce `index.html`; `public/komuna-bg.mp4` already exists.
- Browser tab title (spec §"Browser tab title") → Task 1, `<title>` element.
- Main text (spec §"Main text") → Task 1, `<h1>` content.
- HTML `lang="de"` (spec §"Language") → Task 1.
- Layout & typography (spec §"Visual Design") → Task 2, all CSS rules.
- Video behaviour (spec §"Video Behaviour") → Task 1 attributes (`autoplay muted loop playsinline`).
- Behavioural notes — no JS, black fallback, font swap, iOS playsinline (spec §"Behavioural Notes") → covered by Task 1+2 markup; verified by Task 3.
- Verification checklist (spec §"Verification") → Task 3 steps 1-4 mirror the spec's manual verification steps 1-5 (steps 4 and 5 of the spec are combined under Task 3 step 3 + step 4).

Placeholder scan: no TBD/TODO; every step contains the literal code or command needed.

Type consistency: class names (`bg-video`, `overlay`), file paths (`public/komuna-bg.mp4`), and HTML structure are identical across Task 1 and Task 2.
