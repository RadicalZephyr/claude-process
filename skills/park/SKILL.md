---
name: park
description: Park this project. Write STATUS.md, commit everything, push, so leaving is free.
argument-hint: "optional blocker or reason, e.g. blocked on sodium#12"
disable-model-invocation: true
---

# Park

Park the project so nothing stays stranded in this session or in the user's head. Ten seconds of their attention: write the status file, commit everything, push, report one line. The state worth saving is mostly in this conversation, which is why the user runs this here rather than from a fresh session.

Argument from the user, verbatim, for the Blockers section: `$ARGUMENTS`

## 1. Locate STATUS.md

- Code repo: `STATUS.md` at the repository root.
- Bible, a folder per project each with `notes/`, `decisions/` and `STATUS.md`: the project folder the working directory is in or under. With no project folder in sight, the repository root.

Done when there is one path.

## 2. Write STATUS.md

Overwrite the whole file. It is current state, not a log; git keeps history. One screen at most, and only what no commit, note or issue already says. Point at those instead. Drop a section rather than write "none" under it, except Blockers, where "None" is information.

```markdown
# Status

Parked YYYY-MM-DD on `<branch>`.

## Where things stand
Done and in flight, a few lines, drawn from this conversation.

## Next action
The one thing to do first on return, concrete enough to start cold.

## Blockers
`blocked on sodium#12` style, with a link or path. The user's argument goes here verbatim.

## Pointers
Branch, PR or issue, latest note, anything the next session has to open. Paths and URLs, not summaries.
```

Done when the user could resume from the file alone, a month from now.

## 3. Commit everything

Protect first, applied twice:

- Untracked content is protected by committing it, even when it is unclear whether it matters. A stray file on a WIP branch is reversible; a lost one is not. So stage everything: `git add -A`.
- Secrets are protected by leaving them out. Before staging, list untracked files (`git status --porcelain`) and stop to ask about any that looks like a credential by name (`.env*`, `*.pem`, `*.key`, `id_*`, `*credential*`, `*secret*`, `*token*`) or by content (an API key, a password, a private key block). Stage the rest, ask about those, continue when the user answers.

Commit message: first line `park: <one-line state>`, body naming each untracked file that was swept up, one per line.

Done when `git status` is clean apart from anything the user chose to keep out.

## 4. Push

`git push -u origin <current-branch>`, plain. Never force. Never switch branches. No remote: say so and stop, since a branch with nowhere to go is not parked. Push rejected: report the error and stop, leaving rebase or merge to the user.

On the default branch (`main` or `master`), commit and push anyway and say so in the report.

Done when origin has the parked commit.

## 5. Report

One line: branch, short hash, how many untracked files were swept up, and "on main" when that applies.
