# Going back: revert and reset

**Covers:** git revert, git reset, --soft, --mixed, --hard

## Two ways to go back, for two situations {#overview}
Sometimes a whole commit turns out to be wrong. There are two answers, and the choice depends on one question: has anyone else received that commit? If yes, you add a new commit that cancels it. If not, you may move the branch backwards as if it never happened.

## git revert {#git-revert}
`git revert` undoes an earlier commit by making a new commit that does the opposite. The original stays in the history, and so does the revert. Nothing is rewritten, so it is safe for commits that others already have. It is the polite way to back out a change on a shared branch, and the history shows both the mistake and its correction.

## git reset {#git-reset}
`git reset` moves your current branch back to an earlier commit, in effect rewinding it. The commits after that point are no longer on the branch. What happens to your files depends on the mode, listed next. Because it rewrites the branch, use it only for work that is not shared.

## --soft {#soft}
`--soft` moves the branch only. Your files and the Staging Area stay as they were, so the changes of the removed commits appear as staged. It is the mode for "I want to redo this commit differently": the work is still staged and ready.

## --mixed {#mixed}
`--mixed` is the default. It moves the branch and empties the Staging Area, but keeps your files as they are. The changes come back as ordinary unstaged edits. Use it to take apart a commit and stage the pieces again.

## --hard {#hard}
`--hard` moves the branch and also overwrites your files and the Staging Area to match the target commit. Edits in files that are tracked are lost, and cannot be recovered by Git unless you saved them in a commit. It is the only mode that destroys uncommitted work.

## A worked example {#example}
A branch holds these commits, newest first:

```text
e5f1 Add discount banner
b83c Add search page
a02b Add recipe index
```

**Case 1: the banner commit was already shared.** Use revert.

```bash
git revert e5f1
```

The history becomes:

```text
91ac Revert "Add discount banner"
e5f1 Add discount banner
b83c Add search page
a02b Add recipe index
```

There are 4 commits, one more than before. The banner's files are gone, and everyone who pulls gets the removal as an ordinary update.

**Case 2: the banner commit is local only.** Use reset. Compare the three modes, all starting from the same state and run as `git reset <mode> b83c`:

| Mode | Branch | Staging Area | Your files |
|---|---|---|---|
| `--soft` | Back to `b83c` | Banner changes staged | Banner changes still there |
| `--mixed` | Back to `b83c` | Empty | Banner changes still there, unstaged |
| `--hard` | Back to `b83c` | Empty | Banner changes gone |

After any of them, `git log --oneline` shows 2 commits: `b83c` and `a02b`.

Which to choose: if you only want to recommit with a better message or split it, use `--soft` or `--mixed`. Use `--hard` only when you are sure you want the work gone, for example a failed experiment.

A safety check: before `--hard`, run `git status`. If it shows any modified files you care about, save them first (commit them to another branch, or copy them away). A commit that was reset away can still be found for some time using the reflog, described in a later article, but edits that were never committed cannot.

The question that decides everything is "have others got it?". A reset on shared history makes your branch disagree with theirs, and the next push is rejected or, if forced, removes their work from the server.

## A common mistake {#mistake}
Using `git reset --hard` to "start over" with unsaved work in the folder. That work is lost. Another mistake is resetting a shared branch. Use `git revert` there.

## Key points {#key-points}
- `git revert` adds a new commit that cancels an old one. It is safe on shared history.
- `git reset` moves the branch back. It rewrites history, so use it on local work only.
- `--soft` keeps changes staged, `--mixed` keeps them unstaged, `--hard` throws them away.
