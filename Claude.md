# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a **mulmo-skills** repository — a collection of Claude Code skills that generate [MulmoCast](https://github.com/receptron/mulmocast) JSON video scripts. MulmoCast is a tool for producing narrated slide-style videos from structured JSON.

## Repository Structure

- `.claude/skills/` — skill definitions (SKILL.md files with frontmatter + prompt instructions)
- `templates/` — reference JSON files that skills use as structural templates
- `scripts/` — generated MulmoCast JSON output files (one per word/topic)

## Tools

When I ask you to update mulmocast, run "npm update -g mulmocast"

The schema of MulmoScript is available at the following URL.

https://raw.githubusercontent.com/receptron/mulmocast-cli/refs/heads/main/src/types/schema.ts

## Generating a Movie

To render a script into a video:

```
mulmo movie --grouped {path_to_script}
```

Example: `mulmo movie --grouped scripts/word_conference.json`
