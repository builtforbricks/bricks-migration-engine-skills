---
name: bme-start-here
description: "Load first in any session that finishes, reviews, reports on or asks about an Elementor-to-Bricks migration made with Bricks Migration Engine by Built for Bricks (the bricks-migration-engine/* abilities, the placeholder cards and review notes on converted pages, going live, rolling back). The engine converts; you finish what it left behind."
---

# Bricks Migration Engine: start here

Bricks Migration Engine converts an Elementor site to Bricks on the owner's own WordPress site: pages, posts, headers,
footers, templates and popups become native Bricks data, the Elementor site settings become Bricks variables, classes and
a theme style, and nothing of Elementor or of the plugin has to run afterwards. The conversion is deterministic: the same
site converts the same way every time. Load Bricks' own `bricks-start-here` first when it exists. Every other `bme-*`
skill assumes you have read this one.

## The rule

**The engine converts. You finish.** You never convert, re-convert or roll back anything, and you never rebuild a page
or a template from HTML, a screenshot, or your own reading of the Elementor page. If a document is not converted yet,
stop and tell the owner to convert it on the dashboard (**Bricks Migration** in wp-admin). Your work starts where the
engine's output stops: the placeholder cards and the review notes it leaves on a converted document.

## What the engine leaves behind

- **Placeholder cards.** Where a third-party add-on widget has no Bricks equivalent, the engine leaves a labelled card
  in its place: a purple-tinted box naming the widget in brackets and saying why it is there. The card keeps the
  position, so the page does not reflow around it. The widget's own Elementor settings are kept untouched behind it.
  These are yours to rebuild, from the source context, with Bricks' own elements. See `bme-finishing`.
- **Review notes.** Where the engine approximated a behaviour, changed a shape, or left a decision open, it writes a note
  on the element (a carousel spacing Bricks cannot vary per breakpoint, a query it could not express, a form whose submit
  actions must be set). Since 1.7.0 each note names its element. You fix what a Bricks setting expresses and ask the
  owner about the rest, and record either. See `bme-review-notes`.
- **The design system.** The Elementor global colours, fonts and spacing became Bricks global variables named `el-*`,
  shared element styling became `el2b-*` global classes (when the owner switched that on), the site styles became the
  theme style "El2B: Migrated site defaults", and the global colours also form the colour palette "Migrated from
  Elementor". Use these in anything you build. `get-design-handoff` lists them.

## The tools

- **Bricks' abilities** (`bricks/*`, Bricks 2.4 or later) read and edit converted documents like any Bricks content. That
  is how you build: `bricks/get-element-schema` before you write a setting, `bricks/add-element` to place an element,
  `bricks/remove-element` to take the placeholder card away once its replacement exists.
- **The plugin's abilities** `bricks-migration-engine/*`, only while the owner has switched them on **and** the site has an
  active licence. Reading: `get-status` (call it first), `list-leftovers`, `get-source-context`, `get-design-handoff`.
  Recording: `mark-finished` (a card you rebuilt) and `record-note` (a note you fixed, or a question for the owner).
  Administrators only. There is no convert, re-convert, rollback, mark-checked or settings ability, by design.
- **Through WordPress's MCP Adapter**, abilities that are not listed as direct tools run through
  `mcp-adapter-execute-ability` with `ability_name` (for example `bricks-migration-engine/list-leftovers` or
  `bricks/add-element`) and `parameters`; `mcp-adapter-discover-abilities` lists what the site registers and
  `mcp-adapter-get-ability-info` gives an ability's input schema. Read the schema before the first call of each.
- **When the abilities are missing**, the switch is off or the site is on the free trial. Tell the owner where the switch
  is (**Bricks Migration > Settings > AI assistant**, with a licence active on **License & Account**) and stop. Do not work
  around it with other tools (Bricks' PHP execution, options, the database).

## How a migration goes

1. The owner scans the site and reads the coverage report (what converts, what needs review, what has no equivalent).
2. The owner converts pages first, then the templates they reference, on the dashboard. Each conversion is reversible.
3. **You finish the leftovers**: rebuild each placeholder card in place and record it (`bme-finishing`, `bme-recipes`),
   then work through the review notes, fixing or asking, and record each (`bme-review-notes`).
4. The owner reviews each converted page with the preview switcher, reads what you recorded under each note, and marks
   pages checked.
5. The owner goes live and deactivates Elementor (`bme-going-live`). Rollback is the undo at any point (`bme-rolling-back`).
6. If the owner wants a client report, write it from the plugin's data (`bme-handoff-report`).

## Workflow in a session

1. `get-status`. Say the plugin version, whether the licence is active, how many documents are converted, how many
   cards and notes are open, and whether Bricks' abilities are on.
2. `list-leftovers`. Work from that list: the cards first (`leftovers`), then the notes (`review_notes`), one document at
   a time, with `get-source-context` for each item.
3. **Show the owner what you are about to build before you build it** when the intent is ambiguous (a widget with many
   settings, a dynamic source, a payment or form). Plain cases, build.
4. After each rebuild: remove the card, `mark-finished` with a short summary and the ids you added. After each note:
   `record-note`, fixed or needs_you. Move on.
5. End with a short list per page: what you rebuilt, what you fixed, the questions for the owner, and what to look at.
   If you stop early, say where; the owner can run the same prompt again and the list carries on from there.

## Never

- **Convert, re-convert or roll back**, by any route. Those are the owner's buttons on the dashboard.
- **Rebuild a page from HTML or a screenshot**, or import Elementor content any other way. The engine has converted it.
- **Edit the Elementor original** (`_elementor_data`, Elementor templates, Elementor settings). The owner may still roll
  back to it.
- **Delete anything but the placeholder card** you are replacing, and only after its replacement exists.
- **Touch a document the owner marked checked** without asking: a check vouches for the output they looked at.
- **Mark a page checked** on the owner's behalf. There is no ability for it, and the dashboard is where they do it.
- **Claim the conversion is complete, pixel-perfect or safe to go live.** Describe what you did and what to look at.
- **Invent a value**: a colour, a size, a link, a text. If the source context does not say, ask or leave the card.

## Honest limits

The engine's coverage number counts widget types, not styling: a page can convert cleanly and still want a look.
Review notes list what the engine knows it changed, not every pixel. The abilities read what is stored on the site;
they cannot see the front end rendered, so for anything visual say what the owner should check, or render with
Bricks' own abilities where they allow it. Check the pack against the plugin with `bme-update`.
