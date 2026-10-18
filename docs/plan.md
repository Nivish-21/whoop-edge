# Restore iPhone Edge launch — 2026-08-30

Goal: restore normal home-screen launch of the installed `com.nivish.openstrapEdge`
build on Nivi’s iPhone without deleting its local health data, then confirm it is
running release `0.9.28+60` from this checkout.

Diagnosis: the iPhone has Edge `0.9.28 (60)` installed, matching `main` at
`c02faa3`. The iOS alert appears before Flutter starts. The installed app uses a
personal bundle identifier, so the failure is an invalid/expired personal signing
entitlement, not an Edge runtime or UI defect.

- [ ] Read the ignored local signing override and verify that its bundle identifier
  matches the installed app; do not expose credentials or change signing values.
- [ ] Run the smallest safe iOS build preflight, then re-sign and replace the existing
  app in release mode without uninstalling it.
- [ ] Launch Edge from the iPhone home screen after disconnecting Flutter/Xcode, and
  verify the installed version, launch behaviour, and current UI.
- [ ] Run the relevant lint/format checks, record the result in `docs/status.md`, and
  append the completed work to `docs/changelog.md`.

## Correction and UI comparison — 2026-08-30

Correction: matching `0.9.28+60` establishes that the installed source release is
current; it does not establish that its rendered UI matches the repository screenshots.
The supplied screenshot is an iOS launch-denial alert, not an Edge screen.

- [ ] Restore a launchable release build without uninstalling the current app or
  deleting local data.
- [ ] Capture the actual first in-app screen and compare it with the README screenshots
  and the active `lib/ui2` route tree.
- [ ] Trace any confirmed mismatch to a route, preference, feature gate, or stale build;
  create a separate approved fix plan only if source changes are needed.

## Signing identity recovery — 2026-08-30

Blocker: the provisioned `com.nivish.openstrapEdge` identifier belongs to Apple Team
`V2B9C748M5`; the sole valid local Apple Development certificate belongs to Team
`2ALA6YRM9K`. Installing with that other identity may require deleting the current app
container and its local health data. Do not uninstall or replace the app across teams.

- [ ] Recover a valid Apple Development signing identity for Team `V2B9C748M5`, then
  recreate the ignored signing override using the existing bundle/App Group identifiers.
- [ ] Rebuild and replace Edge under the matching team, preserving the app container.
- [ ] If recovery is impossible, obtain explicit confirmation before deleting the current
  installation and explain that its unrecoverable local health data will be removed.

## Signing evidence correction — 2026-08-30

The iOS Team Provisioning Profile confirms that the App-ID prefix `V2B9C748M5` is
compatible with the included Apple Development certificate for Team `2ALA6YRM9K`.
It authorises `com.nivish.openstrapEdge`, App Groups, HealthKit, and
`group.com.nivish.openstrap`. The earlier cross-team replacement blocker is cleared.

- [x] Recreate the ignored signing override from the verified provisioning-profile
  bundle, certificate-team, and App Group values.
- [ ] Build, install, and launch the release app over the existing installation without
  uninstalling it.
- [ ] Capture and compare the actual Edge UI against the repository reference.

## Xcode account authentication blocker — 2026-08-30

Build evidence: Xcode reports `No Account for Team "2ALA6YRM9K"` for Runner,
OpenStrapWidgetExtension, and OpenStrapWatch Watch App. It consequently cannot locate or
refresh profiles for their matching identifiers. The local certificate and profile are not
sufficient without an authenticated Xcode account for that team.

- [ ] Sign in or reauthenticate the Apple ID owning Team `2ALA6YRM9K` in Xcode Settings
  → Accounts, then select the team in the project signing settings.
- [ ] Re-run the signed release build and install over the existing Edge app without an
  uninstall.
- [ ] Confirm a home-screen launch and compare the live Edge UI with the repository UI.

## Xcode account refresh — 2026-08-30

The terminal and visible UI both use `/Applications/Xcode.app` version `26.6`, so this is
not an alternate-Xcode selection. Despite the displayed Personal Team, command-line
Xcode still cannot resolve it. Refresh manual profiles and restart the Xcode account
session before another build attempt.

- [ ] In Xcode Settings → Apple Accounts → Nivish V, use **Download Manual Profiles**;
  then quit and reopen Xcode and `ios/Runner.xcworkspace`.
- [ ] Confirm signing resolves profiles for Runner, OpenStrapWidgetExtension, and
  OpenStrapWatch Watch App under Team `2ALA6YRM9K`.
- [ ] Re-run the signed release build and preserve the existing iPhone app container.

## Signing team correction — 2026-08-30

