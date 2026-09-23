---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a
**design tree**: every decision branches into the decisions that hang off it.

The **frontier** is every decision whose prerequisites are already settled: the questions
you can ask _now_ without guessing at answers you haven't heard yet. Ask the frontier one
question at a time, and ask first the question whose answer settles or constrains the
most others. Early choices cascade: once they're made, many later questions stop being
questions.

For each question, first print its context and your recommendation as text:

```
❓ **<question title>**: <standalone context, then the question body, might be multiple paragraphs>

➡️ <your recommended answer, and why>
```

Then call `AskUserQuestion` with just that one question. Its `question` is a one-line
restatement, `header` a short topic label, and your recommended answer the first option
with "(Recommended)" at the end of its label. Each option's `description` says what
choosing it commits the user to. The text carries the explanation, so keep the tool call
brief.

Some questions don't have a handful of honest answers: "what failure worries you most?"
Don't invent options to squeeze these into the tool, that narrows the user's thinking
before they've done it. Offer exactly two options, your recommended answer and "Let's talk
it through", and let the user answer in their own words through Other. If the user selects
"Let's talk it through" immediately stop asking questions and prompt the user to
elaborate.

The user answers each question on its own, so the burden of context is on the asking
side, never the answering side:

- Open each question with a sentence or two of context that stand on their own, so it can
  be answered without scrolling back to the question it depends on.
- When a question builds on an earlier answer, restate that answer in words: "the
  two-tier engine you chose, alloc core plus a std feature", not "option a".
- When a question uses an option label defined elsewhere, including one from a
  sub-agent's report or another document, define it inline the first time it appears.

A question read in isolation names every earlier decision it depends on in words. Asking
"wait, what?" of it would turn up nothing new.

After every answer, recompute the frontier before asking again. Settled decisions push it
outward and unblock the questions that depended on them. When earlier answers force a
later decision, don't ask it: state the forced conclusion and the answers that force it,
in words, so the user can object. Nothing is silently assumed, but nothing forced is put
to the user as though it were open.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from
the environment (filesystem, tools, etc.), dispatch a sub-agent in the background to find
it; don't ask the user for anything you could look up yourself. `AskUserQuestion` blocks
until the user answers, so launch the sub-agent _before_ you start asking. A running
exploration is an unsettled prerequisite: only the questions downstream of it wait for
the sub-agent to report, so ask the rest of the frontier meanwhile. The _decisions_ are
the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited,
nothing left silently assumed. Do not act on it until the user confirms you have reached a
shared understanding.
