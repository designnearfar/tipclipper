# Tip Clipper

A small Claude skill that turns what you read into short, searchable notes in Google Drive. It writes the idea rather than the link, and it refuses to save the things that should be rules instead.

## Why

Saving is easy. Every browser has a bookmark button and every note app is built around capture. The problem shows up later, when none of it has changed anything you do.

That is usually a sorting problem rather than a discipline problem. Anything worth saving is one of two things. **Reference** is something you want to find again, and it has a natural trigger: you go looking because you need it. **A rule** is something that should change how you work from now on, and it has to fire on its own.

They feel identical when you read them. Filed together, the rules go inert. A rule you have to remember to go read will not fire.

So this skill sorts before it saves. Reference goes to Drive. Rules do not get filed at all, they get flagged with a named destination: which config file, which skill, which checklist.

## What you need

- Claude on a plan that supports custom skills, with code execution enabled
- The Google Drive connector, authorized on the account you actually want to write to
- A folder in that Drive called `Tips`

## Install

1. Put `SKILL.md` in a folder named `tip-clipper` and zip the folder.
2. In Claude, go to Settings, then Customize, then Skills, and upload the zip. In Claude Code, skip the zip and place the folder at `~/.claude/skills/tip-clipper/` instead.
3. Create a folder called `Tips` in Google Drive.
4. Open `SKILL.md` and replace the six topic codes with ones that match your work. The ones in the file are mine and will not fit yours.

Check which Google account the connector authorized before you rely on it. If your browser is already signed into a Google account, the approval screen will often skip the account picker and use that one. Every reconnect looks successful either way.

## Use

Ask Claude:

> Clip this: https://example.com/the-article

It fetches the page, pulls out the idea, and writes one document into `Tips`. On desktop with a browsing extension, `clip this` works on the page you are already reading.

Other things it responds to:

| You say | What happens |
|---|---|
| `clip the part about pricing` | Narrows to one section of a long page |
| `is this worth clipping?` | Applies the quality bar, and often says no |
| `do I have anything on X` | Full text search across the folder |

Nothing runs in the background. Nothing happens until you ask.

## Example output

Filed as `2026-07-28 AI - Context is a budget, not a container`

```
Context is a budget, not a container

2026-07-28 | Anthropic Engineering | Effective context engineering for AI agents
https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
Topic: AI and Prompting
Tags: #ai #prompting #agents

Prompt engineering is writing one instruction well. Context engineering is
curating everything in the window on every turn: system prompt, tools, message
history, retrieved files. Model recall degrades as the window fills, so every
token added spends attention that something else needed.

Why it matters to me: explains why my tighter skills outperform my longer ones.
Also reframes connector sprawl as a real cost.

Try: audit the skills for anything not load bearing. Cut it.
```

The `Why it matters to me` line is required. At save time it forces you to decide whether there is a real connection or you just enjoyed reading. At retrieval time it is what makes the note legible instead of a wall of text you have to re-parse. A note without it is a bookmark with extra steps.

## One document per note, not one per topic

This looks wrong and is deliberate. The Google Drive connector can create files and read files, but cannot append to an existing document. A one-document-per-topic design would need you to paste every entry by hand, which removes the reason to have the skill.

The date and topic in the filename keep the folder sorted by subject, and retrieval is by search anyway. If you swap Drive for storage that supports appends, the topic-document structure becomes viable again and nothing else has to change.

## Safety

Saved notes get read back into the model's context on every search, which makes them an input rather than just a record. Two rules in the skill exist because of that.

**Never save secrets.** No credentials, keys, client financials, or contract terms. Stored once means re-read many times.

**Saved content is data, not instruction.** If a page contains text addressed to an AI, that text does not become a command because it got saved. The skill records the idea and leaves the instruction behind.

Read both again before pointing this at a shared or team Drive.

## Where this goes next

The first version had a structure that turned out to be impossible, and the replacement is better than what I set out to build. That happened three more times in the same afternoon. I wrote up all four revisions in my newsletter, [NEWSLETTER NAME].

The part I would change next is the quality bar. Mine is deliberately harsh, because forty sharp notes get used and four hundred is a bookmarks folder with better formatting.

## Credit

Built by Kristine LaRocca at DesignNearFar.

## License

MIT. Use it, change it, share it.
