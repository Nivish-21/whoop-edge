# Product

<!-- impeccable:product-schema 1 -->

## Platform

ios

## Users

People who own a reverse-engineered WHOOP 4.0 band and want to understand their
health, sleep, activity, nutrition, and wellbeing without giving their health data to a
cloud service.

## Product Purpose

OpenStrap Edge connects to the band, stores its data on the device, derives health
metrics locally, and presents practical daily guidance. Success is a trustworthy,
usable companion that helps its owner understand what was actually recorded and act on
it.

## Positioning

OpenStrap Edge is a local-first WHOOP companion: BLE offload, SQLite storage and
analytics all run on-device, with no backend owning the user's health data.

## Operating Context

The app is used throughout the day on an iPhone: after sleep, while syncing the band,
before and during workouts, when logging food or medication, and when reviewing trends.
It also receives notification deep links, exports through HealthKit, and shares state
with WidgetKit and Watch components.

## Capabilities and Constraints

- It pairs and synchronises a WHOOP 4.0 band over BLE, retains local data, derives
  metrics off the UI isolate, and may export to HealthKit.
- Primary domains currently include home, health, nutrition, workout, and wellness,
  with detailed flows for sleep, heart, stress, breathing, body, steps, workouts,
  records, recap, profile, devices, alarms, settings, cycle tracking, journals and AI
  briefings.
- Data integrity comes before visual continuity: absent or thin input must remain
  absent or explicitly low confidence. The app never invents metrics.
- Existing notification routes, deep links, accessibility semantics, Dynamic Type,
  Dark Mode, Reduce Motion, safe areas and 44-point touch targets must remain intact.
- The user approved restoring the visual and information language represented by the
  legacy README captures, but not restoring deleted compute, decode or storage paths.

## Brand Commitments

OpenStrap is the product name. Edge is the installed client name and repository name.
The product is open source, self-hosted in spirit, privacy-first, technically honest,
and calm rather than gamified. The supplied legacy captures are binding visual evidence,
not a mandate to repeat obsolete data behaviour.

## Evidence on Hand

- Current implementation: `lib/ui2/`.
- Legacy visual reference: `screenshots/` and the repository README Screens section.
- Current behaviour evidence: physical iPhone launch on 2026-08-30 and the current
  UI2 test suite under `test/ui2_*`.
- No independent brand package, approved new copy, or real health-data fixture was
  supplied. New screens must use live data or existing honest empty, loading and error
  states.

## Product Principles

- Local health data remains local and understandable.
- A missing measurement is more trustworthy than an invented one.
- Daily actions must remain obvious under real-world interruptions.
- Familiar iOS behaviour and accessibility are product features.
- One metric and one action should mean the same thing everywhere.

## Accessibility & Inclusion

The app supports Dynamic Type, system Dark Mode, Reduce Motion, semantic labels and
touch targets suitable for iPhone use. The redesign must preserve and extend those
capabilities.
