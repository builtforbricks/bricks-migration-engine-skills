---
name: bme-review-notes
description: "Use when asked to work through, fix, explain or answer the review notes Bricks Migration Engine left on converted pages (the dashboard's Needs your eyes list): where the engine approximated, dropped a feature or left a decision. The loop: list the notes, read each one's source context, fix it with Bricks' own abilities when a Bricks setting expresses it, ask the owner when it is their decision, and record either with record-note. Also what each note kind means and asks of a human."
---

# The review notes

Where the engine approximates, changes a shape or leaves a decision, it writes a note on the element. The dashboard lists
them per page under **Needs your eyes**. Since Bricks Migration Engine 1.7.0 every note names its element, and you work
through them: **fix** what a Bricks setting expresses, **ask** the owner about the rest, and **record** either one, so the
owner sees your work under the note. The owner then looks at the page and marks it checked; you never do. Read
`bme-start-here` first, and rebuild the placeholder cards before the notes (`bme-finishing`): a card is a hole in the page.

## The loop

1. **`bricks-migration-engine/get-status`**: `notes_open`, `notes_fixed`, `notes_need_you`. If `notes_open` is 0, say so.
2. **`bricks-migration-engine/list-leftovers`**, one document at a time (`document_id`; `scope: "notes"` for the notes
   alone). Each document's `review_notes` entry has `note_id` (n and 7 characters), `kind`, `widget_type`, `note` (the
   engine's sentence, written for the owner), `element_id` (the Bricks element it is about), `elementor_id`, `located`
   and `record`. Notes already recorded are left out unless you pass `include_finished: true`; notes on a page the owner
   marked checked are left out, because that page is signed off.
3. **`bricks-migration-engine/get-source-context`** with `document_id` and `note_id`. You get the element's current
   Bricks settings (`element.bricks_settings`), the Elementor widget's settings as authored (`source.settings`), every
   note on that element, and the design tokens. The comparison of the two sides is the whole job.
   - `element` null with `candidates`: the page was converted before 1.7.0, so the note does not name its element. Take
     the context of each candidate and match the note by its settings. If none matches with confidence, record
     `needs_you`: re-converting the page on the dashboard gives every note its exact element.
   - `element` null and no candidates: the note is about the whole document (a template condition, a dependency, a site
     setting). Explain it, or fix the one setting it names when the owner asked.
4. **Decide**, by the note's family (below): fix, or ask.
5. **Fix.** `bricks/get-element-schema` for the element first. Change only what the note describes, with
   `bricks/update-element` (or add the element or interaction it says is missing), using the `el-*` variables where a
   value is a colour or size. Prefer a native setting to the element's custom CSS: the engine's custom CSS on a converted
   element is measured output, and the plugin keeps it as written when you save other settings, but an edit to the CSS
   itself is saved as Bricks reformats it. Confirm with `bricks/get-page-structure`. Then `bricks-migration-engine/record-note` with
   `outcome: "fixed"`, a
   `summary` for the owner (what you changed, what to look at) and `changed` (the ids you touched).
6. **Ask.** `record-note` with `outcome: "needs_you"` and the question as the `summary`, with the options you see
   ("keep it static, or add a Bricks interaction that…"). Change nothing.
7. **After each document**, tell the owner what you fixed and what you asked. They look, then mark the page checked.
8. **When the owner answers a question**, record that note again (the new record replaces the old): `fixed` with what you
   changed if their answer needed a change, or `decided` with their answer in their words if they chose to keep it as it
   is. Neither signs the page off.

`fixed` means you changed the element. A note you only looked at is `needs_you` with what to look at, never `fixed`.
`decided` is the owner's answer, never your own judgement: without their word, it stays `needs_you`.

## Families: what each asks, and whether you fix or ask

- **Approximations** (`behavior-approx`, `layout-approx`, `text-approximation`, `star-style`, `pointer-approx`,
  `hover-animation`, `image-hover-animation`, `shape-approx`, `transform-precision`, `counter-decimal`, `icon-library`,
  `svg-as-image`, `carousel-space-responsive`): the behaviour exists on both sides, not identically. **Fix** when a Bricks
  control closes the gap without guessing (a value per breakpoint, a closer icon, a decimal format), and say what is
  still different. Otherwise **ask** the owner to look, naming the difference.
