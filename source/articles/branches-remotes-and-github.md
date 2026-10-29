# Branches, remotes, GitHub and cloning

**Covers:** branch, branches, remote, GitHub, git clone

## Several lines of work, in several places {#overview}
A repository holds one project, but not one line of work. Branches let you keep separate lines inside it. Remotes let the same project live in several places. This article covers both, and the command that makes a first copy.

## Branch {#branch}
A branch is a movable label that points at one saved snapshot. It is not a copy of your files. The default branch is usually called `main`. As you save new snapshots, the label moves forward to the newest one. Making a second label costs almost nothing, which is why Git users create branches freely to try ideas. Branches, the plural, are all the labels in one repository.

## Remote {#remote}
A remote is a version of your repository that lives somewhere else, usually on a server, which your copy can send snapshots to and receive them from. A repository may have several remotes, each with its own name and address.

## GitHub {#github}
GitHub is a website that hosts Git repositories and adds tools around them, such as code review, issue tracking and automation. It is a service built on top of Git, and not part of Git itself. Similar services exist, such as GitLab and Bitbucket. Git works without any of them.

## Git clone {#git-clone}
`git clone` downloads a full copy of a remote repository, with its entire history, onto your machine. It creates a folder named after the project and sets up the remote for you.

## A worked example {#example}
Maria and Ken work on a recipe book. The shared copy sits on a hosting site, so it is a remote.

```bash
git clone https://example.com/team/recipes.git
cd recipes
git branch
```

The clone brings the full history, and `git branch` prints `* main`: one branch, with the star showing the one you are on.

Ken makes a branch for his idea, which costs nothing, because it is a label:

```text
main        a02b4f7  Add recipe index
ken-idea    a02b4f7  (same commit, a second label)
```

Both labels point at the same snapshot. When Ken adds a commit on `ken-idea`, only that label moves forward, and `main` stays where it was. Maria meanwhile keeps working on `main` with no interference.

Count the effect on a team of 5. With one shared line of work, a half-finished change by one person is seen by all 5. With branches, each person works on a separate label, and the unfinished work reaches the others only when its owner decides.

| Word | Where it lives | What it is |
|---|---|---|
| Branch | In a repository | A label on a snapshot |
| Remote | On a server or another computer | Another copy of the repository |
| GitHub | On the internet | A service that hosts remotes |

## A common mistake {#mistake}
Thinking that GitHub and Git are the same. Git is the tool on your computer. GitHub is one place to store and share repositories. Another mistake is thinking that a branch is a copy of all the files. It is a label, which is why creating one is instant.

## Key points {#key-points}
- A branch is a movable label on a snapshot, and creating one is instant.
- A remote is a copy of the repository elsewhere, and `git clone` makes your first local copy of it.
- GitHub hosts remotes, but Git does not need it.
