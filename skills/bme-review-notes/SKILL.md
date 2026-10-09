---
name: bme-review-notes
description: "Use when asked what a Bricks Migration Engine review note means, or to help work through the review queue on a converted page: the note kinds (approximations, behaviour gaps, queries, forms, dynamic data, code and shortcodes, templates and popups, motion, WooCommerce), what each asks of a human, and the few that are a rebuild."
---

# The review notes

Where the engine approximates, changes a shape or leaves a decision, it writes a note on the element. The dashboard's
review queue lists them per page; `bricks-migration-engine/list-leftovers` with `scope: "all"` returns them as
`review_notes` (`widget_type`, `kind`, `note`). Most notes need a human eye on the front end, not a rebuild. Read
`bme-start-here` first. You never mark a page checked: the owner does, on the dashboard.

## How to read a note

Each note names the widget, what happened, and what to do. The `kind` tells you the family. The `note` is written for
the owner: quote it, then say what you would check or do.

## Families, and what to do

- **Approximations** (`behavior-approx`, `layout-approx`, `text-approximation`, `star-style`, `pointer-approx`,
  `hover-animation`, `image-hover-animation`, `shape-approx`, `transform-precision`, `counter-decimal`, `icon-library`,
  `svg-as-image`): the behaviour exists on both sides, not identically. Look at it on the front end. Usually accept.
  Only rebuild if the owner says the difference matters.
- **Behaviour gaps** (`behavior-gap`, `unsupported-surface`, `carousel-static`, `carousel-pagination-built`,
  `search-live-results`, `search-results-template`, `search-input-extras`, `responsive-state`, `sticky-*`): a feature of
  the source has no Bricks control, so the converted element renders without it. A decision: keep without it, or add a
  Bricks element or interaction that does the job. Say what the owner loses either way.
- **Queries and loops** (`query-review`, `query-context`, `query-orderby`, `query-terms`, `query-filters`,
  `query-filters-carried`, `archive-context`, `dynamic-style`, `loop-asset-css`): a posts or products loop converted
  with the parts Bricks can express. Verify the set of posts and the order on the front end, in the template context the
  note names. Adjust the Bricks query if the note says a filter was left out.
- **Forms** (`form-actions`, `form-actions-partial`, `form-actions-token`, `form-messages`, `form-field-type`,
  `form-acceptance`, `form-html-field`, `atomic-form-message`, `behavior-wiring`): the fields converted; the submit
  actions did not, by design (Elementor stores them in its own format). Set the actions with the owner (email, redirect,
  a service) before the old form stops running. Never guess a recipient address.
- **Dynamic data** (`dynamic-unmapped`, `dynamic-acf`, `dynamic-internal-url`, `dynamic-content-empty`,
  `dynamic-composite-key`, `atomic-link-dynamic`, `atomic-class-unresolved`): a value came from a dynamic tag the
  engine could not map, so it converted empty or literal. Set the matching Bricks dynamic tag, or ask which field.
- **Code, scripts, shortcodes, markup** (`code-element`, `script-jquery`, `shortcode-carried`, `shortcode-elementor`,
  `shortcode-ids`, `markup-stripped`, `sanitiser-missing`, `attribute-blocked`, `link-scheme-blocked`,
  `highlighter-engine`, `block-content`): a human decision. A Code element runs only when the owner allows code execution
  in Bricks; a shortcode printed by Elementor Pro stops rendering the day Elementor is deactivated. Explain; do not
  switch code execution on, and do not rewrite scripts unasked.
- **Templates and popups** (`template-linked`, `template-unavailable`, `template-cycle`, `template-depth`, `popup-*`,
  `offcanvas-no-target`, `anchor`): structure the owner should know about. A linked template means editing it updates
  every page that uses it. A popup without a target cannot be wired; ask which popup.
- **Motion** (`motion-fx`, `motion-fx-off`, `animations-off`, `animation-visibility`, `animation-responsive`): mouse
  effects have no Bricks equivalent; scroll effects and entrance animations carry when the settings switch is on. Tell the
  owner which switch and that the page re-converts to carry them. Do not re-convert.
- **WooCommerce** (`woo-*`): verify on the shop pages, logged out, with products in the cart where the note is about the
  cart or checkout. Payment and account details are the owner's to re-enter (`payment-config`).
- **Site and chrome** (`needs-config`, `logo-width`, `plugin-dependency`, `sidebar-stale`, `sidebar-chrome`,
  `menu-review`, `context-dependent`, `facebook-sdk`, `share-network`, `size-preset`, `skipped-widget-css`): one thing to
  set or check outside the page: a logo, a menu assignment, a widget area, an SEO plugin for breadcrumbs.
- **Placeholders** (`placeholder`): the cards. These are the rebuilds. See `bme-finishing`.

## What you may do

- Explain a note, in plain words, and say what to look at and where.
- Rebuild a placeholder card (`bme-finishing`).
- Add or adjust a Bricks element the note describes as missing, when the owner asks, with Bricks' abilities.
- Set a Bricks dynamic tag or a query filter the note names, when the owner confirms the field or the set.

## What you may not do

- Mark a page checked, convert, re-convert or roll back: the owner's buttons.
- Switch on code execution, change a form's recipient, re-enter payment details, or edit the Elementor original.
- Dismiss a note as unimportant. The queue does not rank by severity on purpose: the engine measures what changed, not
  what matters on this site. Say what it is; the owner decides.
