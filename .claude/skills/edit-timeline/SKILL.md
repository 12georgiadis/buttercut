---
name: edit-timeline
description: Edit an existing rough cut using natural language. Modify clip order, trim clips, remove segments, add B-roll. Works on YAML roughcut files and re-exports to FCPXML. Use when user says "edit", "cut", "remove", "reorder", "trim", "shorten", or "change" a roughcut.
---

# Skill: Edit Timeline

Edit an existing rough cut with natural language instructions.

## Step 1 — Identify the Roughcut

Ask user which library and roughcut to edit:
```bash
ls libraries/[library-name]/roughcuts/*.yaml
```
If only one YAML exists, use it directly. Otherwise list options and ask.

## Step 2 — Show Current Structure

Read the YAML and display a numbered clip summary:
```
Clip 1: [filename] — 00:00:02.92 → 00:00:28.35 (25.4s) — "In order to tell this one..."
Clip 2: [filename] — 00:00:00.00 → 00:00:05.24 (5.2s) — [B-roll: bus window]
...
Total: Xs across N clips
```

## Step 3 — Gather Edit Instructions

Ask: "What changes do you want to make?"

Accept natural language like:
- "Remove clip 3"
- "Swap clip 2 and clip 4"
- "Trim clip 1 to start at 00:00:08"
- "Cut the last 5 seconds of clip 6"
- "Remove all clips under 3 seconds"
- "Keep only clips 1, 3, 5"

## Step 4 — Launch Edit Agent

Once you have the YAML path and the edit instructions, launch the edit agent:
```
Task tool:
- subagent_type: "general-purpose"
- description: "Edit roughcut timeline"
- prompt: [See agent_instructions.md]
```

Pass to agent: YAML path, library name, edit instructions, clip summary, timestamp.

## Step 5 — Report

When agent returns:
1. Show updated clip summary (numbered)
2. Confirm new total duration
3. State path of new FCPXML file
