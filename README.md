# POPP'EM

A browser and mobile game built entirely in Rive: whack-a-mole, dystopia, and one very stressed secret agent. This repository hosts the POPP'EM case study and the source assets it references.

Studio David Preli, 2026.

## What this repo is

This is a case-study-hosting repo, not the game source. It holds one self-contained case study page and the image assets that page embeds. The page is designed to be opened directly or embedded in the portfolio via iframe.

The playable game lives in Rive (embedded in the case study, see below). The Rive source file is not committed here.

## Contents

| File | Role |
| --- | --- |
| `popem-case-study.html` | The case study. Self-contained: all CSS and JS inline, fonts from Google Fonts, media from external hosts. |
| `facePre.png` | Original copic marker drawing of the face. The "before" in the comparator. 1.3 MB. |
| `face.svg` | Dithered vector version of the face. The "after" in the comparator. |
| `zit.gif` | Animated zit. The core game element, captioned in the page as "the main antagonist." |

Note the filename spelling: the file is `popem-case-study.html` (one P) while the game and title are `POPP'EM` (two P). This is a known inconsistency, left as-is to avoid breaking any links that already point at the file. See TODO.

## The case study page

`popem-case-study.html` is structured in five numbered sections plus header and footer:

- **00 The game**: the playable Rive embed, 1:1 aspect ratio.
- **01 Concept**: the fiction. An evil AI has seized humanity. One agent, named AGENT, has 59 seconds to pop enough pimples to blend in at the regime's final gala. The tone is deadpan and never breaks.
- **02 Build**: Data Binding as game engine. Four bound systems (clock, score, score-triggered SFX, win/fail), then a three-step pipeline (Illustrator, Rive, Logic Pro).
- **03 Philosophy**: the deadpan as a compositional choice. The joke only works if no one laughs.
- **04 Recognition**: Contra homepage feature, following submission to the Rive Game Challenge.

### Build summary, as documented in the page

Every game system runs on Rive Data Binding. No external scripting layer, no JavaScript game loop. The four bound systems:

1. **Game Clock**: a 59-second countdown started and reset through bound state.
2. **Score Tracking**: each pop increments a bound score value in real time.
3. **Score-triggered SFX**: sound effects fire at score milestones via Data Binding conditionals.
4. **Win and Fail Conditions**: pass and fail are calculated from bound values at the end of the run.

The state machine manages pimple visibility, pop animations, idle states, and the transitions between loading screen, game, and result. The fake loading screen is its own state, held for a fixed duration. It loads nothing. That is the point.

Pipeline: Adobe Illustrator (all vectors from scratch) to Rive (animation, state machine, Data Binding) to Apple Logic Pro (music and sound).

## External dependencies

The page pulls media from outside the repo. These are the live references in the source:

- **Rive playable embed**: `https://rive.app/s/jttHPxPXwEaCTcb9OCbEkQ/embed?runtime=rive-renderer`
- **Vimeo, Data Binding case study video**: `https://player.vimeo.com/video/1168624446`
- **Vimeo, state machine editor viewport**: `https://player.vimeo.com/video/1179093437`
- **Fonts**: IBM Plex Mono and DM Serif Display, via Google Fonts.
- **Vimeo Player API**: `https://player.vimeo.com/api/player.js`

The three local images are referenced by absolute `raw.githubusercontent.com` URLs pinned to this repo and the `main` branch, for example `https://raw.githubusercontent.com/StudioDavidPreli/poppEm/main/face.svg`. The page therefore renders its images correctly wherever it is embedded, but it is coupled to this repo path and the branch name `main`. Renaming the branch or moving the files will break the images.

## Embedding

The page is built to be embedded in a parent frame. It reports its own height upward so the parent iframe can resize to fit:

- On load and at intervals (500 ms, 1500 ms, 4000 ms), it posts `{ type: 'resize', height }` to `window.parent`.
- It also listens for an inbound `{ type: 'resize', height }` message and, if it finds a frame with id `popem-cs-frame`, sets that frame's height to `height + 200`.

The parent side of this contract (an iframe with id `popem-cs-frame`) lives in the portfolio, not in this repo.

## The before/after comparator

Section 02 includes a vertical drag comparator over the face asset. `facePre.png` is the base layer (full height). `face.svg` is overlaid and clipped from the top. A draggable bar sets the clip height, revealing the vector version above the original below. It supports mouse and touch.

One touch behaviour to be aware of: `touchmove` is registered passive, so dragging the comparator on a phone does not suppress page scroll. Dragging and scrolling can fight. See TODO.

## Credits

- Built by Studio David Preli. [davidpreli.com](https://davidpreli.com)
- "Pop" sound effect: Joey at [School of Motion](https://www.schoolofmotion.com).

## TODO / unverified

These could not be confirmed from the committed source and should be checked before relying on them:

- **Hosting**: whether the page is published via GitHub Pages (and at what URL) is not verifiable from the repo contents. Confirm Pages status, or document the actual host the portfolio iframe points at.
- **Filename spelling**: decide whether to keep `popem-case-study.html` or rename to match `POPP'EM`. If renamed, update every inbound link first.
- **Comparator scroll-lock on mobile**: confirm whether the passive `touchmove` causes a drag-versus-scroll conflict on target devices, and whether to fix.
- **`facePre.png` weight**: 1.3 MB, loaded eagerly in the comparator. Confirm whether to compress or lazy-load for mobile.
