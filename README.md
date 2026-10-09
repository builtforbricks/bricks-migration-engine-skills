# Bricks Migration Engine skills

Agent skills for [Bricks Migration Engine](https://builtforbricks.com/bricks-migration-engine/), the Elementor-to-Bricks
migration plugin for Bricks Builder by Built for Bricks: how to finish what the engine left behind on converted pages,
what the review notes mean, how to go live, how rollback works, and how to write the hand-off report. Checked against the
plugin before every release.

**Status: complete, awaiting its first release.** The first release goes out with Bricks Migration Engine 1.7.0, the
version it describes (`verifiedUpTo` in `skills/bme-update/references/index.json`).

## The one rule

**The engine converts. Your assistant finishes.** Bricks Migration Engine converts Elementor pages, templates and site
settings to native Bricks data, the same way every time. Where a third-party add-on widget has no Bricks equivalent, it
leaves a labelled placeholder card in place. These skills teach an AI assistant to rebuild those cards from the widget's
original settings, with Bricks' own elements, and to record the work, and nothing else: there is no way for an assistant
to convert, re-convert or roll back through them, by design.

## What a skill is

A folder with a `SKILL.md`: a short front matter (`name`, `description`) and Markdown an AI client loads when the task
matches. It does not talk to the site; the connection does. Bricks 2.4 or later connects a client to the site through
the WordPress MCP Adapter, set up under **Bricks > AI**, and the plugin adds its own abilities once the site's owner
switches on **Bricks Migration > Settings > AI assistant** on a licensed site. The skills tell the client what the
abilities cannot: the method, which Bricks elements stand in for which widgets, what each review note asks of a human,
and what never to touch.

## Requirements

- Bricks Migration Engine 1.7.0 or later, with an active licence, on WordPress 6.9 or later for its abilities.
- Bricks 2.4 or later with its abilities enabled, for a client that works on the site.
- A client that loads skills: Claude Code, Codex, Cursor, GitHub Copilot, or any client that reads `SKILL.md` folders.

## Install

Copy the prompt from **Bricks Migration > Finish with your AI assistant** in WordPress into your client, or do it by hand.

**Claude Code, as a plugin** (updates with `/plugin marketplace update bricks-migration-engine-skills`):

```
/plugin marketplace add builtforbricks/bricks-migration-engine-skills
/plugin install bricks-migration-engine@bricks-migration-engine-skills
```

**Claude Code, Codex, Cursor and others, as a checkout** (one checkout, symlinked, so an upgrade moves every skill):

```sh
git clone https://github.com/builtforbricks/bricks-migration-engine-skills.git ~/.bricks-migration-engine/skills/bricks-migration-engine-skills
~/.bricks-migration-engine/skills/bricks-migration-engine-skills/scripts/bme-skills-upgrade   # pins the checkout to the latest release

# Pick the directory your client scans:
SKILLS_DIR="$HOME/.claude/skills"      # Claude Code, every project    (a project alone: .claude/skills)
# SKILLS_DIR="$HOME/.agents/skills"    # Codex and other agents         (Codex alone: ~/.codex/skills)
# SKILLS_DIR=".cursor/skills"          # Cursor, this project

mkdir -p "$SKILLS_DIR"
for skill in "$HOME"/.bricks-migration-engine/skills/bricks-migration-engine-skills/skills/bme-*; do
  ln -sfn "$skill" "$SKILLS_DIR/$(basename "$skill")"
done
```

Then start a new chat and ask: *List the loaded skills whose names start with `bme-`.*

## What is included

| Skill | Covers |
|---|---|
| `bme-start-here` | Load first: what the engine does, the rule, the tools, the workflow, never do |
| `bme-finishing` | The loop: list the leftovers, read each card's source context, rebuild in place, remove the card, record it |
| `bme-recipes` | Which Bricks elements stand in for which add-on widgets, by what the widget did |
| `bme-review-notes` | The engine's review note kinds, what each asks of a human, the few that are a rebuild |
| `bme-going-live` | The checks before deactivating Elementor, the order, what happens to the plugin |
| `bme-rolling-back` | What rollback does, pages edited in Bricks, rollback all, the free trial, rollback or re-convert |
| `bme-handoff-report` | The client report, from the plugin's data, engine conversion and assistant rebuilds kept apart |
| `bme-update` | Check the pack against the plugin on the site; update the pack when asked |

## The abilities the skills use

Registered by the plugin while the owner's switch is on and the licence is active; administrators only; nothing
converts:

| Ability | Does |
|---|---|
| `bricks-migration-engine/get-status` | Version, licence, documents converted, leftovers open and finished, whether Bricks' abilities are on |
| `bricks-migration-engine/list-leftovers` | Every placeholder card per converted document, with widget, add-on and the engine's note; with `scope: "all"`, every converted document and the review notes too; with `include_finished`, the recorded cards, removed ones included |
| `bricks-migration-engine/get-source-context` | One card's original Elementor settings, its place in the Bricks tree, its add-on, the design tokens it refers to |
| `bricks-migration-engine/get-design-handoff` | The variables, classes, theme style, colour palette and breakpoint the conversion created |
| `bricks-migration-engine/mark-finished` | Records a rebuilt card for the owner, apart from the engine's own output |

Building happens through Bricks' own abilities (`bricks/add-element`, `bricks/remove-element` and the rest).

## Licence

GPL-2.0-or-later, like the plugin.
