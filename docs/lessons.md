# Lessons

## 2026-08-30 — UI conclusion without an in-app screen

Incident: While diagnosing the iOS `“Edge” Is No Longer Available` alert, the assistant
used the installed version match (`com.nivish.openstrapEdge`, `0.9.28+60`) to state that
the UI was current. The supplied image only showed the iOS launcher alert and contained
no Edge UI. Root cause: source-version evidence was incorrectly treated as rendered-UI
evidence. Rule: verify a running in-app screen against the intended reference before
claiming a UI match.

## 2026-08-30 — Signing team mismatch blocks safe replacement

Incident: `ios/Config/Signing.xcconfig` was absent. The local provisioning profile for
`com.nivish.openstrapEdge` belongs to Team `V2B9C748M5`, while `security find-identity`
reported only a valid Apple Development certificate for Team `2ALA6YRM9K`. Attempting a
replacement under the other team risks requiring deletion of the existing app container.
Rule: compare the provisioning-profile team with the signing-certificate team before an
iOS reinstall; stop and obtain recovery or deletion approval on a mismatch.

## 2026-08-30 — App-ID prefixes are not necessarily certificate team IDs

Correction: the provisioning profile displayed by Xcode for `com.nivish.openstrapEdge`
includes the valid certificate `Apple Development: nivishv2004@icloud.com (2ALA6YRM9K)`
while its App-ID prefix is `V2B9C748M5`. The profile explicitly authorises the same app,
App Group `group.com.nivish.openstrap`, and HealthKit. Root cause: treating the profile's
application-identifier prefix as the signing-certificate team ID. Rule: inspect the
profile's included certificate and Xcode validation before declaring a team mismatch.

## 2026-08-30 — Dependency resolution changed unrelated tracked files

Incident: running `flutter pub get` with the installed Flutter version rewrote six
transitive entries in `pubspec.lock` and added generated platform excludes to
`analysis_options.yaml`. These changes were unrelated to the iOS re-sign operation.
Root cause: an unconstrained dependency resolution was used as a build preflight.
Rule: inspect and revert unrelated resolver-generated diffs before continuing a
targeted repair.

## 2026-08-30 — Missing optional build-definition file

Incident: the release build was invoked with `--dart-define-from-file=.env`, but `.env`
does not exist in this checkout. `.env.example` and `lib/cloud/backend_client.dart` show
that both values are optional and default to empty strings. Rule: verify optional build
configuration exists before passing its file to Flutter; use the documented no-define path
when the application supports it.

## 2026-08-30 — Provisioning files do not substitute for an authenticated Xcode account

Incident: a signed release build found the local certificate and provisioning profiles but
failed for Runner, the widget, and the Watch app with `No Account for Team 2ALA6YRM9K`.
Xcode therefore could not locate or refresh matching development profiles. Rule: verify
the owning Apple ID is authenticated in Xcode Accounts before treating local profiles as a
complete signing setup.

## 2026-08-30 — Visible Xcode account did not reach command-line signing

Incident: Xcode Settings displayed the correct Personal Team, yet a build invoked through
the same `/Applications/Xcode.app` still reported `No Account for Team 2ALA6YRM9K`.
Rule: when GUI account visibility and `xcodebuild` disagree, refresh manual profiles and
restart Xcode before retrying; do not assume two Xcode installations without checking
`xcode-select`.

## 2026-08-30 — Certificate identifier is not `DEVELOPMENT_TEAM`

Incident: `ios/Config/Signing.xcconfig` used `2ALA6YRM9K`, the identifier displayed
beside the included Apple Development certificate, for `APPLE_DEVELOPMENT_TEAM`. The
profile's `TeamIdentifier` is `V2B9C748M5`, which is the value Xcode requires. The
result was three failed builds reporting `No Account for Team 2ALA6YRM9K`. Rule: use
the mobileprovision `TeamIdentifier` for `DEVELOPMENT_TEAM`; do not derive it from a
certificate label.

## 2026-08-30 — Fresh development profile needs iPhone trust before launch

Incident: the signed replacement installed, but `devicectl` reported that iOS denied
launch because the profile had not been explicitly trusted. Rule: after a fresh personal
development signing install, complete Settings → General → VPN & Device Management trust
on the device before diagnosing an application launch failure.

## 2026-08-30 — Flutter verification rewrote tracked generated files

