# Starting a repository: git init, origin and URLs

**Covers:** git init, origin, URL

## Two ways to begin {#overview}
You can start a project in two situations. Either you already have a folder of files and want Git to track it, or the project exists elsewhere and you want a copy. Each situation has one command. The first creates a repository from nothing, and the second is `git clone`, which you may have met already. This article looks at the first one, and at the words you need for connecting it to a server.

## git init {#git-init}
`git init` turns an ordinary folder into a Git repository by creating the hidden `.git` folder inside it. It does not touch, move or delete any of the files already there. Right after it, the repository is empty: it has no history, and Git sees your existing files as new things it has not been told about yet.

It is safe to run in a folder that is already a repository. Git reports that it has reinitialized, and keeps the history.

## URL {#url}
A remote repository is found by its URL, a uniform resource locator, which is a web address such as `https://example.com/team/recipes.git`. Two styles are common. An address that starts with `https://` uses ordinary web access, and asks for a login or a token when you send data. An address of the form `git@example.com:team/recipes.git` uses an SSH key, which saves you from typing a password.

## origin {#origin}
Typing long addresses all day would be tiring. So `origin` is the name Git gives by default to the remote you cloned from, or to the first one you connect. It is only a nickname for a URL and nothing special on the technical side. You could call it `server` or `upstream` and everything would work the same.

When you clone, Git sets up `origin` for you. When you start with `git init`, there is no remote at all until you add one.

## A worked example {#example}
Case 1. You have a folder `garden-notes` with three text files and you want to track it.

```bash
cd garden-notes
git init
git remote add origin https://example.com/me/garden-notes.git
git remote -v
```

Here is what each step does:

1. `git init` creates `garden-notes/.git`. The three files are untouched.
2. `git remote add origin <URL>` records the nickname `origin` for the address. Nothing is sent yet.
3. `git remote -v` lists the remotes. You see two lines, `fetch` and `push`, both with the same URL.

Case 2. The project already exists, and you clone it:

```bash
git clone https://example.com/team/recipes.git
cd recipes
git remote -v
```

The output shows `origin` pointing at the address you cloned from. You did nothing to set it up.

Let's count the difference. In case 1, there are 0 commits and 1 remote that you added by hand. In case 2, the clone brings the full history, say 240 commits, and 1 remote that Git added. If you wanted a second remote, for instance a backup copy, you would add it under another name such as `backup`, and `git remote -v` would then list 4 lines, two for each remote.

You can check the folder after case 1 with `ls -a`: you see `.git` beside your files. Remove that one folder and the repository is gone, while the files stay.

## A common mistake {#mistake}
A common slip is running `git init` inside a folder that is already inside another repository. That creates a repository within a repository, and Git treats the inner one as a separate project. Check first with `git rev-parse --show-toplevel`, which prints the top of the repository you are in.

Another slip is thinking that `origin` is special. It is only the default nickname.

## Key points {#key-points}
- `git init` creates a repository in an existing folder and changes none of its files.
- A URL locates a remote. `https://` and SSH-style addresses are the two common forms.
- `origin` is a default nickname for a remote, nothing more.
- `git clone` sets up `origin` for you, and `git init` does not.
