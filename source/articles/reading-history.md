# Reading your history: git log and its options

**Covers:** git log, SHA, --oneline, -n, --stat

## Looking back {#overview}
A history is only useful if you can read it. Git has one main command for that, with a set of options to shorten or expand what it prints. Knowing three or four of them is enough for daily work.

## git log {#git-log}
`git log` lists the saved commits, newest first, showing for each one its identifier, the author, the date and the message. It does not change anything. For a long history, it opens a pager: press space to go on and `q` to leave.

## SHA {#sha}
SHA stands for Secure Hash Algorithm. It is the kind of calculation that Git uses to give each commit its unique identifier, a long string of 40 characters such as `3f2a9c1e7b...`. The identifier is calculated from the content of the commit and of its history, so two different commits never share one, and a changed commit gets a new one. In commands you may write just the first 7 or so characters, as long as they are unique.

## --oneline {#oneline}
`--oneline` prints each commit on a single line: a short identifier and the first line of the message. It makes a long history fit on a screen, and is the best first look.

## -n {#n}
`-n` limits the output to a given number of the most recent commits. `git log -n 5` shows five. You can also write `-5`.

## --stat {#stat}
`--stat` adds a summary under each commit: which files changed, and by how many lines. It tells you the size and spread of a change without printing the changes themselves.

## A worked example {#example}
Look at a project quickly:

```bash
git log --oneline -n 4
```

```text
9d41e07 Add search box to the header
3f2a9c1 Fix login failure for emails with plus signs
b7c0a22 Update contact page
51de8f4 Add contact page
```

Each line is an identifier and a message. The top one is the newest. Count: 4 lines, as `-n 4` asked.

To see how big each change was:

```bash
git log --stat -n 2
```

```text
commit 3f2a9c1e7b...
Author: Maria Lopez <maria@home.example>
Date:   Tue Oct 6 10:14:02 2026

    Fix login failure for emails with plus signs

 login.py      | 6 ++++--
 test_login.py | 3 +++
 2 files changed, 7 insertions(+), 2 deletions(-)
```

You can read that without opening the files: two files, 7 lines added and 2 removed, and one of the files is a test.

A useful property of the identifier: a change to any old commit would change its identifier, and the identifiers of every commit after it. This is how Git notices tampering and corruption. If a colleague says "the bug came in with 3f2a9c1", you both know exactly the same commit, even if you each have your own clone.

Choose options by the question:

| Question | Command |
|---|---|
| What happened lately? | `git log --oneline -n 10` |
| How big were the last changes? | `git log --stat -n 3` |
| What exactly did one commit do? | `git log -n 1` plus its identifier |

For a project with 2,000 commits, the plain `git log` fills thousands of lines. The one-line form with a limit of ten fits on one screen, and is faster to read by a factor of about ten.

## A common mistake {#mistake}
Printing the whole history of a large project and getting lost. Add `--oneline` and `-n`. Another mistake is copying only 3 or 4 characters of an identifier, which may match several commits. Use at least 7.

## Key points {#key-points}
- `git log` lists commits, newest first, and never changes them.
- A SHA is the unique identifier of a commit. The first 7 characters usually suffice.
- `--oneline` gives one line each, `-n` limits the count, `--stat` shows which files changed.
