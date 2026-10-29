# Committing: saving a snapshot with a good message

**Covers:** git commit, imperative mood, -m, -a, --verbose

## Saving for good {#overview}
Staging only prepares a change. A commit makes it permanent. Each commit is a snapshot of the whole project at one moment, with the author, the date and a message. The messages are what you read months later when you wonder why something changed, so writing them well is part of the work.

## git commit {#git-commit}
`git commit` permanently saves everything currently in the Staging Area as a new snapshot in the history. Files that are changed but not staged are left out and stay as they are. Without any option, it opens a text editor for the message. When you save and close the editor, the commit is made. If you close it with an empty message, Git cancels.

A commit points to the one before it, so the history forms a chain. That chain is what `git log` prints, newest first.

## Imperative mood {#imperative-mood}
The imperative mood is a way of writing the commit message as a command, like "Fix login bug" and not "Fixed login bug" or "Fixing login bug". A useful test: the message should complete the sentence "If applied, this commit will ...". This is the style Git itself uses in the messages it writes, such as "Merge branch". Teams adopt it so that the history reads in one consistent voice.

A good message has a short first line, ideally under 50 characters, then a blank line, then, if needed, a longer explanation of why the change was made. The code already shows what changed, so the message should say why.

## -m {#m}
`-m` gives the message on the command line, as in `git commit -m "Fix login bug"`, so the editor is not opened. It is the fastest way for a short message. Repeating `-m` adds further paragraphs.

## -a {#a}
`-a` makes `git commit` stage every modified tracked file first, so you can skip the separate staging step. It does not include new, untracked files. Use it for quick small changes, and not when you want to choose carefully what goes in.

## --verbose {#verbose}
`--verbose`, or `-v`, shows the full difference of the staged changes inside the editor, below the message area. You can read exactly what you are saving while you write why.

## A worked example {#example}
You fixed a bug in a login check. Two files changed and are staged.

```bash
git commit -m "Fix login failure for emails with plus signs"
```

The reply looks like this:

```text
[main 3f2a9c1] Fix login failure for emails with plus signs
 2 files changed, 7 insertions(+), 2 deletions(-)
```

`3f2a9c1` is the start of the commit's identifier. `main` is the branch that moved forward. Check the result with `git log -n 1`.

Compare messages for the same change:

| Message | Verdict |
|---|---|
| `fix` | Too short: what was fixed? |
| `Fixed stuff in login and also changed the footer` | Past tense, and two topics in one commit |
| `Fix login failure for emails with plus signs` | Imperative, one topic, says what and where |

In six months, a developer who runs `git log` on a file of 100 commits will read each first line. With clear messages, finding the one commit about plus signs takes about 10 seconds. With 100 messages like `fix`, they would have to open commits one by one, perhaps for half an hour.

For a bigger change, use the editor and write a body:

```text
Fix login failure for emails with plus signs

The address was cut at the plus sign before the lookup, so users
with names like ann+work@example.com were never found. Encode the
whole address instead.
```

The first line is the summary. The body explains why the old code failed.

To save a quick edit to tracked files in one step: `git commit -a -m "Update contact page"`. Be sure first that `git status` shows only the files you expect.

## A common mistake {#mistake}
Putting unrelated changes into one commit, because it was convenient. Later, you cannot undo one change without the other. Stage and commit one topic at a time. Another mistake is a message like "updates", which tells the reader nothing.

## Key points {#key-points}
- A commit saves the staged changes as a permanent snapshot with a message.
- Write the first line as a command: "Fix", "Add", "Remove".
- `-m` gives the message inline, `-a` stages tracked files first, `--verbose` shows the changes in the editor.
- One commit should tell one story, and the message should say why.
