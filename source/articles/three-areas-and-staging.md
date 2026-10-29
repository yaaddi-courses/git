# The three areas: working, staged and saved

**Covers:** Working Directory, Staging Area, git add, git status, --staged

## A model that explains most of Git {#overview}
Beginners often ask why saving a change in Git takes two steps. The reason is that Git keeps your changes in three places, and each command moves them from one place to the next. Once you see the three places, most commands become predictable.

The places are your files as you edit them, a holding area for the next snapshot, and the saved history.

## Working Directory {#working-directory}
The Working Directory is the actual files on your disk, as you see and edit them right now. It is where you make changes, before Git knows anything about them. Git compares it with the saved history to find out what is different. A file here is either tracked, meaning Git already knows it, or untracked, meaning it is new.

## Staging Area {#staging-area}
The Staging Area is a holding area where you put exactly the changes you want in your next snapshot. It is also called the index. It lets you build a snapshot out of only some of your changes. If you fixed a bug and also started an unrelated feature, you can stage the fix alone, so that the snapshot tells one story.

## git add {#git-add}
`git add` moves a change from the Working Directory into the Staging Area. You give it a file name, a folder, or a dot for everything in the current folder. It copies the file's content at that moment. If you edit the file again afterwards, the new edit is not staged until you add it again.

## git status {#git-status}
`git status` reports the state of the three places: which files are staged, which are changed but not staged, and which are untracked. It only reports. It never changes anything, so it is safe to run as often as you like, and it also suggests the next command to try.

## --staged {#staged}
`--staged` is a flag of the command that compares versions of files. It shows only the changes already in the Staging Area, rather than the changes in the Working Directory. It answers "what exactly will go into the next snapshot?". A related word, `--cached`, means the same.

## A worked example {#example}
You are writing a cooking notes project. You edit two files and create a third.

```bash
git status
```

The report shows:

```text
Changes not staged for commit:
        modified:   pasta.txt
        modified:   soup.txt

Untracked files:
        notes-draft.txt
```

Now stage only the pasta fix:

```bash
git add pasta.txt
git status
```

```text
Changes to be committed:
        modified:   pasta.txt

Changes not staged for commit:
        modified:   soup.txt

Untracked files:
        notes-draft.txt
```

Follow the files:

| File | Working Directory | Staging Area |
|---|---|---|
| `pasta.txt` | Edited | Staged, ready |
| `soup.txt` | Edited | Not staged |
| `notes-draft.txt` | New | Not staged, untracked |

The next snapshot will hold only the pasta change. The soup change and the draft wait.

A subtle case: after `git add pasta.txt`, you edit `pasta.txt` again. Now `git status` lists it twice, once as staged, with the first edit, and once as not staged, with the second. The snapshot would contain the first edit only. To include both, run `git add pasta.txt` again.

To check before saving, the comparison command with `--staged` prints the staged change line by line. A change of 3 lines in pasta, plus 5 lines in soup, if both were staged, would mean a snapshot of 8 lines mixing two topics. Staging them separately gives two snapshots, of 3 and 5 lines, each easy to understand and to undo.

## A common mistake {#mistake}
Using `git add .` without looking, which stages everything, including files that should not be saved, such as temporary files or passwords. Run `git status` first and add files by name when you are unsure. Another mistake is assuming that staging saves anything. It does not. The history changes only at the next step.

## Key points {#key-points}
- Your changes move through three places: the Working Directory, the Staging Area, and the saved history.
- `git add` stages. `git status` shows where everything is and changes nothing.
- Staging lets you build a snapshot from only some of your changes.
- The comparison command with `--staged` shows what the next snapshot will contain.
