---
name: bme-recipes
description: "Which Bricks elements to use when rebuilding a placeholder card left by Bricks Migration Engine, by what the Elementor add-on widget did: typing headlines, counters, progress, flip and info boxes, pricing, testimonials, team, timelines, tabs, sliders, video, dividers, breadcrumbs, off-canvas, read-more, tables, hotspots, listings. Use with bme-finishing."
---

# Rebuild recipes

Pick by what the widget did, not by its name. Read the element's schema (`bricks/get-element-schema`) before writing
settings; the element names below are Bricks' registered names (`bricks/list-element-types`). Where Bricks has no single
element, the recipe is a small tree. Carry the source's text, links, colours (as `var(--el-…)` where it used globals),
sizes and spacing; do not add anything the source did not have.

| The widget did | Bricks element(s) | Notes |
|---|---|---|
| A headline where part of the text types, rotates or highlights (typing, animated, fancy text) | `animated-typing` | Prefix, the rotating strings, suffix. The source has the strings as a repeater or a list; the typing speed and loop are options. |
| A heading in two colours or styles (dual-colour heading) | `heading` | Wrap the second part in a `<span>` and style it with a global class or `_cssCustom`. |
| A number that counts up (counter, stats) | `counter` | Start, end, duration, prefix, suffix, separator. Bricks counts whole numbers; carry a decimal in the suffix. |
| A horizontal skill or progress bar | `progress-bar` | Label, percentage, bar and track colours. |
| A circular or radial progress | `pie-chart` | Value as a percentage; the label in the centre. |
| An icon or image with a title and text (info box, icon box, feature box, call to action) | `icon-box` when it is icon + title + text; otherwise `div` holding `icon` or `image`, `heading`, `text-basic`, `button` | Carry the link: on the whole box as the Div's link, or on the button. |
| A card that flips, slides or reveals on hover (flip box, hover card) | `div` holding two `div` faces | Bricks has no flip element. The front and back as two Divs, the flip as `_cssCustom` on the parent (`transform: rotateY`), or an interaction that toggles a class. Say so in the summary. |
| A pricing table or plan | `pricing-tables` | Plans as items: title, price, period, features, button. |
| Testimonials, reviews, quotes | `testimonials` | Content, name, title, image per item; a slider if the source scrolled. |
| Team members | `team-members` | Image, name, title, description, social links per member. |
| A vertical or horizontal timeline | `div` per entry holding `heading`, `text-basic`, an `icon` for the marker | Bricks has no timeline element. A column Div, one Div per entry, the line as a border on the entry. |
| Tabs | `tabs-nested` | One title and one pane per source tab. |
| An accordion or toggle list, an FAQ | `accordion-nested` | One item per source entry. |
| A read-more / unfold / show-more block | `accordion-nested` with one item, or `div` + `button` with an interaction toggling a class | The source has "visible height" and two button texts. |
| A content slider or carousel | `slider-nested` for slides with free content; `carousel` for a list of images or posts | Arrows, dots, autoplay, slides per view from the source. |
| An image gallery, a gallery slider | `image-gallery` or `carousel` | Columns, gap, lightbox. |
| A self-hosted or HTML5 video, a video with a poster | `video` | Source file, poster, autoplay, loop, muted, controls. |
| A video or image lightbox button | `button` with the Bricks lightbox link type | |
| A divider with text or an icon in the middle | `divider` for a plain line; a `div` row holding `divider`, `text-basic`, `divider` for a labelled one | Bricks' divider has no label of its own. |
| Breadcrumbs | `breadcrumbs` | Home text and separator from the source. Bricks reads its own trail. |
| An off-canvas panel or slide-in menu | `offcanvas` + `toggle` | The toggle opens the panel; the panel holds the content. |
| A sticky or floating button, back to top | `back-to-top` for that; otherwise `button` with `_position: fixed` | |
| A countdown | `countdown` | Target date, fields, labels. |
| A social share or follow bar | `social-icons` or `post-sharing` | Follow links: social-icons. Share this post: post-sharing. |
| Star ratings | `rating` | |
| An alert, notice or message box | `alert` | |
| A map | `map` | Address or coordinates, zoom, marker. |
| A contact or lead form | `form` | Fields, labels, required, the success message. Set the submit actions with the owner: the source's actions do not carry. |
| A post grid, post list, blog, portfolio, listing | `posts`, or a `div`/`block` with a query loop holding the card's elements | Post type, number, order, taxonomy filters from the source. A filter bar is the Bricks filter elements. |
| A data table | none | Bricks has no table element. The `code` element with the table markup (the owner must allow code execution in Bricks) or a `div` grid. Say so. |
| Image hotspots | `div` with the `image` and absolutely positioned `icon` children | The tooltip is `_cssCustom` or an interaction. Say so. |
| A Lottie or animated SVG | none | Bricks has no Lottie element; the engine already converts Elementor's own Lottie widget to a Code element. For an add-on's, the `code` element with the player script, with the owner's consent to code execution. |
| A login, register or membership form, a booking widget, a live feed (social, reviews, prices) | leave the card | These need their plugin. Tell the owner. |

## Anything not listed

Start from what it rendered, not what it was called. Look at the source settings: the content keys tell you the parts
(a title, a text, an image, a link, a list), the style keys tell you the look. Build the parts with the plainest Bricks
elements that hold them (`div`, `heading`, `text-basic`, `image`, `icon`, `button`), carry the style, and say in the
`mark-finished` summary which behaviour you could not carry.
