---
name: toeic
description: Generates a MulmoCast JSON script for a given English vocabulary word. Use when the user wants to create a vocabulary learning video script.
argument-hint: <word> [image style] OR word1, word2, word3 [image style]
---

## Routing

First, check if `$ARGUMENTS` contains multiple words separated by commas or newlines.

- **Single word**: proceed directly with the instructions below.
- **Multiple words**: launch one sub-agent **per word in parallel** using the Agent tool (subagent_type: `general-purpose`). Each sub-agent receives the full prompt below with a single word substituted for `$ARGUMENTS`. Wait for all agents to complete, then summarize the results.

---

Generate a MulmoCast vocabulary video script for the word: $ARGUMENTS

## Instructions

1. Read `templates/word_meticulous_animated.json` as the structural template.
2. Create a new JSON file at `scripts/word_<slug>.json` where `<slug>` is the word lowercased with spaces replaced by underscores (e.g. "a lot of" → `word_a_lot_of.json`).
3. If an image style is specified in the arguments (e.g. "Ghibli style", "anime style"), apply it to all three image prompts. Otherwise use photorealistic style.

## Content to generate

For the target word, determine:
- **Part of speech** (noun, verb, adjective, etc.)
- **Definition** in English (concise, 1–2 lines)
- **3 example sentences** using the word naturally. Make them vivid and memorable — real-world scenarios are best.
- **Japanese translation** for each sentence
- **3 image prompts** (one per sentence) that visually illustrate the scene. Each prompt must be detailed and cinematic.

## JSON structure rules

Follow the template exactly:

- `title`: `"Word: <Word>"`
- `canvasSize`: 1024×1536
- `speechParams`: keep identical to template (Google TTS, Leda voice)
- `audioParams`: keep identical to template (same BGM URL, bgmVolume 0.1)
- `imageParams`: provider google, model `gemini-2.5-flash-image`, with keys `sample_1`, `sample_2`, `sample_3`
- **Beat 1** — intro title card (html_tailwind only, `speechOptions.speed: 2.0`, spoken text `"見るだけで新しい単語が身につく動画です。"`)
  - Adjust `font-size` on the word display to fit: use `148px` for short words, scale down for longer ones
- **Beat 2** — word + definition (`duration: 5.0`, animated badge + definition fade-in)
- **Beat 3** — all 3 example sentences sliding in sequentially (no image, purple gradient bg)
  - Use `text-5xl` for short sentences; drop to `text-4xl` if sentences are long
- **Beats 4–6** — one per sentence, each with the corresponding `sample_N` photo overlay, English sentence + divider + Japanese translation animating in

All animation beats must have `"animation": true` and use `MulmoAnimation` scripts with the same timing pattern as the template.
