---
name: second-brain-audit
description: |
  Audit a personal knowledge base or AI memory system against the seven-step blueprint from "How I Built My Second Brain" (Beyond Reason, October 20, 2026). Point it at a folder of notes, an export, a schema, or the skill that writes the notes; it returns one finding per step with evidence quoted from the user's own files, then the first three fixes in order. Needs no infrastructure: Markdown on SharePoint, an Obsidian vault, a Notion export, a decision log, a homegrown pipeline, and a vector database all audit the same way. Use this whenever someone asks to review, audit, stress-test, grade, or compare their notes, second brain, knowledge base, decision log, memory system, brain-dump skill, or note-taking schema against the blueprint, or asks where their memory system breaks, even if they never say "audit" or "blueprint."
license: MIT
metadata:
  version: "1.0"
  author: Eric Porres
  updated: "2026-10-20"
  source: https://promptedbyeric.substack.com/p/how-i-built-my-second-brain
---

# Second-brain audit

The blueprint is a rubric, not a recipe. The first reader to run it (October 2026) built nothing. He took the seven steps as questions, ran six weeks of his own files through them, and found the blueprint's "agent read half a thread" problem in his data in a different shape: supersession recorded in prose where his validator couldn't see it. That is what this skill does. Run against the blueprint author's own project-state layer (164 files, 2026-10-20), it found the same break: a chain of three decisions, each narrowing the last, all marked active. Steps 3, 4, and 7 of the blueprint name components the author chose so the reader can swap them; the audit runs on whatever the reader already has.

The output is a short report the owner can act on this week, with every finding tied to a file they can open. Opinion without evidence is the thing the audit exists to replace.

## What you need from the user

- Where the knowledge base lives: a path you can read, an export, or a handful of representative files. A description plus the schema or skill that writes the files is enough to start, but without files every finding is marked **Unverified** and the report says so.
- If something is missing, ask for all of it in one message. Don't ask what the files already answer.

## Inventory first

Count before judging. Measure and record, before any finding:

- file count, date range, formats, and how the files are organized (projects, folders, one file per conversation)
- frontmatter or metadata fields, and how often each one is filled
- the tag vocabulary with a count per tag
- relation fields (supersedes, extends, replaces) in structure, and how many such relations appear in prose instead
- index files, validators, scripts, and what each one checks
- whether a read side exists: any search, query skill, or retrieval layer
- whether an answer key exists: a frozen set of questions with the passage that answers each

The numbers go at the top of the report and every finding cites them. A judgment without a count is the audit's own version of an answer key the student wrote. A layer the files describe but you can't read (a search index, a validator, a manual kept elsewhere) counts as described, not verified: say so, and mark whatever depends on it Unverified. If the files contradict the owner's description of the system, the files win, and the finding says where they disagree.

## The seven steps

For each step: what the blueprint says, what to check, and what breaking looks like.

### 1. Decide what counts as memory

Blueprint: ingest records of thinking (conversations, plus the syntheses written on purpose) and leave out the files the thinking produced. One boundary goes with it: employer data stays in employer systems.

Check what gets in, what is excluded, and whether the gate sits at write time or at ingest. Filtering at the door (refusing to save a note with no decision, rejected alternative, blocker, or deliverable) is a legitimate alternative to ingest-everything; name which one the system does. Breaks when outputs are mixed in with the reasoning that produced them, when no boundary is stated, or when employer and personal material share one store.

### 2. Write the answer key before you tune anything

Blueprint: collect real questions the owner has asked and the exact passage that answers each, freeze them, and ship no retrieval change without a before-and-after on that set. Record every rejected change with the result that would reverse it. Someone other than the builder checks the key; a second model counts.

Check whether a key exists, who wrote it, and whether rejections are recorded. The common finding is that nothing exists and the validator checks structure, not retrieval. A key seeded from the system's own earlier results is not a key. If none exists, draft the first ten questions from the knowledge base's own traps (below) and hand them over as the fix.

### 3. Store it where you own it

Blueprint: split conversations into passages, embed them, and keep the vectors in a database the owner controls. Grow the disk before a migration, write in small committed batches.

Check whether the owner can export everything whole, whether a backup exists, and who holds the host. Markdown in a folder the owner controls passes this step. Don't prescribe the blueprint's stack; it names components to make them swappable. Breaks when the only copy sits in a product with no whole export, or when there is no backup.

### 4. Search two ways, then let a reranker read

Blueprint: run a meaning search and a keyword search, fuse them, collapse duplicates before the cut, and blend in recency for time questions, because a memory holds every decision the owner later reversed and the old answer can outrank the new one.

Check first whether a read side exists at all. A write-only knowledge base fails this step, and it is the most common finding: the query layer is always next on the roadmap. Then check how a reader finds "what is true now" versus "why we decided": often two layers exist (a current-state file and the decision history) and the order of authority between them is unwritten. Where both exist, count the projects whose newest decision is later than the last update to the current-state file; that gap is the recency problem at its simplest. Then compute the tag distribution. A tag on more than about half the files filters nothing; that is collapse. A tag on one file is drift. Both fail, and a validator that only watches for singletons misses collapse entirely. If there is no tag field, run the same test on whatever field classifies the files (status values, owner tags, types) and say which field you used. Breaks when there is no query path, when tags have collapsed, or when nothing handles recency.

