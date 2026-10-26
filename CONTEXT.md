# Nova: free voice assistant PWA (project context)
Paste this file at the start of any AI coding session.

## Goal
A free, installable, KITT-style talking assistant that runs in Chrome (Android) and installs from the browser. Original name and look: no Knight Rider name, logos or voice (copyright risk).

## Hard constraints
- Zero cost: no paid APIs, no backend, static hosting only (GitHub Pages, Netlify, Cloudflare Pages).
- Plain HTML/CSS/JS, no build step. Mobile-first, one-hand use.
- Works offline for the shell; voice and Wikipedia need internet (Chrome speech runs on Google servers).
- Sinhala (si-LK) is a first-class language, not an afterthought.

## Files
- index.html: UI, command engine, voice, scanner (all inline)
- sw.js: cache-first service worker (bump `C` version on every release)
- manifest.webmanifest, icon-192.png, icon-512.png: installability

## Architecture
`listen()` -> SpeechRecognition -> `handle(text)` (ordered regex rules, first match wins) -> `say()` (speechSynthesis + chat log). Fallback for any unmatched text is `wiki()`. To add a command, add one rule in `handle()` before the Wikipedia fallback.

## Known browser limits (not bugs)
- Cannot open other apps, set system alarms or listen in the background. Opens websites and uses in-app timers instead.
- window.open after a voice result may be blocked, so every link is also shown as a tappable link.
- Sinhala speech output depends on the phone having a Sinhala TTS voice.

## Roadmap (one phase at a time, finish before the next)
1. DONE: UI, voice in/out, time/date/jokes/timer, open site, search, Wikipedia (en/si), QR scan.
2. QR generator (qrcode.js from cdnjs, cache it in sw.js). Done when "make a QR for <text>" shows a saveable image.
3. Text recognition (Tesseract.js, eng+sin). Done when a photo of a page is read aloud.
4. Face detection (MediaPipe Tasks). Done when the camera shows a box and Nova says how many faces.
5. Sinhala command phrases, news via RSS-to-JSON, settings (voice, name, speed).

## Test checklist
Install from Chrome menu; mic permission; "what is a galaxy"; "open youtube"; "timer for 10 seconds"; scan a QR; switch to Sinhala and ask a topic; airplane mode still loads the shell.
