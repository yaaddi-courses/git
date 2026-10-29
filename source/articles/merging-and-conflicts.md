# Merging branches and resolving conflicts

**Covers:** git merge, fast-forward merge, three-way merge, merge conflict, --log

## Bringing two lines of work together {#overview}
You have finished the work on your branch, and now it has to return to the main line. Git calls this merging. How it goes depends on one question: did the main line move while you were away? Most of the time Git finishes by itself. Sometimes it needs your decision, and this article shows you both cases.

## git merge {#git-merge}
`git merge` combines the commits of another branch into the branch you are on. First you switch to the branch that should receive the work, usually `main`, and then you name the branch to bring in: `git merge add-search-box`. The branch you merge from is left unchanged.

## Fast-forward merge {#fast-forward-merge}
The easiest case is a fast-forward merge. It happens when your branch has not diverged from the one being merged. If `main` has had no new commits since you branched off, Git can slide the `main` label forward to your newest commit. No new commit is made, and the history stays a straight line.

## Three-way merge {#three-way-merge}
What if `main` moved too? A three-way merge happens when both branches have new commits since they split. Git looks at three points: the common ancestor, the tip of one branch and the tip of the other. It combines the changes into one new commit, called a merge commit, which has two parents. The history then shows the split and the join.

## Merge conflict {#merge-conflict}
Sometimes the two sets of changes cannot be combined automatically. A merge conflict occurs when both branches changed the same lines of a file in different ways. Git cannot guess which version you want, so it stops, marks the file and waits. The merge is paused, not lost, and you can still cancel it with `git merge --abort`.

Inside the file, Git writes markers:

```text
<<<<<<< HEAD
Soup of the day
=======
Tomato soup
>>>>>>> add-search-box
```

The part above `=======` is the version from the branch you are on, and the part below is the other one. You edit the file to what it should be, delete the three marker lines, stage it, and finish the merge with a commit.

## --log {#log}
The flag `--log` belongs to `git merge`. It adds one-line summaries of the merged commits to the merge message. That records in the merge commit what came in, which helps when the branch is deleted later.

## A worked example {#example}
Let's say `main` has the commit `a02b`. You branch off as `add-search-box` and make two commits, `b83c` and `c4d1`.

**Case 1: fast-forward.** `main` did not move.

```bash
git switch main
git merge add-search-box
```

Git prints `Fast-forward`. Now `main` points to `c4d1`. The log shows 3 commits in a line, and no merge commit exists.

**Case 2: three-way.** While you worked, `main` received a commit `d7e8` from a colleague, touching another file.

```bash
git switch main
git merge --log add-search-box
```

Git makes a merge commit, `f012`, with two parents: `d7e8` and `c4d1`. The history now has a diamond shape. The message includes the one-line list of your two commits, because of `--log`.

**Case 3: conflict.** Your colleague's commit `d7e8` also changed the first line of `menu.txt`, as you did.

```text
Auto-merging menu.txt
CONFLICT (content): Merge conflict in menu.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Here are the steps to resolve it:

1. Run `git status`. It lists `menu.txt` as "both modified".
2. Open the file, choose or combine the two lines, and remove the markers.
3. Run `git add menu.txt` to say it is resolved.
4. Run `git commit` to finish. Git suggests a message.

If it feels too hard, `git merge --abort` returns everything to the state before the merge.

| Situation | Result |
|---|---|
| Main did not move | Fast-forward, no new commit |
| Both moved, different lines | Three-way merge, new merge commit, no conflict |
| Both changed the same lines | Merge conflict, you decide |

Conflicts are normal, and they are not a sign that you did something wrong. In a team of 5 working on the same files, they happen every week. They stay small when branches are short and when you merge `main` into your branch often.

## A common mistake {#mistake}
A common slip is committing a file that still contains the `<<<<<<<` markers. Search for them before you stage, or use the project's tests.

Another slip is panicking and deleting the whole repository, when `git merge --abort` was all you needed.

## Key points {#key-points}
- Switch to the receiving branch, then `git merge other`.
- Fast-forward moves the label. Three-way creates a merge commit with two parents.
- A conflict means the same lines changed on both sides. Edit, `git add`, commit. Or abort.
- `--log` adds a list of merged commits to the message.