Root cause: `ios/Config/Signing.xcconfig` set `APPLE_DEVELOPMENT_TEAM` to the
certificate identifier `2ALA6YRM9K`. Xcode requires the provisioning profile's
`TeamIdentifier`, `V2B9C748M5`; the certificate identifier is not the value for the
build setting. This incorrect configuration caused all three `No Account for Team
2ALA6YRM9K` failures.

- [x] Change only `APPLE_DEVELOPMENT_TEAM` in the ignored signing override to
  `V2B9C748M5`.
- [ ] Build and install the Release app over the existing app without uninstalling it.
- [ ] Verify the home-screen launch, then capture and compare the live UI before any
  UI-source changes are considered.

## iPhone developer-profile trust — 2026-08-30

The new build installed and its signature verifies locally, but iOS rejected launch with
`Unable to launch ... because ... its profile has not been explicitly trusted by the user`.
The device must explicitly trust the newly signed developer profile before any Flutter code
can run.

- [ ] On Nivi's iPhone, trust the developer profile for `nivishv2004@icloud.com` in
  Settings → General → VPN & Device Management.
- [ ] Launch Edge from the home screen and capture the first in-app screen for the UI
  comparison.

## UI-reference outcome — 2026-08-30

Evidence: the README screenshot assets were last updated in commit `4cd025d` on
2026-07-12. Commit `4203370` on 2026-08-15 removed the legacy `lib/ui` implementation
and replaced it with `lib/ui2`, including the Home · Health · Nutrition · Workout ·
Wellness shell now seen on the phone. The README still presents the deleted legacy UI.

- [x] Restore the iPhone launch and capture the live Home screen.
- [x] Compare the live screen with the README reference and trace the discrepancy.
- [x] Establish that the README screenshots are stale after the UI rewrite, rather than
  treating the live app as an outdated installation.

## Legacy-interface restoration — pending approval — 2026-08-30

Assumption: restore the README's Today · Sleep · Heart · Stress · Breathing · Body ·
Steps · Workouts · Records · Recap · Profile information architecture and visual language,
while preserving every currently shipped capability and the current project invariants. This
is a redesign of `lib/ui2`, not a source revert: the deleted legacy UI discarded material
data-confidence and absence behaviour that must remain intact.

- [ ] Install Taste v2 (`design-taste-frontend`) and use it with Impeccable: Taste owns
  visual-direction critique; Impeccable owns native iOS interaction, accessibility, and
  design-system execution.
- [ ] Complete the Impeccable product/design discovery records, audit every current
  route, notification deep link, settings flow, and data state, and create a one-to-one
  legacy-screen-to-current-capability wiring map before changing UI code.
- [ ] Establish a single replacement visual system from the legacy captures, then rebuild
  the shell and all mapped screens without reviving deleted compute, decode, or storage
  paths.
- [ ] Add or update golden, routing, accessibility, empty/loading/error, and interaction
  tests for every rebuilt surface; verify no screen fabricates a metric when input is absent.
- [ ] Validate on the physical iPhone: tab navigation, deep links, pairing/sync, derived
  data, Health flows, notifications, widgets/Watch hand-off, dark mode, Dynamic Type, and
  reduced motion.
- [ ] Refresh README screenshots only from the verified rebuilt app; run format, analyse,
  focused and full test suites; append changelog/status/decision records.

## Fresh-signing replacement — 2026-08-30

User direction: prefer a new signing identity and does not require the existing local data.
This requires deleting the installed `com.nivish.openstrapEdge` application before iOS can
accept a differently signed replacement.

- [ ] Obtain explicit destructive-operation confirmation naming the app and all local data
  that will be permanently deleted.
- [ ] Delete the installed Edge app from Nivi's iPhone, then build and install a freshly
  signed replacement.
- [ ] Verify launch and capture the actual in-app UI for comparison with the repository.

## UI UX Pro Max integration — pending approval — 2026-08-30

User direction: use `nextlevelbuilder/ui-ux-pro-max-skill` for the legacy-interface
restoration. It will provide Flutter-oriented UI generation and pattern guidance. Taste
continues to audit visual direction, while Impeccable and the repository invariants retain
authority over native iOS conventions, accessibility, data honesty, routing, and complete
capability wiring.

- [ ] Install the upstream skill using the supplied installer and read its complete
  `SKILL.md` before design or implementation actions that rely on it.
- [ ] Reconcile its output with the existing UI2 route and capability inventory; reject
  any generated pattern that would create a second navigation path, fake a metric, obscure
  iOS affordances, or bypass existing tests.
- [ ] Record the resulting division of responsibility in the design decision record,
  then continue the already-approved legacy-interface restoration plan.

### Progress — 2026-08-30

