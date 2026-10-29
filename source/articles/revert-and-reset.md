# Going back: revert and reset

**Covers:** git revert, git reset, --soft, --mixed, --hard

## Two ways to go back, for two situations {#overview}
Sometimes a whole commit turns out to be wrong. Git gives you two answers, and one question picks between them: has anyone else received that commit? If yes, you add a new commit that cancels it. If not, you may move the branch backwards as if the commit never happened. Let's look at each answer.

## git revert {#git-revert}
`git revert` undoes an earlier commit by making a new commit that does the opposite. The original stays in the history, and so does the revert. Nothing is rewritten, so it is safe for commits that others already have. It is the polite way to back out a change on a shared branch, and the history shows both the mistake and its correction.

## git reset {#git-reset}
`git reset` takes the other route. It moves your current branch back to an earlier commit, in effect rewinding it, and the commits after that point are no longer on the branch. What happens to your files depends on the mode, and the next three sections list them. Because reset rewrites the branch, use it only for work that is not shared.

## --soft {#soft}
With `--soft`, only the branch moves. Your files and the Staging Area stay as they were, so the changes of the removed commits appear as staged. It is the mode for "I want to redo this commit differently", because the work is still staged and ready.

## --mixed {#mixed}
The mode `--mixed` is the default. It moves the branch and empties the Staging Area, but keeps your files as they are, so the changes come back as ordinary unstaged edits. Use it to take a commit apart and stage the pieces again.

## --hard {#hard}
The mode `--hard` moves the branch and also overwrites your files and the Staging Area to match the target commit. Edits in tracked files are lost, and Git cannot recover them unless you saved them in a commit. It is the only mode that destroys uncommitted work.

## A worked example {#example}
Let's say a branch holds these commits, newest first:

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

**Case 2: the banner commit is local only.** Use reset. Here are the three modes, all starting from the same state and used as `git reset <mode> b83c`:

| Mode | Branch | Staging Area | Your files |
|---|---|---|---|
| `--soft` | Back to `b83c` | Banner changes staged | Banner changes still there |
| `--mixed` | Back to `b83c` | Empty | Banner changes still there, unstaged |
| `--hard` | Back to `b83c` | Empty | Banner changes gone |

After any of them, `git log --oneline` shows 2 commits: `b83c` and `a02b`.

How do you choose? If you only want to recommit with a better message or split the commit, use `--soft` or `--mixed`. Use `--hard` only when you are sure you want the work gone, for example a failed experiment.

Here is a safety check. Before `--hard`, run `git status`. If it shows modified files you care about, save them first, by committing them to another branch or copying them away. A commit that was reset away can often still be found for some time through the reflog, but edits that were never committed cannot.

The question that decides everything is "have others got it?". A reset on shared history makes your branch disagree with theirs, and the next push is rejected or, if forced, removes their work from the server.

## A common mistake {#mistake}
A common slip is using `git reset --hard` to "start over" with unsaved work in the folder. That work is lost.

Another slip is resetting a shared branch. Use `git revert` there.

## Key points {#key-points}
- `git revert` adds a new commit that cancels an old one. It is safe on shared history.
- `git reset` moves the branch back. It rewrites history, so use it on local work only.
- `--soft` keeps changes staged, `--mixed` keeps them unstaged, `--hard` throws them away.
