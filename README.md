# Typing Speed Tester

A lightweight, single-page typing speed test built with plain HTML/CSS/JavaScript.

This README is geared for developers: how to run locally, where the logic lives, and what to change when you customize the prompt or scoring.

## Features

- **Instant start**: Timer starts on your first keypress in the textarea.
- **Exact-match finish**: Timer stops only when your input matches the full prompt text exactly.
- **Live correctness feedback** via the typing box border:
  - **Gray**: idle / reset state
  - **Blue**: correct so far (your text matches the prompt up to your current length)
  - **Orange**: mismatch (the most recent input diverges from the prompt)
  - **Green**: completed (full text matches)
- **Timer display** in `MM:SS:HS` (minutes : seconds : hundredths).
- **WPM result** displayed after completion.
- **Reset** button to clear input, reset timer, and remove results.

## Project Structure

```
index.html
css/
  style.css
js/
  script.js
```

- `index.html` contains the UI markup and the typing prompt text.
- `css/style.css` styles the layout and the feedback visuals.
- `js/script.js` contains the timer, validation (spell-check) logic, and WPM calculation.

## Local Development

There is no build step and no dependencies.

### Option A: Open the file directly

- Open `index.html` in a browser.

This is usually enough, but some environments apply extra restrictions to `file://` pages. If something behaves oddly, use Option B.

### Option B: Serve as a static site (recommended)

Run any static server from the repo root, then open the printed URL.

- Python: `python -m http.server`
- Node: `npx http-server`

If your team uses a local virtual host (for example, `http://typingspeedtest.localhost/`), point it at this folder and open that URL.

## How It Works

### Timing

- The timer starts when the textarea receives the **first keypress**.
- Timing updates every **10ms** and is rendered as minutes, seconds, and hundredths.
- The timer stops only when the typed content equals the full prompt text.

### Correctness Checking

On each `keyup`, the script compares:

- Your full input to the full prompt (completion check)
- Otherwise, your input to the matching-length substring of the prompt ("correct so far" check)

This is what drives the border color feedback.

### Words Per Minute (WPM)

WPM is computed after you finish:

- The prompt is treated as a fixed number of words using the common rule of thumb: **1 word = 5 characters**.
- The current implementation uses a hard-coded character count (`WORDS = 458 / 5`) rather than computing from the prompt at runtime.
- WPM is then calculated as:

$$\text{WPM} = \frac{\text{estimated words}}{\text{elapsed minutes}}$$

Note: because the app requires an exact full-text match, the WPM result is based on the entire prompt being completed.

## Customize

### Change the prompt text

Edit the paragraph inside the `#origin-text` element in `index.html`.

### Change colors / layout

Edit `css/style.css`. The typing feedback border colors are applied inline by JavaScript to the `.test-wrapper` element.

### Adjust WPM calculation

In `js/script.js`:

- The timer logic lives in `runTimer()`.
- Matching logic lives in `spellCheck()`.
- WPM is computed when the prompt is fully matched.

If you change the prompt text, you should also revisit the `WORDS` constant so WPM stays accurate.

## Notes / Limitations

- The completion condition is **strict**: capitalization, punctuation, spacing, and line breaks must match exactly.
- The timer starts on `keypress`; some mobile keyboards/IME input methods can behave differently than a desktop keyboard.
- There is no persistence—refreshing the page resets everything.

## Tech

- HTML
- CSS
- JavaScript (no frameworks)
