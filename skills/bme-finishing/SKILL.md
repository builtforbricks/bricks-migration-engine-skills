---
name: bme-finishing
description: "Use when asked to finish, complete, clean up or rebuild what Bricks Migration Engine left on converted pages: the placeholder cards (third-party add-on widgets with no Bricks equivalent). The loop: list the leftovers, read each one's source context, rebuild it in place with Bricks' own elements and the migrated design tokens, remove the card, record it with mark-finished."
---

# Finishing the leftovers

A placeholder card marks one Elementor widget the engine does not convert. You turn each card into the thing it stood
for, with Bricks' own elements, in the same place, from the widget's own settings. Read `bme-start-here` first.

## The loop

1. **`bricks-migration-engine/get-status`.** Confirm the licence is active and Bricks' abilities are on. If `leftovers_open`
   is 0, say so and stop.
2. **`bricks-migration-engine/list-leftovers`.** One entry per converted document with its cards: `element_id` (the card's
   Bricks id, 6 characters), `widget_type` (the Elementor widget slug), `widget` and `add_on` (the human names when the
   add-on is still installed), `note` (what the engine said). Pass `document_id` to work on one document. Finished cards
   are left out unless you pass `include_finished: true`, which also lists finished cards already removed from the page
   (`removed: true`). The same list carries the review notes (`review_notes`); `scope: "placeholders"` leaves them out.
   They come after the cards, with `bme-review-notes`.
3. **For each card, `bricks-migration-engine/get-source-context`** with `document_id` and `element_id`. You get:
   - `source.settings`: the widget's Elementor settings as authored, empty values removed. This is the intent.
   - `element.parent_id` and `element.index_among_siblings`, with the siblings: where the rebuild goes.
   - `design`: the Elementor global colours and fonts the settings refer to, as the `var(--el-…)` the conversion created.
   - `engine_note`: why the engine left the card. `source.found` false means the Elementor original is gone: rebuild from
     the note and the card's text alone, or ask.
4. **Read the intent.** Elementor settings follow a few conventions:
   - Text and links are plain keys (`title`, `text`, `content`, `link.url`, `video_url`). HTML in text is the author's.
   - Switches are `"yes"` or absent. Sizes are `{ "unit": "px", "size": 24 }` or `{ "size": "", "unit": "px" }` (unset).
     Spacing is `{ top, right, bottom, left, unit, isLinked }`.
   - A key with `_tablet` or `_mobile` is the same setting at that breakpoint. A key starting with `_` is the Advanced
     tab (margin, padding, background, border, custom CSS, animation, z-index).
   - `__globals__` maps a setting to a global colour or font: use the `design` entry's `var(--el-…)` instead of a literal.
   - `__dynamic__` means the value comes from a dynamic tag (a field, the post title, an image). Rebuild with Bricks'
     dynamic data (`{post_title}`, `{acf_field}`, …) and say which tag you chose; if the tag has no Bricks equivalent, ask.
5. **Choose the Bricks elements** (`bme-recipes`). One widget often becomes a small tree: a Div holding an Image, a
   Heading and a Text. Prefer one native element when Bricks has it (Counter, Video, Breadcrumbs, Pricing Tables).
6. **Read the schema before you write**: `bricks/get-element-schema` for each element type you have not used in this
   session. Set only controls the schema offers; defaults are not written.
7. **Build in place**: `bricks/add-element` under `element.parent_id` at `element.index_among_siblings`, so the rebuild sits
   where the card is (the card moves down one; you remove it next). The index follows the parent's `children` order,
   the order Bricks renders; confirm it with `bricks/get-page-structure` after each write, because positions move as
   you add and remove. Nest children under the element you added. Bricks' element ids are 6 characters; let Bricks
   mint them. Bricks' first save may rewrite how the remaining cards store their styling; they are still cards, and
   `list-leftovers` still lists them.
8. **Style with the design system**: `var(--el-…)` for colours and fonts the source bound to globals; the `el2b-*` global
   class when one matches (`get-design-handoff` lists them); the source's own literals otherwise. Carry margin and
   padding from the `_margin` and `_padding` keys. Do not improve the design; reproduce it.
9. **Check what you built**: `bricks/get-page-structure` (or `bricks/get-page-elements`) shows the tree; where Bricks
   allows, `bricks/render-elements` shows the markup. Fix before you remove anything.
10. **Remove the card**: `bricks/remove-element` on the card's `element_id`, and only now. The card is one `div` with one
    text child; remove the div.
11. **Record it**: `bricks-migration-engine/mark-finished` with `document_id`, `element_id`, a one or two sentence
    `summary` for the owner (what you built, what to look at), and `replaced_with` (the ids you added). The dashboard
    shows it as "finished by your assistant", apart from the engine's own conversion. If the reply says the card is still
    present, you skipped step 10.
12. **Next card.** When the document is done, say what you rebuilt and what the owner should check on the front end.

## When not to rebuild

Leave the card, record nothing, and tell the owner why, when:

- the widget needs its plugin to work at all (a form service, a booking engine, a live feed, a membership gate);
- the intent depends on data you cannot see (a dynamic tag with no Bricks mapping, a listing from a query builder);
- the source settings are empty or the original is gone and the card's text is all there is;
- the owner marked the document checked, until they say to go ahead.

Offer the options in one line each: rebuild with a Bricks element and lose X; keep the add-on active beside Bricks;
drop the piece.

## Rules

- One card at a time. Never batch removals before the rebuilds exist.
- Never touch an element you did not add, except the card you are replacing.
- Never edit the Elementor original. Never convert, re-convert or roll back.
- A document may be a Bricks template (header, footer, archive, popup). Same loop; do not change its conditions.
- If the plugin's abilities disappear mid-session, the licence or the switch changed. Stop and say so.
- Do not call the result complete or faithful. Say what you rebuilt and what to look at.
