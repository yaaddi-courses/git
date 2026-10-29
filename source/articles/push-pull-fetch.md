# Pushing, pulling and fetching

**Covers:** git push, git pull, git fetch, tracking branch, tracking branches

## Moving commits between repositories {#overview}
Commits you make on your machine stay private until you send them. Commits made by others reach you only when you ask for them. Three commands handle this traffic, and a link between local and remote branches saves most of the typing. Let's take the commands in the order of how careful they are.

## git push {#git-push}
`git push` uploads your local commits to a remote repository, so others can see them. You name the remote and the branch: `git push origin main`. Git refuses when the remote branch has commits you do not have, because accepting yours would overwrite them.

## git fetch {#git-fetch}
`git fetch` is the careful one. It downloads new commits from a remote without touching your own branch or files. It updates only your copies of the remote branches, such as `origin/main`, so you can look at what is new before you decide what to do. It is the quiet, safe command.

## git pull {#git-pull}
`git pull` goes one step further. It downloads new commits from the remote and immediately merges them into your current branch, which makes it `git fetch` followed by `git merge`. It is convenient when you are sure you want the changes now, and surprising when you are not.

## Tracking branch {#tracking-branch}
How does Git know which remote branch to use? Through a tracking branch, a local branch linked to a branch on a remote, so `git push` and `git pull` know where to go without arguments. When you clone, `main` tracks `origin/main` automatically. For a new branch, you create the link on the first push: `git push -u origin add-search-box`. The plural, tracking branches, means all such linked pairs, and `git status` uses them to tell you "your branch is ahead of origin/main by 2 commits".

## A worked example {#example}
You and a colleague share a repository. You start your day:

```bash
git fetch
git status
```

```text
On branch main
Your branch is behind 'origin/main' by 3 commits, and can be fast-forwarded.
```

Fetching changed nothing in your files. It only updated `origin/main`. Now you can look: `git log --oneline main..origin/main` lists the 3 new commits. If you are happy with them, bring them in:

```bash
git pull
```

Because you already fetched, `git merge origin/main` does the same.

Now you make 2 commits and push:

```bash
git push
```

```text
To https://example.com/team/recipes.git
   a02b4f7..4d9e2a0  main -> main
```

The range shows that the remote moved from `a02b4f7` to `4d9e2a0`.

If your colleague pushed first, your push is rejected:

```text
! [rejected]        main -> main (fetch first)
```

The cure is to fetch, combine their work with yours and push again. Git is protecting their commits.

A new branch needs its link once:

```bash
git switch -c add-search-box
git push -u origin add-search-box
```

After that, plain `git push` and `git pull` work on it.

| Command | Changes your files? | Changes the remote? |
|---|---|---|
| `git fetch` | No | No |
| `git pull` | Yes | No |
| `git push` | No | Yes |

A good habit is to fetch first, look, and then merge. It costs one extra command and avoids a merge at a bad moment, such as the middle of your work. In a team of 5 pushing daily, pulling blindly in the morning with uncommitted edits can create a conflict before you have had your coffee.

## A common mistake {#mistake}
A common slip is using `git pull` with uncommitted work and getting a surprise merge or conflict. Fetch first, or commit your work.

Another slip is pushing to the wrong branch because no tracking link exists. Read the output of `git status` before you push.

## Key points {#key-points}
- `git push` sends commits. `git fetch` receives without changing your branch. `git pull` is fetch plus merge.
- A tracking branch is linked to a remote branch, so you can type short commands.
- A rejected push means someone else pushed first: fetch, combine, push again.
