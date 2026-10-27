# Fork-specific LoopWorkspace customizations

The build workflow applies every patch in this directory after checking out recursive submodules.

## Retired customization

`loopandlearn-meal_days.patch` has been removed. It extended Carb Absorption history to include the previous day's meals and label entries with relative dates. The requested build will use upstream behavior and no longer apply that patch.

The former `loopandlearn-now_line.patch` was separately retired when upstream added native **Settings → Display → Now Line** support. Enable that setting in Loop if you want the now-line visible.

## Fork safeguards

The fork-specific bundle identifiers and active compilation settings are maintained in `LoopConfigOverride.xcconfig` and `fastlane/Fastfile`; review them before every upstream sync/build. Keep build-workflow-specific security and synchronization checks intact.
