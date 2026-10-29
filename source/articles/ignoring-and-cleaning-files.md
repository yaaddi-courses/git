# Ignoring files, README and cleaning untracked files

**Covers:** README, git clean, --dry-run

## Not everything belongs in history {#overview}
Look inside any project folder after a week of work. Next to the source files you find build output, temporary files, editor settings and sometimes passwords. Saving them pollutes the history and makes it large. In the case of passwords, it is a security problem that lasts, because history is hard to erase. Two tools keep the folder tidy: a list of patterns that Git ignores, and a command that removes what Git does not track. We will also meet the README, the document that explains the project to newcomers.

## The .gitignore file {#gitignore}
A file named `.gitignore` in the project lists patterns of files that Git should pretend not to see. You write one pattern per line: `*.log` ignores all log files, `build/` ignores a folder, and `.env` ignores a settings file with secrets. Lines starting with `#` are comments, and a leading `!` makes an exception. Commit this file, so that every clone has the same rules.

There is one limit to remember. Ignoring affects only files that are not tracked yet. A file that was already committed stays tracked, and changing `.gitignore` does not remove it. To stop tracking it, use `git rm --cached file`.

## README {#readme}
The README is the first document of a project, kept at the top of the folder. It tells a newcomer what the project is for and how to start, and hosting sites show it on the project's front page. It usually has a title, a short description, how to install and use the project, and where to ask for help. Plain text and Markdown both work.

## git clean {#git-clean}
`git clean` deletes untracked files from your Working Directory, meaning files that Git is not tracking at all. It never touches tracked files. Because the deleted files were never saved, you cannot recover them. By default, it refuses to act unless you add `-f`. Folders need `-d`, and ignored files stay unless you add `-x`.

## --dry-run {#dry-run}
Given that deletion is final, you want a preview. The option `--dry-run`, or `-n`, shows what a command would do without doing it. For `git clean`, it prints the list of files that would be removed, and removes none. Make it your first step every time.

## A worked example {#example}
Here is a project folder after a week of work:

```bash
git status --short
```

```text
 M menu.txt
?? debug-output.log
?? scratch/
?? notes-draft.txt
```

`menu.txt` is changed and tracked. The other three are untracked. You want to delete the log and the scratch folder, but you want to keep `notes-draft.txt`.

Step 1. Preview:

```bash
git clean -n -d
```

```text
Would remove debug-output.log
Would remove notes-draft.txt
Would remove scratch/
```

Look at what the list tells you: `notes-draft.txt` would go as well. The preview saved your draft.

Step 2. Protect the draft by staging it, and then clean the rest:

```bash
git add notes-draft.txt
git clean -f -d
```

Only the log and the scratch folder are removed.

Step 3. Stop the log files from ever appearing again:

```text
# .gitignore
*.log
scratch/
```

After you commit `.gitignore`, `git status` stays quiet about both.

Step 4. Make sure a README exists:

```text
# Recipe Notes
Plain-text recipes for the family cookbook.

## Use
Open any `.txt` file in the `recipes` folder.
```

Let's count the effect. A repository that accidentally stores one 200 MB build folder forces every clone, forever, to download 200 MB. With a one-line ignore rule, it never happens.

| Need | Tool |
|---|---|
| Never track `*.log` | `.gitignore` |
| Delete untracked files now | `git clean` (preview first) |
| Explain the project | README |

## A common mistake {#mistake}
A common slip is running `git clean -f` without the preview and losing a file you meant to keep. Always add `--dry-run` first.

Another slip is committing a secret and then adding it to `.gitignore`. By then it is already in the history. Remove the secret and change it, because anyone with a copy of the history has it.

## Key points {#key-points}
- `.gitignore` keeps listed patterns out of Git's sight, for files not yet tracked.
- `git clean` deletes untracked files for good. Preview with `--dry-run`.
- A README says what the project is and how to use it.
- A secret that was committed once must be replaced, not only ignored.
