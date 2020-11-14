# smartfix

Intelligent commit squasher.

# Installation

Install the script using homebrew:
```
brew install mluders/brew/smartfix
```

If you're tired of typing `--autosquash` every time you rebase, you can enable it globally:

```
git config --global rebase.autosquash true
```

# Overview

## The old way

You've changed multiple files, and you'd like to squash each change into a different commit. Normally, you'd add each file one by one, and carefully run `commit --fixup` on each one...

```
$ git status
  modified:   foo.rb
  modified:   bar.rb

$ git add foo.rb
$ git commit --fixup A_PAST_COMMIT

$ git add bar.rb
$ git commit --fixup ANOTHER_PAST_COMMIT
```

...and then auto squash.

```
$ git rebase --interactive --autosquash main
```

At that point, your rebase would look something like this:

```
pick  A_PAST_COMMIT
fixup foo.rb
pick  ANOTHER_PAST_COMMIT
fixup bar.rb
```

## The "smartfix" way
With smartfix, you can accomplish the same workflow above - but much more efficiently.

The `smartfix` command looks at each changed file, finds the most recent commit where it was modified, and automatically runs `commit --fixup` for each one.

```
$ git status
  modified:   foo.rb
  modified:   bar.rb

$ smartfix
$ git rebase --interactive --autosquash main
```

And you get the same result (but much faster):

```
pick  A_PAST_COMMIT
fixup foo.rb
pick  ANOTHER_PAST_COMMIT
fixup bar.rb
```

## Still confused?

Here's the pseudocode version of smartfix:

```
for (file in modified_files)
  last_commit = most_recent_commit(file)
  git commit --fixup last_commit
```

## TL;DR

1. Change a bunch of files (but do not commit them).
2. Run `smartfix`.
3. Run `git rebase --interactive STARTING_SHA`.
4. Your changes will automatically be fixed up into their appropriate ancestor commits.

# FAQ

## Why not use [git absorb](https://github.com/tummychow/git-absorb) instead?

Yeah... it's basically the same thing. I wasn't aware of git absorb when I first wrote smartfix, but it's cool to have my idea validated. Ultimately, my philosophy with smartfix is to provide a dead-simple, no-frills solution. Does this make it better than git absorb? Eh, probably not.
