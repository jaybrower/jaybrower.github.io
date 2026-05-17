# RaceLayer site — screenshot checklist

Tracking captures for the v0.2 site update. Drop new PNGs in this folder and tick items off as we wire them in.

## Filename convention

`<group>-<variant>.png`, where:
- `01-XX` — Hero showcase (cockpit / in-game beauty shots)
- `02-XX` — Per-overlay tight crops
- `03-XX` — Feature callouts (Preview Mode, Layout Mode, etc.)

## Status

### Hero (01-XX) — _wired into the slideshow_
- [x] `01-01.png` — wet conditions, multi-car ahead
- [x] `01-02.png` — clear conditions, single car ahead
- [x] `01-03.png` — close traffic, multiple cars visible

### Per-overlay (02-XX)

Naming pattern: `02-<overlay>-NN.png` — capture 1–3 variants per overlay if easy, we'll pick the strongest for the card.

- [x] **Gauges** — `02-gauges-01.png` ✓ wired (ABS active, RPM full, all readouts populated)
  - [ ] nice-to-have: `02-gauges-NN.png` with RPM bar at shift point (zone color + flash visible)
  - [ ] nice-to-have: `02-gauges-NN.png` with TC indicator flashing
- [ ] **Relative** — `02-relative-NN.png` (ideally with side-indicator chevrons firing)
- [ ] **Pit Strategy** — `02-pit-NN.png` (mid-race with at least one full stint recorded)
- [ ] **Tire Temps** — `02-tires-NN.png` (a few laps in so all four corners have real color)
- [ ] **Radar** — `02-radar-NN.png` (ideally a car alongside)

### Optional (03-XX)
- [ ] `03-preview.png` — Preview Mode toggle or a session running on simulated data

## Capture tips

- **Format:** PNG.
- **Resolution:** at least **2× the display size** so retina screens render crisply. Aim for 1200px wide on per-overlay crops, 1600–2400px wide on hero. (Current `01-XX` set is ~600px wide — fine for 1x but slightly soft on retina; consider re-capturing larger when convenient.)
- **Crops:** tight on the overlay box for `02-XX`; full cockpit/screen for `01-XX`.
- **Conditions:** vary lighting / track / traffic across hero shots so the slideshow doesn't feel repetitive.
- **Data populated:** for `02-XX`, capture mid-session so overlays have real numbers (lap times, deg trend, tire colors) rather than initial blanks.
