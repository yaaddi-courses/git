# Creating and switching branches

**Covers:** git switch, checkout, feature branch, -c, -b, -v

## Working on something without disturbing the rest {#overview}
A branch is a label on a commit, so creating one is nearly free. That makes it normal to start a separate line of work for each task, and the rest of the project stays untouched until you decide to bring your work back. In this article you will learn the commands for creating and moving between branches, and why Git has two sets of them.

## git switch {#git-switch}
`git switch` moves you onto a different existing branch. Git also updates the files in your folder to match that branch, so the folder changes as you move. If you have unsaved edits that would conflict with the other branch, Git refuses and tells you.

## -c {#c}
The flag `-c`, for create, tells `git switch` to make a new branch and move onto it at once: `git switch -c fix-login`. The new branch starts at the commit you are on now.

## checkout {#checkout}
You will also see `checkout` in many guides and scripts. It is the older, multi-purpose command. Among other jobs, it moves you to a branch. It also restores files and moves to a single commit, which is why Git later split these jobs into `git switch` for branches and `git restore` for files. The old command still works.

## -b {#b}
The same idea for `checkout` is the flag `-b`: `git checkout -b fix-login` creates the branch and moves to it.

## Feature branch {#feature-branch}
A feature branch is a branch created to hold the work for one specific change, kept apart from `main` until that work is ready. It has a short life: you create it, make a few commits, bring it back and delete it. Names that describe the work help, for example `fix-login` or `add-search-box`.

## -v {#v}
The flag `-v`, short for verbose, makes `git branch` show more detail: the commit each branch points at, with its message. Without it, you only get names.

## A worked example {#example}
Say you are on `main` and have a task: add a search box.

```bash
git switch -c add-search-box
```

Git answers `Switched to a new branch 'add-search-box'`. You make two commits there. Meanwhile, a colleague reports an urgent bug in `main`:

```bash
git switch main
git switch -c fix-typo-header
```

Your folder now shows the files as they were on `main`, without the search box. You fix the typo, commit and go back:

```bash
git switch add-search-box
```

The search box files return, as you left them.

Now list all the branches:

```bash
git branch -v
```

```text
  add-search-box   4d9e2a0 Add search input to header
  fix-typo-header  9c7b3f1 Fix typo in header title
* main             a02b4f7 Add recipe index
```

The star marks the branch you are on. Each branch points to a different commit, so you have 3 lines of work side by side, while the folder shows only one of them.

The same steps with the older command look like this:

```bash
git checkout -b add-search-box
git checkout main
```

| Task | Modern | Older |
|---|---|---|
| Move to a branch | `git switch name` | `git checkout name` |
| Create and move | `git switch -c name` | `git checkout -b name` |
| Restore a file | `git restore file` | `git checkout -- file` |

New guides use the modern commands because each one does a single job, so there is less risk of mixing up "go to a branch" with "throw away my edits".

Before you switch with uncommitted edits, check `git status`. If the edits do not conflict, they come with you to the other branch, which surprises many people. Commit them first, or put them aside.

## A common mistake {#mistake}
A common slip is making changes on `main` by accident because you forgot to switch first. Run `git branch` before you start working.

Another slip is a long-lived feature branch that drifts far from `main`, which makes the final merge hard. Keep them short.

## Key points {#key-points}
- `git switch name` moves to a branch. `git switch -c name` creates and moves.
- `checkout` does the same and more. `-b` is its create flag.
- A feature branch holds one task and is deleted after use.
- `git branch -v` lists branches with their latest commits.
