# Skynet vault template

An Obsidian vault set up as an **AI-maintained second brain**, built for NSW HSC students who also build stuff. You don't write the notes: Claude (or any coding agent) does. You drop in sources and ask questions. The AI files, links and keeps everything up to date.

It follows Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) pattern, plus a few token-saving ideas from the [AI-first note spec](https://github.com/eugeniughelbur/obsidian-second-brain/blob/main/AI-FIRST.md).

## How it's laid out

```
CORE.md        ~200-token "who I am + rules" card, auto-loaded into every Claude Code session
CLAUDE.md      the rulebook: layers, page format, ingest/query/lint workflows, school recipes
_CLAUDE.md     pointer to CLAUDE.md (some agents look for this name)
AGENTS.md      same pointer, for Codex and friends
index.md       one line per wiki page, generated; the AI reads this instead of browsing folders
log.md         append-only history: ## [date] op | what
raw/           your original files, never edited: inbox/ (drop zone), school/<Subject>/, tech/
wiki/          flat folder of AI-written pages, each with a type and a one-line summary
archive/       old stuff, moved instead of deleted
99-Private/    never shown to cloud AI
tools/vault.py index | find | lint | log   (Python 3, no installs)
```

## Why it's built this way

- **Cheap on tokens.** The AI reads the one-line catalogue (`index.md`), then opens only the pages it needs, so it never wanders through folders. Every page has a `summary:` line that feeds the catalogue.
- **Built from sources.** Past papers, task notifications and booklets go in `raw/`, and pages link back to them. The AI never edits a source.
- **Compounding, not re-searching.** Good answers get filed back as pages, so the wiki grows every time you use it. Karpathy's point is that a maintained wiki beats searching your notes from scratch each time.
- **Self-checking.** `python3 tools/vault.py lint` finds:
  - pages with no summary
  - links to pages that don't exist
  - pages nothing links to
  - stale overview pages
  - past papers no page uses yet
  - files waiting in the inbox

## Set it up

1. Click **Use this template** (or download the ZIP) and put the folder somewhere, e.g. `~/Documents/Skynet`.
2. Fill in `CORE.md`: your name, year, subjects and anything the AI should always know. Keep it short.
3. Rename `wiki/Example subject.md` to each of your subjects (copy it once per subject). Delete the other `Example` pages once you've seen the format.
4. Optional, for Claude Code: merge `setup/claude-settings-hook.json` into `~/.claude/settings.json`. Change the `V=` path if your vault isn't at `~/Documents/Skynet`. Every session will then rebuild the index quietly and load `CORE.md`.
5. Optional: open the folder in Obsidian (**Open folder as vault**) if you want to look at the graph. No plugins are needed.

## Using it

Just talk to the AI from inside the vault folder (or from anywhere, if you set up the hook):

- "Ingest the paper in raw/inbox" files it, maps the questions to topics and updates the calendar.
- "I got 14/20 on the Physics task, here's the feedback" updates the task, the error log and the calendar.
- "What should I study this week?" builds a plan from due dates, weightings and confidence, then files it as a page.
- "Lint the vault" runs the health check and fixes what it finds.

## Your notes stay yours

`.gitignore` keeps `raw/` and `99-Private/` out of git, so you don't push your papers or private notes by accident. If you fork this, keep it private once it has your stuff in it.
