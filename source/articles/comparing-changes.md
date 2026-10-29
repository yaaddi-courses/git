# Comparing changes: git diff and HEAD

**Covers:** git diff, HEAD

## Before you save, look {#overview}
Imagine you have been editing a few files for an hour. Before you save anything, you probably want to ask one simple question: what exactly did I change? Git answers it with one command, and it uses one small word, `HEAD`, to say where you are right now. Let's take them in turn.

## git diff {#git-diff}
`git diff` shows your changes line by line. It puts the version Git already knows next to your current version, and marks the difference. A line that starts with `-` was removed. A line that starts with `+` was added. The unmarked lines around them are only there so you can see where the change sits.

By default, `git diff` compares your working files with what is in the Staging Area. Add `--staged` and it compares the Staging Area with your last commit instead, which answers "what will the next commit contain?". Give it two commit identifiers and it compares those two. Put a file name at the end and it looks at that file only.

## HEAD {#head}
`HEAD` means "the commit I am standing on". Most of the time you are on a branch, and `HEAD` follows that branch to its newest commit. So when you read `HEAD~1`, say "one step back from where I am", and `HEAD~2` is two steps back.

There is one case worth knowing about. If you jump straight to an old commit, `HEAD` points at that commit and not at a branch. Git calls this a detached state. You can look around safely, but any commit you make there belongs to no branch, so it is easy to lose.

## A worked example {#example}
Suppose `menu.txt` held three lines:

```text
Soup
Pasta
Salad
```

You edit it, and now it reads:

```text
Soup of the day
Pasta
Salad
Cake
```

Run `git diff`, and Git shows you this:

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

Read it like a short story. The first line changed: the old `Soup` is crossed out, and `Soup of the day` replaces it. `Pasta` and `Salad` did not change, so they appear without a mark. And `Cake` is new. In total, two lines were added and one was removed.

Now stage the file with `git add menu.txt` and run `git diff` again. It prints nothing, and that can feel alarming. Nothing is lost. Your change has moved to the Staging Area, so the working files and the staged files no longer differ. To see it again, ask for the staged view with `--staged`.

`HEAD` lets you ask a wider question. `git diff HEAD` compares your files with the last commit, staged and unstaged changes together, so it tells you everything you have done since your last save. And you can compare commits:

```bash
git diff HEAD~1 HEAD
```

This shows what the latest commit changed. Use `HEAD~3 HEAD` to see the combined effect of the last three.

| Command | What it compares | Use it when you want to know |
|---|---|---|
| `git diff` | Working files with the staged ones | What have I not staged yet? |
| `git diff --staged` | Staged files with the last commit | What am I about to save? |
| `git diff HEAD` | Working files with the last commit | What have I done since my last save? |
| `git diff HEAD~1 HEAD` | Previous commit with the latest | What did my last commit do? |

Here is a habit worth building: run `git diff --staged` before every commit. Reading 20 lines takes about half a minute, and it is how you catch a forgotten debugging line or an edit in the wrong file before it becomes part of your history.

## A common mistake {#mistake}
The most common slip is running `git diff` right after staging, seeing nothing, and concluding that the change vanished. It did not. It is staged, so ask for `git diff --staged`. The second slip is treating `HEAD` and a branch name as the same thing. They are close: the branch moves when you commit, and `HEAD` follows the branch. But the branch is the label, and `HEAD` is where you stand.

## Key points {#key-points}
- `git diff` shows what changed, line by line. `-` means removed and `+` means added.
- With `--staged`, it shows what your next commit will contain.
- `HEAD` is "where I am now", and `HEAD~1` is one commit before.
- Check `git diff --staged` before every commit.