Incident: `flutter analyze` added platform excludes to `analysis_options.yaml`, and the
iOS build rewrote `ios/Podfile.lock`. Neither change belongs to the signing repair; both
were reverted. Rule: after Flutter tool invocations, inspect the full worktree and reverse
unrelated generated diffs before concluding verification.

## 2026-08-30 — README screenshots were mistaken for current product evidence

Incident: after seeing the first remote README hero image, the assistant characterised the
visual mismatch as promotional artwork. The user then supplied the README Screens section,
which contains real legacy app captures. Git history shows those assets last changed on
2026-07-12, while commit `4203370` replaced `lib/ui` with `lib/ui2` on 2026-08-15 without
refreshing the README. Rule: treat repository screenshots as versioned product evidence;
compare their last asset update with the source UI history before calling them mockups.

## 2026-08-30 — Skill installer permission denied

Incident: invoking `/Users/nivish/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py`
directly failed with `zsh: permission denied` because the supplied installer is not executable
(`-rw-r--r--`). The requested Taste Skill has not been installed. Rule: inspect the installer
mode and available project virtual environment before executing a Python-backed skill installer.

## 2026-08-30 — Root GitHub URL is insufficient for this skill installer

Incident: the Taste installer invocation reached the supplied script through an isolated `uv` runtime,
but failed with `Missing --path for GitHub URL`. The source URL identified only the repository rather
than its skill directory. Rule: for third-party repositories, resolve the exact skill subdirectory from
the repository documentation before invoking the installer.

## 2026-08-30 — Upstream Taste path was assumed from its display name

Incident: installing `leonxlnx/taste-skill` with `--path design-taste-frontend` failed because that
directory is not at the repository root. No skill was installed. Rule: inspect the upstream repository
tree, not just its README terminology, before passing a subpath to a sparse skill installer.

## 2026-08-30 — Browser-wrapper response shape was unavailable

Incident: the web wrapper call to inspect the Taste repository produced no iterable content response,
so no upstream path evidence was extracted. Rule: when a browsing wrapper's response shape is unavailable,
switch to the repository's read-only API rather than guessing or retrying the same parser.

## 2026-08-30 — Widget test was placed outside its Dart test entry point

Incident: while adding the embedded Sleep-tab regression test,
`testWidgets(...)` was written before `void main()`. Dart test declarations must execute
inside the entry point. Root cause: patching against the helper boundary instead of the
suite boundary. Rule: after adding a Dart test, verify the declaration is nested under
the file's `main` function before running the suite.

## 2026-08-30 — Dart formatter rewrote adjacent legacy code

Incident: formatting the four touched Dart files after the embedded Sleep change rewrote
large pre-existing sections, producing a broad whitespace-only diff. `flutter test` also
re-added unrelated platform excludes to `analysis_options.yaml`, which was reverted. Rule:
inspect formatter scope immediately; preserve only formatting required by the changed code
and revert Flutter-generated configuration changes before the next logical step.

## 2026-08-30 — Impeccable craft-floor instruction was loaded late

Incident: the first shell wiring edits occurred after the Impeccable direction record but
before loading `reference/craft-floor.md`, which the skill requires immediately before UI
edits. No visual token decision was made in that interval, but the process order was wrong.
Rule: after direction is settled, load the craft-floor reference directly before the first
visual implementation patch, not before a later palette pass.

## 2026-08-30 — Shell interaction test used an unavailable semantics finder

Incident: `test/ui2_golden_test.dart` attempted to tap `Pressable` tabs through
`find.bySemanticsLabel`, but the wrapper does not expose that label in the test semantics
tree. The test failed while the app shell rendered correctly. Rule: inspect a custom input
wrapper's test-visible boundary before choosing a finder; use its widget predicate or a
stable key when semantics are not exposed.

## 2026-08-30 — Fresh certificate required another explicit iPhone trust

Incident: the release build installed successfully, but `devicectl` launch was denied by
iOS before Flutter started: “its profile has not been explicitly trusted by the user”. The
fresh signing certificate/profile is a new trust subject even though an earlier developer
profile was trusted. Rule: after replacing an iOS development certificate, verify the
on-device trust gate again before diagnosing any app behaviour.

## 2026-08-30 — Device lock denied launch after trust was accepted

Incident: after developer-profile trust was completed, `devicectl` launch was denied with
the explicit reason that the device was locked. This is a physical-device state gate, not a
signature or Flutter fault. Rule: when launching over device services, keep the phone unlocked
and awake before interpreting a process-launch denial.
