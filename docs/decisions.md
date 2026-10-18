# Decisions

## 2026-08-30 — UI UX Pro Max is advisory, not the visual source of truth

`ui-ux-pro-max` correctly reinforces 44-point iOS targets, safe areas, semantic
navigation, Dynamic Type, reduced motion, typed route payloads and predictable back
behaviour. Its generated system selected a web conversion funnel, lavender palette and
neumorphism. Those choices conflict with the user-supplied legacy reference, the
information-dense local health product, and native iOS interaction conventions. The
generated `design-system/openstrap-edge/MASTER.md` is retained as unadopted tool output;
it must not drive implementation. The accepted system remains warm-dark, data-first,
system-type, semantic-token based and minimally animated.

## 2026-08-30 — The legacy screenshots are a visual/IA target, not executable truth

The user explicitly supplied the repository's legacy captures as the desired outcome.
Their visual hierarchy and detailed information architecture should return, but their
deleted UI code and any obsolete data fallbacks must not. Current UI2 routes and tested
honest data states are the behavioural source of truth.

## 2026-08-30 — Build before deleting the stale iPhone installation

The user authorised deletion of Edge and its local data if required. Build the replacement
first and attempt an in-place install under the profile's correct TeamIdentifier. This uses
the newly included certificate while avoiding needless data destruction if iOS accepts the
signed replacement. If installation rejects the existing app's identity, delete precisely
`com.nivish.openstrapEdge` from Nivi's iPhone under the granted confirmation, then install.

## 2026-08-30 — Use Taste v2 with Impeccable for the legacy-interface restoration

Taste v2 (`design-taste-frontend`) will guide the visual direction and guard against generic
interface patterns. Impeccable remains responsible for the replacement-world workflow and
native iOS interaction standards. The existing Flutter data invariants, accessibility tests,
and honest absent-metric handling override either skill where they conflict. Web-specific
Taste recommendations, such as GSAP motion, do not apply to this native Flutter app.

## 2026-08-30 — Legacy tab labels retain current UI2 capability owners

The restored shell uses Today → Home, Sleep → the existing Sleep detail, Heart → Health,
Body → Wellness, and Workouts → Workout. This restores the legacy mental model without
duplicating repositories, computation or state. Nutrition remains an explicit Profile entry
and preserves its existing `/water` deep link; Body owns the existing stress, breathing,
medication and journal deep links. This is the smallest map that makes every present
capability findable while keeping one source of truth per concern.
