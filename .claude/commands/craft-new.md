---
description: Set up a new writer-study command — give a writer's name, get a working /<writer> command wired to their NotebookLM notebook.
argument-hint: <writer's full name, e.g. "Kanchan Pant">
allowed-tools: mcp__notebooklm__notebook_list, mcp__notebooklm__refresh_auth, Write, Read, AskUserQuestion, Bash
---

Set up a new writer-study command for: **$ARGUMENTS**

This creates a personal slash command so the user can study a new writer exactly the way `/anulata` works — `/name` for the writer's overall signature, `/name <story>` for one story in depth. All the analysis logic lives in the shared battery at `C:\Users\chandan.p\.claude\craft-battery.md`; you are only writing a small stub that points at it.

If `$ARGUMENTS` is empty, ask who the writer is and stop.

## Step 1 — Find their notebook

Call `mcp__notebooklm__notebook_list`.

On an auth error: tell the user to run `! notebooklm-mcp-server auth`, then `mcp__notebooklm__refresh_auth` and retry once.

Look for a notebook whose title plausibly matches the writer. Try the surname, the first name, and the full name — titles are often untidy (`Stories BY Anulata_Raj_Nair`).

- **One clear match** → note its exact title and source count.
- **Several** → `AskUserQuestion` to pick.
- **None** → say so plainly. Ask whether they want to create the notebook first and come back, or proceed anyway with a match string that will resolve later. Do not invent a notebook.

Report the source count. If it is 0 or 1, warn now: a single source cannot support any claim about the writer's *pattern*, only about that one piece.

## Step 2 — Settle the five fields

Work out each; ask only what you genuinely cannot infer. Use **one** `AskUserQuestion` call with multiple questions rather than several round-trips.

| Field | How to settle it |
|---|---|
| `WRITER` | From `$ARGUMENTS`, tidied to proper case |
| `PRONOUN` | **Ask.** Never infer from the name. Offer she/her, he/him, they/them |
| `NOTEBOOK_MATCH` | Shortest distinctive substring that matches only their notebook |
| `OUTPUT_DIR` | Default `C:\Users\chandan.p\Pictures\Anulata_Raj_nair_Stories\writers\<writer_slug>\craft` — confirm or take a different path |
| `QUOTE_STYLE` | **Ask** if the source language isn't obviously English. For Hindi/Urdu the user's established preference is roman-script Hinglish plus an English meaning on the next line — offer that as the default |
| command name | Short kebab slug, usually the first name (`kanchan`). Tell the user what you picked |

**Check the command name is free** before writing — list `C:\Users\chandan.p\.claude\commands\`. If taken, pick another and say so. Never overwrite an existing command.

## Step 3 — Write the stub

Create `C:\Users\chandan.p\.claude\commands\<slug>.md`:

```markdown
---
description: Research <WRITER>'s storytelling craft from their NotebookLM notebook — openings, pauses, dilemmas, subtext, perspective shifts.
argument-hint: <story name | craft topic | "draft: <your text>" | "seeds: <theme>" | blank for full profile>
allowed-tools: mcp__notebooklm__notebook_list, mcp__notebooklm__notebook_query, mcp__notebooklm__refresh_auth, Write, Read, AskUserQuestion, Bash
---

Study the craft of **<WRITER>**.

| Field | Value |
|---|---|
| `WRITER` | <WRITER> |
| `PRONOUN` | <PRONOUN> |
| `NOTEBOOK_MATCH` | <NOTEBOOK_MATCH> |
| `OUTPUT_DIR` | <OUTPUT_DIR> |
| `QUOTE_STYLE` | <QUOTE_STYLE> |
| `ARGUMENT` | $ARGUMENTS |

Now `Read` the file `C:\Users\chandan.p\.claude\craft-battery.md` and follow it exactly, using the values above.

Notebook as of <today's date>: `<exact notebook title>` — <N> sources.
<Any writer-specific note: who narrates/performs, known data quirks, duplicate sources.>
```

Substitute the real values — do not leave placeholders in the written file.

## Step 4 — Create the output folder

`mkdir -p` the `OUTPUT_DIR` via Bash.

## Step 5 — Offer the story index

A story index makes session-by-session study possible and is worth building once, now. Offer it: query the notebook for *"List every source in this notebook by its exact title, numbered. Titles only, nothing else."* and save it as `OUTPUT_DIR\STORY-INDEX.md`, grouped sensibly (by series, season, genre — whatever structure the titles reveal).

Skip if the notebook has fewer than about ten sources — the list is short enough to read directly.

## Step 6 — Report back

Tell the user:
- The command they can now type, with both forms: `/<slug>` and `/<slug> <story name>`
- Which notebook it is wired to, and the source count
- Where notes will be saved
- Anything you had to guess, so they can correct it

Then say plainly that the battery is shared: editing `craft-battery.md` changes the analysis for **every** writer command at once, while the stub holds only what is specific to this writer.
