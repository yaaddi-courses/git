# Submodules, large files and how Git stores data

**Covers:** submodule, Git LFS, LFS, blob, tree object

## Three advanced topics, and the idea behind them {#overview}
Some projects include another project, or contain huge files, and some people want to know what Git does with all this data. The three topics connect: a submodule points at a particular snapshot, Git LFS changes what is stored for big files, and the object model explains why those choices matter.

## Submodule {#submodule}
A submodule is a separate Git repository embedded inside another one. The outer repository does not copy the inner one's files into its own history. It records the address and one exact commit of the inner repository. It is a way to include a shared library while keeping its history separate.

The cost is care. After a clone, the inner folder is empty until you run `git submodule update --init`. And the pointer moves only when someone commits the new pointer, so forgetting that step leaves a team on different versions.

## Git LFS {#git-lfs}
Git LFS, short for Git Large File Storage, is an extension that replaces large files, such as videos and design files, with small pointer files in the normal history, and stores the real content on a separate server. You choose files by pattern: `git lfs track "*.mp4"`. Git handles the pointer, and LFS downloads the real file when needed.

## LFS {#lfs}
LFS is the short name of the extension, as used in command names such as `git lfs install` and in the settings file `.gitattributes`. It keeps big files out of the normal repository history, so clones stay small.

## Blob {#blob}
A blob is the Git object that stores a file's raw contents. It does not even store the file's name, only the data. Two files with identical contents share one blob, so a copy of a file costs almost no space.

## Blobs {#blobs}
Blobs, the plural, are all of these content objects. Each is identified by a hash of its content, so changing one byte makes a different blob.

## Tree object {#tree-object}
A tree object stores the structure of one folder: a list of names, each pointing to a blob for a file or to another tree for a subfolder. The names live here, not in the blobs.

## Tree objects {#tree-objects}
Tree objects, in the plural, nest to describe a whole project: one for the top folder, one for each subfolder below.

## A worked example {#example}
Look at how Git stores a very small project:

```text
menu.txt      -> "Soup\nPasta\n"
copy-menu.txt -> "Soup\nPasta\n"
docs/guide.txt -> "Read me\n"
```

Git creates:

- 1 blob for `"Soup\nPasta\n"`, shared by both menu files, since the contents are the same.
- 1 blob for `"Read me\n"`.
- 1 tree for the folder `docs`, listing `guide.txt` and its blob.
- 1 tree for the top folder, listing `menu.txt`, `copy-menu.txt` (both pointing to the same blob) and `docs` (pointing to the tree).
- 1 commit, pointing at the top tree, with the author, date, message and the previous commit.

That is 2 blobs, 2 trees and 1 commit: 5 objects for 3 files. You can look inside with `git cat-file -p HEAD`, which prints the commit, and `git cat-file -p HEAD^{tree}`, which prints the top tree.

Now the cost of a large file. A 100 MB video, edited 10 times, is stored 10 times in a normal repository: about 1,000 MB, since each version is a new blob and compresses poorly. Every clone downloads all of it. With Git LFS, the repository holds ten small pointers of a few hundred bytes each, and the 1,000 MB sit on the LFS server. A clone downloads only the version needed for the files you check out, about 100 MB.

Set it up once:

```bash
git lfs install
git lfs track "*.mp4"
git add .gitattributes
```

Commit the `.gitattributes` file, so that the rule travels with the project.

For a submodule:

```bash
git submodule add https://example.com/team/ui-kit.git libs/ui-kit
git commit -m "Add ui-kit as a submodule"
```

The outer commit records the address and the exact commit of `ui-kit`. A new clone then runs `git submodule update --init` to fill `libs/ui-kit`.

| Tool | Purpose | Main cost |
|---|---|---|
| Submodule | Include another repository at a fixed version | Extra update steps |
| Git LFS | Keep big files out of history | Needs an LFS server |
| Normal blobs | Everything else | Large files bloat history |

## A common mistake {#mistake}
Committing a large file by accident, and only then adding it to LFS. The big blob is already in the history, and stays there. Track the pattern before the first commit of such files. Another mistake is cloning a project with submodules and wondering why a folder is empty.

## Key points {#key-points}
- A submodule pins another repository at one exact commit. Clone, then initialise it.
- Git LFS stores big files elsewhere and keeps small pointers in history.
- A blob holds file contents, a tree object holds a folder's names and links.
- Identical contents share one blob.
