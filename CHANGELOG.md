# Changelog

Each release of this pack names the Bricks Migration Engine version it was checked against (`verifiedUpTo` in
`skills/bme-update/references/index.json`). Skills and plugin are released together; a pack never describes a plugin
that has not shipped.

## 0.1.0, unreleased

Verified up to Bricks Migration Engine 1.7.0, the release that adds the AI assistant's finish-only abilities: six of
them, covering the placeholder cards and the review notes.

- First pack: `bme-start-here`, `bme-finishing`, `bme-recipes`, `bme-review-notes`, `bme-going-live`,
  `bme-rolling-back`, `bme-handoff-report` and `bme-update`.
- `bme-review-notes` is a working loop, not only a glossary: each note names its element, the assistant fixes what a
  Bricks setting expresses, asks the owner about the rest, and records either with `record-note`.
