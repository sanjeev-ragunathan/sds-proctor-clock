# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

SDS Proctor Clock is a distraction-free, full-screen exam clock for use by SDS (Student Disability Services) proctors at Cornell. It is deployed as a GitHub Pages static site at `https://sanjeev-ragunathan.github.io/sds-proctor-clock/`.

## Development

No build step, no dependencies, no package manager. Open `index.html` directly in a browser to develop and test. All code lives in a single file.

## Architecture

Everything is in [index.html](index.html) — HTML structure, CSS, and JavaScript are all inline with no external dependencies.

**State variables** (top of `<script>`): `is24hr`, `isDark`, `showSeconds`, `showDate`, `showInfoBox`, `infoFontSize`, `isBold` — all plain JS booleans/numbers, no framework.

**Persistence**: Exam info text and text formatting preferences (font size, alignment, bold) are saved to `localStorage` under keys prefixed `sds-clock-*`. Nothing else is persisted across page loads.

**Theme switching**: Dark/light mode is applied by directly setting `document.body.style.background` and `document.body.style.color`. CSS selectors like `body[style*="background: white"]` key off these inline styles to re-theme child elements — this is intentional, not a smell.

**Menu UX**: The `☰` menu button and its dropdown are hidden by default (`opacity: 0`) and revealed on hover via CSS, with a JS-managed `.open` / `.active` class pair for click-to-pin behavior. Outside-click closes via a `document` click listener.

**Info box text controls** (A+/A−, alignment, bold) are hidden until the user hovers the `#info-box`, using the same CSS `opacity`/`max-height` transition pattern.
