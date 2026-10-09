---
name: bme-handoff-report
description: "Use when the owner asks for a migration report, a client hand-off, a summary of what was converted and what was rebuilt, or documentation of an Elementor-to-Bricks migration made with Bricks Migration Engine. Written from the plugin's own data; the engine's conversion and the assistant's rebuilds stay apart."
---

# The hand-off report

A migration report tells a client, or the owner's future self, what happened to the site: what the engine converted,
what you rebuilt, what was decided, and what to look at. Write it from the plugin's data, not from memory.

## Sources

- `bricks-migration-engine/get-status`: plugin version, documents converted, leftovers open and finished.
- `bricks-migration-engine/list-leftovers` with `include_finished: true` and `scope: "all"`: every converted document
  (documents with nothing to do included), its cards (open and finished; a finished card already removed from the page
  comes with `removed: true`, its summary and the ids added), and the engine's review notes.
- `bricks-migration-engine/get-design-handoff`: the variables, classes, theme style and breakpoint the conversion created.
- `bricks/get-system-information`: WordPress, Bricks and plugin versions; which Elementor plugins are still active.
- `bricks/list-templates`: the converted templates and their conditions.

## Structure

1. **Summary.** The site, the date, the plugin versions, how many documents were converted, how many cards were
   rebuilt, how many remain, whether Elementor is still active.
2. **Converted by the engine.** The documents, by type (pages, posts, headers, footers, templates, popups). State that the
   conversion is the plugin's deterministic output: the same source converts the same way every time.
3. **Finished by the assistant.** One line per rebuilt card: document, what the widget was, what it is now, from its
   `mark-finished` summary. State that these are rebuilds, made from the widget's original settings with Bricks' own
   elements, and are not part of the engine's conversion. This section is never merged with the one above.
4. **Open items.** Cards left as they are and why (needs its plugin, data missing, the owner's decision pending); review
   notes that need a decision; forms whose actions still need setting.
5. **The design system.** The `el-*` variables (by category), the `el2b-*` classes, the theme style ("El2B: Migrated site
   defaults"), the colour palette ("Migrated from Elementor") and the breakpoint, with one sentence on how to use them
   going forward.
6. **What to look at before going live.** From `bme-going-live`: templates in use, forms, code execution, a phone width.
7. **How to undo.** One paragraph from `bme-rolling-back`: rollback per page, what happens to edited pages.

## Rules

- Numbers come from the abilities. Do not estimate. If two abilities disagree, say so and quote both; do not pick one.
- Quote the engine's notes and your own summaries; do not reword them into claims.
- Say "converted by the engine" and "rebuilt by the assistant", never one word for both. The coverage number, where you
  cite it, is the engine's.
- No guarantees: not "complete", not "pixel-perfect", not "safe". Say what was done and what to check.
- Plain words, short sections, the client's vocabulary (pages, header, footer, forms), Bricks' element names in
  backticks where they help.
- Markdown by default; another format only when asked.
