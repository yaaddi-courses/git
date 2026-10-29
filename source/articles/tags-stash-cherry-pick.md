# Tags, stash and cherry-pick

**Covers:** git tag, git stash, cherry-pick

## Three small tools for three common situations {#overview}
Real work gets interrupted. You need to mark a release, or set aside half-done work for an urgent fix, or take a single fix from one branch to another. Git has a small command for each of these situations, and you will use all three more often than you expect.

## git tag {#git-tag}
Let's start with marking a release. `git tag` permanently attaches a memorable name to one specific commit, most often a version number such as `v2.0`. Unlike a branch, a tag does not move when you commit, because it always names the same commit. A lightweight tag is only the name. An annotated tag, made with `-a`, also stores the author, the date and a message, and it is the one to use for releases.

Tags are not sent by `git push` unless you ask for them: `git push origin v2.0`, or `git push --tags` for all of them.

## git stash {#git-stash}
Now the interruption. `git stash` temporarily puts your uncommitted changes aside, so you get a clean Working Directory. It is meant for the moment when you must switch branches but you are not ready to commit. The changes go onto a stack of stashes. `git stash pop` brings the latest back and removes it from the stack, and `git stash list` shows what is waiting. New, untracked files stay behind unless you add `-u`.

## Cherry-pick {#cherry-pick}
`git cherry-pick` copies one specific commit from another branch onto your current branch, without bringing along the other commits of that branch. The copy is a new commit with a new identifier, but with the same change and message. It is how you take a single bug fix to a release branch.

## A worked example {#example}
Let's say it is Thursday. Your team ships version 2.0 today, you are halfway through a feature, and a bug is reported in the shipped version.

**Step 1: tag the release.**

```bash
git tag -a v2.0 -m "Release 2.0"
git push origin v2.0
```

Anyone can now return to exactly this state with `git switch --detach v2.0`. Next month, when someone asks "what was in 2.0?", the answer is one command away.

**Step 2: put your unfinished feature aside.**

```bash
git stash
git switch main
```

`git stash` prints `Saved working directory and index state`. The folder is clean. Check the stack:

```bash
git stash list
```

```text
stash@{0}: WIP on add-search-box: 4d9e2a0 Add search input
```

**Step 3: fix the bug on main** and commit, say as `c9d8e7f`.

**Step 4: return to the feature and restore the work.**

```bash
git switch add-search-box
git stash pop
```

Your half-done edits are back, exactly as they were.

**Step 5: the same fix is needed in the older 1.9 line.** That branch is `release-1.9`:

```bash
git switch release-1.9
git cherry-pick c9d8e7f
```

Git creates a new commit on `release-1.9` with the same change. If the surrounding code differs, a conflict can occur. You resolve it as in a merge, and then run `git cherry-pick --continue`.

Now count the alternative without these tools. Without a stash, you would make a "work in progress" commit and undo it later, which is 2 extra steps. Without cherry-pick, merging `main` into `release-1.9` would drag in all of the new 2.0 work, perhaps 60 commits, when you wanted 1.

| Need | Tool | Moves? |
|---|---|---|
| Name a release | `git tag` | Never moves |
| Park uncommitted work | `git stash` | Local only, not shared |
| Copy one fix to another branch | `git cherry-pick` | Makes a new commit |

## A common mistake {#mistake}
A common slip is leaving work in a stash and forgetting it. Stashes do not appear in `git log`, and old ones are easy to lose. Keep the stack short, or use a branch for anything that will wait more than a day.

Another slip is cherry-picking a commit and later merging the branch it came from, which can cause duplicate changes and conflicts.

## Key points {#key-points}
- A tag names one commit forever. Use annotated tags for releases, and push them explicitly.
- `git stash` parks uncommitted changes, and `git stash pop` restores them.
- Cherry-pick copies one commit, creating a new one.
- Use a branch, not a stash, for work that will wait.
