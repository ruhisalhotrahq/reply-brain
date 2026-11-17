# Finding conversations worth replying to

Finding scales. Posting does not. Keep the two budgets separate.

## Before any search

1. Run `python3 scripts/brain.py stats` and read today's posted count and the per-person state. Compute every cap from what was posted, never from what was drafted.
2. Read the matrix. Any situation marked skip is out of scope for the search.

## How to search

- Look for conversations, not broadcasts: a person saying something specific that invites a response.
- Every query carries a date floor. Default: nothing older than 24 hours.
- Do not filter latest-results searches by likes. It hides the fresh posts you are looking for.
- Skip job ads, giveaways, engagement bait and anything from an account the person has asked to avoid.

## Two tests on every candidate

1. Can the angle be stated in one sentence that the poster would recognise as a response to what they said?
2. Would a peer with nothing to sell naturally say this?

Fail either and the candidate is rejected.

## Output

For each candidate that passes: the tweet text, author, link, the situation it falls under, and the one-sentence angle. Then run Workflow 2 on it.

Also list every candidate you rejected and the rule that rejected it. If fewer than three candidates qualify, name the main cause. Never lower the bar to reach a number.

## Failure

Login wall, rate limit or no access to X: stop, say what happened, and return nothing. A partial pass presented as a full one is worse than no result.
