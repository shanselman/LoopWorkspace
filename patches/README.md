# patches/ — Loop and Learn customizations (next-dev)

This directory contains the fork-specific patch applied automatically by
`.github/workflows/build_loop.yml` from the workspace root after recursive
submodules are checked out.

## Installed customization

### `loopandlearn-meal_days.patch`
- **Effect:** Changes the Carb Absorption history list/chart to show two days of meal history (today + yesterday) and labels prior-day entries with a relative date/time.
- **Source:** https://github.com/loopandlearn/customization/blob/main/meal_days/nextdev_meal_days.patch
- **Pinned upstream commit:** `022131536132ceb31d8388a4c0fc08fd15ab44e8` (`update meal days and week nextdev patch for latest Loop update (#78)`, 2026-07-13).
- **Submodule touched:** `Loop` (`Loop/View Controller.swift`).

## Native now-line support

The fork's former `loopandlearn-now_line.patch` has been retired because upstream LoopKit/LoopWorkspace now includes the chart now-line implementation and Loop's **Settings → Display → Now Line** option. The upstream default preserves the previous chart behavior; enable the setting in the app to display the line.

## Rollback

The meal-days patch is maintained in this fork. To remove it, revert the commit that introduced or last changed the patch; inspect the commit first and avoid reverting unrelated upstream merges.
