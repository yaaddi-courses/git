# Branches, remotes, GitHub and cloning

**Covers:** branch, branches, remote, GitHub, git clone

## Several lines of work, in several places {#overview}
Imagine you and a friend are writing a recipe book together. You want to try a new chapter without disturbing the finished ones, and you both want the same book to exist on two computers. Git has an answer for each wish. Branches keep separate lines of work inside one repository, and remotes let the same project live in several places. This article covers both, and the command that makes your first copy.

## Branch {#branch}
Let's start with the idea that surprises most beginners. A branch is not a copy of your files. It is a movable label that points at one saved snapshot. The default branch is usually called `main`. When you save new snapshots, the label moves forward to the newest one. Making a second label costs almost nothing, which is why Git users create branches freely to try ideas. When we say branches, in the plural, we mean all the labels in one repository.

## Remote {#remote}
A remote is a version of your repository that lives somewhere else, usually on a server. Your copy can send snapshots to it and receive snapshots from it. A repository may have several remotes, each with its own name and address.

## GitHub {#github}
Where do remotes usually live? Very often on GitHub, a website that hosts Git repositories and adds tools around them, such as code review, issue tracking and automation. Notice that it is a service built on top of Git, not part of Git itself. Similar services exist, such as GitLab and Bitbucket, and Git works without any of them.

## Git clone {#git-clone}
`git clone` is how you get your first copy. It downloads a full copy of a remote repository, with its entire history, onto your machine. It creates a folder named after the project and sets up the remote for you.

## A worked example {#example}
Maria and Ken work on a recipe book. The shared copy sits on a hosting site, so it is a remote.

```bash
git clone https://example.com/team/recipes.git
cd recipes
git branch
```

The clone brings the full history, and `git branch` prints `* main`: one branch, with the star marking the one you are on.

Ken makes a branch for his idea. It costs nothing, because it is only a label:

```text
main        a02b4f7  Add recipe index
ken-idea    a02b4f7  (same commit, a second label)
```

Both labels point at the same snapshot. When Ken adds a commit on `ken-idea`, only that label moves forward, and `main` stays where it was. Maria keeps working on `main` without any interference.

Let's count the effect on a team of 5. With one shared line of work, a half-finished change by one person is seen by all 5. With branches, each person works on a separate label, and unfinished work reaches the others only when its owner decides.

| Word | Where it lives | What it is |
|---|---|---|
| Branch | In a repository | A label on a snapshot |
| Remote | On a server or another computer | Another copy of the repository |
| GitHub | On the internet | A service that hosts remotes |

## A common mistake {#mistake}
A common slip is thinking that GitHub and Git are the same thing. Git is the tool on your computer, and GitHub is one place to store and share repositories.

Another slip is picturing a branch as a copy of all the files. It is a label, which is why creating one is instant.

## Key points {#key-points}
- A branch is a movable label on a snapshot, and creating one is instant.
- A remote is a copy of the repository elsewhere, and `git clone` makes your first local copy of it.
- GitHub hosts remotes, but Git does not need it.
