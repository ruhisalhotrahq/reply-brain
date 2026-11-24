# The memory: what `brain.json` holds and why

Every field exists to stop one specific way replies get worse over time.

## Shape

```json
{
  "version": 2,
  "updatedAt": 0,
  "setupDone": true,
  "identity": {
    "handle": "yourhandle",
    "who": "I",
    "role": "what they do, one line",
    "audience": "who should be reading the replies",
    "reaction": "what a reader should think, in the reader's words"
  },
  "traits": ["short personality statements from the interview"],
  "matrix": [
    {"situation": "win", "move": "kind", "note": "what they bring here", "skip": false}
  ],
  "positions": [
    {"id": "p1", "claim": "", "why": "the mechanism", "proof": "a story or number", "uses": 0, "lastUsed": 0}
  ],
  "never": ["things this person will not say"],
  "style": {"length": "short", "casing": "sentence", "edge": "blunt", "humor": "dry", "emoji": "never"},
  "banned": ["extra phrases to block"],
  "material": [{"id": "m1", "text": "something real from this week", "at": 0, "uses": 0}],
  "feedback": [{"id": "f1", "at": 0, "text": "why a draft was not me, in my words", "draft": "", "tweet": "", "author": "", "url": "", "status": "open | done"}],
  "background": [{"id": "b1", "kind": "fact | story | proof", "text": "", "uses": 0, "lastUsed": 0}],
  "interview": [{"q": "", "a": ""}],
  "approaches": [{"situation": "", "tweet": "", "move": "", "note": ""}],
  "ledger": [],
  "rules": [{"id": "r1", "text": "", "evidence": 3, "at": 0, "reviewBy": 0, "active": true}],
  "caps": {"perDay": 10, "perPersonWeek": 2, "stopAfter": 3}
}
```

`profile --file` accepts any subset of: `identity`, `traits`, `matrix`, `positions`, `never`, `style`, `banned`, `interview`, `approaches`, `background`.

## Situations and moves

Situations (matrix rows): `win` someone shares a win, `struggle` someone is having a hard time, `take_agree` an opinion they agree with, `take_disagree` an opinion they think is wrong, `question` a question in their field, `news` an announcement or industry news, `outside` a topic outside their field.

Moves: `add` add what I know, `ask` ask a specific question, `pushback` disagree with the claim, `story` tell a short story, `joke` a dry joke that makes a point, `kind` plain support with no angle, `skip` do not reply.

## Ledger entry

```json
{
  "id": "e1", "at": 0, "tweet": "", "author": "", "tier": "stranger",
  "situation": "question", "move": "add",
  "draft": "what was drafted", "final": "what was posted",
  "action": "used | edited | skipped",
  "tags": ["too long"], "position": "p1", "material": null,
  "result": {"replied": true, "at": 0},
  "learned": false
}
```

## What each part prevents

| Part | Failure it prevents |
|---|---|
| `matrix` | Replies that are well written but not what this person would do in that situation |
| `positions` with `uses` and `lastUsed` | The same three takes repeated. Least recently used are offered first |
| `feedback` | The person knowing exactly what is wrong but having no way to say it beyond a tag. Open notes go into every prompt until the person marks them fixed |
| `background` | Generic replies with nothing of the person in them, and invented credentials. Only listed facts, stories and numbers may be used; least used offered first |
| `material` | Running out of fresh stories. Unused items are offered first, items older than 30 days are marked stale |
| Ledger `final` text | Sliding back to a generic AI voice. Posted replies are the examples in every prompt |
| Ledger `draft` next to `final` | Throwing away edits. The pairs go into the prompt and into rule learning |
| Ledger `tags` | Repeating a complaint. Recent tags are counted and shown to the drafter |
| Ledger `result` | Optimising for "was it used" only. Replies that got an answer rank higher as examples |
| `rules` with `evidence` and `reviewBy` | Rules that pile up and contradict each other. Max 12 active, each reviewed after 45 days |
| Recent openings list | Every reply starting the same way |
| Similarity check | Near-duplicate replies to different people |
| `caps` | Replying to the same person too often, or after they have ignored three replies |
| Ledger limit of 300 | A log too long to use. Oldest skipped entries are pruned first |

## The check every draft goes through

Blocks: over 280 characters, a banned phrase, the "it's not X, it's Y" pattern, the same first three words as any of the last 30 posted replies, or more than 45% phrase overlap with a recent posted reply.

Warns: a long dash, a hashtag, more than one exclamation mark.

## The record (separate from the brain)

The brain is a working memory: it is bounded, pruned and rewritten. The record is the person's own archive and is never pruned or rewritten. Entry types:

| Type | Fields |
|---|---|
| `answer` | `q`, `a` |
| `approach` | `situation`, `tweet`, `move`, `note` |
| `profile` | `identity`, `traits`, `matrix`, `positions`, `never`, `style` (a snapshot) |
| `reply` | `action`, `tweet`, `author`, `situation`, `move`, `draft`, `final`, `tags` |
| `result` | `final`, `author`, `replied` |
| `rule`, `material` | `text` |
| `background` | `kind`, `text`, `source` |
| `feedback` | `text`, `draft`, `tweet`, `author`, `url` |

Every entry also has `id`, `at` (milliseconds) and `type`.

## Matrix drift

When the person has posted at least three replies in a situation and their most common move there differs from the matrix row, `learn` and the studio both suggest updating the row. The matrix follows the person, not the other way round.
