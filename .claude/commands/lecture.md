---
description: Turn a NotebookLM notebook (podcast/lecture/video) into a rich key-notes markdown file on the Desktop.
argument-hint: <notebook name, partial name, or "list">
allowed-tools: mcp__notebooklm__notebook_list, mcp__notebooklm__notebook_query, Write, AskUserQuestion, Bash
---

You are running the **/lecture** workflow: turn a NotebookLM source (podcast / interview / lecture / video) into a single, rich, well-structured key-notes markdown file saved to the user's Desktop.

The user's argument is: **$ARGUMENTS**

Follow these steps exactly.

## Step 1 — Find the notebook
- Call `mcp__notebooklm__notebook_list`.
- If `$ARGUMENTS` is empty or the word "list": show the user the available notebook titles (most relevant first) and ask which one to use via `AskUserQuestion`. Then stop and wait.
- Otherwise match `$ARGUMENTS` (case-insensitive, partial match allowed) against notebook titles.
  - Exactly one match → use it.
  - Multiple matches → list them and ask the user to pick via `AskUserQuestion`.
  - No match → tell the user, show the closest titles, and ask them to clarify.
- Note the matched notebook's `ID`.

## Step 2 — Research the source (two queries)
Use `mcp__notebooklm__notebook_query` against the notebook ID. Reuse the returned `conversation_id` for the second query.

**Query 1 — overview & structure:**
> "Give me an overview of this source. What type of content is it (podcast/interview/lecture), who is speaking, who is the host/interviewer, the title and approximate length, and the main topics or chapters in chronological order with approximate timestamps for each major section."

**Query 2 — questions, best moments, quotes:**
> "List the specific notable questions the host actually asked, in order, each with an approximate timestamp. Then identify the 6-8 single most insightful 'worth-watching' moments with timestamps and a one-line reason each. Also list memorable quotes with timestamps."

If the source is a solo lecture (no host/questions), skip the "questions asked" section gracefully and expand the takeaways instead.

## Step 3 — Confirm save location
Default save path: `C:\Users\<user>\Desktop\<short-kebab-title>-Notes.md` (derive the user folder from the working directory; derive a short kebab-case title from the speakers/title).
If the user already specified a location in `$ARGUMENTS` or earlier in the conversation, use it without asking. Otherwise default to the Desktop silently — only ask if Desktop can't be determined.

## Step 4 — Write the file
Write a markdown file using **exactly this structure** (adapt content to the source):

```
# <Guest> × <Host> — Key Notes      (or just "# <Title> — Key Notes" for a lecture)

> **Source:** "<title>"
> **Show/Channel:** <show> (host: <host>)
> **Guest/Speaker:** <name> — <one-line credential>
> **Format:** <type> · ~<length>
> **Video link:** _(paste the YouTube URL here)_
> **Notes prepared:** <today's date>

## 1. What this source is about
(2-4 sentences + the single core thesis.)

## 2. Is it worth watching? (and the must-watch segments)
(A clear verdict, then a timestamp table of the 6-8 highest-value segments with a "why" for each.)

## 3. Key takeaways (the big ideas, by theme)
(Grouped lessons with short explanations under each — the actual substance.)

## 4. Questions the host asked (in order, with timestamps)
(A timestamp table of every notable question. Then a shortlist of the most useful questions worth re-asking yourself. Omit this whole section for solo lectures.)

## 5. Memorable quotes
(Timestamp table of the best quotes.)

## 6. So what? — how to apply this
(3-6 concrete, actionable takeaways.)

---
*Notes generated from the NotebookLM source. Timestamps are approximate. Paste the video link above for one-click navigation.*
```

## Step 5 — Report back
Tell the user the full saved path, a 3-bullet summary of what's inside, and the honest caveat that **timestamps are approximate** (NotebookLM estimates them). Offer a shorter one-page revision version if they want it.

### Important honesty rules
- Only state facts the NotebookLM queries actually returned. Do not invent timestamps, quotes, or claims. If something is unknown, say so.
- Keep the same high-quality, scannable formatting as the structure above.
