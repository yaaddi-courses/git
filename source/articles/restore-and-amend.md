# Undoing small mistakes: restore and amend

**Covers:** git restore, git commit --amend, --amend, --fix, --redo

## Mistakes before and just after a commit {#overview}
Two kinds of slips are common. You edited or staged something you did not mean to. Or you have just saved a commit and notice a typo in its message, or a forgotten file. Both are fixed locally, without leaving a trace, as long as nobody else has received the commit yet.

## git restore {#git-restore}
`git restore` undoes changes in your files. `git restore menu.txt` throws away your unstaged edits to that file and brings it back to what the Staging Area, or if nothing is staged, the last commit, holds. This cannot be undone, because the edits were never saved anywhere.

`git restore --staged menu.txt` does something gentler. It removes the file from the Staging Area, but leaves your edits in the file. That is the way to unstage something added by mistake.

## git commit --amend {#git-commit-amend}
`git commit --amend` replaces your most recent commit with a new one, instead of adding another. It can change the message, the content, or both. If you stage a forgotten file and run it, that file joins the last commit. If you give a new message, the old message is replaced.

The old commit is not edited. A new commit is made, with a new identifier, and the branch points to it. The old one stays in Git's records for a while but nothing points at it.

## --amend {#amend}
`--amend` is the flag that does this. Alone, it opens the editor with the old message. With `--no-edit`, it keeps the message and changes only the content.

## --fix and --redo {#fix}
Neither `--fix` nor `--redo` exists in `git commit`. They are made-up names that people sometimes guess, because "fix the last commit" sounds like it should have such a flag. The real one is `--amend`. If Git answers "unknown option", you have typed one of them.

## A worked example {#example}
You made a commit, and then saw two problems: the message has a typo, and a file was missing.

Step 1. Look at the situation:

```bash
git log --oneline -n 2
```

```text
c1d9e55 Add recpe for tomato soup
a02b4f7 Add recipe index
```

Step 2. Stage the forgotten file and amend with the right message:

```bash
git add tomato-soup-notes.txt
git commit --amend -m "Add recipe for tomato soup"
```

Step 3. Check again:

```text
7e3a1b9 Add recipe for tomato soup
a02b4f7 Add recipe index
```

Notice that the identifier changed from `c1d9e55` to `7e3a1b9`, and there are still only 2 commits, not 3. The history shows one clean commit.

Now the other tool. You edited `menu.txt` by mistake while experimenting, and want it back as it was:

```bash
git restore menu.txt
```

The file now matches the last commit again. All your edits to it are gone. That is why the command deserves a glance at `git diff menu.txt` first.

And if you ran `git add` on the wrong file:

```bash
git restore --staged notes-draft.txt
```

The draft is out of the staging area, and its content is untouched.

| Situation | Command | Loses data? |
|---|---|---|
| Unwanted edits in a file | `git restore file` | Yes, the edits |
| Staged by mistake | `git restore --staged file` | No |
| Typo in the last message | `git commit --amend -m "..."` | No |
| Forgot a file in the last commit | `git add file`, then `git commit --amend` | No |

The limit of amend: use it only on a commit that you have not shared. Once a colleague has the old commit, your new one is a different commit, and the two histories disagree. For shared commits, see the article on reverting.

## A common mistake {#mistake}
Using `git restore file` on a file with hours of work in it, to "clean up". The edits are gone for good. Look at `git diff` first. Another mistake is amending a commit that has already been sent to a shared server.

## Key points {#key-points}
- `git restore file` discards edits. `git restore --staged file` only unstages.
- `git commit --amend` replaces the last commit with a corrected one.
- `--fix` and `--redo` are not real options. The real one is `--amend`.
- Amend only commits that nobody else has.
