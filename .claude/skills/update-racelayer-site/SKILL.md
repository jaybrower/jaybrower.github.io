---
name: update-racelayer-site
description: Use this skill when updating the RaceLayer project page (`racelayer/index.html`) to reflect a new RaceLayer release. Trigger when the user asks to "update the RaceLayer site / page / docs" for a release, references release notes from the iracing-overlay repo, or pastes RaceLayer release-notes markdown and asks for the site to reflect it.  Do NOT trigger for the root site (`index.html`) or other subprojects.
---

# Update RaceLayer Site for a Release

## What this skill does

Translates a RaceLayer release-notes file (e.g. `c:\code\iracing-overlay\release-notes\v0.1.4.md`) into edits on `racelayer/index.html`, then commits and pushes to GitHub Pages.

The release-notes file is the authoritative input. **Only user-facing changes go on the site.** Internal / release-process changes (CI workflows, gitignore entries, branch policy updates, dist scripts, etc.) do not belong here.

## Source of truth

- **Release notes file** is typically at `c:\code\iracing-overlay\release-notes\vX.Y.Z.md`. If the user doesn't supply one, ask which version they're updating for and check that file.
- The site has **no version number** hardcoded — the "Download Latest" button uses GitHub's `/releases/latest` redirect, so no version update is needed there.

## What's user-facing vs. internal

| Release-notes section | Update site? |
|---|---|
| **Added** (new overlay features, new visual elements, new toggles) | **Yes** |
| **Changed** (visible behaviour changes) | **Yes** |
| **Fixed** (user-visible bug fixes) | Usually no — only if the bug affected normal user workflow |
| **Internal** | **No** — these are dev/release-process changes |

Examples of "yes" — visible to users: stint-scoped tire-deg, cockpit-only rendering, closing-rate column, pit-mode behaviour, new overlay, new keyboard shortcut, new config toggle, new visual styling.

Examples of "no" — invisible to users: GH Actions workflows, gitignore entries, `dist:pre` script, version bumps, CLAUDE.md updates, release-notes folder conventions, label conventions.

## Sections of `racelayer/index.html`

1. **Hero** (`<section class="hero">`) — h1, tagline, download buttons, chips
   - **Chips** (`<span class="chip">`) are short marquee labels like "Pit-aware", "Stint-aware", "In-app updates". A truly significant feature can earn a chip; minor changes shouldn't add chips (keep the list tight).
2. **Overlays grid** (`<section class="section">` with `<div class="overlay-grid">`) — four cards: Relative, Gauges, Pit Strategy, Tire Temps. Each has an `<h3>` and a `<ul>` of bullets.
   - Edit the relevant card's bullet list when an overlay gains/changes a feature.
3. **Features list** (`<div class="feature-list">`) — cross-cutting features with emoji icons (launch on startup, layout mode, per-session visibility, etc.).
   - Add a new `<div class="feature-item">` for a significant new cross-cutting feature.
4. **Install section** — installer + portable cards, quick-start steps. Rarely needs updating.
5. **Footer** — almost never needs updating.

## Update procedure

1. **Read** the release-notes file the user references (or ask which version).
2. **Read** `racelayer/index.html`.
3. For each user-facing bullet in the release notes, decide:
   - Does it belong on an existing overlay card? → edit that card's `<ul>`.
   - Is it a new cross-cutting feature? → add a new `<div class="feature-item">` to the features list.
   - Is it a marquee feature worth a chip? → add one `<span class="chip">` (sparingly; the chip row is curated, not exhaustive).
4. **Don't touch** anything not driven by the release. Resist refactoring CSS, restructuring sections, or "improving" copy that isn't related to the release.
5. **Skip** anything in the Internal section of the release notes.

## Style conventions

- Feature item descriptions: one sentence, conversational, ends with a period.
- Overlay-card bullets: short noun phrases without trailing punctuation.
- Chips: 1–3 words, no punctuation.
- Emoji icons in feature items match the visual register of the existing ones (small, common emoji — no fancy unicode).
- Match the existing tense/voice ("Overlays detect…", "Pit Strategy compares…", not "We added…").

## Commit and push

After edits:

```bash
cd /c/code/oiddad.github.io
git add racelayer/index.html
git commit -m "docs(racelayer): <one-line summary of what changed for vX.Y.Z>

- <bullet for each meaningful change>"
git push origin main
```

**Commit message conventions:**
- Prefix: `docs(racelayer):`
- One-line summary mentions the version *or* the headline feature
- Body bullets describe what was edited and why

GitHub Pages picks up the change within ~1 minute of the push.

## What NOT to update

- The version in the install-card placeholders (`RaceLayer-x.x.x.exe`) — these are intentional placeholders, kept generic
- The Download Latest button — it uses the GitHub `/releases/latest` redirect
- The README-style "Windows 10/11" / "Free" badges — these are platform/license facts, not release content
- The root `c:\code\oiddad.github.io\index.html` — that's the landing page for all projects, not RaceLayer-specific

## Failure modes to watch for

- **Over-updating** — adding chips for minor changes, padding the feature list with everything, restating bullets that already exist. Be ruthless about user value.
- **Under-updating** — missing a feature that genuinely changes what the user sees (e.g. a new column in an overlay, a new visual indicator). Read each release-notes bullet and explicitly decide skip/include.
- **Leaking internal language** — phrases like "irsdk_bool", "telemetry context", "BrowserWindow", or PR/issue numbers belong in CLAUDE.md and release-notes, not on the marketing site.
- **Stale bullets** — if a release *changes* how a feature works (e.g. v0.1.3 replaced "tire deg vs session-best" with "stint-scoped"), edit the existing bullet rather than appending a new one.

## Example diff shapes

**A new visual indicator in an existing overlay** — add a bullet to the relevant overlay card's `<ul>`, no chip, no feature-list item (unless it's marquee).

**A new behaviour that applies across overlays** (like cockpit-only rendering) — add a feature-list item, no overlay-card edits, possibly a chip.

**A revised mechanic in an existing feature** (like stint-scoped vs. session-best tire deg) — edit the existing overlay-card bullet AND add a feature-list item if the new mechanic deserves cross-cutting visibility. Don't leave the old wording in.
