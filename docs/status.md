# Status

Updated: 2026-08-30

Current state: Edge `0.9.28+60` is signed, installed, trusted, and launches on Nivi's
iPhone. The old iOS `“Edge” Is No Longer Available` message was an expired/untrusted
personal development installation, not a Flutter runtime failure.

UI decision: the current app is the `lib/ui2` five-domain shell (Home, Health, Nutrition,
Workout, Wellness). The README's Today/Sleep/Heart/Body/Workouts screenshots are stale:
they predate the 2026-08-15 UI rewrite that deleted `lib/ui` and introduced `lib/ui2`.

Known issues: `flutter analyze` reports two pre-existing warnings at
`lib/health/health_export.dart:783` and
`lib/health/health_measurement_import.dart:138`; neither was changed for this repair.
The build also created untracked SwiftPM workspace metadata, left untouched because it is
outside the approved repair scope.

Next steps: decide whether to refresh README screenshots to match the shipped UI or plan a
substantial redesign that deliberately restores the older visual/navigation model.
