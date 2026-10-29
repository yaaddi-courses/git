# Configuring Git: your identity and the three levels of settings

**Covers:** git config, user.name, user.email, --system, --global, --local, --all

## Telling Git who you are {#overview}
Every snapshot that Git saves carries the name and email of the person who made it. Git cannot guess these, so you set them once after installing. The same mechanism holds many other settings, and it works on three levels, from the whole machine down to one project.

## git config {#git-config}
`git config` reads and writes Git's settings. A setting has a name made of sections and keys, such as `user.name`, and a value. Run with a name and a value, it stores the setting. Run with a name only, it prints the current value. Run with `--list`, it prints everything that applies.

## user.name and user.email {#user-name}
`user.name` is the setting that holds the name recorded on your snapshots. `user.email` holds the email address. Both appear in the history for everyone to see, and platforms such as GitHub use the email to link a snapshot to an account. Git does not check them: anyone can write any name, so they identify, but do not prove, who made a change.

## The three levels {#levels}
Settings are stored in three places, and the more specific one wins.

- `--system` writes for every user on the machine. It needs administrator rights and is rarely used on a personal computer.
- `--global` writes for your own user account, and applies to all your repositories. This is where most people store their name and email.
- `--local` writes for the current repository only, and is stored inside it. It is the default when you give no level.

When the same setting exists on several levels, the local one overrides the global one, which overrides the system one.

## --all {#all}
`--all` is a flag that applies a command to every item of a kind instead of just the current one. In `git config`, `--get-all` lists every value of a setting that has several. Elsewhere in Git, the same flag widens a command to every branch or every file. It always means "everything, not only the current one", so check what a command will touch before adding it.

## A worked example {#example}
Maria uses one laptop for her job and for her own projects. At work she must use her company email, and at home her private one.

Step 1. Set her usual identity for all repositories:

```bash
git config --global user.name "Maria Lopez"
git config --global user.email "maria@home.example"
```

Step 2. In the work repository, override only the email:

```bash
cd ~/work/billing
git config --local user.email "maria.lopez@company.example"
```

Step 3. Check what applies there:

```bash
git config user.email
```

It prints `maria.lopez@company.example`, since the local value wins. In any other repository, the same command prints `maria@home.example`.

To see where each value comes from, use `git config --list --show-scope`. Each line then starts with the level that holds it. The global values live in a file in your home folder, and the local ones in the `config` file inside the repository's hidden folder.

Count what happens without step 2: every snapshot she made at work would carry her private email, and she would have to amend dozens of snapshots later, or leave her private address in the company's history for good.

## A common mistake {#mistake}
Forgetting to set the identity and then making the first snapshot, which fails or records a guessed name such as the computer's name. Set both settings first. Another mistake is putting a different email in a repository than the one registered on the hosting site, so that the history does not link to your account.

## Key points {#key-points}
- Set `user.name` and `user.email` once with `--global`.
- `--local` overrides `--global`, which overrides `--system`.
- The identity is recorded in every snapshot, and is not verified.
- `--all` widens a command to everything, not only the current item.
