# Forks, pull requests and team workflows

**Covers:** fork, forking, pull request, trunk-based development, Git Flow

## Working with people you do not know {#overview}
In a company, everyone on the team can send commits to the shared repository. In an open-source project, strangers want to help, and the owners cannot give write access to all of them. The answer is a way to propose changes that the owners can review before accepting. Teams then choose a branching style to suit how often they release.

## Fork {#fork}
A fork is your own hosted copy of someone else's repository, usually on GitHub. You can change it freely, because it belongs to you, and the original is unaffected. A fork stays connected to its source, so you can later bring in new work from the original and offer your changes back.

## Forking {#forking}
Forking is the act of making that copy, usually with a button on the project page. It happens on the server, in your account. You then clone your fork to your computer, as with any repository.

## Pull request {#pull-request}
A pull request is a request, opened on GitHub or a similar service, asking a project's maintainers to review and merge changes from your branch or fork into their repository. It is a page with the change, the discussion, automated check results and the decision. Reviewers can comment on single lines, ask for changes and approve. If you push more commits to the same branch, the pull request updates by itself.

## Trunk-based development {#trunk-based-development}
In trunk-based development, everyone merges small changes into the main branch constantly, often several times a day, instead of working for a long time on separate branches. It needs good automated tests and often hides unfinished features behind switches. Its benefit is that merges are tiny, so conflicts are rare.

## Git Flow {#git-flow}
Git Flow is a branching strategy built around long-lived branches for scheduled releases, plus separate branches for features and urgent fixes. It has a `develop` branch for integration and a `main` branch holding only released versions. It is heavier than trunk-based development, and suits products that ship in numbered versions, such as installed software.

## A worked example {#example}
You find a typo in the documentation of an open-source project, and want to fix it.

1. On the project page, click **Fork**. You now have `you/project`.
2. Clone your fork:

```bash
git clone https://example.com/you/project.git
cd project
git switch -c fix-doc-typo
```

3. Edit the file, commit with a clear message such as "Fix typo in install guide", and push:

```bash
git push -u origin fix-doc-typo
```

4. On the site, a banner offers **Compare and pull request**. Click it, describe the change in two sentences, and submit.
5. A maintainer reviews it. They may ask, "please also fix the same typo on line 40". You commit that to the same branch and push, and the pull request updates.
6. The maintainer merges it. Your change is in the original project.

Compare the two team styles for a team of 6 that ships:

| | Trunk-based | Git Flow |
|---|---|---|
| Branch life | Hours, up to a day or two | Days to weeks |
| Merges per week | 30 or more, tiny | A handful, large |
| Release | Any time, from the main branch | From a release branch, on a schedule |
| Needs | Strong tests | Discipline about many branches |

For a web service updated several times a day, trunk-based is the usual choice. For a desktop program released four times a year with older versions still supported, Git Flow fits better.

Within a team, pull requests are used too, even without forks. They give a place to review before anything reaches `main`.

## A common mistake {#mistake}
Opening a pull request with a huge change that nobody can review. Reviewers skim, and bugs get through. Keep them small, ideally under a few hundred changed lines, with one topic. Another mistake is working for weeks on a fork without bringing in the original's new commits, so the pull request is full of conflicts.

## Key points {#key-points}
- A fork is your own copy of a repository. A pull request proposes your changes back.
- Small, focused pull requests get better reviews.
- Trunk-based development merges small changes often. Git Flow uses long-lived release branches.
- Choose the style by how often you release.
