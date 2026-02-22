# Panic Attack App — Design Document
**Date:** 2026-02-18

## Overview

A single-file local web app that provides immediate, actionable calm-down techniques during a panic attack. Zero friction: open the file, click one button, get help.

## Platform

- **Format:** Single `panic.html` file
- **Hosting:** Local — open in browser, no internet required
- **Dependencies:** None

## UX Flow

### Screen 1 — Initial State
Full viewport, white background. One centered button:

```
[ I'm having a panic attack ]
```

Nothing else. No instructions, no navigation, no clutter.

### Screen 2 — Technique Display
After clicking, the button is replaced with a full-screen technique card in large, readable text:

```
5-4-3-2-1 Grounding

Name 5 things you can see.
4 things you can touch.
3 you can hear.
2 you can smell.
1 you can taste.

Take your time.

        [ Next technique ]
```

The "Next technique" button shuffles to a new random technique. Techniques cycle through all ~20 before repeating (Fisher-Yates shuffle).

## Visual Design

- **Style:** Ultra-minimal monochrome — pure white/black
- **Typography:** Large, readable, system font
- **Decoration:** None

## Content

~20 techniques across 4 categories, all equally likely to appear:

| Category | Examples |
|----------|---------|
| Breathing | Box breathing (4-4-4-4), 4-7-8 breath, slow exhale focus |
| Grounding | 5-4-3-2-1 senses, feet on floor, body scan |
| Cognitive | Name 3 facts, this feeling will pass, panic is not danger |
| Physical | Cold water on face, clench and release fists, run in place |
| Affirmations | Short, calm, non-toxic statements |

## Technical

- All content in a single JS array of objects `{ title, steps[] }`
- Fisher-Yates shuffle on load; cycles through full deck before reshuffling
- No localStorage, no tracking, no network calls
- Works offline
