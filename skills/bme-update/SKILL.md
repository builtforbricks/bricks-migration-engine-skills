---
name: bme-update
description: "Use when the user asks to check for or install Bricks Migration Engine skills updates, or when the plugin on the site seems newer than these skills describe. Compares the pack's verifiedUpTo with the plugin version on the connected site and, when asked, runs the upgrade script."
allowed-tools: Bash, Read
---

# Bricks Migration Engine: check for skills updates

Prerequisite: the skills were installed from `https://github.com/builtforbricks/bricks-migration-engine-skills` (a git
checkout, by default at `~/.bricks-migration-engine/skills/bricks-migration-engine-skills`, with each skill folder
symlinked into the client's skills directory).

## What it does

Says whether this pack describes the plugin on the connected site and, when asked, updates the pack. The plugin and
the pack are released together, so a mismatch means one of them moved and the other did not.

## Steps

1. **The pack's version.** Read `verifiedUpTo` in `references/index.json` beside this file. Ignore `version` (the pack's
   own number) for this comparison.
2. **The site's version.** Call `bricks-migration-engine/get-status` and read `plugin_version`. Without the plugin's
   abilities (the owner has not switched them on, or the site is on the trial), call `bricks/get-system-information` and
   read the `version` of the `plugins` entry named "Bricks Migration Engine". With no site connection, say one is needed
   to compare, and offer the pack-side check alone (step 4).
3. **Compare as versions.** Major, minor, patch left to right.
   - Equal: current; say so.
   - Site newer: the pack is behind and may not know a new ability or a changed note; recommend the update and wait.
   - Site older: the pack is ahead; an ability it names may be missing on the site. Say which plugin version the pack
     describes and suggest updating the plugin from its dashboard.
4. **Update the pack**, only when asked: run `scripts/bme-skills-upgrade` from the checkout (`--check` first says what is
   installed and what the latest release is, changing nothing). It prints one line: `BME_SKILLS_ALREADY_CURRENT`,
   `BME_SKILLS_UPDATED <old> <new>`, `BME_SKILLS_PINNED <tag>`, or `BME_SKILLS_<REASON>` on failure. Report it. A new
   chat may be needed to load the updated skills.
5. **Never** fetch the skills from any other source, edit the checkout by hand, or change the plugin.
