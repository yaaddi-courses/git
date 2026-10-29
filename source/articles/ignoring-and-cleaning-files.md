# Ignoring files, README and cleaning untracked files

**Covers:** README, git clean, --dry-run

## Not everything belongs in history {#overview}
A project folder holds more than source files. Build output, temporary files, editor settings and passwords all end up there. Saving them pollutes the history, makes it large, and in the case of passwords is a security problem that lasts, because history is hard to erase. Two tools keep the folder tidy: a list of patterns that Git ignores, and a command that removes what is not tracked.

## The .gitignore file {#gitignore}
A file named `.gitignore` in the project lists patterns of files that Git should pretend not to see. One pattern per line: `*.log` ignores all log files, `build/` ignores a folder, `.env` ignores a settings file with secrets. Lines starting with `#` are comments, and a leading `!` makes an exception. Commit this file, so that every clone has the same rules.

An important limit: ignoring affects only files that are not tracked yet. A file that was already committed stays tracked, and changing `.gitignore` does not remove it. To stop tracking it, use `git rm --cached file`.

## README {#readme}
A README is the first document of a project, kept at the top of the folder, telling a newcomer what the project is for and how to start. Hosting sites show it on the project's front page. It usually has a title, a short description, how to install and run, and where to ask for help. Plain text or Markdown both work.

## git clean {#git-clean}
`git clean` deletes untracked files from the Working Directory, files that Git is not tracking at all. It never touches tracked files. Because the deleted files were never saved, they cannot be recovered. By default, it refuses to run unless you add `-f`. Folders need `-d`. Ignored files are left unless you add `-x`.

## --dry-run {#dry-run}
`--dry-run`, or `-n`, shows what a command would do without doing it. For `git clean`, it prints the list of files that would be removed, and removes none. It should be your first step every time.

## A worked example {#example}
Your project folder after a week of work:

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

Notice what the list reveals: `notes-draft.txt` would go as well. The preview just saved your draft.

Step 2. Save the draft by staging it, and then clean the rest:

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

After committing `.gitignore`, `git status` stays quiet about both.

Step 4. Make sure a README exists:

```text
# Recipe Notes
Plain-text recipes for the family cookbook.

## Use
Open any `.txt` file in the `recipes` folder.
```

Count the effect. A repository that accidentally stores one 200 MB build folder forces every clone, forever, to download 200 MB. With a one-line ignore rule, it never happens.

| Need | Tool |
|---|---|
| Never track `*.log` | `.gitignore` |
| Delete untracked files now | `git clean` (preview first) |
| Explain the project | README |

## A common mistake {#mistake}
Running `git clean -f` without the preview and losing a file you meant to keep. Always run with `--dry-run` first. Another mistake is committing a secret and then adding it to `.gitignore`. It is already in the history. Remove the secret, and change it, because anyone with a copy of the history has it.

## Key points {#key-points}
- `.gitignore` keeps listed patterns out of Git's sight, for files not yet tracked.
- `git clean` deletes untracked files for good. Preview with `--dry-run`.
- A README says what the project is and how to use it.
- A secret that was committed once must be replaced, not just ignored.
