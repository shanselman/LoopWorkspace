# patches/ — Loop and Learn customizations (next-dev)

This directory holds patch files applied automatically by `.github/workflows/build_loop.yml`
during the "Customize Loop" step (`git apply ./patches/* --allow-empty -v --whitespace=fix`,
run against the recursive-submodule checkout of this repo). No workflow edits were required
to enable these two patches — the existing "apply everything in patches/" step picks them up.

## Installed customizations

### `loopandlearn-now_line.patch`
- **Effect:** Adds a vertical "now" guide line to the COB, Carb Effect, Dose, Glucose+Carb,
  IOB, Legacy Dose, and Predicted Glucose charts in Loop's chart UI, so the current time is
  visually marked on every chart. No numeric clamping or scaling behavior is changed.
- **Source:** https://github.com/loopandlearn/customization/blob/main/now_line/nextdev_now_line.patch
- **Pinned upstream commit:** `f3c6ca57abf5552a56f206731ba67ca197ab020d`
  (`now_line: add version compatible with Loop next-dev (#70)`, 2026-07-13)
  This is the `nextdev_now_line.patch` variant, matched to this fork's `next-dev` branch
  (as opposed to the `main`/`dev`-targeted `now_line.patch`).
- **Submodule touched:** `LoopKit` (`LoopKitUI/Charts/*.swift`)

### `loopandlearn-meal_days.patch`
- **Effect:** Changes the Carb Absorption history list/chart to show **two days** of meal
  history (today + yesterday) instead of only the current day, and labels prior-day entries
  with a relative date/time (e.g. "Yesterday, 8:00 PM") instead of a bare time. This is the
  two-day `meal_days` variant, explicitly **not** the `meal_week` (7-day) variant.
- **Source:** https://github.com/loopandlearn/customization/blob/main/meal_days/nextdev_meal_days.patch
- **Pinned upstream commit:** `022131536132ceb31d8388a4c0fc08fd15ab44e8`
  (`update meal days and week nextdev patch for latest Loop update (#78)`, 2026-07-13)
  This is the `nextdev_meal_days.patch` variant, matched to this fork's `next-dev` branch.
- **Submodule touched:** `Loop` (`Loop/View Controllers/CarbAbsorptionViewController.swift`)

Both files were downloaded verbatim (byte-for-byte, via `raw.githubusercontent.com`) at the
pinned commit SHAs above — they are not hand-edited. No other loopandlearn/customization
patches (e.g. chart-clamp/`override_sens`/etc.) were added.

## Rollback

These two patches were added in a single dedicated commit. To remove both customizations
and return to stock next-dev behavior:

```bash
git revert <this-commit-sha>
```

then re-run `build_loop.yml` on `next-dev`. This deletes both patch files (and this README)
in one clean revert commit; no other files are touched, so the revert is a simple,
single-command rollback with no manual patch surgery required.
