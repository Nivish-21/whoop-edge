# Changelog

## 2026-08-30 — Restore local iPhone installation and diagnose UI discrepancy

- Recreated the ignored local iOS signing override from the verified provisioning profile,
  built a signed Release `0.9.28+60`, installed it in place, and completed iPhone profile
  trust. This restored normal Edge launch without requiring an app deletion.
- Verified that the live Home screen is supplied by the current `lib/ui2` implementation.
  The README screenshots are stale legacy `lib/ui` captures from before the 2026-08-15 UI
  rewrite; they do not represent the app currently shipped from `main`.
