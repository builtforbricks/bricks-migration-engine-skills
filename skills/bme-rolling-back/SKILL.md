---
name: bme-rolling-back
description: "Use when the owner asks about undoing a Bricks Migration Engine conversion: what rollback does, what happens to pages edited in Bricks, rollback all, the free trial, and when to roll back versus re-convert. Rollback is the owner's button; you explain and recommend."
---

# Rolling back

Every conversion can be undone. Rollback removes the Bricks version and the page renders from Elementor again, exactly as
before: the Elementor data was never modified. There is no rollback ability for you, by design. You explain, and the owner
clicks **Rollback** on the page's row, or **Rollback all**.

## What rollback does

- A page: its Bricks content is removed; the Elementor version shows again.
- A template (header, footer, archive, popup): the Bricks template the conversion created goes to the Trash, so the
  undo is itself undoable. On the free trial it goes to the Trash **empty**, its content set aside on the trashed post,
  so restoring it does not bring a working template back without converting again.
- The engine's review notes for that document go with it, and so do your `mark-finished` records: a rollback or a
  re-convert voids what was recorded about cards that no longer exist.

## Pages edited in Bricks

If the owner converted a page and then **edited it in Bricks**, rollback keeps that page rather than removing it. The
dashboard marks it *Edited in Bricks* and says the Bricks version was kept because it has been worked on since
converting. Your rebuilds count as edits: a page you finished will not roll back to Elementor unless the owner removes the
Bricks version themselves in Bricks. Say this before the owner clicks.

## Rollback all

Reverts every conversion on the site in one pass, after asking. It also restores the site-wide settings the engine wrote
during conversion (the variables, classes, theme style and breakpoint), which a single-page rollback deliberately leaves
alone, since other converted pages may still use them. Pages edited in Bricks are kept here too, and listed.

## Rollback or re-convert

- The owner changed the Elementor original, or updated the plugin, and wants a fresher conversion: **re-convert**, not
  rollback first. Re-converting rebuilds the Bricks version from the Elementor page. If the page was edited in Bricks,
  the engine warns before replacing those edits.
- The conversion is wrong for this page and the owner wants Elementor back: **rollback**.
- Rollback keeps Bricks edits; re-converting replaces them. That is the difference between the two buttons.

## The free trial

An unlicensed site may have three converted pages at a time (the header, the footer and the template framing a converted
post do not count). Rolling a page back frees its place for another page. Moving a converted page to the Trash does
not: it still counts until it is deleted for good. None of this is yours to act on; say it when asked.

## Never

- Remove Bricks content, trash a template, or delete anything to "roll back" by hand. The owner's button does it, and
  it knows the rules above.
- Promise that a rollback restores a page you have rebuilt: it will be kept, as an edited page.
