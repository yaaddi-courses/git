# Committing: saving a snapshot with a good message

**Covers:** git commit, imperative mood, -m, -a, --verbose

## Saving for good {#overview}
Staging only prepares a change. A commit is what makes it permanent. Each commit is a snapshot of the whole project at one moment, with the author, the date and a message. You will read those messages months later, when you wonder why something changed, so writing them well is part of the work. Let's see the command first, then how to write the message.

## git commit {#git-commit}
`git commit` saves everything in the Staging Area as a new snapshot in the history. Files that changed but are not staged stay as they are and are left out. Without any option, it opens a text editor for the message. When you save and close the editor, the commit is made. If you close it with an empty message, Git cancels.

Each commit points to the one before it, so the history forms a chain. That chain is what `git log` prints, newest first.

## Imperative mood {#imperative-mood}
Here is a style question every team meets. Should the message say "Fixed login bug" or "Fix login bug"? Most teams choose the second, the imperative mood, where the message reads as a command. A useful test: the message should complete the sentence "If applied, this commit will ...". Git itself writes this way, for example "Merge branch", and teams adopt it so the history reads in one consistent voice.

A good message has a short first line, ideally under 50 characters, then a blank line, then, if needed, a longer explanation of why you made the change. The code already shows what changed, so the message should say why.

## -m {#m}
The option `-m` gives the message on the command line, as in `git commit -m "Fix login bug"`, so the editor never opens. It is the fastest way to write a short message. If you repeat `-m`, each one adds another paragraph.

## -a {#a}
The option `-a` makes `git commit` stage every modified tracked file first, so you can skip the separate staging step. It leaves out new, untracked files. Use it for quick small changes, and avoid it when you want to choose carefully what goes in.

## --verbose {#verbose}
The option `--verbose`, or `-v`, shows the full difference of the staged changes inside the editor, below the message area. You can then read exactly what you are saving while you write why.

## A worked example {#example}
Say you fixed a bug in a login check. Two files changed and are staged.

```bash
git commit -m "Fix login failure for emails with plus signs"
```

The reply looks like this:

```text
[main 3f2a9c1] Fix login failure for emails with plus signs
 2 files changed, 7 insertions(+), 2 deletions(-)
```

`3f2a9c1` is the start of the commit's identifier, and `main` is the branch that moved forward. Check the result with `git log -n 1`.

Now compare three messages for the same change:

| Message | Verdict |
|---|---|
| `fix` | Too short: what was fixed? |
| `Fixed stuff in login and also changed the footer` | Past tense, and two topics in one commit |
| `Fix login failure for emails with plus signs` | Imperative, one topic, says what and where |

Think of a developer reading `git log` six months from now, on a file with 100 commits. With clear messages, finding the one commit about plus signs takes about 10 seconds. With 100 messages like `fix`, they would open commits one by one, perhaps for half an hour.

For a bigger change, use the editor and write a body:

```text
Fix login failure for emails with plus signs

The address was cut at the plus sign before the lookup, so users
with names like ann+work@example.com were never found. Encode the
whole address instead.
```

The first line is the summary, and the body explains why the old code failed.

To save a quick edit to tracked files in one step, write `git commit -a -m "Update contact page"`. First make sure `git status` shows only the files you expect.

## A common mistake {#mistake}
A common slip is putting unrelated changes into one commit because it was convenient. Later, you cannot undo one change without the other, so stage and commit one topic at a time.

Another slip is a message like "updates", which tells the reader nothing.

## Key points {#key-points}
- A commit saves the staged changes as a permanent snapshot with a message.
- Write the first line as a command: "Fix", "Add", "Remove".
- `-m` gives the message inline, `-a` stages tracked files first, `--verbose` shows the changes in the editor.
- One commit should tell one story, and the message should say why.