- [x] Installed `ui-ux-pro-max` and read its complete `SKILL.md`.
- [x] Generated and inspected its persisted Flutter design-system proposal, then rejected
  its landing-page, lavender and neumorphic visual prescriptions as incompatible with the
  approved native health-app direction.
- [x] Recorded the tool division and visual-system decision in `docs/decisions.md`.

### Legacy-restoration progress — 2026-08-30

- [x] Captured the product record and audited the current UI2 routes, deep links and
  capability ownership against the legacy five-tab model.
- [x] Replaced the shell anchors with Today · Sleep · Heart · Body · Workouts without
  restoring deleted UI code or creating a second data path. Sleep is embedded through
  its existing detail flow; Nutrition remains available from Profile and its existing
  deep link.
- [x] Added route and interaction coverage for the restored shell and retained
  Nutrition entry point, then ran the focused UI suites successfully.
- [ ] Apply the warm legacy token system across the rebuilt surfaces and inspect it on
  the physical iPhone before closing the restoration.

## Shell interaction-test correction — pending approval — 2026-08-30

The newly added shell interaction test failed because it searched for a semantic label that
`Pressable` does not expose to Flutter's finder. This is a test harness fault, not an app
navigation failure: the existing shell still rendered and all token/accessibility checks passed.

- [x] Replaced the incorrect finder with the shell's actual tappable widget boundary, then
  reran the focused shell, routing, token and contrast suites (257 passed; 192 visual
  golden cases correctly skipped because their image baselines are absent).

## Fresh-profile device launch — awaiting iPhone trust — 2026-08-30

The signed release built and installed successfully, but iOS denied process launch before
Flutter initialised because the fresh developer profile has not been explicitly trusted.

- [ ] Trust the developer profile for `nivishv2004@icloud.com` on Nivi's iPhone, then
  launch `com.nivish.openstrapEdge` and inspect the rebuilt interface.

### Launch follow-up — 2026-08-30

- [x] Developer profile trust completed on the iPhone.
- [ ] Keep the iPhone unlocked while connected, then launch and inspect the release.

## Sync progress and data-freshness investigation — pending approval — 2026-08-30

User report from the freshly installed, trusted release: Today truthfully says that no band
recordings have been processed, but tapping “Sync the band” gives no visible in-progress,
retry or failure state. A completed band sync is not reflected promptly in the derived-data
read model. The UI restoration is paused here: a prettier empty state cannot compensate for
an invisible or stale sync path.

Success criteria: a sync request has a visible state immediately; every BLE/persist/derive
boundary is observable; a completed persisted sync schedules and exposes fresh derived data
without inventing a metric; the connected iPhone demonstrates the result. "As fast as WHOOP"
is not a valid unmeasured target because radio, firmware and history size differ—this work
will instead measure each boundary and remove the proven delay.

- [ ] Reproduce with the connected band and capture the exact AppState, BLE, persistence,
  derivation and repository-read transitions. Add temporary diagnostic instrumentation only
  where existing logs cannot identify the boundary; do not alter sync behaviour yet.
- [ ] Trace the explicit Sync action and the automatic drain path end-to-end, compare them
  with an already-working sync/recompute path, and establish one evidence-backed root cause
  for missing feedback and one for stale/no derived data (they may be the same cause).
- [ ] Write failing regression tests for the proven state transition(s), including loading,
  error/retry, commit-before-ack ordering and an honest no-data outcome.
- [ ] Apply the smallest root-cause fix one boundary at a time, verify focused tests after
  each step, then measure a physical-band sync from tap through data appearing on Today.
- [ ] Run format, analyse and relevant full tests; update status, changelog, decisions and
  this plan only after the iPhone evidence confirms the result.

## Action-feedback coverage — pending approval — 2026-08-30

User direction: every tappable control must give immediate, intelligible feedback. A control
may open its destination, start a visible operation, confirm a completed change, or state why
it cannot proceed and how to recover. Empty data alone must not silently start Bluetooth work:
the user-facing Sync/Retry action starts it, while every dependent screen reads the same shared
operation state.

- [ ] Inventory every `Pressable`, button, row and link in `lib/ui2`; classify its intended
  response and identify controls that are inert, have invisible work, or hide an error.
- [ ] Add a compact shared presentation contract for idle, connecting, syncing, deriving,
  success and retryable-error states only if the existing AppState state cannot already express
  them. Do not duplicate BLE ownership or fabricate completion.
- [ ] Wire the Sync/Retry action and data-dependent empty states to that contract, then add
  focused interaction tests proving immediate feedback and honest recovery for each changed
  route.
- [ ] Verify with the physical iPhone once reconnected; inspect a successful sync, no-band,
  Bluetooth-off and partial/no-data outcome before documenting completion.
