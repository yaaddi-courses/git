# Version control and the Git repository

**Covers:** Version control, Version-control, VCS, distributed, Repository, Repositories, .git

## The problem with copies of files {#overview}
Anyone who has saved `report_final.docx`, `report_final2.docx` and `report_really_final.docx` has met the problem. Copies pile up, nobody knows which one is current, and when two people edit at once, one person's work is lost. Software teams meet this problem with thousands of files and dozens of people, so they use a tool built for it. This article explains the first vocabulary of that tool, Git.

## Version control {#version-control}
Version control is a system that records every change made to a set of files, so you can see what changed, who changed it, when, and why, and return to any earlier state. Instead of keeping copies, you keep one project and a history. Each saved step in the history is called a commit, and holds a snapshot of the files plus a short message.

The hyphenated form, version-control, means the same thing, as in "a version-control system". The short form is VCS.

## VCS {#vcs}
VCS stands for version control system, the software that does the recording. Older systems, such as Subversion, keep the history on one central server, and you must be connected to use it. Git belongs to the newer kind, described next.

## Distributed {#distributed}
Git is distributed: every person who has a copy of the project holds the whole history, not just the latest files. This has three consequences. You can browse history and save your work on a plane, with no network. Every copy is also a backup, so losing the server does not lose the project. And no copy is special by technology: which one counts as the official one is a team agreement.

## Repository {#repository}
A repository, often shortened to "repo", is a project's folder of files together with its complete saved history. Repositories, the plural, can live on your laptop, on a colleague's laptop and on a server, all holding the same project. Git is the program that manages them.

## The .git folder {#git}
The `.git` folder is the hidden folder at the top of a repository where Git keeps the whole history and the settings. Your visible files are only the current state; the past lives here. Two practical rules follow. Never edit anything inside it by hand. And if you delete the folder, the project's files remain, but all of its history is gone, so the folder is the repository in the literal sense.

## A worked example {#example}
A small team keeps a recipe book as text files. Compare two ways of working.

| | Emailing copies | Using a repository |
|---|---|---|
| Who has the latest version? | Unknown | The newest snapshot on `main` |
| Two people edit the same recipe | One edit is lost, or someone merges by hand | Git keeps both histories and helps combine them |
| "What did we change last Tuesday?" | Compare files by eye | Ask Git for the changes of that day |
| A bad edit | Hope an old copy exists | Return to an earlier snapshot |

A repository folder contains the files and the hidden `.git` folder. The history may hold, say, 240 snapshots. Because the project is distributed, all 240 are on every team member's machine, which is why every later look into the past works offline. If the server disappeared tomorrow, any team member's copy could be uploaded to a new server, and nothing would be lost.

Count the saving when two people change a file. Alice edits lines 10 to 20 and Bob edits lines 40 to 50. By email, someone must read both files and combine them: perhaps 15 minutes, with a risk of error. With a repository, Git combines them in a second, because the changes do not overlap.

## A common mistake {#mistake}
Thinking that the history lives only on the server. In Git, every copy has all of it, so deleting the `.git` folder from your copy loses the history of your copy, but the project survives elsewhere.

## Key points {#key-points}
- Version control keeps a history of changes instead of copies of files.
- Git is distributed: every clone holds the full history.
- A repository is the files plus the `.git` folder with the history.