- **Behaviour gaps** (`behavior-gap`, `unsupported-surface`, `carousel-static`, `carousel-pagination-built`,
  `search-live-results`, `search-results-template`, `search-input-extras`, `responsive-state`, `sticky-*`): a feature of
  the source has no Bricks control, so the converted element renders without it. **Ask**: keep it without, or add a
  Bricks element or interaction that does the job; say what the owner loses either way. Build it only on their yes.
- **Queries and loops** (`query-review`, `query-context`, `query-orderby`, `query-terms`, `query-filters`,
  `query-filters-carried`, `archive-context`, `dynamic-style`, `loop-asset-css`): the loop converted with what Bricks can
  express. **Fix** a filter the note names as left out when a Bricks query control holds it; then **ask** the owner to
  check the set of posts and their order on the front end, in the context the note names.
- **Forms** (`form-actions`, `form-actions-partial`, `form-actions-token`, `form-messages`, `form-field-type`,
  `form-acceptance`, `form-html-field`, `atomic-form-message`, `behavior-wiring`): the fields converted, the submit
  actions did not, by design. **Always ask.** The dashboard has a **Carry submit actions** button that carries the
  Elementor form's own actions; point the owner to it. Never set a recipient, a redirect or a service yourself.
- **Dynamic data** (`dynamic-unmapped`, `dynamic-acf`, `dynamic-internal-url`, `dynamic-content-empty`,
  `dynamic-composite-key`, `dynamic-featured-image-unset`, `atomic-link-dynamic`, `atomic-class-unresolved`): a value
  came from a tag the engine could not map. **Fix** when the source names the field and the Bricks tag for it is
  unambiguous (`{acf_field_name}`, `{post_title}`); otherwise **ask** which field.
- **Code, scripts, shortcodes, markup** (`code-element`, `code-execution`, `script-jquery`, `shortcode-carried`,
  `shortcode-elementor`, `shortcode-ids`, `markup-stripped`, `sanitiser-missing`, `attribute-blocked`,
  `link-scheme-blocked`, `highlighter-engine`, `block-content`, `asset-dependency`, `asset-missing`, `embed-iframe`):
  **ask**. A Code element runs only when the owner allows code execution in Bricks; a shortcode printed by Elementor Pro
  stops rendering when Elementor is deactivated. Never switch code execution on; never rewrite a script.
- **Templates and popups** (`template-linked`, `template-unavailable`, `template-cycle`, `template-depth`,
  `template-condition`, `popup-*`, `offcanvas-no-target`, `anchor`): structure the owner should know about. **Ask**
  (which popup a trigger opens, where a template should show). Set a condition only on the owner's word.
- **Motion** (`motion-fx`, `motion-fx-off`, `animations-off`, `animation-visibility`, `animation-responsive`,
  `animation-key-mismatch`, `slideshow-ken-burns`): mouse effects have no Bricks equivalent; scroll effects and entrance
  animations carry when the plugin's setting is on and the page is re-converted. **Ask** the owner to look; say which
  setting and that re-converting is theirs.
- **WooCommerce** (`woo-*`): **ask** the owner to check the shop pages logged out, with a product in the cart where the
  note is about the cart or checkout. Payment and account details are theirs to re-enter (`payment-config`).
- **Site and chrome** (`needs-config`, `logo-width`, `plugin-dependency`, `sidebar-stale`, `sidebar-chrome`,
  `menu-review`, `context-dependent`, `facebook-sdk`, `share-network`, `size-preset`, `skipped-widget-css`,
  `overlay-css`, `price-table-button-hover-animation`): one thing to set or check outside the element: a logo, a menu
  assignment, a widget area. **Fix** it when it is a Bricks setting on the element and unambiguous; otherwise **ask**.
- **Placeholders** (`placeholder`): not notes, cards. See `bme-finishing`.

A kind not listed here: read the note, compare the two sides, and ask.

## Never

- **Mark a page checked, convert, re-convert or roll back.** Those are the owner's buttons on the dashboard.
- **Switch on code execution, set a form's recipient, re-enter payment details, or edit the Elementor original.**
- **Record `fixed` for a note you only looked at**, or for a change you did not confirm with the page structure.
- **Dismiss a note as unimportant.** The list does not rank by severity on purpose: the engine measures what changed,
  not what matters on this site. Say what it is; the owner decides.
- **Guess** a value, a field, an address or a target. Ask.
