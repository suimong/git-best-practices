## Who am I

I'm working here at Yopeso as Ruby developer. My current project is Aureso.
Aureso is a Software as a Service for auto dealers.

## Foreword

Many of us using git in our day-to-day work. I'm noticed that some of you (or may be many of you) just use few features of it.
Like: `add`, `commit`, `push` and `pull`.
There is might be different reasons for that.
You don't know another stuff and/or afraid to use them.
Commands `amend` and `rebase` is nightmare for you.
Now I will try to fix that.

## Why history is important

Git history it's like contents of the book.
It helps other people to understand the purpose of each commit.
What's why your logs should be well named and followed by clear description.

## Some Commit best practices

  * Commit should contain related changes. Fixing two different bugs should produce two separate commits.
    Small commits make it easier to understand for other team members.

  * Commit often. It will allows you commit only related changes.

  * Don't commit half-done work. This means you should not commit unfinished feature at the end of working day.
    This also does not means you should commit whole huge feature in one commit. Just split it into small chuncks.

  * Use branch per feature. That allow you group commits in history.

## Read before commit

There is a good practice to read your emails and messages before send them.
That's works and with `git diff` too. So check your diffs before commit.

Just use `git diff` command for unstaged files and `git diff --cached` for staged.
(staged files are those files what just have added using `git add` command)

I'm usually find some stuff to fix like extra empty line, todo comment or typos.

## How to write good commit message

Never use -m (--message) flag to git commit.
It invites you set short unreadable commit messages.
`git commit` will opens your default editor where you could write more useful commit message.

There is terrific note from Tim Pope about writing commit messages.

Long story short.

Your commit title should be short, capitalized title with 50 or less characters followed by blank line.
Then goes more detailed description of changes wrapped in about to 72 characters.

Write your commit messages in imperative way: Use "fix", "change", "add" instead of "fixed", "changed", "added".
That convention matches up with commit messages generated by `git merge` and `git revert`.

## Add changes interactively

You're probably familiar with adding files into staging area using `add` command.
Sometimes you might make changes whose should belong to different commits.
As we have talk before we don't want to our commits to looks like 'Fixed A and B'.

So here `add -p <filename>` (`add --patch`) comes to help.
Interactive adding will asks to perform some action for each hunk.
These actions are:

    y - stage this hunk
    n - do not stage this hunk
    q - quit; do not stage this hunk nor any of the remaining ones
    a - stage this hunk and all later hunks in the file
    d - do not stage this hunk nor any of the later hunks in the file
    g - select a hunk to go to
    / - search for a hunk matching the given regex
    j - leave this hunk undecided, see next undecided hunk
    J - leave this hunk undecided, see next hunk
    k - leave this hunk undecided, see previous undecided hunk
    K - leave this hunk undecided, see previous hunk
    s - split the current hunk into smaller hunks
    e - manually edit the current hunk
    ? - print help

## What is fast forward

When you're trying to merge two branches and that merge can be made without conflicts git just
moves current pointer to the head - this is called "fast forward".
We could avoid using fast forward for merge and even set is as default behavior for merge.

## Merge with --no-ff

The --no-ff flash ensures there will be a merge commit, even if git could do a fast forward merge.
That picture shows the difference between merges with and without fast forward.

http://i.stack.imgur.com/FMD5h.png

## What is rebase?

From git help:

  "git-rebase - Forward-port local commits to the updated upstream head"

Clear enough?

NO!

Simply put: Rebase is the command to rewrite history in some ways.
Whese ways could be: Moving your commits on top of another branch, squash, split, amend.

Simpliest use:

From your branch `git rebase master`.
Saves all commits made in current branch to temporary area.
Resets your branch to master state then apply commits one by one from temporary area.
Complete result would looks like: [rebase picture]


Example of `git rebase --interactive (or just -i) commits..range` will start interactive rebasing.
It will opens default editor with following result:

    pick 1fc6c95 Patch A
    pick 6b2481b Patch B
    pick dd1475d something I want to split
    pick c619268 A fix for Patch B
    pick fa39187 something to add to patch A
    pick 4ca2acc i cant' typ goods
    pick 7b36971 something to move before patch B

    # Rebase 41a72e6..7b36971 onto 41a72e6
    #
    # Commands:
    #  p, pick = use commit
    #  r, reword = use commit, but edit the commit message
    #  e, edit = use commit, but stop for amending
    #  s, squash = use commit, but meld into previous commit
    #  f, fixup = like "squash", but discard this commit's log message
    #  x, exec = run command (the rest of the line) using shell
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    # However, if you remove everything, the rebase will be aborted.

But. Do not rebase published commits. Rebase changes commits and these commits will differs from previous ones.
People who've got these commits before will face some issues after pulling your changes.
As exception I accept rebasing for feature branches. Because I will throw it out after merge into development branch.

## git pull --rebase

Pulling changes from remote location by default tries to merge changes into you current branch.
It's use fast forward if it's possible but creates merge commit when faces conflicts in changes.
Avoid extra merge commit we can use git pull with --rebase params.
That will move our non published commits on top of history tree.

## My Git workflow

  * Create new branch for every main feature using git checkout -b <feature-branch>
  * Commit your code in small indepent pieces.
  * If you forgot something for last commit use `git --amend`
  * If you forgot something for recent commits then commit is as fix and use `rebase -i` to change commits order and squashing them.
  * Use git rebase <main-branch> (it might be master or develop) from your <feature-branch> to move all your commits on top.
  * Use git merge --no-ff <feature-branch> from <main-branch>

## Aliases

To avoid wasting time you could describe aliases to most used git commands. There is mine:

```
# ~/.gitconfig

[alias]
  st = status
  co = checkout
  aa = add --all
  ff = flow feature
  l = !~/.githelpers
  dc = diff --cached
  rc = rebase --continue
```

The most attentive of you might notice what I'm using file name as `l` alias.
It's how you could get result of external file as your command.

This is my script (To be honest it's stolen from another guy) to format logs

https://github.com/ck3g/dotfiles/blob/master/githelpers

Here is how it looks like in terminal window.

(Add screen here)

# Bonus feature
## Git Bisect

One of my favorite feature. It helped me many times. I'm also like to ask about it on job inverviews.

Lets imagine a situation when you realized what something was broken and you don't know when.
But you're know what 15 commits ago it's used to work. How to find broken commit?
What when git bisect helps.

git bisect uses binary search to find broken commit.

How to use
```
git bisect start
git bisect bad            # Mark current version as bad
git bisect good v1.2.3    # Mark good commit
```

git will checkout you between these commits where you will be able to check your current state.
Then mark commit as good/bad. You should keep going until you're find broken commit.
Check the difference using git show. Understand the problem. And use git reset to finish.

Note:
When your commits isn't atomic, like failing specs and its fixes are in different commits.
You will probably be confused then bisect provide you commit with failing specs.

## Afterword

Git is the powerful tool. We all using it on every day basis. Thus it's a good reason to master it.
Try to use these advices in your everyday work. I hope you will like it.


## Links

  * http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  * http://mislav.uniqpath.com/2013/02/merge-vs-rebase/
  * https://www.atlassian.com/git/tutorials/rewriting-history
  * http://code.tutsplus.com/tutorials/git-tips-from-the-pros--net-29799
  * http://codeinthehole.com/writing/pull-requests-and-other-good-practices-for-teams-using-github/
