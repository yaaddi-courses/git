# Force pushing safely and recovering with the reflog

**Covers:** -f, --force, --force-with-lease, --force-with-___, git reflog

## When history changes and when it is lost {#overview}
Amending, rebasing and resetting all change commits that already exist. If those commits were sent to a remote, the remote and your copy now disagree, and a normal push is refused. Forcing the push settles the argument, by making the remote look like your copy, which is dangerous if it holds work you do not have. Luckily, Git also keeps a private record that lets you recover almost anything you did locally.

## -f {#f}
`-f` is the short form of `--force`, as in `git push -f`. It replaces the remote branch with yours, even if that discards commits that exist only there.

## --force {#force}
`--force` is the long form. It tells Git to carry out a command even when Git would normally refuse for safety. In a push, it overwrites the remote branch without checking whether anyone else added to it. Use it as a last resort, and not on branches that others use.

## --force-with-lease {#force-with-lease}
`--force-with-lease` is a safer version. It forces the push only if the remote branch is still where you last saw it. If someone pushed after your last fetch, it fails, and you learn about their work before destroying it. It costs nothing extra to type, so prefer it every time.

## --force-with-___ {#force-with-blank}
In the course, the gap in `--force-with-___` is `lease`. The full name is `--force-with-lease`. The blank is only a memory exercise, and not a real option name.

## git reflog {#git-reflog}
`git reflog` shows a local log of everywhere your branches and `HEAD` have pointed, even after commits became unreachable through a reset, a rebase or a deleted branch. It is a safety net. Each entry has a position like `HEAD@{3}` and a short note on what happened. It is kept on your machine only, and entries expire after about 90 days by default.

## A worked example {#example}
You rebased your feature branch to tidy it, and the branch was already on the server. Now the normal push fails:

```text
! [rejected] add-search-box -> add-search-box (non-fast-forward)
```

You are sure that the branch is yours alone. Compare the options:

| Command | What it does | Risk |
|---|---|---|
| `git push -f` | Overwrites no matter what | Destroys a colleague's commit if they pushed |
| `git push --force-with-lease` | Overwrites only if the remote is as you last saw it | Fails safely if someone pushed |

Use the second:

```bash
git push --force-with-lease
```

If a colleague had pushed a commit to your branch an hour ago, you would see `stale info`, and the push stops. After a fetch, you can look at their commit, and combine it.

Now a different disaster. You ran `git reset --hard HEAD~3`, and then realized that the three commits you removed had work you needed. The commits are not on the branch, but the reflog remembers:

```bash
git reflog
```

```text
a02b4f7 HEAD@{0}: reset: moving to HEAD~3
e5f1c88 HEAD@{1}: commit: Add discount banner
b83c0d4 HEAD@{2}: commit: Add search page
c4d1a99 HEAD@{3}: commit: Add search input
```

The entry `HEAD@{1}` is where the branch was before the reset. Go back:

```bash
git reset --hard e5f1c88
```

All three commits are back. The whole recovery took 2 commands and less than a minute.

Limits of this safety net: it covers only commits that were once saved. Edits that were never committed are not in it. And it exists only on your machine, so a lost clone also loses its reflog.

## A common mistake {#mistake}
Using `-f` on `main` or on any branch other than your own, and erasing a colleague's work from the server. Their local copy still has it, but the repair is slow and stressful. Use `--force-with-lease`, and only on your own branches. Another mistake is thinking the reflog protects work you never committed.

## Key points {#key-points}
- A rewritten history needs a forced push. Prefer `--force-with-lease` to `-f` or `--force`.
- `--force-with-lease` refuses when the remote has commits you have not seen.
- `git reflog` lists where `HEAD` has been. It lets you recover committed work after a reset.
- The reflog is local and expires.
