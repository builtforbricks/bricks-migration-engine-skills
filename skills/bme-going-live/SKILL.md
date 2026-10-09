---
name: bme-going-live
description: "Use when the owner asks whether the migration is ready, what to check before deactivating Elementor, in which order to remove Elementor and Elementor Pro, or what happens to Bricks Migration Engine afterwards. A checklist you read against the site's data; the owner performs the steps."
---

# Going live and removing Elementor

The end of a migration is the moment Elementor is deactivated and nothing breaks. The dashboard says when the engine
believes the site is there; you help the owner check. You do not deactivate plugins or change settings.

## The signal

Once every page is converted and nothing is waiting in the review queue, the dashboard's progress card says it is safe
to remove Elementor and Elementor Pro. Both conditions, not one: a site with every page converted and notes still
open does not show it. It is as meaningful as the owner's reviewing was.

## Check before deactivating

Read `bricks-migration-engine/get-status` and `list-leftovers` (with `include_finished: true` and `scope: "all"`), then
go through these with the owner:

- **Leftovers.** Every placeholder card rebuilt or deliberately dropped. An open card is a hole on the live site. The
  add-on plugins behind the cards are separate plugins: deactivating Elementor does not remove them, and a card that was
  rebuilt no longer needs its add-on.
- **Templates.** Each converted header, footer and theme-builder template is the one Bricks actually uses: load a real
  page. Assignment and display conditions live outside the template, so a template can convert well and still not be the
  one picked. `bricks/list-templates` and `bricks/get-template-settings` show the conditions.
- **Pages the owner rebuilt by hand.** Their Elementor original no longer matches; that is fine, the Bricks version is the
  one that counts. Say it so nobody is surprised.
- **Forms.** Converted forms carry their layout, not their submit actions or integrations. Each one points at whatever
  handles entries before the old ones stop running.
- **Code execution.** Some widgets convert to a Bricks Code element (an Elementor Lottie, a classic widget). Bricks runs
  those only when the owner has allowed code execution under Bricks' settings, and asks for that consent per site. The
  engine never enables it. If it is off, those elements render nothing.
- **Dynamic data and queries.** The review notes that named a query or a dynamic tag were acted on, and the result was
  seen on the front end, in the right template context.
- **A phone width.** One more pass at mobile on the pages that matter most.

## Removing Elementor

The owner deactivates **Elementor Pro** first, then **Elementor**, and loads the key pages. If something looks wrong,
reactivating is one click: the Elementor data is still there, and any page not rolled back still has both versions.
Recommend keeping the Elementor plugins installed but deactivated for a while rather than deleting them; deleting is the
one step that is not quickly reversible.

## What happens to the plugin

Converted pages do not need Bricks Migration Engine. It writes standard Bricks data, so once the owner is happy they can
deactivate the engine too and everything keeps rendering, Code elements included (Bricks validates their signatures
against the site, not the plugin). Worth keeping it installed while Elementor data is still around: it is what gives the
owner the review queue, the preview switcher, rollback, and your `bricks-migration-engine/*` abilities.

## If it is not done

There is no need to finish in one sitting. A site can run half-converted indefinitely: each page renders from whichever
builder owns it, and the dashboard keeps saying what is left.

## Never

- Deactivate or delete a plugin, switch on code execution, or change a template's conditions yourself.
- Say the site is ready. Say what is done, what is open, and what the owner should load before deciding.
