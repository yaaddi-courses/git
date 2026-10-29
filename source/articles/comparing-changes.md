# Comparing changes: git diff and HEAD

**Covers:** git diff, HEAD

## Seeing exactly what changed {#overview}
Before saving anything, you should know what you are about to save. A comparison shows the lines added and removed, side by side with the old version. Git does this with one command and one special pointer.

## git diff {#git-diff}
`git diff` shows your unstaged changes line by line, comparing the files in your Working Directory with what is in the Staging Area. Lines starting with `-` were removed and lines starting with `+` were added. Unchanged lines around them give context.

With `--staged`, it compares the Staging Area with the last commit, which shows what the next commit will contain. With two commit identifiers, it compares those two. With a file name at the end, it limits the output to that file.

## HEAD {#head}
`HEAD` is a pointer to the commit you currently have checked out, usually through the branch you are on. In everyday terms it means "where I am now". `HEAD~1` means the commit just before it, and `HEAD~2` the one before that.

Most of the time, `HEAD` points to the branch, and the branch points to the newest commit. If you move to an old commit directly, `HEAD` points straight at it, and you are in what Git calls a detached state, in which new commits belong to no branch.

## A worked example {#example}
A file `menu.txt` held:

```text
Soup
Pasta
Salad
```

You changed it to:

```text
Soup of the day
Pasta
Salad
Cake
```

Run `git diff`:

```diff
--- a/menu.txt
+++ b/menu.txt
@@ -1,3 +1,4 @@
-Soup
+Soup of the day
 Pasta
 Salad
+Cake
```

Read it: the first line changed (a `-` and a `+` pair), two lines are unchanged context, and one line, `Cake`, was added. That is 2 lines added and 1 removed.

Now stage the file with `git add menu.txt`. Run `git diff` again: it prints nothing, because the working files and the staged files are the same. Run `git diff --staged` and the same changes appear, this time as "what will be saved".

Use `HEAD` to ask a different question. `git diff HEAD` compares the working files with the last commit, combining staged and unstaged changes into one view: "everything I have done since the last save".

Compare commits with it, too:

```bash
git diff HEAD~1 HEAD
```

This shows what the latest commit changed. `HEAD~3 HEAD` shows the sum of the last three.

| Command | Compares | Use it to |
|---|---|---|
| `git diff` | Working files with staged | See what is not yet staged |
| `git diff --staged` | Staged with last commit | Check what you will save |
| `git diff HEAD` | Working files with last commit | See all work since the last save |
| `git diff HEAD~1 HEAD` | Previous commit with last | See what the last commit did |

A habit worth having: run `git diff --staged` before every commit. A review of 20 lines takes 30 seconds, and it catches debugging output and stray edits before they enter the history.

## A common mistake {#mistake}
Running `git diff` after staging and seeing nothing, then thinking that the change is lost. It is staged. Use `--staged`. Another mistake is confusing `HEAD` with the name of a branch. A branch moves when you commit, and `HEAD` follows the branch, but they are different things.

## Key points {#key-points}
- `git diff` compares working files with the staged ones; with `--staged` it compares staged with the last commit.
- `-` lines were removed and `+` lines were added.
- `HEAD` is "where I am now". `HEAD~1` is the commit before.
- Check `git diff --staged` before every commit.
