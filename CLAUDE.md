# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

SDS Proctor Clock is a distraction-free, full-screen exam clock for use by SDS (Student Disability Services) proctors at Cornell. It is deployed as a GitHub Pages static site at `https://sanjeev-ragunathan.github.io/sds-proctor-clock/`.

## Development

No build step, no dependencies, no package manager. Open `index.html` directly in a browser to develop and test. All code lives in a single file.

## Architecture

Everything is in [index.html](index.html) — HTML structure, CSS, and JavaScript are all inline with no external dependencies.

**State variables** (top of `<script>`): `is24hr`, `isDark`, `showSeconds`, `showDate`, `showInfoBox`, `infoFontSize`, `isBold` — all plain JS booleans/numbers, no framework.

**Persistence**: On page load, `loadPersistentState()` runs immediately to restore saved settings before rendering. Exam info text (stored in `#info-text` textarea) and formatting preferences (font size, alignment, bold) are saved to `localStorage` with keys prefixed `sds-clock-*`. The textarea auto-saves on input via `saveInfoText()`. Toggle functions (`toggleBackground`, `toggleFormat`, etc.) explicitly save their state to localStorage. First-time visitors get `DEFAULT_INFO_TEXT` as a placeholder; this is persisted on first edit.

**Theme switching**: Dark/light mode is applied by directly setting `document.body.style.background` and `document.body.style.color`. CSS selectors like `body[style*="background: white"]` key off these inline styles to re-theme child elements — this is intentional, not a smell.

**Menu UX**: The `☰` menu button and its dropdown are hidden by default (`opacity: 0`) and revealed on hover via CSS, with a JS-managed `.open` / `.active` class pair for click-to-pin behavior. Outside-click closes via a `document` click listener.

**Info box text controls** (A+/A−, alignment, bold) are hidden until the user hovers the `#info-box`, using the same CSS `opacity`/`max-height` transition pattern.

**Textarea auto-resize and scroll**: The `#info-text` textarea auto-expands to fit content via the `autoResize()` function, which measures the element's position in the viewport (via `getBoundingClientRect().top`), collapses it to `1px` to read its `scrollHeight`, then caps the final height to prevent overflow (viewportMax = window.innerHeight − topOfBox − 10px padding). When content exceeds this cap, `overflowY` switches to `auto` for internal scrolling. The `#resize-bar` (a draggable handle below the textarea) becomes visible when text overflows; dragging it adjusts textarea height within the same viewport constraints. Drag behavior uses mouse and touch event handlers to calculate height delta from start position, with a minimum height of 60px enforced.
