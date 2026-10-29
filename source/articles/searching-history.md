# Searching history: log search, blame and bisect

**Covers:** git log -S, -S, --grep, -Z, git blame, git bisect

## Finding the cause of a problem {#overview}
When something breaks, the useful question is not only what is wrong, but also when and why it became wrong. The history holds the answer. Three tools search it in different ways: by the text a change added, by who last touched a line, and by testing the past automatically. Let's use all three on one problem.

## git log -S {#git-log-s}
`git log -S` searches the history for commits that added or removed a given piece of text, such as `git log -S"max_retries"`. People call it the pickaxe search. It finds the commit where a word appeared or disappeared, which is exactly what you need when you want to learn when a setting was introduced or removed. Add `--oneline` to keep the output short.

## -S {#s}
The flag itself is `-S`, followed by the text. The capital letter is required. A related flag, `-G`, takes a pattern instead of plain text.

## --grep {#grep}
`--grep` searches commit messages instead of file contents: `git log --grep="login"` lists the commits whose message mentions login. Use it when the message probably says what you are looking for. The two flags complement each other, since `-S` searches what changed and `--grep` searches what people said about it.

## -Z {#z}
It is easy to guess a wrong letter here. `-Z` is not the option for searching changes. The pickaxe flag is `-S`, with a capital S, and an unknown letter gives an error or a different effect.

## git blame {#git-blame}
`git blame` shows, for every line of a file, which commit last changed it and who made that commit. Despite its name, it is meant for finding context, not for pointing fingers. Once you know the commit, you can read its message and its discussion, which often explains why a line looks strange.

## git bisect {#git-bisect}
When you do not know where to look, `git bisect` finds the exact commit that introduced a bug by testing. You give it one commit that is known good and one that is known bad. It checks out the middle, you test and answer good or bad, and it halves the range again until one commit remains.

## A worked example {#example}
Imagine a report says that discounts stopped working. You have three clues and three tools.

**Clue 1: a setting vanished.** Search for it:

```bash
git log -S"DISCOUNT_RATE" --oneline
```

```text
7b1c9d2 Remove old pricing constants
a40e55f Add discount support
```

Two commits touched that text: one added it and one removed it. The removal looks guilty.

**Clue 2: the message might say so.** Search the messages:

```bash
git log --grep="pricing" --oneline
```

It lists `7b1c9d2` again, which supports the theory.

**Clue 3: who and why for a line.** Look at the current pricing file:

```bash
git blame pricing.py
```

```text
^a40e55f (Maria Lopez 2026-08-02 12) def price(item):
7b1c9d2e (Ken Ito     2026-09-14 13)     return item.cost
```

Line 13 was last changed by commit `7b1c9d2e`. The next step is to read that commit and ask its author what the change was for.

**When the cause is not obvious**, use bisect:

```bash
git bisect start
git bisect bad                 # the current version is broken
git bisect good v2.0           # this old tag worked
```

Say there are 128 commits between them. Git checks out the middle one, number 64. You test it and answer `git bisect good` or `git bisect bad`. Each answer halves the range: 128, 64, 32, 16, 8, 4, 2, 1. That is 7 or 8 tests to find the culprit among 128, against up to 128 tests one by one. You finish with `git bisect reset`.

If you have a script that exits with 0 when the code is good, `git bisect run ./test.sh` does all the steps without you.

| Question | Tool |
|---|---|
| When did this word appear or vanish? | `git log -S` |
| What did the messages say? | `git log --grep` |
| Who changed this line and why? | `git blame` |
| Which commit broke it? | `git bisect` |

## A common mistake {#mistake}
A common slip is using blame to blame. The author of a line often only moved or reformatted it, and the real reason sits in an earlier commit. Read the commit before you draw conclusions.

Another slip is forgetting `git bisect reset`, and then wondering why the folder is in an old state.

## Key points {#key-points}
- `-S"text"` finds commits that added or removed text. `--grep` searches messages.
- `git blame` links each line to its last commit, for context and not for accusation.
- `git bisect` finds the breaking commit with about log base 2 of N tests.
- Always finish a bisect with `git bisect reset`.
