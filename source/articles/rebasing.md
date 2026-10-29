# Rebasing and cleaning up history

**Covers:** git rebase, interactive rebase, squashing, -i, -x

## Another way to combine work {#overview}
Merging keeps the true shape of history, with its splits and joins. Some teams prefer a straight line, which is easier to read. Rebasing gives you that line, and it also lets you tidy your commits before you show them to anyone. Let's see how it works and, equally important, when not to use it.

## git rebase {#git-rebase}
`git rebase` replays your branch's commits, one by one, on top of the latest commit of another branch. The commits are copied with new identifiers, because their starting point changed, and the branch label then moves to the copies. The result is a straight-line history, as if you had started your work from the newest `main`.

For example, `git rebase main` while you are on your feature branch puts your commits after everything in `main`. A later merge into `main` is then a fast-forward.

## Interactive rebase {#interactive-rebase}
What if you want to clean up your commits as well? An interactive rebase is a rebase with the `-i` flag. It opens a list of your recent commits in an editor, one per line, each beginning with a word that says what to do with it. You can reorder lines, change `pick` to `reword` to edit a message, to `edit` to stop there, to `drop` to remove it, or to `squash` to combine it with the one above.

## -i {#i}
The flag `-i` is what makes a rebase interactive. For example, `git rebase -i HEAD~4` lists the last four commits.

## Squashing {#squashing}
Squashing means combining several commits into one. You usually do it during an interactive rebase, to turn a sequence of messy saves, such as "wip", "fix typo" and "try again", into one clear commit.

## -x {#x}
A word of warning about a letter that is easy to mix up. The flag `-x` does not open the interactive list, because that flag is `-i`. In `git rebase`, the real `-x` option runs a command after each commit that is replayed. If you wanted the list of commits and typed `-x`, you typed the wrong letter.

## A worked example {#example}
Say you made five commits on a feature branch:

```text
5  fix typo again
4  try again
3  wip
2  Add search box markup
1  start search box
```

Before you share the branch, clean it up.

```bash
git rebase -i HEAD~5
```

The editor shows the commits, oldest first:

```text
pick 1a1a1a1 start search box
pick 2b2b2b2 Add search box markup
pick 3c3c3c3 wip
pick 4d4d4d4 try again
pick 5e5e5e5 fix typo again
```

You edit it to:

```text
pick 1a1a1a1 start search box
squash 2b2b2b2 Add search box markup
squash 3c3c3c3 wip
squash 4d4d4d4 try again
squash 5e5e5e5 fix typo again
```

Save and close. Git then asks for one combined message, and you write `Add search box to the header`. The result is 1 commit in place of 5, with one clear story.

Now a plain rebase. While you worked, `main` got two new commits, so your branch is behind:

```bash
git switch add-search-box
git rebase main
```

Git replays your single commit on top of the newest `main`, and the history is a straight line of 3 commits. If a conflict occurs, Git stops at the commit that conflicts. You fix the file, run `git add` on it and continue with `git rebase --continue`. To cancel everything, use `git rebase --abort`.

| | Merge | Rebase |
|---|---|---|
| History shape | Shows splits and joins | Straight line |
| Existing commits | Unchanged | Copied with new identifiers |
| Safe on shared branches | Yes | No |

Here is the golden rule: rebase only commits that nobody else has. Rebased commits are new commits, and anyone who built on the old ones is left with a history that no longer matches.

## A common mistake {#mistake}
A common slip is rebasing a branch that others use. Their copies and yours disagree, and the next push needs force. Rebase your own local work only.

Another slip is squashing so much that a useful step-by-step story is lost. Squash the noise, and keep the meaningful steps.

## Key points {#key-points}
- Rebase replays your commits on top of another branch, for a straight history.
- `-i` opens the list for reordering, rewording and squashing.
- Squashing joins several commits into one.
- Never rebase commits that others already have.
