# Typing Speed Tester (Vanilla JS)

A lightweight, single-page typing speed test built with plain HTML/CSS/JavaScript. The test starts timing when you begin typing, provides live correctness feedback as you type, and stops only when you match the prompt text **exactly**—then it displays your typing speed in WPM.

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

- The prompt text is treated as a fixed number of words using the common rule of thumb:
  - **1 word = 5 characters**
- The script uses a fixed character count for the prompt and divides by 5.
- WPM is then calculated as:

$$\text{WPM} = \frac{\text{estimated words}}{\text{elapsed minutes}}$$

Note: because the app requires an exact full-text match, the WPM result is based on the entire prompt being completed.

## Run The Project

### Option A: Open the file directly (simplest)

1. Open `index.html` in your browser (double-click it in File Explorer).
2. Start typing in the textarea.

### Option B: Serve as a static site (recommended)

Use any static file server you like and open the served URL in your browser.

Examples:

- Python:
  - `python -m http.server`
- Node (http-server):
  - `npx http-server`

Then browse to whatever address the server prints.

## Customize

### Change the prompt text

Edit the paragraph inside the `#origin-text` element in `index.html`.

### Change colors / layout

Edit `css/style.css`. The typing feedback border colors are applied inline by JavaScript to the `.test-wrapper` element.

### Adjust WPM calculation

In `js/script.js`:

- The timer logic lives in `runTimer()`.
- Matching logic lives in `spellCheck()`.
- WPM is computed at completion.

If you change the prompt text length significantly, you may want to update how the app estimates word count so it reflects the actual prompt length.

## Notes / Limitations

- The completion condition is **strict**: capitalization, punctuation, spacing, and line breaks must match exactly.
- The timer starts on `keypress`; certain input methods (e.g., mobile keyboards/IME) may behave differently than a desktop keyboard.
- There is no persistence—refreshing the page resets everything.

## Tech

- HTML
- CSS
- JavaScript (no frameworks)
