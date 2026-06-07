# Loop Browser Build — Custom Bundle ID

This is Scott Hanselman's fork of LoopKit/LoopWorkspace, configured for GitHub Actions browser build → TestFlight.

## Critical: Custom Bundle ID

This fork uses a **custom bundle identifier** that differs from the upstream default.

| Setting | Value |
|---------|-------|
| **MAIN_APP_BUNDLE_IDENTIFIER** | `com.hanselmanloop.loopkit` |
| **App bundle ID** | `com.hanselmanloop.loopkit.Loop` |
| **TEAMID** | `TN99MCU5V6` |
| **Apple Developer account** | Same as local Mac/Xcode builds |

The custom bundle ID is set in two places:
1. `LoopConfigOverride.xcconfig` — sets `MAIN_APP_BUNDLE_IDENTIFIER = com.hanselmanloop.loopkit`
2. `fastlane/Fastfile` — all `com.#{TEAMID}.loopkit` references replaced with `com.hanselmanloop.loopkit`

## Why This Matters

The locally-built (Mac/Xcode) Loop app uses `com.hanselmanloop.loopkit.Loop`. If the browser build uses a different bundle ID, iOS treats it as a different app and **all settings, pump connections, and therapy data are lost**.

## After Upstream Sync

The build workflow auto-syncs with `LoopKit/LoopWorkspace` every Sunday. If the upstream `fastlane/Fastfile` changes, the merge may:
- **Succeed silently** — our changes survive (most likely)
- **Conflict** — the sync or build will fail

### Fix if Fastfile Gets Overwritten

```bash
# Clone the fork
gh repo clone shanselman/LoopWorkspace
cd LoopWorkspace

# Re-apply the custom bundle ID (replaces all occurrences)
sed -i '' 's/com\.#{TEAMID}\.loopkit/com.hanselmanloop.loopkit/g' fastlane/Fastfile

# Also ensure LoopConfigOverride.xcconfig has the override uncommented:
# MAIN_APP_BUNDLE_IDENTIFIER = com.hanselmanloop.loopkit

# Commit and push
git add fastlane/Fastfile LoopConfigOverride.xcconfig
git commit -m "Re-apply custom bundle ID (com.hanselmanloop.loopkit)"
git push

# Then trigger build
gh workflow run "4. Build Loop" -R shanselman/LoopWorkspace
```

## Build Configuration

| Variable | Value | Purpose |
|----------|-------|---------|
| `SCHEDULED_BUILD` | `false` | No auto-build; must trigger manually |
| `SCHEDULED_SYNC` | (default: enabled) | Auto-syncs code from upstream weekly |
| `ENABLE_NUKE_CERTS` | `true` | Auto-renews expired certificates |

## How to Rebuild

```bash
gh workflow run "4. Build Loop" -R shanselman/LoopWorkspace
```

TestFlight builds expire after **90 days** — rebuild at least once per quarter.

## Secrets (6 required)

All configured as repository secrets: `TEAMID`, `FASTLANE_ISSUER_ID`, `FASTLANE_KEY_ID`, `FASTLANE_KEY`, `GH_PAT`, `MATCH_PASSWORD`. Do not log or expose these values.
