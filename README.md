# Reply Brain

A Claude skill that drafts X (Twitter) replies in your voice, finds tweets worth replying to every day, and keeps sounding like you after the first month.

Most reply generators get worse over time: the voice is written once, your edits are thrown away, and the same takes repeat. Reply Brain is built around a memory that fixes each of those.

It never posts. You open the tweet, paste the reply and post it yourself.

## How it works

1. **Interview.** About ten specific questions on what you believe and how you talk. No forms, no pasting old replies.
2. **Approach round.** Five tweets, including one outside your field. You say how you would approach each. You do not write the reply.
3. **Your matrix.** What you do in each of seven situations, what you believe, and what you never say. You review and correct it.
4. **Reply.** Paste a tweet, get up to three drafts, each labelled with the move it makes. Drafts that break a rule are held back and shown with the reason.
5. **Daily run.** Set a goal of 20 to 50 drafts a day, the days, the time, and who to look for. A scheduled run reads X in your Chrome and hands you a batch: each tweet with its link and a drafted reply, plus every tweet it rejected and why.
6. **Learn.** Press "I posted this" after posting. Your edits become rules you approve, replies that got answers become its examples, and the matrix updates when your behaviour changes.

## What you need

- [Claude Code](https://claude.com/claude-code), ideally the Claude desktop app.
- Python 3 (already on macOS).
- For the daily run: the Claude desktop app open at the scheduled time, and Chrome with the Claude extension, signed in to X.

## Install

```bash
git clone https://github.com/ruhisalhotrahq/reply-brain.git ~/.claude/skills/reply-brain
```

Then, in Claude:

1. "Open the reply studio." Claude publishes your own private copy of the studio page.
2. Do the interview and the approach round on that page.
3. Optional: "Set up my Reply Brain daily run." Claude schedules the run for the days and time you set on the Daily run tab.

## What is in here

| Path | What it is |
|---|---|
| `SKILL.md` | The spec Claude follows |
| `assets/studio.html` | The studio page: interview, matrix, reply studio, daily run, learning dashboard, your record |
| `scripts/brain.py` | Memory helper for chat use. Python 3, standard library only |
| `references/daily-run.md` | The spec for the scheduled daily run |
| `references/daily-run-task.md` | The scheduled task prompt |
| `references/memory.md` | Every field in the memory and the failure it prevents |
| `references/finding.md` | Rules for finding conversations worth replying to |

## Where your data lives

Nothing you enter is stored in this repository.

- **Studio page:** saved privately to your claude.ai account. Nobody else can read it unless you share the page.
- **Standalone studio** (`python3 scripts/brain.py studio`): saved in that browser.
- **Chat:** `~/.reply-brain/brain.json`, and daily batches in `~/.reply-brain/runs/`.

### Your record

Everything you tell it is also kept in a permanent record: every interview answer, every approach you chose, each version of your matrix, and every reply you posted or skipped. Redoing the interview or erasing the brain does not touch it.

- In the studio, open **Your record** to browse it and download it as Markdown or JSON.
- In chat, it is `~/.reply-brain/journal.jsonl`. Run `python3 scripts/brain.py record --out record.md` for a readable copy.

## Limits

- The studio page cannot search X by itself. The daily run does that through Claude and Chrome.
- The standalone studio is not connected to Claude, so it shows each prompt for you to copy into a Claude chat and paste the answer back.
- At 40 or 50 replies a day, spread your posting out. X limits accounts that reply in fast bursts.

## License

MIT. See [LICENSE](LICENSE).
