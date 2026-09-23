---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a
**design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are
already settled: the questions you can ask _now_ without guessing at answers you haven't
heard yet. Ask the whole frontier in one round: number each question and give your
recommended answer. Number straight through the session, never restarting per round: if
round one ends at Q4, round two starts at Q5, so "Q1" names exactly one question. Then
wait for the user's answers before the next round.

Format a round like so:

```
❓ **Q1** - **<question title>**: <standalone context, then the question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <standalone context, then the question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

The user answers a round tersely, `Q1: a, Q2: agree`, and that only works because the
question carries the context. So the burden of context is on the asking side, never the
answering side:

- Open each question with a sentence or two of context that stand on their own, so it can
  be answered without scrolling back to the round it depends on.
- When a question builds on an earlier answer, restate that answer in words, never by
  letter alone: "the two-tier engine you chose in Q1, alloc core plus a std feature", not
  "Q1 (a)".
- When a question uses an option label defined outside this round, including one from a
  sub-agent's report or another document, define it inline the first time it appears.

A question from a later round, read in isolation, names every earlier decision it depends
on in words. Asking "wait, what?" of it would turn up nothing new.

Each round the user answers reshapes the tree: settled decisions push the frontier outward
and unblock questions that depended on them. Recompute the frontier and ask the next
round. A question whose answer depends on another question still open in this round
belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from
the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the
user for anything you could look up yourself. Don't block on it: a running exploration is
an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent
to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to
them and wait.

The session is done when the frontier is empty: every branch of the design tree visited,
nothing left silently assumed. Do not act on it until the user confirms you have reached a
shared understanding.