### 5. Return pointers with receipts

Blueprint: every result carries its source, speaker, date, and a fingerprint of the exact text. The agent reopens the passage before relying on it. A score ranks; it doesn't vouch.

Check for a provenance field (written live, reconstructed from a transcript, backfilled) and whether a reader can get from an index line to the exact source. Breaks when summaries have no path back, or when the index holds one truncated line per file and detection runs on that digest instead of the files.

### 6. Write the operating manual

Blueprint: a short skill file tells the agent how to use the memory. Three rules have earned their place: a hit is a pointer, never the answer; before calling a question open, ask it again in neutral words and chase the thread forward in time; an empty result means nothing matched, never that the search broke, so errors have to say they are errors.

Check whether such a file exists and whether it carries those three rules or equivalents. Then ask the question the first field report added: where does supersession live? In prose ("extends," "amends," "replaces," "nuances"), in file metadata, or in decision-level IDs with typed relations? Run the granularity test: find a file that declares it supersedes another and check whether every decision in the older file was replaced. If only some were, a two-state schema (active or superseded) can't express it, and the owner has been patching in prose. Count relations in prose against relations in structure: grep for supersedes, replaces, extends, amends, modifies, narrows, nuances, overrides, and reverses, count lines, and say they are line counts. Breaks when supersession lives only in prose, when file-level status can't say "partial," or when several versions of one scope are all marked active.

### 7. Then test it on a public benchmark

Blueprint: LongMemEval_s, all 500 questions, run MemPalace's way (one document per session built from the user's turns, a hit when any correct session lands in the top five, the 50/450 split with seed 42, report the 450 never tuned on). Name the embedder; say plainly that retrieval recall is not answer accuracy.

Only applies to a system with a retrieval pipeline. If there is none, mark it **Not applicable** and say that the step-2 answer key is the benchmark until a pipeline exists. A result older than the last retrieval change is stale: Holds only when the run postdates the latest change, otherwise Breaks, with the date gap named. The benchmark proves a pipeline works on someone else's life; steps 1 and 2 make it work on the owner's.

## Traps

When no answer key exists, draft the first ten questions from what the knowledge base's own history makes hard:

- a decision that was later reversed ("what is the scope of X today?")
- a responsibility that moved between people or systems ("who calculates the P&L now?")
- a name that appears in two projects
- a question whose answer is split across a current-state file and a decision log, with an order of authority between them
- a thread where the latest file is not the last word

Each question gets the exact passage that answers it and the date. Freeze them.

## Report

Use this template every time, so reports from different systems can be compared:

```
# Second-brain audit: <system name>, <date>

Inventory: <N> files across <N> projects, <date range>, <N> decisions if countable.
Formats: <...>. Tags: <top three with counts> of <N> total. Relations: <N> in
structure, <N> in prose. Read side: <yes/no>. Answer key: <yes/no>.

## 1. What counts as memory
Status: Holds | Breaks | Missing | Not applicable | Unverified
Evidence: <two or three quoted lines, each with file path and date>
Finding: <one paragraph: what the data shows and what it costs the reader>

## 2. Answer key
...

(one section per step, 1 through 7)

## Fix first
1. <change>, for step <N>, small enough to do this week
2. ...
3. ...

## Answer key draft
<only when step 2 found none: ten questions, each with the file, the dated
passage that answers it, and the trap that makes it hard>

## Not checked
<what the audit couldn't see: no file access, sampled N of M, scripts not run>
```

One status per step. When a step splits (supersession breaks, the manual is unverified), the status is the worst verified part and the finding names the rest. Order the fixes by what unblocks the next one. A validator catch comes before a schema change; a schema change comes before the query layer that reads it.

## Rules

- Evidence before opinion. Every Breaks or Missing cites a file. No file, no finding; write Unverified instead.
- Don't prescribe the blueprint's stack. The owner's system has to survive on the owner's infrastructure.
- Label inference. "The read side never got built" is a finding. "Because you ran out of time" is a guess, and it says so.
- The audit is a pointer too. The owner confirms before anything changes. Don't rewrite their files.
- One page plus evidence, plus the answer-key draft when there is one. The first field report ran about 700 words and changed the design.

## Example finding

The shape of a step-6 finding on a file-per-conversation knowledge base with frontmatter:

> **Status: Breaks.** Evidence: `projects/budget-app/2026-09-11.md` declares `supersedes: 2026-09-10.md`; the body of `2026-09-10.md` carries a hand-written note "partially superseded" with no field for it; five relations across the base are written in prose ("extends," "modifies," "amends," "replaces," "nuances") against one in frontmatter; the project scope has four versions, all with `status: active`. Finding: supersession at file granularity can't express partial replacement, so the owner patched around it in prose where the validator can't see. The schema needs a third status now and decision-level IDs with typed relations next; the vocabulary already exists in the prose.

## Source

The blueprint: "How I Built My Second Brain," *Beyond Reason*, October 20, 2026, at https://promptedbyeric.substack.com/p/how-i-built-my-second-brain. The steps above summarize it; the article carries the reasoning and the receipts. The author's own build stays private; this skill is the part that runs on anyone's system.
