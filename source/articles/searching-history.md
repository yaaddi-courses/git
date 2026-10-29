# Searching history: log search, blame and bisect

**Covers:** git log -S, -S, --grep, -Z, git blame, git bisect

## Finding the cause of a problem {#overview}
When something breaks, the useful question is not only what is wrong, but when and why it became wrong. History holds the answer, and three tools search it in different ways: by the text a change added, by who last touched a line, and by testing the past automatically.

## git log -S {#git-log-s}
`git log -S` searches the history for commits that added or removed a given piece of text, such as `git log -S"max_retries"`. It is called the pickaxe search. It finds the commit where a word appeared or disappeared, which is exactly what you need to learn when a setting was introduced or removed. Add `--oneline` to keep the output short.

## -S {#s}
`-S` is the flag itself, followed by the text. A capital letter is required. The related `-G` takes a pattern instead of plain text.

## --grep {#grep}
`--grep` searches commit messages, not file contents: `git log --grep="login"` lists commits whose message mentions login. Use it when the message probably says what you are looking for. The two work together: `-S` searches what changed, `--grep` searches what was said about it.

## -Z {#z}
`-Z` is not the option for searching changes. Some people guess a letter, and `-Z` is a wrong one. The pickaxe flag is `-S`, with a capital S, and an unknown letter gives an error or a different effect.

## git blame {#git-blame}
`git blame` shows, for every line of a file, which commit last changed it, and who made it. Despite the name, it is meant for finding context, not for pointing fingers. Knowing the commit lets you read its message and its discussion, which often explains why the line looks strange.

## git bisect {#git-bisect}
`git bisect` finds the exact commit that introduced a bug by testing. You tell it one commit that is known good and one that is known bad. It checks out the middle, you test and answer good or bad, and it halves the range again, until only one commit remains.

## A worked example {#example}
A report says that discounts stopped working. You have three clues and three tools.

**Clue 1: a setting vanished.** Search for it:

```bash
git log -S"DISCOUNT_RATE" --oneline
```

```text
7b1c9d2 Remove old pricing constants
a40e55f Add discount support
```

Two commits touched that text: one added it, one removed it. The removal looks guilty.

**Clue 2: the message might say.** Search messages:

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

Line 13 was last changed by commit `7b1c9d2e`. The next step is to read that commit, and ask the author what the change was for.

**When the cause is not obvious**, bisect:

```bash
git bisect start
git bisect bad                 # the current version is broken
git bisect good v2.0           # this old tag worked
```

Say there are 128 commits between them. Git checks out the middle one, number 64. You run the tests and answer `git bisect good` or `git bisect bad`. Each answer halves the range: 128, 64, 32, 16, 8, 4, 2, 1. That is 7 or 8 tests to find the culprit among 128, against up to 128 tests one by one. Finish with `git bisect reset`.

If you have a script that exits with 0 when the code is good, `git bisect run ./test.sh` does all the steps without you.

| Question | Tool |
|---|---|
| When did this word appear or vanish? | `git log -S` |
| What did the messages say? | `git log --grep` |
| Who changed this line and why? | `git blame` |
| Which commit broke it? | `git bisect` |

## A common mistake {#mistake}
Using blame to blame. The author of a line often only moved it or reformatted it, and the real reason sits in an earlier commit. Read the commit before drawing conclusions. Another mistake is forgetting `git bisect reset` and wondering why the folder is in an old state.

## Key points {#key-points}
- `-S"text"` finds commits that added or removed text. `--grep` searches messages.
- `git blame` links each line to its last commit, for context and not for accusation.
- `git bisect` finds the breaking commit with about log base 2 of N tests.
- Always finish a bisect with `git bisect reset`.
