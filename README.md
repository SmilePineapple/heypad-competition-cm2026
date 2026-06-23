# Jabbla UK — Win a HeyPad (Communication Matters Conference 2026)

A single-page, branded competition site for the Jabbla UK stand at **Communication Matters Conference 2026**. Visitors answer a short quiz about Jabbla products, then enter a prize draw to win a **HeyPad** AAC communication device.

## Features
- Jabbla-branded design (petrol/teal + lime accent, official logo, "We all have a voice").
- 5-question interactive quiz with instant feedback, explanations and a scored result.
- Prize-draw entry form with **required in-person attendance confirmation** and contact consent.
- Entries submit silently to a **Google Form** (linked Google Sheet), with a `localStorage` backup.
- Fully responsive; works offline when opened directly (fonts via CDN).

## Tech / patterns
- Plain **HTML + CSS + vanilla JS** in a single self-contained `index.html` (no build step).
- Google Fonts: Poppins (headings) + Open Sans (body).
- Google Forms integration via `fetch` (`no-cors` + `FormData`) — config lives in the `GOOGLE_FORM` object in `index.html`.

## Local preview
```bash
python -m http.server 8123
# then open http://localhost:8123
```
Or simply open `index.html` in a browser.

## Updating the Google Form mapping
Field IDs are positional to the linked Google Form questions (Name, Email, Organisation/role, Quiz score, Attending, Consent). If the form is recreated or reordered, update the `entry.*` IDs in the `GOOGLE_FORM` config.

## Assets
- `Jabbla_UK_2021.jpg` — official Jabbla UK logo.
- `HeyPad-EN-side-3-no-background.png` — HeyPad hero render.
- `HeyPad-EN-side-2-1024x771.png` — additional HeyPad image.
